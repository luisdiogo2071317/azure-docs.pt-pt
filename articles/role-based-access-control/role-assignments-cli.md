---
title: Gerir o acesso através do RBAC e CLI do Azure | Microsoft Docs
description: Saiba como gerir o acesso de utilizadores, grupos e aplicações, utilizando o controlo de acesso baseado em funções (RBAC) e a CLI do Azure. Isto inclui como lista de acesso, conceder acesso e remover o acesso.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/20/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 16577339f1aa33fbd1a8b90f4beaef1ee4ce806c
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316401"
---
# <a name="manage-access-using-rbac-and-azure-cli"></a>Gerir o acesso através do RBAC e CLI do Azure

[O controlo de acesso baseado em funções (RBAC)](overview.md) é a forma de gerir o acesso a recursos no Azure. Este artigo descreve como gerir o acesso de utilizadores, grupos e aplicações através do RBAC e CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para gerir o acesso, um dos seguintes precisa de:

* [Bash na Shell de nuvem do Azure](/azure/cloud-shell/overview)
* [CLI do Azure](/cli/azure)

## <a name="list-roles"></a>Lista de funções

Para listar todas as definições de funções disponíveis, utilize [lista de definição de função az](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list
```

O exemplo seguinte lista o nome e descrição de todas as definições de funções disponíveis:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.roleName, "description":.description}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs"
}

...
```

O exemplo seguinte apresenta uma lista todas as definições de função incorporada:

```azurecli
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "roleType": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "roleType": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role"></a>Lista de ações de uma função

Para listar as ações de uma definição de função, utilize [lista de definição de função az](/cli/azure/role/definition#az-role-definition-list):

```azurecli
az role definition list --name <role_name>
```

As listas de exemplo seguintes o *contribuinte* definição de função:

```azurecli
az role definition list --name "Contributor"
```

```Output
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

As listas de exemplo seguintes o *ações* e *notActions* do *contribuinte* função:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.permissions[0].actions, "notActions":.permissions[0].notActions}'
```

```Output
{
  "actions": [
    "*"
  ],
  "notActions": [
    "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ]
}
```

O exemplo seguinte lista as ações do *contribuinte de Máquina Virtual* função:

```azurecli
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .permissions[0].actions'
```

```Output
[
  "Microsoft.Authorization/*/read",
  "Microsoft.Compute/availabilitySets/*",
  "Microsoft.Compute/locations/*",
  "Microsoft.Compute/virtualMachines/*",
  "Microsoft.Compute/virtualMachineScaleSets/*",
  "Microsoft.Insights/alertRules/*",
  "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
  "Microsoft.Network/loadBalancers/backendAddressPools/join/action",

  ...

  "Microsoft.Storage/storageAccounts/listKeys/action",
  "Microsoft.Storage/storageAccounts/read"
]
```

## <a name="list-access"></a>Acesso de lista

No RBAC, para acesso de lista, lista as atribuições de funções.

### <a name="list-role-assignments-for-a-user"></a>Listar atribuições de função para um utilizador

Para listar as atribuições de funções para um utilizador específico, utilize [lista de atribuição de função az](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --assignee <assignee>
```

Por predefinição, serão apresentadas apenas atribuições confinadas à subscrição. Para ver as atribuições de um âmbito por recurso ou grupo, utilize `--all`.

O exemplo seguinte lista as atribuições de funções que são atribuídas diretamente para o *patlong@contoso.com* utilizador:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.principalName, "roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "principalName": "patlong@contoso.com",
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
```

### <a name="list-role-assignments-for-a-resource-group"></a>Listar atribuições de função para um grupo de recursos

Para listar as atribuições de função que existem para um grupo de recursos, utilize [lista de atribuição de função az](/cli/azure/role/assignment#az-role-assignment-list):

```azurecli
az role assignment list --resource-group <resource_group>
```

O exemplo seguinte lista as atribuições de funções para o *pharma-vendas-projectforecast* grupo de recursos:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.roleDefinitionName, "scope":.scope}'
```

```Output
{
  "roleDefinitionName": "Backup Operator",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}
{
  "roleDefinitionName": "Virtual Machine Contributor",
  "scope": "/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/pharma-sales-projectforecast"
}

...
```

## <a name="grant-access"></a>Conceder acesso

RBAC, para conceder acesso, crie uma atribuição de função.

### <a name="create-a-role-assignment-for-a-user"></a>Criar uma atribuição de função para um utilizador

Para criar uma atribuição de função para um utilizador no âmbito do grupo de recursos, utilize [de criação da atribuição de função az](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

O seguinte exemplo atribui o *contribuinte de Máquina Virtual* função *patlong@contoso.com* utilizador o *pharma-vendas-projectforecast* âmbito do grupo de recursos:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="create-a-role-assignment-for-a-group"></a>Criar uma atribuição de função para um grupo

Para criar uma atribuição de função para um grupo, utilize [de criação da atribuição de função az](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

O seguinte exemplo atribui o *leitor* função para o *Ann Mack equipa* grupo com o ID 22222222-2222-2222-2222-222222222222 no âmbito de subscrição. Para obter o ID do grupo, pode utilizar [lista de grupos do ad az](/cli/azure/ad/group#az-ad-group-list) ou [mostrar de grupo do ad az](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

O seguinte exemplo atribui o *contribuinte de Máquina Virtual* função para o *Ann Mack equipa* grupo com o ID 22222222-2222-2222-2222-222222222222 num âmbito de recursos para uma rede virtual denominada *pharma vendas-projeto rede*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="create-a-role-assignment-for-an-application"></a>Criar uma atribuição de função para uma aplicação

Para criar uma função para uma aplicação, utilize [de criação da atribuição de função az](/cli/azure/role/assignment#az-role-assignment-create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

O seguinte exemplo atribui o *contribuinte de Máquina Virtual* função a uma aplicação com o ID de objeto 44444444-4444-4444-4444-444444444444 no *pharma-vendas-projectforecast* grupo de recursos âmbito. Para obter o ID de objeto da aplicação, pode utilizar [lista de aplicações do ad az](/cli/azure/ad/app#az-ad-app-list) ou [mostrar de aplicação de ad az](/cli/azure/ad/app#az-ad-app-show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Remover acesso

No RBAC, para remover o acesso, remover uma atribuição de função utilizando [eliminar a atribuição de função az](/cli/azure/role/assignment#az-role-assignment-delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

O exemplo a seguir remove o *contribuinte de Máquina Virtual* atribuição de função do *patlong@contoso.com* utilizador no *pharma-vendas-projectforecast* recursos grupo:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

O exemplo a seguir remove o *leitor* função a partir do *Ann Mack equipa* grupo com o ID 22222222-2222-2222-2222-222222222222 no âmbito de subscrição. Para obter o ID do grupo, pode utilizar [lista de grupos do ad az](/cli/azure/ad/group#az-ad-group-list) ou [mostrar de grupo do ad az](/cli/azure/ad/group#az-ad-group-show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>Passos Seguintes

- [Tutorial: Criar uma função personalizada utilizando a CLI do Azure](tutorial-custom-role-cli.md)
- [Utilizar a CLI do Azure para gerir recursos do Azure e os grupos de recursos](../azure-resource-manager/xplat-cli-azure-resource-manager.md)