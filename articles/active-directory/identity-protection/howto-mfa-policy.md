---
title: Como configurar a política de registo de autenticação multifator no Azure Active Directory Identity Protection | Documentos da Microsoft
description: Saiba como configurar a política de registo de multi-factor authentication do Azure AD Identity Protection.
services: active-directory
keywords: proteção de identidade do Azure Active Directory, descoberta de aplicações na cloud, gestão de aplicações, a segurança, a risco, a nível de risco, a vulnerabilidade, a política de segurança
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.component: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 792a1fc2403e672c973577efd7a05c9c81d45ad4
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47054086"
---
# <a name="how-to-configure-the-multi-factor-authentication-registration-policy"></a>Como: Configurar a política de registo de autenticação multifator

O Azure AD Identity Protection ajuda a gerir o roll-cancelar o registro de autenticação multifator (MFA) ao configurar uma política. Este artigo explica o que pode ser utilizada a política para um como configurá-lo.

## <a name="what-is-the-multi-factor-authentication-registration-policy"></a>O que é a política de registo de autenticação multifator?

Autenticação multifator do Azure é um método de verificação quem é o que requer a utilização de mais do que apenas um nome de utilizador e palavra-passe. Ele fornece uma segunda camada de segurança para inícios de sessão de utilizador e transações.  

Recomendamos que exigem multi-factor authentication para inícios de sessão de utilizador, porque ele:

- Proporciona uma autenticação segura com uma variedade de opções de verificação simples

- Desempenha um papel fundamental na preparação de sua organização para proteger e recuperar a partir de comprometimentos de conta


Para obter mais detalhes, consulte [o que é o Azure multi-factor Authentication?](../authentication/multi-factor-authentication.md)


## <a name="how-do-i-access-the-mfa-registration-policy"></a>Como posso aceder a política de registo MFA?
   
A política de registo MFA está no **configurar** secção sobre o [página do Azure AD Identity Protection](https://portal.azure.com/#blade/Microsoft_AAD_ProtectionCenter/IdentitySecurityDashboardMenuBlade/SignInPolicy).
   
![Política da MFA](./media/howto-mfa-policy/1014.png)




## <a name="policy-settings"></a>Definições da política

Ao configurar a política de risco de início de sessão, tem de definir:

- Os utilizadores e grupos que a política aplica-se a:

    ![Utilizadores e grupos](./media/howto-mfa-policy/11.png)

- O tipo de acesso que pretende ser imposto:  

    ![Utilizadores e grupos](./media/howto-mfa-policy/12.png)

- O estado da política:

    ![Impor a política](./media/howto-mfa-policy/14.png)


A caixa de diálogo de configuração de política fornece uma opção para calcular o impacto da sua configuração.

![Impacto estimado](./media/howto-mfa-policy/15.png)




## <a name="user-experience"></a>Experiência de utilizador


Para uma descrição geral da experiência do usuário relacionadas, consulte:

* [Fluxo de registo de autenticação multifator](flows.md#multi-factor-authentication-registration).  
* [Experiências de início de sessão com o Azure AD Identity Protection](flows.md).  



## <a name="next-steps"></a>Passos Seguintes

Para obter uma visão geral do Azure AD Identity Protection, consulte a [descrição geral do Azure AD Identity Protection](overview.md).
