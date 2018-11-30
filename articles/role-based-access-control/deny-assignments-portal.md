---
title: Vista de negar atribuições com o portal do Azure | Documentos da Microsoft
description: Saiba como ver os utilizadores, grupos, principais de serviço e de identidades geridas que foram negou o acesso a ações específicas num âmbito específico com o portal do Azure.
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
ms.openlocfilehash: f5870ddbbb8be0ebbeae7656485521a327b86d5b
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642810"
---
# <a name="view-deny-assignments-using-the-azure-portal"></a>Vista de negar atribuições com o portal do Azure

[Negar atribuições](deny-assignments.md) impedir que os utilizadores a efetuar ações específicas, mesmo se uma atribuição de função lhes concede acesso. Mesmo que não é possível criar o seu próprio negar atribuições, terá de conseguir ver negar atribuições porque eles podem afetar as suas permissões geral. Para obter informações sobre uma atribuição de negação, tem de ter a permissão `Microsoft.Authorization/denyAssignments/read`, que está incluída na maioria das [funções incorporadas](built-in-roles.md).

Este artigo descreve como utilizar o portal do Azure para ver as atribuições de negação.

> [!NOTE]
> Neste momento, negar atribuições são só de leitura e só podem ser definidas pelo Azure.

## <a name="view-deny-assignments"></a>Ver atribuições de negação

Siga estes passos para ver negar atribuições no âmbito de grupo de subscrição ou gestão.

1. No portal do Azure, clique em **todos os serviços** e, em seguida **grupos de gestão** ou **subscrições**.

1. Clique no grupo de gestão ou a subscrição que pretende ver.

1. Clique em **controlo de acesso (IAM)**.

1. Clique nas **negar atribuições** separador (ou clique no **vista** botão na exibição negar atribuições de mosaico).

    Se existirem quaisquer negar atribuições neste âmbito ou herdadas para este âmbito, estes serão apresentados.

    ![Controlo de acesso – separador atribuições de negação](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Para apresentar as colunas adicionais, clique em **Edit Columns**.

    ![Negar atribuições - colunas](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Nome** | Nome da atribuição de negação. |
    | **Tipo de principal** | Utilizador, grupo, grupo definido pelo sistema ou principal de serviço. |
    | **Negado**  | Nome do principal de segurança que está incluído na atribuição de negação. |
    | **Id** | Identificador exclusivo para a atribuição de negação. |
    | **Principais excluídos** | Se existem entidades de segurança que são excluídas a atribuição de negação. |
    | **Não é aplicável a elementos subordinados** | Se a atribuição de negação é herdada para subscopes. |
    | **Sistema protegido** | Se a atribuição de negação é gerida pelo Azure. Atualmente, sempre Sim. |
    | **Âmbito** | Grupo de gestão, subscrição, grupo de recursos ou recurso. |

1. Adicionar uma marca de verificação para qualquer um dos itens ativados e, em seguida, clique em **OK** para apresentar as colunas selecionadas.

## <a name="view-details-about-a-deny-assignment"></a>Ver detalhes sobre uma atribuição de negação

Siga estes passos para ver detalhes adicionais sobre uma atribuição de negação.

1. Abra o **negar atribuições** painel, conforme descrito na secção anterior.

1. Clique no nome da atribuição de negação para abrir o **utilizadores** painel.

    ![Negar atribuição - utilizadores](./media/deny-assignments-portal/deny-assignment-users.png)

    O **utilizadores** painel inclui as duas secções seguintes.

    |  |  |
    | --- | --- |
    | **Negar a atribuição aplica-se a**  | Entidades de segurança que se aplica a atribuição de negação. |
    | **Negar a exclui da atribuição** | Entidades de segurança que são excluídas a atribuição de negação. |

    **Definido pelo sistema Principal** representa todos os utilizadores, grupos, os principais de serviço e identidades geridas num diretório do Azure AD.

1. Para ver uma lista das permissões que são negado, clique em **negado permissões**.

    ![Negar atribuição - permissões negado](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Tipo de ação | Descrição |
    | --- | --- |
    | **Ações**  | Negado operações de gestão. |
    | **notActions** | Operações de gestão excluídas da operação de gestão negado. |
    | **DataActions**  | Negado operações de dados. |
    | **NotDataActions** | Operações de dados excluídas negado a operação de dados. |

    No exemplo mostrado na captura de ecrã anterior, seguem-se as permissões efetivas:

    - Armazenamento de todas as operações no plano de dados serão negadas, exceto para operações de computação.

1. Para ver as propriedades de uma atribuição de negar, clique em **propriedades**.

    ![Negar atribuição - propriedades](./media/deny-assignments-portal/deny-assignment-properties.png)

    Sobre o **propriedades** painel, pode ver o nome da atribuição de negar, o ID, a descrição e o âmbito. O **não se aplica aos filhos** comutador indica se a atribuição de negação é herdada para subscopes. O **sistema protegido** comutador indica se negar atribuição é gerenciada pelo Azure. Atualmente, isto é **Sim** em todos os casos.

## <a name="next-steps"></a>Passos Seguintes

* [Compreender as atribuições de negação](deny-assignments.md)
* [Lista negar atribuições com o RBAC e a API REST](deny-assignments-rest.md)
