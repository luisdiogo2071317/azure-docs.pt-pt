---
title: "Publique as configurações de implementação para o Azure pilha Development Kit (ASDK) | Microsoft Docs"
description: "Descreve as alterações de configuração recomendada para efetuar depois de instalar o Kit de desenvolvimento de pilha do Azure (ASDK)."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 2183576e87aa2fb31f8be8f676a5aee7d52f68df
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="post-asdk-installation-configuration-tasks"></a>Após a instalação de ASDK tarefas de configuração
Depois de [instalar o ASDK](asdk-install.md), existem algumas alterações de configuração de pós-instalação recomendada ser efetuadas. 

## <a name="install-azure-stack-powershell"></a>Instalar o PowerShell para o Azure Stack 
Pilha do Azure compatíveis módulos do PowerShell do Azure são necessários para funcionar com a pilha do Azure.

Comandos do PowerShell para a pilha do Azure são instalados através da galeria do PowerShell. Para registar o repositório de PSGallery, abra uma sessão elevada do PowerShell e execute o seguinte comando:

``` Powershell
Set-PSRepository `
  -Name "PSGallery" `
  -InstallationPolicy Trusted
```

 Azure pilha compatíveis AzureRM os módulos são instalados através de perfis de versão de API. Pilha do Azure requer que o perfil de versão de API de 2017-03-09-perfil, que está disponível ao instalar o módulo de AzureRM.Bootstrapper. 
 
 Pode instalar o Azure PowerShell de pilha com ou sem ligação à internet para o computador de anfitrião ASDK:

- **Com uma ligação à internet** do computador anfitrião ASDK. Execute o seguinte script do PowerShell para instalar estes módulos na sua instalação do kit de desenvolvimento:

  ``` PowerShell
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
  Se a instalação foi bem-sucedida, os módulos AzureRM e AzureStack são apresentados no resultado.

- **Sem uma ligação à internet** do computador anfitrião ASDK. Num cenário de desligado, primeiro tem de transferir os módulos do PowerShell para uma máquina que tenha acesso à internet com os seguintes comandos do PowerShell:

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
  Em seguida, copie os pacotes transferidos para o computador ASDK e registar a localização como o repositório de predefinido e instalar os módulos AzureRM e AzureStack deste repositório:

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

## <a name="download-the-azure-stack-tools"></a>Transferir as ferramentas de pilha do Azure
[Ferramentas de AzureStack](https://github.com/Azure/AzureStack-Tools) é um repositório do GitHub que aloja os módulos do PowerShell para gerir e implementar recursos com pilha do Azure. Para obter estas ferramentas, clone o repositório do GitHub ou transferir a pasta Ferramentas AzureStack executando o seguinte script:

  ```PowerShell
  # Change directory to the root directory. 
  cd \

  # Enforce usage of TLSv1.2 to download the Azure Stack tools archive from GitHub
  [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
  invoke-webrequest `
    https://github.com/Azure/AzureStack-Tools/archive/master.zip `
    -OutFile master.zip

  # Expand the downloaded files.
  expand-archive master.zip `
    -DestinationPath . `
    -Force

  # Change to the tools directory.
  cd AzureStack-Tools-master
  ```

## <a name="validate-the-asdk-installation"></a>Validar a instalação de ASDK
Para garantir que a implementação de ASDK foi concluída com êxito, pode utilizar o cmdlet de teste AzureStack seguindo estes passos:

1. Inicie sessão como AzureStack\CloudAdmin no computador anfitrião ASDK.
2. Abra o PowerShell como administrador (não ISE do PowerShell).
3. Execute: `Enter-PSSession -ComputerName AzS-ERCS01 -ConfigurationName PrivilegedEndpoint`
4. Execute: `Test-AzureStack`

Os testes demorar alguns minutos a concluir. Se a instalação foi bem sucedida, o resultado aspeto semelhante ao seguinte:

![test-azurestack](media/asdk-post-deploy/test-azurestack.png)

Se Ocorreu uma falha, siga os passos de resolução de problemas para obter ajuda.

## <a name="activate-the-administrator-and-tenant-portals"></a>Ativar os portais de administrador e de inquilino
Depois de implementações que utilizam o Azure AD, tem de ativar ambas as os pilha do Azure administrador portais e de inquilino. Esta ativação permitir para dar o portal de pilha do Azure e o Azure Resource Manager, as permissões corretas (listadas na página de consentimento) para todos os utilizadores do diretório.

- Para o portal de administrador, navegue para https://adminportal.local.azurestack.external/guest/signup, leia as informações e, em seguida, clique em **aceitar**. Depois de a aceitar, pode adicionar os administradores de serviço que também não são administradores de inquilinos do diretório.

- Para o portal de inquilinos, navegue para https://portal.local.azurestack.external/guest/signup, leia as informações e, em seguida, clique em **aceitar**. Depois de a aceitar, os utilizadores no diretório podem iniciar sessão no portal do inquilino. 

> [!NOTE] 
> Se os portais não estão ativados, apenas o administrador da directory pode iniciar sessão e utilizar os portais. Se outro utilizador inicia sessão, verá um erro a indicar que o administrador não concedidas permissões a outros utilizadores. Quando o administrador não pertence nativamente no diretório do que Azure pilha está registada, o diretório de pilha do Azure tem anexado ao URL de ativação. Por exemplo, se a pilha do Azure está registada para fabrikam.onmicrosoft.com e o utilizador de admin é admin@contoso.com, navegue para https://portal.local.azurestack.external/guest/signup/fabrikam.onmicrosoft.com para ativar o portal. 

## <a name="reset-the-password-expiration-policy"></a>Repor a política de expiração de palavra-passe 
Para se certificar de que a palavra-passe para o anfitrião do kit de desenvolvimento não expira antes de terminar o período de avaliação, siga estes passos depois de implementar o ASDK.

### <a name="to-change-the-password-expiration-policy-from-powershell"></a>Para alterar a política de expiração de palavra-passe a partir do Powershell:
A partir de uma consola elevada do Powershell, execute o comando:

```powershell
Set-ADDefaultDomainPasswordPolicy -MaxPasswordAge 180.00:00:00 -Identity azurestack.local
```

### <a name="to-change-the-password-expiration-policy-manually"></a>Para alterar manualmente a política de expiração de palavra-passe:
1. No anfitrião do kit de desenvolvimento, abra **gestão de políticas de grupo** (GPMC. MMC) e navegue para **gestão de políticas de grupo** – **floresta: azurestack.local** – **domínios** – **azurestack.local**.
2. Clique com botão direito **política de domínio predefinida** e clique em **editar**.
3. No Editor de grupo política de gestão, navegue para **configuração do computador** – **políticas** – **definições do Windows** – **definições de segurança**– **Políticas de conta** – **política de palavra-passe**.
4. No painel da direita, faça duplo clique **antiguidade de palavra-passe máximo**.
5. No **antiguidade de palavra-passe máximo propriedades** caixa de diálogo, altere o **palavra-passe expira em** valor **180**e, em seguida, clique em **OK**.

![Consola de gestão de políticas de grupo](media/asdk-post-deploy/gpmc.png)


## <a name="next-steps"></a>Passos Seguintes
[Registar o ASDK com o Azure](asdk-register.md)
