---
title: "As variáveis de modelo do Azure Resource Manager | Microsoft Docs"
description: "Descreve como definir as variáveis num modelos Azure Resource Manager utilizando a sintaxe declarativa de JSON."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2017
ms.author: tomfitz
ms.openlocfilehash: 8d9f227ad1f450cf6cdfca1dafb1b51bc6f6c9f9
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/13/2017
---
# <a name="variables-section-of-azure-resource-manager-templates"></a>Secção de variáveis de modelos Azure Resource Manager
Na secção de variáveis, construir valores que podem ser utilizados em todo o seu modelo. Não é necessário definir as variáveis, mas são, muitas vezes, simplificam o modelo ao reduzir as expressões complexas.

## <a name="define-and-use-a-variable"></a>Definir e utilizar uma variável

O exemplo seguinte mostra uma definição de variável. Cria um valor de cadeia para um nome de conta de armazenamento. Utiliza várias funções de modelo para obter um valor de parâmetro e concatená-la a uma cadeia exclusiva.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Utilize a variável quando se definem o recurso.

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    ...
```

## <a name="available-definitions"></a>Definições disponíveis

O exemplo anterior mostrou uma forma de definir uma variável. Pode utilizar qualquer uma das seguintes definições:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    },
    "<variable-object-name>": {
        "copy": [
            {
                "name": "<name-of-array-property>",
                "count": <number-of-iterations>,
                "input": {
                    <properties-to-repeat>
                }
            }
        ]
    },
    "copy": [
        {
            "name": "<variable-array-name>",
            "count": <number-of-iterations>,
            "input": {
                <properties-to-repeat>
            }
        }
    ]
}
```

## <a name="configuration-variables"></a>Variáveis de configuração

Pode utilizar os tipos complexos JSON para definir os valores relacionados para um ambiente. 

```json
"variables": {
    "environmentSettings": {
        "test": {
            "instanceSize": "Small",
            "instanceCount": 1
        },
        "prod": {
            "instanceSize": "Large",
            "instanceCount": 4
        }
    }
},
```

Nos parâmetros, crie um valor que indica que configuração valores a utilizar.

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
```

Obter as definições atuais com:

```json
"[variables('environmentSettings')[parameters('environmentName')].instanceSize]"
```

## <a name="use-copy-element-in-variable-definition"></a>Utilize o elemento de cópia na definição de variável

Pode utilizar o **cópia** sintaxe para criar uma variável com uma matriz de vários elementos. Forneça uma contagem do número de elementos. Cada elemento contém as propriedades dentro de **entrada** objeto. Pode utilizar a cópia dentro de uma variável ou para criar a variável. Quando definir uma variável e utilize **cópia** dentro essa variável, criar um objeto que tem uma propriedade de matriz. Quando utiliza **cópia** no nível superior e definir uma ou mais variáveis dentro da mesma, criar matrizes de um ou mais. Ambas as abordagens são mostradas no exemplo seguinte:

```json
"variables": {
    "disk-array-on-object": {
        "copy": [
            {
                "name": "disks",
                "count": 3,
                "input": {
                    "name": "[concat('myDataDisk', copyIndex('disks', 1))]",
                    "diskSizeGB": "1",
                    "diskIndex": "[copyIndex('disks')]"
                }
            }
        ]
    },
    "copy": [
        {
            "name": "disks-top-level-array",
            "count": 3,
            "input": {
                "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('disks-top-level-array')]"
            }
        }
    ]
},
```

A variável **disco matriz no objeto** contém o seguinte objeto com uma matriz com o nome **discos**:

```json
{
  "disks": [
    {
      "name": "myDataDisk1",
      "diskSizeGB": "1",
      "diskIndex": 0
    },
    {
      "name": "myDataDisk2",
      "diskSizeGB": "1",
      "diskIndex": 1
    },
    {
      "name": "myDataDisk3",
      "diskSizeGB": "1",
      "diskIndex": 2
    }
  ]
}
```

A variável **discos top-nível matriz** contém a matriz seguinte:

```json
[
  {
    "name": "myDataDisk1",
    "diskSizeGB": "1",
    "diskIndex": 0
  },
  {
    "name": "myDataDisk2",
    "diskSizeGB": "1",
    "diskIndex": 1
  },
  {
    "name": "myDataDisk3",
    "diskSizeGB": "1",
    "diskIndex": 2
  }
]
```

Também pode especificar mais de um objeto ao utilizar a cópia para criar variáveis. O exemplo seguinte define duas matrizes como variáveis. Um nome **discos top-nível matriz** e tem cinco elementos. O outro é denominado **a diferentes matriz** e tem três elementos.

```json
"variables": {
    "copy": [
        {
            "name": "disks-top-level-array",
            "count": 5,
            "input": {
                "name": "[concat('oneDataDisk', copyIndex('disks-top-level-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('disks-top-level-array')]"
            }
        },
        {
            "name": "a-different-array",
            "count": 3,
            "input": {
                "name": "[concat('twoDataDisk', copyIndex('a-different-array', 1))]",
                "diskSizeGB": "1",
                "diskIndex": "[copyIndex('a-different-array')]"
            }
        }
    ]
},
```

Esta abordagem funciona bem quando tem de ter valores de parâmetros e certifique-se de que estão no formato correto para um valor de modelo. O exemplo seguinte formata os valores de parâmetros para utilização em definir regras de segurança:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "securityRules": {
      "type": "array"
    }
  },
  "variables": {
    "copy": [
      {
        "name": "securityRules",
        "count": "[length(parameters('securityRules'))]",
        "input": {
          "name": "[parameters('securityRules')[copyIndex('securityRules')].name]",
          "properties": {
            "description": "[parameters('securityRules')[copyIndex('securityRules')].description]",
            "priority": "[parameters('securityRules')[copyIndex('securityRules')].priority]",
            "protocol": "[parameters('securityRules')[copyIndex('securityRules')].protocol]",
            "sourcePortRange": "[parameters('securityRules')[copyIndex('securityRules')].sourcePortRange]",
            "destinationPortRange": "[parameters('securityRules')[copyIndex('securityRules')].destinationPortRange]",
            "sourceAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].sourceAddressPrefix]",
            "destinationAddressPrefix": "[parameters('securityRules')[copyIndex('securityRules')].destinationAddressPrefix]",
            "access": "[parameters('securityRules')[copyIndex('securityRules')].access]",
            "direction": "[parameters('securityRules')[copyIndex('securityRules')].direction]"
          }
        }
      }
    ]
  },
  "resources": [
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "NSG1",
      "location": "[resourceGroup().location]",
      "properties": {
        "securityRules": "[variables('securityRules')]"
      }
    }
  ],
  "outputs": {}
}
```

