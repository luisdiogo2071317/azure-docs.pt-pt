---
title: Gerir alertas a partir de outros serviços de monitorização no Azure Monitor
description: Gestão de alertas do Nagios, Zabbix e do SCOM no Azure Monitor
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: bc597d42fe89c0e03c4af1db3a935031b9043a98
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346305"
---
# <a name="manage-alerts-from-other-monitoring-services"></a>Gerir alertas a partir de outros serviços de monitorização

Agora, pode ver os alertas do Nagios e do System Center Operations Manager no Zabbix a [experiência de alertas unificada](https://aka.ms/azure-alerts-overview). Estes alertas provenientes de integrações com servidores da Nagios/Zabbix ou System Center Operations Manager para o Log Analytics. 

## <a name="prerequisites"></a>Pré-requisitos
Quaisquer registos no repositório do Log Analytics com o tipo de alerta serão importados para a experiência de alertas unificada, pelo que tem de efetuar a configuração necessária para recolher estes registos.
1. Para **Nagios** e **Zabbix** alertas, [configurar esses servidores](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents) para enviar alertas para o Log Analytics.
1. Para **System Center Operations Manager** alertas, [ligar o seu grupo de gestão do Operations Manager à sua área de trabalho do Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Todos os alertas criados no System Center Operations Manager são importados para o Log Analytics.

## <a name="view-your-alert-instances"></a>Ver as suas instâncias de alerta
Assim que tiver configurado a importação para o Log Analytics, pode começar a ver alerta instâncias de um estes serviços de monitorização a [experiência de alertas unificada](https://aka.ms/azure-alerts-overview). Assim que estiverem presentes na experiência de alertas unificada, pode [gerir instâncias de alerta](https://aka.ms/managing-alert-instances), [gerir smart grupos criados nestes alertas](https://aka.ms/managing-smart-groups) e [alterar o estado dos seus alertas e inteligente grupos](https://aka.ms/managing-alert-smart-group-states).

> [!NOTE]
>  Alertas do Nagios na experiência unificada de alertas não têm monitorização de estado – por exemplo, o [monitorizar a condição](https://aka.ms/azure-alerts-overview) de um alerta não vai de "Fired" como "Resolvido". Em vez disso, o "Fired" e "Resolvido" são apresentadas como instâncias separadas de alerta. 
