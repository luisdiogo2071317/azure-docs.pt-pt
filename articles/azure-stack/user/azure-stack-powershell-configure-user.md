---
title: Configurar o Azure pilha ambiente do utilizador do PowerShell | Microsoft Docs
description: Configurar o Azure pilha ambiente do utilizador do PowerShell
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: F4ED2238-AAF2-4930-AA7F-7C140311E10F
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 5/15/2018
ms.author: mabrigg
ms.reviewer: Balsu.G
ms.openlocfilehash: 2655b682d35dd1879c649ed58d524ecd80808896
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34258027"
---
# <a name="configure-the-azure-stack-users-powershell-environment"></a>Configurar o Azure pilha ambiente do utilizador do PowerShell

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Utilize as instruções neste artigo para configurar o ambiente de PowerShell para um utilizador de pilha do Azure.
Depois de configurar o ambiente, pode utilizar o PowerShell para gerir os recursos de pilha do Azure. Por exemplo, pode utilizar o PowerShell para subscrever ofertas, criar máquinas virtuais e implementar modelos Azure Resource Manager.

>[!NOTE]
>Este artigo tem um âmbito para ambientes de utilizador de pilha do Azure. Se pretender configurar o PowerShell para o ambiente de operador da nuvem, consulte o [configurar o ambiente de PowerShell o operador de pilha do Azure](../azure-stack-powershell-configure-admin.md) artigo.

## <a name="prerequisites"></a>Pré-requisitos

Pode configurar os pré-requisitos do [kit de desenvolvimento](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), ou de um baseados em Windows externo cliente se [ligado através de VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Instalar [módulos do Azure pilha compatível com o Azure PowerShell](azure-stack-powershell-install.md).
* Transferir o [ferramentas necessárias para trabalhar com a pilha de Azure](azure-stack-powershell-download.md).

## <a name="configure-the-user-environment-and-sign-in-to-azure-stack"></a>Configurar o ambiente de utilizador e inicie sessão na pilha do Azure

Com base no tipo de implementação pilha do Azure (Azure AD ou AD FS), execute um dos seguintes scripts para configurar o PowerShell para a pilha do Azure.

Certifique-se de que substitui as seguintes variáveis de script valores da sua configuração de pilha do Azure:

* AAD tenantName
* Ponto final de GraphAudience
* ArmEndpoint

### <a name="azure-active-directory-aad-based-deployments"></a>Implementações baseadas em do Azure Active Directory (AAD)

  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack development kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAudience endpoint for your environment>"

  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  # Set the GraphEndpointResourceId value
  Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
    -EnvironmentName "AzureStackUser"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $TenantID
   ```

### <a name="active-directory-federation-services-ad-fs-based-deployments"></a>Implementações baseadas em serviços de Federação do Active Directory (AD FS)

  ```powershell
  # Navigate to the downloaded folder and import the **Connect** PowerShell module
  Set-ExecutionPolicy RemoteSigned
  Import-Module .\Connect\AzureStack.Connect.psm1

  # For Azure Stack development kit, this value is set to https://management.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack development kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAudience endpoint for your environment>"

  # Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment `
    -Name "AzureStackUser" `
    -ArmEndpoint $ArmEndpoint

  # Set the GraphEndpointResourceId value
  Set-AzureRmEnvironment `
    -Name "AzureStackUser" `
    -GraphAudience $GraphAudience `
    -EnableAdfsAuthentication:$true

  # Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId `
    -ADFS `
    -EnvironmentName "AzureStackUser"

  # Sign in to your environment
  Login-AzureRmAccount `
    -EnvironmentName "AzureStackUser" `
    -TenantId $TenantID
  ```

## <a name="register-resource-providers"></a>Registar fornecedores de recursos

Fornecedores de recursos não estão registados automaticamente para novas subscrições de utilizador não tem quaisquer recursos implementados através do portal. Pode registar explicitamente um fornecedor de recursos, executando o seguinte script:

```powershell
foreach($s in (Get-AzureRmSubscription)) {
        Select-AzureRmSubscription -SubscriptionId $s.SubscriptionId | Out-Null
        Write-Progress $($s.SubscriptionId + " : " + $s.SubscriptionName)
Get-AzureRmResourceProvider -ListAvailable | Register-AzureRmResourceProvider -Force
    }
```

## <a name="test-the-connectivity"></a>Testar a conectividade

Quando já tem tudo configurar, teste a conectividade com o PowerShell para criar recursos na pilha do Azure. Como um teste, crie um grupo de recursos para uma aplicação e adicionar uma máquina virtual. Execute o seguinte comando para criar um grupo de recursos com o nome "MyResourceGroup":

```powershell
New-AzureRmResourceGroup -Name "MyResourceGroup" -Location "Local"
```

## <a name="next-steps"></a>Passos Seguintes

* [Desenvolver modelos para a pilha do Azure](azure-stack-develop-templates.md)
* [Implementar modelos com o PowerShell](azure-stack-deploy-template-powershell.md)
