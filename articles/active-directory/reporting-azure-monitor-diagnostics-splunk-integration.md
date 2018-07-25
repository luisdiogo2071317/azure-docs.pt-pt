---
title: Como integrar registos do Azure Active Directory com Splunk através do Azure Monitor (pré-visualização) | Documentos da Microsoft
description: Saiba como integrar registos do Azure Active Directory com Splunk através do Azure Monitor (pré-visualização)
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
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d1dd62d06c7e3ed634795604ce9660694ea073ca
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239991"
---
# <a name="integrate-azure-active-directory-logs-with-splunk-using-azure-monitor-preview"></a>Integrar registos do Azure Active Directory com o Splunk através do Azure Monitor (pré-visualização)

Neste artigo, aprenderá como integrar registos do Azure Active Directory com Splunk através do Azure Monitor. Em primeiro lugar, precisaria encaminhar os registos para um hub de eventos do Azure, em seguida, integrá-lo ao Splunk.

## <a name="prerequisites"></a>Pré-requisitos

1. Registos do Hub de eventos do Azure que contém a atividade do Azure AD. Saiba como [transmitir os registos de atividade para o Hub de eventos](reporting-azure-monitor-diagnostics-azure-event-hub.md). 
2. Utilize o seguinte procedimento [instruções](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md) para transferir o suplemento do monitor do Azure para Splunk e configurar a sua instância do Splunk.

## <a name="tutorial"></a>Tutorial 

1. Abra a sua instância do Splunk e clique em **resumo de dados**.
    ![Resumo dos dados](./media/reporting-azure-monitor-diagnostics-splunk-integration/DataSummary.png "resumo de dados")

2. Navegue para o **Sourcetypes** separador e selecione **amal: aadal:audit** ![Sourcetypes separador](./media/reporting-azure-monitor-diagnostics-splunk-integration/sourcetypeaadal.png "Sourcetypes separador")

3. Verá que o Azure registos de atividades do AD, conforme mostrado na figura a seguir.
    ![Registos de atividades](./media/reporting-azure-monitor-diagnostics-splunk-integration/activitylogs.png "registos de atividades")

> [!NOTE]
> Se não pode instalar um suplemento na sua instância de Splunk (por exemplo, se estiver a utilizar um proxy ou em execução na Splunk Cloud), pode reencaminhar estes eventos para o Recoletor de eventos de HTTP do Splunk através desta [função do Azure que é acionada por mensagens novas no hub de eventos](https://github.com/Microsoft/AzureFunctionforSplunkVS). " 
>

## <a name="next-steps"></a>Passos Seguintes

* [Interpretar o esquema de registos de auditoria no Azure monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Interpretar o esquema de registos de início de sessão no Azure monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Perguntas mais frequentes perguntas e problemas conhecidos](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)