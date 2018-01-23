---
title: "Secção de parâmetro de modelo do Azure Resource Manager | Microsoft Docs"
description: "Descreve a secção de parâmetros de modelos Azure Resource Manager utilizando a sintaxe declarativa de JSON."
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
ms.date: 01/19/2018
ms.author: tomfitz
ms.openlocfilehash: 5a519908f43193e41da9237a236d720fe2db58eb
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Secção de parâmetros de modelos Azure Resource Manager
Na secção de parâmetros do modelo, especifique os valores que pode introduzir quando implementar os recursos. Estes valores de parâmetros permitem-lhe personalizar a implementação, fornecendo valores que são adaptados para um ambiente específico (por exemplo, o desenvolvimento, teste e produção). Não é necessário fornecer os parâmetros no modelo, mas sem parâmetros do modelo implementaria sempre os mesmos recursos com os mesmos nomes, localizações e as propriedades.

Está limitado a 255 parâmetros de num modelo. Pode reduzir o número de parâmetros utilizando os objetos que contêm várias propriedades, como a mostrar neste artigo.

## <a name="define-and-use-a-parameter"></a>Definir e utilizar um parâmetro

O exemplo seguinte mostra uma definição do parâmetro simples. Define o nome do parâmetro e especifica que demora um valor de cadeia. O parâmetro só aceita valores que façam sentido para a sua utilização pretendida. Especifica um valor predefinido quando não é fornecido nenhum valor durante a implementação. Por fim, o parâmetro inclui uma descrição da sua utilização. 

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }   
}
```

Modelo de referenciar o valor para o parâmetro com a seguinte sintaxe:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>Propriedades disponíveis

O exemplo anterior mostrou apenas algumas das propriedades que pode utilizar a secção de parâmetro. As propriedades disponíveis são:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| Nome do elemento | Necessário | Descrição |
|:--- |:--- |:--- |
| parameterName |Sim |Nome do parâmetro. Tem de ser um identificador de JavaScript válido. |
| tipo |Sim |Tipo do valor de parâmetro. Os tipos permitidos e os valores são **cadeia**, **secureString**, **int**, **bool**, **objeto**, **secureObject**, e **matriz**. |
| defaultValue |Não |Valor predefinido para o parâmetro se não for fornecido nenhum valor para o parâmetro. |
| allowedValues |Não |Matriz de valores permitidos para o parâmetro para se certificar de que é fornecido o valor correto. |
| minValue |Não |O valor mínimo de parâmetros de tipo int, este valor é inclusive. |
| maxValue |Não |O valor máximo de parâmetros de tipo int, este valor é inclusive. |
| minLength |Não |O comprimento mínimo de cadeia, secureString e parâmetros de tipo de matriz, este valor é inclusive. |
| maxLength |Não |O comprimento máximo da cadeia, secureString e parâmetros de tipo de matriz, este valor é inclusive. |
| descrição |Não |Descrição do parâmetro que é apresentada aos utilizadores através do portal. |

## <a name="template-functions-with-parameters"></a>Funções de modelo com parâmetros

Ao fornecer o valor predefinido para um parâmetro, pode utilizar a maioria das funções de modelo. Pode utilizar outro valor de parâmetro para criar um valor predefinido. O modelo seguinte demonstra a utilização de funções no valor predefinido:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Não é possível utilizar o `reference` função na secção de parâmetros. Os parâmetros são avaliados antes da implementação, por isso, o `reference` função não é possível obter o estado do tempo de execução de um recurso. 

## <a name="objects-as-parameters"></a>Objetos como parâmetros

Pode ser mais fácil organizar os valores relacionados transferindo-os na como um objeto. Esta abordagem também reduz o número de parâmetros no modelo.

Defina o parâmetro no seu modelo e especifique um objeto JSON em vez de um valor único durante a implementação. 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "eastus",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Em seguida, fazer referência a subproperties do parâmetro utilizando o operador de ponto.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="recommendations"></a>Recomendações
As seguintes informações podem ser úteis quando trabalha com parâmetros:

* Estará a minimizar a utilização de parâmetros. Sempre que possível, utilize uma variável ou um valor literal. Utilize parâmetros apenas para estes cenários:
   
   * Definições que pretende utilizar variações de acordo com o ambiente (SKU, tamanho, capacidade).
   * Nomes de recursos que pretende especificar para fins de identificação fácil.
   * Valores que utilizar com frequência para realizar outras tarefas (por exemplo, um nome de utilizador de admin).
   * Segredos (tal como as palavras-passe).
   * O número ou a matriz de valores a utilizar quando criar várias instâncias de um tipo de recurso.
* Utilize camel case para os nomes dos parâmetros.
* Forneça uma descrição de cada parâmetro nos metadados:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Defina os valores predefinidos para os parâmetros (exceto para palavras-passe e chaves SSH). Ao fornecer um valor predefinido, o parâmetro torna-se opcional durante a implementação. O valor predefinido pode ser uma cadeia vazia. 
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Utilize **SecureString** para todas as palavras-passe e segredos. Se passar dados confidenciais num objeto JSON, utilize o **secureObject** tipo. Não não possível ler os parâmetros do modelo tipos secureString ou secureObject após a implementação de recursos. 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Utilize um parâmetro para especificar a localização e partilhar esse valor de parâmetro quanto possível com os recursos que são provável que esteja na mesma localização. Esta abordagem minimiza o número de vezes que os utilizadores são-lhe pedidos para fornecer informações de localização. Se um tipo de recurso é suportado em apenas um número limitado de localizações, poderá especificar uma localização válida diretamente no modelo ou adicione outro parâmetro de localização. Quando uma organização limita as regiões permitidas aos utilizadores, a **resourceGroup (). location** expressão pode impedir um utilizador que está a ser possível implementar o modelo. Por exemplo, um utilizador cria um grupo de recursos numa região. Um segundo utilizador tem de implementar a esse grupo de recursos, mas não tem acesso para a região. 
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[parameters('location')]",
         ...
     }
   ]
   ```
    
