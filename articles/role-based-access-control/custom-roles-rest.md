---
title: Criar funções personalizadas utilizando a API de REST - Azure | Microsoft Docs
description: Saiba como criar funções personalizadas para o controlo de acesso baseado em funções (RBAC) utilizando a API REST. Isto inclui como listar, criar, atualizar e eliminar funções personalizadas.
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
ms.openlocfilehash: 8267846fed30baf2c37dcddd453ae9ead9341da9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36320591"
---
# <a name="create-custom-roles-using-the-rest-api"></a>Criar funções personalizadas utilizando a API REST

Se o [funções incorporadas](built-in-roles.md) não satisfazer as necessidades específicas da sua organização, pode criar as suas próprias funções personalizadas. Este artigo descreve como criar e gerir funções personalizadas utilizando a API REST.

## <a name="list-roles"></a>Lista de funções

Para listar todas as funções ou obter informações sobre uma única função utilizando o respetivo nome a apresentar, utilize o [as definições de função - lista](/rest/api/authorization/roledefinitions/list) REST API. Para chamar esta API, tem de ter acesso para o `Microsoft.Authorization/roleDefinitions/read` operação no âmbito. Vários [funções incorporadas](built-in-roles.md) é concedido o acesso a esta operação.

1. Começar com o pedido seguinte:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version=2015-07-01&$filter={filter}
    ```

1. No URI, substitua *{âmbito}* com o âmbito para o qual pretende para listar as funções.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{filtro}* com a condição que pretende aplicar para filtrar a lista de função.

    | Filtro | Descrição |
    | --- | --- |
    | `$filter=atScopeAndBelow()` | Lista de funções disponíveis para atribuição no âmbito especificado e nenhum dos respetivos âmbitos subordinados. |
    | `$filter=roleName%20eq%20'{roleDisplayName}'` | Utilize o formato de URL codificado do nome de apresentação exato da função. Por exemplo, `$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="get-information-about-a-role"></a>Obter informações sobre uma função

Para obter informações sobre uma função utilizando o respetivo identificador de definição de função, utilize o [obter as definições de função -](/rest/api/authorization/roledefinitions/get) REST API. Para chamar esta API, tem de ter acesso para o `Microsoft.Authorization/roleDefinitions/read` operação no âmbito. Vários [funções incorporadas](built-in-roles.md) é concedido o acesso a esta operação.

