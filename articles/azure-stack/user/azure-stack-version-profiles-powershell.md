---
title: Através de perfis de versão de API com o PowerShell na pilha do Azure | Microsoft Docs
description: Saiba como utilizar perfis de versão de API com o PowerShell na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: EBAEA4D2-098B-4B5A-A197-2CEA631A1882
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: b09799fe102522e1ad91f4983cf4f5fa8122b2c1
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="use-api-version-profiles-for-powershell-in-azure-stack"></a>Utilize perfis de versão de API para o PowerShell na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Os perfis de versão de API fornecem uma forma de gerir as diferenças de versão entre o Azure e pilha do Azure. Um perfil de versão de API é um conjunto de módulos do PowerShell de AzureRM com versões de API específicas. Cada plataforma de nuvem tem um conjunto de perfis de versão de API suportados. Por exemplo, pilha do Azure suporta uma versão de perfil com a data específica, tal como **2017-03-09-perfil**, e suporta o Azure o **mais recente** perfil da versão de API. Quando instalar um perfil, são instalados os módulos do PowerShell de AzureRM que correspondem para o perfil especificado.

## <a name="install-the-powershell-module-required-to-use-api-version-profiles"></a>Instalar o módulo do PowerShell necessário para utilizar perfis de versão de API

O **AzureRM.Bootstrapper** módulo que está disponível através da galeria do PowerShell fornece cmdlets do PowerShell que são necessárias para trabalhar com perfis de versão de API. Utilize o cmdlet seguinte para instalar o módulo de AzureRM.Bootstrapper:

```PowerShell
Install-Module -Name AzureRm.BootStrapper
```

## <a name="install-a-profile"></a>Instalar um perfil

Utilize o **instalação AzureRmProfile** cmdlet com o **2017-03-09-perfil** perfil de versão de API para instalar os módulos de AzureRM necessários pela pilha de Azure. Os módulos de operador de pilha do Azure não estão instalados com este perfil da versão de API. Deve ser instalados separadamente conforme especificado no passo 3 do [instale o PowerShell para Azure pilha](azure-stack-powershell-install.md) artigo.

```PowerShell 
Install-AzureRMProfile -Profile 2017-03-09-profile
```
## <a name="install-and-import-modules-in-a-profile"></a>Instalar e importar módulos num perfil

Utilize o **utilize AzureRmProfile** cmdlet para instalar e importar módulos que estão associados um perfil de versão de API. Pode importar apenas um perfil de versão de API numa sessão do PowerShell. Para importar um perfil de versão de API diferente, tem de abrir uma nova sessão do PowerShell. O cmdlet de utilização-AzureRMProfile executa as seguintes tarefas:  
1. Verifica se os módulos do PowerShell associados ao perfil de versão de API especificado estão instalados no âmbito atual.  
2. Transfere e instala os módulos se não estiver já instalados.   
3. Importa os módulos para a sessão atual do PowerShell. 

```PowerShell
# Installs and imports the specified API version profile into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser

# Installs and imports the specified API version profile into the current PowerShell session without any prompts
Use-AzureRmProfile -Profile 2017-03-09-profile -Scope CurrentUser -Force
```

Para instalar e importar módulos de AzureRM selecionados a partir de um perfil de versão de API, execute o cmdlet de utilização-AzureRMProfile com o **módulo** parâmetro:

```PowerShell
# Installs and imports the compute, Storage and Network modules from the specified API version profile into your current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile -Module AzureRM.Compute, AzureRM.Storage, AzureRM.Network
```

## <a name="get-the-installed-profiles"></a>Obter os perfis instalados

Utilize o **Get-AzureRmProfile** cmdlet para obter a lista de perfis disponíveis de versão de API: 

```PowerShell
# lists all API version profiles provided by the AzureRM.BootStrapper module.
Get-AzureRmProfile -ListAvailable 

# lists the API version profiles which are installed on your machine
Get-AzureRmProfile
```
## <a name="update-profiles"></a>Perfis de atualização

Utilize o **atualização AzureRmProfile** cmdlet para atualizar os módulos num perfil de versão de API para a versão mais recente dos módulos que estão disponíveis no PSGallery. É recomendado executar sempre o **atualização AzureRmProfile** cmdlet numa nova sessão do PowerShell para evitar conflitos ao importar os módulos. O cmdlet Update-AzureRmProfile executa as seguintes tarefas:

1. Verifica se as versões mais recentes de módulos estão instaladas no perfil de versão de API especificado para o âmbito atual.  
2. Pede-lhe instalar, se não estiver já instalado.  
3. Instala e importa os módulos atualizados para a sessão atual do PowerShell.  

```PowerShell
Update-AzureRmProfile -Profile 2017-03-09-profile
```

Para remover as versões dos módulos anteriormente instaladas antes de atualizar para a versão mais recente, utilize o cmdlet Update-AzureRmProfile juntamente com o **- RemovePreviousVersions** parâmetro:

```PowerShell 
Update-AzureRmProfile -Profile 2017-03-09-profile -RemovePreviousVersions
```

Este cmdlet é executado as seguintes tarefas:  

1. Verifica se as versões mais recentes de módulos estão instaladas no perfil de versão de API especificado para o âmbito atual.  
2. Remove as versões mais antigas de módulos do perfil de versão de API atual e na sessão atual do PowerShell.  
4. pede-lhe que instale a versão mais recente.  
5. Instala e importa os módulos atualizados para a sessão atual do PowerShell.  
 
## <a name="uninstall-profiles"></a>Desinstalar perfis

Utilize o **desinstalação AzureRmProfile** cmdlet para desinstalar o perfil de versão de API especificada.

```PowerShell 
Uninstall-AzureRmProfile -Profile 2017-03-09-profile
```

## <a name="next-steps"></a>Passos Seguintes
* [Instalar o PowerShell para o Azure Stack](azure-stack-powershell-install.md)
* [Configurar o Azure pilha ambiente do utilizador do PowerShell](azure-stack-powershell-configure-user.md)  
