---
title: Compreender as definições de função no Azure RBAC | Microsoft Docs
description: Saiba mais sobre as definições de função no controlo de acesso baseado em funções (RBAC) e como definir funções personalizadas para a gestão de acesso detalhada de recursos no Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/12/2018
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: ''
ms.openlocfilehash: 7a9e257d445ff7dadfe27d1c75cde6f58a393397
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="understand-role-definitions"></a>Compreender as definições de função

Se estiver a tentar compreender como funciona uma função ou se estiver a criar os seus próprios [função personalizada](custom-roles.md), é útil compreender a forma como as funções são definidas. Este artigo descreve os detalhes das definições de funções e fornece alguns exemplos.

## <a name="role-definition-structure"></a>Estrutura de definição de função

A *definição de função* é uma coleção de permissões. Por vezes, apenas é denominado um *função*. Uma definição de função lista as operações que podem ser executadas, tais como de leitura, escrita e eliminação. -Pode também lista as operações que não não possível efetuar. Uma definição de função tem a estrutura seguinte:

```
assignableScopes []
description
id
name
permissions []
  actions []
  notActions []
roleName
roleType
type
```

Operações de são especificadas com cadeias que tem o seguinte formato:

- `Microsoft.{ProviderName}/{ChildResourceType}/{action}`

O `{action}` parte de uma cadeia de operação Especifica o tipo de operações que pode executar num tipo de recurso. Por exemplo, verá as seguintes subcadeias no `{action}`:

| Subcadeia de ação    | Descrição         |
| ------------------- | ------------------- |
| `*` | O caráter universal concede acesso a todas as operações que corresponde à cadeia. |
| `read` | Permite operações (GET) de leitura. |
| `write` | Permite operações (PUT, POST e PATCH) de escrita. |
| `delete` | Permite elimina (DELETE) de operações. |

