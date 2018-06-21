---
title: Utilizar o portal do Azure para criar alertas clássicas para serviços do Azure | Microsoft Docs
description: Acionar mensagens de correio eletrónico ou notificações ou chame URLs de Web site (webhooks) ou automatização quando são satisfeitas as condições que especificar.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/23/2016
ms.author: robb
ms.component: alerts
ms.openlocfilehash: d0e5512eb3f963898ded6d7155f8b75cfb6ef911
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36287434"
---
# <a name="use-the-azure-portal-to-create-classic-metric-alerts-in-azure-monitor-for-azure-services"></a>Utilize o portal do Azure para criar alertas métricas clássicas no Monitor do Azure para serviços do Azure 

> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>

> [!NOTE]
> Este artigo descreve como criar alertas de métricas clássicas mais antigas. Azure suporta agora a monitorizar [alertas métricas mais recentes](monitoring-near-real-time-metric-alerts.md). 


Este artigo mostra como configurar alertas de métricas do Azure clássicas através do portal do Azure. 

Pode receber um alerta com base nas métricas para os serviços do Azure ou pode receber alertas de eventos que ocorrem no Azure.

* **Valores métricos**: O alerta é acionado quando o valor de uma métrica especificado atravesse um limiar que atribuir em qualquer direção. Ou seja, aciona ambas quando a condição for satisfeita primeiro e, em seguida, quando que já não está a ser condição.    

* **Eventos de registo de atividade**: pode acionar um alerta num *cada* eventos ou quando ocorrem determinados eventos. Saiba mais sobre [alertas de registo de atividade](monitoring-activity-log-alerts.md).

Pode configurar um alerta de métrico clássico para fazer o seguinte quando aciona:

* Envie notificações por e-mail para o administrador de serviços e coadministradores.
* Envie correio eletrónico para os endereços de e-mail adicionais que especificar.
* Chame um webhook.
* Inicie a execução de um runbook do Azure (apenas a partir do portal do Azure).

Pode configurar e obter informações sobre regras de alerta métricas clássicas das seguintes localizações: 

* [Portal do Azure](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [CLI do Azure](insights-alerts-command-line-interface.md)
* [API de REST de Monitor do Azure](https://msdn.microsoft.com/library/azure/dn931945.aspx)

## <a name="create-an-alert-rule-on-a-metric-with-the-azure-portal"></a>Criar uma regra de alerta numa métrica com o portal do Azure
1. No [portal](https://portal.azure.com/), localize o recurso que pretende monitorizar e, em seguida, selecioná-lo.

2. No **monitorização** secção, selecione **alertas (clássica)**. Ícone de texto e podem variar ligeiramente diferentes recursos. Se não encontrar **alertas (clássica)** aqui, poderá achar no **alertas** ou **regras de alerta**.

    ![Monitorização](./media/insights-alerts-portal/AlertRulesButton.png)

3. Selecione o **Adicionar alerta métrica (clássica)** comando e, em seguida, preencha os campos.

    ![Adicionar Alerta](./media/insights-alerts-portal/AddAlertOnlyParamsPage.png)

4. **Nome** a regra de alerta. Em seguida, escolha um **Descrição**, que também é apresentado em notificações por e-mail.

5. Selecione o **métrica** que pretende monitorizar. Em seguida, escolha um **condição** e **limiar** valor para a métrica. Também escolher o **período** de tempo que a regra de métrica deve ser satisfeita antes dos acionadores de alerta. Por exemplo, se utilizar o período "nos últimos 5 minutos" e o alerta procura uma CPU superior a 80%, o alerta acionado quando a CPU foi consistentemente acima 80% para 5 minutos. Após o primeiro acionador ocorre, que é acionado novamente quando a CPU permanece inferior a 80% para 5 minutos. A medição de métrica de CPU acontece a cada minuto.

6. Selecione **proprietários de E-Mail...**  se pretender que os administradores e coadministradores para receber notificações por e-mail quando o alerta é acionado.

7. Se pretender enviar notificações para endereços de e-mail adicionais quando o alerta é acionado, adicioná-los no **administrador adicionais email(s)** campo. Separe várias mensagens de correio eletrónico com ponto e vírgula, no seguinte formato:  *email@contoso.com; email2@contoso.com*

8. Colocar um URI válido no **Webhook** campo se quiser ser chamado quando o alerta é acionado.

9. Se utilizar a automatização do Azure, pode selecionar um runbook a ser executada quando o alerta é acionado.

10. Selecione **OK** para criar o alerta.   

Dentro de alguns minutos, o alerta está ativo e aciona conforme descrito anteriormente.

## <a name="manage-your-alerts"></a>Gerir os alertas
Depois de criar um alerta, pode selecioná-lo e efetue uma das seguintes tarefas:

* Ver um gráfico que mostra o limiar de métrico e os valores reais do dia anterior.
* Editar ou eliminá-lo.
* **Desativar** ou **ativar** -se de que pretende temporariamente interromper ou retomar a receção de notificações para esse alerta.

## <a name="next-steps"></a>Passos Seguintes
* [Obter uma descrição geral da monitorização do Azure](monitoring-overview.md), incluindo os tipos de informações que pode recolher e monitorizar.
* Saiba mais sobre o [alertas métricas mais recentes](monitoring-near-real-time-metric-alerts.md).
* Saiba mais sobre [configurar webhooks alertas](insights-webhooks-alerts.md).
* Saiba mais sobre [configurar alertas de eventos de registo de atividade](monitoring-activity-log-alerts.md).
* Saiba mais sobre [runbooks de automatização do Azure](../automation/automation-starting-a-runbook.md).
* Obter um [descrição geral dos registos de diagnóstico](monitoring-overview-of-diagnostic-logs.md)e recolher métricas de alta frequência detalhadas do seu serviço.
* Obter um [descrição geral da coleção de métricas](insights-how-to-customize-monitoring.md) para se certificar de que o serviço está disponível e reativa.
