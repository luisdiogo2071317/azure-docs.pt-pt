---
title: "Gerir o controlo de acesso baseado em funções (RBAC) com a CLI do Azure | Microsoft Docs"
description: "Saiba como gerir baseada em funções controlo de acesso (RBAC) com a interface de linha de comandos do Azure através da lista de funções e as ações de função e atribuir funções para os âmbitos de subscrição e a aplicação."
services: active-directory
documentationcenter: 
author: rolyon
manager: mtillman
ms.assetid: 3483ee01-8177-49e7-b337-4d5cb14f5e32
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.openlocfilehash: 5c099a7fd8848c2934603ec9b2db8947885226f9
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Gerir o controlo de acesso baseado em funções com a interface de linha de comandos do Azure

> [!div class="op_single_selector"]
> * [PowerShell](role-based-access-control-manage-access-powershell.md)
> * [CLI do Azure](role-based-access-control-manage-access-azure-cli.md)
> * [API REST](role-based-access-control-manage-access-rest.md)


Com baseada em funções controlo de acesso (RBAC), é possível definir acesso para utilizadores, grupos e principais de serviço através da atribuição de funções num determinado âmbito. Este artigo descreve como gerir o acesso através da interface de linha de comandos do Azure (CLI).

## <a name="prerequisites"></a>Pré-requisitos

Para utilizar a CLI do Azure para gerir o RBAC, tem de ter os seguintes pré-requisitos:

* [CLI 2.0 do Azure](/cli/azure/overview). Pode utilizá-la no seu browser com o [Azure Cloud Shell](../cloud-shell/overview.md) ou pode [instalá-la](/cli/azure/install-azure-cli) no macOS, Linux e Windows e executá-la a partir da linha de comandos.

## <a name="list-roles"></a>Lista de funções

### <a name="list-role-definitions"></a>Definições de função de lista

Para listar todas as definições de funções disponíveis, utilize [lista de definição de função az](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list
```

O exemplo seguinte lista o nome e descrição de todas as definições de funções disponíveis:

```azurecli
az role definition list --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
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
az role definition list --custom-role-only false --output json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description, "type":.properties.type}'
```

```Output
{
  "roleName": "API Management Service Contributor",
  "description": "Can manage service and the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Operator Role",
  "description": "Can manage service but not the APIs",
  "type": "BuiltInRole"
}
{
  "roleName": "API Management Service Reader Role",
  "description": "Read-only access to service and APIs",
  "type": "BuiltInRole"
}

