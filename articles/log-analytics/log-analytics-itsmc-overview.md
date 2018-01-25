---
title: "IT Service conector de gestão na análise de registos do Azure | Microsoft Docs"
description: "Este artigo fornece uma descrição geral de TI serviço de gestão. o conector (ITSMC) e informações sobre como utilizar esta solução para monitorizar e gerir o ITSM centralmente itens de trabalho no Log Analytics do Azure e resolva os problemas rapidamente."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: v-jysur
ms.openlocfilehash: d586ee1b96b34d6ca83e1ffd76aee38e79bdd727
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="connect-azure-to-itsm-tools-using-it-service-management-connector"></a>Ligar o Azure com ferramentas de ITSM utilizando o conector de gestão do serviço de TI

![Símbolo do conector de gestão do serviço IT](./media/log-analytics-itsmc/itsmc-symbol.png)

O conector de gestão do serviço de TI (ITSMC) permite-lhe ligar do Azure e um suportados produtos/serviços de gestão de serviço de TI (ITSM).

Serviços do Azure, como a análise de registos e Monitor do Azure fornecem ferramentas para detetar, analisar e resolver problemas com o Azure e os recursos do Azure não. No entanto, os itens de trabalho relacionados com um problema normalmente residem em ITSM produtos/serviços. O conector ITSM fornece uma ligação de bidirecional entre o Azure e ITSM ferramentas para ajudar a resolver problemas mais rapidamente.

ITSMC suporta ligações com as ferramentas ITSM seguintes:

-   ServiceNow
-   O System Center Service Manager
-   Provance
-   Cherwell

Com ITSMC, pode

-  Crie itens de trabalho na ferramenta ITSM, com base na sua alertas do Azure (alertas métricas, alertas de registo de atividade e alertas de análise de registos).
-  Opcionalmente, pode sincronizar o seu incidente e dados de pedido de alteração da sua ferramenta ITSM para uma área de trabalho do Log Analytics do Azure.


Pode começar a utilizar o conector de ITSM utilizando os seguintes passos:

1.  [Adicionar a solução de conector ITSM](#adding-the-it-service-management-connector-solution)
2.  [Criar uma ligação de ITSM](#creating-an-itsm-connection)
3.  [Utilize a ligação](#using-the-solution)


##  <a name="adding-the-it-service-management-connector-solution"></a>Adicionar o IT Service a solução de gestão de conector

Antes de poder criar uma ligação, terá de adicionar a solução de conector ITSM.

1.  No portal do Azure, clique em **+ novo** ícone.

    ![Novo recurso do Azure](./media/log-analytics-itsmc/azure-add-new-resource.png)

2.  Procurar **conector de gestão de serviços de TI** no Marketplace e clique **criar**.

    ![Adicionar solução ITSMC](./media/log-analytics-itsmc/add-itsmc-solution.png)

3.  No **área de trabalho OMS** secção, selecione a área de trabalho do Log Analytics do Azure onde pretende instalar a solução.
4.  No **as definições de área de trabalho do OMS** secção, selecione o ResourceGroup onde pretende criar o recurso de solução.

    ![Área de trabalho ITSMC](./media/log-analytics-itsmc/itsmc-solution-workspace.png)

5.  Clique em **Criar**.

Quando o recurso de solução é implementado, é apresentada uma notificação na parte superior direito da janela.


## <a name="creating-an-itsm--connection"></a>Criar uma ligação de ITSM

Depois de ter instalado a solução, pode criar uma ligação.

Para criar uma ligação, terá de prep a sua ferramenta ITSM para permitir a ligação da solução ITSM conector.  

Consoante o produto ITSM que está a ligar, utilize os seguintes passos:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-azure)
- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-azure)
- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-azure)  
- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-azure)

Depois de ter prepped as ferramentas ITSM, siga os passos abaixo para criar uma ligação:

1.  Aceda a **todos os recursos**, procure **ServiceDesk(YourWorkspaceName)**.
2.  Em **ORIGENS de dados da área de trabalho** no painel esquerdo, clique em **ITSM ligações**.
    ![Ligações de ITSM](./media/log-analytics-itsmc/itsm-connections.png)

    Esta página apresenta a lista de ligações.
3.  Clique em **adicionar ligação**.

    ![Adicionar ligação ITSM](./media/log-analytics-itsmc/add-new-itsm-connection.png)