* Evite utilizar um parâmetro ou variável para a versão da API para um tipo de recurso. Propriedades de recursos e os valores podem variar pelo número de versão. O IntelliSense num editor de código não é possível determinar o esquema correto quando a versão de API está definida como um parâmetro ou variável. Em vez disso, versão de codificar a API no modelo.
* Evite especificar um nome de parâmetro no modelo que corresponda a um parâmetro de comando de implementação. Gestor de recursos resolve este conflito de atribuição de nomes ao adicionar o sufixo **FromTemplate** para o parâmetro de modelo. Por exemplo, se incluir um parâmetro com o nome **ResourceGroupName** no seu modelo, está em conflito com o **ResourceGroupName** parâmetro o [New-AzureRmResourceGroupDeployment ](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet. Durante a implementação, lhe for pedido para fornecer um valor para **ResourceGroupNameFromTemplate**.

## <a name="example-templates"></a>Modelos de exemplo

Estes modelos de exemplo demonstram alguns cenários de utilização de parâmetros. Implementá-las para testar a forma como os parâmetros são processados em cenários diferentes.

|Modelo  |Descrição  |
|---------|---------|
|[parâmetros com as funções para os valores predefinidos](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Demonstra como utilizar funções de modelo quando definir os valores predefinidos para os parâmetros. O modelo não implementa todos os recursos. Constrói valores de parâmetros e devolve esses valores. |
|[objeto de parâmetro](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Demonstra a utilização de um objeto para um parâmetro. O modelo não implementa todos os recursos. Constrói valores de parâmetros e devolve esses valores. |

## <a name="next-steps"></a>Passos Seguintes

* Para ver modelos completos para vários tipos de soluções, veja os [Modelos de Início Rápido do Azure](https://azure.microsoft.com/documentation/templates/).
* Para saber como os valores de parâmetros de entrada durante a implementação, consulte [implementar uma aplicação com o modelo Azure Resource Manager](resource-group-template-deploy.md). 
* Para obter detalhes sobre as funções que pode utilizar a partir de um modelo, consulte [funções de modelo do Azure Resource Manager](resource-group-template-functions.md).
* Para obter informações sobre a utilização de um objeto parameter, consulte [utilizar um objeto como um parâmetro num modelo Azure Resource Manager](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).