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
ms.openlocfilehash: ea26da91e2a0b88e8afa54a4210b7a2365e4949b
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240292"
---
# <a name="tutorial-archive-azure-active-directory-logs-to-an-azure-storage-account-preview"></a>Tutorial: Arquivar registos do Azure Active Directory numa conta de armazenamento do Azure (pré-visualização)

Neste tutorial, irá aprender como configurar definições de diagnóstico do Azure Monitor para encaminhar registos do Azure Active Directory para uma conta de armazenamento do Azure.

## <a name="prerequisites"></a>Pré-requisitos 

É necessário:

* Uma subscrição do Azure com uma conta de armazenamento do Azure. Se não tiver uma subscrição do Azure, pode [inscrever-se para obter uma avaliação gratuita](https://azure.microsoft.com/free/).
* Um inquilino do Azure Active Directory.
* Um utilizador, que é um administrador global ou administrador de segurança para esse inquilino.

## <a name="archive-logs-to-an-azure-storage-account"></a>Arquivar os registos para uma conta de armazenamento do Azure

1. Inicie sessão no [portal do Azure](https://portal.azure.com). 
2. Clique em **Azure Active Directory** -> **Atividade** -> **Registos de auditoria**. 
3. Clique em **Exportar Definições** para abrir o painel Definições de Diagnóstico. Clique em **Editar definição** se pretender alterar as definições existentes ou clique em **Adicionar definição de diagnóstico** para adicionar uma nova. Pode ter um máximo de três definições. 
    ![Exportar definições](./media/reporting-azure-monitor-diagnostics-azure-storage-account/ExportSettings.png "Exportar definições")

4. Adicione um nome amigável para a definição para lembrá-lo da sua finalidade. Por exemplo, "Enviar para a conta de armazenamento do Azure". 
5. Assinale a caixa de verificação **Arquivar numa conta de armazenamento** e clique em **Conta de armazenamento** para escolher a conta de armazenamento do Azure. 
6. Selecione a conta de armazenamento e a subscrição do Azure em que pretende encaminhar os registos, e clique em **OK** para fechar a configuração.
7. Assinale a caixa de verificação **AuditLogs** para enviar registos de auditoria para a conta de armazenamento. 
8. Assinale a caixa de verificação **SignInLogs** para enviar registos de início de sessão para a conta de armazenamento.
9. Utilize o controlo de deslize para definir a retenção nos seus dados de registo. Por predefinição, este valor é "0" e os registos serão mantidos na conta de armazenamento indefinidamente. Caso contrário, pode definir um valor e eventos mais antigos do que o número de dias selecionado e será limpo automaticamente.
10. Clique em **Guardar** para guardar as definições.
    ![Definições de diagnóstico](./media/reporting-azure-monitor-diagnostics-azure-storage-account/DiagnosticSettings.png "Definições de diagnóstico")

11. Depois de cerca de 15 minutos, certifique-se de que os registos são enviados para a sua conta de armazenamento. Aceda ao portal do Azure, clique em **Contas de armazenamento**, escolha a conta de armazenamento que utilizou anteriormente e clique em **Blobs**. 
12. Para os **Registos de auditoria**, clique em **insights-log-audit**. Para os **Registos de início de sessão**, clique em **insights-log-signin**.
    ![Conta de armazenamento](./media/reporting-azure-monitor-diagnostics-azure-storage-account/StorageAccount.png "Conta de armazenamento")

## <a name="next-steps"></a>Passos seguintes

* [Interpretar o esquema de registo de auditoria no monitor do Azure](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Interpretar o esquema de registo de início de sessão no monitor do Azure](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Perguntas mais frequentes e problemas conhecidos](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)