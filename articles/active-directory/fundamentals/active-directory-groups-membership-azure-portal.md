---
title: Gira os grupos a que o seu grupo pertence no Azure AD | Microsoft Docs
description: Os grupos podem conter outros grupos no Azure Active Directory. Eis como gerir essas associações.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 8a71677ae3ceb5617f0a817a8eff438d5e3f2774
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860446"
---
# <a name="manage-to-which-groups-a-group-belongs-in-your-azure-active-directory-tenant"></a>Gira a que grupos um grupo pertence no seu inquilino do Azure Active Directory
Os grupos podem conter outros grupos no Azure Active Directory. Eis como gerir essas associações.

## <a name="how-do-i-find-the-groups-of-which-my-group-is-a-member"></a>Como encontro os grupos em que o meu grupo é um membro?
1. Inicie sessão no [Centro de administradores do Azure AD](https://aad.portal.azure.com) com uma conta que seja administrador global do diretório.
2. Selecionar **Utilizadores e grupos**.

   ![Abrir imagem de utilizadores e grupos](./media/active-directory-groups-membership-azure-portal/search-user-management.png)
1. Selecione **Todos os grupos**.

   ![Selecionar imagem de grupos](./media/active-directory-groups-membership-azure-portal/view-groups-blade.png)
1. Selecione um grupo.
2. Selecione **Associações a grupos**.

   ![Abrir imagem de associações a grupos](./media/active-directory-groups-membership-azure-portal/group-membership-blade.png)
1. Para adicionar o seu grupo como membro de outro grupo, no painel **Grupo - Associações a grupos**, selecione o comando **Adicionar**.
2. Selecione um grupo no painel **Selecionar Grupo** e, em seguida, selecione o botão **Selecionar** na parte inferior do painel. Pode adicionar o grupo a apenas um grupo de cada vez. A caixa **Utilizador** filtra a apresentação com base na correspondência da sua entrada a qualquer parte de um nome de utilizador ou dispositivo. Os carateres universais não são aceites nessa caixa.

   ![Adicionar uma associação de grupo](./media/active-directory-groups-membership-azure-portal/add-group-membership.png)
8. Para remover o seu grupo como membro de outro grupo, no painel **Grupo - Associações a grupos**, selecione um grupo.
9. Selecione o comando **Remover** e confirme a sua escolha na linha de comandos.

   ![remover comando da associação](./media/active-directory-groups-membership-azure-portal/remove-group-membership.png)
10. Quando terminar de alterar as associações a grupos para o seu grupo, selecione **Guardar**.

## <a name="additional-information"></a>Informações adicionais
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Ver grupos existentes](active-directory-groups-view-azure-portal.md)
* [Criar um novo grupo e adicionar membros](active-directory-groups-create-azure-portal.md)
* [Gerir definições de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerir membros de um grupo](active-directory-groups-members-azure-portal.md)
* [Gerir regras dinâmicas dos utilizadores num grupo](../users-groups-roles/groups-dynamic-membership.md)
