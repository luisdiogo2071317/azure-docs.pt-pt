---
title: Automatizar a criação de relatórios personalizadas com dados do Azure Application Insights
description: Automatizar relatórios semanais/diariamente/mensal personalizados com dados do Azure Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: c8cff54c67ab2c9c3d09f9261617b6312cc4434a
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025954"
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
* Utilize o "Application Insights agendado Resumo" [função do Azure](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) modelo no cenário de monitorização. Esta função utiliza SendGrid para entregar o e-mail. 

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

## <a name="application-insights-scheduled-digest-report"></a>Relatório do Application Insights resumida agendada

1. A partir do portal do Azure, selecione **crie um recurso** > **computação** > **aplicação de função**.

   ![Criar uma captura de ecrã da aplicação de função de recursos do Azure](./media/automate-custom-reports/function-app-01.png)

2. Introduza as informações adequadas para a aplicação e selecione _criar_. (Application Insights _no_ só é necessário se pretender monitorizar a nova aplicação de função com o Application Insights)

   ![Criar uma captura de ecrã de definições de aplicação de função de recursos do Azure](./media/automate-custom-reports/function-app-02.png)

3. Depois de concluída a implementação, a nova aplicação de função selecione **aceda a recursos**.

4. Selecione **nova função**.

   ![Criar uma nova captura de ecrã de função](./media/automate-custom-reports/function-app-03.png)

5. Selecione o  **_Application Insights resumida agendada modelo_**.

   ![Captura de ecrã de função Application Insights modelo novo](./media/automate-custom-reports/function-app-04.png)

6. Introduza um endereço de e-mail do destinatário adequado para o relatório e selecione **criar**.

   ![Captura de ecrã de definições de função](./media/automate-custom-reports/function-app-05.png)

7. Selecione o **aplicação de função** > **funcionalidades da plataforma** > **definições da aplicação**.

    ![Captura de ecrã de definições do Azure a aplicação de função](./media/automate-custom-reports/function-app-07.png)

8. Crie três novas definições de aplicação com valores correspondentes adequados ``AI_APP_ID``, ``AI_APP_KEY``, e ``SendGridAPI``. Selecione **Guardar**.

     ![Captura de ecrã de interface de integração de função](./media/automate-custom-reports/function-app-08.png)
    
    (Os valores AI_ podem ser encontrados na acesso à API para o recurso de informações de aplicação que pretende comunicar. Se não tiver uma chave de API do Application Insights, há a opção para **criar chave de API**.)
    
    * AI_APP_ID = o ID da aplicação
    * AI_APP_KEY = chave de API
    * SendGridAPI = SendGrid chave de API

    > [!NOTE]
    > Se não tiver uma conta do SendGrid que pode criar uma. Documentação do SendGrid para as funções do Azure é [aqui](https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-sendgrid). Se apenas pretende uma explicação de como SendGrid o programa de configuração e gerar uma chave de API um é fornecido no final deste artigo mínima. 

9. Selecione **integrar** e em clique saídas **SendGrid ($return)**.

     ![Captura de ecrã de saída](./media/automate-custom-reports/function-app-09.png)

10. Sob o **definição de aplicação de chave SendGridAPI**, selecione a definição de aplicação criado recentemente para **SendGridAPI**.

     ![Execute a captura de ecrã da aplicação de função](./media/automate-custom-reports/function-app-010.png)

11. Executar e testar a aplicação de função.

     ![Captura de ecrã do teste](./media/automate-custom-reports/function-app-11.png)

12. Verifique o correio eletrónico para confirmar que a mensagem foi enviado/recebido com êxito.

     ![Captura de ecrã de linha de assunto de correio electrónico](./media/automate-custom-reports/function-app-12.png)

## <a name="sendgrid-with-azure"></a>SendGrid com o Azure

Estes passos só se aplicam se ainda não tiver uma conta do SendGrid configurada.

1. No portal do Azure, selecione **crie um recurso** procurar **entrega de correio eletrónico do SendGrid** > clique **criar** > e preencher saída específico do SendGrid criar instruções. 

     ![Criar o recurso do SendGrid captura de ecrã](./media/automate-custom-reports/function-app-13.png)

2. Depois de criar em contas do SendGrid selecionar **gerir**.

     ![Captura de ecrã de chave de API de definições](./media/automate-custom-reports/function-app-14.png)

3. Este procedimento inicia o site do SendGrid. Selecione **definições** > **chaves de API**.

     ![Criar e ver a captura de ecrã de aplicação de chave de API](./media/automate-custom-reports/function-app-15.png)

4. Criar uma chave de API > Escolha **Criar & vista** (. Reveja a documentação do SendGrid no acesso restrito para determinar o nível de permissões é apropriado para a sua chave de API. Acesso total é selecionado aqui exemplo apenas para fins de.)

   ![Captura de ecrã de acesso total](./media/automate-custom-reports/function-app-16.png)

5. Copie a chave completa, este valor é o que precisa de nas definições da aplicação de função como o valor para SendGridAPI

   ![Copie a captura de ecrã de chave de API](./media/automate-custom-reports/function-app-17.png)

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre como criar [as consultas de análises](app-insights-analytics-using.md).
* Saiba mais sobre [consultar programaticamente os dados do Application Insights](https://dev.applicationinsights.io/)
* Saiba mais sobre o [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).
* Saiba mais sobre [Microsoft Flow](https://ms.flow.microsoft.com).
