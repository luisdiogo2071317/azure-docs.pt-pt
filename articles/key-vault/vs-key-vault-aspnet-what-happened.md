---
title: As alterações efetuadas a um ASP.NET projeto quando connec ao Cofre de chaves do Azure | Microsoft Docs
description: Descreve o que acontece ao seu projeto ASP.NET quando se liga cofre toKey utilizando os serviços do Visual Studio ligado.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: a43f893c7ee87ffb02179c06ea5786715547e93a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787361"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-key-vault-connected-service"></a>O que aconteceu ao projeto do ASP.NET (serviço ligado da Cofre de chaves do Visual Studio)?

> [!div class="op_single_selector"]
> - [Introdução](vs-key-vault-aspnet-get-started.md)
> - [O que aconteceu](vs-key-vault-aspnet-what-happened.md)

Este artigo identifica as exatas alterações a um projeto ASP.NET ao adicionar o [Cofre de chaves ligado serviço com o Visual Studio](vs-key-vault-add-connected-service.md).

Para obter informações sobre como trabalhar com o serviço ligado, consulte [introdução](vs-key-vault-aspnet-get-started.md).

## <a name="added-references"></a>Referências adicionadas

Afeta as referências *.NET do ficheiro de projeto e `packages.config` (NuGet referências).

| Tipo | Referência |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

## <a name="added-files"></a>Foram adicionados ficheiros

- ConnectedService.json adicionado, que regista algumas informações sobre o fornecedor de serviços ligados, versão e uma hiperligação para a documentação.

## <a name="project-file-changes"></a>Alterações ao ficheiro de projeto

- Adicionar o ficheiro ItemGroup de serviços ligados e ConnectedServices.json.
- Referências a assemblagens .NET descritas a [adicionar referências](#added-references) secção.

## <a name="webconfig-or-appconfig-changes"></a>alterações do Web. config ou o App. config

- Adicionar as seguintes entradas de configuração:

    ```xml
    <appSettings>
       <add key="vaultName" value="<your Key Vault name>" />
       <add key="vaultUri" value="<the URI to your Key Vault in Azure>" />
    </appSettings>
    ```

## <a name="changes-on-azure"></a>Alterações no Azure

- Criar um grupo de recursos (ou utilizar um existente).
- Criar um cofre de chaves no grupo de recursos especificado.

