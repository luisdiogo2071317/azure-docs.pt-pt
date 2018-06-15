---
title: Colaboração B2B do Active Directory Azure API e personalização | Microsoft Docs
description: A colaboração do B2B Azure Active Directory suporta as relações entre empresas, permitindo a parceiros de negócios acederem, seletivamente, às suas aplicações empresariais
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/11/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: f1bd93ac2ef6aa75e07eeec3e3cb2222b6febc1c
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
ms.locfileid: "33932375"
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Colaboração B2B do Active Directory Azure API e personalização

Iremos tenha tido muitos clientes diga-nos de que pretende personalizar o processo de convite de uma forma que funciona melhor das suas organizações. Com a nossa API, pode fazê-lo imediatamente. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>Capacidades do convite API
A API oferece as seguintes capacidades:

1. Convidar um utilizador externo com *qualquer* endereço de e-mail.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Personalize onde pretende que os utilizadores encaminhado para assim que aceitarem os respetivos convite.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Optar por enviar o correio de convite padrão através dos EUA

    ```
    "sendInvitationMessage": true
    ```

  com uma mensagem para o destinatário pode personalizar

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. E optar por cc: pessoas que pretende manter no ciclo sobre o inviting este colaborador.

5. Ou personalizar por completo o convite e fluxo de trabalho de integração ao optar por não enviar notificações através do Azure AD.

    ```
    "sendInvitationMessage": false
    ```

  Neste caso, regressar um URL de resgate partir da API de poder incorporar um modelo de correio eletrónico, MI ou outro método de distribuição da sua preferência.

6. Por fim, se for um administrador, pode optar por convidar o utilizador como membro.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Modelo de autorização
A API pode ser executada num seguintes modos de autorização:

### <a name="app--user-mode"></a>Aplicação + modo de utilizador
Neste modo, quem está a utilizar as necessidades de API para ter as permissões para ser criar convites B2B.

### <a name="app-only-mode"></a>Modo só de aplicação
No contexto de aplicação apenas a aplicação tem do âmbito de User.Invite.All para o convite com êxito.

Para obter mais informações, consulte: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Agora é possível utilizar o PowerShell para adicionar e convidar utilizadores externos para uma organização facilmente. Crie um convite utilizando o cmdlet:

```
New-AzureADMSInvitation
```

Pode utilizar as seguintes opções:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Também pode consultar a referência da API convite no [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Passos Seguintes

- [O que é a colaboração B2B do Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Os elementos do e-mail de convite de colaboração B2B](active-directory-b2b-invitation-email.md)
- [Resgate de convite de colaboração B2B](active-directory-b2b-redemption-experience.md)
- [Adicionar utilizadores de colaboração B2B sem um convite](active-directory-b2b-add-user-without-invite.md)

