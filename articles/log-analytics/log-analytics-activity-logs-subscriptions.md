---
title: "Recolher registos de atividade do Azure para análise de registos nas subscrições | Microsoft Docs"
description: "Utilize os Hubs de eventos e Logic Apps para recolher dados a partir do registo de atividade do Azure e enviá-lo para uma área de trabalho do Log Analytics do Azure de um inquilino diferente."
services: log-analytics, logic-apps, event-hubs
documentationcenter: 
author: richrundmsft
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: richrund; bwren
ms.openlocfilehash: ded0b4cdcbac747d52435023a24b5719f3c58758
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="collect-azure-activity-logs-into-log-analytics-across-subscriptions"></a>Recolher registos de atividade do Azure para análise de registos, entre subscrições

Passos neste artigo, através de um método para recolher registos de atividade do Azure para uma área de trabalho de análise de registos, utilizando o conector do Recoletor de dados de análise de registos do Azure para aplicações lógicas. Utilize o processo neste artigo, quando tiver de enviar os registos para uma área de trabalho uma diferente do Azure Active Directory. Por exemplo, se for um fornecedor de serviço geridas, poderá pretender recolher registos de atividade de subscrição de um cliente e armazená-las numa área de trabalho de análise de registos na sua própria subscrição.

Se a área de trabalho de análise de registos na mesma subscrição do Azure, ou numa subscrição diferente, mas no mesmo do Azure Active Directory, utilize os passos a [solução de registo de atividade do Azure](../log-analytics/log-analytics-activity.md) para recolher registos de atividade do Azure.

## <a name="overview"></a>Descrição geral

A estratégia utilizada neste cenário é tem registo de atividade do Azure enviar eventos para um [Hub de eventos](../event-hubs/event-hubs-what-is-event-hubs.md) onde um [aplicação lógica](../logic-apps/logic-apps-overview.md) envia-as para a sua área de trabalho de análise de registos. 

![imagem do fluxo de dados de análise do registo para o registo de atividade](media/log-analytics-activity-logs-subscriptions/data-flow-overview.png)

Vantagens desta abordagem incluem:
- Baixa latência, uma vez que o registo de atividade do Azure é transmitida em fluxo para o Hub de eventos.  A aplicação lógica, em seguida, é acionada e envia os dados para análise de registos. 
- Código mínimas é necessário, e não existe nenhuma infraestrutura de servidor para implementar.

Este artigo disponibiliza os passos como:
1. Crie um Hub de eventos. 
2. Exporte registos de atividade para um Hub de eventos utilizando o perfil de exportação de registo de atividade do Azure.
3. Crie uma aplicação lógica para ler a partir do Event Hub e enviar eventos para análise de registos.

## <a name="requirements"></a>Requisitos
Seguem-se os requisitos para os recursos do Azure utilizados neste cenário.

- O espaço de nomes do Hub de eventos têm de estar na mesma subscrição que a subscrição emitir os registos. O utilizador que configura a definição tem de ter permissões de acesso adequadas para ambas as subscrições. Se tiver várias subscrições no mesmo diretório do Active Directory do Azure, pode enviar os registos de atividade para todas as subscrições para um hub de eventos único.
- A aplicação lógica pode estar numa subscrição diferente do hub de eventos e não precisa de estar no mesmo do Azure Active Directory. A aplicação lógica lê do Hub de eventos utilizando a chave de acesso partilhado do Hub de eventos.
- A área de trabalho de análise de registos pode ser numa subscrição diferente e o Azure Active Directory da aplicação lógica, mas para simplicidade, recomendamos que estão na mesma subscrição. A aplicação lógica envia ao Log Analytics com o ID da área de trabalho de análise de registos e a chave.



## <a name="step-1---create-an-event-hub"></a>Passo 1 – criar um Hub de eventos

<!-- Follow the steps in [how to create an Event Hubs namespace and Event Hub](../event-hubs/event-hubs-create.md) to create your event hub. -->

