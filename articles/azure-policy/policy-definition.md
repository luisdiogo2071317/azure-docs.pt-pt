---
title: Estrutura de definição do Azure Policy
description: Descreve como definição de política de recurso é utilizada pela política do Azure para estabelecer as convenções de recursos na sua organização ao descrever quando a política é imposta e o efeito resultante a tomar.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 05/24/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: 2f756d65fa167b3812772088aec7232d08b04b9f
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937337"
---
# <a name="azure-policy-definition-structure"></a>Estrutura de definição do Azure Policy

Definição de política de recurso utilizada pela política do Azure permite-lhe estabelecer as convenções de recursos na sua organização ao descrever quando a política é imposta e o efeito resultante a tomar. Se definir convenções, pode controlar os custos e gerir mais facilmente os seus recursos. Por exemplo, pode especificar que apenas determinados tipos de máquinas virtuais são permitidos. Em alternativa, pode exigir que todos os recursos tem uma tag específica. As políticas são herdadas por todos os recursos subordinados. Por isso, se uma política é aplicada a um grupo de recursos, é aplicável a todos os recursos nesse grupo de recursos.

O esquema utilizado pela política do Azure pode ser encontrado aqui: [https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json](https://schema.management.azure.com/schemas/2016-12-01/policyDefinition.json)

Utilize o JSON para criar uma definição de política. A definição de política contém elementos para:

- mode
- parâmetros
- nome a apresentar
- descrição
- regra de política
  - avaliação lógica
  - efeito

Por exemplo, o seguinte JSON mostra uma política que limita a onde os recursos são implementados:

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

Todas as amostras de política do Azure estão em [amostras de política](json-samples.md).

## <a name="mode"></a>Modo

O **modo** determina que tipos de recursos serão avaliados para uma política. Os modos suportados são:

- `all`: avaliar os grupos de recursos e todos os tipos de recursos
- `indexed`: apenas avaliar os tipos de recursos que suportam as etiquetas e a localização

Recomendamos que defina **modo** para `all` na maioria dos casos. Todas as definições de política criadas através da utilização do portal de `all` modo. Se utiliza o PowerShell ou a CLI do Azure, pode especificar o **modo** parâmetro manualmente. Se a definição de política não contém um **modo** valor-assume a predefinição `all` no Azure PowerShell e a `null` na CLI do Azure, que é equivalente a `indexed`, para efeitos compatibilidade.

`indexed` deve ser utilizado quando a criação de políticas que irá impor localizações ou etiquetas. Não é necessária mas impedirá a recursos que não suportam as etiquetas e localizações de ser apresentado como não conformes nos resultados de compatibilidade. A única exceção é **grupos de recursos**. Devem definir políticas que estão a tentar aplicar etiquetas num grupo de recursos ou localização **modo** para `all` e o destino especificamente o `Microsoft.Resources/subscriptions/resourceGroup` tipo. Por exemplo, consulte [etiquetas de grupo de recursos de impor](scripts/enforce-tag-rg.md).

## <a name="parameters"></a>Parâmetros

Parâmetros de ajudam a simplificar a gestão de política, reduzindo o número de definições de política. Considere parâmetros, como os campos de um formulário – `name`, `address`, `city`, `state`. Estes parâmetros sempre permanecem igual ao, no entanto alterar os respetivos valores com base no individuais preencher o formulário. Os parâmetros funcionam da mesma forma na criação de políticas. Incluindo parâmetros na definição de política, pode reutilizar essa política para diferentes cenários utilizando valores diferentes.

Por exemplo, pode definir uma política para uma propriedade de recurso limitar as localizações onde os recursos podem ser implementados. Neste caso, seriam declarar os parâmetros seguintes ao criar a política:

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

Dentro da propriedade de metadados, pode utilizar **strongType** para fornecer uma lista selecionar vários das opções no portal do Azure.  Valores para permitidos **strongType** atualmente incluem:

- `"location"`
- `"resourceTypes"`
- `"storageSkus"`
- `"vmSKUs"`
- `"existingResourceGroups"`
- `"omsWorkspace"`

A regra de política, referenciar parâmetros com a seguinte sintaxe:

```json
{
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="definition-location"></a>Localização da definição

Ao criar uma definição de política ou iniciativa, é importante que especifique a localização de definição.

A localização de definição determina o âmbito em que a definição de política ou iniciativa pode ser atribuída a. A localização pode ser especificada como um grupo de gestão ou uma subscrição.

> [!NOTE]
> Se planeia aplicar esta definição de política para várias subscrições, a localização tem de ser um grupo de gestão que contém as subscrições que atribuirá a iniciativa ou a política.

## <a name="display-name-and-description"></a>Nome a apresentar e uma descrição

Pode utilizar **displayName** e **Descrição** para identificar a definição de política e fornecer contexto para quando é utilizado.

## <a name="policy-rule"></a>Regra de política

A regra de política é composta por **se** e **, em seguida,** blocos. No **se** bloquear, definir uma ou mais condições que especificar quando a política é imposta. Pode aplicar operadores lógicos para estas condições precisamente definir o cenário de uma política.

No **, em seguida,** bloquear, definir o efeito que ocorre quando o **se** condições são verdadeiras.

```json
{
    "if": {
        <condition> | <logical operator>
    },
    "then": {
        "effect": "deny | audit | append | auditIfNotExists | deployIfNotExists"
    }
}
```

### <a name="logical-operators"></a>Operadores lógicos

Operadores lógicos suportados são:

- `"not": {condition  or operator}`
- `"allOf": [{condition or operator},{condition or operator}]`
- `"anyOf": [{condition or operator},{condition or operator}]`

O **não** sintaxe inverts o resultado da condição. O **tudo** sintaxe (semelhante ao lógico **e** operação) requer que todas as condições. O **anyOf** sintaxe (semelhante ao lógico **ou** operação) requer um ou mais condições.

Podem aninhar operadores lógicos. O seguinte exemplo mostra um **não** operação que está aninhada dentro de um **tudo** operação.

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

A condição for avaliada se um **campo** cumpra determinados critérios. As condições suportadas são:

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

Ao utilizar o **como** e **notLike** condições, pode fornecer um caráter universal `*` no valor.
O valor não deve conter mais de um caráter universal `*`.

Ao utilizar o **corresponder** e **notMatch** condições, fornecer `#` para representar um dígito, `?` para uma letra e quaisquer outros carateres para representar esse caráter real. Para obter exemplos, consulte [permitir várias padrões de nome](scripts/allow-multiple-name-patterns.md).

### <a name="fields"></a>Campos

Condições são formadas campos. Um campo representa propriedades no payload de pedido de recurso que é utilizada para descrever o estado do recurso.  

São suportados os seguintes campos:

- `name`
- `fullName`
  - Devolve o nome completo do recurso. O nome completo de um recurso é o nome de recurso prepended por quaisquer nomes de recursos do principal (por exemplo "myServer/myDatabase").
- `kind`
- `type`
- `location`
- `tags`
- `tags.tagName`
- `tags[tagName]`
  - Esta sintaxe Reto suporta nomes de etiqueta, que podem conter períodos de
- aliases de propriedade - para obter uma lista, consulte [Aliases](#aliases).

### <a name="alternative-accessors"></a>Acessores alternativos

**Campo** é o acessor primário utilizado nas regras de política. -Lo diretamente inspeciona o recurso que está a ser avaliado. No entanto, a política suporta um outro acessor **origem**.

```json
"source": "action",
"equals": "Microsoft.Compute/virtualMachines/write"
```

**Origem** suporta apenas um valor, **ação**. Ação devolve a ação de autorização do pedido que está a ser avaliado. Ações de autorização são expostas na secção autorização o [registo de atividade](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

Quando a política está a avaliar os recursos existentes em segundo plano, define **ação** para um `/write` ação de autorização no tipo de recurso.

### <a name="effect"></a>Efeito

Política suporta os seguintes tipos de efeito:

- **Negar**: gera um evento no registo de auditoria e falhar o pedido
- **Auditoria**: gera um evento de aviso no registo de auditoria, mas não a falhar o pedido
- **Acrescentar**: Adiciona um conjunto definido de campos para o pedido
- **AuditIfNotExists**: permite que o se não existir um recurso de auditoria
- **DeployIfNotExists**: implementa um recurso, se já existir. Atualmente, este efeito só é suportado através de políticas incorporadas.

Para **acrescentar**, tem de fornecer os seguintes detalhes:

```json
"effect": "append",
"details": [{
    "field": "field name",
    "value": "value of the field"
}]
```

O valor pode ser uma cadeia ou um objeto de formato JSON.

Com **AuditIfNotExists** e **DeployIfNotExists** pode avaliar a existência de um recurso relacionado e aplicar uma regra e um efeito correspondente ao recurso de não existe. Por exemplo, pode exigir que um observador de rede é implementado para todas as redes virtuais.
Para obter um exemplo de auditoria quando uma extensão da máquina virtual não está a ser implementada, consulte [auditoria se a extensão não existe](scripts/audit-ext-not-exist.md).

Para obter detalhes completos sobre cada efeito, ordem de avaliação, propriedades e exemplos, consulte [efeitos de política de compreender](policy-effects.md).

## <a name="aliases"></a>Aliases

Pode utilizar os aliases de propriedade para aceder às propriedades específicas para um tipo de recurso. Aliases permitem-lhe restringir os valores ou condições são permitidas para uma propriedade num recurso. Cada alias mapeia para caminhos em diferentes versões de API para um tipo de recurso especificado. Durante a avaliação da política, o motor de política obtém o caminho da propriedade para essa versão de API.

A lista de aliases sempre está a crescer. Para detetar os aliases são atualmente suportados pela política do Azure, utilize um dos seguintes métodos:

- Azure PowerShell

  ```azurepowershell-interactive
  # Login first with Connect-AzureRmAccount if not using Cloud Shell

  $azContext = Get-AzureRmContext
  $azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
  $profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
  $token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
  $authHeader = @{
      'Content-Type'='application/json'
      'Authorization'='Bearer ' + $token.AccessToken
  }

  # Invoke the REST API
  $response = Invoke-RestMethod -Uri 'https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases' -Method Get -Headers $authHeader

  # Create an Array List to hold discovered aliases
  $aliases = New-Object System.Collections.ArrayList

  foreach ($ns in $response.value) {
      foreach ($rT in $ns.resourceTypes) {
          if ($rT.aliases) {
              foreach ($obj in $rT.aliases) {
                  $alias = [PSCustomObject]@{
                      Namespace       = $ns.namespace
                      resourceType    = $rT.resourceType
                      alias           = $obj.name
                  }
                  $aliases.Add($alias) | Out-Null
              }
          }
      }
  }

  # Output the list, sort, and format. You can customize with Where-Object to limit as desired.
  $aliases | Sort-Object -Property Namespace, resourceType, alias | Format-Table
  ```

- CLI do Azure

  ```azurecli-interactive
  # Login first with az login if not using Cloud Shell

  # Get Azure Policy aliases for a specific Namespace
  az provider show --namespace Microsoft.Automation --expand "resourceTypes/aliases" --query "resourceTypes[].aliases[].name"
  ```

- REST API / ARMClient

  ```http
  GET https://management.azure.com/providers/?api-version=2017-08-01&$expand=resourceTypes/aliases
  ```

## <a name="initiatives"></a>Iniciativas

Iniciativas permitem-lhe agrupar várias definições de política relacionada para simplificar a gestão e atribuições porque funciona com um grupo como um único item. Por exemplo, pode agrupar relacionados todas as definições de política de etiquetagem numa iniciativa único. Em vez de atribuir a cada política individualmente, aplique a iniciativa.

O exemplo a seguir ilustra como criar uma iniciativa para processamento de duas etiquetas: `costCenter` e `productName`. Utiliza duas políticas incorporadas para aplicar o valor da etiqueta predefinida.

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

- Veja mais exemplos nas [amostras do Azure Policy](json-samples.md).
