---
title: Bibliotecas de autenticação do Azure Active Directory | Microsoft Docs
description: O Azure AD Authentication Library (ADAL) permite aos programadores de aplicações autenticar facilmente os utilizadores para a nuvem de cliente ou no local do Active Directory (AD) e, em seguida, obter os tokens de acesso para proteger a chamadas de API.
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
ms.openlocfilehash: 27901608e5a7e76a799fb34b6f1f33773c0d8413
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35260581"
---
# <a name="azure-active-directory-authentication-libraries"></a>Bibliotecas de autenticação do Azure Active Directory

A v 1.0 do Azure Active Directory Authentication Library (ADAL) permite aos programadores da aplicação autenticar os utilizadores na nuvem ou no local do Active Directory (AD) e obter os tokens para proteger a chamadas de API. ADAL facilita a autenticação para os programadores através de funcionalidades, tais como:

- Cache de token configurável que arquivos de tokens de acesso e tokens de atualização
- atualização automática de token quando um token de acesso expira e um token de atualização está disponível
- suporte para chamadas de método assíncronas

> [!NOTE]
> Procurar as bibliotecas de v 2.0 do Azure AD (MSAL)? Finalizar a [guia da biblioteca MSAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-libraries).
>
>

## <a name="microsoft-supported-client-libraries"></a>Bibliotecas de cliente suportados pelo Microsoft

| Plataforma | Biblioteca | Transferência | Código de origem | Sample | Referência
| --- | --- | --- | --- | --- | --- |
| Cliente de .NET, Loja Windows, UWP, Xamarin iOS e Android |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Aplicação de ambiente de trabalho](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Referência](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) |
| .NET cliente, a loja Windows, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Aplicação de ambiente de trabalho](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | |
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Aplicação de página única](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[aplicação iOS](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Referência](https://cocoadocs.org/docsets/ADAL/)|
| Android |ADAL |[O repositório Central](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Aplicação Android](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | [Aplicação Web Node.Js](https://github.com/Azure-Samples/active-directory-node-webapp-openidconnect)|[Referência](https://docs.microsoft.com/en-us/javascript/api/adal-node/?view=azure-node-latest) |
| Java |ADAL4J |[Maven](http://search.maven.org/#search%7Cga%7C1%7Ca%3Aadal4j%20g%3Acom.microsoft.azure) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Aplicação Web Java](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect) |[Referência](http://javadoc.io/doc/com.microsoft.azure/adal4j) |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[Aplicação web Python](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi) |[Referência](http://adal-python.readthedocs.io/) |

## <a name="microsoft-supported-server-libraries"></a>Microsoft suportados pelo serviço servidor bibliotecas

| Plataforma | Biblioteca | Transferência | Código de origem | Sample | Referência
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN para AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[CodePlex](http://katanaproject.codeplex.com) |[Aplicação MVC](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |OWIN para OpenIDConnect |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[CodePlex](http://katanaproject.codeplex.com) |[Aplicação Web](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| .NET |OWIN para WS-Federation |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[CodePlex](http://katanaproject.codeplex.com) |[Aplicação Web MVC](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |Extensões de protocolo de identidade para o .NET 4.5 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |Processador JWT para o .NET 4.5 |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| Node.js |Passport do Azure AD |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [API Web](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |

## <a name="scenarios"></a>Cenários

Seguem-se três cenários comuns para utilizar a ADAL num cliente de que acede a um recurso remoto:

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Autenticação de utilizadores de uma aplicação de cliente nativo em execução num dispositivo

Neste cenário, um programador tem um cliente para dispositivos móveis ou a aplicação de ambiente de trabalho que necessitem de aceder a um recurso remoto, tais como uma API web. A API web não permite chamadas anónimas e tem de ser chamada no contexto de um utilizador autenticado. A API web está pré-configurada para tokens de acesso emitidos por um específicos de confiança inquilino do Azure AD. Azure AD está pré-configurada para emitir tokens de acesso para esse recurso. Para invocar a API web do cliente, o programador utiliza a ADAL para facilitar a autenticação com o Azure AD. É a forma mais segura para utilizar a ADAL para composição da interface de utilizador para recolher as credenciais de utilizador (compostas como janela do browser).

ADAL torna mais fácil autenticar o utilizador, obter um token de acesso e o token de atualização do Azure AD e, em seguida, chame a API utilizando o acesso web token.

Para um código de exemplo que demonstra este cenário através de autenticação para o Azure AD, consulte [aplicação nativa do WPF de cliente para Web API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>Autenticação de uma aplicação de cliente confidencial em execução num servidor web

Neste cenário, um programador tem uma aplicação em execução num servidor que tem de aceder a um recurso remoto, tais como uma API web. Chama o web que API não permitir anónima, pelo que deve ser chamado a partir de um serviço autorizado. A API web está pré-configurada para tokens de acesso emitidos por um específicos de confiança inquilino do Azure AD. Azure AD está pré-configurada para emitir tokens de acesso para esse recurso a um serviço com credenciais do cliente (ID de cliente e segredo). ADAL facilita a autenticação do serviço com o Azure AD, devolvendo um token de acesso que pode ser utilizado para chamar a API web. ADAL também faz a gestão da duração do token de acesso através da colocação em cache-lo e renová-la conforme for necessário. Para um código de exemplo que demonstra neste cenário, consulte [consola Daemon aplicação Web API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Uma aplicação de cliente confidencial em execução num servidor, em nome de um utilizador a autenticar

Neste cenário, um programador tem uma aplicação web em execução num servidor que tem de aceder a um recurso remoto, tais como uma API web. A API web não permite chamadas anónimas, pelo que deve ser chamado a partir de um serviço autorizado em nome de um utilizador autenticado. A API web está pré-configurada para confiar tokens de acesso emitidos por um específico do Azure AD e um inquilino do Azure AD está pré-configurada para emitir tokens de acesso para esse recurso a um serviço com credenciais do cliente. Depois do utilizador é autenticado na aplicação web, a aplicação pode obter um código de autorização para o utilizador a partir do Azure AD. A aplicação web, em seguida, pode utilizar a ADAL para obter um token de acesso e atualizar o token em nome de um utilizador com as credenciais de cliente e código de autorização associadas à aplicação do Azure AD. Assim que a aplicação web estiver na posse do token de acesso, pode chamar a API web até que o token expira. Quando o token expira, a aplicação web pode utilizar a ADAL para obter um novo token de acesso utilizando a atualização de token que foi anteriormente recebido. Para um código de exemplo que demonstra neste cenário, consulte [Native client a API Web para Web API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Consultar Também

- [Guia para programadores do Azure Active Directory](active-directory-developers-guide.md)
- [Cenários de autenticação para o Azure Active directory](active-directory-authentication-scenarios.md)
- [Exemplos de código do Azure Active Directory](active-directory-code-samples.md)
