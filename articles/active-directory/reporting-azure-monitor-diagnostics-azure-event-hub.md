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
ms.openlocfilehash: c4f5bf1b49d7f59b2bb938a7ddc53b96c1d354ef
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240222"
---
# <a name="tutorial-stream-azure-active-directory-logs-to-an-azure-event-hub-preview"></a>Tutorial: Transmitir registos do Azure Active Directory num hub de eventos do Azure (pré-visualização)

Neste tutorial, irá aprender como configurar definições de diagnóstico do Azure Monitor para transmitir registos do Azure Active Directory para um hub de eventos do Azure. Utilize este mecanismo para integrar os seus registos com ferramentas SIEM de terceiros, como Splunk e QRadar.

## <a name="prerequisites"></a>Pré-requisitos 

É necessário:

* Uma subscrição do Azure. Se não tiver uma subscrição do Azure, pode [inscrever-se para obter uma avaliação gratuita](https://azure.microsoft.com/free/).
* Um inquilino do Azure Active Directory
* Um utilizador, que é um administrador global ou administrador de segurança para esse inquilino
* Um espaço de nomes dos Hubs de Eventos e hub de eventos na sua subscrição do Azure. Saiba como [criar um aqui](https://docs.microsoft.com/azure/event-hubs/event-hubs-create.md).

## <a name="archive-logs-to-event-hub"></a>Arquivar registos no Hub de eventos

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 
2. Clique em **Azure Active Directory** -> **Atividade** -> **Registos de auditoria**. 
3. Clique em **Exportar Definições** para abrir o painel Definições de Diagnóstico. Clique em **Editar definição** se pretender alterar as definições existentes ou clique em **Adicionar definição de diagnóstico** para adicionar uma nova. Pode ter até três definições. 
    ![Exportar definições](./media/reporting-azure-monitor-diagnostics-azure-event-hub/ExportSettings.png "Exportar definições")

4. Assinale a caixa de verificação **Transmitir para um hub de eventos** e clique em **Hub de Eventos/Configurar**.
5. Selecione uma subscrição do Azure e o espaço de nomes dos Hubs de Eventos para onde pretende encaminhar os registos. A subscrição e o espaço de nomes dos Hubs de Eventos devem ambos ser associados ao inquilino do Active Directory de onde os registos são transmitidos. Também pode especificar um hub de eventos no espaço de nomes dos Hubs de Eventos para onde os registos devem ser enviados. Se não for especificado nenhum hub de eventos, será criado um hub de eventos no espaço de nomes com o nome predefinido **insights-logs-audit**.
6. Clique em **OK** para sair da configuração do hub de eventos.
7. Assinale a caixa de verificação **AuditLogs** para enviar registos de auditoria para a conta de armazenamento. 
8. Assinale a caixa de verificação **SignInLogs** para enviar registos de início de sessão para a conta de armazenamento.
9. Clique em **Guardar** para guardar as definições.
    ![Definições de diagnóstico](./media/reporting-azure-monitor-diagnostics-azure-event-hub/DiagnosticSettings.png "Definições de diagnóstico")

10. Depois de cerca de 15 minutos, certifique-se de que os eventos são apresentados no seu hub de eventos. Para tal, navegue para o hub de eventos a partir do portal e certifique-se de que a contagem das **mensagens de entrada** é superior a zero. 
    ![Registos de auditoria](./media/reporting-azure-monitor-diagnostics-azure-event-hub/InsightsLogsAudit.png "Registos de auditoria")


## <a name="access-data-from-event-hubs"></a>Aceder a dados a partir dos Hubs de Eventos

Assim que os dados aparecem no hub de eventos, pode aceder aos mesmos de duas formas.

* **Configurar uma ferramenta SIEM suportada para ler os dados**: a maioria das ferramentas exige a cadeia de ligação do hub de eventos e determinadas permissões para a sua subscrição do Azure para ler dados do hub de eventos. Aqui está uma lista parcial das ferramentas com a integração do Azure Monitor:
    1. **Splunk**: para obter mais informações sobre como integrar registos do Azure AD no Splunk, veja [Como integrar registos do Azure Active Directory no Splunk com o Azure Monitor](reporting-azure-monitor-diagnostics-splunk-integration.md).
    
    2. **IBM QRadar**: O Microsoft Azure DSM e o Protocolo de Hub de Eventos do Microsoft Azure estão disponíveis para transferência a partir do [site de suporte da IBM](http://www.ibm.com/support). Pode [Saber mais sobre a integração com o Azure aqui](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
    
    3. **SumoLogic**: siga [estas instruções](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub) para configurar o SumoLogic para consumir dados de um hub de eventos. 

* **Configurar as ferramentas personalizadas para lerem os dados**: se a sua SIEM atual ainda não é suportada no diagnóstico do monitor do Azure, pode configurar as ferramentas personalizadas com as APIs do Hub de eventos. Para obter mais informações, veja as [APIs do Hub de eventos](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph).


## <a name="next-steps"></a>Passos seguintes

* [Integrar registos do Azure Active Directory no Splunk com o Diagnóstico do Azure Monitor](reporting-azure-monitor-diagnostics-splunk-integration.md)
* [Interpretar o esquema de registos de auditoria nos diagnósticos do monitor do Azure](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Interpretar o esquema de registos de início de sessão nos diagnósticos do monitor do Azure](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)