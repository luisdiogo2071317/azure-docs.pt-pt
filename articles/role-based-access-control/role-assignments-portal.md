---
title: Gerir o acesso através do RBAC e portal do Azure | Microsoft Docs
description: Saiba como gerir o acesso de utilizadores, grupos e aplicações, utilizando o controlo de acesso baseado em funções (RBAC) e o portal do Azure. Isto inclui como listar, conceder e remover acesso.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/05/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 14ac23e6b69302ac412aac3ecab06345e5d722fd
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296351"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>Gerir o acesso através do RBAC e portal do Azure

[O controlo de acesso baseado em funções (RBAC)](overview.md) é a forma de gerir o acesso a recursos no Azure. Este artigo descreve como gerir o acesso de utilizadores, grupos e aplicações através do RBAC e portal do Azure.

## <a name="list-roles"></a>Listar funções

Uma definição de função é uma coleção de permissões que é utilizada para atribuições de funções. O Azure tem mais de 70 [funções incorporadas](built-in-roles.md). Siga estes passos para uma lista das funções no portal.

1. No portal do Azure, escolha **Todos os serviços** e, em seguida, **Subscrições**.

1. Escolha a sua subscrição.

1. Escolha **Controlo de acesso (IAM)**.

   ![Opção Funções](./media/role-assignments-portal/list-subscription-access-control.png)

1. Escolha **Funções** para ver uma lista de todas as funções incorporadas e personalizadas.

   ![Opção Funções](./media/role-assignments-portal/roles-option.png)

   Pode ver o número de utilizadores e grupos que estão atribuídos a cada função.

   ![Lista de funções](./media/role-assignments-portal/roles-list.png)

## <a name="list-access"></a>Listar o acesso

Ao gerir o acesso, pretende saber quem tem acesso, quais são as suas permissões e em qual nível. Para listar o acesso, liste as atribuições de funções. Siga estes passos para listar o acesso dos utilizadores e lista o acesso em âmbitos diferentes.

### <a name="list-role-assignments-for-a-user"></a>Listar atribuições de funções para um utilizador

1. Na lista de navegação, escolha **Azure Active Directory**.

1. Escolha **Utilizadores** para abrir **Todos os utilizadores**.

   ![Painel Todos os utilizadores do Azure Active Directory](./media/role-assignments-portal/aad-all-users.png)

1. Escolha um utilizador individual na lista.

1. Na secção **Gerir**, escolha **Recursos do Azure**.

   ![Utilizador do Azure Active Directory Recursos do Azure](./media/role-assignments-portal/aad-user-azure-resources.png)

   No painel de recursos do Azure, pode ver as atribuições de funções para o utilizador selecionado e a subscrição selecionada. Esta lista inclui apenas as atribuições de funções de recursos que tem permissão para ler. Por exemplo, se o utilizador também tem atribuições de funções que não poderá ler, essas atribuições de funções não serão apresentada na lista.

1. Se tiver várias subscrições, pode escolher o **subscrição** na lista pendente para ver as atribuições de funções numa subscrição diferente.

### <a name="list-role-assignments-for-a-resource-group"></a>Listar atribuições de funções para um grupo de recursos

1. Na lista de navegação, selecione **Grupos de recursos**.

1. Escolha um grupo de recursos e, em seguida, escolha **Controlo de acesso (IAM)**.

   No painel de controlo (IAM) de acesso, também conhecido como gerenciamento de identidades e acessos, pode ver quem tem acesso a este grupo de recursos. Tenha em conta que algumas funções têm o âmbito **Este recurso**, ao passo que outras são **(Herdadas)** de outro âmbito. O acesso é atribuído especificamente ao grupo de recursos ou herdado de uma atribuição à subscrição principal.

   ![Grupos de recursos](./media/role-assignments-portal/resource-group-access-control.png)

### <a name="list-role-assignments-for-a-subscription"></a>Listar atribuições de funções para uma subscrição

1. No portal do Azure, escolha **Todos os serviços** e, em seguida, **Subscrições**.

1. Escolha a sua subscrição.

