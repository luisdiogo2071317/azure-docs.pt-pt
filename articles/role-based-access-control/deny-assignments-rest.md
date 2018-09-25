---
title: Lista negar atribuições com o RBAC e a API de REST - Azure | Documentos da Microsoft
description: Saiba como listar atribuições de utilizadores, grupos e aplicações, com controlo de acesso baseado em funções (RBAC) e a API REST de negação.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ''
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 44c1d3b18bb9bdc63247379fe3f277cb6542f2da
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46975591"
---
# <a name="list-deny-assignments-using-rbac-and-the-rest-api"></a>Lista negar atribuições com o RBAC e a API REST

Atualmente, negar atribuições são **só de leitura** e só pode ser definido pelo Azure. Mesmo que não é possível criar o seu próprio negar atribuições, pode listar atribuições de negação porque elas poderiam afetar as suas permissões em vigor. Este artigo descreve como negar que para listar atribuições com o RBAC e a API REST.

## <a name="list-a-single-deny-assignment"></a>Lista de um único negar atribuição

1. Começar com o pedido seguinte:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments/{deny-assignment-id}?api-version=2018-07-01-preview
    ```

1. No URI, substitua *{âmbito}* com o âmbito para o qual pretende listar as atribuições de negação.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{id de atribuição de negar}* com o identificador de atribuição de negar que pretende recuperar.

## <a name="list-multiple-deny-assignments"></a>Listar várias atribuições de negação

1. Começar com um dos pedidos seguintes:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview
    ```

    Com os parâmetros opcionais:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. No URI, substitua *{âmbito}* com o âmbito para o qual pretende listar as atribuições de negação.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{filtro}* com a condição que pretende aplicar para filtrar a lista de atribuição de negação.

    | Filtro | Descrição |
    | --- | --- |
    | (sem filtro) | Lista todos os negar atribuições em acima e abaixo âmbito especificado. |
    | `$filter=atScope()` | Lista de negação atribuições para apenas o âmbito especificado e superior. Não inclui as atribuições de negação em subscopes. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Lista de negação atribuições com o nome especificado. |

## <a name="list-deny-assignments-at-the-root-scope-"></a>Lista negar atribuições no âmbito de raiz (/)

1. Elevar o acesso, conforme descrito em [elevar o acesso de Administrador Global no Azure Active Directory](elevate-access-global-admin.md).

1. Utilize o seguinte pedido:

    ```http
    GET https://management.azure.com/providers/Microsoft.Authorization/denyAssignments?api-version=2018-07-01-preview&$filter={filter}
    ```

1. Substitua *{filtro}* com a condição que pretende aplicar para filtrar a lista de atribuição de negação. Um filtro é necessário.

    | Filtro | Descrição |
    | --- | --- |
    | `$filter=atScope()` | Lista de negação atribuições para apenas o âmbito de raiz. Não inclui as atribuições de negação em subscopes. |
    | `$filter=denyAssignmentName%20eq%20'{deny-assignment-name}'` | Lista de negação atribuições com o nome especificado. |

1. Remova acesso elevado.

## <a name="next-steps"></a>Passos Seguintes

- [Compreender as atribuições de negação](deny-assignments.md)
- [Elevar o acesso de Administrador Global no Azure Active Directory](elevate-access-global-admin.md)
- [Referência à API REST do Azure](/rest/api/azure/)
