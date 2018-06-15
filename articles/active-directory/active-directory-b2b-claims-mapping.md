---
title: Mapeamento no Azure Active Directory de afirmações de utilizador de colaboração do B2B | Microsoft Docs
description: Personalize as afirmações de utilizador que são emitidas no token SAML para utilizadores do B2B Azure Active Directory (Azure AD).
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/06/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: bab339a07503a042f459c0933e92e0ed4a4fb27e
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33927819"
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Afirmações de utilizador de colaboração do B2B mapeamento no Azure Active Directory

Personalizar as afirmações que são emitidas no token SAML para os utilizadores de colaboração B2B do Azure suporta o Active Directory (Azure AD). Quando um utilizador efetua a autenticação para a aplicação, o Azure AD emite um token SAML para a aplicação que contém informações (ou afirmações) sobre o utilizador que identifica exclusivamente. Por predefinição, isto inclui o nome de utilizador, endereço de correio eletrónico, nome próprio e apelido do utilizador.

No [portal do Azure](https://portal.azure.com), pode ver ou editar as afirmações que são enviadas no token SAML para a aplicação. Para aceder às definições, selecione **do Azure Active Directory** > **aplicações empresariais** > a aplicação que está configurada para o início de sessão único > **de sessão único-** . Ver as definições de token SAML no **atributos de utilizador** secção.

![Mostra os atributos de token SAML na IU](media/active-directory-b2b-claims-mapping/view-claims-in-saml-token.png)

Existem duas razões possíveis por que razão poderá ser necessário editar as afirmações que são emitidas no SAML token:

1. A aplicação requer um conjunto diferente de URIs ou valores de afirmação.

2. A aplicação requer a afirmação NameIdentifier ser algo que o nome de principal de utilizador (UPN) que é armazenado no Azure AD.

Para obter informações sobre como adicionar e editar afirmações, consulte [personalizar afirmações emitidas no token SAML para aplicações da empresa no Azure Active Directory](develop/active-directory-saml-claims-customization.md).

Para colaboração B2B utilizadores, mapeamento entre-inquilinos NameID e UPN são impedidos por motivos de segurança.

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre as propriedades de utilizador de colaboração B2B, consulte [propriedades de um utilizador de colaboração do Azure Active Directory B2B](active-directory-b2b-user-properties.md).
- Para obter informações sobre os tokens de utilizador para os utilizadores de colaboração do B2B, consulte [compreender tokens de utilizador em colaboração B2B do Azure AD](active-directory-b2b-user-token.md).