...
```

### <a name="list-actions-of-a-role-definition"></a>Lista de ações de uma definição de função

Para listar as ações de uma definição de função, utilize [lista de definição de função az](/cli/azure/role/definition#az_role_definition_list):

```azurecli
az role definition list --name <role_name>
```

As listas de exemplo seguintes o *contribuinte* definição de função:

```azurecli
az role definition list --name "Contributor"
```

```Output
[
  {
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "properties": {
      "additionalProperties": {
        "createdBy": null,
        "createdOn": "0001-01-01T08:00:00.0000000Z",
        "updatedBy": null,
        "updatedOn": "2016-12-14T02:04:45.1393855Z"
      },
      "assignableScopes": [
        "/"
      ],
      "description": "Lets you manage everything except access to resources.",
      "permissions": [
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
      ],
      "roleName": "Contributor",
      "type": "BuiltInRole"
    },
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

As listas de exemplo seguintes o *ações* e *notActions* do *contribuinte* função:

```azurecli
az role definition list --name "Contributor" --output json | jq '.[] | {"actions":.properties.permissions[0].actions, "notActions":.properties.permissions[0].notActions}'
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
az role definition list --name "Virtual Machine Contributor" --output json | jq '.[] | .properties.permissions[0].actions'
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

### <a name="list-role-assignments-for-a-user"></a>Listar atribuições de função para um utilizador

Para listar as atribuições de funções para um utilizador específico, utilize [lista de atribuição de função az](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --assignee <assignee>
```

Por predefinição, serão apresentadas apenas atribuições confinadas à subscrição. Para ver as atribuições de um âmbito por recurso ou grupo, utilize `--all`.

O exemplo seguinte lista as atribuições de funções que são atribuídas diretamente para o  *patlong@contoso.com*  utilizador:

```azurecli
az role assignment list --all --assignee patlong@contoso.com --output json | jq '.[] | {"principalName":.properties.principalName, "roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
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

Para listar as atribuições de função que existem para um grupo de recursos, utilize [lista de atribuição de função az](/cli/azure/role/assignment#az_role_assignment_list):

```azurecli
az role assignment list --resource-group <resource_group>
```

O exemplo seguinte lista as atribuições de funções para o *pharma-vendas-projectforecast* grupo de recursos:

```azurecli
az role assignment list --resource-group pharma-sales-projectforecast --output json | jq '.[] | {"roleDefinitionName":.properties.roleDefinitionName, "scope":.properties.scope}'
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

## <a name="assign-access"></a>Atribuir acesso

### <a name="assign-a-role-to-a-user"></a>Atribuir uma função a um utilizador

Para atribuir uma função a um utilizador no âmbito do grupo de recursos, utilize [de criação da atribuição de função az](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee <assignee> --resource-group <resource_group>
```

O seguinte exemplo atribui o *contribuinte de Máquina Virtual* função  *patlong@contoso.com*  utilizador o *pharma-vendas-projectforecast* âmbito do grupo de recursos:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee patlong@contoso.com --resource-group pharma-sales-projectforecast
```

### <a name="assign-a-role-to-a-group"></a>Atribuir uma função a um grupo

Para atribuir uma função a um grupo, utilize [de criação da atribuição de função az](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

O seguinte exemplo atribui o *leitor* função para o *Ann Mack equipa* grupo com o ID 22222222-2222-2222-2222-222222222222 no âmbito de subscrição. Para obter o ID do grupo, pode utilizar [lista de grupos do ad az](/cli/azure/ad/group#az_ad_group_list) ou [mostrar de grupo do ad az](/cli/azure/ad/group#az_ad_group_show).

```azurecli
az role assignment create --role Reader --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

O seguinte exemplo atribui o *contribuinte de Máquina Virtual* função para o *Ann Mack equipa* grupo com o ID 22222222-2222-2222-2222-222222222222 num âmbito de recursos para uma rede virtual denominada *pharma vendas-projeto rede*:

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 22222222-2222-2222-2222-222222222222 --scope /subscriptions/11111111-1111-1111-1111-111111111111/resourcegroups/pharma-sales-projectforecast/providers/Microsoft.Network/virtualNetworks/pharma-sales-project-network
```

### <a name="assign-a-role-to-an-application"></a>Atribuir uma função a uma aplicação

Para atribuir uma função a uma aplicação, utilize [de criação da atribuição de função az](/cli/azure/role/assignment#az_role_assignment_create):

```azurecli
az role assignment create --role <role> --assignee-object-id <assignee_object_id> --resource-group <resource_group> --scope </subscriptions/subscription_id>
```

O seguinte exemplo atribui o *contribuinte de Máquina Virtual* função a uma aplicação com o ID de objeto 44444444-4444-4444-4444-444444444444 no *pharma-vendas-projectforecast* grupo de recursos âmbito. Para obter o ID de objeto da aplicação, pode utilizar [lista de aplicações do ad az](/cli/azure/ad/app#az_ad_app_list) ou [mostrar de aplicação de ad az](/cli/azure/ad/app#az_ad_app_show).

```azurecli
az role assignment create --role "Virtual Machine Contributor" --assignee-object-id 44444444-4444-4444-4444-444444444444 --resource-group pharma-sales-projectforecast
```

## <a name="remove-access"></a>Remover o acesso

### <a name="remove-a-role-assignment"></a>Remover uma atribuição de função

Para remover uma atribuição de função, utilize [eliminar a atribuição de função az](/cli/azure/role/assignment#az_role_assignment_delete):

```azurecli
az role assignment delete --assignee <assignee> --role <role> --resource-group <resource_group>
```

O exemplo a seguir remove o *contribuinte de Máquina Virtual* atribuição de função do  *patlong@contoso.com*  utilizador no *pharma-vendas-projectforecast* recursos grupo:

```azurecli
az role assignment delete --assignee patlong@contoso.com --role "Virtual Machine Contributor" --resource-group pharma-sales-projectforecast
```

O exemplo a seguir remove o *leitor* função a partir do *Ann Mack equipa* grupo com o ID 22222222-2222-2222-2222-222222222222 no âmbito de subscrição. Para obter o ID do grupo, pode utilizar [lista de grupos do ad az](/cli/azure/ad/group#az_ad_group_list) ou [mostrar de grupo do ad az](/cli/azure/ad/group#az_ad_group_show).

```azurecli
az role assignment delete --assignee 22222222-2222-2222-2222-222222222222 --role "Reader" --scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="custom-roles"></a>Funções personalizadas

### <a name="list-custom-roles"></a>Lista de funções personalizadas

Para listar as funções que estão disponíveis para atribuição a um âmbito, utilize [lista de definição de função az](/cli/azure/role/definition#az_role_definition_list).

Ambos os exemplos seguintes listam todas as funções personalizadas na subscrição atual:

```azurecli
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.properties.roleName, "type":.properties.type}'
```

```azurecli
az role definition list --output json | jq '.[] | if .properties.type == "CustomRole" then {"roleName":.properties.roleName, "type":.properties.type} else empty end'
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

Para criar uma função personalizada, utilize [Criar definição de função az](/cli/azure/role/definition#az_role_definition_create). A definição de função pode ser uma descrição JSON ou um caminho para um ficheiro que contém uma descrição JSON.

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

Para atualizar uma função personalizada, primeiro utilizar [lista de definição de função az](/cli/azure/role/definition#az_role_definition_list) ao obter a definição de função. Segundo, efetue as alterações pretendidas para a definição de função. Por último, utilize [atualização az da definição de função](/cli/azure/role/definition#az_role_definition_update) para guardar a definição de função atualizada.

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

Para eliminar uma função personalizada, utilize [eliminar a definição de função az](/cli/azure/role/definition#az_role_definition_delete). Para especificar a role para eliminar, utilize o nome da função ou o ID de função. Para determinar o ID de função, utilize [lista de definição de função az](/cli/azure/role/definition#az_role_definition_list).

```azurecli
az role definition delete --name <role_name or role_id>
```

O exemplo seguinte elimina a *operador de Máquina Virtual* função personalizada:

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Passos Seguintes

[!INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]

