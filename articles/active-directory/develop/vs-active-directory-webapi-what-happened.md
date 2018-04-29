---
title: Alterações efetuadas a um projeto de end WebAPI quando ligar ao Azure AD
description: Descreve o que acontece ao seu projeto de end WebAPI quando ligar ao Azure AD utilizando o Visual Studio
services: active-directory
author: ghogen
manager: douge
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: b8303ccf6fc96e8d0708c9e414fdf511e0cf2fdf
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/23/2018
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a>O que aconteceu ao meu projeto end WebAPI (serviço ligado da Visual Studio do Azure Active Directory)

> [!div class="op_single_selector"]
> - [Introdução](vs-active-directory-webapi-getting-started.md)
> - [O que aconteceu](vs-active-directory-webapi-what-happened.md)

Este artigo identifica as alterações exatas efetuadas para projetos end de ASP.NET WebAPI, a aplicação de página única de ASP.NET e a API do ASP.NET do Azure, ao adicionar o [do Azure Active Directory ligado serviço com o Visual Studio](vs-active-directory-add-connected-service.md). Também se aplica os projetos do serviço de Mobile do ASP.NET do Azure no Visual Studio 2015.

Para obter informações sobre como trabalhar com o serviço ligado, consulte [introdução](vs-active-directory-webapi-getting-started.md).

## <a name="added-references"></a>Referências adicionadas

Afeta as referências *.NET do ficheiro de projeto) e `packages.config` (NuGet referências).

| Tipo | Referência |
| --- | --- |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.ActiveDirectory |
| .NET; NuGet | Microsoft.Owin.Security.Jwt |
| .NET; NuGet | Microsoft.Owin.Security.OAuth |
| .NET; NuGet | Owin |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |

Referências adicionais se tiver selecionado o **ler os dados de diretório** opção:

| Tipo | Referência |
| --- | --- |
| .NET; NuGet | EntityFramework |
| .NET        | EntityFramework.SqlServer (Visual Studio 2015 apenas) |
| .NET; NuGet | Microsoft.Azure.ActiveDirectory.GraphClient |
| .NET; NuGet | Microsoft.Data.Edm |
| .NET; NuGet | Microsoft.Data.OData |
| .NET; NuGet | Microsoft.Data.Services.Client |
| .NET; NuGet | Microsoft.IdentityModel.Clients.ActiveDirectory |
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms<br>(Visual Studio 2015 apenas) |
| .NET; NuGet | System.Spatial |

As seguintes referências são removidas (ASP.NET 4 projetos só, como no Visual Studio 2015):

| Tipo | Referência |
| --- | --- |
| .NET; NuGet | Microsoft.AspNet.Identity.Core |
| .NET; NuGet | Microsoft.AspNet.Identity.EntityFramework |
| .NET; NuGet | Microsoft.AspNet.Identity.Owin |

## <a name="project-file-changes"></a>Alterações ao ficheiro de projeto

- Defina a propriedade `IISExpressSSLPort` para um número diferente.
- Defina a propriedade `WebProject_DirectoryAccessLevelKey` para 0 ou 1, se tiver selecionado o **ler os dados de diretório** opção.
- Defina a propriedade `IISUrl` para `https://localhost:<port>/` onde `<port>` corresponde a `IISExpressSSLPort` valor.

## <a name="webconfig-or-appconfig-changes"></a>alterações do Web. config ou o App. config

- Adicionar as seguintes entradas de configuração:

    ```xml
    <appSettings>
        <add key="ida:ClientId" value="<ClientId from the new Azure AD app>" />
        <add key="ida:Tenant" value="<your selected Azure domain>" />
        <add key="ida:Audience" value="<your selected domain + / + project name>" />
    </appSettings>
    ```

- Visual de apenas Studio 2017: também adicionada a seguinte entrada em `<appSettings>`"

    ```xml
    <add key="ida:MetadataAddress" value="<domain URL + /federationmetadata/2007-06/federationmetadata.xml>" />
    ```

- Adicionado `<dependentAssembly>` elementos sob o `<runtime><assemblyBinding>` nós para `System.IdentityModel.Tokens.Jwt`.

- Se tiver selecionado o **ler os dados de diretório** opção, adicionar a seguinte entrada de configuração em `<appSettings>`:

    ```xml
    <add key="ida:Password" value="<Your Azure AD app's new password>" />
    ```

## <a name="code-changes-and-additions"></a>As adições e alterações de código

- Adicionar o `[Authorize]` atributo para `Controllers/ValueController.cs` e quaisquer outros controladores existentes.

- Adicionar uma classe de startup da autenticação, `App_Start/Startup.Auth.cs`, que contém a lógica de arranque para a autenticação do Azure AD ou modificado-lo em conformidade. Se tiver selecionado o **ler os dados de diretório** opção, este ficheiro também contém um código para receber um código de OAuth e exchange-lo para um token de acesso.

- (Visual Studio 2015 com o ASP.NET 4 aplicação apenas) Remover `App_Start/IdentityConfig.cs` e adicionados `Controllers/AccountController.cs`, `Models/IdentityModel.cs`, e `Providers/ApplicationAuthProvider.cs`.

- Adicionar `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) ou `Service References/Azure AD/ConnectedService.json` (Visual Studio 2015), que contém as informações Visual Studio utiliza para controlar a adição do serviço ligado.

### <a name="file-backup-visual-studio-2015"></a>Cópia de segurança de ficheiros (Visual Studio 2015)

Ao adicionar o serviço ligado, Visual Studio 2015 efetua a cópia de segurança de ficheiros alterados e removidos. Todos os ficheiros afetados são guardados na pasta `Backup/AzureAD`. Visual Studio 2017 não cria cópias de segurança.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ApplicationOAuthProvider.cs`

## <a name="changes-on-azure"></a>Alterações no Azure

- Criar uma aplicação do Azure AD no domínio que selecionou quando adicionar o serviço ligado.
- Atualizar a aplicação para incluir o **ler os dados de diretório** permissão se essa opção tiver sido selecionada.

[Saiba mais sobre o Azure Active Directory](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Passos Seguintes

- [Cenários de autenticação do Azure Active Directory](active-directory-authentication-scenarios.md)
- [Adicionar início de sessão com a Microsoft para uma aplicação web ASP.NET](guidedsetups/active-directory-aspnetwebapp-v1.md)