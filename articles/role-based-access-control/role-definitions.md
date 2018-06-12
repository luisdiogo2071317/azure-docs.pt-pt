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
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 9bb7808f2b483fe9cd7d22c6df3fe80d4a98f1f4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266861"
---
# <a name="understand-role-definitions"></a>Compreender as definições de função

Se estiver a tentar compreender como funciona uma função ou se estiver a criar os seus próprios [função personalizada](custom-roles.md), é útil compreender a forma como as funções são definidas. Este artigo descreve os detalhes das definições de funções e fornece alguns exemplos.

## <a name="role-definition-structure"></a>Estrutura de definição de função

A *definição de função* é uma coleção de permissões. Por vezes, apenas é denominado um *função*. Uma definição de função lista as operações que podem ser executadas, tais como de leitura, escrita e eliminação. -Pode também uma lista de operações que não não possível efetuar ou operações relacionadas com a dados subjacentes. Uma definição de função tem a estrutura seguinte:

```
assignableScopes []
description
id
name
permissions []
  actions []
  dataActions []
  notActions []
  notDataActions []
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

## <a name="management-and-data-operations-preview"></a>Operações de gestão e os dados (pré-visualização)

Controlo de acesso baseado em funções para operações de gestão especificado no `actions` e `notActions` secções de uma definição de função. Seguem-se alguns exemplos de operações de gestão no Azure:

- Gerir o acesso a uma conta de armazenamento
- Criar, atualizar ou eliminar um contentor do blob
- Eliminar um grupo de recursos e todos os respetivos recursos

Acesso de gestão não é herdado aos seus dados. Esta separação impede funções com carateres universais (`*`) de ter acesso sem restrições aos seus dados. Por exemplo, se um utilizador tiver um [leitor](built-in-roles.md#reader) função em subscrições, em seguida, podem visualizar a conta de armazenamento, mas por predefinição, não é possível ver os dados subjacentes.

Anteriormente, o controlo de acesso baseado em funções não foi utilizado para operações de dados. Diversificados autorização para operações de dados através de fornecedores de recursos. O mesmo modelo de autorização de controlo baseado na função de acesso utilizado para operações de gestão tiver sido expandido para operações de dados (atualmente em pré-visualização).

Para suportar operações de dados, foram adicionadas duas novas secções dados para a estrutura de definição de função. Operações de dados estão especificadas no `dataActions` e `notDataActions` secções. Ao adicionar estas secções de dados, a separação entre a gestão e de dados é mantida. Isto impede que as atribuições de função atual com carateres universais (`*`) de subitamente ter acesso a dados. Seguem-se algumas operações de dados que podem ser especificadas em `dataActions` e `notDataActions`:

- Ler uma lista de blobs num contentor
- Escrever um blob de armazenamento num contentor
- Eliminar uma mensagem numa fila

Eis o [leitor de dados de BLOBs de armazenamento (pré-visualização)](built-in-roles.md#storage-blob-data-reader-preview) definição de função, o que inclui operações em ambos os `actions` e `dataActions` secções. Esta função permite-lhe ler o contentor de blob e também os dados de blob subjacente.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Allows for read access to Azure Storage blob containers and data.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/read"
        ],
        "additionalProperties": {},
        "dataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
        ],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Storage Blob Data Reader (Preview)",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Operações de dados só podem ser adicionadas para o `dataActions` e `notDataActions` secções. Fornecedores de recursos de identificar que operações operações de dados, definindo o `isDataAction` propriedade `true`. Para ver uma lista das operações onde `isDataAction` é `true`, consulte [operações de fornecedor de recursos](resource-provider-operations.md). Funções que não dispõe de operações de dados não são necessários para ter `dataActions` e `notDataActions` secções na definição de função.

Autorização para todas as chamadas de API de operação de gestão é processada pelo Azure Resource Manager. Autorização para chamadas de API de operação de dados é processada por um fornecedor de recursos ou o Azure Resource Manager.

### <a name="data-operations-example"></a>Exemplo de operações de dados

Para melhor compreender como funcionam as operações de gestão e os dados, vejamos um exemplo específico. Alice foi atribuída o [proprietário](built-in-roles.md#owner) função no âmbito de subscrição. Bernardo foi atribuído o [contribuinte de dados de BLOBs de armazenamento (pré-visualização)](built-in-roles.md#storage-blob-data-contributor-preview) função num âmbito da conta de armazenamento. O diagrama seguinte mostra este exemplo.

![Controlo de acesso baseado em funções tiver sido expandido para suportar a gestão e operações de dados](./media/role-definitions/rbac-management-data.png)

O [proprietário](built-in-roles.md#owner) função para Alice e [contribuinte de dados de BLOBs de armazenamento (pré-visualização)](built-in-roles.md#storage-blob-data-contributor-preview) função para Bernardo tem as seguintes ações:

Proprietário

&nbsp;&nbsp;&nbsp;&nbsp;ações<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Contribuinte de Dados do Armazenamento de Blobs (Pré-visualização)

&nbsp;&nbsp;&nbsp;&nbsp;ações<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;DataActions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Uma vez que Alice tem um caráter universal (`*`) ação num âmbito de subscrição, obtenção permissões herdam para baixo para permitir-lhe efetuar todas as ações de gestão. No entanto, a Adriana não é possível efetuar operações de dados. Por exemplo, por predefinição, Alice não é possível ler os blobs no interior de um contentor, mas ela pode ler, escrever e eliminar contentores.

Permissões do de Bob estão limitadas a apenas o `actions` e `dataActions` especificado no [contribuinte de dados de BLOBs de armazenamento (pré-visualização)](built-in-roles.md#storage-blob-data-contributor-preview) função. Com base na função, João pode efetuar a gestão e operações de dados. Por exemplo, pode ler, escrever e eliminar contentores na conta de armazenamento especificado Bernardo e também ele pode ler, escrever e eliminar os blobs.

### <a name="what-tools-support-using-rbac-for-data-operations"></a>As ferramentas de suportem utilizando o RBAC para operações de dados?

Para visualizar e trabalhar com operações de dados, tem de ter as versões corretas dos SDKs ou ferramentas:

| Ferramenta  | Versão  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-azurerm-ps) | 5.6.0 ou posterior |
| [CLI do Azure](/cli/azure/install-azure-cli) | 2.0.30 ou posterior |
| [Azure para .NET](/dotnet/azure/) | 2.8.0-Preview ou posterior |
| [Azure SDK para ir](/go/azure/azure-sdk-go-install) | 15.0.0 ou posterior |
| [Azure para Java](/java/azure/) | 1.9.0 ou posterior |
| [Azure para Python](/python/azure) | 0.40.0 ou posterior |
| [Azure SDK for Ruby](https://rubygems.org/gems/azure_sdk) (Azure SDK para Ruby) | 0.17.1 ou posterior |

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

## <a name="dataactions-preview"></a>dataActions (pré-visualização)

O `dataActions` permissão Especifica as operações de dados para os quais a função conceda acesso aos seus dados dentro desse objeto. Por exemplo, se um utilizador tem acesso de dados de BLOBs para uma conta de armazenamento de leitura, em seguida, podem ler os blobs dentro dessa conta de armazenamento. Seguem-se alguns exemplos de operações de dados que podem ser utilizados em `dataActions`.

| Cadeia de operação    | Descrição         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Devolve um blob ou uma lista de blobs. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Devolve o resultado de escrever um blob. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Devolve uma mensagem. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Devolve uma mensagem ou o resultado de escrever ou eliminar uma mensagem. |

## <a name="notdataactions-preview"></a>notDataActions (pré-visualização)

O `notDataActions` permissão Especifica as operações de dados que são excluídas da permitidos `dataActions`. O acesso concedido por uma função (permissões efetivas) é calculado subtraindo o `notDataActions` operações a partir do `dataActions` operações. Cada fornecedor de recursos fornece o respetivo conjunto de APIs para realizar operações de dados.

> [!NOTE]
> Se um utilizador é atribuído uma função que exclui a gravação de uma operação de dados no `notDataActions`e é atribuído uma segunda função concede acesso à mesma operação de dados, o utilizador tem permissão para efetuar essa operação de dados. `notDataActions` Não é uma negação regra – é simplesmente uma maneira conveniente para criar um conjunto de dados permitido operações quando precisam de operações de dados específicas a serem excluídos.
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
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
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

## <a name="see-also"></a>Consulte também

* [Funções incorporadas](built-in-roles.md)
* [Funções personalizadas](custom-roles.md)
* [Operações de fornecedor de recursos do Azure Resource Manager](resource-provider-operations.md)