Eis o [contribuinte](built-in-roles.md#contributor) definição de função no formato JSON. O caráter universal (`*`) operação em `actions` indica que o principal atribuído a esta função pode executar todas as ações ou por outras palavras, pode gerir tudo. Isto inclui ações definidas no futuro, como o Azure adiciona novos tipos de recursos. As operações em `notActions` são subtraído `actions`. Em case do [contribuinte](built-in-roles.md#contributor) função, `notActions` remove a capacidade desta função de gerir o acesso a recursos e atribuir também acesso a recursos.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

## <a name="management-operations"></a>Operações de gestão

Controlo de acesso baseado em funções para operações de gestão especificado no `actions` e `notActions` secções de uma definição de função. Seguem-se alguns exemplos de operações de gestão no Azure:

- Gerir o acesso a uma conta de armazenamento
- Criar, atualizar ou eliminar um contentor do blob
- Eliminar um grupo de recursos e todos os respetivos recursos

Acesso de gestão não é herdado aos seus dados. Esta separação impede funções com carateres universais (`*`) de ter acesso sem restrições aos seus dados. Por exemplo, se um utilizador tiver um [leitor](built-in-roles.md#reader) função em subscrições, em seguida, podem visualizar a conta de armazenamento, mas não é possível ver os dados subjacentes.

## <a name="actions"></a>ações

O `actions` permissão Especifica as operações de gestão ao qual a função concede acesso. É uma coleção de cadeias de operação que identificam as operações com capacidade de segurança de fornecedores de recursos do Azure. Seguem-se alguns exemplos de operações de gestão que podem ser utilizados em `actions`.

| Cadeia de operação    | Descrição         |
| ------------------- | ------------------- |
| `*/read` | concede acesso de leitura de operações para todos os tipos de recursos de todos os fornecedores de recursos do Azure.|
| `Microsoft.Compute/*` | concede acesso a todas as operações para todos os tipos de recurso no fornecedor de recursos Microsoft. Compute.|
| `Microsoft.Network/*/read` | Concede acesso de leitura de operações para todos os tipos de recurso no fornecedor de recursos de Network.|
| `Microsoft.Compute/virtualMachines/*` | concede acesso a todas as operações de máquinas virtuais e respetivos subordinados tipos de recursos.|
| `microsoft.web/sites/restart/Action` | Concede acesso ao reiniciar uma aplicação web.|

## <a name="notactions"></a>NotActions

O `notActions` permissão Especifica as operações de gestão que são excluídas da permitidos `actions`. Utilize o `notActions` permissão se o conjunto de operações que pretende permitir mais facilmente é definido excluindo operações restritas. O acesso concedido por uma função (permissões efetivas) é calculado subtraindo o `notActions` operações a partir do `actions` operações.

> [!NOTE]
> Se um utilizador é atribuído uma função que exclui a gravação de uma operação em `notActions`e é atribuído uma segunda função concede acesso à mesma operação, o utilizador tem permissão para executar essa operação. `notActions` Não é uma negação regra – é simplesmente uma maneira conveniente para criar um conjunto de operações permitidas quando precisam de operações específicas a serem excluídos.
>

## <a name="assignablescopes"></a>AssignableScopes

O `assignableScopes` secção especifica os âmbitos (grupos de gestão (atualmente em pré-visualização), as subscrições, grupos de recursos ou recursos) se a função está disponível para atribuição. Pode efetuar a função disponíveis para atribuição no apenas as subscrições ou grupos de recursos que necessitam e não o utilizador clutter experiência para o resto do subscrições ou grupos de recursos. Tem de utilizar a gestão pelo menos um grupo, o subscrição, o grupo de recursos ou o ID de recurso.

Tem de funções incorporadas `assignableScopes` definido para o âmbito de raiz (`"/"`). O âmbito de raiz indica que a função está disponível para atribuição de todos os âmbitos. Não é possível utilizar o âmbito de raiz nas suas próprias funções personalizadas. Se tentar, obterá um erro de autorização.

Os exemplos de âmbitos atribuíveis válidos incluem:

| Cenário | Exemplo |
|----------|---------|
| Função está disponível para atribuição de uma única subscrição | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| Função está disponível para atribuição em duas subscrições | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| Função está disponível para atribuição apenas no grupo de recursos de rede | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| Função está disponível para atribuição de todos os âmbitos | `"/"` |

## <a name="assignablescopes-and-custom-roles"></a>assignableScopes e funções personalizadas

O `assignableScopes` secção para uma função personalizada também controla quem pode criar, eliminar, modificar ou ver a função personalizada.

| Tarefa | Operação | Descrição |
| --- | --- | --- |
| Criar/eliminar uma função personalizada | `Microsoft.Authorization/ roleDefinition/write` | Os utilizadores que recebem esta operação em todos os o `assignableScopes` da função personalizada pode criar (ou eliminar) funções personalizadas para utilização nesses âmbitos. Por exemplo, [proprietários](built-in-roles.md#owner) e [administradores do acesso de utilizador](built-in-roles.md#user-access-administrator) de subscrições, grupos de recursos e recursos. |
| Modificar uma função personalizada | `Microsoft.Authorization/ roleDefinition/write` | Os utilizadores que recebem esta operação em todos os o `assignableScopes` da função personalizada, pode modificar as funções personalizadas nesses âmbitos. Por exemplo, [proprietários](built-in-roles.md#owner) e [administradores do acesso de utilizador](built-in-roles.md#user-access-administrator) de subscrições, grupos de recursos e recursos. |
| Ver uma função personalizada | `Microsoft.Authorization/ roleDefinition/read` | Os utilizadores que recebem esta operação num âmbito podem ver as funções personalizadas que estão disponíveis para atribuição a esse âmbito. Todas as funções incorporadas permitem funções personalizadas estar disponível para atribuição. |

## <a name="role-definition-examples"></a>Exemplos de definição de função

O seguinte exemplo mostra o [leitor](built-in-roles.md#reader) definição de função, tal como apresentado utilizando a CLI do Azure:

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
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

O exemplo seguinte mostra uma função personalizada de monitorização e reiniciar as máquinas virtuais como apresentado com o Azure PowerShell:

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
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

## <a name="see-also"></a>Consulte também

* [Funções incorporadas](built-in-roles.md)
* [Funções personalizadas](custom-roles.md)
* [Operações de fornecedor de recursos do Azure Resource Manager](resource-provider-operations.md)
