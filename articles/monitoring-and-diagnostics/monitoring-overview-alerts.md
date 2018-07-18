---
title: Descrição geral dos alertas Clássicos no Microsoft Azure e o Azure Monitor
description: Alertas permitem-lhe monitorizar métricas de recurso do Azure, eventos ou os registos e ser notificado quando for cumprida uma condição que especificar.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: a0abcbdaa7e998413efb717be6e0addc5607ec5c
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114016"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>O que são alertas Clássicos no Microsoft Azure?

> [!NOTE]
> Este artigo descreve como criar alertas de métricas mais antigas e clássicas. Agora suporta o Monitor do Azure [alertas de métricas de tempo quase real, mais novos e uma nova experiência de alertas](monitoring-overview-unified-alerts.md). 
>

Ao utilizar alertas, pode configurar as condições sobre os dados e ser notificado quando as condições corresponderem aos dados monitorização mais recentes.


## <a name="alerts-on-azure-monitor-data"></a>Alertas de dados do Azure Monitor
Existem dois tipos de alertas clássicos disponíveis: alertas de métricas e alertas de registo de atividade.

* **Alertas de métricas clássicas**: este alerta é acionada quando o valor de uma métrica especificado ultrapassar um limiar que atribuir. O alerta gera uma notificação quando o it da "ativado" (quando o limiar é cruzado e for cumprida a condição do alerta). Ele também gera um alerta quando este estiver "resolvido" (quando o limiar é cruzado novamente e já não for cumprida a condição). 

* **Alertas de registo de atividade clássica**: este alerta de registo de transmissão em fluxo aciona quando um evento de registo de atividade é gerado que corresponde ao filtrar os critérios que atribuiu. Estes alertas têm apenas um Estado, "ativado", porque o mecanismo de alerta simplesmente aplica-se os critérios de filtro para qualquer novo evento. Estes alertas podem notificá-lo quando ocorre um novo incidente do Estado de funcionamento do serviço ou quando um utilizador ou aplicação realiza uma operação na sua subscrição, como "eliminar máquina virtual."

Para receber registos dados de diagnóstico disponíveis que está disponíveis através do Azure Monitor, encaminhar os dados para o Log Analytics (anteriormente conhecido como o Operations Management Suite) e utilize um alerta de consulta do Log Analytics. Utiliza agora de análise de registo do [novos alertas método](monitoring-overview-unified-alerts.md). 

O diagrama seguinte resume as origens de dados no Azure Monitor e sugere como pode alertar desativar estes dados.

![Alertas explicadas](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="taxonomy-of-azure-monitor-alerts-classic"></a>Taxonomia do Azure Monitor de alertas (clássico)
O Azure utiliza os seguintes termos para descrever alertas clássicos e suas funções:
* **Alerta**: A definição dos critérios (um ou mais regras ou condições), o que torna-se ativado quando cumpridos.
* **Active Directory**: O Estado que ocorre quando os critérios que é definido por um alerta clássico é cumprido.
* **Resolvido**: O Estado que ocorre quando os critérios que é definido por um alerta clássico já não for cumprida após ter anteriormente foram cumpridos.
* **Notificação**: A ação que é executada com base em quando um alerta clássico fica ativo.
* **Ação**: A chamada específica que é enviada para o destinatário de uma notificação (por exemplo, uma mensagem de e-mail ou um post para um URL de webhook). Notificações, normalmente, podem acionar várias ações.

## <a name="how-do-i-receive-notifications-from-an-azure-monitor-classic-alert"></a>Como faço para receber notificações de um alerta clássico do Azure Monitor?
Historicamente, os alertas do Azure a partir de diferentes serviços utilizados seus próprios métodos de notificação internos. 

Agora o Azure Monitor oferece chamado de agrupamento de notificação reutilizáveis *grupos de ação*. Grupos de ação de especificar um conjunto de recetores para uma notificação. Quando um alerta é ativado que referencia o grupo de ação, todos os recetores recebem essa notificação. Esta funcionalidade permite-lhe reutilizar um agrupamento de recetores (por exemplo, sua lista de engenheiro na chamada) em vários objetos de alerta. Grupos de ação suportam a notificação através de vários métodos. Esses métodos podem incluir o lançamento para um URL do webhook, envio de e-mails, mensagens SMS e diversas outras ações. Para obter mais informações, consulte [criar e gerir grupos de ação no portal do Azure](monitoring-action-groups.md). 

Alertas de registo de atividade clássica mais antigas, utilize grupos de ação.

No entanto, os alertas de métricas mais antigos não utilizam grupos de ação. Em vez disso, pode configurar as seguintes ações: 
* Envie notificações por e-mail para o administrador de serviços, coadministradores, ou para endereços de e-mail adicionais que especificar.
* Chame um webhook, o que permite-lhe iniciar ações adicionais de automatização.

Webhooks habilitar a automação e remediação, por exemplo, utilizando os seguintes serviços:
- Runbook da Automatização do Azure
- Funções do Azure
- Aplicação lógica do Azure
- Um serviço de terceiros

## <a name="next-steps"></a>Passos Seguintes
Obter informações sobre regras de alerta e como configurá-los utilizando a seguinte documentação:

* Saiba mais sobre [métricas](monitoring-overview-metrics.md)
* Configurar [alertas de métrica clássicas com o portal do Azure](insights-alerts-portal.md)
* Configurar [alertas de métrica clássicas com o PowerShell](insights-alerts-powershell.md)
* Configurar [alertas de métrica clássicas com a CLI do Azure](insights-alerts-command-line-interface.md)
* Configurar [alertas de métricas clássicas utilizando a API de REST do Azure Monitor](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Saiba mais sobre [registos de atividades](monitoring-overview-activity-logs.md)
* Configurar [alertas de registo de atividade com o portal do Azure](monitoring-activity-log-alerts.md)
* Configurar [alertas de registo de atividade com Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Reveja o [esquema de webhook de alerta de registo de atividades](monitoring-activity-log-alerts-webhook.md)
* Saiba mais sobre [grupos de ação](monitoring-action-groups.md)
* Configurar [alertas mais recentes](monitor-alerts-unified-usage.md)
