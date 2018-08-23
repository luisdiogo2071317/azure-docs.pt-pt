---
title: Bibliotecas de autenticação v2.0 do Active Directory do Azure | Documentos da Microsoft
description: Bibliotecas de cliente compatível e bibliotecas de middleware de servidor e biblioteca relacionada, origem e ligações de exemplos, para o ponto de final de v2.0 do Azure Active Directory.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 19cec615-e51f-4141-9f8c-aaf38ff9f746
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2018
ms.author: celested
ms.reviewer: jmprieur, saeeda
ms.custom: aaddev
ms.openlocfilehash: f3ac6050ca73f96facd621823dc4bad413b139c7
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056285"
---
# <a name="azure-active-directory-v20-authentication-libraries"></a>Bibliotecas de autenticação de v2.0 do Azure Active Directory

O [ponto final de v2.0 do Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare) suporta os protocolos padrão da indústria de OAuth 2.0 e OpenID Connect 1.0. A Microsoft Authentication Library (MSAL) foi concebida para funcionar com o ponto de final de v2.0 do Azure AD. Também é possível usar bibliotecas de código-fonte aberto que suportem o OAuth 2.0 e OpenID Connect 1.0.

É recomendável que use bibliotecas escritas por especialistas de domínio de protocolo que siga uma metodologia de ciclo de vida de desenvolvimento da segurança (SDL), como [aquele seguido pela Microsoft][Microsoft-SDL]. Se optar por suporte de codificar manualmente para os protocolos, siga uma metodologia de como o SDL e o pagamento da Microsoft requer muita atenção às considerações de segurança nas especificações de padrões para cada protocolo.

> [!NOTE]
> Está à procura para as bibliotecas de versão 1.0 do Azure AD (ADAL)? Check-Out do [guia de biblioteca ADAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries).
>
>

## <a name="types-of-libraries"></a>Tipos de bibliotecas

Ponto final de v2.0 do Azure AD funciona com dois tipos de bibliotecas:

* **Bibliotecas de cliente**. Servidores e clientes nativos utilizam bibliotecas de cliente para obter os tokens de acesso para chamar um recurso, como o Microsoft Graph.
* **Bibliotecas de middleware de servidor**. Aplicações Web utilizam bibliotecas de middleware de servidor para o início de sessão do utilizador. As APIs da Web utilizam bibliotecas de middleware de servidor para validar os tokens que são enviados por clientes nativos ou por outros servidores.

## <a name="library-support"></a>Suporte de bibliotecas

