---
title: Como gerir um utilizador atribuído a identidade gerida no portal do Azure
description: Instruções passo a passo obter instruções sobre como criar, listar e eliminar um utilizador atribuído a identidade gerida.
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
ms.openlocfilehash: bd6327aa5c16d57c550025667659f9245a5b0b65
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/16/2018
ms.locfileid: "42060251"
---
# <a name="create-list-or-delete-a-user-assigned-identity-using-the-azure-portal"></a>Criar, lista ou eliminar um utilizador atribuído a identidade no portal do Azure

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Identidade de serviço gerida fornece serviços do Azure com uma identidade gerida no Azure Active Directory. Pode utilizar esta identidade para autenticar para serviços que suportam a autenticação do Azure AD, sem a necessidade de credenciais no seu código. 

Neste artigo, saiba como criar, listar e eliminar um utilizador atribuído a identidade com o Portal do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- Se não estiver familiarizado com a identidade do serviço gerido, veja a [secção Descrição geral](overview.md). **Certifique-se de que reveja os [diferença entre um sistema atribuído e a identidade atribuída ao utilizador](overview.md#how-does-it-work)**.
- Se ainda não tiver uma conta do Azure, [inscreva-se numa conta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para efetuar as operações de gestão neste artigo, a conta tem das atribuições de funções seguintes:
    - [Contribuidor de identidade de geridos](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) função para criar, ler (lista), atualizar e eliminar uma identidade atribuída ao utilizador.
    - [Gerido operador de identidade](/azure/role-based-access-control/built-in-roles#managed-identity-operator) função para ler as propriedades de uma identidade de utilizador atribuída de (lista).

## <a name="create-a-user-assigned-managed-identity"></a>Criar um utilizador atribuído a identidade gerida

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure para criar o utilizador atribuído a identidade gerida.
2. Na caixa de pesquisa, escreva *identidades geridas por*e, em **serviços**, clique em **identidades geridas por**.
3. Clique em **Add** e introduza os valores nos campos a seguir sob **atribuída ao utilizador criar geridos** painel de identidade:
   - **Nome do recurso**: Este é o nome para o utilizador atribuído geridos identidade, por exemplo UAI1.
   - **Subscrição**: Escolha a subscrição para criar o utilizador atribuído a identidade gerida em
   - **Grupo de recursos**: Crie um novo grupo de recursos para conter a sua identidade atribuída ao utilizador ou escolha **utilizar existente** para criar o utilizador atribuída uma identidade gerida num grupo de recursos existente.
   - **Localização**: Escolha uma localização para implementar o utilizador atribuído a identidade gerida, por exemplo **E.U.A. oeste**.
4. Clique em **Criar**.

![Criar um utilizador atribuído a identidade gerida](./media/how-to-manage-ua-identity-portal/create-user-assigned-managed-identity-portal.png)

## <a name="list-user-assigned-identities"></a>Utilizador de lista identidades atribuído

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure para listar o utilizador atribuído identidades geridas.
2. Na caixa de pesquisa, escreva *identidades geridas por*e em serviços, clique em **identidades geridas por**.
3. Devolve uma lista do utilizador atribuído identidades geridas para a sua subscrição.  Para ver os detalhes de um utilizador atribuído, clique no respetivo nome.

![Utilizador da lista atribuído a identidade gerida](./media/how-to-manage-ua-identity-portal/list-user-assigned-managed-identity-portal.png)

## <a name="delete-a-user-assigned-identity"></a>Eliminar um identidade atribuída ao utilizador

1. Inicie sessão para o [portal do Azure](https://portal.azure.com) através de uma conta associada à subscrição do Azure para eliminar um utilizador atribuído a identidade gerida.
2. Selecionar a identidade atribuída ao utilizador e clique em **eliminar**.
3. Na caixa de confirmação escolha, **Sim**.

![Eliminar identidade gerida atribuída pelo utilizador](./media/how-to-manage-ua-identity-portal/delete-user-assigned-managed-identity-portal.png)