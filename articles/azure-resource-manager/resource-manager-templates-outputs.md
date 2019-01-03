---
title: Saídas do modelo Azure Resource Manager | Documentos da Microsoft
description: Descreve como definir as saídas de um modelos Azure Resource Manager usando sintaxe declarativa do JSON.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/18/2018
ms.author: tomfitz
ms.openlocfilehash: 9a46d813f2e50831240303ba47380da39e2cb6af
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53725817"
---
# <a name="outputs-section-in-azure-resource-manager-templates"></a>Secção de saídas em modelos do Azure Resource Manager
Na secção de saídas, especifique os valores que são devolvidos da implementação. Por exemplo, pode devolver o URI para aceder a um recurso implementado.

## <a name="define-and-use-output-values"></a>Definir e utilizar valores de saída

O exemplo seguinte mostra como devolver o ID de recurso para um endereço IP público:

```json
"outputs": {
  "resourceID": {
    "type": "string",
    "value": "[resourceId('Microsoft.Network/publicIPAddresses', parameters('publicIPAddresses_name'))]"
  }
}
```

Após a implementação, pode recuperar o valor com o script. Para o PowerShell, utilize:

```powershell
(Get-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Para a CLI do Azure, utilize:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

Pode obter o valor de saída a partir de um modelo ligado utilizando a [referência](resource-group-template-functions-resource.md#reference) função. Para obter um valor de saída a partir de um modelo ligado, obter o valor da propriedade com a sintaxe, como: `"[reference('deploymentName').outputs.propertyName.value]"`.

Ao obter uma propriedade de saída a partir de um modelo ligado, o nome da propriedade não pode incluir um traço.

Por exemplo, pode definir o endereço IP num Balanceador de carga ao obter um valor a partir de um modelo ligado.

```json
"publicIPAddress": {
    "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Não é possível utilizar o `reference` função na secção de saídas de um [modelo aninhado](resource-group-linked-templates.md#link-or-nest-a-template). Para devolver os valores para um recurso implementado num modelo aninhado, converta seu modelo aninhado para um modelo ligado.

## <a name="available-properties"></a>Propriedades disponíveis

O exemplo seguinte mostra a estrutura de uma definição de saída:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Nome do elemento | Necessário | Descrição |
|:--- |:--- |:--- |
| outputName |Sim |Nome do valor de saída. Tem de ser um identificador de JavaScript válido. |
| tipo |Sim |Tipo do valor de saída. Valores de saída suportam os mesmos tipos de parâmetros de entrada de modelo. |
| valor |Sim |Expressão de linguagem de modelo que é avaliada e devolvida como valor de saída. |


## <a name="example-templates"></a>Modelos de exemplo

|Modelo  |Descrição  |
|---------|---------|
|[Copie as variáveis](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Cria variáveis complexo e produz esses valores. Não implemente todos os recursos. |
|[Endereço IP público](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Cria um endereço IP público e devolve o ID de recurso. |
|[Balanceador de carga](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Ligações para o modelo anterior. Utiliza o ID de recurso na saída, ao criar o Balanceador de carga. |


## <a name="next-steps"></a>Passos Seguintes
* Para ver modelos completos para vários tipos de soluções, veja os [Modelos de Início Rápido do Azure](https://azure.microsoft.com/documentation/templates/).
* Para obter detalhes sobre as funções que pode utilizar a partir de dentro de um modelo, consulte [funções de modelo do Azure Resource Manager](resource-group-template-functions.md).
* Para obter recomendações sobre a criação de modelos, veja [práticas recomendadas do modelo do Azure Resource Manager](template-best-practices.md).
