---
title: Gerir o acesso através do RBAC e CLI do Azure | Microsoft Docs
description: Saiba como gerir o acesso de utilizadores, grupos e aplicações, utilizando o controlo de acesso baseado em funções (RBAC) e a CLI do Azure. Isto inclui a listagem acesso, conceder acesso e remoção de acesso.
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
ms.date: 04/03/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 15ff519f5af7471d6adaae44e2af19422ad44fea
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294409"
---
# <a name="manage-access-using-rbac-and-azure-cli"></a>Gerir o acesso através do RBAC e CLI do Azure

[O controlo de acesso baseado em funções (RBAC)](overview.md) é a forma de gerir o acesso a recursos no Azure. Este artigo descreve como gerir o acesso de utilizadores, grupos e aplicações através do RBAC e CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a CLI do Azure para gerir as atribuições de função, tem de ter os seguintes pré-requisitos:

* [CLI do Azure](/cli/azure). Pode utilizá-la no seu browser com o [Azure Cloud Shell](../cloud-shell/overview.md) ou pode [instalá-la](/cli/azure/install-azure-cli) no macOS, Linux e Windows e executá-la a partir da linha de comandos.

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

## <a name="custom-roles"></a>Funções personalizadas

### <a name="list-custom-roles"></a>Lista de funções personalizadas

Para listar as funções que estão disponíveis para atribuição a um âmbito, utilize [lista de definição de função az](/cli/azure/role/definition#az-role-definition-list).

Ambos os exemplos seguintes listam todas as funções personalizadas na subscrição atual:

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.roleName, "roleType":.roleType}'
```

```azurecli
az role definition list --output json | jq '.[] | if .roleType == "CustomRole" then {"roleName":.roleName, "roleType":.roleType} else empty end'
```

```Output
{
  "roleName": "My Management Contributor",
  "type": "CustomRole"
}
{
  "roleName": "My Service Operator Role",
  "type": "CustomRole"
}
{
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}

...
```

### <a name="create-a-custom-role"></a>Criar uma função personalizada

Para criar uma função personalizada, utilize [Criar definição de função az](/cli/azure/role/definition#az-role-definition-create). A definição de função pode ser uma descrição JSON ou um caminho para um ficheiro que contém uma descrição JSON.

```azurecli
az role definition create --role-definition <role_definition>
```

O exemplo seguinte cria uma função personalizada com o nome *operador de Máquina Virtual*. Esta função personalizada atribui acesso a todas as operações de leitura de *Microsoft. Compute*, *Microsoft*, e *Network* fornecedores e atribui acesso a recursos Para começar, reiniciar e monitorizar máquinas virtuais. Esta função personalizada pode ser utilizada em duas subscrições. Este exemplo utiliza um ficheiro JSON como entrada.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
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
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition create --role-definition ~/roles/vmoperator.json
```

### <a name="update-a-custom-role"></a>Atualizar uma função personalizada

Para atualizar uma função personalizada, primeiro utilizar [lista de definição de função az](/cli/azure/role/definition#az-role-definition-list) ao obter a definição de função. Segundo, efetue as alterações pretendidas para a definição de função. Por último, utilize [atualização az da definição de função](/cli/azure/role/definition#az-role-definition-update) para guardar a definição de função atualizada.

```azurecli
az role definition update --role-definition <role_definition>
```

O exemplo seguinte adiciona o *Microsoft.Insights/diagnosticSettings/* operação para o *ações* do *operador de Máquina Virtual* função personalizada.

vmoperator.json

```json
{
  "Name": "Virtual Machine Operator",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
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
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/11111111-1111-1111-1111-111111111111",
    "/subscriptions/33333333-3333-3333-3333-333333333333"
  ]
}
```

```azurecli
az role definition update --role-definition ~/roles/vmoperator.json
```

### <a name="delete-a-custom-role"></a>Eliminar uma função personalizada

Para eliminar uma função personalizada, utilize [eliminar a definição de função az](/cli/azure/role/definition#az-role-definition-delete). Para especificar a role para eliminar, utilize o nome da função ou o ID de função. Para determinar o ID de função, utilize [lista de definição de função az](/cli/azure/role/definition#az-role-definition-list).

```azurecli
az role definition delete --name <role_name or role_id>
```

O exemplo seguinte elimina a *operador de Máquina Virtual* função personalizada:

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Passos Seguintes

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

