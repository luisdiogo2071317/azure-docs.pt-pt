---
title: 'Guia de Início Rápido: Conceder acesso a um utilizador com o RBAC e o Portal do Azure | Microsoft Docs'
description: Utilize o controlo de acesso baseado em funções (RBAC) para conceder permissões a um utilizador ao atribuir uma função no portal do Azure.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 06/11/2018
ms.author: rolyon
ms.openlocfilehash: 74ecca671409b6e163bc0db29d66167d240b645c
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47092527"
---
# <a name="quickstart-grant-access-for-a-user-using-rbac-and-the-azure-portal"></a>Guia de Início Rápido: Conceder acesso a um utilizador com o RBAC e o Portal do Azure

O controlo de acesso baseado em funções (RBAC) é a forma de gerir o acesso a recursos no Azure. Neste guia de início rápido, irá conceder a um utilizador acesso para criar e gerir máquinas virtuais num grupo de recursos.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

1. Na lista de navegação, selecione **Grupos de recursos**.

1. Selecione **Adicionar** para abrir o painel **Grupo de recursos**.

   ![Adicionar um novo grupo de recursos](./media/quickstart-assign-role-user-portal/resource-group.png)

1. Para **Nome do grupo de recursos**, introduza **rbac-quickstart-resource-group**.

1. Selecione uma subscrição e uma localização.

1. Selecione **Criar** para criar o grupo de recursos.

1. Selecione **Atualizar** para atualizar a lista de grupos de recursos.

   O novo grupo de recursos é apresentado na lista de grupos de recursos.

   ![Lista de grupos de recursos](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Conceder acesso

No RBAC, para conceder acesso, crie uma atribuição de função.

1. Na lista de **Grupos de recursos**, selecione o novo grupo de recursos **rbac-quickstart-resource-group**.

1. Selecione **Controlo de acesso (IAM)** para ver a lista atual de atribuições de funções.

   ![Painel Controlo de acesso (IAM) para grupo de recursos](./media/quickstart-assign-role-user-portal/access-control.png)

1. Escolha **Adicionar** para abrir o painel **Adicionar permissões**.

   Se não tiver permissões para atribuir funções, não verá a opção **Adicionar**.

   ![Painel Adicionar permissões](./media/quickstart-assign-role-user-portal/add-permissions.png)

1. Na lista pendente **Função**, selecione **Contribuidor de Máquina Virtual**.

1. Na lista **Selecionar**, selecione-se a si mesmo ou outro utilizador.

1. Escolha **Guardar** para criar a atribuição de função.

   Após alguns instantes, o utilizador é atribuído à função Contribuidor de Máquina Virtual no âmbito do grupo de recursos rbac-quickstart-resource-group.

   ![Atribuição da função Contribuidor de Máquina Virtual](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Remover o acesso

No RBAC, para remover o acesso, remova uma atribuição de função.

1. Na lista de atribuições de funções, adicione uma marca de verificação junto ao utilizador com a função Contribuidor de Máquina Virtual.

1. Escolha **Remover**.

   ![Mensagem Remover atribuição de função](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Na mensagem para remover atribuição de função que é apresentada, escolha **Sim**.

## <a name="clean-up"></a>Limpeza

1. Na lista de navegação, selecione **Grupos de recursos**.

1. Selecione **rbac-quickstart-resource-group** para abrir o grupo de recursos.

1. Selecione **Eliminar grupo de recursos** para eliminar o grupo de recursos.

   ![Eliminar grupo de recursos](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. No painel **Tem a certeza de que quer eliminar**, escreva o nome do grupo de recursos: **rbac-quickstart-resource-group**.

1. Selecione **Eliminar** para eliminar o grupo de recursos.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Tutorial: Conceder acesso a um utilizador com o RBAC e o PowerShell](tutorial-role-assignments-user-powershell.md)

