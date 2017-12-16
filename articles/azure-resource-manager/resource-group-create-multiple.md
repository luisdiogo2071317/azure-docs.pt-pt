---
title: "Implementar várias instâncias de recursos do Azure | Microsoft Docs"
description: "Utilize a operação de cópia e matrizes de um modelo Azure Resource Manager para iterar várias vezes durante a implementação de recursos."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2017
ms.author: tomfitz
ms.openlocfilehash: e19833cb58f37f5f8b83d5558d74255583137684
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Implementar várias instâncias de um recurso ou a propriedade de modelos Azure Resource Manager
Este artigo mostra-lhe como implementar condicionalmente um recurso e como iterar no modelo Azure Resource Manager para criar várias instâncias de um recurso.

## <a name="conditionally-deploy-resource"></a>Implementar condicionalmente recursos

Quando tem de decidir durante a implementação para criar uma instância ou não existem instâncias de um recurso, utilize o `condition` elemento. O valor para este elemento é resolvido para true ou false. Quando o valor for VERDADEIRO, o recurso é implementado. Quando o valor for FALSO, o recurso não está implementado. Por exemplo, para especificar se for implementada uma nova conta de armazenamento ou uma conta de armazenamento existente é utilizada, utilize:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

## <a name="resource-iteration"></a>Iteração de recursos
Quando tem de decidir durante a implementação para criar uma ou mais instâncias de um recurso, adicione um `copy` elemento para que o tipo de recurso. No elemento de cópia, especifique o número de iterações e um nome para este ciclo. O valor da contagem tem de ser um número inteiro positivo e não pode exceder 800. 

O recurso para criar várias vezes assume o formato seguinte:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

Tenha em atenção que o nome de cada recurso inclui o `copyIndex()` função, que devolve a atual iteração do ciclo. `copyIndex()`é baseado em zero. Deste modo, o exemplo seguinte:

```json
"name": "[concat('storage', copyIndex())]",
```

Cria estes nomes de:

* storage0
* storage1
* storage2.

Para alterar o valor de índice, pode transferir um valor na função copyindex (). O número de iterações para efetuar ainda é especificado no elemento copiar, mas o valor de copyIndex é deslocamento pelo valor especificado. Deste modo, o exemplo seguinte:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Cria estes nomes de:

* storage1
* storage2
* storage3

A operação de cópia é útil ao trabalhar com as matrizes porque pode iterar através de cada elemento na matriz. Utilize o `length` função na matriz para especificar a contagem de iterações, e `copyIndex` para obter o índice atual na matriz. Deste modo, o exemplo seguinte:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

Cria estes nomes de:

* storagecontoso
* storagefabrikam
* storagecoho

Por predefinição, o Gestor de recursos cria os recursos em paralelo. Por conseguinte, não é garantida a ordem em que são criados. No entanto, pode pretender especificar que os recursos são implementados numa sequência. Por exemplo, ao atualizar um ambiente de produção, poderá pretender escalonar as atualizações, por isso, apenas um determinado número são atualizadas ao mesmo tempo.

Para implementar serialmente várias instâncias de um recurso, defina `mode` para **série** e `batchSize` para o número de instâncias para implementar cada vez. Com o modo de série, o Gestor de recursos cria uma dependência em instâncias anteriores no ciclo de, pelo que não for iniciado um batch até que o lote anterior seja concluída.

Por exemplo, para implementar serialmente contas do storage dois num momento, utilize:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            }
        }
    ],
    "outputs": {}
}
``` 

A propriedade de modo também aceita **paralelas**, que é o valor predefinido.

## <a name="property-iteration"></a>Iteração de propriedade

Para criar vários valores para uma propriedade num recurso, adicione um `copy` matriz no elemento de propriedades. Esta matriz contém objetos e cada objeto tem as seguintes propriedades:

* nome – o nome da propriedade para criar vários valores para
* Conte - o número de valores a criar
* um objeto que contém os valores a atribuir à propriedade - de entrada  

O exemplo seguinte mostra como aplicar `copy` para a propriedade dataDisks numa máquina virtual:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "copy": [{
          "name": "dataDisks",
          "count": 3,
          "input": {
              "lun": "[copyIndex('dataDisks')]",
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Repare que, quando utilizar `copyIndex` dentro de uma iteração de propriedade, tem de fornecer o nome da iteração. Não é necessário que fornecer o nome quando utilizado com iteração do recurso.

O Resource Manager expande o `copy` matriz durante a implementação. O nome da matriz torna-se o nome da propriedade. Os valores de entrada tornar-se as propriedades do objeto. O modelo implementado torna-se:

```json
{
  "name": "examplevm",
  "type": "Microsoft.Compute/virtualMachines",
  "apiVersion": "2017-03-30",
  "properties": {
    "storageProfile": {
      "dataDisks": [
          {
              "lun": 0,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 1,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          },
          {
              "lun": 2,
              "createOption": "Empty",
              "diskSizeGB": "1023"
          }
      }],
      ...
```

Pode utilizar iteração do recurso e a propriedade em conjunto. Referência da iteração de propriedade por nome.

```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[concat(parameters('vnetname'), copyIndex())]",
    "apiVersion": "2016-06-01",
    "copy":{
        "count": 2,
        "name": "vnetloop"
    },
    "location": "[resourceGroup().location]",
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "copy": [
            {
                "name": "subnets",
                "count": 2,
                "input": {
                    "name": "[concat('subnet-', copyIndex('subnets'))]",
                    "properties": {
                        "addressPrefix": "[variables('subnetAddressPrefix')[copyIndex('subnets')]]"
                    }
                }
            }
        ]
    }
}
```

## <a name="variable-iteration"></a>Iteração variável

Para criar várias instâncias de uma variável, utilize o `copy` elemento na secção de variáveis. Pode criar várias instâncias de objetos com os valores relacionados e, em seguida, atribuir esses valores para as instâncias do recurso. Pode utilizar a cópia para criar o objeto com uma propriedade de matriz ou uma matriz. Ambas as abordagens são mostradas no exemplo seguinte:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "disk-array-on-object": {
      "copy": [
        {
          "name": "disks",
          "count": 5,
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
        "count": 5,
        "input": {
          "name": "[concat('myDataDisk', copyIndex('disks-top-level-array', 1))]",
          "diskSizeGB": "1",
          "diskIndex": "[copyIndex('disks-top-level-array')]"
        }
      }
    ]
  },
  "resources": [],
  "outputs": {
    "exampleObject": {
      "value": "[variables('disk-array-on-object')]",
      "type": "object"
    },
    "exampleArrayOnObject": {
      "value": "[variables('disk-array-on-object').disks]",
      "type" : "array"
    },
    "exampleArray": {
      "value": "[variables('disks-top-level-array')]",
      "type" : "array"
    }
  }
}
```

