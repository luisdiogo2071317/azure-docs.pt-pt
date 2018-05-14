---
title: Automatizar a criação de relatórios personalizadas com dados do Azure Application Insights
description: Automatizar relatórios semanais/diariamente/mensal personalizados com dados do Azure Application Insights
services: application-insights
documentationcenter: ''
author: sdash
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: sdash
ms.openlocfilehash: 804e8c7a43d1ab16d11b6075be44599b33b46a3e
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Automatizar a criação de relatórios personalizadas com dados do Azure Application Insights

Periodical relatórios ajudam a manter uma equipa informada sobre a forma como os respetivos serviços críticos de negócio estão a fazer. Os programadores, as equipas de DevOps/SRE e os seus gestores podem ser produtivos com relatórios automatizados fiável entrega insights sem necessidade de todas as pessoas iniciar sessão no portal. Esses relatórios também podem ajudar a identificar o aumento gradual na latências, taxas de carga ou a falha que não podem acionar qualquer alerta regras.

Cada empresa tem as Reporting Services necessidades exclusivas, tais como: 

* Agregações de percentil específico de métricas ou métricas personalizadas num relatório.
* Ter diferentes relatórios para diárias, semanais e mensais roll-ups dos dados para audiências diferentes.
* Segmentação de atributos personalizados como região, ou o ambiente. 
* Agrupe alguns recursos de AI num único relatório mesmo que poderá estar em diferentes subscrições ou recurso grupos, etc.
* Separar relatórios que contém as métricas confidenciais enviadas para o público-alvo seletiva.
* Relatórios para intervenientes quem podem não ter acesso aos recursos portais.

> [!NOTE] 
> O e-mail de texto implícita Application Insights semanal não permitiu a qualquer personalização e será descontinuado favor opções personalizadas listadas abaixo. Última semanal resumida correio eletrónico será enviado em 11 de Junho de 2018. Configure uma das seguintes opções para obter relatórios personalizados semelhantes (utilize a consulta sugerida abaixo).

## <a name="to-automate-custom-report-emails"></a>Para automatizar e-mails de relatório personalizado

Pode [programaticamente consultar Application Insights](https://dev.applicationinsights.io/) dados para gerar relatórios personalizados com base numa agenda. As opções seguintes podem ajudá-lo a começar rapidamente a utilizar:

* [Automatizar a criação de relatórios com o Microsoft Flow](app-insights-automate-with-flow.md)
* [Automatizar a criação de relatórios com Logic Apps](automate-with-logic-apps.md)
* Utilize o "Application Insights agendado Resumo" [função do Azure](https://azure.microsoft.com/services/functions/) modelo no cenário de monitorização. Esta função utiliza SendGrid para entregar o e-mail. 

    ![Modelo de função do Azure](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Consulta de exemplo para um e-mail de texto implícita semanal
A consulta seguinte mostra a associação entre vários conjuntos de dados para um e-mail de texto implícita semanal, como o relatório. Personalize-o conforme necessário e utilizá-la com qualquer uma das opções listadas acima para automatizar um relatório semanal.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

  
## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre como criar [as consultas de análises](app-insights-analytics-using.md).
- Saiba mais sobre [consultar programaticamente os dados do Application Insights](https://dev.applicationinsights.io/)
- Saiba mais sobre o [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).
- Saiba mais sobre [Microsoft Flow](https://ms.flow.microsoft.com).


