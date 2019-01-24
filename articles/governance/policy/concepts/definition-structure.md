---
title: Detalhes da estrutura de definição de política
description: Descreve como a definição de política de recurso do Azure Policy é utilizada para estabelecer as convenções para recursos na sua organização com a descrição quando a política é imposta e o efeito resultante para tirar.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 0fe15cc87e0d30f58dc26ae925efa6d65b243f5b
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2019
ms.locfileid: "54851662"
---
# <a name="azure-policy-definition-structure"></a>Estrutura de definição do Azure Policy

Definições de política de recursos são utilizadas pelo Azure Policy para estabelecer as convenções de recursos. Cada definição descreve o que afetar a tomar quando um recurso está em conformidade e de conformidade de recursos.
Ao definir as convenções, pode controlar os custos e gerir mais facilmente os seus recursos. Por exemplo, pode especificar que apenas determinados tipos de máquinas virtuais são permitidos. Em alternativa, pode exigir que todos os recursos tenham uma etiqueta específica. As políticas são herdadas por todos os recursos subordinados. Se uma política é aplicada a um grupo de recursos, é aplicável a todos os recursos nesse grupo de recursos.

O esquema utilizado pela política do Azure pode ser encontrado aqui: [https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json](https://schema.management.azure.com/schemas/2018-05-01/policyDefinition.json)

Utilizar o JSON para criar uma definição de política. A definição de política contém elementos para:

- mode
- parâmetros
- Nome a apresentar
- descrição
- regra de política
  - avaliação de lógica
  - em vigor

Por exemplo, o JSON seguinte mostra uma política que limita a onde os recursos são implementados:

```json
{
    "properties": {
        "mode": "all",
        "parameters": {
            "allowedLocations": {
                "type": "array",
                "metadata": {
                    "description": "The list of locations that can be specified when deploying resources",
                    "strongType": "location",
                    "displayName": "Allowed locations"
                }
            }
        },
        "displayName": "Allowed locations",
        "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
        "policyRule": {
            "if": {
                "not": {
                    "field": "location",
                    "in": "[parameters('allowedLocations')]"
                }
            },
            "then": {
                "effect": "deny"
            }
        }
    }
}
```

Todos os exemplos do Azure Policy correm [exemplos de política](../samples/index.md).

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="mode"></a>Modo

O **modo** determina quais tipos de recursos serão avaliados para uma política. Os modos suportados são:

- `all`: avaliar a grupos de recursos e todos os tipos de recursos
- `indexed`: avaliar apenas tipos de recursos que oferecem suporte a marcas e localização

Recomendamos que defina **modo** para `all` na maioria dos casos. Todas as definições de política criadas através da utilização de portal a `all` modo. Se utilizar o PowerShell ou da CLI do Azure, pode especificar a **modo** parâmetro manualmente. Se a definição de política não inclui um **modo** valor, assume como predefinição `all` no Azure PowerShell e, a `null` na CLI do Azure. R `null` modo de é igual a utilizar `indexed` para suportar a compatibilidade com versões anteriores.

`indexed` deve ser usado durante a criação de políticas que aplicar etiquetas ou localizações. Embora seja necessário nota, impede que os recursos que não suportam etiquetas e localizações de aparecer como não conformes nos resultados de compatibilidade. A exceção é **grupos de recursos**. Devem definir políticas que impõem a localização ou etiquetas num grupo de recursos **modo** ao `all` e o destino especificamente o `Microsoft.Resources/subscriptions/resourceGroup` tipo. Por exemplo, veja [impor etiquetas do grupo de recursos](../samples/enforce-tag-rg.md).

## <a name="parameters"></a>Parâmetros

Parâmetros ajudam a simplificar a gestão de políticas ao reduzir o número de definições de política. Pense em parâmetros como os campos num formulário – `name`, `address`, `city`, `state`. Esses parâmetros são sempre os mesmos, no entanto alterar seus valores com base em individuais a preencher o formulário.
Parâmetros funcionam da mesma forma, na criação de políticas. Ao incluir parâmetros na definição de política, pode reutilizar essa política para diferentes cenários com valores diferentes.

> [!NOTE]
> A definição de parâmetros para uma política ou a definição de iniciativa apenas pode ser configurada durante a criação inicial da política ou iniciativa. A definição de parâmetros não é possível alterar mais tarde.
> Isto impede que existente atribuições da política ou iniciativa indiretamente que estão sendo feitas inválido.

Por exemplo, pode definir uma política para limitar as localizações onde os recursos podem ser implementados.
Poderia declarar os seguintes parâmetros ao criar a política:

```json
"parameters": {
    "allowedLocations": {
        "type": "array",
        "metadata": {
            "description": "The list of allowed locations for resources.",
            "displayName": "Allowed locations",
            "strongType": "location"
        }
    }
}
```

O tipo de um parâmetro pode ser a cadeia ou matriz. A propriedade de metadados é utilizada para ferramentas como o portal do Azure para apresentar informações amigável de utilizador.

A propriedade de metadados, pode utilizar **strongType** para fornecer uma lista de seleção múltipla de opções no portal do Azure. Valores para permitidos **strongType** atualmente incluem:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

A regra de política, fazer referência a parâmetros com o seguinte `parameters` sintaxe de função de valor de implementação:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Localização da definição

Ao criar uma política ou iniciativa, é necessário especificar a localização da definição. A localização da definição tem de ser um grupo de gestão ou de uma subscrição. Esta localização determina o âmbito para o qual pode ser atribuída a política ou iniciativa. Recursos tem de ser membros diretos dos ou elementos subordinados na hierarquia da localização da definição de destino para a atribuição.

Se a localização da definição é r:

- **Subscrição** – apenas recursos nessa subscrição podem ser atribuídos a política.
- **Grupo de gestão** – apenas recursos dentro de grupos de gestão de subordinados e subscrições de subordinados podem ser atribuídos a política. Se planeia aplicar a definição de política para várias subscrições, a localização tem de ser um grupo de gestão que contém nessas subscrições.

## <a name="display-name-and-description"></a>Nome a apresentar e descrição

Utilizar **displayName** e **Descrição** para identificar a definição de política e fornecer contexto para quando é utilizado.

## <a name="policy-rule"></a>regra de política

A regra de política é composta por **se** e **, em seguida,** blocos. Na **se** bloco, define uma ou mais condições que especificar quando a política é imposta. Pode aplicar operadores lógicos para estas condições para definir exatamente o cenário para uma política.

Na **, em seguida,** bloco, define o efeito que acontece quando o **se** condições são cumpridas.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists | disabled"
    }
}
```

### <a name="logical-operators"></a>Operadores lógicos

Operadores lógicos suportados são:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

O **não** sintaxe inverts o resultado da condição. O **tudo** sintaxe (semelhante ao lógico **e** operação) requer que todas as condições como true. O **anyOf** sintaxe (semelhante ao lógico **ou** operação) requer um ou mais condições como true.

Pode aninhar operadores lógicos. A exemplo a seguir mostra um **não** operação que esteja aninhada dentro de um **tudo** operação.

```json
"if": {
    "allOf": [{
            "not": {
                "field": "tags",
                "containsKey": "application"
            }
        },
        {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
        }
    ]
},
```

### <a name="conditions"></a>Condições

A condição for avaliada se um **campo** atende a certos critérios. As condições suportadas são:

- `"equals": "value"`
- `"notEquals": "value"`
- `"like": "value"`
- `"notLike": "value"`
- `"match": "value"`
- `"notMatch": "value"`
- `"contains": "value"`
- `"notContains": "value"`
- `"in": ["value1","value2"]`
- `"notIn": ["value1","value2"]`
- `"containsKey": "keyName"`
- `"notContainsKey": "keyName"`
- `"exists": "bool"`

Ao utilizar o **, como** e **notLike** condições, que fornece um caráter universal `*` no valor.
O valor não deve ter mais de um caráter universal `*`.

Ao utilizar o **corresponder** e **notMatch** condições, fornecer `#` de acordo com um dígito, `?` para uma letra, `.` de acordo com todos os caracteres e qualquer outro caractere para corresponder ao esse caractere real. Para obter exemplos, consulte [permitir que vários padrões de nome](../samples/allow-multiple-name-patterns.md).

