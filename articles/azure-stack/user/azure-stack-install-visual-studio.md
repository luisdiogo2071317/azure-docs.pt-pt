---
title: Instalar o Visual Studio e ligue à pilha do Azure | Microsoft Docs
description: Obter os passos necessários para instalar o Visual Studio e ligue à pilha do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2018
ms.author: mabrigg
ms.reviewer: unknown
ms.openlocfilehash: cbd5e5dbcdd2565e8066b0721f45863569bfd90a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295035"
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Instalar o Visual Studio e ligue à pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode utilizar o Visual Studio para escrever e implementar o Azure Resource Manager [modelos](azure-stack-arm-templates.md) à pilha do Azure. Os passos neste artigo explica como instalar o Visual Studio a [pilha do Azure](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou um computador externo se pretender pilha do Azure através de [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn).

## <a name="install-visual-studio"></a>Instalar o Visual Studio

1. Transfira e execute o [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).  

2. Abra o **instalador de plataforma Web da Microsoft**.

3. Procurar **Visual Studio Community 2015 com o SDK do Microsoft Azure - 2.9.6**. Clique em **adicionar**, e **instalar**.

4. Desinstalar o **do Microsoft Azure PowerShell** que é instalado como parte do Azure SDK.

    ![Passos de instalação de captura de ecrã de WebPI](./media/azure-stack-install-visual-studio/image1.png) 

5. [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)

6. Reinicie o sistema operativo após a conclusão da instalação.

## <a name="connect-to-azure-stack-with-azure-ad"></a>Ligar a pilha do Azure com o Azure AD

1. Inicie o Visual Studio.

2. Do **vista** menu, selecione **Cloud Explorer**.

3. No painel de novo, selecione **adicionar conta** e inicie sessão com as suas credenciais do Azure Active Directory (Azure AD).  

    ![Captura de ecrã da Cloud Explorer após a sessão iniciada e ligado à pilha do Azure](./media/azure-stack-install-visual-studio/image2.png)

Depois de a sessão, pode [implementar modelos](azure-stack-deploy-template-visual-studio.md) ou procurar tipos de recursos disponíveis e grupos de recursos para criar os seus próprios modelos.  

## <a name="connect-to-azure-stack-with-ad-fs"></a>Ligar a pilha do Azure com o AD FS

1. Inicie o Visual Studio.

2. De **ferramentas**, selecione **opções**.

3. Expanda **ambiente** no **painel de navegação** e selecione **contas**.

4. Selecione **adicionar**e introduza o ponto final de utilizador do Azure Resource Manager.  
  Para o kit de desenvolvimento de pilha do Azure, o URL é: `https://management.local.azurestack/external`.  
  Para a pilha do Azure integrado sistemas o URL é: `https://management.[Region}.[External FQDN]`.

    ![X](./media/azure-stack-install-visual-studio/image5.png)

5. Selecione **Adicionar**.  

    Visual Studio chama Manager de recursos do Azure e Deteta os pontos finais, incluindo o ponto final de autenticação para a Azure Directory Federated Services (AD FS).

    ![Captura de ecrã da Cloud Explorer após a sessão iniciada e ligado à pilha do Azure](./media/azure-stack-install-visual-studio/image6.png)

6. Selecione **Cloud Explorer** do **vista** menu.
7. Selecione **adicionar conta** e inicie sessão com as suas credenciais do AD FS.  

    ![X](./media/azure-stack-install-visual-studio/image7.png)

    Cloud Explorer consulta as subscrições disponíveis. Pode selecionar um uma subscrição disponível para gerir.

    ![X](./media/azure-stack-install-visual-studio/image8.png)

8. Navegação nos seus recursos existentes, os grupos de recursos ou implementar modelos.

## <a name="next-steps"></a>Passos seguintes

 - Leia mais sobre [coexistência](https://msdn.microsoft.com/library/ms246609.aspx) com outras versões de Visual Studio.
 - [Desenvolver modelos para a pilha do Azure](azure-stack-develop-templates.md)