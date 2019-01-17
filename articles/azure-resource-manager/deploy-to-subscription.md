---
title: Criar o grupo de recursos e recursos na subscrição - modelo Azure Resource Manager
description: Descreve como criar um grupo de recursos num modelo Azure Resource Manager. Ela também mostra como implementar recursos no âmbito da subscrição do Azure.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/15/2018
ms.author: tomfitz
ms.openlocfilehash: 542993d803282bbf62e2e401cab1968a656a8971
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352279"
---
# <a name="create-resource-groups-and-resources-for-an-azure-subscription"></a>Criar grupos de recursos e recursos para uma subscrição do Azure

Normalmente, implementa recursos para um grupo de recursos na sua subscrição do Azure. No entanto, pode utilizar as implementações ao nível da subscrição para criar grupos de recursos e recursos que se aplicam na sua subscrição.

Para criar um grupo de recursos num modelo Azure Resource Manager, defina uma **Microsoft.Resources/resourceGroups** recurso com um nome e local para o grupo de recursos. Pode criar um grupo de recursos e implementar recursos no grupo de recursos no mesmo modelo.

[As políticas](../azure-policy/azure-policy-introduction.md), [controlo de acesso baseado em funções](../role-based-access-control/overview.md), e [Centro de segurança do Azure](../security-center/security-center-intro.md) são os serviços que talvez deseje aplicam-se ao nível da subscrição, em vez de nível do grupo de recursos.

Este artigo mostra como criar grupos de recursos e como criar recursos aplicam-se numa subscrição. Utiliza a CLI do Azure e o PowerShell para implementar os modelos. Não é possível utilizar o portal para implementar os modelos porque a interface do portal implementa o grupo de recursos, não a subscrição do Azure.

## <a name="schema-and-commands"></a>Esquema e os comandos

O esquema e os comandos que utilizar para implementações de nível de assinatura são diferentes de implementações do grupo de recursos. 

Para o esquema, utilize `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`.

Para o comando de implementação da CLI do Azure, utilize [criar a implementação de az](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create).

Para o comando de implementação do PowerShell, utilize [New-AzureRmDeployment](/powershell/module/azurerm.resources/new-azurermdeployment).

## <a name="name-and-location"></a>Nome e localização

Ao implementar a sua subscrição, tem de fornecer uma localização para a implementação. Também pode fornecer um nome para a implementação. Se não especificar um nome para a implementação, o nome do modelo é utilizado como o nome da implementação. Por exemplo, implementar um modelo com o nome **azuredeploy. JSON** cria um nome de implementação do padrão de **azuredeploy**.

A localização das implementações de nível de subscrição é imutável. Não é possível criar uma implementação num único local quando existe uma implementação existente com o mesmo nome mas localização diferente. Se receber o código de erro `InvalidDeploymentLocation`, utilize um nome diferente ou a mesma localização como a implementação anterior para esse nome.

## <a name="using-template-functions"></a>Com as funções de modelo

Para implementações de nível de subscrição, existem algumas considerações importantes ao utilizar as funções de modelo:

