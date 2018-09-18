---
title: Instalar e configurar o PowerShell para o início rápido do Azure Stack | Documentos da Microsoft
description: Saiba mais sobre como instalar e configurar o PowerShell para o Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2018
ms.author: mabrigg
ms.openlocfilehash: 75b7f9c78418883344ce3c066135fe0847f649ac
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981975"
---
# <a name="get-up-and-running-with-powershell-in-azure-stack"></a>Começar a trabalhar com o PowerShell no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Este início rápido ajuda-o para instalar e configurar um ambiente do Azure Stack com o PowerShell. O script que fornecemos neste artigo tem como escopo o **operador do Azure Stack** apenas.

Este artigo é uma versão condensada dos passos descritos no [instalar o PowerShell]( azure-stack-powershell-install.md), [transferir ferramentas]( azure-stack-powershell-download.md), e [configurar o ambiente de PowerShell do operador Azure Stack]( azure-stack-powershell-configure-admin.md) artigos. Usando os scripts neste artigo, pode configurar o PowerShell para ambientes do Azure Stack que são implementadas com o Azure Active Directory ou os serviços de Federação do Active Directory (AD FS).  

## <a name="set-up-powershell-for-azure-active-directory-based-deployments"></a>Configurar o PowerShell para Implantações com base no Azure Active Directory  

Inicie sessão para o seu Kit de desenvolvimento do Azure Stack ou um cliente externo com base no Windows, se estiver ligado através de VPN. Abra uma sessão elevada do ISE do PowerShell e, em seguida, execute os seguintes scripts.

Certifique-se atualizar o **TenantName**, **ArmEndpoint**, e **GraphAudience** variáveis para a configuração do seu ambiente, conforme necessário:

```PowerShell  
# Specify Azure Active Directory tenant name.
$TenantName = "<mydirectory>.onmicrosoft.com"

# Set the module repository and the execution policy.
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions, and then run the following commands:
Get-Module -ListAvailable -Name Azure* | Uninstall-Module
Get-Module Azs.* -ListAvailable | Uninstall-Module -force

# Install PowerShell for Azure Stack.
Install-Module -Name AzureRm.BootStrapper -Force
```

Carregar o módulo de perfil e o administrador de API para a sua versão do Azure Stack.

  - O Azure Stack 1808 ou posterior.

  ```PowerShell  
    Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
    Install-Module -Name AzureStack -RequiredVersion 1.5.0 -Force
  ```

  - O Azure Stack 1807 ou anterior.

  ```PowerShell  
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.4.0 -Force
  ```

  - O Azure Stack 1804 ou anterior.

  ```PowerShell  
    Use-AzureRmProfile -Profile 2017-03-09-profile -Force
    Install-Module -Name AzureStack -RequiredVersion 1.2.11 -Force
  ```

Baixe as ferramentas do Azure Stack e ligue-se.

```PowerShell  
# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip -OutFile master.zip

expand-archive master.zip -DestinationPath . -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
  Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack
  $TenantID = Get-AzsDirectoryTenantId -AADTenantName $TenantName -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
  Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID 
```

## <a name="set-up-powershell-for-ad-fs-based-deployments"></a>Configurar o PowerShell para o AD FS-Implantações com base no

Pode utilizar o script a seguir se estiver a utilizar o Azure Stack quando estiver ligado à internet. No entanto se estiver a utilizar o Azure Stack sem conectividade à internet, utilize o [desligado a forma de instalar o PowerShell](azure-stack-powershell-install.md) e os cmdlets para configurar o PowerShell irá permanecer a mesmos, conforme mostrado neste script. Inicie sessão para o seu Kit de desenvolvimento do Azure Stack ou um cliente externo com base no Windows, se estiver ligado através de VPN. Abra uma sessão elevada do ISE do PowerShell e, em seguida, execute o seguinte script. Certifique-se atualizar o **ArmEndpoint** e **GraphAudience** variáveis para a configuração do seu ambiente, conforme necessário:

```PowerShell  

# Set the module repository and the execution policy.
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted

Set-ExecutionPolicy RemoteSigned -force

# Uninstall any existing Azure PowerShell modules. To uninstall, close all the active PowerShell sessions and run the following command:
Get-Module -ListAvailable -Name Azure* | Uninstall-Module

# Install PowerShell for Azure Stack.
Install-Module -Name AzureRm.BootStrapper -Force
```

Carregar o módulo de perfil e o administrador de API para a sua versão do Azure Stack.

  - O Azure Stack 1808 ou posterior.

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 2.3.0
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.5.0
    ````

  - O Azure Stack 1807 ou anterior.

    > [!Note]  
    Para atualizar a partir do 1.2.11 versão, consulte a [guia de migração](https://aka.ms/azspowershellmigration).

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.4.0
    ````

  - O Azure Stack 1804 ou anterior.

    ````PowerShell  
    Import-Module -Name PowerShellGet -ErrorAction Stop
    Import-Module -Name PackageManagement -ErrorAction Stop

      $Path = "<Path that is used to save the packages>"
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureRM -Path $Path -Force -RequiredVersion 1.2.11
      Save-Package -ProviderName NuGet -Source https://www.powershellgallery.com/api/v2 -Name AzureStack -Path $Path -Force -RequiredVersion 1.3.0
    ````

Baixe as ferramentas do Azure Stack e ligue-se.

```PowerShell  
# Download Azure Stack tools from GitHub and import the connect module.
cd \
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
invoke-webrequest `
https://github.com/Azure/AzureStack-Tools/archive/master.zip -OutFile master.zip

expand-archive master.zip -DestinationPath . -Force

cd AzureStack-Tools-master

Import-Module .\Connect\AzureStack.Connect.psm1

# For Azure Stack development kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
$ArmEndpoint = "<Resource Manager endpoint for your environment>"

# Register an AzureRM environment that targets your Azure Stack instance
Add-AzureRMEnvironment -Name "AzureStackAdmin" -ArmEndpoint $ArmEndpoint

# Get the Active Directory tenantId that is used to deploy Azure Stack     
$TenantID = Get-AzsDirectoryTenantId -ADFS -EnvironmentName "AzureStackAdmin"

# Sign in to your environment
Add-AzureRmAccount -EnvironmentName "AzureStackAdmin" -TenantId $TenantID
```

## <a name="test-the-connectivity"></a>Testar a conectividade

Agora que configurou o PowerShell, pode testar a configuração através da criação de um grupo de recursos:

```PowerShell  
New-AzureRMResourceGroup -Name "ContosoVMRG" -Location Local
```

> [!note]  
> Para especificar um grupo de recursos, terá de ter um grupo de recursos na sua subscrição. Para obter mais informações sobre as subscrições, veja [descrição geral do plano, oferta, quota e subscrição](azure-stack-plan-offer-quota-overview.md)

Depois de criar o grupo de recursos, o **estado de aprovisionamento** estiver definida como **Succeeded**.

## <a name="next-steps"></a>Passos Seguintes

 - [Instalar e configurar a CLI](azure-stack-connect-cli.md)
 - [Desenvolver modelos](user/azure-stack-develop-templates.md)
