---
title: Gerir o acesso através do RBAC e portal do Azure | Microsoft Docs
description: Saiba como gerir o acesso para utilizadores, grupos, os principais de serviço e identidades geridas, com o controlo de acesso baseado em funções (RBAC) e o portal do Azure. Isto inclui como listar, conceder e remover acesso.
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
ms.date: 11/30/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c339556353967db26f022384f2cf877962dc6d83
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2019
ms.locfileid: "54412313"
---
# <a name="manage-access-using-rbac-and-the-azure-portal"></a>Gerir o acesso através do RBAC e portal do Azure

[O controlo de acesso baseado em funções (RBAC)](overview.md) é a forma de gerir o acesso a recursos no Azure. Este artigo descreve como gerir o acesso para utilizadores, grupos, os principais de serviço e identidades geridas utilizando o RBAC e o portal do Azure.

## <a name="open-access-control-iam"></a>Abra o controlo de acesso (IAM)

O **controlo de acesso (IAM)** é apresentado o painel, também conhecido como gerenciamento de identidades e acessos, em todo o portal. Para ver ou gerir o acesso no portal, a primeira coisa que costuma fazer é abrir o painel de controlo (IAM) de acesso no âmbito em que pretende ver ou fazer uma alteração.

1. No portal do Azure, clique em **todos os serviços** e, em seguida, selecione o âmbito ou recurso que pretende ver ou gerir. Por exemplo, pode selecionar **grupos de gestão**, **subscrições**, **grupos de recursos**, ou um recurso.

1. Clique no recurso específico que pretende ver ou gerir.

1. Clique em **controlo de acesso (IAM)**.

    O código a seguir mostra um exemplo de painel de controlo (IAM) de acesso para uma subscrição.

    ![Painel de controlo (IAM) de acesso para uma subscrição](./media/role-assignments-portal/access-control-subscription.png)

## <a name="view-roles-and-permissions"></a>Ver funções e permissões

Uma definição de função é uma coleção de permissões que é utilizada para atribuições de funções. O Azure tem mais de 70 [funções incorporadas](built-in-roles.md). Siga estes passos para ver as funções e permissões que podem ser executadas na gestão e plano de dados.

1. Open **controlo de acesso (IAM)** num âmbito, como o grupo de gestão, subscrição, grupo de recursos ou recurso, em que pretende ver funções e permissões.

1. Clique nas **funções** separador para ver uma lista de todas as funções incorporadas e personalizadas.

   Pode ver o número de utilizadores e grupos que estão atribuídos a cada função neste âmbito.

   ![Lista de funções](./media/role-assignments-portal/roles-list.png)

1. Clique numa função individual para ver a quem foi atribuída a esta função e também ver as permissões para a função.

   ![Atribuições de funções](./media/role-assignments-portal/role-assignments.png)

## <a name="view-role-assignments"></a>Ver atribuições de funções

Ao gerir o acesso, pretende saber quem tem acesso, quais são as suas permissões e em qual nível. Para o acesso de lista para um utilizador, grupo, principal de serviço ou uma identidade gerida, visualizar as atribuições de funções.

### <a name="view-role-assignments-for-a-single-user"></a>Atribuições de funções de exibição para um único utilizador

Siga estes passos para ver o acesso a um único utilizador, grupo, principal de serviço ou uma identidade gerida num determinado âmbito.

1. Open **controlo de acesso (IAM)** num âmbito, como o grupo de gestão, subscrição, grupo de recursos ou recurso, em que pretende ver o acesso.

1. Clique nas **verificar acesso** separador.

    ![Controlo de acesso - separador de acesso de verificação](./media/role-assignments-portal/access-control-check-access.png)

1. Na **encontrar** , selecione o tipo de entidade de segurança que pretende verificar o acesso.

1. Na caixa de pesquisa, introduza uma cadeia de caracteres para procurar o diretório para os nomes a apresentar, endereços de e-mail ou identificadores de objetos.

    ![Verifique a lista de seleção de acesso](./media/role-assignments-portal/check-access-select.png)

1. Clique a entidade de segurança para abrir o **atribuições** painel.

    ![Painel de atribuições](./media/role-assignments-portal/check-access-assignments.png)

    Neste painel, pode ver as funções atribuídas para a entidade de segurança selecionado e o escopo. Se existirem quaisquer negar atribuições neste âmbito ou herdadas para este âmbito, estes serão apresentados.

### <a name="view-all-role-assignments-at-a-scope"></a>Ver todas as atribuições de função num âmbito

1. Open **controlo de acesso (IAM)** num âmbito, como o grupo de gestão, subscrição, grupo de recursos ou recurso, em que pretende ver o acesso.

1. Clique a **atribuições de funções** separador (ou clique no **vista** botão no mosaico de atribuições de função do modo de exibição) para ver todas as atribuições de função neste âmbito.

   ![Controlo de acesso - separador de atribuições de função](./media/role-assignments-portal/access-control-role-assignments.png)

   No separador de atribuições de função, pode ver quem tem acesso a este âmbito. Tenha em conta que algumas funções têm o âmbito **Este recurso**, ao passo que outras são **(Herdadas)** de outro âmbito. Acesso é atribuído especificamente para este recurso ou herdado de uma atribuição ao âmbito principal.

