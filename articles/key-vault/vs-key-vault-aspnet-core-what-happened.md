---
title: As alterações efetuadas a um ASP.NET Core projeto quando connec ao Cofre de chaves do Azure | Microsoft Docs
description: Descreve o que acontece ao seu projeto ASP.NET Core quando se liga cofre toKey utilizando os serviços do Visual Studio ligado.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 8b6c590344db2997c1a987da14cabba76cdca83b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787375"
---
# <a name="what-happened-to-my-aspnet-core-project-visual-studio-key-vault-connected-service"></a>O que aconteceu ao meu projeto em ASP.NET Core (serviço ligado da Cofre de chaves do Visual Studio)?

> [!div class="op_single_selector"]
> - [Introdução](vs-key-vault-aspnet-core-get-started.md)
> - [O que aconteceu](vs-key-vault-aspnet-core-what-happened.md)

Este artigo identifica as exatas alterações a um projeto ASP.NET ao adicionar o [Cofre de chaves ligado serviço com o Visual Studio](vs-key-vault-add-connected-service.md).

Para obter informações sobre como trabalhar com o serviço ligado, consulte [introdução](vs-key-vault-aspnet-core-get-started.md).

## <a name="added-references"></a>Referências adicionadas

Afeta o referências *.NET do ficheiro de projeto e referências de pacote NuGet.

| Tipo | Referência |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

## <a name="added-files"></a>Foram adicionados ficheiros

- ConnectedService.json adicionado, que regista algumas informações sobre o fornecedor de serviços ligados, versão e uma ligação a documentação.

## <a name="project-file-changes"></a>Alterações ao ficheiro de projeto

- Adicionar o ficheiro ItemGroup de serviços ligados e ConnectedServices.json.

## <a name="launchsettingsjson-changes"></a>alterações de launchsettings.JSON

- Adicionar as seguintes entradas de variável de ambiente para o perfil de Express de IIS e o perfil que corresponda ao seu nome de projeto web:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

## <a name="changes-on-azure"></a>Alterações no Azure

- Criar um grupo de recursos (ou utilizar um existente).
- Criar um cofre de chaves no grupo de recursos especificado.

