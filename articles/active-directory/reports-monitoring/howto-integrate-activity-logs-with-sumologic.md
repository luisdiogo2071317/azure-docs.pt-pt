---
title: Como integrar registos do Azure Active Directory com SumoLogic com o Azure Monitor (pré-visualização) | Documentos da Microsoft
description: Saiba como integrar registos do Azure Active Directory com SumoLogic com o Azure Monitor (pré-visualização)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 2a90e70a9e7951b298be408992ee5e5fc6332131
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729334"
---
# <a name="integrate-azure-ad-logs-with-sumologic-by-using-azure-monitor-preview"></a>Integrar registos do Azure AD com SumoLogic com o Azure Monitor (pré-visualização)

Neste artigo, saiba como integrar registos do Azure Active Directory (Azure AD) com SumoLogic com o Azure Monitor. Primeiro encaminha os registos para um hub de eventos do Azure e, em seguida, integrar o hub de eventos com SumoLogic.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar esta funcionalidade, precisa de:
* Registos de um hub de eventos do Azure que contém a atividade do Azure AD. Saiba como [transmitir os registos de atividade para um hub de eventos](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Um SumoLogic início de sessão único ativado na subscrição.

## <a name="steps-to-integrate-azure-ad-logs-with-sumologic"></a>Passos para integrar registos do Azure AD com SumoLogic 

1. Primeiro, [transmitir os registos do Azure AD para um hub de eventos do Azure](quickstart-azure-monitor-stream-logs-to-event-hub.md).
2. Configurar a sua instância de SumoLogic para [recolher registos do Azure Active Directory](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory).
3. [Instalar a aplicação do Azure AD SumoLogic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards) para utilizar os dashboards pré-configuradas que fornecem análise em tempo real do seu ambiente.

 ![Dashboard](./media/howto-integrate-activity-logs-with-sumologic/overview-dashboard.png)

## <a name="next-steps"></a>Passos Seguintes

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Interpretar o esquema dos registos de auditoria no Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Interpretar o esquema dos registos de início de sessão no Azure Monitor)
* [Perguntas mais frequentes e problemas conhecidos](concept-activity-logs-in-azure-monitor.md#frequently-asked-questions)
