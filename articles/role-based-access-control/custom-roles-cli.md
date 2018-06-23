---
title: Criar funções personalizadas com a CLI do Azure | Microsoft Docs
description: Saiba como criar funções personalizadas para o controlo de acesso baseado em funções (RBAC) utilizando a CLI do Azure. Isto inclui como listar, criar, atualizar e eliminar funções personalizadas.
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
ms.openlocfilehash: b1df50c73497e3f5ad64a7ba7210079871b155e0
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321158"
---
# <a name="create-custom-roles-using-azure-cli"></a>Criar funções personalizadas com a CLI do Azure

Se o [funções incorporadas](built-in-roles.md) não satisfazer as necessidades específicas da sua organização, pode criar as suas próprias funções personalizadas. Este artigo descreve como criar e gerir funções personalizadas utilizando a CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para criar funções personalizadas, tem de:

- Permissões para criar funções personalizadas, tais como [proprietário](built-in-roles.md#owner) ou [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator)
- [CLI do Azure](/cli/azure/install-azure-cli) instalado localmente

## <a name="list-custom-roles"></a>Lista de funções personalizadas

Lista de funções personalizadas que estão disponíveis para atribuição, utilize [lista de definição de função az](/cli/azure/role/definition#az-role-definition-list). Os exemplos seguintes listam todas as funções personalizadas na subscrição atual.

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
  "roleName": "My Service Reader Role",
  "type": "CustomRole"
}
{
  "roleName": "Virtual Machine Operator",
  "type": "CustomRole"
}

...
```

## <a name="create-a-custom-role"></a>Criar uma função personalizada

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

## <a name="update-a-custom-role"></a>Atualizar uma função personalizada

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

## <a name="delete-a-custom-role"></a>Eliminar uma função personalizada

Para eliminar uma função personalizada, utilize [eliminar a definição de função az](/cli/azure/role/definition#az-role-definition-delete). Para especificar a role para eliminar, utilize o nome da função ou o ID de função. Para determinar o ID de função, utilize [lista de definição de função az](/cli/azure/role/definition#az-role-definition-list).

```azurecli
az role definition delete --name <role_name or role_id>
```

O exemplo seguinte elimina a *operador de Máquina Virtual* função personalizada.

```azurecli
az role definition delete --name "Virtual Machine Operator"
```

## <a name="next-steps"></a>Passos Seguintes

- [Tutorial: Criar uma função personalizada utilizando a CLI do Azure](tutorial-custom-role-cli.md)
- [Funções personalizadas no Azure](custom-roles.md)
- [Operações de fornecedor de recursos do Azure Resource Manager](resource-provider-operations.md)