---
title: Criar, ver e gerir alertas de registo utilizando o Azure Monitor
description: Utilize o Azure Monitor para criar, ver e gerir regras de alerta de registo no Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/15/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6e6db7ef2f2de075aea41a29e69b2c674efafa17
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635361"
---
# <a name="create-view-and-manage-log-alerts-using-azure-monitor"></a>Criar, ver e gerir alertas de registo com o Azure Monitor  

## <a name="overview"></a>Descrição geral
Este artigo mostra-lhe como configurar alertas de registo utilizando a interface de alertas no portal do Azure. Definição de uma regra de alerta está nas três partes:
- Destino: Recursos do Azure específica, que está a monitorizar
- Critérios: Condição específica ou a lógica que quando visto no sinal, deve acionar a ação
- Ação: Chamada específica enviada para um recetor de uma notificação - enviar um e-mail, SMS, webhook, etc.

O termo **alertas de registo** para descrever os alertas em que o sinal é a consulta personalizada com base nos [do Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) ou [Application Insights](../application-insights/app-insights-analytics.md). Saiba mais sobre funcionalidade, terminologia e tipos a partir [alertas - descrição geral de registo](monitor-alerts-unified-log.md).

> [!NOTE]
> Dados de registos populares [do Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) também está agora disponível na plataforma de métrica no Azure Monitor. Para a vista de detalhes, [alerta de métrica para os registos](monitoring-metric-alerts-logs.md)

## <a name="managing-log-alerts-from-the-azure-portal"></a>Gerir alertas de registo a partir do portal do Azure

Próxima detalhada é um guia passo a passo para utilizar os alertas de registo através da interface do portal do Azure.

