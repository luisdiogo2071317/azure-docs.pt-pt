---
title: 'Tutorial: Atividade de diagnóstico e dados de registo no Explorador de dados do Azure sem uma linha de código de ingestão'
description: Neste tutorial, irá aprender ingerir dados para o Explorador de dados do Azure sem uma linha de código e consultar dados.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: tutorial
ms.date: 2/5/2019
ms.openlocfilehash: 94ecfda127422e395cf10a4fac1bb07cd0614f5c
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416620"
---
# <a name="tutorial-ingest-data-in-azure-data-explorer-without-one-line-of-code"></a>Tutorial: Ingestão de dados no Explorador de dados do Azure sem uma linha de código

Este tutorial irá ensiná-lo como ingestão de diagnóstico e dados de registo de atividade para um cluster do Explorador de dados do Azure sem uma linha de código. Este método de ingestão simples permite-lhe começar rapidamente a consultar o Explorador de dados do Azure para análise de dados.

Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Crie tabelas e mapeamento de ingestão num banco de dados do Explorador de dados do Azure.
> * Formate os dados ingeridos através de uma política de atualização.
> * Criar uma [Hub de eventos](/azure/event-hubs/event-hubs-about) e ligá-lo ao Explorador de dados do Azure.
> * Stream dados para um Hub de eventos a partir [registos de diagnóstico do Azure Monitor](/azure/azure-monitor/platform/diagnostic-logs-overview) e [registos de atividades do Azure Monitor](/azure/azure-monitor/platform/activity-logs-overview).
> * Consulte os dados ingeridos com o Explorador de dados do Azure.

> [!NOTE]
> Crie todos os recursos na mesma região do Azure localização /. Este é um requisito para os registos de diagnóstico do Azure Monitor.

## <a name="prerequisites"></a>Pré-requisitos