4.  Especifique as definições de ligação conforme descrito em [configurar a ligação de ITSMC com o seu artigo de produtos/serviços ITSM](log-analytics-itsmc-connections.md).

    > [!NOTE]

    > Por predefinição, o ITSMC atualiza os dados de configuração da ligação uma vez em cada 24 horas. Para atualizar os dados da ligação de forma instantânea para qualquer modelo ou as edições de atualizações que efetuar, clique no botão de "Atualizar" apresentado junto a ligação.

    ![Atualização de ligação](./media/log-analytics-itsmc/itsmc-connections-refresh.png)


## <a name="using-the-solution"></a>Utilizar a solução
   Ao utilizar a solução de conector ITSM, pode criar itens de trabalho de alertas do Azure, os alertas de análise de registos e registos de análise de registos.

## <a name="create-itsm-work-items-from-azure-alerts"></a>Criar itens de trabalho ITSM a partir dos alertas do Azure

Assim que tiver a ligação de ITSM criada, pode criar itens de trabalho na sua ferramenta ITSM com base em alertas do Azure, utilizando o **ITSM ação** no **ação grupos**.

Grupos de ação proporcionam uma forma modular e reutilizável de a acionar ações para os alertas do Azure. Pode utilizar grupos de ação com alertas métricas, alertas de registo de atividade e alertas do Log Analytics do Azure no portal do Azure.

Utilize o seguinte procedimento:

1. No portal do Azure, clique em **Monitor**.
2. No painel esquerdo, clique em **grupos ação**. O **adicionar grupo de ação** surge a janela.

    ![Grupos de Ação](media/log-analytics-itsmc/action-groups.png)

3. Fornecer **nome** e **ShortName** para o grupo de ação. Selecione o **grupo de recursos** e **subscrição** onde pretende criar o grupo de ação.

    ![Detalhe de grupos de ação](media/log-analytics-itsmc/action-groups-details.png)

4. Na lista de ações, selecione **ITSM** no menu pendente para **tipo de ação**. Forneça um **nome** para a ação e clique em **editar detalhes**.
5. Selecione o **subscrição** onde está localizada a sua área de trabalho de análise de registos. Selecione o **ligação** name (nome do seu conector ITSM), seguido pelo nome da sua área de trabalho. Por exemplo, "MyITSMMConnector(MyWorkspace)".

    ![Detalhes de ação de ITSM](./media/log-analytics-itsmc/itsm-action-details.png)

6. Selecione **Item de trabalho** tipo a partir do menu pendente.
   Optar por utilizar um modelo existente ou preencher os campos necessários pelo seu produto ITSM.
7. Clique em **OK**.

Quando a criação/edição de uma regra de alerta do Azure, utilize um grupo de ação, que tem uma ação de ITSM. Quando o alerta é acionado, o item de trabalho é criado/atualizar na ferramenta de ITSM.

>[!NOTE]

