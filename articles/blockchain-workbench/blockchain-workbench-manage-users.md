---
title: Gerir utilizadores no Azure Blockchain Workbench
description: Como gerir os utilizadores no Azure Blockchain Workbench.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 4/26/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: zeyadr
manager: femila
ms.openlocfilehash: c17a406a0fd6f1172599ac0379c6afc139403148
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="manage-users-in-azure-blockchain-workbench"></a>Gerir utilizadores no Azure Blockchain Workbench

Azure Blockchain Workbench inclui a gestão de utilizadores para pessoas e as organizações que fazem parte do seu consortium.

## <a name="prerequisites"></a>Pré-requisitos

Não é necessária uma implementação de Blockchain Workbench. Consulte [implementação do Azure Blockchain Workbench](blockchain-workbench-deploy.md) para obter detalhes sobre a implementação.

## <a name="add-azure-ad-users"></a>Adicionar utilizadores do Azure AD

O Blockchain Workbench do Azure utiliza o Azure Active Directory (Azure AD) para autenticação, o controlo de acesso e funções. Os utilizadores no inquilino do Workbench Blockchain do Azure AD podem autenticar e utilizar Blockchain Workbench. Adicione utilizadores à função de aplicação administrador interagir e efetuar ações.

Os utilizadores do Blockchain Workbench tem de existir no inquilino do Azure AD antes de pode atribuir-lhes a aplicações e funções. Para adicionar utilizadores ao Azure AD, utilize os seguintes passos:

1.  Inicie sessão no [portal do Azure](https://portal.azure.com).
2.  Selecione a sua conta no canto superior direito e mudar para o inquilino do Azure AD associado Blockchain Workbench.
3.  Selecione **do Azure Active Directory > utilizadores**. É apresentada uma lista de utilizadores no seu diretório.
4.  Para adicionar utilizadores ao diretório, selecione **novo utilizador**. Para utilizadores externos, selecione **novo utilizador convidado**.

    ![Novo utilizador](media/blockchain-workbench-manage-users/add-ad-user.png)

5.  Preencha os campos obrigatórios para o novo utilizador. Selecione **Criar**.

Visite [do Azure AD](../active-directory/add-users-azure-active-directory.md) documentação para obter mais detalhes sobre como gerir os utilizadores no Azure AD.

## <a name="manage-blockchain-workbench-administrators"></a>Gerir administradores Blockchain Workbench

Assim que os utilizadores foram adicionados ao diretório, o próximo passo é escolher quais os utilizadores que são administradores de Blockchain Workbench. Os utilizadores a **administradores** grupo estão associados a **função de administrador de aplicações** no Blockchain Workbench. Os administradores podem adicionar ou remover utilizadores, atribuir utilizadores a cenários específicos e crie novas aplicações.

Para adicionar os utilizadores a **administradores** grupo no diretório do Azure AD:

1.  Inicie sessão no [portal do Azure](https://portal.azure.com).
2.  Certifique-se de que está no inquilino do Azure AD associado Blockchain Workbench, selecionando a sua conta no canto superior direito.
3.  Selecione **do Azure Active Directory > aplicações da empresa**.
4.  Selecione a aplicação de cliente do Azure AD para o Blockchain Workbench
    
    ![Todos os registos de aplicações da empresa](media/blockchain-workbench-manage-users/select-blockchain-client-app.png)

5.  Selecione **utilizadores e grupos > Adicionar utilizador**.
6.  No **adicionar atribuição**, selecione **utilizadores**. Escolher ou procure o utilizador que pretende adicionar como um administrador. Clique em **selecione** quando terminar a escolher.

    ![Adicionar atribuição](media/blockchain-workbench-manage-users/add-user-assignment.png)

9.  Certifique-se **função** está definido como **administrador**
10. Selecione **Atribuir**. Os utilizadores adicionados são apresentados na lista com a função de administrador atribuída.

    ![Utilizadores de aplicações de cliente Blockchain](media/blockchain-workbench-manage-users/blockchain-admin-list.png)

## <a name="managing-blockchain-workbench-members"></a>Gerir membros do Blockchain Workbench

Utilize a aplicação do Blockchain Workbench para gerir utilizadores e organizações que fazem parte do seu consortium. Pode adicionar ou remover utilizadores de aplicações e funções.

[Abrir o Blockchain Workbench](blockchain-workbench-deploy.md#blockchain-workbench-web-url) no seu browser e inicie sessão como administrador.

![Blockchain Workbench](media/blockchain-workbench-manage-users/blockchain-workbench-applications.png)

### <a name="managing-members-in-applications"></a>Gerir membros de aplicações

Os membros são adicionados a cada aplicação. Os membros podem ter uma ou mais funções de aplicação para iniciar contratos ou executar ações.

Para adicionar membros a uma aplicação, selecione um mosaico de aplicação no **aplicações** painel.

![Selecionar aplicação](media/blockchain-workbench-manage-users/blockchain-workbench-select-application.png)

> [!NOTE]
> O número de membros associados à aplicação selecionada é refletido no mosaico de membros.

#### <a name="add-member-to-application"></a>Adicionar membro a aplicação

1. Selecione o mosaico de membro para apresentar uma lista de membros actuais.
2. Selecione **adicionar membros**.

    ![Adicionar membros](media/blockchain-workbench-manage-users/application-add-members.png)

3. Procure o nome do utilizador.  Apenas os utilizadores do AD do Azure que existe no inquilino do Blockchain Workbench estão listados. Se o utilizador não for encontrado, terá de [adicionar utilizadores do Azure AD](#add-azure-ad-users).

    ![Adicionar membros](media/blockchain-workbench-manage-users/find-user.png)

4. Selecione um **função** da lista pendente.

    ![Selecione os membros da função](media/blockchain-workbench-manage-users/application-select-role.png)

5. Selecione **adicionar** para adicionar o membro com a função associada à aplicação.

#### <a name="remove-member-from-application"></a>Remover membro da aplicação

1. Selecione o mosaico de membro para apresentar uma lista de membros actuais.
2. Para o utilizador que pretende remover, escolha **remover** da função de lista pendente.

    ![Remover membro](media/blockchain-workbench-manage-users/application-remove-member.png)

#### <a name="change-or-add-role"></a>Alterar ou Adicionar função

1. Selecione o mosaico de membro para apresentar uma lista de membros actuais.
2. Para o utilizador que pretende alterar, clique na lista pendente e selecione a nova função.

    ![Alteração da função](media/blockchain-workbench-manage-users/application-change-role.png)

## <a name="next-steps"></a>Passos Seguintes

[Criar uma aplicação de blockchain no Azure Blockchain Workbench](blockchain-workbench-create-app.md)
