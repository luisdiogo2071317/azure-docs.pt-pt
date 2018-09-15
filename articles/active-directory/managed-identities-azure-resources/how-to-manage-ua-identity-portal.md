---
title: Como gerir uma identidade gerida atribuído ao utilizador com o portal do Azure
description: Instruções passo a passo instruções sobre como criar, listar, eliminam e atribuir uma função para uma identidade gerida atribuído ao utilizador.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/16/2018
ms.author: daveba
ms.openlocfilehash: 180d4092a2570b719e77d98319ab9b329f2e48c5
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45637430"
---
# <a name="create-list-delete-or-assign-a-role-to-a-user-assigned-managed-identity-using-the-azure-portal"></a>Criar, listar, eliminar ou atribuir uma função para uma identidade gerida atribuído ao utilizador com o portal do Azure

[!INCLUDE [preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidades geridas para recursos do Azure fornece serviços do Azure com uma identidade gerida no Azure Active Directory. Pode utilizar esta identidade para autenticar para serviços que suportam a autenticação do Azure AD, sem a necessidade de credenciais no seu código. 

Neste artigo, irá aprender a criar, listar, eliminar ou atribuir uma função para um um atribuído ao utilizador gerido identidade através do Portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com identidades geridas para recursos do Azure, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre uma identidade gerida atribuído de sistema e atribuído ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar, ler (lista), atualizar e eliminar uma identidade gerida atribuído ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para ler (lista) as propriedades de uma identidade gerida atribuído ao utilizador.

## <a name="create-a-user-assigned-managed-identity"></a>Criar uma identidade gerida atribuída pelo utilizador

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) usando uma conta associada à subscrição do Azure para criar a identidade gerida atribuído ao utilizador.
2. Na caixa de pesquisa, escreva *identidades geridas por*e, em **serviços**, clique em **identidades geridas por**.
3. Clique em **Add** e introduza os valores nos campos a seguir sob **atribuída ao utilizador criar geridos** painel de identidade:
   - **Nome do recurso**: Este é o nome para o seu utilizador atribuído identidade gerida, por exemplo UAI1.
   - **Subscrição**: Escolha a subscrição para criar a identidade gerida atribuído ao utilizador em
   - **Grupo de recursos**: Crie um novo grupo de recursos para conter a sua identidade gerida atribuído ao utilizador ou escolha **utilizar existente** para criar a identidade gerida atribuído ao utilizador no grupo de recursos existente.
   - **Localização**: Escolha uma localização para implementar a identidade gerida atribuído ao utilizador, por exemplo **E.U.A. oeste**.
4. Clique em **Criar**.

![Criar uma identidade gerida atribuída pelo utilizador](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-managed-identities"></a>Lista de identidades geridas atribuído ao utilizador

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure para listar as identidades geridas atribuído ao utilizador.
2. Na caixa de pesquisa, escreva *identidades geridas por*e em serviços, clique em **identidades geridas por**.
3. Devolve uma lista de identidades geridas atribuído ao utilizador para a sua subscrição.  Para ver os detalhes de uma identidade gerida atribuído ao utilizador, clique no respetivo nome.

![Lista atribuído ao utilizador com identidade gerida](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-managed-identity"></a>Eliminar uma identidade gerida atribuído ao utilizador

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure para eliminar uma identidade gerida atribuído ao utilizador.
2. Selecionar a identidade gerida atribuído ao utilizador e clique em **eliminar**.
3. Na caixa de confirmação escolha, **Sim**.

![Eliminar utilizador atribuído a identidade gerida](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)

## <a name="assign-a-role-to-a-user-assigned-managed-identity"></a>Atribuir uma função para uma identidade gerida atribuído ao utilizador 

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure para listar as identidades geridas atribuído ao utilizador.
2. Na caixa de pesquisa, escreva *identidades geridas por*e em serviços, clique em **identidades geridas por**.
3. Devolve uma lista de identidades geridas atribuído ao utilizador para a sua subscrição.  Selecione a identidade de gerido atribuído ao utilizador que pretende atribuir uma função.
4. Selecione **controlo de acesso (IAM)** e, em seguida, selecione **Add**.

   ![Início de atribuído ao utilizador a identidade gerida](./media/how-to-manage-ua-identity-portal/assign-role-screenshot1.png)

5. No painel adicionar permissões, configure os seguintes valores e, em seguida, clique em **guardar**:
   - **Função** -a função para atribuir
   - **Atribuir acesso a** -o recurso para atribuir o atribuído ao utilizador de identidade gerido
   - **Selecione** -o membro atribuir acesso
   
   ![Identidade gerida atribuído ao utilizador IAM](./media/how-to-manage-ua-identity-portal/assign-role-screenshot2.png)  