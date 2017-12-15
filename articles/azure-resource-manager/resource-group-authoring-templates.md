---
title: Estrutura de modelo do Azure Resource Manager e sintaxe | Microsoft Docs
description: Descreve a estrutura e propriedades de modelos Azure Resource Manager utilizando a sintaxe declarativa de JSON.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/14/2017
ms.author: tomfitz
ms.openlocfilehash: b0bc5abd768be0fa5876aaef108cd71a15d94510
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Compreender a estrutura e a sintaxe de modelos Azure Resource Manager
Este artigo descreve a estrutura de um modelo Azure Resource Manager. Apresente as diferentes secções de um modelo e as propriedades que estão disponíveis dessas secções. O modelo é constituído por JSON e expressões que pode utilizar para construir valores para a sua implementação. Para um tutorial passo a passo sobre como criar um modelo, consulte [criar o primeiro modelo Azure Resource Manager](resource-manager-create-first-template.md).

## <a name="template-format"></a>Formato de modelo
Na sua estrutura mais simples, um modelo contém os seguintes elementos:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Nome do elemento | Necessário | Descrição |
|:--- |:--- |:--- |
| $schema |Sim |Localização do ficheiro de esquema JSON que descreve a versão do idioma do modelo. Utilize o URL apresentado no exemplo anterior. |
| contentVersion |Sim |Versão do modelo (por exemplo, 1.0.0.0). Pode fornecer qualquer valor para este elemento. Quando implementar recursos com o modelo, este valor pode ser utilizado para se certificar de que está a ser utilizado o modelo à direita. |
| parâmetros |Não |Valores que são fornecidos durante a implementação é executada para personalizar a implementação de recursos. |
| variáveis |Não |Valores que são utilizados como fragmentos JSON no modelo para simplificar as expressões de idioma do modelo. |
| recursos |Sim |Tipos de recursos que são implementados ou atualizados num grupo de recursos. |
| saídas |Não |Valores que são devolvidos após a implementação. |

