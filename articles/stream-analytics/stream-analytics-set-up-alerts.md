---
title: Configurar a monitorização de alertas para tarefas do Azure Stream Analytics
description: Este artigo descreve como utilizar o portal do Azure para configurar a monitorização e alertas para tarefas do Azure Stream Analytics.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/26/2017
ms.openlocfilehash: fb69f1b52e5b17dd6794c896969c29003a929c9b
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-set-up-alerts-for-azure-stream-analytics-jobs"></a>Tutorial: Configurar alertas para tarefas do Azure Stream Analytics
Pode configurar alertas para acionar um alerta quando uma métrica atinge uma condição que especificar. Por exemplo, pode configurar um alerta para uma condição como o seguinte:

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

Regras pode ser configuradas nas métricas através do portal, ou podem ser configuradas [programaticamente](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) sobre dados de registos de operações.

## <a name="set-up-alerts-in-the-azure-portal"></a>Configurar alertas no portal do Azure
1. No portal do Azure, abra a tarefa de Stream Analytics que pretende criar um alerta para. 

2. No **tarefa** painel, clique em de **monitorização** secção.  

3. No **métrica** painel, clique em de **Adicionar alerta** comando.

      ![Configuração do portal do Azure](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Introduza um nome e uma descrição.

5. Utilize os seletores para definir a condição em que o alerta será enviado.

6. Fornece informações sobre onde deve passar o alerta.

      ![Configurar um alerta para uma tarefa de análise de transmissão em fluxo do Azure](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Para obter mais detalhes sobre como configurar alertas no portal do Azure, consulte [receber notificações de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Obter ajuda
Para mais assistência, tente ler o nosso [fórum do Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Passos Seguintes
* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-get-started.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