## <a name="add-a-role-assignment"></a>Adicionar uma atribuição de função

No RBAC, para conceder acesso, atribua uma função a um utilizador, o grupo, o principal de serviço ou a identidade gerida. Siga estes passos para conceder acesso em âmbitos diferentes.

### <a name="assign-a-role-at-a-scope"></a>Atribuir uma função num âmbito

1. Open **controlo de acesso (IAM)** num âmbito, como o grupo de gestão, subscrição, grupo de recursos ou recurso, onde pretende conceder acesso.

1. Clique nas **atribuições de funções** separador para ver todas as atribuições de função neste âmbito.

1. Clique em **adicionar atribuição de função** para abrir o painel de atribuição de função de adicionar.

   Se não tiver permissões para atribuir funções, a opção de atribuição de função de adicionar será desativada.

   ![Adicionar o painel de atribuição de função](./media/role-assignments-portal/add-role-assignment.png)

1. Na lista pendente **Função**, selecione uma função, como **Contribuidor de Máquina Virtual**.

1. Na **selecione** , selecione um utilizador, o grupo, o principal de serviço ou a identidade gerida. Se não vir o principal de segurança na lista, pode escrever na caixa **Selecionar** para procurar no diretório os nomes a apresentar, endereços de e-mail e identificadores de objetos.

1. Clique em **guardar** para atribuir a função.

   Após alguns instantes, a entidade de segurança é atribuída a função no âmbito selecionado.

### <a name="assign-a-user-as-an-administrator-of-a-subscription"></a>Atribuir um utilizador como administrador de uma subscrição

Para tornar um utilizador administrador de uma subscrição do Azure, atribuí-los a [proprietário](built-in-roles.md#owner) função no âmbito da subscrição. A função de proprietário dá ao usuário acesso total a todos os recursos na subscrição, incluindo o direito de delegar o acesso a outras pessoas. Estes passos são os mesmos como qualquer outra atribuição de função.

1. No portal do Azure, clique em **todos os serviços** e, em seguida **subscrições**.

1. Clique a subscrição em que pretende conceder acesso.

1. Clique em **controlo de acesso (IAM)**.

1. Clique nas **atribuições de funções** separador para ver todas as atribuições de função para esta subscrição.

1. Clique em **adicionar atribuição de função** para abrir o painel de atribuição de função de adicionar.

   Se não tiver permissões para atribuir funções, a opção de atribuição de função de adicionar será desativada.

   ![Adicionar o painel de atribuição de função](./media/role-assignments-portal/add-role-assignment.png)

1. Na **função** na lista pendente, selecione a **proprietário** função.

1. Na **selecione** , selecione um utilizador. Se não vir o utilizador na lista, pode digitar o **selecione** caixa para procurar o diretório para os nomes a apresentar e endereços de e-mail.

1. Clique em **guardar** para atribuir a função.

   Após alguns instantes, o utilizador tem atribuída a função de proprietário no âmbito da subscrição.

## <a name="remove-role-assignments"></a>Remover atribuições de funções

No RBAC, para remover o acesso, remova uma atribuição de função. Siga estes passos para remover o acesso.

1. Open **controlo de acesso (IAM)** num âmbito, como o grupo de gestão, subscrição, grupo de recursos ou recurso, em que pretende remover o acesso.

1. Clique nas **atribuições de funções** separador para ver todas as atribuições de função para esta subscrição.

1. Na lista de atribuições de funções, adicione uma marca de verificação junto ao principal de segurança com a atribuição de função que pretende remover.

   ![Mensagem Remover atribuição de função](./media/role-assignments-portal/remove-role-assignment-select.png)

1. Clique em **remover**.

   ![Mensagem Remover atribuição de função](./media/role-assignments-portal/remove-role-assignment.png)

1. Na mensagem de atribuição de função remove que aparece, clique em **Sim**.

    As atribuições de funções herdadas não podem ser removidas. Se precisar de remover uma atribuição de função herdada, tem de o fazer no âmbito em que a atribuição da função foi criada. Na **âmbito** coluna, junto a **(herdado)** há um link que leva-o para o âmbito em que esta função foi atribuída. Vá para o âmbito aí listado para remover a atribuição de função.

   ![Mensagem Remover atribuição de função](./media/role-assignments-portal/remove-role-assignment-inherited.png)

## <a name="next-steps"></a>Passos Seguintes

* [Tutorial: Conceder acesso de utilizador utilizando o RBAC e o portal do Azure](quickstart-assign-role-user-portal.md)
* [Tutorial: Conceder acesso de utilizador utilizando o RBAC e o Azure PowerShell](tutorial-role-assignments-user-powershell.md)
* [Resolver problemas relacionados com o RBAC no Azure](troubleshooting.md)
* [Organizar os recursos com grupos de gestão do Azure](../azure-resource-manager/management-groups-overview.md)