1. Escolha **Controlo de acesso (IAM)**.

    No painel acesso de controlo (IAM), pode ver quem tem acesso a esta subscrição e a respetiva função.

    ![Painel de controlo (IAM) de acesso para uma subscrição](./media/role-assignments-portal/subscription-access-control.png)

    Os administradores de subscrição clássica e coadministradores são considerados proprietários da subscrição no modelo de RBAC.

### <a name="list-role-assignments-for-a-management-group"></a>Lista de atribuições de funções para um grupo de gestão

1. No portal do Azure, escolha **todos os serviços** e, em seguida **grupos de gestão**.

1. Escolha o grupo de gestão.

1. Escolher **(detalhes)** para o seu grupo de gestão selecionado.

    ![Grupos de gestão](./media/role-assignments-portal/management-groups-list.png)

1. Escolha **Controlo de acesso (IAM)**.

    No painel acesso de controlo (IAM), pode ver quem tem acesso a este grupo de gestão e a respetiva função.

    ![Painel de controlo (IAM) de acesso para um grupo de gestão](./media/role-assignments-portal/management-groups-access-control.png)

## <a name="grant-access"></a>Conceder acesso

No RBAC, para conceder acesso, atribuir uma função. Siga estes passos para conceder acesso em âmbitos diferentes.

### <a name="assign-a-role-at-a-resource-group-scope"></a>Atribuir uma função com um âmbito de grupo de recursos

1. Na lista de navegação, selecione **Grupos de recursos**.

1. Escolha um grupo de recursos.

1. Escolha **Controlo de acesso (IAM)** para ver a lista atual de atribuições de funções no âmbito do grupo de recursos.

   ![Painel de controlo (IAM) de acesso para um grupo de recursos](./media/role-assignments-portal/grant-resource-group-access-control.png)

1. Escolha **Adicionar** para abrir o painel **Adicionar permissões**.

   Se não tiver permissões para atribuir funções, não verá a opção **Adicionar**.

   ![Painel Adicionar permissões](./media/role-assignments-portal/add-permissions.png)

1. Na lista pendente **Função**, selecione uma função, como **Contribuidor de Máquina Virtual**.

1. Na lista **Selecionar**, selecione um utilizador, grupo ou aplicação. Se não vir o principal de segurança na lista, pode escrever na caixa **Selecionar** para procurar no diretório os nomes a apresentar, endereços de e-mail e identificadores de objetos.

1. Escolher **guardar** para atribuir a função.

   Após alguns instantes, é atribuída ao principal de segurança a função no âmbito do grupo de recursos.

### <a name="assign-a-role-at-a-subscription-scope"></a>Atribuir uma função no âmbito da subscrição

1. No portal do Azure, escolha **Todos os serviços** e, em seguida, **Subscrições**.

1. Escolha a sua subscrição.

1. Escolha **Controlo de acesso (IAM)** para ver a lista atual de atribuições de funções no âmbito da subscrição.

   ![Painel de controlo (IAM) de acesso para uma subscrição](./media/role-assignments-portal/grant-subscription-access-control.png)

1. Escolha **Adicionar** para abrir o painel **Adicionar permissões**.

   Se não tiver permissões para atribuir funções, não verá a opção **Adicionar**.

   ![Painel Adicionar permissões](./media/role-assignments-portal/add-permissions.png)

1. Na lista pendente **Função**, selecione uma função, como **Contribuidor de Máquina Virtual**.

1. Na lista **Selecionar**, selecione um utilizador, grupo ou aplicação. Se não vir o principal de segurança na lista, pode escrever na caixa **Selecionar** para procurar no diretório os nomes a apresentar, endereços de e-mail e identificadores de objetos.

1. Escolher **guardar** para atribuir a função.

   Após alguns instantes, é atribuída ao principal de segurança a função no âmbito da subscrição.

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Atribuir um utilizador como administrador de uma subscrição

