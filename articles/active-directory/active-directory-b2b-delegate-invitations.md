---
title: Delegar convites para colaboração B2B do Azure Active Directory do | Microsoft Docs
description: Propriedades de utilizador de colaboração B2B do Active Directory do Azure são configuráveis
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/23/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: a4a1303c3992b8c576650859eea65591392b9b20
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a>Delegar convites para colaboração B2B do Azure Active Directory do

Com a colaboração de empresa-empresa (B2B) do Azure Active Directory (Azure AD), tem de ser um administrador global para enviar convites para. Em vez disso, pode utilizar as políticas e delegar convites aos utilizadores cujas funções de lhes enviar convites para permitam. Uma nova forma de importante para delegar convites de utilizador convidado é através da função de convidado Inviter.

## <a name="guest-inviter-role"></a>Função de convidado Inviter
Iremos pode atribuir o utilizador à função de convidado Inviter enviar convites para. Não tem de ser membro da função de administrador global para enviar convites para. Por predefinição, os utilizadores regulares podem também invocar a API de convite a menos que um administrador global desativado convites para os utilizadores regulares. Um utilizador pode também invocar a API utilizando o portal do Azure ou o PowerShell.

Eis um exemplo que mostra como utilizar o PowerShell para adicionar um utilizador à função de convidado Inviter:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a>Controlar quem pode convidar

![Controlar como convidar](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

Com a colaboração B2B do Azure AD, um administrador de inquilino pode definir as políticas de convite seguintes:

- Desativar convites para
- Apenas administradores e utilizadores na função de convidado Inviter podem convidar
- Administradores, a função de convidado Inviter e membros podem convidar
- Todos os utilizadores, incluindo nos convidados, podem convidar

Por predefinição, os inquilinos estão definidos como #4. (Todos os utilizadores, incluindo nos convidados, podem convidar os utilizadores de B2B).

## <a name="next-steps"></a>Passos Seguintes

Consulte os artigos seguintes na colaboração B2B do Azure AD:

- [O que é a colaboração B2B do Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Adicionar utilizadores de convidado de colaboração B2B sem um convite](active-directory-b2b-add-user-without-invite.md)
- [A adição de um utilizador de colaboração B2B a uma função](active-directory-b2b-add-guest-to-role.md)


