---
title: Tutorial - conceder acesso de utilizador utilizando o RBAC e o portal do Azure | Documentos da Microsoft
description: Utilize o controlo de acesso baseado em funções (RBAC) para conceder permissões a um utilizador ao atribuir uma função no portal do Azure.
services: role-based-access-control
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 11/30/2018
ms.author: rolyon
ms.openlocfilehash: 8caa5c3b33ac1b483429251e0c1256636c4ece1a
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634477"
---
# <a name="tutorial-grant-access-for-a-user-using-rbac-and-the-azure-portal"></a>Tutorial: Conceder acesso de utilizador utilizando o RBAC e o portal do Azure

[O controlo de acesso baseado em funções (RBAC)](overview.md) é a forma de gerir o acesso a recursos no Azure. Neste tutorial, pode conceder um acesso de utilizador para criar e gerir máquinas virtuais num grupo de recursos.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Conceder acesso para um utilizador com um âmbito de grupo de recursos
> * Remover o acesso

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

1. Na lista de navegação, selecione **Grupos de recursos**.

1. Selecione **Adicionar** para abrir o painel **Grupo de recursos**.

   ![Adicionar um novo grupo de recursos](./media/quickstart-assign-role-user-portal/resource-group.png)

1. Para **nome do grupo de recursos**, introduza **grupo de recursos de rbac**.

1. Selecione uma subscrição e uma localização.

1. Selecione **Criar** para criar o grupo de recursos.

1. Selecione **Atualizar** para atualizar a lista de grupos de recursos.

   O novo grupo de recursos é apresentado na lista de grupos de recursos.

   ![Lista de grupos de recursos](./media/quickstart-assign-role-user-portal/resource-group-list.png)

## <a name="grant-access"></a>Conceder acesso

No RBAC, para conceder acesso, crie uma atribuição de função.

1. Na lista de **grupos de recursos**, escolha a nova **grupo de recursos de rbac** grupo de recursos.

1. Escolha **Controlo de acesso (IAM)**.

1. Escolha o **atribuições de funções** guia para ver a lista atual de atribuições de funções.

   ![Painel Controlo de acesso (IAM) para grupo de recursos](./media/quickstart-assign-role-user-portal/access-control.png)

1. Escolher **adicionar atribuição de função** para abrir o painel de atribuição de função de adicionar.

   Se não tiver permissões para atribuir funções, a opção de atribuição de função de adicionar será desativada.

   ![Adicionar o painel de atribuição de função](./media/quickstart-assign-role-user-portal/add-role-assignment.png)

1. Na lista pendente **Função**, selecione **Contribuidor de Máquina Virtual**.

1. Na lista **Selecionar**, selecione-se a si mesmo ou outro utilizador.

1. Escolha **Guardar** para criar a atribuição de função.

   Após alguns instantes, o utilizador tem atribuída a função de contribuinte de Máquina Virtual no âmbito do grupo de recursos do grupo de recursos de rbac.

   ![Atribuição da função Contribuidor de Máquina Virtual](./media/quickstart-assign-role-user-portal/vm-contributor-assignment.png)

## <a name="remove-access"></a>Remover o acesso

No RBAC, para remover o acesso, remova uma atribuição de função.

1. Na lista de atribuições de função, adicione uma marca de verificação junto ao utilizador com a função de contribuinte de Máquina Virtual.

1. Escolha **Remover**.

   ![Mensagem Remover atribuição de função](./media/quickstart-assign-role-user-portal/remove-role-assignment.png)

1. Na mensagem para remover atribuição de função que é apresentada, escolha **Sim**.

## <a name="clean-up"></a>Limpeza

1. Na lista de navegação, selecione **Grupos de recursos**.

1. Escolher **grupo de recursos de rbac** para abrir o grupo de recursos.

1. Selecione **Eliminar grupo de recursos** para eliminar o grupo de recursos.

   ![Eliminar grupo de recursos](./media/quickstart-assign-role-user-portal/delete-resource-group.png)

1. Sobre o **tem a certeza de que pretende eliminar** painel, escreva o nome do grupo de recursos: **grupo de recursos de rbac**.

1. Selecione **Eliminar** para eliminar o grupo de recursos.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial: Conceder acesso a um utilizador com o RBAC e o PowerShell](tutorial-role-assignments-user-powershell.md)