1. No portal do Azure, selecione **novo**> **Internet das coisas** > **Event Hubs**.

   ![Novo hub de eventos do Marketplace](media/log-analytics-activity-logs-subscriptions/marketplace-new-event-hub.png)

3. Em **criar espaço de nomes**, quer introduza um novo espaço de nomes ou selecione um existente. O sistema verifica imediatamente a disponibilidade do nome.

   ![imagem de criar a caixa de diálogo de hub de eventos](media/log-analytics-activity-logs-subscriptions/create-event-hub1.png)

4. Escolha o escalão de preço (básico ou Standard), uma localização para o novo recurso, grupo de recursos e subscrição do Azure.  Clique em **Criar** para criar o espaço de nome. Poderá ter de aguardar alguns minutos para que o sistema totalmente aprovisionar os recursos.
6. Clique no espaço de nomes que acabou de criar na lista.
7. Selecione **políticas de acesso partilhado**e, em seguida, clique em **RootManageSharedAccessKey**.

   ![Imagem das políticas de acesso partilhado do hub de eventos](media/log-analytics-activity-logs-subscriptions/create-event-hub7.png)
   
8. Clique no botão copiar para copiar a cadeia de ligação **RootManageSharedAccessKey** para a área de transferência. 

   ![imagem da chave de acesso partilhado do hub de eventos](media/log-analytics-activity-logs-subscriptions/create-event-hub8.png)

9. Numa localização temporária, como o Notepad, manter uma cópia, o nome de Hub de eventos e a primária ou secundária Hub de eventos cadeia de ligação. A aplicação lógica requer estes valores.  Para a cadeia de ligação do Hub de eventos, pode utilizar o **RootManageSharedAccessKey** ligação string ou crie um separado.  A cadeia de ligação que utilizar tem de começar com `Endpoint=sb://` e para uma política que tem de ser o **gerir** política de acesso.


## <a name="step-2---export-activity-logs-to-event-hub"></a>Passo 2 – registos de atividade de exportação para o Hub de eventos

Para ativar a transmissão em fluxo de registo de atividade, escolha um espaço de nome de Hub de eventos e uma política de acesso partilhado para esse espaço de nomes. Um Hub de eventos é criado nesse espaço de nomes quando ocorre o primeiro evento de registo de atividade de novo. 

Pode utilizar um espaço de nomes de hub de eventos que não se encontra na mesma subscrição que a subscrição emitir os registos, no entanto, as subscrições têm de estar no mesmo do Azure Active Directory. O utilizador que configura a definição tem de ter o RBAC adequado para aceder a ambas as subscrições. 

1. No portal do Azure, selecione **Monitor** > **registo de atividade**.
3. Clique em de **exportar** botão na parte superior da página.

   ![imagem do monitor do azure no painel Navegação](media/log-analytics-activity-logs-subscriptions/activity-log-blade.png)

4. Selecione o **subscrição** para exportar do e, em seguida, clique em **Selecionar tudo** no **regiões** pendente para selecionar eventos para recursos em todas as regiões. Clique em de **exportar para um hub de eventos** caixa de verificação.
7. Clique em **espaço de nomes do Service bus**e, em seguida, selecione o **subscrição** com o hub de eventos, o **espaço de nomes de hub de eventos**e um **nome de política de hub de eventos**.

    ![imagem de exportação para a página de hub de eventos](media/log-analytics-activity-logs-subscriptions/export-activity-log2.png)

11. Clique em **OK** e, em seguida, **guardar** para guardar estas definições. As definições são imediatamente ser aplicadas à sua subscrição.

<!-- Follow the steps in [stream the Azure Activity Log to Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) to configure a log profile that writes activity logs to an event hub. -->

## <a name="step-3---create-logic-app"></a>Passo 3 - criar a aplicação lógica

Assim que estiver a escrever os registos de atividade para o hub de eventos, criar uma aplicação lógica para recolher os registos do hub de eventos e escrevê-las à análise de registos.

