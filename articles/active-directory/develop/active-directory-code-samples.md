---
title: Exemplos de código do Azure Active Directory | Microsoft Docs
description: Fornece exemplos de código, organizados por cenário de um índice da Azure Active Directory (v1 ponto final).
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
ms.openlocfilehash: 299e0d4fa53f9b8a2aef2fc881b136aa41aacfe4
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="azure-active-directory-code-samples-v1-endpoint"></a>Exemplos de código do Azure Active Directory (V1 ponto final)

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Pode utilizar o Microsoft Azure Active Directory (Azure AD) para adicionar a autenticação e autorização para as suas aplicações web e web APIs.

Esta secção fornece ligações para exemplos que pode utilizar para obter mais informações sobre o ponto final do Azure AD V1. Estes exemplos mostram como é efetuada, juntamente com os fragmentos de código que pode utilizar nas suas aplicações. Na página de exemplo de código, encontrará detalhada Leia-me tópicos de ajudam com requisitos, instalação e configuração. E o código é comentado para o ajudar a compreender as secções críticas.

> [!NOTE]
> Se estiver interessado em exemplos de código do Azure AD V2, consulte o artigo [v 2.0 exemplos de código por cenário](active-directory-v2-code-samples.md).

Para compreender o cenário básico para cada tipo de exemplo, consulte [cenários de autenticação para o Azure AD](active-directory-authentication-scenarios.md).

