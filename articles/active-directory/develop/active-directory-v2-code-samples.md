---
title: Exemplos de código do Azure Active Directory | Microsoft Docs
description: Fornece exemplos de código, organizados por cenário de um índice de disponíveis do Azure Active Directory (V2 ponto final).
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2018
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 39c2c5adef72db830ef226228017065c1b70f496
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Exemplos de código do Azure Active Directory (V2 ponto final)

Pode utilizar o Microsoft Azure Active Directory (Azure AD) para:

- Adicionar autenticação e autorização às suas aplicações web e web APIs.
- Exigir um token de acesso para aceder a uma API web protegido.

Este artigo descreve resumidamente e fornece ligações para exemplos para o ponto final do Azure AD V2. Estes exemplos mostram como estar concluído, juntamente com os fragmentos de código que pode utilizar nas suas aplicações. Na página de exemplo de código, encontrará tópicos de detalhado Leia-me que ajudam com requisitos de instalação e configurar. Comentários dentro do código existem para o ajudar a compreender as secções críticas.

> [!NOTE]
> Se estiver interessado em V1 exemplos, consulte o artigo [exemplos de código do Azure AD (V1 ponto final)](active-directory-code-samples.md).

Para compreender o cenário básico para cada tipo de exemplo, consulte [tipos de aplicação para o ponto de final de v 2.0 do Azure Active Directory](active-directory-v2-flows.md).

Também pode contribuir para exemplos em GitHub. Para saber como, consulte [amostras do Microsoft Azure Active Directory e a documentação](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-apps"></a>Aplicações de ambiente de trabalho e dispositivos móveis cliente público

Os exemplos seguintes mostram cliente pública aplicações (aplicações de ambiente de trabalho/mobile) que acedem à Microsoft Graph ou uma API Web do nome de um utilizador.

aplicação de cliente | Plataforma | Fluxo/conceder | Chamadas Microsoft Graph | Chama uma API Web do ASP.NET Core 2.0
------------------ | -------- | ---------- | -------------------- | -------------------------
Ambiente de trabalho (WPF)      | .NET / C #  | Interativo | [DotNet-ambiente de trabalho-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [DotNet-admin-restrito-âmbitos-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [DotNet-nativo-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Mobile (UWP)   | .NET / # C (UWP) | Interativo | [DotNet-nativo-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Móveis (Android, iOS, UWP)   | .NET / # C (Xamarin) | Interativo | [xamarin-nativo-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Móvel (iOS)       | iOS / Objective C ou swift | Interativo | [IOS-swift-nativo-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [IOS-nativo-nxoauth2 da-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Móveis (Android)   | Android / Java | Interativo |   [Android-nativo-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="web-applications"></a>Aplicações Web

Os exemplos seguintes mostram as aplicações web que os utilizadores a iniciar sessão, chamar Microsoft Graph ou chamar uma API web com a identidade do utilizador.

 Plataforma | Apenas inicia sessão dos utilizadores | Inicia sessão dos utilizadores e chama o Microsoft Graph 
 -------- | ------------------- | --------------------------------- 
ASP.NET 4. x | [appmodelv2-webapp-openIDConnect-dotNet](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [DotNet-webapp-openidconnect-v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [ASPNET-connect-rest-sample](https://github.com/microsoftgraph/aspnet-connect-rest-sample)   
Núcleo de ASP.NET 2.0 | [aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [exemplo aspnetcore ligar](https://github.com/microsoftgraph/aspnetcore-connect-sample)   
Node.js      |                   | [AppModelv2-WebApp-OpenIDConnect-nodejs](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)     
Ruby      |                   | [Ruby-connect-rest-sample](https://github.com/microsoftgraph/ruby-connect-rest-sample)     

## <a name="daemon-applications"></a>Aplicações do daemon

Os exemplos seguintes mostram as aplicações de ambiente de trabalho ou da web que acedem à Microsoft Graph ou uma API web com a identidade da aplicação (nenhum utilizador).

aplicação de cliente | Plataforma | Fluxo/conceder | Chamadas Microsoft Graph 
------------------ | -------- | ---------- | -------------------- 
Aplicação Web | .NET / C #  | Credenciais do cliente | [o daemon-DotNet-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) 

## <a name="single-page-applications-spa"></a>Aplicações de página única (SPA)

Este exemplo mostra como escrever uma aplicação de página única protegida com o Azure AD.

 Plataforma |  Chamadas Microsoft Graph 
 -------- |  --------------------- 
JavaScript (msal.js)  | [JavaScript-graphapi-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-v2) 
JavaScript (msal.js + AngularJS) | [angular para-connect-rest-sample](https://github.com/microsoftgraph/angular-connect-rest-sample) 
JavaScript (Hello.JS)  | [JavaScript-graphapi-web-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-web-v2) 
JavaScript (hello.js + 4 angular para) | [exemplo angular4 ligar](https://github.com/microsoftgraph/angular4-connect-sample) 

## <a name="web-apis"></a>APIs da Web

### <a name="web-api-protected-by-azure-ad"></a>API Web protegida pelo Azure AD

O exemplo seguinte mostra como proteger uma API web com o ponto final do Azure AD V2.

Plataforma | Sample | Descrição
 -------- | ------------------- | ---------------------
Node.js | [DotNet-nativo-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2) | Chama uma API Web do ASP.NET Core a partir de uma aplicação WPF utilizando o Azure AD V2.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>API Web chamar Microsoft Graph ou outra API Web

Este exemplo ainda não está disponível.

## <a name="other-microsoft-graph-samples"></a>Outros exemplos de Microsoft Graph

Para saber mais sobre [amostras](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) e tutoriais que demonstram padrões de utilização diferentes para a Graph API do Microsoft, incluindo a autenticação com o Azure AD, consulte [amostras de Comunidade do Microsoft Graph & tutoriais](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Consulte também

[Guia para programadores do Azure Active Directory](active-directory-developers-guide.md)

[Azure AD Graph API conceptual e de referência](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteca do Azure AD Graph API do programa auxiliar](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