* Se não tiver uma subscrição do Azure, crie uma [conta do Azure gratuita](https://azure.microsoft.com/free/) antes de começar.
* [Um cluster do Explorador de dados do Azure e a base de dados](create-cluster-database-portal.md). Neste tutorial, é o nome de base de dados *AzureMonitoring*.

## <a name="azure-monitoring-data-provider---diagnostic-and-activity-logs"></a>Fornecedor de dados - diagnóstico de monitorização do Azure e os registos de atividades

Ver e compreender os dados fornecidos pelos registos de diagnóstico e atividade de monitorização do Azure. Iremos criar um pipeline de ingestão com base nesses esquemas de dados.

### <a name="diagnostic-logs-example"></a>Exemplo de registos de diagnóstico

Registos de diagnóstico do Azure são métricas emitidas por um serviço do Azure que fornece dados sobre a operação desse serviço. Os dados são agregados com grão de tempo de 1 minuto. Cada evento de registos de diagnóstico contém um registo. Segue-se um exemplo de um esquema de eventos de métrica do Explorador de dados do Azure, na duração de consulta:

```json
{
    "count": 14,
    "total": 0,
    "minimum": 0,
    "maximum": 0,
    "average": 0,
    "resourceId": "/SUBSCRIPTIONS/F3101802-8C4F-4E6E-819C-A3B5794D33DD/RESOURCEGROUPS/KEDAMARI/PROVIDERS/MICROSOFT.KUSTO/CLUSTERS/KEREN",
    "time": "2018-12-20T17:00:00.0000000Z",
    "metricName": "QueryDuration",
    "timeGrain": "PT1M"
}
```

### <a name="activity-logs-example"></a>Exemplo de registos de atividade

Registos de atividades do Azure são registos ao nível da subscrição que contém uma coleção de registos. Os registos de fornecem informações sobre as operações executadas nos recursos da sua subscrição. Ao contrário dos registos de diagnóstico, um evento de registos de atividade tem uma matriz de registos. Precisamos de dividir essa matriz de registos mais tarde no tutorial. Segue-se um exemplo de um evento de registo de atividade para a verificação de acesso:

```json
{
    "records": [
    {
        "time": "2018-12-26T16:23:06.1090193Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Start",
        "resultSignature": "Started.",
        "durationMs": 0,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            ...
        }
    },
    {
        "time": "2018-12-26T16:23:06.3040244Z",
        "resourceId": "/SUBSCRIPTIONS/F80EB51C-C534-4F0B-80AB-AEBC290C1C19/RESOURCEGROUPS/CLEANUPSERVICE/PROVIDERS/MICROSOFT.WEB/SITES/CLNB5F73B70-DCA2-47C2-BB24-77B1A2CAAB4D/PROVIDERS/MICROSOFT.AUTHORIZATION",
        "operationName": "MICROSOFT.AUTHORIZATION/CHECKACCESS/ACTION",
        "category": "Action",
        "resultType": "Success",
        "resultSignature": "Succeeded.OK",
        "durationMs": 194,
        "callerIpAddress": "13.66.225.188",
        "correlationId": "0de9f4bc-4adc-4209-a774-1b4f4ae573ed",
        "identity": {
            "authorization": {
                ...
            },
            "claims": {
                ...
            }
        },
        "level": "Information",
        "location": "global",
        "properties": {
            "statusCode": "OK",
            "serviceRequestId": "87acdebc-945f-4c0c-b931-03050e085626"
        }
    }]
}
```

## <a name="set-up-ingestion-pipeline-in-azure-data-explorer"></a>Configurar os pipelines de ingestão no Explorador de dados do Azure 

A configuração de pipeline do Explorador de dados do Azure contém diversas etapas que incluem [ingestão de dados e criação de tabela](/azure/data-explorer/ingest-sample-data#ingest-data). Também pode manipular, mapear e atualizar os dados.

### <a name="connect-to-azure-data-explorer-web-ui"></a>Ligar à IU de Web do Explorador de dados do Azure

1. No seu Explorador de dados do Azure *AzureMonitoring* base de dados, selecione **consulta**, que irá abrir a IU web do Explorador de dados do Azure.

    ![Consulta](media/ingest-data-no-code/query-database.png)

### <a name="create-target-tables"></a>Criar tabelas de destino

Utilize a IU web do Explorador de dados do Azure para criar as tabelas de destino na base de dados do Explorador de dados do Azure.

#### <a name="diagnostic-logs-table"></a>Tabela de registos de diagnóstico

1. Criar uma tabela *DiagnosticLogsRecords* no *AzureMonitoring* da base de dados que irá receber o registo de diagnóstico registos ao utilizar o `.create table` controlar o comando:

    ```kusto
    .create table DiagnosticLogsRecords (Timestamp:datetime, ResourceId:string, MetricName:string, Count:int, Total:double, Minimum:double, Maximum:double, Average:double, TimeGrain:string)
    ```

1. Selecione **executar** para criar a tabela.

    ![Executar Consulta](media/ingest-data-no-code/run-query.png)

#### <a name="activity-logs-tables"></a>Tabelas de registos de atividades

Uma vez que a estrutura de registos de Atividades não estiver em tabela, precisará manipular os dados e expanda cada evento para um ou mais registos. Os dados não processados que irão ser ingeridos para uma tabela de nível intermediária *ActivityLogsRawRecords*. Nessa altura, os dados serão manipulados e expandidos. Os dados expandidos, em seguida, irão ser ingeridos para o *ActivityLogsRecords* através de uma política de atualização de tabela. Por conseguinte, terá de criar duas tabelas separadas para ingestão de registos de atividade.

1. Criar uma tabela *ActivityLogsRecords* no *AzureMonitoring* base de dados que irá receber os registros de log de atividade. Execute a seguinte consulta do Explorador de dados do Azure para criar a tabela:

    ```kusto
    .create table ActivityLogsRecords (Timestamp:datetime, ResourceId:string, OperationName:string, Category:string, ResultType:string, ResultSignature:string, DurationMs:int, IdentityAuthorization:dynamic, IdentityClaims:dynamic, Location:string, Level:string)
    ```

1. Criar a tabela de dados intermediários *ActivityLogsRawRecords* no *AzureMonitoring* base de dados para manipulação de dados:

    ```kusto
    .create table ActivityLogsRawRecords (Records:dynamic)
    ```

<!--
     ```kusto
     .alter-merge table ActivityLogsRawRecords policy retention softdelete = 0d
    <[Retention](/azure/kusto/management/retention-policy) for an intermediate data table is set at zero retention policy.
-->

### <a name="create-table-mappings"></a>Criar mapeamentos de tabela

 O formato de dados é `json`, por isso, o mapeamento de dados é necessário. O `json` mapeamento mapeia cada caminho json para um nome de coluna de tabela.

#### <a name="diagnostic-logs-table-mapping"></a>Mapeamento de tabela de registos de diagnóstico

Utilize a seguinte consulta para mapear os dados para a tabela:

```kusto
.create table DiagnosticLogsRecords ingestion json mapping 'DiagnosticLogsRecordsMapping' '[
{"column":"Timestamp","path":"$.time"},
{"column":"ResourceId","path":"$.resourceId"},
{"column":"MetricName","path":"$.metricName"},
{"column":"Count","path":"$.count"},
{"column":"Total","path":"$.total"},
{"column":"Minimum","path":"$.minimum"},
{"column":"Maximum","path":"$.maximum"},
{"column":"Average","path":"$.average"},
{"column":"TimeGrain","path":"$.timeGrain"}]'
```

#### <a name="activity-logs-table-mapping"></a>Mapeamento de tabelas de registos de atividade

Utilize a seguinte consulta para mapear os dados para a tabela:

```kusto
.create table ActivityLogsRawRecords ingestion json mapping 'ActivityLogsRawRecordsMapping' '[
{"column":"Records","path":"$.records"}]'
```

### <a name="create-update-policy"></a>Criar política de atualização

1. Criar uma [função](/azure/kusto/management/functions) que expande a recolha de registos, para que cada valor na coleção recebe uma linha separada. Utilize o [ `mvexpand` ](/azure/kusto/query/mvexpandoperator) operador:

    ```kusto
    .create function ActivityLogRecordsExpand() {
        ActivityLogsRawRecords
        | mvexpand events = Records
        | project
            Timestamp = todatetime(events["time"]),
            ResourceId = tostring(events["resourceId"]),
            OperationName = tostring(events["operationName"]),
            Category = tostring(events["category"]),
            ResultType = tostring(events["resultType"]),
            ResultSignature = tostring(events["resultSignature"]),
            DurationMs = toint(events["durationMs"]),
            IdentityAuthorization = events.identity.authorization,
            IdentityClaims = events.identity.claims,
            Location = tostring(events["location"]),
            Level = tostring(events["level"])
    }
    ```

2. Adicionar uma [atualizar política](/azure/kusto/concepts/updatepolicy) para a tabela de destino. Irá automaticamente executar a consulta em quaisquer dados ingeridos recentemente na *ActivityLogsRawRecords* tabela de dados intermediários e seus resultados em de ingestão *ActivityLogsRecords* tabela:

    ```kusto
    .alter table ActivityLogsRecords policy update @'[{"Source": "ActivityLogsRawRecords", "Query": "ActivityLogRecordsExpand()", "IsEnabled": "True"}]'
    ```

## <a name="create-an-event-hub-namespace"></a>Criar um espaço de nomes do Hub de eventos

Registos de diagnóstico do Azure permitem exportar métricas para uma conta de armazenamento ou de um Hub de eventos. Neste tutorial, podemos encaminhar as métricas através de um Hub de eventos. Irá criar um espaço de nomes de Hubs de eventos e o Hub de eventos para os registos de diagnóstico nos passos seguintes. Monitorização do Azure irá criar o Hub de eventos *insights-operational-logs* para registos de atividades.

1. Crie um hub de eventos com um modelo Azure Resource Manager no portal do Azure. Utilize o botão seguinte para iniciar a implementação. Com o botão direito e selecione **abrir numa janela nova**, pelo que pode seguir o resto dos passos neste artigo. O **implementar no Azure** botão leva-o para o portal do Azure.

    [![Implementar no Azure](media/ingest-data-no-code/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

1. Crie um espaço de nomes do Hub de eventos e um Hub de eventos para os registos de diagnóstico.

    ![Criação de Hub de Eventos](media/ingest-data-no-code/event-hub.png)

    Preencha o formulário com as seguintes informações. Utilize as predefinições para todas as definições não listadas na tabela seguinte.

    **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | Subscrição | A sua subscrição | Selecione a subscrição do Azure que quer utilizar para o hub de eventos.|
    | Grupo de recursos | *test-resource-group* | Crie um novo grupo de recursos. |
    | Localização | Selecione a região que melhor atenda às suas necessidades. | Crie o espaço de nomes do hub de eventos na mesma localização que outros recursos.
    | Nome do espaço de nomes | *AzureMonitoringData* | Escolha um nome exclusivo que identifique o seu espaço de nomes.
    | Nome do hub de eventos | *DiagnosticLogsData* | O hub de eventos encontra-se no espaço de nomes, que fornece um contentor de âmbito exclusivo. |
    | Nome do grupo de consumidores | *adxpipeline* | Crie um nome de grupo de consumidor. Permite o uso de vários aplicativos a cada um tem uma vista separada do fluxo de eventos. |
    | | |

## <a name="connect-azure-monitoring-logs-to-event-hub"></a>Ligar os registos de monitorização do Azure para o Hub de eventos

### <a name="diagnostic-logs-connection-to-event-hub"></a>Ligação de registos de diagnóstico para o Hub de eventos

Selecione um recurso do qual pretende exportar as métricas. Existem vários tipos de recursos que permitem exportar registos de diagnóstico, incluindo o espaço de nomes de Hub de eventos, o Cofre de chaves, o IoT Hub e o cluster do Explorador de dados do Azure. Neste tutorial, utilizamos o cluster do Explorador de dados do Azure como o nosso recurso.

1. Selecione o cluster de Kusto no portal do Azure.

    ![Definições de diagnóstico](media/ingest-data-no-code/diagnostic-settings.png)

1. Selecione **das definições de diagnóstico** no menu esquerdo.
1. Clique em **ativar os diagnósticos** ligação. O **as definições de diagnóstico** é aberta a janela.

    ![Janela de definições de diagnóstico](media/ingest-data-no-code/diagnostic-settings-window.png)

1. Na **as definições de diagnóstico** painel:
    1. Nome do diagnóstico de dados de registo: *ADXExportedData*
    1. Selecione **AllMetrics** caixa de verificação (opcional).
    1. Selecione **Stream para um hub de eventos** caixa de verificação.
    1. Clique em **configurar**

1. Na **hub de eventos selecione** painel Configurar a exportação para o hub de eventos que criou:
    1. **Selecione o espaço de nomes de hub de eventos** *AzureMonitoringData* no menu pendente.
    1. **Nome do hub de eventos selecione** *diagnosticlogsdata* no menu pendente.
    1. **Nome de política do hub de eventos selecione** no menu pendente.
    1. Clique em **OK**.

1. Clique em **Guardar**. O espaço de nomes do hub de eventos, o nome e o nome da política aparecerá na janela.

    ![Guardar as definições de diagnóstico](media/ingest-data-no-code/save-diagnostic-settings.png)

### <a name="activity-logs-connection-to-event-hub"></a>Ligação de registos de atividades para o Hub de eventos

1. No menu à esquerda do portal do Azure, selecione **registo de atividades**
1. **Registo de atividades** é aberta a janela. **Clique em exportar para o Hub de eventos**

    ![Registo de atividades](media/ingest-data-no-code/activity-log.png)

1. Na **exportar registo de atividades** janela:
 
    ![Exportar registos de atividade](media/ingest-data-no-code/export-activity-log.png)

    1. Selecione a sua subscrição.
    1. Na **regiões** menu pendente, escolha **Selecionar tudo**
    1. Selecione **exportar para um hub de eventos** caixa de verificação.
    1. Clique em **selecione um espaço de nomes do service bus** para abrir o **hub de eventos selecione** painel.
    1. Na **hub de eventos Select** painel, selecione um dos menus de lista pendente: a sua subscrição, o espaço de nomes do evento *AzureMonitoringData*e o nome de política do hub de eventos padrão.
    1. Clique em **OK**.
    1. Clique em **guardar** no lado superior direito da janela. Um hub de eventos com o nome *insights-operational-logs* será criado.

### <a name="see-data-flowing-to-your-event-hubs"></a>Ver o fluxo de dados para os Hubs de eventos

1. Aguarde alguns minutos até que a ligação é definida e exportação de registo de atividade para o hub de eventos é concluída. Aceda ao seu espaço de nomes do Hub de eventos para ver os hubs de eventos que criou.

    ![Hubs de eventos criados](media/ingest-data-no-code/event-hubs-created.png)

1. Ver o fluxo de dados para o Hub de eventos:

    ![Dados de Hubs de eventos](media/ingest-data-no-code/event-hubs-data.png)

## <a name="connect-event-hub-to-azure-data-explorer"></a>Ligar o Hub de eventos ao Explorador de dados do Azure

### <a name="diagnostic-logs-data-connection"></a>Ligação de dados de registos de diagnóstico

1. No seu cluster do Azure Data Explorer *kustodocs*, selecione **bases de dados** no menu à esquerda.
1. Na **bases de dados** janela, selecione o nome de base de dados *AzureMonitoring*
1. No menu da esquerda, selecione **ingestão de dados**
1. Na **ingestão de dados** janela, clique em **+ adicionar ligação de dados**
1. Na **ligação de dados** janela, introduza as seguintes informações:

    ![Ligação do Hub de Eventos](media/ingest-data-no-code/event-hub-data-connection.png)

    Origem de dados:

    **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | Nome da ligação de dados | *DiagnosticsLogsConnection* | O nome da ligação que quer criar no Azure Data Explorer.|
    | Espaço de nomes do hub de eventos | *AzureMonitoringData* | O nome que escolheu anteriormente que identifica o seu espaço de nomes. |
    | Hub de eventos | *diagnosticlogsdata* | O hub de eventos que criou. |
    | Grupo de consumidores | *adxpipeline* | O grupo de consumidores definido no hub de eventos que criou. |
    | | |

    Tabela de destino:

    Existem duas opções para o encaminhamento: *estático* e *dinâmico*. Para este tutorial, utilize o encaminhamento (predefinição), onde especifica o nome da tabela, o formato de ficheiro e o mapeamento estático. Por conseguinte, deixe **meus dados incluem informações de encaminhamento** não selecionada.

     **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | Tabela | *DiagnosticLogsRecords* | A tabela criada na *AzureMonitoring* base de dados. |
    | Formato de dados | *JSON* | Formato na tabela. |
    | Mapeamento de colunas | *DiagnosticLogsRecordsMapping* | O mapeamento que criou no *AzureMonitoring* base de dados, que mapeia os dados recebidos de JSON para os tipos de dados e os nomes de coluna de *DiagnosticLogsRecords*.|
    | | |

1. Clique em **Criar**.  

### <a name="activity-logs-data-connection"></a>Ligação de dados de registos de atividade

Repita os passos a [ligação de dados de registos de diagnóstico](#diagnostic-logs-data-connection) secção para criar a atividade de registos de ligação de dados.

1. Inserir as seguintes definições na **ligação de dados** janela:

    Origem de dados:

    **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | Nome da ligação de dados | *ActivityLogsConnection* | O nome da ligação que quer criar no Azure Data Explorer.|
    | Espaço de nomes do hub de eventos | *AzureMonitoringData* | O nome que escolheu anteriormente que identifica o seu espaço de nomes. |
    | Hub de eventos | *insights-operational-logs* | O hub de eventos que criou. |
    | Grupo de consumidores | *$Default* | Grupo de consumidores predefinido. Se for necessário, pode criar um grupo de consumidores diferentes. |
    | | |

    Tabela de destino:

    Existem duas opções para o encaminhamento: *estático* e *dinâmico*. Para este tutorial, utilize o encaminhamento (predefinição), onde especifica o nome da tabela, o formato de ficheiro e o mapeamento estático. Por conseguinte, deixe **meus dados incluem informações de encaminhamento** não selecionada.

     **Definição** | **Valor sugerido** | **Descrição do campo**
    |---|---|---|
    | Tabela | *ActivityLogsRawRecords* | A tabela criada na *AzureMonitoring* base de dados. |
    | Formato de dados | *JSON* | Formato na tabela. |
    | Mapeamento de colunas | *ActivityLogsRawRecordsMapping* | O mapeamento que criou no *AzureMonitoring* base de dados, que mapeia os dados recebidos de JSON para os tipos de dados e os nomes de coluna de *ActivityLogsRawRecords*.|
    | | |

1. Clique em **Criar**.  

## <a name="query-the-new-tables"></a>Consultar as novas tabelas

Tem um pipeline com o fluxo de dados. Ingestão por meio do cluster demora 5 minutos, por predefinição, assim, permita o fluxo de dados durante alguns minutos antes de começar a consulta.

### <a name="diagnostic-logs-table-query-example"></a>Exemplo de consulta de tabela de registos de diagnóstico

A seguinte consulta analisa dados de duração de consulta de registos de registo de diagnóstico do Explorador de dados do Azure:

```kusto
DiagnosticLogsRecords
| where Timestamp > ago(15m) and MetricName == 'QueryDuration'
| summarize avg(Average)
```

Resultados de consulta:

|   |   |
| --- | --- |
|   |  avg_Average |
|   | 00:06.156 |
| | |

### <a name="activity-logs-table-query-example"></a>Exemplo de consulta de tabela de registos de atividade

A seguinte consulta analisa dados de registos de registo de atividade do Explorador de dados do Azure:

```kusto
ActivityLogsRecords
| where OperationName == 'MICROSOFT.EVENTHUB/NAMESPACES/AUTHORIZATIONRULES/LISTKEYS/ACTION'
| where ResultType == 'Success'
| summarize avg(DurationMs)
```

Resultados de consulta:
|   |   |
| --- | --- |
|   |  avg(DurationMs) |
|   | 768.333 |
| | |

## <a name="next-steps"></a>Passos Seguintes

Aprenda a escrever muitos mais consultas nos dados extraído de Explorador de dados do Azure utilizando o seguinte artigo:

> [!div class="nextstepaction"]
> [Escrever consultas para o Explorador de dados do Azure](write-queries.md)
