---
title: Como integrar registos do Azure Active Directory com Splunk com o Azure Monitor (pré-visualização) | Documentos da Microsoft
description: Saiba como integrar registos do Azure Active Directory com Splunk com o Azure Monitor (pré-visualização)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f2f00b94b50ecf1fc3d1b08a04063dbffa62501e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736143"
---
# <a name="integrate-azure-ad-logs-with-splunk-by-using-azure-monitor-preview"></a>Integrar registos do Azure AD com Splunk com o Azure Monitor (pré-visualização)

Neste artigo, saiba como integrar registos do Azure Active Directory (Azure AD) com Splunk com o Azure Monitor. Primeiro encaminha os registos para um hub de eventos do Azure e, em seguida, integrar o hub de eventos com Splunk.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar esta funcionalidade, precisa de:
* Registos de um hub de eventos do Azure que contém a atividade do Azure AD. Saiba como [transmitir os registos de atividade para um hub de eventos](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* O suplemento de Monitor do Azure para o Splunk. [Transferir e configurar a sua instância do Splunk](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md).

## <a name="tutorial"></a>Tutorial 

1. Abra a sua instância do Splunk e selecione **resumo de dados**.

    ![O botão "Resumo de dados"](./media/tutorial-integrate-activity-logs-with-splunk/DataSummary.png)

2. Selecione o **Sourcetypes** separador e, em seguida, selecione **amal: aadal:audit**

    ![A guia Sourcetypes de resumo de dados](./media/tutorial-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    A atividade do Azure AD, os registos são mostrados na figura a seguir:

    ![Registos de atividade](./media/tutorial-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> Se não pode instalar um suplemento na sua instância de Splunk (por exemplo, se estiver a utilizar um proxy ou em execução na Splunk Cloud), pode reencaminhar estes eventos para o Recoletor de eventos do Splunk HTTP. Para tal, utilize este [função do Azure](https://github.com/Microsoft/AzureFunctionforSplunkVS), que é acionado por mensagens novas no hub de eventos. 
>

## <a name="next-steps"></a>Passos Seguintes

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Interpretar o esquema dos registos de auditoria no Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Interpretar o esquema dos registos de início de sessão no Azure Monitor)
* [Perguntas mais frequentes e problemas conhecidos](concept-activity-logs-in-azure-monitor.md#frequently-asked-questions)
