---
title: No Azure Active Directory de reposição de palavra-passe | Documentos da Microsoft
description: Para um utilizador no Azure Active Directory de reposição de palavra-passe de iniciada pelo administrador
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
editor: ''
ms.assetid: fad5624b-2f13-4abc-b3d4-b347903a8f16
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: joflore
ms.reviewer: sahenry
ms.custom: it-pro
ms.openlocfilehash: 689ab264e56bc8559db6237eee19566e9e3c429f
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35648442"
---
# <a name="reset-the-password-for-a-user-in-azure-active-directory"></a>Repor a palavra-passe para um utilizador no Azure Active Directory

Os administradores podem precisar de repor a palavra-passe de um utilizador em casos em que eles se esqueceu, eram forçadas out ou outros cenários. Os passos seguintes guiá-lo a repor a palavra-passe de um utilizador.

## <a name="how-to-reset-the-password-for-a-user"></a>Como repor a palavra-passe para um utilizador

1. Inicie sessão para o [Centro de administração do Azure AD](https://aad.portal.azure.com) com uma conta que tenha permissões de diretório para repor palavras-passe do utilizador.
2. Selecione **do Azure Active Directory** > **utilizadores e grupos** > **todos os utilizadores**.
3. Selecione o utilizador que pretende repor a palavra-passe.
2. Para o utilizador selecionado, selecione **Repor palavra-passe**.

    ![Repor uma palavra-passe de perfil do usuário, um utilizador no Azure AD](./media/active-directory-users-reset-password-azure-portal/user-password-reset.png)
    
6. No **Repor palavra-passe**, selecione **Repor palavra-passe**.
7. Uma palavra-passe temporária é apresentada a que, em seguida, pode fornecer ao usuário. O usuário será solicitado para, em seguida, alterar a palavra-passe da próxima vez que iniciar sessão. 

   > [!NOTE]
   > Esta palavra-passe temporária não têm um prazo de expiração para que ele será válido até que iniciarem sessão e são, em seguida, são forçados a alterá-la. 

## <a name="next-steps"></a>Passos Seguintes
* [Adicionar um utilizador](../add-users-azure-active-directory.md)
* [Atribuir funções de administrador a um utilizador](active-directory-users-assign-role-azure-portal.md)
* [Gerir perfis de utilizador](active-directory-users-profile-azure-portal.md)
* [Eliminar um utilizador no Azure AD](../add-users-azure-active-directory.md)
