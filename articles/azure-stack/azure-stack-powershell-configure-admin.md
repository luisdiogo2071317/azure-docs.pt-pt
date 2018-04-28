---
title: Configurar o ambiente de PowerShell o operador de pilha do Azure | Microsoft Docs
description: Saiba como configurar o ambiente de PowerShell o operador de pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 37D9CAC9-538B-4504-B51B-7336158D8A6B
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: mabrigg
ms.openlocfilehash: a8ab52de6c57e84bb2c90ce6bcf53ef1b92e30af
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="configure-the-azure-stack-operators-powershell-environment"></a>Configurar o ambiente de PowerShell o operador de pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pode configurar a pilha do Azure para utilizar o PowerShell para gerir recursos, tais como criar ofertas, esquemas, quotas e alertas. Este tópico ajuda-o a configurar o ambiente de operador. Se pretender configurar o PowerShell para o ambiente do utilizador, consulte o artigo para [configurar o Azure pilha ambiente do utilizador PowerShell](user/azure-stack-powershell-configure-user.md) artigo.

## <a name="prerequisites"></a>Pré-requisitos

Execute os seguintes pré-requisitos a partir de [kit de desenvolvimento](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou de um baseados em Windows externo cliente se estiver [ligado através de VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn): 

* Instalar [módulos do Azure pilha compatível com o Azure PowerShell](azure-stack-powershell-install.md).  
* Transferir o [ferramentas necessárias para trabalhar com a pilha de Azure](azure-stack-powershell-download.md).  

## <a name="configure-the-operator-environment-and-sign-in-to-azure-stack"></a>Configurar o ambiente do operador e inicie sessão na pilha do Azure

Configure o ambiente de operador de pilha do Azure com o PowerShell. Com base no tipo de implementação, Azure AD ou AD FS, execute um dos seguintes scripts: substitua a tenantName do Azure AD, GraphAudience endpoint e valores de ArmEndpoint com a sua própria configuração de ambiente.

### <a name="azure-active-directory-azure-ad-based-deployments"></a>Implementações de Active Directory (Azure AD) com base do Azure

````powershell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# Get the value of your Directory Tenant ID
$TenantID = Get-AzsDirectoryTenantId -AADTenantName "<mydirectorytenant>.onmicrosoft.com" -EnvironmentName AzureStackAdmin

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````


### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Implementações baseadas em serviços de Federação do Active Directory (AD FS)

````powershell  
#  Create an administrator environment
Add-AzureRMEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external"

# Get the value of your Directory Tenant ID
$TenantID = Get-AzsDirectoryTenantId -ADFS -EnvironmentName AzureStackAdmin

# After registering the AzureRM environment, cmdlets can be 
# easily targeted at your Azure Stack instance.
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
````

## <a name="test-the-connectivity"></a>Testar a conectividade

Agora que já tem tudo configuração, vamos utilizar o PowerShell para criar recursos na pilha do Azure. Por exemplo, pode criar um grupo de recursos para uma aplicação e adicionar uma máquina virtual. Utilize o seguinte comando para criar um grupo de recursos com o nome "MyResourceGroup":

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Passos Seguintes
* [Desenvolver modelos para a pilha do Azure](user/azure-stack-develop-templates.md)
* [Implementar modelos com o PowerShell](user/azure-stack-deploy-template-powershell.md)
