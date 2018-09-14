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
ms.openlocfilehash: e626260dba3155ef56ee4a784aab2c6fd6897295
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45581364"
---
# <a name="how-to-configure-the-multi-factor-authentication-registration-policy"></a>Como: Configurar a política de registo de autenticação multifator

O Azure AD Identity Protection ajuda a gerir o roll-cancelar o registro de autenticação multifator (MFA) ao configurar uma política. Este artigo explica o que pode ser utilizada a política para um como configurá-lo.

## <a name="what-is-the-multi-factor-authentication-registration-policy"></a>O que é a política de registo de autenticação multifator?

Autenticação multifator do Azure é um método de verificação quem é o que requer a utilização de mais do que apenas um nome de utilizador e palavra-passe. Ele fornece uma segunda camada de segurança para inícios de sessão de utilizador e transações.  
Recomendamos que exigem multi-factor authentication para inícios de sessão de utilizador, porque ele:

* Proporciona uma autenticação segura com uma variedade de opções de verificação simples
* Desempenha um papel fundamental na preparação de sua organização para proteger e recuperar a partir de comprometimentos de conta

![Política de risco do utilizador](./media/howto-mfa-policy/1019.png "política de risco do utilizador")

Para obter mais detalhes, consulte [o que é o Azure multi-factor Authentication?](../authentication/multi-factor-authentication.md)

## <a name="configuration"></a>Configuração

**Para abrir a caixa de diálogo de configuração relacionados**:

- Sobre o **do Azure AD Identity Protection** painel, na **configurar** secção, clique em **registo de multi-factor authentication**.

    ![Política da MFA](./media/howto-mfa-policy/1019.png "política da MFA")

### <a name="settings"></a>Definições

* Defina os utilizadores e grupos que a política aplica-se a:

    ![Política da MFA](./media/howto-mfa-policy/1020.png "política da MFA")
* Defina os controlos a serem impostas quando a política aciona::  

    ![Política da MFA](./media/howto-mfa-policy/1021.png "política da MFA")
* Alterne o estado da política:

    ![Política da MFA](./media/howto-mfa-policy/403.png "política da MFA")
* Ver o estado atual do registo:

    ![Política da MFA](./media/howto-mfa-policy/1022.png "política da MFA")

Para uma descrição geral da experiência do usuário relacionadas, consulte:

* [Fluxo de registo de autenticação multifator](flows.md#multi-factor-authentication-registration).  
* [Experiências de início de sessão com o Azure AD Identity Protection](flows.md).  



## <a name="next-steps"></a>Passos Seguintes

Para obter uma visão geral do Azure AD Identity Protection, consulte a [descrição geral do Azure AD Identity Protection](overview.md).
