---
title: Como gerir contas de utilizador na API Management do Azure | Microsoft Docs
description: Saiba como criar ou convidar os utilizadores na API Management do Azure
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 53ed914a3890aab1dd49536856feeb8975986e00
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Como gerir contas de utilizador na API Management do Azure
Na API Management, os programadores são os utilizadores com as APIs que expõe a utilizar a API Management. Este guia mostra a como criar e convidar programadores para utilizar as APIs e os produtos que disponibiliza aos mesmos com a instância de API Management. Para informações sobre como gerir contas de utilizador através de programação, consulte o [entidade utilizador](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentação o [API REST de gestão](https://msdn.microsoft.com/library/azure/dn776326.aspx) referência.

## <a name="prerequisites"></a>Pré-requisitos

Concluir tarefas neste artigo: [criar uma instância de API Management do Azure](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-developer"> </a>Criar um novo Programador

Para adicionar um novo utilizador, siga os passos nesta secção:

1. Selecione o **utilizadores** separador à esquerda do ecrã.
2. Prima **+ adicionar**.
3. Introduza as informações adequadas para o utilizador.
4. Prima **Adicionar**.

    ![Adicionar um novo utilizador](./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png)

Por predefinição, são contas de programador recém-criado **Active Directory**e associados a **programadores** grupo. Contas de programador, que estão num **Active Directory** Estado pode ser utilizado para aceder a todas as APIs para o qual têm subscrições. Para associar o programador recentemente criado grupos adicionais, consulte [como associar grupos a programadores][How to associate groups with developers].

## <a name="invite-developer"> </a>Convidar um programador
Para convidar um programador, siga os passos nesta secção:

1. Selecione o **utilizadores** separador à esquerda do ecrã.
2. Prima **+ convidar**.

É apresentada uma mensagem de confirmação, mas o programador recentemente convidado não aparecer na lista de até depois de serem aceitarem o convite. 

Quando um programador está convidado, é enviado um e-mail para o programador. Este e-mail é gerado utilizando um modelo e é personalizável. Para obter mais informações, consulte [configurar modelos de e-mail][Configure email templates].

Depois do convite é aceite, a conta fica ativa.

## <a name="block-developer"> </a> Desativar ou reativar uma conta de programador

Por predefinição, são contas de programador recentemente criados ou convidados **Active Directory**. Para desativar uma conta de programador, clique em **bloco**. Para reativar uma conta de programador bloqueadas, clique em **ativar**. Uma conta de programador bloqueadas não é possível aceder ao portal de programador ou chamar as APIs. Para eliminar uma conta de utilizador, clique em **eliminar**.

Para bloquear um utilizador, siga os passos seguintes.

1. Selecione o **utilizadores** separador à esquerda do ecrã.
2. Clique no utilizador que pretende bloquear.
3. Prima **bloco**.

## <a name="reset-a-user-password"></a>Repor uma palavra-passe do utilizador

Para trabalhar programaticamente com contas de utilizador, consulte o [entidade utilizador](https://msdn.microsoft.com/library/azure/dn776330.aspx) documentação o [API REST de gestão](https://msdn.microsoft.com/library/azure/dn776326.aspx) referência. Para repor uma palavra-passe da conta de utilizador para um valor específico, pode utilizar o [atualizar um utilizador](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) operação e especifique a palavra-passe pretendida.

## <a name="next-steps"> </a>Passos seguintes
Quando é criada uma conta de programador, pode associá-lo com as funções e subscrever os produtos e APIs. Para obter mais informações, consulte [como criar e utilizar grupos][How to create and use groups].

[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png


[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[Configure email templates]: api-management-howto-configure-notifications.md#email-templates
