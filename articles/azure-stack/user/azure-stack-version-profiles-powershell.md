---
title: Através de perfis de versão de API com o PowerShell no Azure Stack | Documentos da Microsoft
description: Saiba como utilizar perfis de versão de API com o PowerShell no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: EBAEA4D2-098B-4B5A-A197-2CEA631A1882
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 8da02641cc54f9308f8e0bbb8d2b28da9a930aa2
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42054384"
---
# <a name="use-api-version-profiles-for-powershell-in-azure-stack"></a>Utilizar perfis de versão de API para o PowerShell no Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Os perfis de versão de API fornecem uma forma de gerenciar as diferenças de versão entre o Azure e o Azure Stack. Um perfil da versão de API é um conjunto de módulos AzureRM PowerShell em versões de API específicas. Cada plataforma de cloud tem um conjunto de perfis de versão de API suportados. Por exemplo, o Azure Stack suporta uma versão de perfil com data específico, tal como **2017-03-09-perfil**, e o Azure suporta o **mais recente** perfil da versão de API. Ao instalar um perfil, são instalados os módulos AzureRM PowerShell que correspondem para o perfil especificado.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Instalar o módulo do PowerShell necessário para utilizar perfis de versão de API

O **AzureRM.Bootstrapper** módulo que está disponível através da galeria do PowerShell fornece cmdlets do PowerShell que são necessárias para trabalhar com perfis de versão de API. Utilize o cmdlet seguinte para instalar o módulo de AzureRM.Bootstrapper:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```

## <a name="install-a-profile"></a>Instalar um perfil

Utilize o **Install-AzureRmProfile** cmdlet com o **2017-03-09-perfil** perfil da versão de API para instalar os módulos AzureRM necessários para o Azure Stack. Os módulos de operador do Azure Stack não estão instalados com este perfil da versão de API. Deve ser instalados separadamente conforme especificado no passo 3 do [instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md) artigo.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Instalar e importar módulos num perfil

Utilize o **utilização-AzureRmProfile** cmdlet para instalar e importar módulos que estão associados um perfil da versão de API. Pode importar apenas um perfil da versão de API numa sessão do PowerShell. Para importar um perfil de versão de API diferente, tem de abrir uma nova sessão do PowerShell. O cmdlet de utilização-AzureRMProfile executa as seguintes tarefas:  
1. Verifica se os módulos do PowerShell associados ao perfil de versão de API especificado estão instalados no âmbito atual.  
2. Transfere e instala os módulos se ainda não estiver instalados.   
3. Importa os módulos para a atual sessão do PowerShell. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Para instalar e importar os módulos de AzureRM selecionados um perfil da versão de API, execute o cmdlet de utilização-AzureRMProfile com o **módulo** parâmetro:

```PowerShell
# Installs and imports the compute, Storage and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Obter os perfis instalados

Utilize o **Get-AzureRmProfile** cmdlet para obter a lista de perfis de versão de API disponíveis: 

```PowerShell
# lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# lists the API version profiles which are installed on your machine
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Perfis de atualização

Utilize o **Update-AzureRmProfile** cmdlet para atualizar os módulos num perfil da versão de API para a versão mais recente dos módulos que estão disponíveis no PSGallery. É recomendado que para sempre executar o **Update-AzureRmProfile** cmdlet numa nova sessão do PowerShell para evitar conflitos ao importar módulos. O cmdlet Update-AzureRmProfile executa as seguintes tarefas:

1. Verifica se as versões mais recentes dos módulos são instaladas no perfil de versão de API fornecido para o âmbito atual.  
2. Pede-lhe para instalar, se não estiverem já instalados.  
3. Instala e importa os módulos atualizados para a atual sessão do PowerShell.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Para remover as versões instaladas anteriormente dos módulos antes de atualizar para a versão mais recente disponível, utilize o cmdlet Update-AzureRmProfile juntamente com o **- RemovePreviousVersions** parâmetro:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Este cmdlet é executado as seguintes tarefas:  

1. Verifica se as versões mais recentes dos módulos são instaladas no perfil de versão de API fornecido para o âmbito atual.  
2. Remove as versões mais antigas dos módulos do perfil de versão de API atual e na sessão atual do PowerShell.  
4. pede-lhe para instalar a versão mais recente.  
5. Instala e importa os módulos atualizados para a atual sessão do PowerShell.  
 
## <a name="uninstall-profiles"></a>Desinstalar perfis

Utilize o **Uninstall-AzureRmProfile** cmdlet para desinstalar o perfil da versão de API especificado.

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Passos Seguintes
* [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)
* [Configurar o ambiente do PowerShell do utilizador do Azure Stack](azure-stack-powershell-configure-user.md)  
