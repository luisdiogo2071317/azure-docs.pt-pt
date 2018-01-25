---
title: Azure Active Directory para programadores | Microsoft Docs
description: "Este artigo fornece uma descrição geral sobre o início de sessão nas contas profissionais e escolares da Microsoft através da utilização do Azure Active Directory."
services: active-directory
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 5c872c89-ef04-4f4c-98de-bc0c7460c7c2
ms.service: active-directory
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 89a232af6387f6403e6e341cced16d06e9979dae
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="azure-active-directory-for-developers"></a>Azure Active Directory para programadores
O Azure Active Directory (Azure AD) é um serviço de identidade em cloud que permite que os programadores iniciem a sessão dos utilizadores de forma segura com uma conta profissional ou escolar da Microsoft. Esta documentação mostra-lhe como adicionar suporte do Azure AD à aplicação através da utilização de protocolos padrão da indústria: o OAuth2.0 e o OpenID Connect.

| | |
| --- | --- |
|[Noções básicas de autenticação](active-directory-authentication-scenarios.md) | Introdução à autenticação com o Azure AD. |
|[Tipos de aplicações](active-directory-authentication-scenarios.md#application-types-and-scenarios) | Descrição geral dos cenários autenticação que são suportado pelo Azure AD. |                                
                                                                              
## <a name="get-started"></a>Introdução
As seguintes configurações assistidas vão guiá-lo através da utilização das bibliotecas de autenticação da Microsoft para iniciar a sessão dos utilizadores do Azure AD.

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Aplicações móveis e de ambiente de trabalho](./media/active-directory-developers-guide/NativeApp_Icon.png)<br />Aplicações móveis e de ambiente de trabalho</center> | [Descrição geral](active-directory-authentication-scenarios.md#native-application-to-web-api)<br /><br />[iOS](active-directory-devquickstarts-ios.md)<br /><br />[Android](active-directory-devquickstarts-android.md) | [.NET (WPF)](active-directory-devquickstarts-dotnet.md)<br /><br />[.NET (UWP)](active-directory-devquickstarts-windowsstore.md)<br /><br />[Xamarin](active-directory-devquickstarts-xamarin.md) | [Cordova](active-directory-devquickstarts-cordova.md) |
| <center>![Aplicações Web](./media/active-directory-developers-guide/Web_app.png)<br />Aplicações Web</center> | [Descrição geral](active-directory-authentication-scenarios.md#web-browser-to-web-application)<br /><br />[ASP.NET](active-directory-devquickstarts-webapp-dotnet.md)<br /><br />[Java](active-directory-devquickstarts-webapp-java.md) | [Node.js](active-directory-devquickstarts-openidconnect-nodejs.md) |  |
| <center>![Aplicações de página única](./media/active-directory-developers-guide/SPA.png)<br />Aplicações de página única</center> | [Descrição geral](active-directory-authentication-scenarios.md#single-page-application-spa)<br /><br />[AngularJS](active-directory-devquickstarts-angular.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) |  |  |
| <center>![APIs da Web](./media/active-directory-developers-guide/Web_API.png)<br />APIs da Web</center> | [Descrição geral](active-directory-authentication-scenarios.md#web-application-to-web-api)<br /><br />[ASP.NET](active-directory-devquickstarts-webapi-dotnet.md)<br /><br />[Node.js](active-directory-devquickstarts-webapi-nodejs.md) | &nbsp; |
| <center>![Serviço a serviço](./media/active-directory-developers-guide/Service_App.png)<br />Serviço a Serviço</center> | [Descrição geral](active-directory-authentication-scenarios.md#daemon-or-server-application-to-web-api)<br /><br />[.NET](active-directory-code-samples.md#server-or-daemon-application-to-web-api)|  |

## <a name="how-to-guides"></a>Guias de procedimentos
As seguintes guias informam-no sobre como realizar tarefas comuns com o Azure AD.

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


> [!NOTE]
> Caso precise de iniciar sessão em contas pessoais Microsoft, poderá querer considerar a utilização do [ponto final do Azure AD v2.0](active-directory-appmodel-v2-overview.md). O ponto final do Azure ADv 2.0 é unificação de contas pessoais e contas profissionais da Microsoft (a partir do Azure AD) num sistema de autenticação único.


[!INCLUDE  [Help and support](../../../includes/active-directory-develop-help-support-include.md)]