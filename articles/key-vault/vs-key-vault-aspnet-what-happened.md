---
title: As alterações feitas a um ASP.NET do projeto quando ligar ao Azure Key Vault | Documentos da Microsoft
description: Descreve o que acontece ao seu projeto ASP.NET quando se liga toKey cofre com os serviços ligados do Visual Studio.
services: key-vault
author: ghogen
manager: douge
tags: azure-resource-manager
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: a74e4e10681f6b91e028067d8985408b0745dcd2
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056114"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-key-vault-connected-service"></a>O que aconteceu ao meu projeto ASP.NET (serviço ligado do Cofre de chaves do Visual Studio)?

> [!div class="op_single_selector"]
> - [Introdução](vs-key-vault-aspnet-get-started.md)
> - [O que aconteceu](vs-key-vault-aspnet-what-happened.md)

Este artigo identifica as alterações exatas feitas para um projeto do ASP.NET, ao adicionar o [com o Visual Studio do serviço de ligado do Key Vault](vs-key-vault-add-connected-service.md).

Para obter informações sobre como trabalhar com o serviço ligado, consulte [introdução ao](vs-key-vault-aspnet-get-started.md).

## <a name="added-references"></a>Foi adicionadas referências

Afeta as referências *.NET do arquivo de projeto e `packages.config` (referências de NuGet).

| Tipo | Referência |
| --- | --- |
| .NET; NuGet | Microsoft.Azure.KeyVault |
| .NET; NuGet | Microsoft.Azure.KeyVault.WebKey |
| .NET; NuGet | Microsoft.Rest.ClientRuntime |
| .NET; NuGet | Microsoft.Rest.ClientRuntime.Azure |

## <a name="added-files"></a>Ficheiros adicionados

- ConnectedService.json adicionado, que regista algumas informações sobre o fornecedor de serviços ligados, versão e uma ligação para a documentação.

## <a name="project-file-changes"></a>Alterações de ficheiros de projeto

- Adicionar o ficheiro de ItemGroup de serviços ligados e ConnectedServices.json.
- Referências aos assemblies do .NET, descritos a [adicionado referências](#added-references) secção.

## <a name="webconfig-or-appconfig-changes"></a>alterações de Web. config ou App. config

- Adicionadas as seguintes entradas de configuração:

    ```xml
    <appSettings>
       <add key="vaultName" value="<your Key Vault name>" />
       <add key="vaultUri" value="<the URI to your Key Vault in Azure>" />
    </appSettings>
    ```

## <a name="changes-on-azure"></a>Alterações no Azure

- Criou um grupo de recursos (ou utilizar um já existente).
- Criou um Key Vault no grupo de recursos especificado.