Também pode contribuir para os nossos exemplos em GitHub. Para saber como, consulte [amostras do Microsoft Azure Active Directory e a documentação](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-applications-calling-microsoft-graph-or-a-web-api"></a>Aplicações de ambiente de trabalho e dispositivos móveis cliente pública chamar Microsoft Graph ou uma API Web

Os exemplos seguintes mostram cliente pública aplicações (aplicações de ambiente de trabalho/mobile) que acedem à Microsoft Graph ou uma API Web do nome de um utilizador.

aplicação de cliente | Plataforma | Fluxo/conceder | Chamadas Microsoft Graph | Chama uma API Web de ASP.NET Core 2.0 ou ASP.NET
------------------ | -------- | ---------- | -------------------- | -------------------------
Ambiente de trabalho (WPF)           | .NET / C # | Interativo | [DotNet-nativo-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) | [DotNet-nativo-ambiente de trabalho](https://github.com/Azure-Samples/active-directory-dotnet-native-desktop) </p> [DotNet-nativo-aspnetcore](https://azure.microsoft.com/resources/samples/active-directory-dotnet-native-aspnetcore/)</p> [DotNet end webapi-manual-jwt-validação](https://github.com/azure-samples/active-directory-dotnet-webapi-manual-jwt-validation)
Mobile (UWP)            | .NET / C #  | Interativo | [DotNet-nativo uwp wam](https://github.com/azure-samples/active-directory-dotnet-native-uwp-wam) |  [loja do windows do DotNet](https://github.com/Azure-Samples/active-directory-dotnet-windows-store) (único inquilino Web API) </p> [DotNet-end webapi-multi-inquilino-windows-arquivo](https://github.com/Azure-Samples/active-directory-dotnet-webapi-multitenant-windows-store) (multi-inquilino Web API)|
Móveis (Android, iOS, UWP)   | .NET / # C (Xamarin) | Interativo | [DotNet-nativo-multitarget](https://github.com/azure-samples/active-directory-dotnet-native-multitarget) |
Móveis (Android)           | Android/Java | Interativo |   [Android](https://github.com/Azure-Samples/active-directory-android) |
Móvel (iOS)           | iOS/Objective C | Interativo |   [nativeClient iOS](https://github.com/azureadquickstarts/nativeclient-ios) |
Ambiente de trabalho (consola)          | .NET / C # | Nome de utilizador / palavra-passe </p> Autenticação Integrada do Windows | | [DotNet-nativo-sem interface](https://github.com/azure-samples/active-directory-dotnet-native-headless)
Ambiente de trabalho (consola)           | .NET core / c# | Perfil de dispositivo | | [DotNet deviceprofile](https://github.com/Azure-Samples/active-directory-dotnet-deviceprofile)

## <a name="web-applications"></a>Aplicações Web

### <a name="web-applications-signing-in-users-calling-microsoft-graph-or-a-web-api-with-the-users-identity"></a>Aplicações Web, os utilizadores de início de sessão, chamar Microsoft Graph, ou uma API Web com a identidade do utilizador

 Plataforma | Apenas inicia sessão dos utilizadores | Chamadas Microsoft Graph ou gráfico do AAD| Chama outro ASP.NET ou API Web do ASP.NET Core 2.0
 -------- | ------------------- | --------------------- | -------------------------
ASP.NET 4.5 | [webApp-openidconnect-dotnet](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-aspnetwebapp-v1) </p> [WebApp-WSFederation-dotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-wsfederation) | [DotNet-webapp multi-inquilino openidconnect](https://github.com/Azure-Samples/active-directory-dotnet-webapp-multitenant-openidconnect) (gráfico de AAD) |
Núcleo de ASP.NET 2.0 | [DotNet-webapp openidconnect aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore) | [WebApp-end webapi-multi-inquilino-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-webapp-webapi-multitenant-openidconnect-aspnetcore/) (gráfico de AAD) | [DotNet-webapp-end webapi-openidconnect-aspnetcore](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore)
ASP.NET 4.5 | [DotNet-webapp-end webapi-oauth2-useridentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-useridentity) | |
Python | | [Python-webapp-graphapi](https://github.com/Azure-Samples/active-directory-python-webapp-graphapi)  |
Java | | [webapp-Java-openidconnect](https://github.com/azure-samples/active-directory-java-webapp-openidconnect)  |
PHP | | [PHP-graphapi-web](https://github.com/Azure-Samples/active-directory-php-graphapi-web)  |

### <a name="web-applications-demonstrating-role-based-access-control-authorization"></a>Aplicações Web demonstrar o controlo de acesso baseado em funções (autorização)

Os exemplos seguintes mostram como implementar o controlo de acesso baseado em funções, o que é utilizado para restringir as permissões de determinadas funcionalidades de uma aplicação web a determinados utilizadores. Os utilizadores estão autorizados, dependendo se pertencem a uma função ou grupo do Azure AD.

Plataforma | Sample | Descrição
 -------- | ------------------- | ---------------------
ASP.NET 4.5 | [webapp-DotNet-groupclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-groupclaims) | Uma aplicação de web de MVC do .NET 4.5 que utiliza o Azure AD **grupos** para autorização
ASP.NET 4.5 | [webapp-DotNet-roleclaims](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) | Uma aplicação de web de MVC do .NET 4.5 que utiliza o Azure AD **funções** para autorização

## <a name="daemon-applications-accessing-web-apis-with-the-applications-identity"></a>Aplicações de daemon (aceder a APIs da Web com a identidade da aplicação)

Os exemplos seguintes mostram as aplicações de ambiente de trabalho ou da web que acedem à Microsoft Graph ou uma API web com nenhum utilizador (com a identidade da aplicação).

aplicação de cliente | Plataforma | Fluxo/conceder | Chamadas Microsoft Graph | Chama uma API Web de ASP.NET Core 2.0 ou ASP.NET
------------------ | -------- | ---------- | -------------------- | -------------------------
O daemon aplicação (consola)          | .NET / C #  | Credenciais do cliente com o segredo da aplicação ou o certificado | | [daemon de DotNet](https://github.com/azure-samples/active-directory-dotnet-daemon)</p> [DotNet-daemon-certificados-credential](https://github.com/azure-samples/active-directory-dotnet-daemon-certificate-credential)
O daemon aplicação (consola)         | .NET core / c# | Credenciais do cliente com certificado| | [dotnetcore-daemon-certificados-credential](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-certificate-credential)
Ambiente de trabalho            | Java | Credenciais de cliente |   [Java-nativo-sem interface](https://github.com/azure-samples/active-directory-java-native-headless) |
Aplicação ASP.NET Web  | .NET / C # | Credenciais de cliente |    | [DotNet-webapp-end webapi-oauth2-appidentity](https://github.com/Azure-Samples/active-directory-dotnet-webapp-webapi-oauth2-appidentity)

## <a name="web-apis"></a>APIs da Web

### <a name="web-api-protected-by-azure-active-directory"></a>API Web protegida pelo Azure Active Directory

O exemplo seguinte mostra como proteger uma API com o Azure AD da web de node.js.

Plataforma | Sample | Descrição
 -------- | ------------------- | ---------------------
Node.js | [end webapi do nó](https://github.com/Azure-Samples/active-directory-node-webapi) |  NodeJS web API que está protegida com o Azure AD e os tokens de acesso de OAuth 2.0.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>API Web chamar Microsoft Graph ou outra API Web

Os exemplos seguintes demonstram uma API web do que chama a API web do outro. O segundo exemplo mostra como lidar com acesso condicional.

 Plataforma |  Chamadas Microsoft Graph | Chama outro ASP.NET ou API Web do ASP.NET Core 2.0
 -------- |  --------------------- | -------------------------
ASP.NET 4.5 | [end webapi-DotNet-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof) |[end webapi-DotNet-onbehalfof](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof)
ASP.NET 4.5 | [DotNet-end webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca) |[DotNet-end webapi-onbehalfof-ca](https://github.com/azure-samples/active-directory-dotnet-webapi-onbehalfof-ca)

## <a name="single-page-applications"></a>Aplicações de página única

Este exemplo mostra como escrever uma aplicação de página única protegida com o Azure AD.

 Plataforma |  Chamadas Microsoft Graph | Chama a sua própria API
 -------- |  --------------------- | -------------------------
JavaScript (angular para) / ASP.NET 4. x |  | [angularjs singlepageapp](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp)

## <a name="other-microsoft-graph-samples"></a>Outros exemplos de Microsoft Graph

Para exemplos e tutoriais que demonstram padrões de utilização diferentes para a Graph API do Microsoft, incluindo a autenticação com o Azure AD, consulte [amostras de Comunidade do Microsoft Graph & tutoriais](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Consulte também

[Guia para programadores do Azure Active Directory](active-directory-developers-guide.md)

[Azure AD Graph API Conceptual e de referência](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteca do Azure AD Graph API do programa auxiliar](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