### <a name="create-a-log-alert-rule-with-the-azure-portal"></a>Criar uma regra de alerta de registo com o portal do Azure
1. Na [portal](https://portal.azure.com/), selecione **Monitor** e, na secção MONITOR - escolher **alertas**.  
    ![Monitorização](media/alert-log/AlertsPreviewMenu.png)

1. Selecione o **nova regra de alerta** botão para criar um novo alerta no Azure.
    ![Adicionar alerta](media/alert-log/AlertsPreviewOption.png)

1. A secção de criar o alerta é mostrada com três partes consiste em: *definir a condição de alerta*, *definir detalhes do alerta*, e *Definir grupo de ação*.

    ![Criar regra](media/alert-log/AlertsPreviewAdd.png)

1.  Definir a condição de alerta com o **selecionar recurso** ligação e especificando o destino ao selecionar um recurso. Filtro ao selecionar o _subscrição_, _tipo de recurso_e necessária _recurso_. 

    >[!NOTE]

    > Para criar um registo de alerta – verificar a **log** sinal está disponível para o recurso selecionado antes de continuar.
    ![Selecione o recurso](media/alert-log/Alert-SelectResourceLog.png)

 
1. *Alertas de registo*: Certifique-se **tipo de recurso** é uma origem de análise, como *do Log Analytics* ou *Application Insights* e sinalizar tipo como **registo** , em seguida, uma vez adequado **recurso** está selecionado, clique em *feito*. Em seguida utilize o **adicionar critérios** botão para ver a lista de opções de sinal disponíveis para o recurso e a partir da lista de sinal **pesquisa de registos personalizado** opção para escolhida iniciar o monitor de serviço, como *registo Análise* ou *Application Insights*.

   ![Selecione um recurso - pesquisa de registos personalizado](media/alert-log/AlertsPreviewResourceSelectionLog.png)

   > [!NOTE]

   > Alertas de listas, podem importar consulta do analytics como tipo de sinal - **Log (consulta guardada)**, como mostra acima ilustração. Para que os utilizadores possam aperfeiçoa sua consulta do Analytics e, em seguida, guarde-as para utilização futura nos alertas - mais detalhes sobre como utilizar a guardar consulta disponível em [utilizando a pesquisa de registos no log analytics](../log-analytics/log-analytics-queries.md) ou [consulta partilhada no application insights análise](../log-analytics/log-analytics-queries.md). 

1.  *Alertas de registo*: depois de selecionada, a consulta para alertas pode ser indicada na **consulta de pesquisa** campo; se a sintaxe de consulta está incorreta o campo apresenta o erro em vermelho. Se a sintaxe de consulta está correta – para referência, histórico de dados da consulta declarado é mostrado como um gráfico com a opção para ajustar a janela de tempo da última seis horas da semana passada.

 ![Configurar a regra de alerta](media/alert-log/AlertsPreviewAlertLog.png)

 > [!NOTE]

    > Visualização de dados históricos pode ser apresentada apenas se os resultados da consulta tem os detalhes de tempo. Se a sua consulta resulta em dados resumidos ou valores de coluna de específicos - mesmo é mostrado como um desenho singular.

    >  Para o tipo de medida da métrica de alertas de registo com o Application insights, pode especificar qual variável específico para agrupar os dados utilizando o **agregada no** opção; conforme ilustrado na abaixo:

    ![Agregar na opção](media/alert-log/aggregate-on.png)

1.  *Alertas de registo*: com a visualização no local, **Alert Logic** podem ser selecionados a partir das opções apresentadas da condição, a agregação e, finalmente, limiar. Por fim especifique na lógica de tempo para avaliar a condição especificada, utilizando **período** opção. Juntamente com a frequência com que o alerta deve executar selecionando **frequência**.
Para **alertas de registo** alertas podem ser baseados em:
   - *Número de registos*: é criado um alerta se a contagem de registos devolvidos pela consulta for maior ou menor que o valor fornecido.
   - *Medida da métrica*: é criado um alerta se cada *agregar valor* nos resultados excede o valor de limiar fornecido e é *agrupados por* escolhido o valor. O número de falhas de um alerta é o número de vezes que o limite é excedido no período de tempo escolhido. Pode especificar o Total de falhas para qualquer combinação de violações de entre o conjunto de resultados ou falhas consecutivas para exigir que as violações devem ocorrer no amostras consecutivas. Saiba mais sobre [alertas de registo e os tipos de](monitor-alerts-unified-log.md).


1. Como o segundo passo, defina um nome para o alerta no **nome da regra de alerta** campo juntamente com um **Descrição** com detalhes sobre as especificações para o alerta e **gravidade** partir das opções fornecidas. Esses detalhes são reutilizados em todos os e-mails de alerta, notificações ou push feita pelo Azure Monitor. Além disso, o utilizador pode optar por ativar imediatamente a regra de alerta após a criação, ativando adequadamente **ativar regra após a criação** opção.

    Para **alertas de registo** apenas, algumas funcionalidades adicionais estão disponível nos detalhes do alerta:

    - **Suprimir alertas**: Quando ativar a supressão para a regra de alerta, as ações da regra estão desativadas para um comprimento de definidos de tempo depois de criar um novo alerta. A regra ainda está em execução e cria registos de alerta, desde que os critérios são cumpridos. Permitindo que tempo para corrigir o problema sem executar ações duplicadas.

        ![Suprimir alertas para alertas de registo](media/alert-log/AlertsPreviewSuppress.png)

        > [!TIP]
        > Especifique um valor alerta suppress superior à frequência do alerta para garantir a notificações são paradas sem sobreposição

1. Como a terceira e última etapa, especifique se houver **grupo de ação** tem de ser acionada para a regra de alerta quando for cumprida a condição do alerta. Pode escolher qualquer grupo de ação existente com o alerta ou crie um novo grupo de ação. Em conformidade com selecionado o grupo de ação, quando o alerta é o acionador do Azure será: enviar email(s), Enviar SMS(s), chamar o webhook (s), remediar através de Runbooks do Azure, push para a ferramenta ITSM, entre outras. Saiba mais sobre [grupos de ação](monitoring-action-groups.md).

    > [!NOTE]
    > Consulte a [limites de serviço de subscrição do Azure](../azure-subscription-service-limits.md) para limites em cargas de Runbook acionadas para alertas de registo através de grupos de ação do Azure

    Para **alertas de registo** algumas funcionalidades adicionais estão disponíveis para substituir as ações padrão:

    - **Notificação por e-mail**: substitui *assunto do e-mail* no e-mail, enviado por grupo de ação; se uma ou mais ações de e-mail do grupo de ação disse. Não é possível modificar o corpo da mensagem e este campo é **não** para endereço de e-mail.
    - **Incluir payload Json personalizado**: substitui o JSON utilizado por grupos de ação do webhook, se uma ou mais ações de webhook do grupo de ação disse. Utilizador pode especificar o formato do JSON a ser utilizado para todos os webhooks configurados no grupo de ação associado; Para obter mais informações sobre formatos de webhook, veja [ação do webhook para alertas de registo](monitor-alerts-unified-log-webhook.md). Opção de Webhook do modo de exibição é fornecida para verificar o formato com dados JSON de exemplo.

        ![Substituições de ação para alertas de registo](media/alert-log/AlertsPreviewOverrideLog.png)


1. Se todos os campos são válidos e com escala verde a **criar regra de alerta** botão pode ser clicado e é criado um alerta no Azure Monitor - alertas. Todos os alertas podem ser visualizados a partir os alertas do Dashboard.

    ![Criação de regras](media/alert-log/AlertsPreviewCreate.png)

    Em poucos minutos, o alerta está ativo e aciona conforme descrito anteriormente.

Os utilizadores podem também finalizado sua consulta do analytics num [página de análise de registos no portal do Azure](../log-analytics/log-analytics-log-search-portals.md#log-analytics-page
) e, em seguida, enviá-la para criar um alerta através do botão 'Definir alerta' -, em seguida, seguir instruções da etapa 6 ou posterior no tutorial anterior.

 ![O log Analytics - definir alerta](media/alert-log/AlertsAnalyticsCreate.png)

### <a name="view--manage-log-alerts-in-azure-portal"></a>Ver e gerir alertas de registo no portal do Azure

1. Na [portal](https://portal.azure.com/), selecione **Monitor** e, na secção MONITOR - escolher **alertas**.  

1. O **Dashboard alertas** é apresentado - na qual todos os alertas do Azure (incluindo alertas de registo) são apresentados num quadro singular; incluindo todas as instâncias de quando regra do alerta do seu registo foi disparado. Para obter mais informações, consulte [gestão de alertas](https://aka.ms/managealertinstances).
    > [!NOTE]
    > Regras de alerta de registo compõem de lógica personalizada com base na consulta fornecida por utilizadores e, por conseguinte, sem estado resolvido. Devido a que cada vez que as condições especificadas na regra de alerta de registo são cumpridas, ele é disparado. 


1. Selecione o **gerir regras** botão na barra superior, para navegar para a secção de gestão de regra - onde estão apresentadas os todas as regras de alerta criadas; incluindo alertas que foram desativadas.
    ![ Gerir regras de alerta](media/alert-log/manage-alert-rules.png)

## <a name="managing-log-alerts-using-azure-resource-template"></a>Gerir alertas de registo com o modelo de recursos do Azure
Atualmente registo de alertas podem ser criados através de dois modelos de recursos diferentes, com a base de sobre qual plataforma de análise, o alerta é se baseie (ou seja) o Log Analytics ou o Application Insights.

Por conseguinte, a secção abaixo fornecem detalhes sobre como usar o modelo do Resource para alertas de registo para cada plataforma de análise.

### <a name="azure-resource-template-for-log-analytics"></a>Modelo de recursos do Azure para o Log Analytics
Alertas de registo para o Log Analytics são criadas por regras de alerta que executam uma procura guardada num intervalo regular. Se os resultados da correspondência de consulta especificados critérios, é criado um registo de alerta e uma ou mais ações são executadas. 

Modelo de recurso para pesquisa e alertas do Log analytics de guardada do Log analytics estão disponíveis na seção do Log Analytics da documentação. Para obter mais informações, consulte [adicionando o Log Analytics guardar pesquisas e alertas](../azure-monitor/insights/solutions-resources-searches-alerts.md); que inclui exemplos ilustrativos, bem como detalhes de esquema.

### <a name="azure-resource-template-for-application-insights"></a>Modelo de recursos do Azure para o Application Insights
Alerta de registo de recursos do Application Insights tem um tipo de `Microsoft.Insights/scheduledQueryRules/`. Para obter mais informações sobre este tipo de recurso, consulte [do Azure Monitor - referência da API de regras de consulta agendada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

Segue-se a estrutura para [criação de regras de consulta agendada](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) com base em modelo de recursos, com o conjunto de dados de exemplo como variáveis.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
    "variables": {
    "alertLocation": "southcentralus",
    "alertName": "samplelogalert",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
    "alertDesription": "Sample log search alert",
    "alertStatus": "true",
    "alertSource":{
        "Query":"requests",
        "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
        "Type":"ResultCount"
         },
     "alertSchedule":{
         "Frequency": 15,
         "Time": 60
         },
     "alertActions":{
         "SeverityLevel": "4"
         },
      "alertTrigger":{
        "Operator":"GreaterThan",
        "Threshold":"1"
         },
       "actionGrp":{
        "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
        "Subject": "Customized Email Header",
        "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"           
         }
  },
  "resources":[ {
    "name":"[variables('alertName')]",
    "type":"Microsoft.Insights/scheduledQueryRules",
    "apiVersion": "2018-04-16",
    "location": "[variables('alertLocation')]",
    "tags":{"[variables('alertTag')]": "Resource"},
    "properties":{
       "description": "[variables('alertDesription')]",
       "enabled": "[variables('alertStatus')]",
       "source": {
           "query": "[variables('alertSource').Query]",
           "dataSourceId": "[variables('alertSource').SourceId]",
           "queryType":"[variables('alertSource').Type]"
       },
      "schedule":{
           "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
           "timeWindowInMinutes": "[variables('alertSchedule').Time]"    
       },
      "action":{
           "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
           "severity":"[variables('alertActions').SeverityLevel]",
           "aznsAction":{
               "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
               "emailSubject":"[variables('actionGrp').Subject]",
               "customWebhookPayload":"[variables('actionGrp').Webhook]"
           },
       "trigger":{
               "thresholdOperator":"[variables('alertTrigger').Operator]",
               "threshold":"[variables('alertTrigger').Threshold]"
           }
       }
     }
   }
 ]
}
```
> [!IMPORTANT]
> Campo de etiqueta com ligação de oculto para o recurso de destino é obrigatório na utilização de [regras de consulta agendada ](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) modelo de chamada ou recurso de API. 

O json de exemplo acima podem ser salvas como (Digamos) sampleScheduledQueryRule.json com o objetivo deste passo a passo e podem ser implementado com [Gestor de recursos do Azure no portal do Azure](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).


## <a name="managing-log-alerts-using-powershell-cli-or-api"></a>Gerir alertas de registo com o PowerShell, CLI ou API
Atualmente registo de alertas podem ser criados usando duas diferentes em conformidade com APIs do Resource Manager, com a base de sobre qual plataforma de análise, o alerta é se baseie (ou seja) o Log Analytics ou o Application Insights.

Por conseguinte, a secção abaixo fornecem detalhes sobre como utilizar a API através do Powershell ou CLI para os alertas de registo para cada plataforma de análise.

### <a name="powershell-cli-or-api-for-log-analytics"></a>PowerShell, CLI ou de API para o Log Analytics
A API de REST alerta do Log Analytics é RESTful e podem ser acessada através da API de REST do Azure Resource Manager. A API, portanto, pode ser acessada a partir de uma linha de comando do PowerShell e irá enviar os resultados da pesquisa para si no formato JSON, permitindo que use os resultados de várias formas diferentes através de programação.

Saiba mais sobre [criar e gerir regras de alerta no Log Analytics com a REST API](../azure-monitor/platform/api-alerts.md), incluindo exemplos de aceder à API a partir do Powershell.

### <a name="powershell-cli-or-api-for-application-insights"></a>PowerShell, CLI ou de API do Application Insights
[O Azure Monitor - regras de consulta agendada API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) é uma REST API e totalmente compatível com a API de REST do Azure Resource Manager. Por conseguinte, pode ser utilizada através do Powershell com o cmdlet do Gestor de recursos, bem como a CLI do Azure.

Ilustrado abaixo utilização através do cmdlet do PowerShell do Azure Resource Manager para o exemplo de modelo do Resource mostrado anteriormente (sampleScheduledQueryRule.json) na [secção do modelo do Resource](#azure-resource-template-for-application-insights) :
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Ilustrado abaixo utilização através do comando do Azure Resource Manager na CLI do Azure para o exemplo de modelo do Resource mostrado anteriormente (sampleScheduledQueryRule.json) na [secção do modelo do Resource](#azure-resource-template-for-application-insights) :

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Numa operação concluída com êxito, será devolvido 201 à criação de regra de alerta do estado novo ou 200 vai ser devolvido se uma regra de alerta existente foi modificada.


  
## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [alertas de registo nos alertas do Azure](monitor-alerts-unified-log.md)
* Compreender [ações de Webhook para alertas de registo](monitor-alerts-unified-log-webhook.md)
* Saiba mais sobre [Application Insights](../application-insights/app-insights-analytics.md)
* Saiba mais sobre [do Log Analytics](../log-analytics/log-analytics-queries.md). 