A aplicação lógica inclui o seguinte:
- Um [conector de Hub de eventos](https://docs.microsoft.com/connectors/eventhubs/) acionador ler a partir do Hub de eventos.
- A [ação analisar JSON](../logic-apps/logic-apps-content-type.md) para extrair os eventos JSON.
- A [compor ação](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) para converter o JSON para um objeto.
- A [Log Analytics enviar o conector de dados](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) publicar os dados para análise de registos.

   ![imagem da adição de Acionador de hub de eventos nas logic apps](media/log-analytics-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="logic-app-requirements"></a>Requisitos da aplicação lógica
Antes de criar a sua aplicação lógica, certifique-se de que tem as seguintes informações dos passos anteriores:
- Nome do Hub de eventos
- Evento Hub cadeia de ligação (primário ou secundário) para o espaço de nomes do Hub de eventos.
- ID de área de trabalho de análise do registo
- Análise de registos chave partilhada

Para obter o cadeia de ligação e nome de Hub de eventos, siga os passos no [permissões de espaço de nomes de verificação de Event Hubs e localizar a cadeia de ligação](../connectors/connectors-create-api-azure-event-hubs.md#check-event-hubs-namespace-permissions-and-find-the-connection-string).


### <a name="create-a-new-blank-logic-app"></a>Criar uma nova aplicação lógica em branco

1. No portal do Azure, escolha **crie um recurso** > **integração empresarial com** > **aplicação lógica**.

    ![Nova aplicação de lógica de Marketplace](media/log-analytics-activity-logs-subscriptions/marketplace-new-logic-app.png)

2. Introduza as definições na tabela abaixo.

    ![Criar uma aplicação lógica](media/log-analytics-activity-logs-subscriptions/create-logic-app.png)

   |Definição | Descrição  |
   |:---|:---|
   | Nome           | Nome exclusivo para a aplicação lógica. |
   | Subscrição   | Selecione a subscrição do Azure que irá conter a aplicação lógica. |
   | Grupo de Recursos | Selecione um grupo de recursos do Azure existente ou crie um novo para a aplicação lógica. |
   | Localização       | Selecione a região do datacenter para implementar a sua aplicação lógica. |
   | Log Analytics  | Selecione se pretende iniciar o estado de cada execução da sua aplicação lógica na análise de registos.  |

    
3. Selecione **Criar**. Quando **implementação concluída com êxito** apresenta de notificação, clique em **aceda a recursos** para abrir a sua aplicação lógica.

4. Em **Modelos**, escolha **Aplicação Lógica em Branco**. 

O Designer de aplicações lógicas agora mostra conectores disponíveis e as respetivas acionadores, que utiliza para iniciar o fluxo de trabalho de aplicação lógica.

<!-- Learn [how to create a logic app](../logic-apps/quickstart-create-first-logic-app-workflow.md). -->

### <a name="add-event-hub-trigger"></a>Adicionar o acionador de Hub de eventos

1. Na caixa de pesquisa para o Designer de aplicação lógica, escreva *dos event hubs* para o filtro. Selecione o acionador **Hubs de eventos - quando eventos estão disponíveis no Hub de eventos**.

   ![imagem da adição de Acionador de hub de eventos nas logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-trigger.png)

2. Quando lhe for pedida credenciais, ligar-se ao seu espaço de nomes de Event Hubs. Introduza um nome para a ligação e, em seguida, a cadeia de ligação que copiou.  Selecione **Criar**.

   ![imagem da adição de ligação do hub de eventos nas logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-add-connection.png)

3. Depois de criar a ligação, edite as definições para o acionador. Comece por selecionar **insights registos operacionais** do **nome de Hub de eventos** pendente.

   ![Quando os eventos são diálogo disponível](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-read-events.png)

5. Expanda **Mostrar opções avançadas** e alterar **tipo de conteúdo** para *application/json*

   ![Caixa de diálogo de configuração de hub de eventos](media/log-analytics-activity-logs-subscriptions/logic-apps-event-hub-configuration.png)

### <a name="add-parse-json-action"></a>Adicionar ação analisar JSON

A saída do Hub de eventos contém um payload JSON com uma matriz de registos. O [analisar JSON](../logic-apps/logic-apps-content-type.md) ação é utilizada para extrair apenas a matriz de registos para enviar para análise de registos.

1. Clique em **novo passo** > **adicionar uma ação**
2. Na caixa de pesquisa, escreva *analisar json* para o filtro. Selecione a ação **operações de dados - analisar JSON**.

   ![A adição de ação de json de analisar as logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-add-parse-json-action.png)

3. Clique no **conteúdo** campo e, em seguida, selecione *corpo*.

4. Copie e cole o seguinte esquema para o **esquema** campo.  Este esquema corresponde o resultado da ação de Hub de eventos.  

   ![Analisar a caixa de diálogo de json](media/log-analytics-activity-logs-subscriptions/logic-apps-parse-json-configuration.png)

``` json-schema
{
    "properties": {
        "body": {
            "properties": {
                "ContentData": {
                    "type": "string"
                },
                "Properties": {
                    "properties": {
                        "ProfileName": {
                            "type": "string"
                        },
                        "x-opt-enqueued-time": {
                            "type": "string"
                        },
                        "x-opt-offset": {
                            "type": "string"
                        },
                        "x-opt-sequence-number": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                },
                "SystemProperties": {
                    "properties": {
                        "EnqueuedTimeUtc": {
                            "type": "string"
                        },
                        "Offset": {
                            "type": "string"
                        },
                        "PartitionKey": {},
                        "SequenceNumber": {
                            "type": "number"
                        }
                    },
                    "type": "object"
                }
            },
            "type": "object"
        },
        "headers": {
            "properties": {
                "Cache-Control": {
                    "type": "string"
                },
                "Content-Length": {
                    "type": "string"
                },
                "Content-Type": {
                    "type": "string"
                },
                "Date": {
                    "type": "string"
                },
                "Expires": {
                    "type": "string"
                },
                "Location": {
                    "type": "string"
                },
                "Pragma": {
                    "type": "string"
                },
                "Retry-After": {
                    "type": "string"
                },
                "Timing-Allow-Origin": {
                    "type": "string"
                },
                "Transfer-Encoding": {
                    "type": "string"
                },
                "Vary": {
                    "type": "string"
                },
                "X-AspNet-Version": {
                    "type": "string"
                },
                "X-Powered-By": {
                    "type": "string"
                },
                "x-ms-request-id": {
                    "type": "string"
                }
            },
            "type": "object"
        }
    },
    "type": "object"
}
```

>[!TIP]
> Pode obter um payload de exemplo ao clicar em **executar** e observar o **em bruto de saída** do Hub de eventos.  Em seguida, pode utilizar este resultado com **payload de exemplo de utilização para gerar o esquema** no **analisar JSON** atividade para gerar o esquema.

### <a name="add-compose-action"></a>Adicionar ação Compose
O [Compose](../logic-apps/logic-apps-workflow-actions-triggers.md#compose-action) ação demora a saída JSON e cria um objeto que pode ser utilizado pela ação de análise de registos.

1. Clique em **novo passo** > **adicionar uma ação**
2. Tipo *compor* para o seu filtro e, em seguida, selecione a ação **operações de dados - compor**.

    ![Adicionar ação Compose](media/log-analytics-activity-logs-subscriptions/logic-apps-add-compose-action.png)

3. Clique em de **entradas** campo e selecione **corpo** sob o **analisar JSON** atividade.


### <a name="add-log-analytics-send-data-action"></a>Adicionar ação de enviar dados do Log Analytics
O [Recoletor de dados de análise de registos do Azure](https://docs.microsoft.com/connectors/azureloganalyticsdatacollector/) ação demora o objeto da ação Compose e envia-a para análise de registos.

1. Clique em **novo passo** > **adicionar uma ação**
2. Tipo *Iniciar análise* para o seu filtro e, em seguida, selecione a ação **Recoletor de dados de análise de registos do Azure - enviar dados**.

   ![Análise de registos ao adicionar envia ação dados em logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-connector.png)

3. Introduza um nome para a sua ligação e cole o **ID da área de trabalho** e **chave da área de trabalho** para a sua área de trabalho de análise de registos.  Clique em **Criar**.

   ![A adicionar a ligação de análise do registo nas logic apps](media/log-analytics-activity-logs-subscriptions/logic-apps-log-analytics-add-connection.png)

4. Depois de criar a ligação, edite as definições na tabela abaixo. 

    ![Configure a ação de envio de dados](media/log-analytics-activity-logs-subscriptions/logic-apps-send-data-to-log-analytics-configuration.png)

   |Definição        | Valor           | Descrição  |
   |---------------|---------------------------|--------------|
   |Corpo do pedido JSON  | **Saída** do **Compose** ação | Obtém os registos a partir do corpo da ação Compose. |
   | Nome de registo personalizado | AzureActivity | Nome da tabela de registo personalizado para criar na análise de registos para armazenar os dados importados. |
   | campo Hora gerado | hora | Não selecione o campo JSON para **tempo** -escreva a hora do word. Se selecionar o campo JSON coloca o estruturador de **enviar dados** ação para um *para cada* ciclo, ou seja, não como pretendido. |




10. Clique em **guardar** para guardar as alterações efetuadas à sua aplicação lógica.

## <a name="step-4---test-and-troubleshoot-the-logic-app"></a>Passo 4 - teste e resolver problemas com a aplicação lógica
Com o fluxo de trabalho concluído, pode testar no designer para verificar se está a funcionar sem erros.

No Designer de aplicação lógica, clique em **executar** para testar a aplicação lógica. Cada passo na aplicação lógica mostra um ícone de estado, com uma marca de verificação branco num círculo verde a indicar sucesso.

   ![Aplicação de lógica de teste](media/log-analytics-activity-logs-subscriptions/test-logic-app.png)

Para ver informações detalhadas sobre cada passo, clique no nome do passo para expandir. Clique em **Mostrar entradas em bruto** e **Mostrar saídas não processadas** para obter mais informações sobre os dados recebidos e enviados em cada passo.

## <a name="step-5---view-azure-activity-log-in-log-analytics"></a>Passo 5 - ver o registo de atividade do Azure na análise de registos
É o último passo para verificar a área de trabalho de análise de registos para se certificar de que os dados estão a ser recolhidos conforme esperado.

1. No portal do Azure, selecione **Log Analytics**.
2. Selecione a sua área de trabalho e, em seguida, o **pesquisa registo** mosaico.
3. Na barra de consulta de pesquisa, escreva `AzureActivity_CL` e clique no botão de procura. Se não tiver nome o início de sessão personalizado *AzureActivity*, escreva o nome que escolheu e acrescentar `_CL`.

>[!NOTE]
> Na primeira vez que um novo registo personalizado é enviado para análise de registos, pode demorar uma hora para o registo personalizado para ser pesquisáveis.

>[!NOTE]
> Os registos de atividade são escritos para uma tabela personalizada e não apresentar o [solução de registo de atividade](./log-analytics-activity.md).


![Aplicação de lógica de teste](media/log-analytics-activity-logs-subscriptions/log-analytics-results.png)

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, criou uma aplicação lógica para ler os registos de atividade do Azure a partir de um Hub de eventos e enviá-los para análise de registos para análise. Para obter mais informações sobre como visualizar dados na análise de registos, incluindo criar dashboards, consulte o tutorial para visualizar dados.

> [!div class="nextstepaction"]
> [Visualizar o tutorial de dados de pesquisa de registo](./log-analytics-tutorial-dashboards.md)
