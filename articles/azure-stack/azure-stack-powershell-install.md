---
title: Instalar o PowerShell para o Azure Stack | Documentos da Microsoft
description: Saiba como instalar o PowerShell para o Azure Stack.
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
ms.date: 07/10/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: e2785b0beeab042d4b1ad9a9eb5f545dbb58b8b9
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38487506"
---
# <a name="install-powershell-for-azure-stack"></a>Instalar o PowerShell para o Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Módulos do Azure Stack compatíveis do PowerShell do Azure são necessários para trabalhar com o Azure Stack. Neste guia, iremos guiá-lo pelos passos necessários para instalar o PowerShell para o Azure Stack.

Este artigo tem detalhadas instruções para instalar o PowerShell para o Azure Stack.

> [!Note]  
> Os seguintes passos requerem o PowerShell 5.0. Para verificar a sua versão, execute $PSVersionTable.PSVersion e comparar os **principais** versão.

Comandos do PowerShell para o Azure Stack são instalados através da galeria do PowerShell. Pode utilizar o procedimento seguinte para confirmar se o PSGallery está registado como um repositório, abra uma sessão elevada do PowerShell e execute o seguinte comando:

```PowerShell  
Get-PSRepository -Name "PSGallery"
```

Se não estiver registado no repositório, abra uma sessão elevada do PowerShell e execute o seguinte comando:

```PowerShell  
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```
> [!Note]  
> Este passo requer acesso à Internet. 

## <a name="uninstall-existing-versions-of-the-azure-stack-powershell-modules"></a>Desinstalar as versões existentes dos módulos do Azure Stack do PowerShell

Antes de instalar a versão necessária, certifique-se de que desinstale qualquer instalados anteriormente módulos do Azure Stack do AzureRM PowerShell. Pode desinstalá-las através de um dos seguintes dois métodos:

 - Para desinstalar os módulos AzureRM PowerShell existentes, feche todas as sessões ativas do PowerShell e execute o seguinte comando:

  ```PowerShell
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ```

 - Eliminar todas as pastas que começam com "Azure" a partir da `C:\Program Files\WindowsPowerShell\Modules` e `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` pastas. A eliminar essas pastas remove quaisquer módulos existentes do PowerShell.

As secções seguintes descrevem os passos necessários para instalar o PowerShell para o Azure Stack. PowerShell pode ser instalado no Azure Stack, que é operada no ligado, ligado parcialmente ou num cenário de desligado.

## <a name="install-the-azure-stack-powershell-modules-in-a-connected-scenario-with-internet-connectivity"></a>Instalar os módulos do Azure Stack do PowerShell num cenário conectado (com ligação à Internet)

Módulos do Azure Stack compatíveis AzureRM são instalados por meio de perfis de versão de API. O Azure Stack requer os **2017-03-09-perfil** perfil da versão de API, que está disponível ao instalar o módulo de AzureRM.Bootstrapper. Para saber mais sobre os perfis de versão de API e os cmdlets fornecidos pelo-las, consulte a [gerir perfis de versão de API](user/azure-stack-version-profiles.md). Além dos módulos AzureRM, também deve instalar os módulos do PowerShell do Azure Stack específicas. Execute o seguinte script do PowerShell para instalar esses módulos em sua estação de trabalho de desenvolvimento:

  ```PowerShell  
# Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet 
Install-Module -Name AzureRm.BootStrapper 

# Install and import the API Version Profile required by Azure Stack into the current PowerShell session. 
Use-AzureRmProfile -Profile 2017-03-09-profile -Force 

# Install Module Version 1.3.0 if Azure Stack is running 1804 at a minimum 
Install-Module -Name AzureStack -RequiredVersion 1.3.0 

# Install Module Version 1.2.11 if Azure Stack is running a lower version than 1804 
Install-Module -Name AzureStack -RequiredVersion 1.2.11 
  ```

Para confirmar a instalação, execute o seguinte comando:

```PowerShell  
Get-Module -ListAvailable | where-Object {$_.Name -like "Azs*"}
```

Se a instalação for bem-sucedida, os módulos AzureRM e AzureStack são apresentados no resultado.

## <a name="install-the-azure-stack-powershell-modules-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Instalar os módulos do Azure Stack do PowerShell num cenário parcialmente conectado ou um desligado (com uma conectividade de Internet limitada)

Num cenário de desligado, tem de transferir primeiro os módulos do PowerShell para um computador que tenha conectividade à Internet e, em seguida, transferi-las para o Development Kit do Azure Stack para a instalação.

> [!IMPORTANT]  
> A versão do módulo do PowerShell do Azure Stack 1.3.0 vem com uma lista de alterações recentes. Para atualizar a partir do 1.2.11 versão, consulte a [guia de migração](https://aka.ms/azspowershellmigration).

1. Inicie sessão no computador em que tem ligação à internet e utilize o seguinte script para transferir o AzureRM e pacotes de AzureStack no seu computador local:

   ```PowerShell  
   $Path = "<Path that is used to save the packages>"

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureRM `
     -Path $Path `
     -Force `
     -RequiredVersion 1.2.11

   Save-Package `
     -ProviderName NuGet `
     -Source https://www.powershellgallery.com/api/v2 `
     -Name AzureStack `
     -Path $Path `
     -Force `
     -RequiredVersion 1.3.0 
   ```

  > [!Important]  
  > Se não estiver a executar Azure Stack com atualização 1804 ou superior, altere a **requiredversion** valor do parâmetro `1.2.11`. 

2. Copie os pacotes transferidos para um dispositivo USB.

3. Inicie sessão para a estação de trabalho e copie os pacotes do dispositivo USB num local na estação de trabalho.

4. Agora tem de registar esta localização como o repositório de padrão e instalar os módulos AzureRM e AzureStack partir este repositório:

   ```PowerShell
   $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
   $RepoName = "MyNuGetSource"

   Register-PSRepository `
     -Name $RepoName `
     -SourceLocation $SourceLocation `
     -InstallationPolicy Trusted

   Install-Module AzureRM `
     -Repository $RepoName

   Install-Module AzureStack `
     -Repository $RepoName 
   ```

## <a name="configure-powershell-to-use-a-proxy-server"></a>Configurar o PowerShell para utilizar um servidor proxy

Em cenários que necessitam de um servidor proxy para aceder à internet, primeiro tem de configurar o PowerShell para utilizar um servidor de proxy existente.

1. Abra uma linha de comandos elevada do PowerShell.
2. Execute os seguintes comandos:

````PowerShell  
  #To use Windows credentials for proxy authentication
  [System.Net.WebRequest]::DefaultWebProxy.Credentials = [System.Net.CredentialCache]::DefaultCredentials

  #Alternatively, to prompt for separate credentials that can be used for #proxy authentication

  [System.Net.WebRequest]::DefaultWebProxy.Credentials = Get-Credential
````

## <a name="next-steps"></a>Passos Seguintes

 - [Transferir ferramentas do Azure Stack a partir do GitHub](azure-stack-powershell-download.md)
 - [Configurar o ambiente do PowerShell do utilizador do Azure Stack](user/azure-stack-powershell-configure-user.md)  
 - [Configurar o ambiente do PowerShell da operadora do Azure Stack](azure-stack-powershell-configure-admin.md) 
 - [Gerir perfis de versão de API no Azure Stack](user/azure-stack-version-profiles.md)  
