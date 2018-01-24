---
title: Produz o modelo Azure Resource Manager | Microsoft Docs
description: "Descreve como definir saídas para um modelos Azure Resource Manager utilizando a sintaxe declarativa de JSON."
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
ms.date: 12/14/2017
ms.author: tomfitz
ms.openlocfilehash: 64d7a0ea72b2f629160f31e4bc1fb4a90f10653d
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="outputs-section-in-azure-resource-manager-templates"></a>Secção saídas modelos Azure Resource Manager
Na secção saídas, especifique os valores que são devolvidos por implementação. Por exemplo, pode devolver o URI para aceder a um recurso implementado.

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

Após a implementação, pode obter o valor com script. Para o PowerShell, utilize:

```powershell
(Get-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -Name <deployment-name>).Outputs.resourceID.value
```

Para a CLI do Azure, utilize:

```azurecli-interactive
az group deployment show -g <resource-group-name> -n <deployment-name> --query properties.outputs.resourceID.value
```

Pode obter o valor de saída a partir de um modelo ligado utilizando a [referência](resource-group-template-functions-resource.md#reference) função. Para obter um valor de saída a partir de um modelo ligado, obter o valor da propriedade com sintaxe como: `"[reference('<name-of-deployment>').outputs.<property-name>.value]"`.

Por exemplo, pode definir o endereço IP num Balanceador de carga por obter um valor a partir de um modelo ligado.

```json
"publicIPAddress": {
    "id": "[reference('linkedTemplate').outputs.resourceID.value]"
}
```

Não é possível utilizar o `reference` função na secção saídas um [modelo aninhado](resource-group-linked-templates.md#link-or-nest-a-template). Para devolver os valores para um recurso implementado num modelo aninhado, converta o modelo aninhado num modelo ligado.

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
| tipo |Sim |Tipo de valor de saída. Valores de saída suportam os mesmos tipos de parâmetros de entrada de modelo. |
| valor |Sim |Expressão de linguagem do modelo que é avaliada e devolvido como valor de saída. |

## <a name="recommendations"></a>Recomendações

Se utilizar um modelo para criar os endereços IP públicos, inclua uma secção de saídas devolve detalhes do endereço IP e o nome de domínio completamente qualificado (FQDN). Pode utilizar valores de saída para obter detalhes sobre o público endereços IP e FQDNs facilmente após a implementação.

```json
"outputs": {
    "fqdn": {
        "value": "[reference(parameters('publicIPAddresses_name')).dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(parameters('publicIPAddresses_name')).ipAddress]",
        "type": "string"
    }
}
```

## <a name="example-templates"></a>Modelos de exemplo


|Modelo  |Descrição  |
|---------|---------|
|[Variáveis de cópia](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/multipleinstance/copyvariables.json) | Cria complexas variáveis e produz esses valores. Não implementa todos os recursos. |
|[Endereço IP público](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip.json) | Cria um endereço IP público e produz o ID de recurso. |
|[Balanceador de carga](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/linkedtemplates/public-ip-parentloadbalancer.json) | Ligações para o modelo anterior. Utiliza o ID de recurso na saída ao criar o Balanceador de carga. |


## <a name="next-steps"></a>Passos Seguintes
* Para ver modelos completos para vários tipos de soluções, veja os [Modelos de Início Rápido do Azure](https://azure.microsoft.com/documentation/templates/).
* Para obter detalhes sobre as funções que pode utilizar a partir de um modelo, consulte [funções de modelo do Azure Resource Manager](resource-group-template-functions.md).
* Combinar vários modelos durante a implementação, consulte [utilizar modelos ligados com o Azure Resource Manager](resource-group-linked-templates.md).
* Terá de utilizar os recursos que existe dentro de um grupo de recursos diferente. Este cenário é comum quando trabalhar com as contas de armazenamento e as redes virtuais que são partilhadas em vários grupos de recursos. Para obter mais informações, consulte o [resourceId função](resource-group-template-functions-resource.md#resourceid).