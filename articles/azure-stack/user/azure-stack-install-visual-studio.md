---
title: Instalar o Visual Studio e ligue à pilha do Azure | Microsoft Docs
description: Obter os passos necessários para instalar o Visual Studio e ligue à pilha do Azure
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2018
ms.author: mabrigg
ms.openlocfilehash: 3eaefbe011c4d98fe9a76d4f277a76a2f167b191
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Instalar o Visual Studio e ligue à pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Utilizar o Visual Studio para criar e implementar o Azure Resource Manager [modelos](azure-stack-arm-templates.md) na pilha do Azure. Pode utilizar os passos descritos neste artigo para instalar o Visual Studio do [Kit de desenvolvimento de pilha do Azure](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou de um baseados em Windows externo cliente se estiver ligado através de [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). Estes passos neste artigo são para uma nova instalação de edição de Comunidade de 2015 do Visual Studio. Leia mais sobre [coexistência](https://msdn.microsoft.com/library/ms246609.aspx) com outras versões de Visual Studio.

## <a name="install-visual-studio"></a>Instalar o Visual Studio

1. Transfira e execute o [instalador de plataforma Web](https://www.microsoft.com/web/downloads/platform.aspx).
2. Procurar **Visual Studio Community 2015 com o SDK do Microsoft Azure - 2.9.6**, selecione **adicionar**e, em seguida, selecione **instalar**.

    ![Captura de ecrã dos passos de instalação WebPI](./media/azure-stack-install-visual-studio/image1.png)

3. Desinstalar o **do Microsoft Azure PowerShell** que é instalado como parte do Azure SDK.

    ![Captura de ecrã da interface de adicionar/remover programas para o Azure PowerShell](./media/azure-stack-install-visual-studio/image2.png)

4. [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)

5. Reinicie o sistema operativo após a conclusão da instalação.

## <a name="connect-to-azure-stack"></a>Ligar ao Azure Stack

1. Inicie o Visual Studio.

2. Do **vista** menu, selecione **Cloud Explorer**.

3. No painel de novo, selecione **adicionar conta** e inicie sessão com as suas credenciais do Azure Active Directory.
    ![Captura de ecrã da Cloud Explorer após o início de sessão e ligado à pilha do Azure](./media/azure-stack-install-visual-studio/image6.png)

Depois de a sessão, pode [implementar modelos](azure-stack-deploy-template-visual-studio.md) ou procurar tipos de recursos disponíveis e grupos de recursos para criar os seus próprios modelos.

## <a name="next-steps"></a>Passos Seguintes

* [Desenvolver modelos para a pilha do Azure](azure-stack-develop-templates.md)
