---
title: Azure Active Directory para programadores | Microsoft Docs
description: Este artigo fornece uma descrição geral sobre o início de sessão nas contas profissionais e escolares da Microsoft através da utilização do Azure Active Directory.
services: active-directory
author: dstrockis
manager: mtillman
editor: ''
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 8d70f36c5e434a26fce4d6b4bd1ddefc22234ab5
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory para programadores
O Azure Active Directory (Azure AD) é um serviço de identidade na cloud que permite que os programadores criem aplicações que iniciam a sessão dos utilizadores de forma segura com uma conta escolar ou profissional da Microsoft. O Azure AD suporta programadores que criam aplicações de inquilino único e de linha de negócio (LOB), bem como os programadores que procuram desenvolver aplicações multi-inquilino. Para além do início de sessão básico, o Azure AD também permite que as aplicações chamem APIs da Microsoft, como o [Microsoft Graph](https://developer.microsoft.com/en-us/graph/docs/concepts/overview), e APIs personalizadas que são criadas na plataforma do Azure AD.  Esta documentação mostra como adicionar suporte do Azure AD à sua aplicação através da utilização de protocolos padrão da indústria, como o OAuth2.0 e o OpenID Connect. 

> [!NOTE]
> A maioria do conteúdo desta página centra-se no ponto final do Azure AD v1, que suporta apenas contas escolares ou profissionais da Microsoft. Se pretender iniciar sessão de contas Microsoft pessoais ou de consumidor, veja mais informações sobre o [ponto final do Azure AD v2.0](active-directory-appmodel-v2-overview.md). O ponto final do Azure AD v2.0 oferece uma experiência de programação unificada para as aplicações que pretendem iniciar sessão de utilizadores com contas do Azure AD (profissionais e escolares) e contas Microsoft pessoais. 

| | |
| --- | --- |
|[Noções básicas de autenticação](active-directory-authentication-scenarios.md) | Introdução à autenticação com o Azure AD. |
|[Tipos de aplicações](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Descrição geral dos cenários autenticação que são suportado pelo Azure AD. |                                
                                                                              
## <a name="get-started"></a>Introdução
As configurações orientadas abaixo explicam como criar uma aplicação na sua plataforma preferida com o SDK do Azure Active Directory Library (ADAL). Se está à procura de informações sobre como utilizar o Microsoft Authentication Library (MSAL), veja a nossa documentação sobre o [ponto final do Azure AD v2.0](active-directory-appmodel-v2-overview.md).

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Aplicações móveis e de ambiente de trabalho](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Aplicações móveis e de ambiente de trabalho</center> | [Descrição geral](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET (WPF)](active-directory-devquickstarts-dotnet.md)<br /><br />[.NET (UWP)](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md) |
| <center>![Aplicações Web](./media/active-directory-developers-guide/Web_app.png)<br />Aplicações Web</center> | [Descrição geral](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |  |
| <center>![Aplicações de página única](./media/active-directory-developers-guide/SPA.png)<br />Aplicações de página única</center> | [Descrição geral](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![APIs da Web](./media/active-directory-developers-guide/Web_API.png)<br />APIs da Web</center> | [Descrição geral](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Serviço a serviço](./media/active-directory-developers-guide/Service_App.png)<br />Serviço a Serviço</center> | [Descrição geral](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)|  |

## <a name="how-to-guides"></a>Guias de procedimentos
Os guias abaixo explicam algumas das tarefas mais comuns no Azure AD.

|                                                                           |  |
|---------------------------------------------------------------------------| --- |
|[Registo da aplicação](active-directory-integrating-applications.md)           | Como registar uma aplicação no Azure AD. |
|[Aplicações multi-inquilino](active-directory-devhowto-multi-tenant-overview.md)    | Como iniciar sessão numa conta profissional Microsoft. |
|[Protocolos OAuth e OpenID Connect](active-directory-protocols-openid-connect-code.md)| Como iniciar a sessão de utilizadores e chamar as APIs de web utilizando os protocolos de autenticação da Microsoft. |
|[Guias de adicionais](active-directory-developers-guide-index.md#guides)        |  Uma lista de guias de que estão disponíveis para o Azure AD.   |

## <a name="reference-topics"></a>Tópicos de referência
Os seguintes artigos fornecem informações detalhadas sobre APIs, mensagens de protocolo e termos que são utilizados no Azure AD.

|                                                                                   | |
| ----------------------------------------------------------------------------------| --- |
| [Bibliotecas de Autenticação (ADAL)](active-directory-authentication-libraries.md)   | Descrição geral de bibliotecas e SDKs que são fornecidos pelo Azure AD. |
| [Exemplos de código](active-directory-code-samples.md)                                  | Uma lista de todos os exemplos de código do Azure AD. |
| [Glossário](active-directory-dev-glossary.md)                                      | Terminologia e definições de palavras que são utilizadas nesta documentação. |
| [Tópicos de referência adicionais](active-directory-developers-guide-index.md#reference)| Uma lista de tópicos de referência que estão disponíveis para o Azure AD.   |


[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
