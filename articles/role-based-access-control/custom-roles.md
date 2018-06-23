---
title: Funções personalizadas no Azure | Microsoft Docs
description: Saiba como definir funções personalizadas com controlo de acesso do Azure baseada em funções (RBAC) para a gestão de acesso detalhada de recursos no Azure.
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
ms.date: 06/12/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 074c305cb15bc1fb25dfa5cfc52dcce53b661a7e
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321179"
---
# <a name="custom-roles-in-azure"></a>Funções personalizadas no Azure

Se o [funções incorporadas](built-in-roles.md) não satisfazer as necessidades específicas da sua organização, pode criar as suas próprias funções personalizadas. Tal como funções incorporadas, pode atribuir funções personalizadas para os utilizadores, grupos e principais de serviço na subscrição, o grupo de recursos e âmbitos de recursos. Funções personalizadas são armazenadas num inquilino do Azure Active Directory (Azure AD) e podem ser partilhadas entre subscrições. Cada inquilino pode ter até 2000 de funções personalizadas. Funções personalizadas podem ser criadas utilizando o Azure PowerShell, a CLI do Azure ou a API REST.

## <a name="custom-role-example"></a>Exemplo de função personalizada

O seguinte mostra uma função personalizada de monitorização e reiniciar as máquinas virtuais como apresentado com o Azure PowerShell:

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
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
  "NotActions":  [

                 ],
  "DataActions":  [

                  ],
  "NotDataActions":  [

                     ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

Depois de criar uma função personalizada, é apresentado no portal do Azure com um ícone de laranja recursos.

![Ícone de função personalizada](./media/custom-roles/roles-custom-role-icon.png)

## <a name="steps-to-create-a-custom-role"></a>Passos para criar uma função personalizada

1. Determinar as permissões que precisa

    Quando cria uma função personalizada, terá de saber o recurso de operações de fornecedor que estão disponíveis para definir as suas permissões. Para ver a lista de operações, pode utilizar o [Get-AzureRMProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation) ou [a lista de operações de fornecedor de az](/cli/azure/provider/operation#az-provider-operation-list) comandos.
    Para especificar as permissões para a função personalizada, adicione as operações de `actions` ou `notActions` propriedades do [definição de função](role-definitions.md). Se tiver de operações de dados, adicione os para o `dataActions` ou `notDataActions` propriedades.

2. Criar a função personalizada

    Pode utilizar o Azure PowerShell ou a CLI do Azure para criar a função personalizada. Normalmente, comece com uma função incorporada existente e, em seguida, modificá-lo para as suas necessidades. Utilizar o [New-AzureRmRoleDefinition](/powershell/module/azurerm.resources/new-azurermroledefinition) ou [Criar definição de função az](/cli/azure/role/definition#az-role-definition-create) comandos para criar a função personalizada. Para criar uma função personalizada, tem de ter o `Microsoft.Authorization/roleDefinitions/write` permissão em todos os `assignableScopes`, tais como [proprietário](built-in-roles.md#owner) ou [administrador de acesso de utilizador](built-in-roles.md#user-access-administrator).

3. Testar a função personalizada

    Assim que tiver a função personalizada, terá de testá-lo para verificar se funciona como esperado. Se ajustes tem de ser efetuada, pode atualizar a função personalizada.

## <a name="custom-role-properties"></a>Propriedades de função personalizada

Uma função personalizada tem as seguintes propriedades.

| Propriedade | Necessário | Tipo | Descrição |
| --- | --- | --- | --- |
| `Name` | Sim | Cadeia | O nome a apresentar da função personalizada. Tem de ser exclusivo no seu inquilino. Pode incluir letras, números, espaços e carateres especiais. Número máximo de carateres é 128. |
| `Id` | Sim | Cadeia | O ID exclusivo da função personalizada. Para o Azure PowerShell e a CLI do Azure, este ID é gerado automaticamente quando criar uma nova função. |
| `IsCustom` | Sim | Cadeia | Indica se se trata de uma função personalizada. Definido como `true` para funções personalizadas. |
| `Description` | Sim | Cadeia | A descrição da função personalizada. Pode incluir letras, números, espaços e carateres especiais. Número máximo de carateres é 1024. |
| `Actions` | Sim | String] | Uma matriz de cadeias que especifica as operações de gestão que permite que a função para ser executada. Para obter mais informações, consulte [ações](role-definitions.md#actions). |
| `NotActions` | Não | String] | Uma matriz de cadeias que especifica as operações de gestão que são excluídas da permitidos `actions`. Para obter mais informações, consulte [notActions](role-definitions.md#notactions). |
| `DataActions` | Não | String] | Uma matriz de cadeias que especifica as operações de dados que permite que a função ser efetuadas aos seus dados dentro desse objeto. Para obter mais informações, consulte [dataActions (pré-visualização)](role-definitions.md#dataactions-preview). |
| `NotDataActions` | Não | String] | Uma matriz de cadeias que especifica as operações de dados que são excluídas da permitidos `dataActions`. Para obter mais informações, consulte [notDataActions (pré-visualização)](role-definitions.md#notdataactions-preview). |
| `AssignableScopes` | Sim | String] | Uma matriz de cadeias que especifica os âmbitos que a função personalizada está disponível para atribuição. Não é possível definir âmbito de raiz (`"/"`). Para obter mais informações, consulte [assignableScopes](role-definitions.md#assignablescopes). |

## <a name="assignablescopes-for-custom-roles"></a>assignableScopes para funções personalizadas

Tal como funções incorporadas, a `assignableScopes` propriedade especifica os âmbitos que a função está disponível para atribuição. No entanto, não é possível utilizar o âmbito de raiz (`"/"`) na suas própria funções personalizadas. Se tentar, obterá um erro de autorização. O `assignableScopes` propriedade para uma função personalizada também controla quem pode criar, eliminar, modificar ou ver a função personalizada.

| Tarefa | Operação | Descrição |
| --- | --- | --- |
| Criar/eliminar uma função personalizada | `Microsoft.Authorization/ roleDefinition/write` | Os utilizadores que recebem esta operação em todos os o `assignableScopes` da função personalizada pode criar (ou eliminar) funções personalizadas para utilização nesses âmbitos. Por exemplo, [proprietários](built-in-roles.md#owner) e [administradores do acesso de utilizador](built-in-roles.md#user-access-administrator) de subscrições, grupos de recursos e recursos. |
| Modificar uma função personalizada | `Microsoft.Authorization/ roleDefinition/write` | Os utilizadores que recebem esta operação em todos os o `assignableScopes` da função personalizada, pode modificar as funções personalizadas nesses âmbitos. Por exemplo, [proprietários](built-in-roles.md#owner) e [administradores do acesso de utilizador](built-in-roles.md#user-access-administrator) de subscrições, grupos de recursos e recursos. |
| Ver uma função personalizada | `Microsoft.Authorization/ roleDefinition/read` | Os utilizadores que recebem esta operação num âmbito podem ver as funções personalizadas que estão disponíveis para atribuição a esse âmbito. Todas as funções incorporadas permitem funções personalizadas estar disponível para atribuição. |

## <a name="next-steps"></a>Passos Seguintes
- [Criar funções personalizadas com o Azure PowerShell](custom-roles-powershell.md)
- [Criar funções personalizadas com a CLI do Azure](custom-roles-cli.md)
- [Compreender as definições de função](role-definitions.md)
