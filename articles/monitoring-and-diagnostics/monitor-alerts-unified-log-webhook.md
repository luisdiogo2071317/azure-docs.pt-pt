---
title: As ações de Webhook para alertas de registo nos alertas do Azure | Microsoft Docs
description: Este artigo descreve como para uma regra de alerta de registo utilizando o registo analytics ou o application insights, transmite dados como HTTP webhook e detalhes sobre as personalizações diferentes possíveis.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 28c8e6ab6a23a46bdea31c71b08b9c6a28d1be33
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
---
# <a name="webhook-actions-for-log-alert-rules"></a>Ações de Webhook para regras de alerta de registo
Quando um [alerta é criado no Azure ](monitor-alerts-unified-usage.md), tem a opção de [configuração utilizar grupos de ação](monitoring-action-groups.md) para efetuar uma ou mais ações.  Este artigo descreve as ações de webhook diferentes que estão disponíveis e detalhes sobre como configurar o webhook com base em JSON personalizado.


## <a name="webhook-actions"></a>Ações de Webhook

As ações de Webhook permitem-lhe invocar um processo externo através de um pedido de HTTP POST único.  O serviço que está a ser chamado deve suportar webhooks e determinar a forma como utiliza qualquer payload recebe.   Exemplos de como utilizar um webhook em resposta a um alerta estão a enviar uma mensagem [Slack](http://slack.com) ou criar um incidente no [PagerDuty](http://pagerduty.com/).  

As ações de Webhook requerem as propriedades na tabela seguinte:

| Propriedade | Descrição |
|:--- |:--- |
| URL do Webhook |O URL do webhook. |
| Payload JSON personalizado |Payload personalizado para enviar com o webhook, quando esta opção é escolhida durante a criação do alerta. Detalhes disponíveis em [gerir alertas através de alertas do Azure ](monitor-alerts-unified-usage.md) |

> [!NOTE]
> Testar o botão de Webhook juntamente com *incluir payload JSON personalizado do webhook* opção para alerta de registo, irá acionar fictício chamada para testar o URL do webhook. Não contém dados real e representativo do esquema JSON utilizado para os alertas de registo. 

Webhooks incluir um URL e um payload formatado em JSON que é os dados enviados para o serviço externo.  Por predefinição, o payload inclui os valores na tabela seguinte: pode optar por substituir este payload por um personalizado da sua própria.  Nesse caso pode utilizar as variáveis na tabela para cada um dos parâmetros para incluir o respetivo valor no payload personalizado.


| Parâmetro | Variável | Descrição |
|:--- |:--- |:--- |
| AlertRuleName |#alertrulename |Nome da regra de alerta. |
| Gravidade |#severity |Gravidade definida para o alerta de registo desencadeou. |
| AlertThresholdOperator |#thresholdoperator |Operador de limiar para a regra de alerta.  *Maior* ou *menor*. |
| AlertThresholdValue |#thresholdvalue |Valor de limiar para a regra de alerta. |
| LinkToSearchResults |#linktosearchresults |Ligar a pesquisa de registo de análise de registos que devolve os registos da consulta que criou o alerta. |
| O parâmetro ResultCount |#searchresultcount |Número de registos nos resultados da pesquisa. |
| Hora de fim do intervalo de pesquisa |#searchintervalendtimeutc |Hora de fim para a consulta em formato UTC. |
| Intervalo de pesquisa |#searchinterval |Janela de tempo para a regra de alerta. |
| StartTime de intervalo de pesquisa |#searchintervalstarttimeutc |Hora de início para a consulta em formato UTC. 
| SearchQuery |#searchquery |Consulta de pesquisa de registo utilizada pela regra de alerta. |
| SearchResults |"IncludeSearchResults": VERDADEIRO|Registos devolvidos pela consulta como uma tabela de JSON, limitado para os primeiro 1000 registos; Se "IncludeSearchResults": true foi adicionado na definição de webhook JSON personalizada como uma propriedade de nível superior. |
| WorkspaceID |#workspaceid |ID da sua área de trabalho de análise de registos. |
| ID da aplicação |#applicationid |ID do Application Insight aplicação. |
| ID da subscrição |#subscriptionid |ID de subscrição do Azure utilizados com o Application Insights. 


Por exemplo, poderá especificar o payload personalizado seguinte, que inclui um único parâmetro chamado *texto*.  O serviço que chama o webhook seria possível esperar este parâmetro.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Este payload de exemplo seria resolver algo semelhante ao seguinte quando enviados para o webhook.

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```

Para incluir os resultados da pesquisa no payload personalizado, certifique-se de que **IncudeSearchResults** está definido como uma propriedade de nível superior no json payload. 

## <a name="sample-payloads"></a>Payloads de exemplo
Esta secção mostra o payload de exemplo do webhook para alertas de registo, incluindo payload é padrão e a respetiva personalizada.

> [!NOTE]
> Para garantir a retrocompatibilidade, o payload de webhook padrão para alertas através de Log Analytics do Azure é igual ao [análise de registos de gestão de alertas](../log-analytics/log-analytics-alerts-creating.md). Mas para os alertas de registo utilizando [Application Insights](../application-insights/app-insights-analytics.md), o payload de webhook padrão baseia-se no esquema de ação grupo.

### <a name="standard-webhook-for-log-alerts"></a>Webhook Standard para os alertas de registo 
Ambos estes exemplos tiverem indicado um payload fictício com apenas duas colunas e linhas de dois.

#### <a name="log-alert-for-azure-log-analytics"></a>Alerta de registo para análise de registos do Azure
Segue-se um payload de exemplo para uma ação do webhook padrão *sem opção de Json personalizado* a ser utilizado para alertas com base na análise do registo.

```json
{
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://workspaceID.portal.mms.microsoft.com/#Workspace/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Warning"
 }
 ```   

#### <a name="log-alert-for-azure-application-insights"></a>Alerta de registo para o Azure Application Insights
Segue-se um payload de exemplo para um webhook padrão *sem opção de Json personalizado* quando utilizado para o application insights-alertas com base em registo-.
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule","SearchQuery":"search *",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://analytics.applicationinsights.io/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "Error",
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Alerta de registo com o Payload JSON personalizado
Por exemplo, para criar um payload personalizado que inclua apenas o nome do alerta e os resultados da pesquisa, pode utilizar o seguinte: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

Segue-se um payload de exemplo para uma ação de webhook personalizadas para qualquer alerta de registo.
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [registar alertas nos alertas do Azure ](monitor-alerts-unified-log.md)
- Criar e gerir [grupos de ação no Azure](monitoring-action-groups.md)
- Saiba mais sobre [Application Insights](../application-insights/app-insights-analytics.md)
- Saiba mais sobre [Log Analytics](../log-analytics/log-analytics-overview.md). 