### <a name="fields"></a>Campos

Condições são formadas com campos. Um campo corresponde a propriedades no payload de pedido de recurso e descreve o estado do recurso.

São suportados os seguintes campos:

- `name`
- `fullName`
  - Devolve o nome completo do recurso. O nome completo de um recurso é o nome de recurso anexado por nenhum nome de recurso principal (por exemplo "myServer/myDatabase").
- `kind`
- `type`
- `location`
  - Uso **global** para recursos que desconhecem de localização. Por exemplo, veja [exemplos - localizações permitidos](../samples/allowed-locations.md).
- `identity.type`
  - Devolve o tipo de [identidade gerido](../../../active-directory/managed-identities-azure-resources/overview.md) ativada no recurso.
- `tags`
- `tags.<tagName>`
  - Em que **\<tagName\>** é o nome da etiqueta para validar a condição para.
  - Exemplo: `tags.CostCenter` em que **Centrodecustos** é o nome da etiqueta.
- `tags[<tagName>]`
  - Essa sintaxe de colchete suporta nomes de etiqueta têm um período.
  - Em que **\<tagName\>** é o nome da etiqueta para validar a condição para.
  - Exemplo: `tags[Acct.CostCenter]` em que **Acct.CostCenter** é o nome da etiqueta.
- aliases de propriedade - para obter uma lista, consulte [Aliases](#aliases).

### <a name="effect"></a>Efeito

Política suporta os seguintes tipos de efeito:

- **Negar**: gera um evento no registo de atividades e falha do pedido
- **Auditoria**: gera um evento de aviso no registo de atividades, mas não falhar o pedido
- **Acrescentar**: Adiciona o conjunto definido de campos ao pedido
- **AuditIfNotExists**: permite a auditoria se não existir um recurso
- **DeployIfNotExists**: implementa um recurso, se ainda não exista
- **Desativado**: não avaliar os recursos de conformidade para a regra de política

Para **acrescentar**, tem de fornecer os seguintes detalhes:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

O valor pode ser uma cadeia de caracteres ou um objeto de formato JSON.

**AuditIfNotExists** e **DeployIfNotExists** avaliar a existência de um recurso relacionado e aplicar uma regra. Se o recurso não corresponder a regra, o efeito é implementado. Por exemplo, pode exigir que um observador de rede é implementado para todas as redes virtuais. Para obter mais informações, consulte a [auditar se a extensão não existe](../samples/audit-ext-not-exist.md) exemplo.

O **DeployIfNotExists** efeito requer a **roleDefinitionId** propriedade no **detalhes** parte da regra de política. Para obter mais informações, consulte [remediação - configurar a definição de política](../how-to/remediate-resources.md#configure-policy-definition).

```json
"details": {
    ...
    "roleDefinitionIds": [
        "/subscription/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/{roleGUID}",
        "/providers/Microsoft.Authorization/roleDefinitions/{builtinroleGUID}"
    ]
}
```

Para obter detalhes completos sobre cada efeito, a ordem de avaliação, propriedades e exemplos, consulte [efeitos de política de compreensão](effects.md).

### <a name="policy-functions"></a>Funções de política

Várias [funções de modelo do Resource Manager](../../../azure-resource-manager/resource-group-template-functions.md) estão disponíveis para uso dentro de uma regra de política. As funções atualmente suportadas são:

- [parameters](../../../azure-resource-manager/resource-group-template-functions-deployment.md#parameters)
- [concat](../../../azure-resource-manager/resource-group-template-functions-array.md#concat)
- [resourceGroup](../../../azure-resource-manager/resource-group-template-functions-resource.md#resourcegroup)
- [subscrição](../../../azure-resource-manager/resource-group-template-functions-resource.md#subscription)

Além disso, o `field` função está disponível para as regras de política. `field` é utilizado principalmente com **AuditIfNotExists** e **DeployIfNotExists** aos campos de referência no recurso que estão a ser avaliados. Um exemplo desta utilização pode ser visto na [DeployIfNotExists exemplo](effects.md#deployifnotexists-example).

#### <a name="policy-function-examples"></a>Exemplos de função de política

Este exemplo de regra de política utiliza o `resourceGroup` função de recursos para obter o **nome** propriedade, combinada com o `concat` matriz e objeto de função para criar um `like` condição que impõe o nome do recurso para começar com o nome do grupo de recursos.

```json
{
    "if": {
        "not": {
            "field": "name",
            "like": "[concat(resourceGroup().name,'*')]"
        }
    },
    "then": {
        "effect": "deny"
    }
}
```

Este exemplo de regra de política utiliza o `resourceGroup` função de recursos para obter o **etiquetas** valor de propriedade de matriz da **Centrodecustos** etiqueta no grupo de recursos e acrescentá-lo para o **Centrodecustos**  Etiquetar no novo recurso.

```json
{
    "if": {
        "field": "tags.CostCenter",
        "exists": "false"
    },
    "then": {
        "effect": "append",
        "details": [{
            "field": "tags.CostCenter",
            "value": "[resourceGroup().tags.CostCenter]"
        }]
    }
}
```

## <a name="aliases"></a>Aliases

Utilize aliases de propriedade para aceder às propriedades específicas para um tipo de recurso. Aliases permitem-lhe restringir quais valores ou condições são permitidas para uma propriedade num recurso. Cada alias mapeia para caminhos em diferentes versões de API para um tipo de recurso específico. Durante a avaliação da política, o mecanismo da diretiva obtém o caminho de propriedade para essa versão de API.

A lista de aliases está sempre a aumentar. Para localizar os aliases são atualmente suportadas pelo Azure Policy, utilize um dos seguintes métodos:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzAccount if not using Cloud Shell

  # Use Get-AzPolicyAlias to list available providers
  Get-AzPolicyAlias -ListAvailable

  # Use Get-AzPolicyAlias to list aliases for a Namespace (such as Azure Automation -- Microsoft.Automation)
  Get-AzPolicyAlias -NamespaceMatch 'automation'
  ```

- CLI do Azure

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # List namespaces
  az provider list --query [*].namespace

  # Get Azure Policy aliases for a specific Namespace (such as Azure Automation -- Microsoft.Automation)
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

### <a name="understanding-the--alias"></a>Noções básicas sobre o alias [*]

Vários dos aliases que estão disponíveis possuem uma versão que é apresentado como um nome de "normal" e outro que tenha **[\*]** ligados ao mesmo. Por exemplo:

- `Microsoft.Storage/storageAccounts/networkAcls.ipRules`
- `Microsoft.Storage/storageAccounts/networkAcls.ipRules[*]`

O primeiro exemplo é utilizado para avaliar a matriz de inteira, onde o **[\*]** alias avalia cada elemento da matriz.

Vamos examinar uma regra de política como exemplo. Esta política será **negar** uma conta de armazenamento que tenha ipRules configurada e se **nenhum** do ipRules tem um valor de "127.0.0.1".

```json
"policyRule": {
    "if": {
        "allOf": [{
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
                "exists": "true"
            },
            {
                "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
                "notEquals": "127.0.0.1"
            }
        ]
    },
    "then": {
        "effect": "deny",
    }
}
```

O **ipRules** matriz é o seguinte exemplo:

```json
"ipRules": [{
        "value": "127.0.0.1",
        "action": "Allow"
    },
    {
        "value": "192.168.1.1",
        "action": "Allow"
    }
]
```

Eis como neste exemplo é processado:

- `networkAcls.ipRules` -Verifique que a matriz é não nulo. Ela é avaliada true, para que a avaliação continua.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules",
    "exists": "true"
  }
  ```

- `networkAcls.ipRules[*].value` -Examina cada um deles _valor_ propriedade na **ipRules** matriz.

  ```json
  {
    "field": "Microsoft.Storage/storageAccounts/networkAcls.ipRules[*].value",
    "notEquals": "127.0.0.1"
  }
  ```

  - Como uma matriz, cada elemento será processado.

    - "127.0.0.1"! = "127.0.0.1" avalia como falso.
    - "127.0.0.1"! = "192.168.1.1" avalia como verdadeiro.
    - Pelo menos um _valor_ propriedade na **ipRules** avaliada como false, para que a avaliação irá parar de matriz.

Como condição avaliada como falsa, o **negar** efeito não é acionado.

## <a name="initiatives"></a>Iniciativas

Iniciativas permitem-lhe agrupar várias definições de política relacionada para simplificar a gestão e as atribuições de como trabalhar com um grupo como um único item. Por exemplo, pode agrupar as definições de política de etiquetagem relacionados numa única iniciativa. Em vez de atribuir cada política individualmente, aplicar a iniciativa.

O exemplo a seguir ilustra como criar uma iniciativa para lidar com duas etiquetas: `costCenter` e `productName`. Ele usa duas políticas incorporadas para aplicar o valor da etiqueta predefinida.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [{
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

## <a name="next-steps"></a>Passos Seguintes

- Reveja exemplos em [exemplos do Azure Policy](../samples/index.md)
- Revisão [Noções básicas sobre os efeitos de política](effects.md)
- Compreender como [criar políticas programaticamente](../how-to/programmatically-create.md)
- Saiba como [obter dados de conformidade](../how-to/getting-compliance-data.md)
- Saiba como [remediar recursos não compatíveis](../how-to/remediate-resources.md)
- Rever o que é um grupo de gestão, com [Organizar os recursos com grupos de gestão do Azure](../../management-groups/overview.md)