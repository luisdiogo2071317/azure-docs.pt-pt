---
title: Exemplos de código do Azure Active Directory | Documentos da Microsoft
description: Fornece exemplos de código, organizados por cenário de um índice de disponíveis do Azure Active Directory (ponto final V2).
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2018
ms.author: celested
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8d2ce5bb2f44d9e21fb12e96f9c68d4ecc0fc501
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581875"
---
# <a name="azure-active-directory-code-samples-v2-endpoint"></a>Exemplos de código do Azure Active Directory (ponto final V2)

Pode utilizar o Microsoft Azure Active Directory (Azure AD) para:

- Adicionar autenticação e autorização às suas aplicações web e web APIs.
- Exigir um token de acesso para aceder a uma API web protegida.

Este artigo descreve resumidamente e fornece links para amostras para o ponto final do Azure AD V2. Estes exemplos mostram-lhe como isso é feito, juntamente com os trechos de código que pode usar em seus aplicativos. Na página de exemplo de código, encontrará Leiame detalhadas tópicos de ajudam com requisitos de instalação e configurar. Comentários dentro do código estão lá ajudar a compreender as seções críticas.

> [!NOTE]
> Se estiver interessado em exemplos de V1, veja [exemplos de código do Azure AD (ponto de final V1)](sample-v1-code.md).

Para compreender o cenário básico para cada tipo de exemplo, veja [tipos de aplicações para o ponto de final de v2.0 do Azure Active Directory](active-directory-v2-flows.md).

Também pode contribuir para os exemplos no GitHub. Para saber como, veja [Microsoft Azure Active Directory amostras e documentação](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="desktop-and-mobile-public-client-apps"></a>Aplicações de dispositivos móveis e computadores cliente público

Os exemplos seguintes mostram aplicações (aplicações de ambiente de trabalho/móvel) que o Microsoft Graph ou uma API Web no nome de um utilizador de acesso de cliente público.

Aplicação de cliente | Plataforma | Fluxo/concessão | Chamadas Microsoft Graph | Chama uma API Web ASP.NET Core 2.0
------------------ | -------- | ---------- | -------------------- | -------------------------
Ambiente de trabalho (WPF)      | .NET / C#  | Interativo | [DotNet-ambiente de trabalho-msgraph-v2](http://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) <p/> [DotNet-admin-restrito-âmbitos-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) | [DotNet-nativo-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2)
Mobile (UWP)   | .NET / C# (UWP) | Interativo | [DotNet-nativo-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) |
Móveis (Android, iOS, UWP)   | .NET / c# (Xamarin) | Interativo | [xamarin-nativo-v2](https://Github.com/azure-samples/active-directory-xamarin-native-v2) |
Móveis (iOS)       | iOS / Objective C ou swift | Interativo | [IOS swift-nativo v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) <p/> [IOS nativo-nxoauth2 da v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |
Móveis (Android)   | Android / Java | Interativo |   [Android-nativo-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |

## <a name="web-applications"></a>Aplicações Web

As seguintes amostras ilustram aplicativos da web que iniciar sessão dos utilizadores, chamam o Microsoft Graph ou chamam uma API web com a identidade do utilizador.

 Plataforma | Apenas inicia sessão dos utilizadores | Inicia sessão dos utilizadores e chama o Microsoft Graph 
 -------- | ------------------- | --------------------------------- 
ASP.NET 4.x | [appmodelv2-webapp-openIDConnect-dotNet](https://GitHub.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) <p/> [DotNet-webapp-openidconnect-v2](https://GitHub.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |              [ASPNET-connect--exemplo do rest](https://github.com/microsoftgraph/aspnet-connect-rest-sample)   
ASP.NET Core 2.0 | [aspnetcore-webapp-openidconnect-v2](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) |              [aspnetcore-connect-sample](https://github.com/microsoftgraph/aspnetcore-connect-sample)   
Node.js      |                   | [AppModelv2-WebApp-OpenIDConnect-nodejs](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs)     
Ruby      |                   | [Ruby-connect--exemplo do rest](https://github.com/microsoftgraph/ruby-connect-rest-sample)     

## <a name="daemon-applications"></a>Aplicações daemon

Os exemplos seguintes mostram aplicativos de área de trabalho ou web que acessam o Microsoft Graph ou uma API web com a identidade da aplicação (nenhum utilizador).

Aplicação de cliente | Plataforma | Fluxo/concessão | Chamadas Microsoft Graph 
------------------ | -------- | ---------- | -------------------- 
Aplicação Web | .NET / C#  | Credenciais de cliente | [DotNet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) 

## <a name="single-page-applications-spa"></a>Aplicações de página única (SPA)

Este exemplo mostra como escrever uma aplicação de página única protegida com o Azure AD.

 Plataforma |  Chamadas Microsoft Graph 
 -------- |  --------------------- 
JavaScript (msal)  | [JavaScript-Graph-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-v2) 
JavaScript (msal + AngularJS) | [angular-connect--exemplo do rest](https://github.com/microsoftgraph/angular-connect-rest-sample) 
JavaScript (Hello.JS)  | [JavaScript-Graph-web-v2](https://github.com/azure-samples/active-directory-javascript-graphapi-web-v2) 
JavaScript (hello.js + Angular 4) | [exemplo angular4 ligar](https://github.com/microsoftgraph/angular4-connect-sample) 

## <a name="web-apis"></a>APIs da Web

### <a name="web-api-protected-by-azure-ad"></a>API Web protegida pelo Azure AD

O exemplo a seguir mostra como proteger uma API web com o ponto final do Azure AD V2.

Plataforma | Sample | Descrição
 -------- | ------------------- | ---------------------
Node.js | [DotNet-nativo-aspnetcore-v2](https://GitHub.com/azure-samples/active-directory-dotnet-native-aspnetcore-v2) | Chama uma API Web do ASP.NET Core a partir de um aplicativo do WPF com o Azure AD V2.

### <a name="web-api-calling-microsoft-graph-or-another-web-api"></a>API de Web chamada Microsoft Graph ou outra API Web

Este exemplo ainda não está disponível.

## <a name="other-microsoft-graph-samples"></a>Outros exemplos do Microsoft Graph

Para saber mais sobre [amostras](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) e tutoriais que demonstram os diferentes padrões de uso para a Graph API da Microsoft, incluindo a autenticação com o Azure AD, consulte [exemplos da Comunidade do Microsoft Graph e tutoriais](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Consulte também

[Guia para programadores do Azure Active Directory](azure-ad-developers-guide.md)

[Azure AD Graph API conceitual e de referência](https://msdn.microsoft.com/library/azure/hh974476.aspx)

[Biblioteca de auxiliar de API do Graph do Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