Para obter informações sobre uma única função utilizando o respetivo nome a apresentar, consulte o artigo anterior [lista funções](custom-roles-rest.md#list-roles) secção.

1. Utilize o [as definições de função - lista](/rest/api/authorization/roledefinitions/list) API REST para obter o identificador GUID para a função. Para funções incorporadas, também pode obter o identificador de [funções incorporadas](built-in-roles.md).

1. Começar com o pedido seguinte:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. No URI, substitua *{âmbito}* com o âmbito para o qual pretende para listar as funções.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{em roleDefinitionId}* com o identificador GUID da definição de função.

## <a name="create-a-custom-role"></a>Criar uma função personalizada

Para criar uma função personalizada, utilize o [as definições de função - Criar ou atualizar](/rest/api/authorization/roledefinitions/createorupdate) REST API. Para chamar esta API, tem de ter acesso para o `Microsoft.Authorization/roleDefinitions/write` operação em todos os o `assignableScopes`. Das funções incorporadas, apenas [proprietário](built-in-roles.md#owner) e [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator) é concedido o acesso a esta operação. 

1. Reveja a lista de [operações de fornecedor de recursos](resource-provider-operations.md) que estão disponíveis para criar as permissões para a função personalizada.

1. Utilize uma ferramenta GUID para gerar um identificador exclusivo que será utilizado para o identificador de função personalizada. O identificador tem o formato: `00000000-0000-0000-0000-000000000000`

1. Começar a utilizar o seguinte pedido e corpo:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. No URI, substitua *{âmbito}* com o primeiro `assignableScopes` da função personalizada.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{em roleDefinitionId}* com o identificador GUID da função personalizada.

1. Dentro do corpo do pedido, no `assignableScopes` propriedade, substitua *{em roleDefinitionId}* com o identificador GUID.

1. Substitua *{subscriptionId}* com o identificador de subscrição.

1. No `actions` propriedade, adicionar as operações que permite que a função para ser executada.

1. No `notActions` propriedade, adicionar as operações que são excluídas da permitidos `actions`.

1. No `roleName` e `description` propriedades, especifique um nome de função exclusivo e uma descrição. Para obter mais informações sobre as propriedades, consulte [funções personalizadas](custom-roles.md).

    O seguinte mostra um exemplo de um corpo do pedido:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="update-a-custom-role"></a>Atualizar uma função personalizada

Para atualizar uma função personalizada, utilize o [as definições de função - Criar ou atualizar](/rest/api/authorization/roledefinitions/createorupdate) REST API. Para chamar esta API, tem de ter acesso para o `Microsoft.Authorization/roleDefinitions/write` operação em todos os o `assignableScopes`. Das funções incorporadas, apenas [proprietário](built-in-roles.md#owner) e [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator) é concedido o acesso a esta operação. 

1. Utilize o [as definições de função - lista](/rest/api/authorization/roledefinitions/list) ou [obter as definições de função -](/rest/api/authorization/roledefinitions/get) API REST para obter informações sobre a função personalizada. Para obter mais informações, consulte o anteriormente [lista funções](custom-roles-rest.md#list-roles) secção.

1. Começar com o pedido seguinte:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. No URI, substitua *{âmbito}* com o primeiro `assignableScopes` da função personalizada.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{em roleDefinitionId}* com o identificador GUID da função personalizada.

1. Com base nas informações sobre a função personalizada, crie um corpo do pedido com o seguinte formato:

    ```json
    {
      "name": "{roleDefinitionId}",
      "properties": {
        "roleName": "",
        "description": "",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
    
            ],
            "notActions": [
    
            ]
          }
        ],
        "assignableScopes": [
          "/subscriptions/{subscriptionId}"
        ]
      }
    }
    ```

1. Atualize o corpo do pedido com as alterações que pretende efetuar para a função personalizada.

    O seguinte mostra um exemplo de um corpo do pedido com uma ação de definições de diagnóstico novas adicionada:

    ```json
    {
      "name": "88888888-8888-8888-8888-888888888888",
      "properties": {
        "roleName": "Virtual Machine Operator",
        "description": "Can monitor and restart virtual machines.",
        "type": "CustomRole",
        "permissions": [
          {
            "actions": [
              "Microsoft.Storage/*/read",
              "Microsoft.Network/*/read",
              "Microsoft.Compute/*/read",
              "Microsoft.Compute/virtualMachines/start/action",
              "Microsoft.Compute/virtualMachines/restart/action",
              "Microsoft.Authorization/*/read",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Insights/diagnosticSettings/*",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "assignableScopes": [
          "/subscriptions/00000000-0000-0000-0000-000000000000"
        ]
      }
    }
    ```

## <a name="delete-a-custom-role"></a>Eliminar uma função personalizada

Para eliminar uma função personalizada, utilize o [eliminar as definições de função -](/rest/api/authorization/roledefinitions/delete) REST API. Para chamar esta API, tem de ter acesso para o `Microsoft.Authorization/roleDefinitions/delete` operação em todos os o `assignableScopes`. Das funções incorporadas, apenas [proprietário](built-in-roles.md#owner) e [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator) é concedido o acesso a esta operação. 

1. Utilize o [as definições de função - lista](/rest/api/authorization/roledefinitions/list) ou [obter as definições de função -](/rest/api/authorization/roledefinitions/get) API REST para obter o identificador GUID da função personalizada. Para obter mais informações, consulte o anteriormente [lista funções](custom-roles-rest.md#list-roles) secção.

1. Começar com o pedido seguinte:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}?api-version=2015-07-01
    ```

1. No URI, substitua *{âmbito}* com o âmbito de que pretende eliminar a função personalizada.

    | Âmbito | Tipo |
    | --- | --- |
    | `subscriptions/{subscriptionId}` | Subscrição |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

1. Substitua *{em roleDefinitionId}* com o identificador GUID da função personalizada.

## <a name="next-steps"></a>Passos Seguintes

- [Funções personalizadas no Azure](custom-roles.md)
- [Gerir o acesso através do RBAC e a API REST](role-assignments-rest.md)
- [Referência da API REST do Azure](/rest/api/azure/)