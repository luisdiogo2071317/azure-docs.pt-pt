---
title: Criar funções personalizadas para RBAC do Azure | Microsoft Docs
description: Saiba como definir funções personalizadas com controlo de acesso em funções do Azure mais precisa para gestão de identidades na sua subscrição do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: e4206ea9-52c3-47ee-af29-f6eef7566fa5
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/12/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9e2ea46ea1a6b5bd3f50d4d4c15492c16c5241c0
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="create-custom-roles-in-azure"></a>Criar funções personalizadas no Azure

Se o [funções incorporadas](built-in-roles.md) não satisfazer as suas necessidades de acesso específicas, pode criar as suas próprias funções personalizadas. Tal como funções incorporadas, pode atribuir funções personalizadas para os utilizadores, grupos e principais de serviço na subscrição, o grupo de recursos e âmbitos de recursos. Funções personalizadas são armazenadas num inquilino do Azure Active Directory (Azure AD) e podem ser partilhadas entre subscrições. Funções personalizadas podem ser criadas utilizando o Azure PowerShell, a CLI do Azure ou a API REST. Este artigo descreve um exemplo de como começar a criar funções personalizadas com o PowerShell e da CLI do Azure.

## <a name="create-a-custom-role-to-open-support-requests-using-powershell"></a>Criar uma função personalizada para abrir os pedidos de suporte utilizando o PowerShell

Para criar uma função personalizada, pode começar com uma função incorporada, editá-lo e, em seguida, crie uma nova função. Neste exemplo, incorporada [leitor](built-in-roles.md#reader) função personalizada para criar uma função personalizada com o nome "nível de acesso de pedidos de suporte de leitor". Permite ao utilizador para ver tudo na subscrição e também pedidos de suporte aberta.

> [!NOTE]
> As funções incorporadas apenas dois que permitir que um utilizador abrir a pedidos de suporte são [proprietário](built-in-roles.md#owner) e [contribuinte](built-in-roles.md#contributor). Para um utilizador poderá abrir os pedidos de suporte, ele tem de ser atribuído uma função no âmbito de subscrição, porque todos os pedidos de suporte são criados com base numa subscrição do Azure.

No PowerShell, utilize o [Get-AzureRmRoleDefinition](/powershell/module/azurerm.resources/get-azurermroledefinition) comando para exportar o [leitor](built-in-roles.md#reader) função no formato JSON.

```azurepowershell
Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File C:\rbacrole2.json
```

O seguinte mostra a saída JSON para o [leitor](built-in-roles.md#reader) função. Uma função de típica é composta por três secções principais, `Actions`, `NotActions`, e `AssignableScopes`. O `Actions` secção apresenta uma lista de todas as operações permitidas para a função. Para excluir operações a partir de `Actions`, pode adicioná-los ao `NotActions`. As permissões efetivas é calculada subtraindo o `NotActions` operações a partir de `Actions` operações.

```json
{
    "Name":  "Reader",
    "Id":  "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "IsCustom":  false,
    "Description":  "Lets you view everything, but not make any changes.",
    "Actions":  [
                    "*/read"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/"
                         ]
}
```

Em seguida, edite o JSON de saída para criar a sua função personalizada. Neste caso criar o suporte de permissões, o `Microsoft.Support/*` operação tem de ser adicionada. Cada operação é efetuada disponível a partir de um fornecedor de recursos. Para obter uma lista de operações para um fornecedor de recursos, pode utilizar o [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) comando ou consulte [operações de fornecedor de recursos do Azure Resource Manager](resource-provider-operations.md).

É obrigatório que a função contém a subscrição explícita IDs em que é utilizado. Os IDs de subscrição estão listados na `AssignableScopes`, caso contrário, não serão permitidas para importar a função para a sua subscrição.

Por fim, tem de definir o `IsCustom` propriedade `true` para especificar que se trata de uma função personalizada.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes":  [
                             "/subscriptions/11111111-1111-1111-1111-111111111111"
                         ]
}
```

Para criar a nova função personalizada, utilize o [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) de comandos e disponibilize o ficheiro de definição de função JSON atualizado.

```azurepowershell
New-AzureRmRoleDefinition -InputFile "C:\rbacrole2.json"
```

Após a execução [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition), a nova função personalizada está disponível no portal do Azure e podem ser atribuída a utilizadores.

![captura de ecrã da função personalizada importada no portal do Azure](./media/custom-roles/18.png)

![captura de ecrã de atribuição de função importada personalizada ao utilizador no mesmo diretório](./media/custom-roles/19.png)

![captura de ecrã de permissões de função importada personalizada](./media/custom-roles/20.png)

Os utilizadores com esta função personalizada, podem criar novos pedidos de suporte.

![captura de ecrã da função personalizada criar pedidos de suporte](./media/custom-roles/21.png)

Os utilizadores com esta função personalizada não é possível efetuar outras ações, tais como criar VMs ou criar grupos de recursos.

![captura de ecrã da função personalizada não é possível criar VMs](./media/custom-roles/22.png)

![captura de ecrã da função personalizada não é possível criar o novo RGs](./media/custom-roles/23.png)

## <a name="create-a-custom-role-to-open-support-requests-using-azure-cli"></a>Criar uma função personalizada para abrir os pedidos de suporte utilizando a CLI do Azure

Os passos para criar uma função personalizada utilizando a CLI do Azure são semelhantes ao utilizar o PowerShell, exceto que a saída JSON é diferente.

Neste exemplo, pode iniciar com incorporada [leitor](built-in-roles.md#reader) função. Para listar as ações do [leitor](built-in-roles.md#reader) função, utilize o [lista de definição de função az](/cli/azure/role/definition#az_role_definition_list) comando.

```azurecli
az role definition list --name "Reader" --output json
```

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "notActions": [],
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Crie um ficheiro JSON com o seguinte formato. O `Microsoft.Support/*` operação tenha sido adicionada no `Actions` secções para que este utilizador pode abrir pedidos de suporte ao continuar a ser um leitor. Tem de adicionar o ID de subscrição em que esta função será utilizada o `AssignableScopes` secção.

```json
{
    "Name":  "Reader support tickets access level",
    "IsCustom":  true,
    "Description":  "View everything in the subscription and also open support requests.",
    "Actions":  [
                    "*/read",
                    "Microsoft.Support/*"
                ],
    "NotActions":  [

                   ],
    "AssignableScopes": [
                            "/subscriptions/11111111-1111-1111-1111-111111111111"
                        ]
}
```

Para criar a nova função personalizada, utilize o [Criar definição de função az](/cli/azure/role/definition#az_role_definition_create) comando.

```azurecli
az role definition create --role-definition ~/roles/rbacrole1.json
```

A nova função personalizada está agora disponível no portal do Azure e o processo para utilizar esta função é o mesmo que a secção anterior do PowerShell.

![Captura de ecrã portal do Azure de função personalizada que criou com a CLI 1.0](./media/custom-roles/26.png)


## <a name="see-also"></a>Consulte também
- [Compreender as definições de função](role-definitions.md)
- [Gerir o controlo de acesso baseado em funções com AzurePowerShell](role-assignments-powershell.md)
- [Gerir o controlo de acesso baseado em funções com a CLI do Azure](role-assignments-cli.md)
- [Gerir o controlo de acesso baseado em funções com a API REST](role-assignments-rest.md)
