---
title: Configurar o ambiente do Azure PowerShell de pilha | Microsoft Docs
description: Saiba como configurar o ambiente do Azure PowerShell da pilha.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: article
ms.date: 06/22/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: 74a5a9408a78dd0da12fb3f8ed721774030cc438
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749866"
---
# <a name="configure-the-azure-stack-powershell-environment"></a>Configurar o ambiente do PowerShell de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode configurar a pilha do Azure para utilizar o PowerShell para gerir recursos, tais como criar ofertas, esquemas, quotas e alertas. Este tópico ajuda-o a configurar o ambiente de operador.

## <a name="prerequisites"></a>Pré-requisitos

Execute os seguintes pré-requisitos a partir de [kit de desenvolvimento](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou de um baseados em Windows externo cliente se estiver [ligado através de VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn). 

 - Instalar [módulos do Azure pilha compatível com o Azure PowerShell](azure-stack-powershell-install.md).  
 - Transferir o [ferramentas necessárias para trabalhar com a pilha de Azure](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Configurar o ambiente do operador e inicie sessão na pilha do Azure

Configure o ambiente de operador de pilha do Azure com o PowerShell. Execute um dos seguintes scripts: substitua a tenantName do Azure AD, GraphAudience endpoint e valores de ArmEndpoint com a sua própria configuração de ambiente.

````PowerShell  
    # For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external.
    # To get this value for Azure Stack integrated systems, contact your service provider.
    $ArmEndpoint = "<Admin Resource Manager endpoint for your environment>"

    # Register an AzureRM environment that targets your Azure Stack instance
    Add-AzureRMEnvironment `
        -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

    # After signing in to your environment, Azure Stack cmdlets
    # can be easily targeted at your Azure Stack instance.
    Add-AzureRmAccount -EnvironmentName "AzureStackAdmin"
````

## <a name="test-the-connectivity"></a>Testar a conectividade

Agora que já tem tudo configurar, utilizar o PowerShell para criar recursos na pilha do Azure. Por exemplo, pode criar um grupo de recursos para uma aplicação e adicionar uma máquina virtual. Utilize o seguinte comando para criar um grupo de recursos denominado **MyResourceGroup**.

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Passos Seguintes

 - [Desenvolver modelos para a pilha do Azure](user/azure-stack-develop-templates.md)
 - [Implementar modelos com o PowerShell](user/azure-stack-deploy-template-powershell.md)
