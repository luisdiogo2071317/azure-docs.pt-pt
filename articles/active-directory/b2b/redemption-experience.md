---
title: Resgate convite na colaboração B2B - Azure Active Directory | Microsoft Docs
description: Descreve a experiência de resgate do convite de colaboração B2B do Azure AD para utilizadores finais, incluindo o contrato de termos de privacidade.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 05/11/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 0a0c900bbfbb2778d8fabcbb71e339fd3dabcf47
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260175"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Resgate de convite de colaboração B2B do Active Directory do Azure

Para colaborar com os utilizadores de organizações de parceiro através de colaboração B2B do Azure Active Directory (Azure AD), pode convidar os utilizadores convidados para aceder às aplicações partilhadas. Depois de um utilizador convidado é adicionado ao diretório através da interface de utilizador ou o utilizador está convidado através do PowerShell, os utilizadores convidados tem de passar por um processo de consentimento de primeira hora em que estão a concordar com [termos de privacidade](#privacy-policy-agreement). Este processo ocorre em qualquer uma das seguintes formas:

- Envia o inviter convidado terminar uma ligação direta para uma aplicação partilhada. O invitee clica na ligação para iniciar sessão, aceita os termos de privacidade e acede perfeitamente o recurso partilhado. (O utilizador de convidados ainda recebe um e-mail de convite com um URL de resgate, mas que não sejam alguns casos especiais, tem já não é necessária para utilizar o e-mail de convite).  
- O utilizador convidado recebe um e-mail de convite e clica no URL de resgate. Como parte do tempo de primeiro início de sessão, são-lhe pedido que aceite os termos de privacidade.

## <a name="redemption-through-a-direct-link"></a>Resgate através de uma ligação direta

Um inviter convidado pode convidar um utilizador convidado através do envio de uma ligação direta para uma aplicação partilhada. Para o utilizador convidado, a experiência de resgate é tão fácil como o início de sessão na aplicação que foi partilhada com os mesmos. Pode clicar numa ligação para a aplicação, reveja e aceite os termos de privacidade e, em seguida, aceder perfeitamente a aplicação. Na maioria dos casos, os utilizadores convidados já não é necessitam clicar num URL de resgate no e-mail de convite.

Se convidou utilizadores convidados através da interface de utilizador ou optar por enviar o e-mail de convite como parte da experiência de convite de PowerShell, o utilizador convidado ainda recebe um e-mail de convite. Este e-mail é útil para os seguintes casos especiais:

- O utilizador não tem uma conta do Azure AD ou uma conta Microsoft (MSA). Neste caso, o utilizador tem de criar uma MSA antes de clicar na ligação ou podem utilizar o URL de resgate no e-mail de convite. O processo de resgate automaticamente solicita ao utilizador para criar uma MSA.
- Por vezes, o objeto de utilizador convidados não pode ter um endereço de e-mail devido a um conflito com um objeto de contacto (por exemplo, um Outlook contacto objeto). Neste caso, o utilizador tem de clicar no URL de resgate no e-mail de convite.
- O utilizador pode iniciar sessão com um alias do endereço de e-mail que foi convidado. (O alias é um endereço de e-mail adicionais associado a uma conta de e-mail). Neste caso, o utilizador tem de clicar no URL de resgate no e-mail de convite.

Se estes casos especiais são importantes para a sua organização, recomendamos que convidar os utilizadores através de métodos ainda enviar o e-mail de convite. Além disso, se um utilizador não abrangidos por uma nestes casos especiais, pode ainda clicarem o URL de um e-mail de convite de obter acesso.

## <a name="redemption-through-the-invitation-email"></a>Resgate através do e-mail de convite

Se convidado através de um método que envia um e-mail de convite, os utilizadores também podem resgatar um convite através do e-mail de convite. Um utilizador de convidados pode clique no URL de resgate no e-mail e, em seguida, reveja e aceite os termos de privacidade. O processo é descrito aqui mais detalhadamente:

1.  Após a ser convidado, o invitee recebe um convite através de correio eletrónico que é enviado a partir de **Microsoft Invitations**.
2.  Seleciona o invitee **começar** no e-mail.
3.  Se o invitee não tiver uma conta do Azure AD ou uma MSA, são-lhe pedido que crie uma MSA.
4.  O invitee é redirecionado para o **rever permissões** ecrã, onde pode consultar a declaração de privacidade da organização convidando e aceite os termos.

## <a name="privacy-policy-agreement"></a>Contrato de política de privacidade

Depois de qualquer utilizador convidado inicia sessão aceder a recursos numa organização parceira pela primeira vez, este vê uma **rever permissões** ecrã. Aqui, podem rever a declaração de privacidade da organização convidando. Um utilizador tem de aceitar a utilização das respetivas informações na accordance para as políticas de privacidade da organização convidando para continuar.

![Captura de ecrã que mostra as definições de utilizador no painel de acesso](media/redemption-experience/ConsentScreen.png) 

Para obter informações sobre como como um administrador inquilino pode ligar a declaração de privacidade da sua organização, consulte [procedimentos: adicionar informações de privacidade da sua organização no Azure Active Directory](https://aka.ms/adprivacystatement).

## <a name="next-steps"></a>Passos Seguintes

- [O que é a colaboração B2B do Azure AD?](what-is-b2b.md)
- [Adicionar utilizadores de colaboração do B2B de diretório Active Directory do Azure no portal do Azure](add-users-administrator.md)
- [Como técnicos de informação adicionar utilizadores de colaboração B2B do Azure Active Directory?](add-users-information-worker.md)
- [Adicionar utilizadores de colaboração do B2B de diretório Active Directory do Azure utilizando o PowerShell](customize-invitation-api.md#powershell)
- [Sair de uma organização como um utilizador convidado](leave-the-organization.md)