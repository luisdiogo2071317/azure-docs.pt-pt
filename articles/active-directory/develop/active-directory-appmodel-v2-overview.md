---
title: Ponto final de v 2.0 do Azure Active Directory | Microsoft Docs
description: "Uma introdução à criação de aplicações com o Azure Active Directory e Account Microsoft início de sessão."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 446e39f57448cbc0cd9a6f93a388159d7eca5863
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2017
---
# <a name="sign-in-microsoft-account-and-azure-active-directory-users-in-a-single-application"></a>Inicie sessão no Azure Active Directory e Account Microsoft a utilizadores numa única aplicação
No passado, os programadores de aplicações que pretendiam para suportar ambas as contas pessoais da Microsoft e contas do Azure Active Directory profissionais tinham que integrar com dois sistemas separados. O ponto de final de v 2.0 do Azure Active Directory (Azure AD) apresenta uma nova versão de API de autenticação que simplifica este processo. Ponto final v 2.0 do Azure AD permite início de sessão a partir de ambos os tipos de contas através de uma única integração. As aplicações que utilizam o ponto final v 2.0 do Azure AD também podem consumir as APIs REST do [Microsoft Graph API](https://graph.microsoft.io) através da utilização de qualquer tipo de conta.

## <a name="getting-started"></a>Introdução
Escolha a sua plataforma favorita na lista seguinte para criar uma aplicação utilizando o Microsoft abrir estruturas e bibliotecas de origem. Também pode utilizar os protocolos de OAuth 2.0 e o OpenID Connect para enviar e receber mensagens de protocolo diretamente, sem utilizar uma biblioteca de autenticação.
<br />

[!INCLUDE [Azure AD v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-azure-ad-v20-endpoint"></a>Saiba mais sobre o ponto final v 2.0 do Azure AD
Saiba mais sobre o que pode fazer com o ponto de final de v 2.0 do Azure AD:

* Detetar o [tipos de aplicações que podem ser criados com o ponto de final de v 2.0 do Azure AD](active-directory-v2-flows.md).
* Compreender o [limitações e restrições](active-directory-v2-limitations.md) com o ponto de final de v 2.0 do Azure AD.
* Veja este vídeo para uma descrição geral do ponto final v 2.0 do Azure AD:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Recursos adicionais
Explore informações aprofundadas sobre a plataforma de ponto final de v 2.0 do Azure AD:

* [Referenciam de protocolos de v 2.0 do Azure AD](active-directory-v2-protocols.md)
* [Referência de tokens do Azure AD v 2.0](active-directory-v2-tokens.md)
* [Referenciam a bibliotecas de autenticação de v 2.0 do Azure AD](active-directory-v2-libraries.md)
* [Âmbitos e consentimento no ponto final v 2.0 do Azure AD](active-directory-v2-scopes.md)
* [O Microsoft Graph API](https://graph.microsoft.io)

> [!NOTE]
> Se só precisa de iniciar sessão em contas profissionais e escolares do Azure Active Directory, começar a utilizar o [guia para programadores do Azure AD](active-directory-developers-guide.md). Ponto final v 2.0 do Azure AD destina-se a programadores que necessitem explicitamente para iniciar sessão em contas pessoais da Microsoft.

[!INCLUDE  [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
