---
title: Gerir o acesso através do RBAC e a API de REST - Azure | Microsoft Docs
description: Saiba como gerir o acesso de utilizadores, grupos e aplicações, utilizando o controlo de acesso baseado em funções (RBAC) e a API REST. Isto inclui como lista de acesso, conceder acesso e remover o acesso.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: cfcb87fdff8105b25d4f7e63b775aaf9243d2a90
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36317012"
---
# <a name="manage-access-using-rbac-and-the-rest-api"></a>Gerir o acesso através do RBAC e a API REST

[O controlo de acesso baseado em funções (RBAC)](overview.md) é a forma de gerir o acesso a recursos no Azure. Este artigo descreve como gerir o acesso de utilizadores, grupos e aplicações através do RBAC e a API REST.

## <a name="list-access"></a>Acesso de lista

No RBAC, para acesso de lista, lista as atribuições de funções. Para listar atribuições de função, utilize um do [atribuições de funções - lista](/rest/api/authorization/roleassignments/list) REST APIs. Para refinar os resultados, especifique um âmbito e um filtro opcional. Para chamar a API, tem de ter acesso para o `Microsoft.Authorization/roleAssignments/read` operação no âmbito especificado. Vários [funções incorporadas](built-in-roles.md) é concedido o acesso a esta operação.

1. Começar com o pedido seguinte:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. No URI, substitua *{âmbito}* com o âmbito para o qual pretende para listar as atribuições de funções.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{filtro}* com a condição que pretende aplicar para filtrar a lista de atribuição de função.

    | Filtro | Descrição |
    | --- | --- |
    | `$filter=atScope()` | Listar atribuições de funções para apenas o âmbito especificado, não incluindo as atribuições de função no subscopes. |
    | `$filter=principalId%20eq%20'{objectId}'` | Listar atribuições de funções para um utilizador especificado, grupo ou principal de serviço. |
    | `$filter=assignedTo('{objectId}')` | Listar atribuições de funções para um utilizador especificado, incluindo aqueles herdada a partir de grupos. |

## <a name="grant-access"></a>Conceder acesso

RBAC, para conceder acesso, crie uma atribuição de função. Para criar uma atribuição de função, utilize o [atribuições de funções - criar](/rest/api/authorization/roleassignments/create) REST API e especifique o principal de segurança, a definição de função e o âmbito. Para chamar esta API, tem de ter acesso para o `Microsoft.Authorization/roleAssignments/write` operação. Das funções incorporadas, apenas [proprietário](built-in-roles.md#owner) e [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator) é concedido o acesso a esta operação.

1. Utilize o [as definições de função - lista](/rest/api/authorization/roledefinitions/list) REST API ou consulte [funções incorporadas](built-in-roles.md) para obter o identificador para a definição de função que pretende atribuir.

1. Utilize uma ferramenta GUID para gerar um identificador exclusivo que será utilizado para o identificador de atribuição de função. O identificador tem o formato: `00000000-0000-0000-0000-000000000000`

1. Começar a utilizar o seguinte pedido e corpo:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```
    
1. No URI, substitua *{âmbito}* com o âmbito de atribuição de função.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{roleAssignmentName}* com o identificador GUID de atribuição de função.

1. Dentro do corpo do pedido, substitua *{subscriptionId}* com o identificador de subscrição.

1. Substitua *{em roleDefinitionId}* com o identificador de definição de função.

1. Substitua *{principalId}* com o identificador de objeto do principal de serviço que será atribuído a função, grupo ou utilizador.

## <a name="remove-access"></a>Remover acesso

No RBAC, para remover o acesso, remova uma atribuição de função. Para remover uma atribuição de função, utilize o [atribuições de funções - eliminar](/rest/api/authorization/roleassignments/delete) REST API. Para chamar esta API, tem de ter acesso para o `Microsoft.Authorization/roleAssignments/delete` operação. Das funções incorporadas, apenas [proprietário](built-in-roles.md#owner) e [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator) é concedido o acesso a esta operação.

1. Obter o identificador de atribuição de função (GUID). Este identificador é devolvido quando cria pela primeira vez a atribuição de função ou pode obtê-lo por listar as atribuições de funções.

1. Começar com o pedido seguinte:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. No URI, substitua *{âmbito}* com o âmbito para remover a atribuição de função.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{roleAssignmentName}* com o identificador GUID de atribuição de função.

## <a name="next-steps"></a>Passos Seguintes

- [Implementar recursos com modelos do Resource Manager e a API REST do Resource Manager](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Referência da API REST do Azure](/rest/api/azure/)
- [Criar funções personalizadas utilizando a API REST](custom-roles-rest.md)