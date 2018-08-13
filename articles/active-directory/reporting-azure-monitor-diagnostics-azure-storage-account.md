---
title: Tutorial – Arquivar registos do Azure Active Directory numa conta de armazenamento do Azure (pré-visualização) | Microsoft Docs
description: Saiba como configurar o Diagnóstico do Azure para enviar registos do Azure Active Directory para uma conta de armazenamento (pré-visualização)
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
ms.openlocfilehash: bf4114ebd436fd35a3364c1c16a1720c79cd32ca
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502165"
---
# <a name="tutorial-archive-azure-ad-logs-to-an-azure-storage-account-preview"></a>Tutorial: Arquivar registos do Azure AD numa conta de armazenamento do Azure (pré-visualização)

Neste tutorial, vai aprender a configurar as definições de diagnóstico do Azure Monitor para encaminhar registos do Azure Active Directory (Azure AD) para uma conta de armazenamento do Azure.

## <a name="prerequisites"></a>Pré-requisitos 

Para utilizar esta funcionalidade, precisa de:

* Uma subscrição do Azure com uma conta de armazenamento do Azure. Se não tiver uma subscrição do Azure, pode [inscrever-se para obter uma avaliação gratuita](https://azure.microsoft.com/free/).
* Um inquilino do Azure AD.
* Um utilizador que seja *administrador global* ou *administrador de segurança* do inquilino do Azure AD.

## <a name="archive-logs-to-an-azure-storage-account"></a>Arquivar os registos para uma conta de armazenamento do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 

2. Selecione **Azure Active Directory** > **Activity** > **Registos de auditoria**. 

3. Selecione **Exportar Definições**. 

4. No painel **Definições de diagnósticos**, escolha uma das opções abaixo:
    * Para alterar as definições existentes, selecione **Editar definição**.
    * Para adicionar definições novas, selecione **Adicionar definição de diagnósticos**.  
      Pode ter até três definições. 

    ![Exportar definições](./media/reporting-azure-monitor-diagnostics-azure-storage-account/ExportSettings.png)

5. Introduza um nome amigável para a definição e que lhe lembre do propósito da mesma (por exemplo, *Enviar para conta de armazenamento do Azure*). 

6. Assinale a caixa de verificação **Arquivar numa conta de armazenamento** e selecione **Conta de armazenamento**. 

7. Selecione a subscrição do Azure e a conta de armazenamento para onde pretende encaminhar os registos.
 
8. Selecione **OK** para sair da configuração.

9. Escolha uma ou ambas as opções abaixo:
    * Para enviar os registos de auditoria para a conta de armazenamento, selecione a caixa de verificação **AuditLogs**. 
    * Para enviar os registos de início de sessão para a conta de armazenamento, selecione a caixa de verificação **SignInLogs**.

10. Utilize o controlo de deslize para definir a retenção dos seus dados de registo. Por predefinição, este valor é *0*, o que significa que os registos são mantidos na conta de armazenamento indefinidamente. Se definir outro valor, os eventos mais antigos do que o número de dias selecionado são limpos automaticamente.

11. Selecione **Guardar** para guardar a definição.

    ![Definições de diagnósticos](./media/reporting-azure-monitor-diagnostics-azure-storage-account/DiagnosticSettings.png)

12. Depois de cerca de 15 minutos, confirme que os registos são enviados para a sua conta de armazenamento. Aceda ao [portal do Azure](https://portal.azure.com), selecione **Contas de armazenamento**, selecione a conta de armazenamento que utilizou antes e, por fim, selecione **Blobs**. 

13. Em **Registos de auditoria**, selecione **insights-log-audit**. Em **Registos de início de sessão**, selecione **insights-log-signin**.
    ![Conta de armazenamento](./media/reporting-azure-monitor-diagnostics-azure-storage-account/StorageAccount.png)

## <a name="next-steps"></a>Passos seguintes

* [Interpret audit logs schema in Azure Monitor](reporting-azure-monitor-diagnostics-audit-log-schema.md) (Interpretar o esquema dos registos de auditoria no Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reporting-azure-monitor-diagnostics-sign-in-log-schema.md) (Interpretar o esquema dos registos de início de sessão no Azure Monitor)
* [Perguntas mais frequentes e problemas conhecidos](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)
