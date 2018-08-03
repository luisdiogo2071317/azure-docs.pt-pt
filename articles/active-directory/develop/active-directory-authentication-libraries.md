---
title: Bibliotecas de autenticação do Active Directory do Azure | Documentos da Microsoft
description: O Azure AD Authentication Library (ADAL) permite que o cliente os desenvolvedores de aplicativos autenticar facilmente os utilizadores para a cloud ou no local do Active Directory (AD) e, em seguida, obter tokens de acesso para proteger as chamadas de API.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/13/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: bb8191ee715c2fee99e408035209e0d2c8f9f02b
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447562"
---
# <a name="azure-active-directory-authentication-libraries"></a>Bibliotecas de autenticação do Azure Active Directory

A versão 1.0 do Azure Active Directory Authentication Library (ADAL) permite que os desenvolvedores de aplicativos autenticar os utilizadores para a cloud ou no local do Active Directory (AD) e obtenha tokens para proteger as chamadas de API. ADAL facilita a autenticação para os desenvolvedores por meio de recursos, tais como:

- Cache de tokens configurável que os arquivos de tokens de acesso e tokens de atualização
- Atualização automática de token quando um token de acesso expira e um token de atualização está disponível
- Suporte para chamadas de método assíncrono

> [!NOTE]
> Está à procura para as bibliotecas de v2.0 do Azure AD (MSAL)? Check-Out do [guia de biblioteca MSAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

## <a name="microsoft-supported-client-libraries"></a>Bibliotecas de cliente suportados por Microsoft

| Plataforma | Biblioteca | Transferência | Código-fonte | Sample | Referência
| --- | --- | --- | --- | --- | --- |
| Cliente de .NET, Windows Store, UWP, Xamarin iOS e Android |V3 .NET da ADAL |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Aplicativo de Desktop](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Referência](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) |
| .NET client, Windows Store, Windows Phone 8.1 |.NET da ADAL v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Aplicativo de Desktop](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | |
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Aplicação de página única](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[aplicação iOS](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Referência](http://cocoadocs.org/docsets/ADAL/2.5.1/)|
| Android |ADAL |[O repositório Central](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Aplicação Android](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Aplicação Web Node.Js](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)|[Referência](https://docs.microsoft.com/en-us/javascript/api/adal-node/?view=azure-node-latest) |
| Java |ADAL4J |[Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Aplicação Web Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) |[Referência](http://javadoc.io/doc/com.microsoft.azure/adal4j) |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Aplicação web Python](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Referência](http://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Bibliotecas de suporte do Microsoft Server

| Plataforma | Biblioteca | Transferência | Código-fonte | Sample | Referência
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN para AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[Aplicação MVC](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN para OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Aplicação Web](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |OWIN para WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[Aplicação Web MVC](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Extensões de protocolo de identidade para o .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |JWT manipulador para o .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |O Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [API Web](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Cenários

Seguem-se três cenários comuns para a utilização da ADAL num cliente que acede a um recurso remoto:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Autenticar utilizadores com uma aplicação de cliente nativo em execução num dispositivo

Neste cenário, um programador tem um cliente móvel ou um aplicativo de área de trabalho que precisa para aceder a um recurso remoto, como uma API web. A API web não permite chamadas de anônimas e tem de ser chamada no contexto de um usuário autenticado. A API web está pré-configurada para confiar tokens de acesso emitidos por uma específicas do inquilino do Azure AD. O Azure AD é pré-configurado para emitir tokens de acesso para esse recurso. Para invocar a API web do cliente, o desenvolvedor utiliza a ADAL para facilitar a autenticação com o Azure AD. A forma mais segura para utilizar a ADAL é que a composição da interface de utilizador para recolher as credenciais de utilizador (incorporadas como a janela do browser).

ADAL torna mais fácil autenticar o utilizador, obter um token de acesso e o token de atualização do Azure AD e, em seguida, chamar a API através do acesso web token.

Para obter um exemplo de código que demonstra este cenário através de autenticação para o Azure AD, consulte [aplicativo nativo do WPF de cliente para Web API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Autenticar uma aplicação de cliente confidencial em execução num servidor web

Neste cenário, um programador tem uma aplicação em execução num servidor que tem de aceder a um recurso remoto, como uma API web. Chama o web que API não permitir anónima, pelo que tem de ser chamado a partir de um serviço autorizado. A API web está pré-configurada para confiar tokens de acesso emitidos por uma específicas do inquilino do Azure AD. O Azure AD é pré-configurado para emitir tokens de acesso para esse recurso para um serviço com as credenciais de cliente (ID de cliente e segredo). ADAL facilita a autenticação do serviço com o Azure AD, retornando um token de acesso que pode ser utilizado para chamar a API web. ADAL também processa a gerir o ciclo de vida do token de acesso através da colocação em cache e efetuar a renovação conforme necessário. Para obter um exemplo de código que demonstra este cenário, consulte [consola de Daemon aplicação à Web API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Autenticar uma aplicação de cliente confidencial em execução num servidor, nome de um utilizador

Neste cenário, um programador tem uma aplicação web em execução num servidor que tem de aceder a um recurso remoto, como uma API web. A API web não permite chamadas de anônimas, portanto, tem de ser chamado a partir de um serviço autorizado em nome de um usuário autenticado. A API web está pré-configurada para confiar tokens de acesso emitidos por um específico do Azure AD e um inquilino do Azure AD está pré-configurada para emitir tokens de acesso para esse recurso para um serviço com as credenciais de cliente. Assim que o utilizador é autenticado na aplicação web, o aplicativo pode obter um código de autorização para o utilizador do Azure AD. O aplicativo web, em seguida, pode utilizar a ADAL para obter um token de acesso e atualizar o token em nome de utilizador utilizando as credenciais de cliente e de código de autorização associadas com a aplicação do Azure AD. Assim que a aplicação web estiver na posse do token de acesso, ele pode chamar a API web até o token expirar. Quando o token expira, o aplicativo web pode utilizar a ADAL para obter um novo token de acesso ao utilizar a atualização de token que recebeu anteriormente. Para obter um exemplo de código que demonstra este cenário, consulte [cliente nativo para a API Web para Web API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Consultar Também

- [Guia para programadores do Azure Active Directory](azure-ad-developers-guide.md)
- [Cenários de autenticação para o Azure Active directory](active-directory-authentication-scenarios.md)
- [Exemplos de código do Azure Active Directory](active-directory-code-samples.md)
