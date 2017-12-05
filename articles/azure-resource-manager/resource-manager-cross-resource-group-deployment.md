---
title: "Implementar os recursos do Azure a vários grupos de recursos e subscrição | Microsoft Docs"
description: "Mostra como mais de um grupo de recursos e subscrição do Azure de destino durante a implementação."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2017
ms.author: tomfitz
ms.openlocfilehash: 763f46b9b5be7edf06ee0604bfc51a2482405b60
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Implementar recursos do Azure em mais do que uma subscrição ou grupo de recursos

Normalmente, implementa todos os recursos no seu modelo para um grupo de recursos única. No entanto, existem cenários onde pretende implementar um conjunto de recursos em conjunto, mas colocá-los em grupos de recursos diferente ou subscrições. Por exemplo, poderá implementar a máquina virtual de cópia de segurança do Azure Site Recovery para um grupo de recursos separada e uma localização. O Resource Manager permite-lhe utilizar modelos aninhados para o destino subscrições e grupos de recursos que a subscrição e o grupo de recursos utilizados para o modelo de principal.

O grupo de recursos é o contentor de ciclo de vida da aplicação e a sua coleção de recursos. Criar o grupo de recursos fora do modelo e especifique o grupo de recursos de destino durante a implementação. Para uma introdução aos grupos de recursos, consulte [descrição geral do Azure Resource Manager](resource-group-overview.md).

## <a name="specify-a-subscription-and-resource-group"></a>Especifique um grupo de recursos e subscrição

Para um outro recurso de destino, tem de utilizar um modelo aninhado ou ligado durante a implementação. O `Microsoft.Resources/deployments` tipo de recurso fornece parâmetros para `subscriptionId` e `resourceGroup`. Estas propriedades permitem-lhe especificar um grupo de recursos e subscrição diferente para a implementação aninhada. Todos os grupos de recursos tem de existir antes de executar a implementação. Se não especificar o grupo de recursos ou um ID de subscrição, a subscrição e grupo de recursos do modelo principal é utilizada.

O exemplo seguinte implementa duas contas de armazenamento - um grupo de recursos especificado durante a implementação, e especificar um grupo de recursos no `secondResourceGroup` parâmetro:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        },
        "secondResourceGroup": {
            "type": "string"
        },
        "secondSubscriptionID": {
            "type": "string",
            "defaultValue": ""
        },
        "secondStorageLocation": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "firstStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
        "secondStorageName": "[concat(parameters('storagePrefix'), uniqueString(parameters('secondSubscriptionID'), parameters('secondResourceGroup')))]"
    },
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "[parameters('secondResourceGroup')]",
            "subscriptionId": "[parameters('secondSubscriptionID')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[variables('secondStorageName')]",
                            "apiVersion": "2017-06-01",
                            "location": "[parameters('secondStorageLocation')]",
                            "sku":{
                                "name": "Standard_LRS"
                            },
                            "kind": "Storage",
                            "properties": {
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('firstStorageName')]",
            "apiVersion": "2017-06-01",
            "location": "[resourceGroup().location]",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        }
    ]
}
```

Se definir `resourceGroup` ao nome de um grupo de recursos que não existe, a implementação falhará.

## <a name="deploy-the-template"></a>Implementar o modelo

Para implementar o modelo de exemplo, utilize uma versão do Azure PowerShell ou a CLI do Azure de Maio de 2017 ou posterior. Para estes exemplos, utilize o [cruzada subscrição modelo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) no GitHub.

### <a name="two-resource-groups-in-the-same-subscription"></a>Dois grupos de recursos na mesma subscrição

Para o PowerShell, para implementar duas contas de armazenamento em dois grupos de recursos na mesma subscrição, utilize:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzureRmResourceGroup -Name $firstRG -Location southcentralus
New-AzureRmResourceGroup -Name $secondRG -Location eastus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

Para a CLI do Azure, para implementar duas contas de armazenamento em dois grupos de recursos na mesma subscrição, utilize:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

Após a conclusão da implementação, verá dois grupos de recursos. Cada grupo de recursos contém uma conta de armazenamento.

### <a name="two-resource-groups-in-different-subscriptions"></a>Dois grupos de recursos em subscrições diferentes

Para o PowerShell, para implementar duas contas de armazenamento em duas subscrições, utilize:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzureRmSubscription -Subscription $secondSub
New-AzureRmResourceGroup -Name $secondRG -Location eastus

Select-AzureRmSubscription -Subscription $firstSub
New-AzureRmResourceGroup -Name $firstRG -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

Para a CLI do Azure, para implementar duas contas de armazenamento em duas subscrições, utilize:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

## <a name="use-the-resourcegroup-function"></a>Utilize a função de resourceGroup()

Para cruzada implementações do grupo de recursos, o [resourceGroup() função](resource-group-template-functions-resource.md#resourcegroup) resolve forma diferente, consoante como especificar o modelo aninhado. 

Se incorporar um modelo dentro de outro modelo, resourceGroup() no modelo aninhado é resolvido para o grupo de recursos principais. Um modelo incorporado utiliza o seguinte formato:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

Se ligar a um modelo separado, resourceGroup() no modelo ligado é resolvido para o grupo de recursos aninhados. Um modelo ligado utiliza o seguinte formato:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

Para testar as diferentes formas `resourceGroup()` seja resolvido, implemente um [modelo de exemplo](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) que devolve o objeto de grupo de recursos do elemento principal modelo, o modelo de inline e o modelo de ligado. O modelo de principal e inline que ambos de resolver para o mesmo grupo de recursos. O modelo ligado é resolvido para o grupo de recursos associados.

Para o PowerShell, utilize:

```powershell
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Para a CLI do Azure, utilize:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

## <a name="next-steps"></a>Passos seguintes

* Para compreender como definir parâmetros no modelo, consulte [compreender a estrutura e a sintaxe de modelos Azure Resource Manager](resource-group-authoring-templates.md).
* Para sugestões sobre como resolver erros comuns de implementação, consulte [resolver erros comuns de implementação do Azure com o Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Para obter informações sobre a implementação de um modelo que necessita de um token SAS, consulte [implementar a modelo privado com o SAS token](resource-manager-powershell-sas-token.md).