Para tornar um utilizador administrador de uma subscrição do Azure, atribuí-los a [proprietário](built-in-roles.md#owner) função no âmbito da subscrição. A função de proprietário dá ao usuário acesso total a todos os recursos na subscrição, incluindo o direito de delegar o acesso a outras pessoas. Estes passos são os mesmos como qualquer outra atribuição de função.

1. No portal do Azure, escolha **Todos os serviços** e, em seguida, **Subscrições**.

1. Escolha a sua subscrição.

1. Escolha **Controlo de acesso (IAM)** para ver a lista atual de atribuições de funções no âmbito da subscrição.

   ![Painel de controlo (IAM) de acesso para uma subscrição](./media/role-assignments-portal/grant-subscription-access-control.png)

1. Escolha **Adicionar** para abrir o painel **Adicionar permissões**.

   Se não tiver permissões para atribuir funções, não verá a opção **Adicionar**.

   ![Painel Adicionar permissões](./media/role-assignments-portal/add-permissions.png)

1. Na **função** na lista pendente, selecione a **proprietário** função.

1. Na **selecione** , selecione um utilizador. Se não vir o utilizador na lista, pode digitar o **selecione** caixa para procurar o diretório para os nomes a apresentar e endereços de e-mail.

1. Escolher **guardar** para atribuir a função.

   Após alguns instantes, o utilizador tem atribuída a função de proprietário no âmbito da subscrição.

### <a name="assign-a-role-at-a-management-group-scope"></a>Atribuir uma função com um âmbito de grupo de gestão

1. No portal do Azure, escolha **todos os serviços** e, em seguida **grupos de gestão**.

1. Escolha o grupo de gestão.

1. Escolher **(detalhes)** para o seu grupo de gestão selecionado.

    ![Grupos de gestão](./media/role-assignments-portal/management-groups-list.png)

1. Escolha **Controlo de acesso (IAM)** para ver a lista atual de atribuições de funções no âmbito da subscrição.

   ![Painel de controlo (IAM) de acesso para um grupo de gestão](./media/role-assignments-portal/grant-management-groups-access-control.png)

1. Escolha **Adicionar** para abrir o painel **Adicionar permissões**.

   Se não tiver permissões para atribuir funções, não verá a opção **Adicionar**.

   ![Painel Adicionar permissões](./media/role-assignments-portal/add-permissions-management-groups.png)

1. Na **função** na lista pendente, selecione uma função, como **contribuinte do grupo de gestão**.

    Para obter informações sobre as ações suportadas em grupos de gestão para várias funções, consulte [organizar os recursos com grupos de gestão do Azure](../azure-resource-manager/management-groups-overview.md#management-group-access).

1. Na lista **Selecionar**, selecione um utilizador, grupo ou aplicação. Se não vir o principal de segurança na lista, pode escrever na caixa **Selecionar** para procurar no diretório os nomes a apresentar, endereços de e-mail e identificadores de objetos.

1. Escolher **guardar** para atribuir a função.

   Após alguns instantes, o principal de segurança é atribuída a função no âmbito do grupo de gestão.

## <a name="remove-access"></a>Remover o acesso

No RBAC, para remover o acesso, remova uma atribuição de função. Siga estes passos para remover o acesso.

### <a name="remove-a-role-assignment"></a>Remover uma atribuição de função

1. Abra o **controlo de acesso (IAM)** painel para o grupo de gestão, a subscrição, o grupo de recursos ou o recurso com a atribuição de função que pretende remover.

1. Na lista de atribuições de funções, adicione uma marca de verificação junto ao principal de segurança com a atribuição de função que pretende remover.

   ![Mensagem Remover atribuição de função](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Escolha **Remover**.

   ![Mensagem Remover atribuição de função](./media/role-assignments-portal/remove-role-assignment.png)

1. Na mensagem para remover atribuição de função que é apresentada, escolha **Sim**.

    As atribuições de funções herdadas não podem ser removidas. Se precisar de remover uma atribuição de função herdada, tem de o fazer no âmbito em que a atribuição da função foi criada. Na **âmbito** coluna, junto a **(herdado)** há um link que leva-o para o âmbito em que esta função foi atribuída. Vá para o âmbito aí listado para remover a atribuição de função.

   ![Mensagem Remover atribuição de função](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Passos Seguintes

* [Início Rápido: Conceder acesso a um utilizador com o RBAC e o Portal do Azure](quickstart-assign-role-user-portal.md)
* [Tutorial: Conceder acesso a um utilizador com o RBAC e o Azure PowerShell](tutorial-role-assignments-user-powershell.md)
* [Funções incorporadas](built-in-roles.md)
* [Organizar os recursos com grupos de gestão do Azure](../azure-resource-manager/management-groups-overview.md)