## <a name="recommendations"></a>Recomendações
As seguintes informações podem ser úteis quando trabalha com variáveis:

* Utilize as variáveis para valores que tem de utilizar mais do que uma vez num modelo. Se for utilizado um valor apenas uma vez, um valor hard-coded faz com que o seu modelo mais fáceis de ler.
* Não é possível utilizar o [referência](resource-group-template-functions-resource.md#reference) funcionar o **variáveis** secção do modelo. O **referência** deriva de função de valor de estado de runtime do recurso. No entanto, as variáveis são resolvidas durante a análise inicial do modelo. Construção valores que tem o **referência** funcionarem diretamente no **recursos** ou **produz** secção do modelo.
* Inclua as variáveis para os nomes de recursos que têm de ser exclusivos.

## <a name="example-templates"></a>Modelos de exemplo

Estes modelos de exemplo demonstram alguns cenários de utilização de variáveis. Implementá-las para testar a forma como as variáveis são processadas em cenários diferentes. 

|Modelo  |Descrição  |
|---------|---------|
| [definições de variável](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variables.json) | Demonstra os diferentes tipos de variáveis. O modelo não implementa todos os recursos. Constrói valores das variáveis e devolve esses valores. |
| [variável de configuração](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/variablesconfigurations.json) | Demonstra a utilização de uma variável que define valores de configuração. O modelo não implementa todos os recursos. Constrói valores das variáveis e devolve esses valores. |
| [regras de segurança de rede](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) e [ficheiro de parâmetros](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.parameters.json) | Constrói uma matriz no formato correto para a atribuição de regras de segurança para um grupo de segurança de rede. |


## <a name="next-steps"></a>Passos seguintes
* Para ver modelos completos para vários tipos de soluções, veja os [Modelos de Início Rápido do Azure](https://azure.microsoft.com/documentation/templates/).
* Para obter detalhes sobre as funções que pode utilizar a partir de um modelo, consulte [funções de modelo do Azure Resource Manager](resource-group-template-functions.md).
* Combinar vários modelos durante a implementação, consulte [utilizar modelos ligados com o Azure Resource Manager](resource-group-linked-templates.md).
* Terá de utilizar os recursos que existe dentro de um grupo de recursos diferente. Este cenário é comum quando trabalhar com as contas de armazenamento e as redes virtuais que são partilhadas em vários grupos de recursos. Para obter mais informações, consulte o [resourceId função](resource-group-template-functions-resource.md#resourceid).