* O [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) função é **não** suportado.
* O [resourceId()](resource-group-template-functions-resource.md#resourceid) função é suportada. Utilize-o para obter o ID de recurso para recursos que são utilizados em implementações de nível de subscrição. Por exemplo, obter o ID de recurso para uma definição de política com `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`
* O [reference()](resource-group-template-functions-resource.md#reference) e [List ()](resource-group-template-functions-resource.md#list) as funções são suportadas.

## <a name="create-resource-group"></a>Criar grupo de recursos

O exemplo seguinte cria um grupo de recursos vazio.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Para implementar este modelo com a CLI do Azure, utilize:

```azurecli-interactive
az deployment create \
  -n demoEmptyRG \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoRG rgLocation=northcentralus
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name demoEmptyRG `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demogroup `
  -rgLocation northcentralus
```

## <a name="create-several-resource-groups"></a>Criar vários grupos de recursos

Utilize o [elemento de cópia](resource-group-create-multiple.md) com grupos de recursos para criar mais do que um grupo de recursos. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgNamePrefix": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "instanceCount": {
            "type": "int"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
            "copy": {
                "name": "rgCopy",
                "count": "[parameters('instanceCount')]"
            },
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Para implementar este modelo com a CLI do Azure e criar três grupos de recursos, utilize:

```azurecli-interactive
az deployment create \
  -n demoCopyRG \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/copyRG.json \
  --parameters rgNamePrefix=demoRG rgLocation=northcentralus instanceCount=3
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name demoCopyRG `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/copyRG.json `
  -rgNamePrefix demogroup `
  -rgLocation northcentralus `
  -instanceCount 3
```

## <a name="create-resource-group-and-deploy-resource"></a>Criar grupo de recursos e implementar recursos

Para criar o grupo de recursos e implementar recursos na mesma, utilize um modelo aninhado. O modelo aninhado define os recursos necessários para implementar o grupo de recursos. Defina o modelo aninhado como o grupo de recursos para se certificar de que o grupo de recursos existe antes de implementar os recursos dependentes.

O exemplo seguinte cria um grupo de recursos e implementa uma conta de armazenamento para o grupo de recursos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        }
    },
    "variables": {
        "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "storageDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
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
                            "apiVersion": "2017-10-01",
                            "name": "[variables('storageName')]",
                            "location": "[parameters('rgLocation')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Standard_LRS"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

Para implementar este modelo com a CLI do Azure, utilize:

```azurecli-interactive
az deployment create \
  -n demoRGStorage \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/newRGWithStorage.json \
  --parameters rgName=rgStorage rgLocation=northcentralus storagePrefix=storage
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name demoRGStorage `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/newRGWithStorage.json `
  -rgName rgStorage `
  -rgLocation northcentralus `
  -storagePrefix storage
```

## <a name="assign-policy"></a>Atribuir política

O exemplo seguinte atribui uma definição de política existente para a subscrição. Se a política tem parâmetros, forneça-los como um objeto. Se a política não aceitam parâmetros, utilize o objeto vazio padrão.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

Para aplicar uma política incorporada a sua subscrição do Azure, utilize os seguintes comandos do CLI do Azure:

```azurecli-interactive
# Built-in policy that does not accept parameters
definition=$(az policy definition list --query "[?displayName=='Audit resource location matches resource group location'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=auditRGLocation
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit resource location matches resource group location' }

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName auditRGLocation
```

Para aplicar uma política incorporada a sua subscrição do Azure, utilize os seguintes comandos do CLI do Azure:

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

## <a name="define-and-assign-policy"></a>Definir e atribuir política

Pode [definir](../azure-policy/policy-definition.md) e atribuir uma política no modelo do mesmo.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2018-05-01",
            "dependsOn": [
                "locationpolicy"
            ],
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
            }
        }
    ]
}
```

Para criar a definição de política na sua subscrição e aplicá-la para a subscrição, utilize o seguinte comando da CLI:

```azurecli-interactive
az deployment create \
  -n definePolicy \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name definePolicy `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="assign-role-at-subscription"></a>Atribuir função na subscrição

O exemplo seguinte atribui uma função a um utilizador ou grupo para a subscrição. Neste exemplo, não especifique um âmbito para a atribuição de uma vez que o âmbito é automaticamente definido para a subscrição.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "[guid(parameters('principalId'), deployment().name)]",
            "apiVersion": "2017-09-01",
            "properties": {
                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Para atribuir um grupo do Active Directory a uma função para a sua subscrição, utilize os seguintes comandos do CLI do Azure:

```azurecli-interactive
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  -n demoRole \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
$role = Get-AzureRmRoleDefinition -Name Contributor

$adgroup = Get-AzureRmADGroup -DisplayName demogroup

New-AzureRmDeployment `
  -Name demoRole `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id
```

## <a name="assign-role-at-scope"></a>Atribuir função no âmbito

O modelo de nível de subscrição seguinte atribui uma função a um utilizador ou grupo que está confinado para um grupo de recursos dentro da subscrição. O âmbito tem de ser igual ou abaixo do nível de implementação. Pode implementar para uma subscrição e especificar uma atribuição de função no âmbito de um grupo de recursos nessa subscrição. No entanto, não é possível implementar um grupo de recursos e especificar um âmbito de atribuição de função à subscrição.

Para atribuir a função a um âmbito, utilize uma implementação aninhada. Tenha em atenção que o nome do grupo de recursos é especificado nas propriedades para o recurso de implantação e na propriedade do âmbito da atribuição de função.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        },
        "rgName": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "assignRole",
            "resourceGroup": "[parameters('rgName')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Authorization/roleAssignments",
                            "name": "[guid(parameters('principalId'), deployment().name)]",
                            "apiVersion": "2017-09-01",
                            "properties": {
                                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                                "principalId": "[parameters('principalId')]",
                                "scope": "[concat(subscription().id, '/resourceGroups/', parameters('rgName'))]"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

Para atribuir um grupo do Active Directory a uma função para a sua subscrição, utilize os seguintes comandos do CLI do Azure:

```azurecli-interactive
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  -n demoRole \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scopedRoleAssign.json \
  --parameters principalId=$principalid roleDefinitionId=$role rgName demoRg
```

Para implementar este modelo com o PowerShell, utilize:

```azurepowershell-interactive
$role = Get-AzureRmRoleDefinition -Name Contributor

$adgroup = Get-AzureRmADGroup -DisplayName demogroup

New-AzureRmDeployment `
  -Name demoRole `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/scopedRoleAssign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id `
  -rgName demoRg
```

## <a name="next-steps"></a>Passos Seguintes
* Para obter um exemplo de implementação de definições de área de trabalho para o Centro de segurança do Azure, veja [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Para saber mais sobre a criação de modelos Azure Resource Manager, veja [criação de modelos](resource-group-authoring-templates.md). 
* Para obter uma lista das funções disponíveis num modelo, consulte [funções de modelo](resource-group-template-functions.md).
