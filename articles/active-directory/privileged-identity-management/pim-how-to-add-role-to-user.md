---
title: Atribuir funções de diretório do Azure AD no PIM | Documentos da Microsoft
description: Saiba como atribuir funções de diretório do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: pim
ms.date: 07/23/2018
ms.author: rolyon
ms.openlocfilehash: 33bfe28bf612c47c9f42345dabccc017337c3d45
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190161"
---
# <a name="assign-azure-ad-directory-roles-in-pim"></a>Atribuir funções de diretório do Azure AD no PIM

Com o Azure Active Directory (Azure AD), pode tornar um Administrador Global **permanente** atribuições de funções de diretório. Estas atribuições de função podem ser criadas com o [portal do Azure](../users-groups-roles/directory-assign-admin-roles.md) ou a utilizar [comandos do PowerShell](/powershell/module/azuread#directory_roles).

O serviço do Azure AD Privileged Identity Management (PIM) também permite que os administradores de função com privilégios tornar as atribuições de funções de diretório permanente. Além disso, os administradores de função com privilégios podem tornar os usuários **elegíveis** para funções de diretório. Um administrador elegível pode ativar a função quando surge a necessidade e, em seguida, as respetivas permissões expirarem assim que estiverem concluídos. Para obter informações sobre as funções que pode gerir com o PIM, consulte [funções de diretório do Azure AD pode gerir no PIM](pim-roles.md).

## <a name="make-a-user-eligible-for-a-role"></a>Tornar um utilizador elegível para uma função

Siga estes passos para tornar um utilizador elegível para uma função de diretório do Azure AD.

1. Inicie sessão no [portal do Azure](https://portal.azure.com/) com um utilizador que seja um membro a [administrador com função privilegiada](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator) função.

    Para obter informações sobre como conceder outro acesso de administrador para gerir o PIM, consulte [conceder acesso a outros administradores para gerir o PIM](pim-how-to-give-access-to-pim.md).

1. Open **do Azure AD Privileged Identity Management**.

    Se ainda não começou PIM no portal do Azure, aceda a [começar a utilizar o PIM](pim-getting-started.md).

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **funções** ou **membros**.

    ![Funções do diretório do Azure AD](./media/pim-how-to-add-role-to-user/pim-directory-roles.png)

1. Clique em **Add member** abrir Adicionar geridos membros.

1. Clique em **selecionar uma função**, clique numa função que pretende gerir e, em seguida, clique em **selecione**.

    ![Selecionar uma função](./media/pim-how-to-add-role-to-user/pim-select-a-role.png)

1. Clique em **selecionar membros**, selecione os utilizadores que pretende atribuir à função e, em seguida, clique em **selecione**.

    ![Selecionar uma função](./media/pim-how-to-add-role-to-user/pim-select-members.png)

1. Em Adicionar membros geridos, clique em **OK** para adicionar o utilizador à função.

1. Na lista de funções, clique na função atribuída apenas para ver a lista de membros.

     Quando a função é atribuída, o utilizador selecionou irá aparecer na lista de membros como **elegíveis** para a função.

    ![Utilizador elegível para uma função](./media/pim-how-to-add-role-to-user/pim-directory-role-eligible.png)

1. Agora que o utilizador é elegível para a função, informá-los de que eles podem fazê-lo, de acordo com as instruções em [ativar as minhas funções de diretório do Azure AD no PIM](pim-how-to-activate-role.md).

    Os administradores elegíveis são-lhe pedidos para se registrar para o Azure multi-factor Authentication (MFA) durante a ativação. Se um utilizador não é possível registar para MFA ou está a utilizar uma conta Microsoft (normalmente @outlook.com), precisa para torná-los permanente em todas as suas funções.

## <a name="make-a-role-assignment-permanent"></a>Tornar uma atribuição de função permanente

Por predefinição, os novos utilizadores só são elegíveis para uma função de diretório. Siga estes passos, se pretender disponibilizar uma atribuição de função permanente.

1. Open **do Azure AD Privileged Identity Management**.

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **membros**.

    ![Lista de membros](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Clique num **elegíveis** função que pretende tornar permanente.

1. Clique em **mais** e, em seguida, clique em **permanente de tornar**.

    ![Tornar permanente a atribuição de função](./media/pim-how-to-add-role-to-user/pim-make-perm.png)

    A função é agora listada como **permanente**.

    ![Lista de membros com alteração permanente](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members-permanent.png)

## <a name="remove-a-user-from-a-role"></a>Remover um utilizador de uma função

Pode remover os utilizadores de atribuições de funções, mas certifique-se sempre existe pelo menos um utilizador que seja um Administrador Global permanente. Se não tem a certeza de que os utilizadores ainda precisam de suas atribuições de funções, pode [iniciar uma revisão de acesso para a função](pim-how-to-start-security-review.md).

Siga estes passos para remover um utilizador específico de uma função de diretório.

1. Open **do Azure AD Privileged Identity Management**.

1. Clique em **funções de diretório do Azure AD**.

1. Clique em **membros**.

    ![Lista de membros](./media/pim-how-to-add-role-to-user/pim-directory-role-list-members.png)

1. Clique numa atribuição de função que pretende remover.

1. Clique em **mais** e, em seguida, clique em **remover**.

    ![Remover uma função](./media/pim-how-to-add-role-to-user/pim-remove-role.png)

1. Na mensagem que lhe pede para confirmar, clique em **Sim**.

    ![Remover uma função](./media/pim-how-to-add-role-to-user/pim-remove-role-confirm.png)

    A atribuição de função é removida.

## <a name="next-steps"></a>Passos Seguintes

- [Configurar definições de função de diretório do Azure AD no PIM](pim-how-to-change-default-settings.md)
- [Atribuir funções de recursos do Azure no PIM](pim-resource-roles-assign-roles.md)