Cada elemento contém propriedades, que pode definir. O exemplo seguinte contém a sintaxe completa para um modelo:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
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
    },
    "variables": {
        "<variable-name>": "<variable-value>",
        "<variable-object-name>": {
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
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Este artigo descreve as secções do modelo em maior detalhe.

## <a name="syntax"></a>Sintaxe
A sintaxe básica do modelo é JSON. No entanto, expressões e as funções expandem os valores JSON disponíveis no modelo.  As expressões são escritas no literais de cadeia JSON cujo primeiro e último carateres são Retos: `[` e `]`, respetivamente. O valor da expressão é avaliado quando o modelo é implementado. Enquanto escritos como uma cadeia literal, o resultado da avaliação da expressão pode ser um tipo JSON diferente, tal como uma matriz nem um número inteiro, consoante a expressão real.  Para ter um literal de cadeia começa com um parêntese `[`, mas não o tiver interpretado como uma expressão, adicione um parêntesis adicional para iniciar a cadeia com `[[`.

Normalmente, utilizar expressões com as funções para executar operações para configurar a implementação. Apenas como em JavaScript, chamadas de função estejam formatadas como `functionName(arg1,arg2,arg3)`. Referenciar propriedades utilizando os operadores de ponto e [Índice].

O exemplo seguinte mostra como utilizar várias funções quando construir um valor:

```json
"variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
}
```

Para obter a lista completa de funções de modelo, consulte [funções de modelo do Azure Resource Manager](resource-group-template-functions.md). 

## <a name="parameters"></a>Parâmetros
Na secção de parâmetros do modelo, especifique os valores que pode introduzir quando implementar os recursos. Estes valores de parâmetros permitem-lhe personalizar a implementação, fornecendo valores que são adaptados para um ambiente específico (por exemplo, o desenvolvimento, teste e produção). Não é necessário fornecer os parâmetros no modelo, mas sem parâmetros do modelo implementaria sempre os mesmos recursos com os mesmos nomes, localizações e as propriedades.

O exemplo seguinte mostra uma definição do parâmetro simples:

```json
"parameters": {
  "siteNamePrefix": {
    "type": "string",
    "metadata": {
      "description": "The name prefix of the web app that you wish to create."
    }
  },
},
```

Para obter informações sobre como definir parâmetros, consulte [secção de parâmetros de modelos Azure Resource Manager](resource-manager-templates-parameters.md).

## <a name="variables"></a>Variáveis
Na secção de variáveis, construir valores que podem ser utilizados em todo o seu modelo. Não é necessário definir as variáveis, mas são, muitas vezes, simplificam o modelo ao reduzir as expressões complexas.

O exemplo seguinte mostra uma definição de variável simple:

```json
"variables": {
  "webSiteName": "[concat(parameters('siteNamePrefix'), uniqueString(resourceGroup().id))]",
},
```

Para obter informações sobre como definir variáveis, consulte [secção de variáveis de modelos Azure Resource Manager](resource-manager-templates-variables.md).

## <a name="resources"></a>Recursos
Na secção de recursos, é possível definir os recursos que são implementados ou atualizados. Nesta secção pode obter complicada porque tem de compreender os tipos que está a implementar para fornecer os valores corretos.

```json
"resources": [
  {
    "apiVersion": "2016-08-01",
    "name": "[variables('webSiteName')]",
    "type": "Microsoft.Web/sites",
    "location": "[resourceGroup().location]",
    "properties": {
      "serverFarmId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group-name>/providers/Microsoft.Web/serverFarms/<plan-name>"
    }
  }
],
```

Para obter mais informações, consulte [secção de recursos de modelos Azure Resource Manager](resource-manager-templates-resources.md).

## <a name="outputs"></a>Saídas
Na secção saídas, especifique os valores que são devolvidos por implementação. Por exemplo, pode devolver o URI para aceder a um recurso implementado.

```json
"outputs": {
  "newHostName": {
    "type": "string",
    "value": "[reference(variables('webSiteName')).defaultHostName]"
  }
}
```

Para obter mais informações, consulte [produz a secção de modelos Azure Resource Manager](resource-manager-templates-outputs.md).

## <a name="template-limits"></a>Limites de modelo

Limite o tamanho do seu modelo para 1 MB e cada ficheiro de parâmetros para 64 KB. O limite de 1 MB aplica-se para o estado final do modelo de depois foi expandido com as definições do recurso interativo e os valores de parâmetros e variáveis. 

Também estão limitados a:

* 256 parâmetros
* 256 variáveis
* 800 recursos (incluindo o número de cópias)
* 64 valores de saída
* 24,576 carateres de uma expressão de modelo

Pode exceder alguns limites de modelo ao utilizar um modelo aninhado. Para obter mais informações, consulte [utilizar modelos ligados ao implementar os recursos do Azure](resource-group-linked-templates.md). Para reduzir o número de parâmetros, variáveis ou saídas, pode combinar vários valores para um objeto. Para obter mais informações, consulte [objetos como parâmetros](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Passos seguintes
* Para ver modelos completos para vários tipos de soluções, veja os [Modelos de Início Rápido do Azure](https://azure.microsoft.com/documentation/templates/).
* Para obter detalhes sobre as funções que pode utilizar a partir de um modelo, consulte [funções de modelo do Azure Resource Manager](resource-group-template-functions.md).
* Combinar vários modelos durante a implementação, consulte [utilizar modelos ligados com o Azure Resource Manager](resource-group-linked-templates.md).
* Terá de utilizar os recursos que existe dentro de um grupo de recursos diferente. Este cenário é comum quando trabalhar com as contas de armazenamento e as redes virtuais que são partilhadas em vários grupos de recursos. Para obter mais informações, consulte o [resourceId função](resource-group-template-functions-resource.md#resourceid).
