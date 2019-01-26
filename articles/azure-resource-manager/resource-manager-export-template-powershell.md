---
title: Exportar o modelo do Resource Manager com o Azure PowerShell | Documentos da Microsoft
description: Utilize o Azure Resource Manager e o Azure PowerShell para exportar um modelo a partir de um grupo de recursos.
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
ms.date: 02/23/2018
ms.author: tomfitz
ms.openlocfilehash: cca81bf3f5a46b32cc901a0ac6024eb7888685f7
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/26/2019
ms.locfileid: "55081610"
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a>Exportar modelos do Azure Resource Manager com o PowerShell

O Gestor de Recursos permite-lhe exportar um modelo do Gestor de Recursos a partir de recursos existentes na sua subscrição. Pode utilizar esse modelo gerado para saber mais sobre a sintaxe do modelo ou para automatizar a reimplementação da sua solução conforme necessário.

É importante observar que existem duas formas diferentes de exportar um modelo:

* Pode exportar os **modelo que foi utilizado para uma implementação**. O modelo exportado inclui todos os parâmetros e variáveis exatamente como apareciam no modelo original. Esta abordagem é útil quando precisar de obter um modelo.
* Pode exportar um **modelo gerado que representa o estado atual do grupo de recursos**. O modelo exportado não é baseado em qualquer modelo utilizado para implementação. Em vez disso, ele cria um modelo que é um "instantâneo" ou "cópia de segurança" do grupo de recursos. O modelo exportado tem muitos valores codificados e provavelmente não tantos parâmetros como normalmente seriam definidos. Utilize esta opção para voltar a implementar recursos para o mesmo grupo de recursos. Para utilizar este modelo para outro grupo de recursos, poderá ter de significativamente modificá-lo.

Este artigo mostra ambas as abordagens.

## <a name="deploy-a-solution"></a>Implementar uma solução

Para ilustrar as duas abordagens para exportar um modelo, vamos começar por implementar uma solução à sua subscrição. Se já tiver um grupo de recursos na sua subscrição que pretende exportar, não tem de implementar esta solução. No entanto, o resto do artigo refere-se para o modelo para esta solução. O script de exemplo implementa uma conta de armazenamento.

```powershell
New-AzResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a>Guardar modelo no histórico de implementações

Pode obter um modelo a partir do seu histórico de implementação utilizando o [Save-AzureRmResourceGroupDeploymentTemplate](/powershell/module/az.resources/save-azresourcegroupdeploymenttemplate) comando. O exemplo a seguir salva o modelo que implementou anteriormente:

```powershell
Save-AzResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

Devolve a localização do modelo.

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

Abra o ficheiro e tenha em atenção que é o modelo exato utilizado para a implementação. Os parâmetros e variáveis correspondem ao modelo do GitHub. Pode voltar a implementar este modelo.

## <a name="export-resource-group-as-template"></a>Exportar grupo de recursos como modelo

Em vez de obter um modelo a partir do histórico de implementação, pode obter um modelo que representa o estado atual de um grupo de recursos, utilizando o [Export-AzureRmResourceGroup](/powershell/module/az.resources/export-azresourcegroup) comando. Utilize este comando quando fez muitas alterações ao seu grupo de recursos e nenhum modelo existente representa todas as alterações. Destina-se como um instantâneo do grupo de recursos, que pode utilizar para Reimplementar no mesmo grupo de recursos. Para utilizar o modelo exportado para outras soluções, tem de modificá-lo significativamente.

```powershell
Export-AzResourceGroup -ResourceGroupName ExampleGroup
```

Devolve a localização do modelo.

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

Abra o ficheiro e tenha em atenção que é diferente do que o modelo no GitHub. Ele tem parâmetros diferentes e nenhuma variável. O SKU de armazenamento e a localização estão hard-coded para valores. O exemplo seguinte mostra o modelo exportado, mas o seu modelo tem um nome de parâmetro ligeiramente diferente:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccounts_nf3mvst4nqb36standardsa_name": {
      "defaultValue": null,
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[parameters('storageAccounts_nf3mvst4nqb36standardsa_name')]",
      "apiVersion": "2016-01-01",
      "location": "southcentralus",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Pode voltar a implementar este modelo, mas exige adivinhar um nome exclusivo para a conta de armazenamento. O nome do seu parâmetro é ligeiramente diferente.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a>Personalizar o modelo exportado

Pode modificar este modelo para torná-lo mais fácil de usar e mais flexível. Para permitir mais localizações, altere a propriedade de localização para utilizar a mesma localização que o grupo de recursos:

```json
"location": "[resourceGroup().location]",
```

Para evitar ter de procurar um nome de uniques para a conta de armazenamento, remova o parâmetro para o nome da conta de armazenamento. Adicione um parâmetro para um sufixo de nome de armazenamento e um SKU de armazenamento:

```json
"parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
},
```

Adicione uma variável que constrói o nome da conta de armazenamento com a função de uniqueString:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Defina o nome da conta de armazenamento para a variável:

```json
"name": "[variables('storageAccountName')]",
```

Defina o SKU para o parâmetro:

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

Agora, o seu modelo tem este aspeto:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), parameters('storageSuffix'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "[parameters('storageAccountType')]",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

Reimplemente o modelo modificado.

## <a name="next-steps"></a>Passos Seguintes
* Para obter informações sobre como utilizar o portal para exportar um modelo, consulte [exportar um modelo Azure Resource Manager a partir de recursos existentes](resource-manager-export-template.md).
* Para definir parâmetros no modelo, veja [criação de modelos](resource-group-authoring-templates.md#parameters).
* Para obter dicas sobre como resolver erros comuns de implementação, consulte [resolver erros comuns de implementação do Azure com o Azure Resource Manager](resource-manager-common-deployment-errors.md).
