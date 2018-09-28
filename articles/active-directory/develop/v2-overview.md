---
title: Sobre o v2.0 | Azure
description: Saiba mais sobre a plataforma e o ponto final do v2.0.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: adcb1b15ed52e6954846ea09be0a87a118222c10
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989589"
---
# <a name="about-v20"></a>Sobre o v2.0

A plataforma e o ponto final do v2.0 estão em pré-visualização e são continuamente melhorados. Hoje em dia, os cenários de aplicação de página única (SPA) do JavaScript incluem todas as funcionalidades e convidamo-lo a utilizar o MSAL.js para criar aplicações baseadas no browser e enviar-nos comentários, para que possamos atualizar o estado da pré-visualização para disponibilidade geral (GA).

> [!NOTE]
> A MSAL Android, iOS e .NET ainda têm funcionalidades em desenvolvimento. Pode utilizá-los para criar aplicações e enviar-nos comentários.

A experiência do programador do portal do Azure foi significativamente atualizada para incluir todas as suas aplicações criadas com a ADAL ou a MSAL, e para melhorar a usabilidade.

No passado, os programadores de aplicações que queriam suportar as contas pessoais e profissionais da Microsoft do Azure Active Directory (Azure AD) tinham de se integrar em dois sistemas separados. A plataforma e o ponto final do v2.0 oferece uma versão da API de autenticação que simplifica este processo. Permite inícios de sessão de ambos os tipos de contas, através de uma única integração. As aplicações que utilizam o ponto final do v2.0 também podem consumir as APIs REST do [Microsoft Graph API](https://graph.microsoft.io) ao utilizar ambos os tipos de contas.

## <a name="getting-started"></a>Introdução

Escolha a sua plataforma favorita na lista seguinte para criar uma aplicação com as bibliotecas e arquiteturas de código aberto da Microsoft. Também pode utilizar os protocolos OAuth 2.0 e OpenID Connect para enviar e receber mensagens de protocolo diretamente, sem utilizar uma biblioteca de autenticação.

[!INCLUDE [v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-v20-endpoint-and-platform"></a>Saiba mais sobre a plataforma e o ponto final do v2.0

Saiba mais sobre o que pode fazer com o ponto final do Azure AD v2.0:

* Detete os [tipos de aplicações que pode criar com o ponto de final do Azure AD v2.0](v2-app-types.md).
* Compreenda os [limites e restrições](active-directory-v2-limitations.md) com o ponto final do Azure AD v2.0.
* Veja este vídeo para obter uma descrição geral do ponto final do Azure AD v2.0:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Recursos adicionais

Explore informações aprofundadas sobre o v2.0:

* [Referência de protocolos do v2.0](active-directory-v2-protocols.md)
* [Referência de tokens de acesso](access-tokens.md)
* [`id_tokens` referência](id-tokens.md)
* [Referência de bibliotecas de autenticação d v2.0](reference-v2-libraries.md)
* [Âmbitos e consentimento no v2.0](v2-permissions-and-consent.md)
* [A Microsoft Graph API](https://graph.microsoft.io)

> [!NOTE]
> Se apenas tiver de iniciar sessão nas contas profissionais e escolares do Azure Active Directory, comece com o [Guia do programador do Azure AD](azure-ad-developers-guide.md). O ponto final do v2.0 destina-se a programadores que precisam explicitamente de iniciar sessão em contas pessoais da Microsoft.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
