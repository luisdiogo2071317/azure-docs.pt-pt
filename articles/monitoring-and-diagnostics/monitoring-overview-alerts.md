---
title: Descrição geral dos alertas clássicas no Microsoft Azure e o Monitor do Azure | Microsoft Docs
description: Alertas permitem-lhe monitorizar métricas de recurso do Azure, eventos ou os registos e ser notificado quando for cumprida uma condição que especificar.
author: rboucher
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2018
ms.author: robb
ms.openlocfilehash: 06ba05f71cf1f696033099c04448526a0421ad42
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Quais são clássicas alertas no Microsoft Azure?

> [!NOTE]
> Este artigo descreve como criar alertas de métricas clássicas mais antigas. Azure suporta agora a monitorizar [mais recentes alertas de métrica de quase em tempo real](monitoring-overview-unified-alerts.md)
>

Este artigo descreve várias origens de alertas no Microsoft Azure, que está os fins de alertas, os benefícios e como começar a utilizá-los. Aplica-se especificamente a alertas de monitorização do Azure clássicas. Alertas oferecem um método de monitorização no Azure que permite-lhe configurar condições sobre dados e ficar notificado quando as condições corresponderem aos dados monitorização mais recentes.


## <a name="taxonomy-of-azure-monitor-classic-alerts"></a>Taxonomia de alertas de monitorização do Azure de clássicas
Azure utiliza os seguintes termos para descrever clássicas alertas e as respetivas funções:
* **Alerta** -uma definição de critérios (um ou mais regras ou condições) que torna-se ativado quando cumpridos.
* **Active Directory** -o estado quando os critérios definidos por um alerta clássico for satisfeita.
* **Resolvido** -o estado quando os critérios definidos por um alerta clássico já não for cumprida após ter anteriormente foram cumpridos.
* **Notificação** - a ação tomada baseada em termina a sessão de um alerta clássico se tornar Active Directory.
* **Ação** -uma chamada específica enviada para um recetor de uma notificação (por exemplo, relacionada um endereço ou publicação para um URL do webhook). As notificações, normalmente, podem acionar várias ações.


## <a name="alerts-on-azure-monitor-data"></a>Alertas nos dados de monitorização do Azure
Existem três tipos de alertas retire dados disponíveis a partir do Monitor do Azure – métricas alertas, quase em tempo real alertas métricas e alertas de registo de atividade.

* **Alertas de métricas clássicas** -este alerta é acionado quando o valor de uma métrica especificado atravesse um limiar que atribuir. O alerta gera uma notificação quando o alerta é "ativado" (quando o limiar for ultrapassado e a condição de alerta for cumprida), bem como quando for "resolvida" (quando o limiar é cruzado novamente e a condição já não for cumprida). Estes são alertas de métricas mais antigas. Existência de alertas de métricas mais recentes, veja a seguir.

* **Quase em tempo real alertas métricas** (experiência de alerta mais recente) - estes são mais recente geração de alertas métricas com funcionalidades melhoradas quando comparado com a alertas de métricas anteriores. Estes alertas podem ser executados uma frequência de 1 min. Também suportam várias métricas (atualmente dois) de monitorização.  O alerta gera uma notificação quando o alerta é "ativado" (quando os limiares para cada métrica são ultrapassados ao mesmo tempo e a condição de alerta for cumprida), bem como quando for "resolvida" (quando, pelo menos, uma métrica atravesse o limiar novamente e a condição é nenhuma já está preenchido).

* **Alertas de registo de atividade clássico** -um alerta de registo de transmissão em fluxo que é acionado quando é gerado um evento de registo de atividade que corresponde ao filtrar os critérios que foram atribuídos. Estes alertas tem apenas um Estado "Ativado,", uma vez que o motor de alerta simplesmente aplica-se os critérios de filtro para qualquer novo evento. Estes alertas podem ser utilizados para ser notificado quando ocorre um novo incidente de estado de funcionamento do serviço ou quando um utilizador ou aplicação efetua uma operação na sua subscrição, por exemplo, "Eliminar a máquina virtual."

Para dados de registo de diagnóstico disponíveis através do Monitor do Azure, sugerimos que encaminhar os dados para análise de registos e a utilização de um alerta de análise de registos. O diagrama a seguir resume as origens de dados no Monitor do Azure e, concecionais, como podem alertá retire dados.

![Alertas explicadas](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="how-do-i-receive-a-notification-on-an-azure-monitor-classic-alert"></a>Como posso receber uma notificação num alerta clássico do Azure Monitor?
Historicamente, os alertas do Azure a partir de diferentes serviços utilizado os seus próprios métodos de notificação incorporada. Doravante, o Monitor de Azure oferece um agrupamento de notificação reutilizáveis denominado grupos de ação. Grupos de ação especifiquem um conjunto de recetores para uma notificação – qualquer número de endereços de correio eletrónico, números de telefone (para o SMS) ou URLs de webhook - e sempre que um alerta é ativado que referencia o grupo de ação, todos os recetores recebem essa notificação. Isto permite-lhe reutilizar um agrupamento de recetores (por exemplo, a lista de engenheiro chamada no) em muitos objetos de alerta. Atualmente, apenas os alertas de registo de atividade tornar a utilização de grupos de ação, mas estão a funcionar utilizando a ação grupos, bem como vários outros tipos de alerta do Azure.

Grupos de ação suportam notificações através da publicação para um URL do webhook para além dos endereços de e-mail e números SMS. Isto permite a automatização e remediação, por exemplo, utilizando:
    - Runbook da Automatização do Azure
    - Função do Azure
    - Aplicação lógica do Azure
    - um serviço de terceiros

Quase em tempo real métrica alertas e alertas de registo de atividade utilizam grupos de ação.

Alertas de métricas mais antigas disponíveis em alertas (clássica) não utilizam grupos de ação. Num alerta métrico individual pode configurar notificações para:
* Envie notificações por e-mail para o administrador de serviço para os coadministradores ou para endereços de e-mail adicionais que especificar.
* Chame um webhook, o que permite iniciar ações adicionais de automatização.

## <a name="next-steps"></a>Passos seguintes
Obter informações sobre regras de alertas e configurá-los utilizando:

* Saiba mais sobre [métricas](monitoring-overview-metrics.md)
* Configurar [clássico de métrica de alertas, através do portal do Azure](insights-alerts-portal.md)
* Configurar [PowerShell de alertas de métrica clássico](insights-alerts-powershell.md)
* Configurar [interface de linha de comandos de alertas de métrica clássico (CLI)](insights-alerts-command-line-interface.md)
* Configurar [clássico métrica alertas Monitor API REST do Azure](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Saiba mais sobre [registo de atividade](monitoring-overview-activity-logs.md)
* Configurar [alertas de registo de atividade através do portal do Azure](monitoring-activity-log-alerts.md)
* Configurar [alertas de registo de atividade através do Gestor de recursos](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Reveja o [esquema de webhook alerta de registo de atividade](monitoring-activity-log-alerts-webhook.md)
* Saiba mais sobre [mais recente de métrica de alertas](monitoring-near-real-time-metric-alerts.md)
* Saiba mais sobre [notificações de serviço](monitoring-service-notifications.md)
* Saiba mais sobre [grupos de ação](monitoring-action-groups.md)
* Configurar [alertas](monitor-alerts-unified-usage.md)
