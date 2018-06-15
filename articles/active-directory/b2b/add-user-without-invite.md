---
title: Adicionar utilizadores de colaboração B2B do Azure Active Directory sem um convite | Microsoft Docs
description: Pode permitir que um utilizador convidado adicionar outros utilizadores convidados ao seu Azure AD sem resgatar um convite na colaboração B2B do Azure Active Directory do.
services: active-directory
documentationcenter: ''
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/21/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 50c64386f1eab07c299112617283b1d8d7295295
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591056"
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a>Adicionar utilizadores de convidado de colaboração B2B sem um convite

Agora pode convidar os utilizadores convidados através do envio de uma ligação direta para uma aplicação partilhada. Com este método, os utilizadores convidados já não precisam de utilizar o e-mail de convite, exceto em alguns casos especiais. Um utilizador convidado clica na ligação de aplicação, revê e aceita os termos de privacidade e, em seguida, acede totalmente integrada a aplicação. Para obter mais informações, consulte [resgate de convite de colaboração B2B](redemption-experience.md).   

Antes deste novo método estava disponível, pode convidar os utilizadores convidados sem necessidade de e-mail de convite adicionando um inviter (da sua organização ou de uma organização de parceiro) para o **inviter convidado** função de diretório e, em seguida, ter inviter adicionar utilizadores de convidado para o diretório, grupos ou aplicações através da IU ou através do PowerShell. (Se utilizar o PowerShell, pode suprimir o e-mail de convite completamente). Por exemplo:

1. Um utilizador na organização do anfitrião (por exemplo, o WoodGrove) invites um utilizador da organização de parceiro (por exemplo, Sam@litware.com) como convidado.
2. O administrador na organização do anfitrião [configura políticas](delegate-invitations.md) que permitem Sam identificar e adicionar outros utilizadores da organização de parceiro (Litware). (Sam tem de ser adicionado para o **inviter convidado** função.)
3. Agora, Sam pode adicionar outros utilizadores de Litware para o diretório de WoodGrove, grupos ou aplicações sem ser necessário convites para ser resgatadas. Se Sam tem os privilégios adequados enumeração Litware, ocorre automaticamente.
 
Este método original ainda funciona. No entanto, há uma pequena diferença no comportamento. Se utilizar o PowerShell, verá que agora tem uma conta de convidado convidados um **PendingAcceptance** Estado em vez de mostrar imediatamente **aceites**. Embora o estado seja pendente, o utilizador convidado ainda pode iniciar sessão e aceder à aplicação sem clicar numa ligação de convite de e-mail. O estado pendente significa que o utilizador não ainda tornou-se de [consentimento experiência](redemption-experience.md#privacy-policy-agreement), em que aceitem os termos de privacidade da organização convidando. O utilizador convidado vê este ecrã de consentimento quando iniciam sessão pela primeira vez. 

Se convidar um utilizador ao diretório, o utilizador convidado tem de aceder ao recurso inquilino específico do portal do Azure diretamente o URL (tais como https://portal.azure.com/ *resourcetenant*. c o m) para ver e aceitar os termos de privacidade.

### <a name="next-steps"></a>Passos Seguintes

- [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
- [Resgate de convite de colaboração B2B](redemption-experience.md)
- [Delegar convites para colaboração B2B do Azure Active Directory do](delegate-invitations.md)
- [Como é que os infotrabalhadores adicionar utilizadores de colaboração do B2B?](add-users-information-worker.md)

