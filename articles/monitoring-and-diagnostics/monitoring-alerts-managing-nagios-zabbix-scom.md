---
title: Gerir alertas a partir de outros serviços de monitorização
description: Gestão de alertas do Nagios, Zabbix e do SCOM no Azure Monitor
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 0a3e0f1ecc40213aca37e37e80c9ba35abedb3d6
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961219"
---
# <a name="manage-alerts-from-other-monitoring-services"></a>Gerir alertas a partir de outros serviços de monitorização

Agora, pode ver os alertas do Nagios e do System Center Operations Manager no Zabbix a [experiência de alertas unificada](https://aka.ms/azure-alerts-overview). Estes alertas provenientes de integrações com servidores da Nagios/Zabbix ou System Center Operations Manager para o Log Analytics. 

## <a name="prerequisites"></a>Pré-requisitos
Quaisquer registos no repositório do Log Analytics com o tipo de alerta serão importados para a experiência de alertas unificada, pelo que tem de efetuar a configuração necessária para recolher estes registos.
1. Para **Nagios** e **Zabbix** alertas, [configurar esses servidores](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-linux-agents) para enviar alertas para o Log Analytics.
1. Para **System Center Operations Manager** alertas, [ligar o seu grupo de gestão do Operations Manager à sua área de trabalho do Log Analytics](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-om-agents). Todos os alertas criados no System Center Operations Manager são importados para o Log Analytics.

## <a name="view-your-alert-instances"></a>Ver as suas instâncias de alerta
Assim que tiver configurado a importação para o Log Analytics, deve começar a ver alerta instâncias destes monitorizar os serviços no [experiência de alertas unificada](https://aka.ms/azure-alerts-overview). Assim que estiverem presentes na experiência de alertas unificada, pode [gerir instâncias de alerta](https://aka.ms/managing-alert-instances), [gerir smart grupos criados nestes alertas](https://aka.ms/managing-smart-groups) e [alterar o estado dos seus alertas e inteligente grupos](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  Alertas do Nagios na experiência unificada de alertas não têm monitorização de estado – por exemplo, o [monitorizar a condição](https://aka.ms/azure-alerts-overview) de um alerta não vai de "Fired" como "Resolvido". Em vez disso, o "Fired" e "Resolvido" são apresentadas como instâncias separadas de alerta. 
