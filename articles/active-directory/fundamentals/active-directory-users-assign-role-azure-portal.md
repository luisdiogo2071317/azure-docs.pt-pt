---
title: Atribua um utilizador a funções de administrador no Azure Active Directory | Microsoft Docs
description: Como alterar informações administrativas do utilizador no Azure Active Directory
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 06/25/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.openlocfilehash: 7ed564d5954841f96109568b33183908d25bb8be
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939545"
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Atribua um utilizador a funções de administrador no Azure Active Directory
Este artigo explica como atribuir uma função administrativa a um utilizador no Azure Active Directory (Azure AD). Para obter informações sobre a adição de novos utilizadores à organização, veja [Adicionar novos utilizadores ao Azure Active Directory](../add-users-azure-active-directory.md). Por predefinição, os utilizadores adicionados não têm permissões de administrador. No entanto, pode atribuir-lhes funções em qualquer altura.

## <a name="assign-a-role-to-a-user"></a>Atribuir uma função a um utilizador
1. Iniciar sessão no [portal do Azure](https://portal.azure.com) com uma conta que seja um administrador global ou administrador de função com privilégios do diretório.

2. Selecione **Azure Active Directory**, selecione **Utilizadores**e, em seguida, selecione um utilizador específico da lista.

    ![Abrir gestão de utilizadores](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3. Para o utilizador selecionado, selecione **Função de diretório**, selecione **Adicionar função** e, em seguida, escolha as funções administrativas apropriadas na lista **Funções de diretório**, como **Administrador de acesso condicional**. Para obter mais informações sobre as funções administrativas, veja [Atribuir funções de administrador no Azure AD](../active-directory-assign-admin-roles-azure-portal.md). 

    ![Atribuir um utilizador a uma função](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

1. Prima **Selecionar** para guardar.

## <a name="next-steps"></a>Passos seguintes
* [Início rápido: Adicionar ou eliminar utilizadores no Azure Active Directory](add-users-azure-active-directory.md)
* [Gerir perfis de utilizador](active-directory-users-profile-azure-portal.md)
* [Adicionar utilizadores convidados a partir de outro diretório](../b2b/what-is-b2b.md) 
* [Atribuir um utilizador a uma função no seu Azure AD](active-directory-users-assign-role-azure-portal.md)
* [Restaurar um utilizador eliminado](active-directory-users-restore.md)
