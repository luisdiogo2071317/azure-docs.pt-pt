---
title: Tutorial – Transmitir registos do Azure Active Directory num hub de eventos do Azure (pré-visualização) | Microsoft Docs
description: Saiba como configurar o Diagnóstico do Azure para enviar registos do Azure Active Directory para um hub de eventos (pré-visualização)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 50e7b7c35386bafe521078308a8809c4d3038dd8
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501701"
---
# <a name="tutorial-stream-azure-ad-logs-to-an-azure-event-hub-preview"></a>Tutorial: Transmitir registos do Azure AD para um hub de eventos do Azure (pré-visualização)

Neste tutorial, vai aprender a configurar as definições de diagnóstico do Azure Monitor para transmitir registos do Azure Active Directory (Azure AD) para um hub de eventos do Azure. Utilize este mecanismo para integrar os seus registos com ferramentas de Gestão de Informações e Eventos de Segurança (SIEM) de terceiros, como Splunk e QRadar.

## <a name="prerequisites"></a>Pré-requisitos 

Para utilizar esta funcionalidade, precisa de:

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, pode [inscrever-se para obter uma avaliação gratuita](https://azure.microsoft.com/free/).
* Um inquilino do Azure AD.
* Um utilizador que seja *administrador global* ou *administrador de segurança* do inquilino do Azure AD.
* Um espaço de nomes dos Hubs de Eventos e um hub de eventos na sua subscrição do Azure. Saiba como [criar um hub de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-create.md).

## <a name="archive-logs-to-an-event-hub"></a>Arquivar registos num hub de eventos

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 

2. Selecione **Azure Active Directory** > **Activity** > **Registos de auditoria**. 

3. Selecione **Exportar Definições**.  
    
4. No painel **Definições de diagnósticos**, escolha uma das opções abaixo:
    * Para alterar as definições existentes, selecione **Editar definição**.
    * Para adicionar definições novas, selecione **Adicionar definição de diagnósticos**.  
      Pode ter até três definições.

      ![Exportar definições](./media/reporting-azure-monitor-diagnostics-azure-event-hub/ExportSettings.png)

5. Assinale a caixa de verificação **Transmitir para um hub de eventos** e selecione **Hub de Eventos/Configurar**.

6. Selecione a subscrição do Azure e o espaço de nomes dos Hubs de Eventos para onde pretende encaminhar os registos.  
    A subscrição e o espaço de nomes dos Hubs de Eventos devem ambos ser associados ao inquilino do Azure AD de onde os registos são transmitidos. Também pode especificar um hub de eventos no espaço de nomes dos Hubs de Eventos para onde os registos devem ser enviados. Se não for especificado nenhum hub de eventos, é criado um no espaço de nomes com o nome predefinido **insights-logs-audit**.

7. Selecione **OK** para sair da configuração do hub de eventos.

8. Escolha uma ou ambas as opções abaixo:
    * Para enviar os registos de auditoria para a conta de armazenamento, selecione a caixa de verificação **AuditLogs**. 
    * Para enviar os registos de início de sessão para a conta de armazenamento, selecione a caixa de verificação **SignInLogs**.

9. Selecione **Guardar** para guardar a definição.

    ![Definições de diagnósticos](./media/reporting-azure-monitor-diagnostics-azure-event-hub/DiagnosticSettings.png)

10. Depois de cerca de 15 minutos, verifique se os eventos são apresentados no seu hub de eventos. Para tal, aceda ao hub de eventos a partir do portal e confirme que a contagem das **mensagens de entrada** é superior a zero. 

    ![Registos de auditoria](./media/reporting-azure-monitor-diagnostics-azure-event-hub/InsightsLogsAudit.png)

## <a name="access-data-from-your-event-hub"></a>Aceder a dados a partir do seu hub de eventos

Quando os dados forem apresentados no hub de eventos, pode aceder e lê-los de duas formas:

* **Configurar uma ferramenta SIEM suportada**. Para ler os dados do hub de eventos, a maioria das ferramentas precisa da cadeia de ligação do hub de eventos e de determinadas permissões para a sua subscrição do Azure. As ferramentas de terceiros com integração do Azure Monitor incluem, entre outras:
    * **Splunk**: para obter mais informações sobre como integrar registos do Azure AD no Splunk, veja [Integrate Azure AD logs with Splunk by using Azure Monitor](reporting-azure-monitor-diagnostics-splunk-integration.md) (Utilizar o Azure Monitor para integrar registos do Azure AD no Splunk).
    
    * **IBM QRadar**: o DSM e o Protocolo dos Hubs de Eventos do Azure estão disponíveis para transferência no [suporte da IBM](http://www.ibm.com/support). Para obter mais informações sobre a integração no Azure, aceda ao site [IBM QRadar Security Intelligence Platform 7.3.0](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
    
    * **Sumo Logic**: para configurar o Sumo Logic para consumir dados de hubs de eventos, veja [Collect logs for Azure activity logs from an event hub](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub) (Recolher registos de registos de atividades do Azure a partir de um hub de eventos). 

* **Configurar ferramentas personalizadas**. Se a sua SIEM atual ainda não for suportada nos diagnóstico do Azure Monitor, pode configurar as ferramentas personalizadas com a API dos Hubs de Eventos. Para saber mais, veja [Começar a receber mensagens a partir de um hub de eventos](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph).


## <a name="next-steps"></a>Passos seguintes

* [Integrate Azure AD logs with Splunk by using Azure Monitor](reporting-azure-monitor-diagnostics-splunk-integration.md) (Utilizar o Azure Monitor para integrar registos do Azure AD no Splunk)
* [Interpret audit logs schema in Azure Monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md) (Interpretar o esquema dos registos de auditoria no Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) (Interpretar o esquema dos registos de início de sessão no Azure Monitor)
