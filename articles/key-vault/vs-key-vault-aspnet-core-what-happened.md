---
title: As alterações feitas a do ASP.NET Core do projeto quando ligar ao Azure Key Vault | Documentos da Microsoft
description: Descreve o que acontece ao seu projeto ASP.NET Core quando se liga toKey cofre com os serviços ligados do Visual Studio.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: b7cbe55fa3a524965e0ebc16c5ff350a60d6e440
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056046"
---
# <a name="what-happened-to-my-aspnet-core-project-visual-studio-key-vault-connected-service"></a>O que aconteceu ao meu projeto ASP.NET Core (serviço ligado do Cofre de chaves do Visual Studio)?

> [!div class="op_single_selector"]
> - [Introdução](vs-key-vault-aspnet-core-get-started.md)
> - [O que aconteceu](vs-key-vault-aspnet-core-what-happened.md)

Este artigo identifica as alterações exatas feitas para um projeto do ASP.NET, ao adicionar o [com o Visual Studio do serviço de ligado do Key Vault](vs-key-vault-add-connected-service.md).

Para obter informações sobre como trabalhar com o serviço ligado, consulte [introdução ao](vs-key-vault-aspnet-core-get-started.md).

## <a name="added-references"></a>Foi adicionadas referências

Afeta as referências *.NET do arquivo de projeto e referências de pacote NuGet.

| Tipo | Referência |
| --- | --- |
| NuGet | Microsoft.AspNetCore.AzureKeyVault.HostingStartup |

## <a name="added-files"></a>Ficheiros adicionados

- ConnectedService.json adicionado, que regista algumas informações sobre o fornecedor de serviços ligados, versão e uma ligação a documentação.

## <a name="project-file-changes"></a>Alterações de ficheiros de projeto

- Adicionar o ficheiro de ItemGroup de serviços ligados e ConnectedServices.json.

## <a name="launchsettingsjson-changes"></a>alterações de launchsettings.JSON

- Adicionadas as seguintes entradas de variável de ambiente para o perfil de IIS Express e o perfil que corresponda ao seu nome de projeto da web:

    ```json
      "environmentVariables": {
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED": "true",
        "ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT": "<your keyvault URL>"
      }
    ```

## <a name="changes-on-azure"></a>Alterações no Azure

- Criou um grupo de recursos (ou utilizar um já existente).
- Criou um Key Vault no grupo de recursos especificado.

