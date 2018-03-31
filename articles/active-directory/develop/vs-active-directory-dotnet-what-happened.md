---
title: As alterações efetuadas a um projeto MVC quando ligar ao Azure AD | Microsoft Docs
description: Descreve o que acontece ao seu projeto MVC quando ligar ao Azure AD utilizando os serviços do Visual Studio ligado
services: active-directory
documentationcenter: na
author: ghogen
manager: douge
editor: ''
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: ghogen
ms.custom: aaddev
ms.openlocfilehash: 1925a32ce5745673c08af3f9cfe63090d4adfa23
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/30/2018
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a>O que aconteceu ao meu projeto MVC (serviço ligado da Visual Studio do Azure Active Directory)?

> [!div class="op_single_selector"]
> - [Introdução](vs-active-directory-dotnet-getting-started.md)
> - [O que aconteceu](vs-active-directory-dotnet-what-happened.md)

Este artigo identifica as alterações exatas efetuadas am projeto de ASP.NET MVC ao adicionar o [do Azure Active Directory ligado serviço com o Visual Studio](vs-active-directory-add-connected-service.md).

Para obter informações sobre como trabalhar com o serviço ligado, consulte [introdução](vs-active-directory-dotnet-getting-started.md).

## <a name="added-references"></a>Referências adicionadas

Afeta as referências *.NET do ficheiro de projeto) e `packages.config` (NuGet referências).

| Tipo | Referência |
| --- | --- |
| .NET; NuGet | Microsoft.IdentityModel.Protocol.Extensions |
| .NET; NuGet | Microsoft.Owin |
| .NET; NuGet | Microsoft.Owin.Host.SystemWeb |
| .NET; NuGet | Microsoft.Owin.Security |
| .NET; NuGet | Microsoft.Owin.Security.Cookies |
| .NET; NuGet | Microsoft.Owin.Security.OpenIdConnect |
| .NET; NuGet | Owin |
| .NET        | System.IdentityModel |
| .NET; NuGet | System.IdentityModel.Tokens.Jwt |
| .NET        | System.Runtime.Serialization |

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
| .NET        | Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms (Visual Studio 2015 apenas) |
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
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="<your selected Azure domain>" />
        <add key="ida:TenantId" value="<the Id of your selected Azure AD tenant>" />
        <add key="ida:PostLogoutRedirectUri" value="<project start page, such as https://localhost:44335>" />
    </appSettings>
    ```

- Adicionado `<dependentAssembly>` elementos sob o `<runtime><assemblyBinding>` nós para `System.IdentityModel.Tokens.Jwt` e `Microsoft.IdentityModel.Protocol.Extensions`.

Alterações adicionais se tiver selecionado o **ler os dados de diretório** opção:

- Adicionar a seguinte entrada de configuração em `<appSettings>`:

    ```xml
    <add key="ida:ClientSecret" value="<Azure AD app's new client secret>" />
    ```

- Adicionar os seguintes elementos em `<configuration>`; os valores para o ficheiro de projeto mdf e o id de catálogo de projeto irão variar:

    ```xml
    <configSections>
      <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
      <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>

    <connectionStrings>
      <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\<project-mdf-file>.mdf;Initial Catalog=<project-catalog-id>;Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>

    <entityFramework>
      <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
        <parameters>
          <parameter value="mssqllocaldb" />
        </parameters>
      </defaultConnectionFactory>
      <providers>
        <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
      </providers>
    </entityFramework>
    ```

- Adicionado `<dependentAssembly>` elementos sob o `<runtime><assemblyBinding>` nós para `Microsoft.Data.Services.Client`, `Microsoft.Data.Edm`, e `Microsoft.Data.OData`.

## <a name="code-changes-and-additions"></a>As adições e alterações de código

- Adicionar o `[Authorize]` atributo para `Controllers/HomeController.cs` e quaisquer outros controladores existentes.

- Adicionar uma classe de startup da autenticação, `App_Start/Startup.Auth.cs`, que contém a lógica de arranque para a autenticação do Azure AD. Se tiver selecionado o **ler os dados de diretório** opção, este ficheiro também contém um código para receber um código de OAuth e exchange-lo para um token de acesso.

- Adicionar uma classe de controlador, `Controllers/AccountController.cs`, que contêm `SignIn` e `SignOut` métodos.

- Adicionar uma vista parcial, `Views/Shared/_LoginPartial.cshtml`, que contém uma hiperligação de ação para `SignIn` e `SignOut`.

- Adicionar uma vista parcial, `Views/Account/SignoutCallback.cshtml`, que contém HTML para a IU do fim de sessão.

- Atualizar o `Startup.Configuration` método para incluir uma chamada para `ConfigureAuth(app)` se a classe já existia; caso contrário, adicionar um `Startup` classe que inclui chama o método.

- Adicionar `Connected Services/AzureAD/ConnectedService.json` (Visual Studio 2017) ou `Service References/Azure AD/ConnectedService.json` (Visual Studio 2015), que contém as informações Visual Studio utiliza para controlar a adição do serviço ligado.

- Se tiver selecionado o **ler os dados de diretório** opção, adicionada `Models/ADALTokenCache.cs` e `Models/ApplicationDbContext.cs` para suportar a colocação em cache de token. Também adicionada um controlador adicional e a vista para ilustrar ao aceder aos informações de perfil de utilizador através do gráfico APIs do Azure: `Controllers/UserProfileController.cs`, `Views/UserProfile/Index.cshtml`, e `Views/UserProfile/Relogin.cshtml`

### <a name="file-backup-visual-studio-2015"></a>Cópia de segurança de ficheiros (Visual Studio 2015)

Ao adicionar o serviço ligado, Visual Studio 2015 efetua a cópia de segurança de ficheiros alterados e removidos. Todos os ficheiros afetados são guardados na pasta `Backup/AzureAD`. Visual Studio 2017 não cria cópias de segurança.

- `Startup.cs`
- `App_Start\IdentityConfig.cs`
- `App_Start\Startup.Auth.cs`
- `Controllers\AccountController.cs`
- `Controllers\ManageController.cs`
- `Models\IdentityModels.cs`
- `Models\ManageViewModels.cs`
- `Views\Shared\_LoginPartial.cshtml`

## <a name="changes-on-azure"></a>Alterações no Azure

- Criar uma aplicação do Azure AD no domínio que selecionou quando adicionar o serviço ligado.
- Atualizar a aplicação para incluir o **ler os dados de diretório** permissão se essa opção tiver sido selecionada.

[Saiba mais sobre o Azure Active Directory](https://azure.microsoft.com/services/active-directory/).

## <a name="next-steps"></a>Passos Seguintes

- [Cenários de autenticação do Azure Active Directory](active-directory-authentication-scenarios.md)
- [Adicionar início de sessão com a Microsoft para uma aplicação web ASP.NET](guidedsetups/active-directory-aspnetwebapp-v1.md)