## <a name="depend-on-resources-in-a-loop"></a>Dependem de recursos num ciclo
Especifique a que um recurso é implementado depois de outro recurso utilizando o `dependsOn` elemento. Para implementar um recurso que depende da coleção de recursos num ciclo, forneça o nome do ciclo de cópia do elemento dependsOn. O exemplo seguinte mostra como implementar três contas de armazenamento antes de implementar a Máquina Virtual. Não é apresentada a definição de Máquina Virtual completa. Tenha em atenção que o elemento de cópia tem name definido como `storagecopy` e o elemento de dependsOn para as máquinas virtuais também está definido como `storagecopy`.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="iteration-for-a-child-resource"></a>Iteração para um recurso de subordinados
Não é possível utilizar um ciclo de cópia para um recurso subordinado. Para criar várias instâncias de um recurso que normalmente definir como aninhada dentro de outro recurso, em vez disso, tem de criar o recurso de como um recurso de nível superior. É possível definir a relação com o recurso principal através de propriedades de tipo e nome.

Por exemplo, suponha que, normalmente, é possível definir um conjunto de dados como um recurso subordinado dentro de uma fábrica de dados.

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

Para criar várias instâncias de conjuntos de dados, movê-la fora da fábrica de dados. O conjunto de dados tem de estar no mesmo nível que a fábrica de dados, mas ainda é um recurso de subordinados da fábrica de dados. Manter a relação entre o conjunto de dados e a fábrica de dados através de propriedades de tipo e nome. Uma vez que já não pode ser inferido tipo da posição no modelo, tem de fornecer o tipo completamente qualificado no formato: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Para estabelecer uma relação principal/subordinado com uma instância do data factory, forneça um nome para o conjunto de dados que inclui o nome do recurso principal. Utilize o formato: `{parent-resource-name}/{child-resource-name}`.  

O exemplo seguinte mostra a implementação:

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="example-templates"></a>Modelos de exemplo

Os exemplos seguintes mostram cenários comuns para criar vários recursos ou propriedades.

|Modelo  |Descrição  |
|---------|---------|
|[Armazenamento de cópia](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystorage.json) |Implementa múltiplas contas de armazenamento com um número de índice no nome. |
|[Armazenamento de cópia de série](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/serialcopystorage.json) |Implementa múltiplas contas de armazenamento um momento. O nome inclui o número de índice. |
|[Armazenamento de cópia com matriz](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copystoragewitharray.json) |Implementa múltiplas contas de armazenamento. O nome inclui um valor de uma matriz. |
|[VM com um novo ou existente rede Virtual, do armazenamento e um IP público](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-new-or-existing-conditions) |Implementa condicionalmente recursos novos ou existentes com uma máquina virtual. |
|[Implementação da VM com um número de variável de discos de dados](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-windows-copy-datadisks) |Implementa vários discos de dados com uma máquina virtual. |
|[Variáveis de cópia](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) |Demonstra as diferentes formas de iterating em variáveis. |
|[Várias regras de segurança](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/multiplesecurityrules.json) |Implementa múltiplas regras de segurança para um grupo de segurança de rede. -Constrói as regras de segurança de um parâmetro. |

## <a name="next-steps"></a>Passos seguintes
* Se pretender saber mais sobre as secções de um modelo, consulte [criação de modelos do Azure Resource Manager](resource-group-authoring-templates.md).
* Para saber como implementar o modelo, consulte o artigo [implementar uma aplicação com o modelo do Azure Resource Manager](resource-group-template-deploy.md).

