---
title: Criar alertas para os serviços do Azure - portal do Azure | Microsoft Docs
description: Acionador e-mails, as notificações, chamar URLs de Web sites (webhooks) ou automatização quando forem cumpridas condições que especificar.
author: rboucher
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb
ms.openlocfilehash: b0d938112aaea4d86dd539b53a1749cc800607a7
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-classic-metric-alerts-in-azure-monitor-for-azure-services---azure-portal"></a>Criar alertas métricas clássicas no Monitor do Azure para serviços do Azure - portal do Azure
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Descrição geral

> [!NOTE]
> Este artigo descreve como criar alertas de métricas clássicas mais antigas. Azure suporta agora a monitorizar [alertas métricas mais recentes](monitoring-near-real-time-metric-alerts.md). 
>
>

Este artigo mostra como configurar alertas métricas clássicas do Azure no portal do Azure. 

Pode receber um alerta com base na monitorização métricas para ou eventos nos seus serviços do Azure.

* **Valores métricos** -o alerta é acionado quando o valor de uma métrica especificado atravesse um limiar atribuir em qualquer direção. Ou seja, aciona ambas quando a condição for satisfeita primeiro e, em seguida, posteriormente quando condição que é já não está a ser cumprido.    
* **Eventos de registo de atividade** -pode acionar um alerta num *cada* eventos ou apenas quando ocorre determinado evento. Saiba mais sobre [alertas de registo de atividade](monitoring-activity-log-alerts.md).

Pode configurar um alerta de métrico clássico para fazer o seguinte quando aciona:

* enviar notificações por e-mail para o administrador de serviços e coadministradores
* envie correio eletrónico para e-mails adicionais que especificar.
* Chamar um webhook
* iniciar a execução de um runbook do Azure (apenas a partir do portal do Azure)

Pode configurar e obter informações sobre regras de alerta métricas clássicas a utilizar

* [Portal do Azure](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Interface de linha de comandos (CLI)](insights-alerts-command-line-interface.md)
* [API de REST de Monitor do Azure](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Criar uma regra de alerta numa métrica com o portal do Azure
1. No [portal](https://portal.azure.com/), localize o recurso estiver interessado na monitorização e selecione-o.

2. Selecione **alertas (clássica)** na secção monitorização. Ícone de texto e podem variar devido às ligeiramente diferentes recursos. Se não encontrar **alertas (clássica)**, poderá encontrá-los em **alertas** ou **regras de alertas**

    ![Monitorização](./media/insights-alerts-portal/AlertRulesButton.png)

3. Selecione o **Adicionar alerta métrica (clássica)** de comandos e preencha os campos.

    ![Adicionar Alerta](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Nome** o alerta de regra e escolha um **Descrição**, que também mostra nos e-mails de notificação.

5. Selecione o **métrica** que pretende monitorizar, em seguida, escolha um **condição** e **limiar** valor para a métrica. Também escolher o **período** de tempo que a regra de métrica deve ser satisfeita antes dos acionadores de alerta. Por isso, por exemplo, se utilizar o período "nos últimos 5 minutos" e o alerta procura CPU superior a 80%, o alerta acionado quando a CPU foi consistentemente acima 80% para 5 minutos. Depois de ocorre o primeiro acionador, novamente acionado quando a CPU permanece inferior a 80% para 5 minutos. A medição de métrica de CPU ocorre a cada minuto.

6. Verifique **proprietários de E-Mail...**  se pretender que os administradores e coadministradores para serem enviadas por e-mail quando o alerta é acionado.

7. Se pretender que os e-mails adicionais para receber uma notificação quando o alerta é acionado, adicioná-los no **administrador adicionais email(s)** campo. Separe várias mensagens de correio eletrónico com ponto e vírgula -  *email@contoso.com;email2@contoso.com*

8. Colocar um URI válido no **Webhook** campo se quiser chamou quando o alerta é acionado.

9. Se utilizar a automatização do Azure, pode selecionar um Runbook a ser executada quando o alerta é acionado.

10. Selecione **OK** quando pronto para criar o alerta.   

Dentro de alguns minutos, o alerta está ativo e aciona conforme descrito anteriormente.

## <a name="managing-your-alerts"></a>Gerir os alertas
Assim que tiver criado um alerta, pode selecionar e:

* Ver um gráfico que mostra o limiar de métrico e os valores reais do dia anterior.
* Editar ou eliminá-lo.
* **Desativar** ou **ativar** -se de que pretende temporariamente interromper ou retomar a receção de notificações para esse alerta.

## <a name="next-steps"></a>Passos seguintes
* [Obter uma descrição geral da monitorização do Azure](monitoring-overview.md) , incluindo os tipos de informações que pode recolher e monitorizar.
* Saiba mais sobre o [alertas métricas mais recentes](monitoring-near-real-time-metric-alerts.md)
* Saiba mais sobre [configurar webhooks alertas](insights-webhooks-alerts.md).
* Saiba mais sobre [configurar alertas de eventos de registo de atividade](monitoring-activity-log-alerts.md).
* Saiba mais sobre [Runbooks de automatização do Azure](../automation/automation-starting-a-runbook.md).
* Obter um [descrição geral dos registos de diagnóstico](monitoring-overview-of-diagnostic-logs.md) e recolher métricas de alta frequência detalhadas do seu serviço.
* Obter um [descrição geral da coleção de métricas](insights-how-to-customize-monitoring.md) para se certificar de que o serviço está disponível e reativa.