Como pode escolher qualquer biblioteca compatíveis com os padrões ao utilizar o ponto final v2.0, é importante saber onde obter suporte. Para problemas e pedidos de funcionalidades no código de biblioteca, contacte o proprietário de biblioteca. Para problemas e pedidos de funcionalidades na implementação de protocolo do lado do serviço, contacte a Microsoft. [Um pedido de funcionalidade de ficheiros](https://feedback.azure.com/forums/169401-azure-active-directory) para as funcionalidades adicionais que gostaria de ver no protocolo. [Criar um pedido de suporte](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) se encontrar um problema em que o ponto de final de v2.0 do Azure AD não está em conformidade com OAuth 2.0 ou OpenID Connect 1.0.

Bibliotecas vêm em duas categorias de suporte:

* **Suporte Microsoft**. A Microsoft fornece correções para essas bibliotecas e fez o SDL auditoria detalhada dessas bibliotecas.
* **Compatível**. A Microsoft testou essas bibliotecas em cenários básicos e confirmado que elas funcionam com o ponto final v2.0. A Microsoft não fornece correções para essas bibliotecas e não fez uma revisão dessas bibliotecas. Problemas e pedidos de funcionalidades devem ser direcionados para o projeto de código aberto da biblioteca.

Para obter uma lista de bibliotecas que funcionam com o ponto final v2.0, consulte as secções seguintes neste artigo.

## <a name="microsoft-supported-client-libraries"></a>Bibliotecas de cliente suportados por Microsoft

> [!IMPORTANT]
> As bibliotecas de pré-visualização da MSAL são adequadas para utilização num ambiente de produção. A Microsoft fornece o mesmo suporte de nível de produção para essas bibliotecas como as bibliotecas de produção atual (ADAL). Durante a pré-visualização, esperar que as alterações para a API de MSAL, o formato de cache interna e outros mecanismos dessas bibliotecas, sem aviso prévio, que será necessário levar juntamente com correções de erros ou recurso. Isto pode afetar a sua aplicação. Por exemplo, uma alteração para o formato de cache pode exigir aos utilizadores iniciar sessão novamente. Uma alteração de API pode exigir que atualizar seu código. Quando a versão de disponibilidade geral (GA) estiver disponível, tem de atualizar todos os aplicativos usando uma versão de pré-visualização da biblioteca dentro de seis meses ou poderá deixar de funcionar.

| Plataforma | Biblioteca | Transferência | Código-fonte | Sample | Referência
| --- | --- | --- | --- | --- | --- |
| Cliente de .NET, Windows Store, UWP, Xamarin iOS e Android | A MSAL .NET (pré-visualização) |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Aplicativo de Desktop](guidedsetups/active-directory-mobileanddesktopapp-windowsdesktop-intro.md) |  |
| JavaScript | Msal (pré-visualização) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Aplicação de página única](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |  |
| iOS, macOS | MSAL (pré-visualização) | [GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [Aplicação iOS](https://github.com/Azure-Samples/active-directory-msal-ios-swift) |  |
| Android | MSAL (pré-visualização) | [O repositório Central](https://repo1.maven.org/maven2/com/microsoft/identity/client/msal/) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Aplicação Android](guidedsetups/active-directory-mobileanddesktopapp-android-intro.md) | [JavaDocs](http://javadoc.io/doc/com.microsoft.identity.client/msal) |

## <a name="microsoft-supported-server-middleware-libraries"></a>Bibliotecas de middleware de servidor suportados por Microsoft

| Plataforma | Biblioteca | Transferência | Código-fonte | Sample | Referência
| --- | --- | --- | --- | --- | --- |
| .NET 4.x | Middleware da OWIN OpenID Connect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[GitHub](https://github.com/aspnet/AspNetKatana/) |[Aplicação MVC](guidedsetups/active-directory-serversidewebapp-aspnetwebappowin-intro.md) | |
| .NET 4.x | Middleware de portador de OAuth da OWIN para AzureAD |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[GitHub](https://github.com/aspnet/AspNetKatana/) |  | |
| .NET 4.x | JWT manipulador para o .NET 4.5 | [NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt/4.0.4.403061554) | [GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET Core | ASP.NET middleware de OpenID Connect |[Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] |[Segurança do ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] |[Aplicação MVC](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2) |
| .NET Core | Middleware de portador do OAuth do ASP.NET |[Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] |[Segurança do ASP.NET (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] |  |
| .NET Core | Manipulador JWT para .NET Core  |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |O Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Aplicação Web](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs)| |

## <a name="compatible-client-libraries"></a>Bibliotecas de cliente compatível

| Plataforma | Nome da biblioteca | Versão testada | Código de origem | Sample |
|:---:|:---:|:---:|:---:|:---:|
| Android |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/) |0.2.1 |[OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) |[Exemplo de aplicação nativa](active-directory-v2-devquickstarts-android.md) |
| iOS |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |1.2.8 |[NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) |[Exemplo de aplicação nativa](active-directory-v2-devquickstarts-ios.md) |
| JavaScript |[Hello.js](https://adodson.com/hello.js/) |1.13.5 |[Hello.js](https://github.com/MrSwitch/hello.js) |[SPA](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| Java | [Há Java scribejava](https://github.com/scribejava/scribejava) | [Versão 3.2.0](https://github.com/scribejava/scribejava/releases/tag/scribejava-3.2.0) | [ScribeJava](https://github.com/scribejava/scribejava/) | |
| PHP | [O PHP liga Esportiva cliente oauth2](https://github.com/thephpleague/oauth2-client) | [Versão 1.4.2](https://github.com/thephpleague/oauth2-client/releases/tag/1.4.2) | [oauth2-client](https://github.com/thephpleague/oauth2-client/) | |
| Ruby |[OmniAuth](https://github.com/omniauth/omniauth/wiki) |omniauth:1.3.1</br>omniauth-oauth2:1.4.0 |[OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) |  |

## <a name="related-content"></a>Conteúdo relacionado
Para obter mais informações sobre o ponto de final de v2.0 do Azure AD, consulte a [visão geral v2.0 do modelo de aplicação do Azure AD][AAD-App-Model-V2-Overview].

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: ../active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:/
[ClientLib-Iosmac-Lib]:/
[ClientLib-Iosmac-Repo]:/
[ClientLib-Iosmac-Sample]:/
[ClientLib-Android-Lib]:/
[ClientLib-Android-Repo]:/
[ClientLib-Android-Sample]:/
[ClientLib-Js-Lib]:/
[ClientLib-Js-Repo]:/
[ClientLib-Js-Sample]:/

[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/documentation/articles/active-directory-v2-devquickstarts-node-web/
