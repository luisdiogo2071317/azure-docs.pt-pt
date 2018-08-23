---
title: Instalar o Visual Studio e ligue-se ao Azure Stack | Documentos da Microsoft
description: Saiba os passos necessários para instalar o Visual Studio e ligue-se ao Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: 2afbea68c017805e9bd7db43b03face0705608b7
ms.sourcegitcommit: fab878ff9aaf4efb3eaff6b7656184b0bafba13b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/22/2018
ms.locfileid: "42358752"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Instalar o Visual Studio e ligue-se ao Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Pode utilizar o Visual Studio para escrever e implementar o Azure Resource Manager [modelos](azure-stack-arm-templates.md) ao Azure Stack. Os passos neste artigo descrevem como instalar o Visual Studio no [do Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou num computador externo se pretender ao Azure Stack através do [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>Instalar o Visual Studio

1. Transferir e executar o [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Abra o **instalador de plataforma Microsoft Web**.

3. Procure **Visual Studio Community 2015 com o SDK do Microsoft Azure - 2.9.6**. Clique em **adicione**, e **instalar**.

4. Desinstalar o **do Microsoft Azure PowerShell** que é instalado como parte do SDK do Azure.

    ![Passos de instalação de captura de ecrã de WebPI](./media/azure-stack-install-visual-studio/image1.png) 

5. [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)

6. Depois de concluída a instalação, reinicie o sistema operativo.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Ligar ao Azure Stack com o Azure AD

1. Inicie o Visual Studio.

2. Partir do **View** menu, selecione **Cloud Explorer**.

3. No novo painel, selecione **adicionar conta** e inicie sessão com as suas credenciais do Azure Active Directory (Azure AD).  

    ![Captura de ecrã de Cloud Explorer, uma vez conectado e ligada ao Azure Stack](./media/azure-stack-install-visual-studio/image2.png)

Depois de iniciar sessão, pode [implementar modelos](azure-stack-deploy-template-visual-studio.md) ou procurar tipos de recursos disponíveis e grupos de recursos para criar seus próprios modelos.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>Ligar ao Azure Stack com o AD FS

1. Inicie o Visual Studio.

2. Partir **ferramentas**, selecione **opções**.

3. Expanda **ambiente** no **painel de navegação** e selecione **contas**.

4. Selecione **adicionar**e introduza o ponto de extremidade do utilizador do Azure Resource Manager.  
  Para o Azure Stack Development kit, o URL é: `https://management.local.azurestack/external`.  
  Para o Azure Stack sistemas integrados o URL é: `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. Selecione **Adicionar**.  

    Visual Studio chama o gerente de recursos do Azure e Deteta os pontos de extremidade incluindo o ponto final de autenticação do Azure Directory Federated Services (AD FS).

    ![Captura de ecrã de Cloud Explorer, uma vez conectado e ligada ao Azure Stack](./media/azure-stack-install-visual-studio/image6.png)

6. Selecione **Cloud Explorer** partir do **vista** menu.
7. Selecione **adicionar conta** e inicie sessão com as suas credenciais de AD FS.  

    ![X](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer consulta as subscrições disponíveis. Pode selecionar uma uma subscrição disponível para gerir.

    ![X](./media/azure-stack-install-visual-studio/image8.png)

8. Navegação nos seus recursos existentes, os grupos de recursos ou implementar modelos.

## <a name="next-steps"></a>Passos Seguintes

 - Leia mais sobre [coexistência](https://msdn.microsoft.com/library/ms246609.aspx) com outras versões do Visual Studio.
 - [Desenvolver modelos para o Azure Stack](azure-stack-develop-templates.md)