---
title: Gerir os membros de um grupo no Azure AD | Microsoft Docs
description: Como adicionar ou remover utilizadores e dispositivos de um grupo no Azure Active Directory
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/28/2017
ms.author: lizross
ms.custom: it-pro
ms.reviewer: krbain
ms.openlocfilehash: 947b0c11aba211530e3ae25d6617079bcaf2995f
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860494"
---
# <a name="manage-group-membership-for-users-in-your-azure-active-directory-tenant"></a>Gerir a associação de grupo para utilizadores no seu inquilino do Azure Active Directory
Este artigo explica como gerir os membros de um grupo no Azure Active Directory (Azure AD).

## <a name="how-do-i-find-the-members-and-manage-them"></a>Como encontrar os membros e geri-los?
1. Iniciar sessão no [Portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global do diretório.
2. Selecione **Todos os serviços**, introduza **Utilizadores e grupos** na caixa de texto e, em seguida, selecione **Enter**.

   ![Abrir gestão de utilizadores](./media/active-directory-groups-members-azure-portal/search-user-management.png)
3. No painel **Utilizadores e grupos**, selecione **Todos os grupos**.

   ![Abrir o painel de grupos](./media/active-directory-groups-members-azure-portal/view-groups-blade.png)
4. No painel **Utilizadores e grupos - Todos os grupos**, selecione um grupo.
5. No painel **Grupo - *groupname*** , selecione **Membros**.

   ![Abrir o painel Membros](./media/active-directory-groups-members-azure-portal/view-group-members.png)
6. Para adicionar membros ao grupo, no painel **Grupo - Membros**, selecione **Adicionar Membros**.

   ![Comando Adicionar Membros](./media/active-directory-groups-members-azure-portal/add-group-members-command.png)
7. No painel **Membros**, selecione um ou mais utilizadores ou dispositivos para adicionar ao grupo e selecione o botão **Selecionar** na parte inferior do painel para adicioná-los ao grupo. A caixa **Utilizador** filtra a apresentação com base na correspondência da sua entrada a qualquer parte de um nome de utilizador ou dispositivo. Os carateres universais não são aceites nessa caixa.
8. Para remover membros do grupo, no painel **Grupo - Membros**, selecione um membro.
9. No painel ***membername***, selecione o comando **Remover** e confirme a sua escolha na linha de comandos.

   ![Comando remover Membros](./media/active-directory-groups-members-azure-portal/remove-group-members-command.png)
10. Quando terminar de alterar os membros do grupo, selecione **Guardar**.

## <a name="additional-information"></a>Informações adicionais
Estes artigos fornecem informações adicionais acerca do Azure Active Directory.

* [Ver grupos existentes](active-directory-groups-view-azure-portal.md)
* [Criar um novo grupo e adicionar membros](active-directory-groups-create-azure-portal.md)
* [Gerir definições de um grupo](active-directory-groups-settings-azure-portal.md)
* [Gerir associações de um grupo](active-directory-groups-membership-azure-portal.md)
* [Gerir regras dinâmicas dos utilizadores num grupo](../users-groups-roles/groups-dynamic-membership.md)
