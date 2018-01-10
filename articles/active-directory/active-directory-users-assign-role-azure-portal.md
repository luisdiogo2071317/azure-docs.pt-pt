---
title: "Atribuir um utilizador a funções de administrador no Azure Active Directory | Microsoft Docs"
description: "Explica como alterar as informações administrativas do utilizador no Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: a1ca1a53-50d8-4bf0-ae8f-73fa1253e2d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.openlocfilehash: dcb52e9de98d881474007410f3db599682e151ce
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="assign-a-user-to-administrator-roles-in-azure-active-directory"></a>Atribuir um utilizador a funções de administrador no Azure Active Directory
Este artigo explica como atribuir uma função administrativa para um utilizador no Azure Active Directory (Azure AD). Para obter informações sobre como adicionar novos utilizadores na sua organização, consulte [adicionar novos utilizadores ao Azure Active Directory](active-directory-users-create-azure-portal.md). Por predefinição, os utilizadores adicionados não têm permissões de administrador. No entanto, pode atribuir-lhes funções em qualquer altura.

## <a name="assign-a-role-to-a-user"></a>Atribuir uma função a um utilizador
1. Inicie sessão no [Centro de administradores do Azure AD](https://aad.portal.azure.com) com uma conta que seja administrador global do diretório.
2. Selecione **utilizadores e grupos**.

   ![Gestão de utilizadores de abertura](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)
3. Selecione **todos os utilizadores**.
  
  ![Abrir todos os grupo de utilizadores](./media/active-directory-users-assign-role-azure-portal/create-users-open-users-blade.png)
4. Selecione um utilizador da lista.
5. Para o utilizador selecionado, selecione **função de diretório** e, em seguida, atribua o utilizador a uma função do **função de diretório** lista. Para obter mais informações sobre as funções de utilizador e administrador, consulte [Atribuir funções de administrador no Azure AD](active-directory-assign-admin-roles-azure-portal.md).

      ![Atribuição de um utilizador a uma função](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)
6. Selecione **Guardar**.

## <a name="next-steps"></a>Passos Seguintes
* [Guia de introdução: Adicionar ou eliminar os utilizadores no Azure Active Directory](add-users-azure-active-directory.md)
* [Gerir perfis de utilizador](active-directory-users-profile-azure-portal.md)
* [Adicionar utilizadores convidados a partir de outro diretório](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Atribuir um utilizador a uma função no seu Azure AD](active-directory-users-assign-role-azure-portal.md)
* [Restaurar um utilizador eliminado](active-directory-users-restore.md)
