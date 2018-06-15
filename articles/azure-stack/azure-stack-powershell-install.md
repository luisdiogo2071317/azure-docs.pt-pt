---
title: Instale o PowerShell para a pilha do Azure | Microsoft Docs
description: Saiba como instalar o PowerShell para a pilha do Azure.
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
ms.date: 5/18/2018
ms.author: mabrigg
ms.reviewer: thoroet
ms.openlocfilehash: b3c09582f5135655640768bcbcbef91750827bfa
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358895"
---
# <a name="install-powershell-for-azure-stack"></a>Instale o PowerShell para a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pilha do Azure compatíveis módulos do PowerShell do Azure são necessários para funcionar com a pilha do Azure. Neste guia, iremos orientá-lo pelos passos necessários para instalar o PowerShell para a pilha do Azure.

Este artigo descreve em pormenor as instruções para instalar o PowerShell para a pilha do Azure.

> [!Note]
> Os seguintes passos necessitam do PowerShell 5.0. Para verificar a sua versão, execute $PSVersionTable.PSVersion e comparar o **principais** versão.

Comandos do PowerShell para a pilha do Azure são instalados através da galeria do PowerShell. Pode utilizar o procedimento seguinte para validar se PSGallery está registado como um repositório, abra uma sessão elevada do PowerShell e execute o seguinte comando:

```PowerShell  
Get-PSRepository -Name "PSGallery"
```

Se o repositório não estiver registado, abra uma sessão elevada do PowerShell e execute o seguinte comando:

```PowerShell  
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
```
> [!Note]  
> Este passo necessita de acesso à Internet. 

## <a name="uninstall-existing-versions-of-powershell"></a>Desinstalar versões existentes do PowerShell

Antes de instalar a versão necessária, certifique-se de que desinstalar quaisquer instalados anteriormente módulos do PowerShell de pilha do Azure. Pode desinstalá-las utilizando um dos seguintes dois métodos:

 - Para desinstalar os módulos do PowerShell existentes, feche todas as sessões ativas do PowerShell e execute o seguinte comando:

  ```PowerShell
    Uninstall-Module AzureRM.AzureStackAdmin -Force
    Uninstall-Module AzureRM.AzureStackStorage -Force
    Uninstall-Module -Name AzureStack -Force
  ```

 - Eliminar todas as pastas que começam por "Azure" do `C:\Program Files\WindowsPowerShell\Modules` e `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` pastas. Eliminar estas pastas remove quaisquer existentes módulos do PowerShell.

As secções seguintes descrevem os passos necessários para instalar o PowerShell para a pilha do Azure. PowerShell pode ser instalado na pilha do Azure que manipula no ligado, ligado parcialmente ou um cenário de desligado.

## <a name="install-powershell-in-a-connected-scenario-with-internet-connectivity"></a>Instale o PowerShell num cenário ligado (com acesso à Internet)

Azure pilha compatíveis AzureRM os módulos são instalados através de perfis de versão de API. Pilha do Azure requer o **2017-03-09-perfil** perfil de versão de API, o que está disponível ao instalar o módulo de AzureRM.Bootstrapper. Para mais informações sobre perfis de versão de API e os cmdlets fornecidos pelo-las, consulte o [gerir perfis de versão de API](user/azure-stack-version-profiles.md). Para além de módulos AzureRM, deve também instalar os módulos do PowerShell de pilha específicos do Azure. Execute o seguinte script do PowerShell para instalar estes módulos na sua estação de trabalho de desenvolvimento:

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

Se a instalação foi bem-sucedida, os módulos AzureRM e AzureStack são apresentados no resultado.

## <a name="install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Instale o PowerShell num desligado ou um cenário parcialmente ligado (com acesso à Internet limitado)

Num cenário de desligado, tem de transferir primeiro os módulos do PowerShell para uma máquina que tenha acesso à Internet e, em seguida, transfere-os para o Kit de desenvolvimento de pilha do Azure para a instalação.

> [!IMPORTANT]  
> A versão do módulo do PowerShell do Azure de pilha 1.3.0 vem com uma lista de alterações de última hora. Para atualizar a partir de 1.2.11 versão, consulte o [guia de migração](https://aka.ms/azspowershellmigration).

1. Inicie sessão computador onde tem conectividade internet e utilize o seguinte script de transferir o AzureRM e pacotes de AzureStack no seu computador local:

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
  > Se não estiver a executar pilha do Azure com a atualização 1804 ou superior, altere o **requiredversion** valor do parâmetro para `1.2.11`. 

2. Copie os pacotes transferidos através de um dispositivo USB.

3. Inicie sessão na estação de trabalho e copie os pacotes do dispositivo USB para uma localização na estação de trabalho.

4. Agora tem de registar nesta localização como o repositório de predefinido e instalar os módulos AzureRM e AzureStack deste repositório:

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

 - [Descarregar as ferramentas de pilha do Azure a partir do GitHub](azure-stack-powershell-download.md)
 - [Configurar o Azure pilha ambiente do utilizador do PowerShell](user/azure-stack-powershell-configure-user.md)  
 - [Configurar o ambiente de PowerShell o operador de pilha do Azure](azure-stack-powershell-configure-admin.md) 
 - [Gerir perfis de versão de API na pilha do Azure](user/azure-stack-version-profiles.md)  
