---
title: Exemplos de código do Azure Active Directory | Documentos da Microsoft
description: Fornece exemplos de código, organizados por cenário de um índice do Azure Active Directory (ponto de final v1).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 35d837f346e3aab4f8b08286f1e0806f385762ec
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39493456"
---
# <a name="azure-active-directory-code-samples-v1-endpoint"></a>Exemplos de código do Azure Active Directory (ponto de final V1)

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Pode utilizar o Microsoft Azure Active Directory (Azure AD) para adicionar a autenticação e autorização para seus aplicativos web e web APIs.

Esta secção fornece ligações para exemplos que pode utilizar para saber mais sobre o ponto de final do Azure AD V1. Estes exemplos mostram-lhe como é feito, juntamente com os trechos de código que pode usar em seus aplicativos. Na página de exemplo de código, encontrará leitura detalhada-me tópicos de ajudam com os requisitos de instalação e configuração. E o código é comentado para ajudar a compreender as seções críticas.

> [!NOTE]
> Se estiver interessado em exemplos de código do Azure AD V2, veja [exemplos de código v2.0 por cenário](active-directory-v2-code-samples.md).

Para compreender o cenário básico para cada tipo de exemplo, veja [cenários de autenticação do Azure AD](authentication-scenarios.md).

Também pode contribuir com nossos exemplos no GitHub. Para saber como, veja [Microsoft Azure Active Directory amostras e documentação](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Aplicações de dispositivos móveis e computadores cliente pública chamada Microsoft Graph ou uma API Web

Os exemplos seguintes mostram aplicações (aplicações de ambiente de trabalho/móvel) que o Microsoft Graph ou uma API Web no nome de um utilizador de acesso de cliente público.

Aplicação de cliente | Plataforma | Fluxo/concessão | Chamadas Microsoft Graph | Chama uma API ASP.NET ou Web ASP.NET Core 2.0
------------------ | -------- | ---------- | -------------------- | -------------------------
Ambiente de trabalho (WPF)           | .NET / C# | Interativo | [nativo-DotNet-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [DotNet-nativo-ambiente de trabalho](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [nativo-DotNet-aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [DotNet-webapi-manual-jwt-validação](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobile (UWP)            | .NET / C#  | Interativo | [DotNet-nativo-uwp-wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  [DotNet-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (único inquilino Web API) </p> [DotNet-webapi-multi-inquilino-windows-store](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (Web API de multi-inquilino)|
Móveis (Android, iOS, UWP)   | .NET / c# (Xamarin) | Interativo | [nativo-DotNet-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Móveis (Android)           | Android/Java | Interativo |   [Android](https://github.com/Azure-Samples/active-directory-android) |
Móveis (iOS)           | iOS/Objective C | Interativo |   [nativeClient-iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Ambiente de trabalho (consola)          | .NET / C# | Nome de utilizador / palavra-passe </p> Autenticação Integrada do Windows | | [DotNet-nativo-sem periféricos](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Ambiente de trabalho (consola)           | .NET core / c# | Perfil de dispositivo | | [DotNet deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="web-applications"></a>Aplicações Web

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Aplicativos Web a iniciar sessão dos utilizadores, chamando o Microsoft Graph ou uma API Web com a identidade do utilizador

 Plataforma | Apenas inicia sessão dos utilizadores | Chamadas Microsoft Graph ou Graph do AAD| Chama outro ASP.NET ou API Web do ASP.NET Core 2.0
 -------- | ------------------- | --------------------- | -------------------------
ASP.NET 4.5 | [aplicação Web-openidconnect-dotnet](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-aspnetwebapp-v1) </p> [aplicação Web-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | [DotNet-webapp-multi-inquilino-openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) (Graph do AAD) |
ASP.NET Core 2.0 | [DotNet-webapp-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [aplicação Web-webapi-multi-inquilino-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) (Graph do AAD) | [DotNet-webapp-webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
ASP.NET 4.5 | [DotNet-webapp-webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | |
Python | | [aplicação Web-Python-Graph](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
Java | | [aplicação Web-Java-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
PHP | | [web-Graph-PHP](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Aplicativos Web demonstrar o controlo de acesso baseado em funções (autorização)

Os exemplos seguintes mostram como implementar o controlo de acesso baseado em funções, o que é utilizado para restringir as permissões de determinadas funcionalidades de um aplicativo da web a determinados utilizadores. Os utilizadores estão autorizados a dependendo se pertencem a uma função ou grupo do Azure AD.

Plataforma | Sample | Descrição
 -------- | ------------------- | ---------------------
ASP.NET 4.5 | [aplicação Web-DotNet-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Uma aplicação web de MVC do .NET 4.5 que utiliza o Azure AD **grupos** para autorização
ASP.NET 4.5 | [aplicação Web-DotNet-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Uma aplicação web de MVC do .NET 4.5 que utiliza o Azure AD **funções** para autorização

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Aplicações daemon (aceder a APIs da Web com a identidade da aplicação)

Os exemplos seguintes mostram aplicativos de área de trabalho ou web que acessam o Microsoft Graph ou de uma web API com nenhum utilizador (com a identidade da aplicação).

Aplicação de cliente | Plataforma | Fluxo/concessão | Chamadas Microsoft Graph | Chama uma API ASP.NET ou Web ASP.NET Core 2.0
------------------ | -------- | ---------- | -------------------- | -------------------------
Aplicação de daemon (consola)          | .NET / C#  | Credenciais de cliente com o segredo da aplicação ou certificado | | [DotNet-daemon](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [DotNet-daemon-certificado-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
Aplicação de daemon (consola)         | .NET core / c# | Credenciais de cliente com certificado| | [dotnetcore-daemon-certificado-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
Ambiente de trabalho            | Java | Credenciais de cliente |   [Java-nativo-sem periféricos](https://github.com/azure-samples/active-directory-java-native-headless) |
Aplicação ASP.NET Web  | .NET / C# | Credenciais de cliente |    | [DotNet-webapp-webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>APIs da Web

### <a name="web-api-protected-by-azure-active-directory"></a>API Web protegida pelo Azure Active Directory

O exemplo a seguir mostra como proteger uma API com o Azure AD da web de node. js.

Plataforma | Sample | Descrição
 -------- | ------------------- | ---------------------
Node.js | [nó webapi](https://github.com/Azure-Samples/active-directory-node-webapi) |  API que está protegida com o Azure AD de web do NodeJS e tokens de acesso de OAuth 2.0.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>API de Web chamada Microsoft Graph ou outra API Web

Os exemplos seguintes demonstram uma web API, que chama a API web de outra. O segundo exemplo mostra como lidar com acesso condicional.

 Plataforma |  Chamadas Microsoft Graph | Chama outro ASP.NET ou API Web do ASP.NET Core 2.0
 -------- |  --------------------- | -------------------------
ASP.NET 4.5 | [webapi-DotNet-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) |[webapi-DotNet-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof)
ASP.NET 4.5 | [DotNet-webapi-onbehalfof-AC](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |[DotNet-webapi-onbehalfof-AC](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca)

## <a name="single-page-applications"></a>Aplicações de página única

Este exemplo mostra como escrever uma aplicação de página única protegida com o Azure AD.

 Plataforma |  Chamadas Microsoft Graph | Chama a sua própria API | Chama outra API Web
 -------- |  --------------------- | ------------------ | ----------------
JavaScript / ASP.NET 4.x |  | [singlepageapp de JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |
JavaScript (AngularJS) / ASP.NET 4.x |  | [angularjs singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp) |
JavaScript (AngularJS) / ASP.NET 4.x |  |  | [angularjs-singlepageapp-cors](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi)

## <a name="other-microsoft-graph-samples"></a>Outros exemplos do Microsoft Graph

Para exemplos e tutoriais que demonstram os diferentes padrões de uso para a Graph API da Microsoft, incluindo a autenticação com o Azure AD, consulte [exemplos de Comunidade do Microsoft Graph e tutoriais](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Consulte também

[Guia para programadores do Azure Active Directory](azure-ad-developers-guide.md)

[Azure AD Graph API conceitual e de referência](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteca de auxiliar de API do Graph do Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
