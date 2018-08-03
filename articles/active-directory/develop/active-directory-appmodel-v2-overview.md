---
title: Ponto final de v2.0 do Azure Active Directory | Documentos da Microsoft
description: Uma introdução à criação de aplicativos com Microsoft Account e Azure Active Directory início de sessão.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: celested
ms.reviewer: hirsin, jmprieur, elisol, dastrock
ms.custom: aaddev
ms.openlocfilehash: b90e03ad15f3656c06f36d6114250298db91e9ad
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430328"
---
# <a name="sign-in-microsoft-account-and-azure-active-directory-users-in-a-single-application"></a>Inicie sessão no Microsoft Account e Azure Active Directory a usuários num único aplicativo
No passado, os desenvolvedores de aplicativos que queriam suportar ambas as contas Microsoft pessoais e contas do Azure Active Directory profissionais tinham de integrar com dois sistemas separados. O ponto de final de v2.0 do Azure Active Directory (Azure AD) introduz uma nova versão de API de autenticação que simplifica este processo. O ponto de final de v2.0 do Azure AD permite inícios de sessão de ambos os tipos de contas através de uma integração única. Aplicações que utilizam o ponto de final de v2.0 do Azure AD também podem consumir as APIs de REST do [Microsoft Graph API](https://graph.microsoft.io) utilizando ambos os tipos de conta.

## <a name="getting-started"></a>Introdução
Escolha a sua plataforma favorita na lista seguinte para criar uma aplicação com o Microsoft abrir estruturas e bibliotecas de origem. Também pode utilizar os protocolos OAuth 2.0 e OpenID Connect para enviar e receber mensagens de protocolo diretamente, sem utilizar uma biblioteca de autenticação.
<br />

[!INCLUDE [Azure AD v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-azure-ad-v20-endpoint"></a>Saiba mais sobre o ponto de final de v2.0 do Azure AD
Saiba mais sobre o que pode fazer com o ponto de final de v2.0 do Azure AD:

* Detetar a [tipos de aplicativos que pode criar com o ponto de final de v2.0 do Azure AD](active-directory-v2-flows.md).
* Compreender os [limitações e restrições](active-directory-v2-limitations.md) com o ponto de final de v2.0 do Azure AD.
* Veja este vídeo para obter uma descrição geral do ponto final v2.0 do Azure AD:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Recursos adicionais
Explore informações aprofundadas sobre a plataforma de ponto final de v2.0 do Azure AD:

* [Referenciam de protocolos de v2.0 do Azure AD](active-directory-v2-protocols.md)
* [Referência de tokens do Azure AD v2.0](active-directory-v2-tokens.md)
* [Referenciam de bibliotecas de autenticação de v2.0 do Azure AD](active-directory-v2-libraries.md)
* [Âmbitos e consentimento no ponto de final de v2.0 do Azure AD](active-directory-v2-scopes.md)
* [O Microsoft Graph API](https://graph.microsoft.io)

> [!NOTE]
> Se tiver apenas de iniciar sessão nas contas profissionais e escolares do Azure Active Directory, comece com o [Guia do programador do Azure AD](azure-ad-developers-guide.md). O ponto de final de v2.0 do Azure AD destina-se a desenvolvedores que precisam explicitamente para iniciar sessão em contas pessoais da Microsoft.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