> Para obter informações sobre os preços de ITSM ação, consulte o [página de preços](https://azure.microsoft.com/pricing/details/monitor/) para grupos de ação.


## <a name="create-itsm-work-items-from-log-analytics-alerts"></a>Criar itens de trabalho ITSM a partir dos alertas de análise de registos

Pode configurar regras de alertas no portal do Log Analytics do Azure para criar itens de trabalho na ferramenta ITSM, utilizando o procedimento seguinte.

1. De **pesquisa registo** janela, executar uma consulta de pesquisa de registo para ver os dados. Os resultados da consulta são a origem para itens de trabalho.
2. No **pesquisa registo**, clique em **alerta** para abrir o **Adicionar regra de alerta** página.

    ![Ecrã de análise do registo](./media/log-analytics-itsmc/itsmc-work-items-for-azure-alerts.png)

3. No **Adicionar regra de alerta** janela, forneça os detalhes necessários para **nome**, **gravidade**, **consulta de pesquisa**, e **alerta critérios** (medida janela/métrica de tempo).
4. Selecione **Sim** para **ITSM ações**.
5. Selecione a ligação ITSM a partir de **selecione ligação** lista.
6. Forneça os detalhes conforme necessário.
7. Para criar um item de trabalho separada para cada entrada de registo deste alerta, selecione o **criar itens de trabalho individuais para cada entrada de registo** caixa de verificação.

    Ou

    Deixe esta caixa de verificação desmarcada para criar apenas um item de trabalho para qualquer número de entradas de registo neste alerta.

7. Clique em **Guardar**.

Pode ver o alerta de análise de registos que criou em **definições > alertas**. Itens de trabalho a ligação ITSM correspondentes são criados quando condição o alerta especificado for satisfeita.


## <a name="create-itsm-work-items-from-log-analytics-log-records"></a>Criar itens de trabalho ITSM a partir de registos de análise de registos

Também pode criar itens de trabalho nas origens ITSM ligadas diretamente a partir de um registo. Isto pode ser utilizado para testar se a ligação está a funcionar corretamente.


1. De **pesquisa registo**, procure os dados necessários, selecione os detalhes e clique em **Criar item de trabalho**.

    O **Criar Item de trabalho ITSM** surge a janela:

    ![Ecrã de análise do registo](media/log-analytics-itsmc/itsmc-work-items-from-azure-logs.png)

2.   Adicione os seguintes detalhes:

  - **Título do item de trabalho**: título para o item de trabalho.
  - **Descrição do item de trabalho**: uma descrição para o novo item de trabalho.
  - **Afetados computador**: nome do computador em que estes dados de registo foi encontrados.
  - **Selecione a ligação**: ligação ITSM no qual pretende criar este item de trabalho.
  - **Item de trabalho**: tipo de item de trabalho.

3. Para utilizar um modelo de item de trabalho existente para um incidente, clique em **Sim** em **item de trabalho gerar baseado num modelo** opção e, em seguida, clique em **criar**.

    Ou,

    Clique em **não** se pretender fornecer os valores personalizados.

4. Forneça os valores adequados no **contacte tipo**, **impacto**, **urgência**, **categoria**, e **Sub categoria** caixas de texto e, em seguida, clique em **criar**.


##<a name="visualize-and-analyze-the-incident-and-change-request-data"></a>Visualizar e analisar o incidente e dados de pedido de alteração

Com base na configuração quando configurar uma ligação de conector ITSM pode sincronizar máximo de 120 dias de dados de pedido de incidente e altere. O esquema de registo de registo para estes dados é fornecido no [secção seguinte](#additional-information).

Os dados de pedido de incidente e de alterações podem ser visualizados com o dashboard de conector ITSM na solução.

![Ecrã de análise do registo](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

O dashboard também fornece informações sobre o estado do conector que pode ser utilizado como um ponto de partida para analisar quaisquer problemas com as ligações.

Também pode visualizar os incidentes sincronizados contra computadores afectados, na solução de mapa de serviço.

Mapa de serviço automaticamente Deteta os componentes da aplicação em sistemas Windows e Linux e mapeia a comunicação entre os serviços. Permite-lhe ver os servidores que acha que deles – como interligados sistemas que fornecem serviços críticos. Mapa de serviço mostra as ligações entre servidores, processos, e portas em qualquer arquitetura TCP ligados sem qualquer configuração necessária à instalação de um agente. [Saiba mais](../operations-management-suite/operations-management-suite-service-map.md).

Se estiver a utilizar a solução de mapa de serviço, pode ver os itens de suporte técnico de serviço criados nas soluções ITSM conforme mostrado no exemplo seguinte:

![Ecrã de análise do registo](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)

Obter mais informações: [mapa de serviço](../operations-management-suite/operations-management-suite-service-map.md)


## <a name="additional-information"></a>Informações adicionais

### <a name="data-synced-from-itsm-product"></a>Dados sincronizados a partir do produto ITSM
Incidentes e pedidos de alteração são sincronizados a partir do seu produto ITSM à sua área de trabalho de análise de registos com base na configuração da ligação.

As seguintes informações mostram exemplos de dados recolhidos pelo ITSMC:

> [!NOTE]

> Dependendo do tipo de item de trabalho importados para análise de registos, **ServiceDesk_CL** contém os seguintes campos:

**Item de trabalho:** **incidentes**  
ServiceDeskWorkItemType_s="Incident"

**Campos**

- ServiceDeskConnectionName
- ID do serviço de suporte técnico
- Estado
- Urgência
- Impacto
- Prioridade
- Escalamento
- Criado Por
- Resolvido pelo
- Fechada pelo
- Origem
- Atribuído a
- Categoria
- Cargo
- Descrição
- Data de Criação
- Data de fecho
- Data de resolução
- Data da Última Modificação
- Computador


**Item de trabalho:** **pedidos de alteração**

ServiceDeskWorkItemType_s="ChangeRequest"

**Campos**
- ServiceDeskConnectionName
- ID do serviço de suporte técnico
- Criado Por
- Fechada pelo
- Origem
- Atribuído a
- Cargo
- Tipo
- Categoria
- Estado
- Escalamento
- Estado de conflito
- Urgência
- Prioridade
- Risco
- Impacto
- Atribuído a
- Data de Criação
- Data de fecho
- Data da Última Modificação
- Data de pedido
- Data de início planeada
- A ser planeada a data de fim
- Data de início do trabalho
- Data de fim de trabalho
- Descrição
- Computador

## <a name="output-data-for-a-servicenow-incident"></a>Dados de saída para um incidente de ServiceNow

| Campo de análise do registo | Campo de ServiceNow |
|:--- |:--- |
| ServiceDeskId_s| Número |
| IncidentState_s | Estado |
| Urgency_s |Urgência |
| Impact_s |Impacto|
| Priority_s | Prioridade |
| CreatedBy_s | Abrir por |
| ResolvedBy_s | Resolvido por|
| ClosedBy_s  | Fechada pelo |
| Source_s| Tipo de contacto |
| AssignedTo_s | Atribuído a  |
| Category_s | Categoria |
| Title_s|  Breve descrição |
| Description_s|  Notas |
| CreatedDate_t|  Aberturas |
| ClosedDate_t| Fechado|
| ResolvedDate_t|Resolvido|
| Computador  | item de configuração |

## <a name="output-data-for-a-servicenow-change-request"></a>Pedido de alteração de dados de saída para um ServiceNow

| Log Analytics | Campo de ServieNow |
|:--- |:--- |
| ServiceDeskId_s| Número |
| CreatedBy_s | Pedido por |
| ClosedBy_s | Fechada pelo |
| AssignedTo_s | Atribuído a  |
| Title_s|  Breve descrição |
| Type_s|  Tipo |
| Category_s|  Categoria |
| CRState_s|  Estado|
| Urgency_s|  Urgência |
| Priority_s| Prioridade|
| Risk_s| Risco|
| Impact_s| Impacto|
| RequestedDate_t  | Pedido por data |
| ClosedDate_t | Data de fecho |
| PlannedStartDate_t  |     Data de início planeada |
| PlannedEndDate_t  |   Data de fim planeada |
| WorkStartDate_t  | Data de início real |
| WorkEndDate_t | Data de fim real|
| Description_s | Descrição |
| Computador  | Item de configuração |


## <a name="troubleshoot-itsm-connections"></a>Resolver problemas de ligações de ITSM
1.  Se a ligação falhar da IU da origem ligados com um **erro na ligação de guardar** da mensagem, siga os passos seguintes:
- Para ligações de ServiceNow, Cherwell e Provance,  
       -Certifique-se corretamente introduziu o nome de utilizador, palavra-passe, ID de cliente e segredo do cliente para cada uma das ligações.  
       -Verifique se tem privilégios suficientes no produto ITSM correspondente para efetuar a ligação.  
- Para ligações do Service Manager,  
       -Certifique-se de que a aplicação Web é implementada com êxito e é criada a ligação híbrida. Para verificar a ligação é estabelecida com êxito com a máquina do Service Manager no local, visite o URL da aplicação Web como detalhadas da documentação para efetuar o [da ligação híbrida](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).  

2.  Se não está a obter sincronizados dados a partir do ServiceNow ao Log Analytics, certifique-se de que o ServiceNow instância não está suspenso. Instâncias de desenvolvimento do ServiceNow, por vezes, aceda a suspensão quando inativo durante um longo período. Relatório de outra forma, o problema.
3.  Se OMS alertas acionados mas funcionar itens não são criados no produto ITSM ou itens de configuração não são criados/ligados a itens de trabalho ou para outras informações genéricos, procure nos seguintes locais:
 -  ITSMC: A solução mostra um resumo ligações/computador de trabalho itens/etc. Clique em Mostrar o mosaico **estado do conector**, que demora a **pesquisa registo** com a consulta relevante. Observe os registos de registo com LogType_S como erro para obter mais informações.
 - **Registo de pesquisa** página: ver as informações relacionadas com erros/diretamente utilizando a consulta *tipo = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Resolver problemas de implementação de aplicação de Web do Service Manager
1.  Em caso de problemas de implementação de aplicações web, certifique-se de que tem permissões suficientes na subscrição mencionada para criar/implementar recursos.
2.  Se obtiver um **"Referência não foi definida para a instância de um objeto de objeto"** Ocorreu um erro ao executar o [script](log-analytics-itsmc-service-manager-script.md), certifique-se de que introduziu valores válidos em **configuração do utilizador** secção .
3.  Se falhar criar o espaço de nomes do service bus reencaminhamento, certifique-se de que o fornecedor de recursos necessários está registado na subscrição. Se não registado, crie manualmente o espaço de nomes do service bus reencaminhamento do portal do Azure. Também pode criá-la ao [criar a ligação híbrida](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) do portal do Azure.


## <a name="contact-us"></a>Contacte-nos

Para consultas ou comentários sobre o conector de gestão do serviço de TI, contacte-nos [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Passos Seguintes
[Adicionar ITSM produtos/serviços para o conector de gestão do serviço de TI](log-analytics-itsmc-connections.md).
