---
title: Instale o PowerShell para a pilha do Azure | Microsoft Docs
description: Saiba como instalar o PowerShell para a pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: F8D99A91-15B5-4073-BE07-A43514A6D2CF
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2018
ms.author: mabrigg
ms.openlocfilehash: 86eae6813d6181b1c42e8316d159c8bbe523330b
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="install-powershell-for-azure-stack"></a>Instale o PowerShell para a pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Pilha do Azure compatíveis módulos do PowerShell do Azure são necessários para funcionar com a pilha do Azure. Este artigo orienta-o pelos passos necessários para instalar o PowerShell para a pilha do Azure. Pode utilizar os passos descritos neste artigo do Kit de desenvolvimento de pilha do Azure ou de um cliente externo baseado no Windows, se estiver ligado através de VPN.

Este artigo fornece instruções detalhadas para a instalação do PowerShell. No entanto, se pretender instalar e configurar o PowerShell rapidamente, pode utilizar o script fornecido no artigo "Começar a trabalhar com o PowerShell".

> [!NOTE]
> Os seguintes passos necessitam do PowerShell 5.0. Para verificar a sua versão, execute $PSVersionTable.PSVersion e comparar a versão "Principal".

Comandos do PowerShell para a pilha do Azure são instalados através da galeria do PowerShell. Para registar o repositório de PSGallery, abra uma sessão do PowerShell elevada do kit de desenvolvimento ou de um cliente externo baseado em Windows se estiverem ligados através de VPN e execute o seguinte comando:

```PowerShell  
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
```

## <a name="uninstall-existing-versions-of-powershell"></a>Desinstalar versões existentes do PowerShell

Antes de instalar a versão necessária, certifique-se de que desinstalar quaisquer módulos do PowerShell do Azure existentes. Pode desinstalá-las utilizando um dos seguintes dois métodos:

* Para desinstalar os módulos do PowerShell existentes, inicie sessão para o kit de desenvolvimento ou para o cliente externo baseados em Windows se estiver a planear estabelecer uma ligação VPN. Feche todas as sessões ativas do PowerShell e execute o seguinte comando:

   ```PowerShell  
   Get-Module -ListAvailable | where-Object {$_.Name -like “Azure*”} | Uninstall-Module
   ```

* Inicie sessão para o kit de desenvolvimento ou para o cliente externo baseados em Windows se estiver a planear estabelecer uma ligação VPN. Eliminar todas as pastas que começam por "Azure" do `C:\Program Files (x86)\WindowsPowerShell\Modules` e `C:\Users\AzureStackAdmin\Documents\WindowsPowerShell\Modules` pastas. Eliminar estas pastas remove quaisquer módulos do PowerShell existentes dos âmbitos de utilizador "global" e "AzureStackAdmin".

As secções seguintes descrevem os passos necessários para instalar o PowerShell para a pilha do Azure. PowerShell pode ser instalado na pilha do Azure que manipula no ligado, ligado parcialmente ou um cenário de desligado.

## <a name="install-powershell-in-a-connected-scenario-with-internet-connectivity"></a>Instale o PowerShell num cenário ligado (com acesso à Internet)

Azure pilha compatíveis AzureRM os módulos são instalados através de perfis de versão de API. Pilha do Azure requer o **2017-03-09-perfil** perfil de versão de API, o que está disponível ao instalar o módulo de AzureRM.Bootstrapper. Para mais informações sobre perfis de versão de API e os cmdlets fornecidos pelo-las, consulte o [gerir perfis de versão de API](azure-stack-version-profiles-powershell.md). Para além de módulos AzureRM, deve também instalar os módulos do PowerShell de pilha específicos do Azure. Execute o seguinte script do PowerShell para instalar estes módulos na sua estação de trabalho de desenvolvimento:

  ```PowerShell  
  # Install the AzureRM.Bootstrapper module. Select Yes when prompted to install NuGet
  Install-Module `
    -Name AzureRm.BootStrapper

  # Install and import the API Version Profile required by Azure Stack into the current PowerShell session.
  Use-AzureRmProfile `
    -Profile 2017-03-09-profile -Force

  Install-Module `
    -Name AzureStack `
    -RequiredVersion 1.2.11
  ```

Para confirmar a instalação, execute o seguinte comando:

  ```PowerShell  
  Get-Module `
    -ListAvailable | where-Object {$_.Name -like “Azure*”}
  ```

  Se a instalação foi bem-sucedida, os módulos AzureRM e pilha do Azure são apresentados no resultado.

## <a name="install-powershell-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Instale o PowerShell num desligado ou um cenário parcialmente ligado (com acesso à internet limitada)

Num cenário parcialmente ligado ou desligado, tem de transferir primeiro os módulos do PowerShell para uma máquina que tenha acesso à internet e, em seguida, transfere-os para o Kit de desenvolvimento de pilha do Azure para a instalação.

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
      -RequiredVersion 1.2.11
    ```

2. Copie os pacotes transferidos através de um dispositivo USB.

3. Inicie sessão para o kit de desenvolvimento e copie os pacotes do dispositivo USB para uma localização no kit de desenvolvimento.

4. Agora tem de registar nesta localização como o repositório de predefinido e instalar os módulos AzureRM e AzureStack deste repositório:

    ```PowerShell  
    $SourceLocation = "<Location on the development kit that contains the PowerShell packages>"
    $RepoName = "MyNuGetSource"

    Register-PSRepository `
      -Name $RepoName `
      -SourceLocation $SourceLocation `
      -InstallationPolicy Trusted

    ```PowerShell  
    Install-Module AzureRM `
      -Repository $RepoName

    Install-Module AzureStack `
      -Repository $RepoName
    ```

## <a name="next-steps"></a>Passos Seguintes

* [Descarregar as ferramentas de pilha do Azure a partir do GitHub](azure-stack-powershell-download.md)
* [Configurar o Azure pilha ambiente do utilizador do PowerShell](azure-stack-powershell-configure-user.md)
* [Gerir perfis de versão de API na pilha do Azure](azure-stack-version-profiles-powershell.md)
