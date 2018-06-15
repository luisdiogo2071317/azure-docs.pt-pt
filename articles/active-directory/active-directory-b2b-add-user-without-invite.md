---
title: Adicionar utilizadores de colaboração B2B do Azure Active Directory sem um convite | Microsoft Docs
description: Pode permitir que um utilizador convidado adicionar outros utilizadores convidados ao seu Azure AD sem resgatar um convite na colaboração B2B do Azure Active Directory do.
services: active-directory
documentationcenter: ''
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 5a37a5a14dcb07db7e078558072f7edad7432d9b
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075628"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Adicionar utilizadores de convidado de colaboração B2B sem um convite

> [!NOTE]
> Agora, os utilizadores convidados já não precisam do e-mail de convite, exceto em alguns casos especiais. Para obter mais informações, consulte [resgate de convite de colaboração B2B](active-directory-b2b-redemption-experience.md).  

Pode permitir que um utilizador, tal como um representante de parceiro, para adicionar utilizadores do parceiro à sua organização sem necessitar de convites para ser resgatadas. Tudo o que deve fazer é conceder privilégios de enumeração no diretório que está a utilizar para o org. de parceiro a que o utilizador 

Conceda estes privilégios quando:

1. Um utilizador na organização do anfitrião (por exemplo, o WoodGrove) invites um utilizador da organização de parceiro (por exemplo, Sam@litware.com) como convidado.
2. O administrador na organização do anfitrião [configura políticas](active-directory-b2b-delegate-invitations.md) que permitem Sam identificar e adicionar outros utilizadores da organização de parceiro (Litware).
3. Agora Sam pode adicionar outros utilizadores de Litware para o diretório de WoodGrove, grupos ou aplicações sem ser necessário convites para ser resgatadas. Se Sam tem os privilégios adequados enumeração Litware, ocorre automaticamente.

### <a name="next-steps"></a>Passos Seguintes

- [O que é a colaboração B2B do Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Como é que os infotrabalhadores adicionar utilizadores de colaboração do B2B?](active-directory-b2b-iw-add-users.md)
- [Resgate de convite de colaboração B2B](active-directory-b2b-redemption-experience.md)
- [Delegar convites para colaboração B2B do Azure Active Directory do](active-directory-b2b-delegate-invitations.md)

