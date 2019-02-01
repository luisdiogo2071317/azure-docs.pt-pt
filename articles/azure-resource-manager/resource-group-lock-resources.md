---
title: Bloquear recursos do Azure para impedir alterações | Documentos da Microsoft
description: Impedi os utilizadores de atualização e a eliminação de recursos do Azure críticos ao aplicar um bloqueio para todos os utilizadores e funções.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 53c57e8f-741c-4026-80e0-f4c02638c98b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: 209a5a9c213a48920452230b1d684fdf0738e87d
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55496912"
---
# <a name="lock-resources-to-prevent-unexpected-changes"></a>Bloquear recursos pode prevenir alterações inesperadas 

Como administrador, terá de bloquear uma subscrição, grupo de recursos ou recursos para impedir que outros utilizadores na sua organização acidentalmente eliminem ou modifiquem recursos críticos. Pode definir o nível do bloqueio para **CanNotDelete** ou **ReadOnly**. No portal, os bloqueios são chamados **elimine** e **só de leitura** , respetivamente.

* **CanNotDelete** significa que os utilizadores autorizados ainda podem ler e modificar um recurso, mas eles não é possível eliminar o recurso. 
* **Só de leitura** significa que os utilizadores autorizados podem ler um recurso, mas não é possível eliminar ou atualizar o recurso. Aplicar esse bloqueio é semelhante ao restringir todos os utilizadores autorizados para as permissões concedidas pela **leitor** função. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="how-locks-are-applied"></a>Como os bloqueios são aplicados

Quando aplica um bloqueio num âmbito principal, todos os recursos dentro daquele escopo herdam o mesmo bloqueio. Recursos até mesmo que adicionar posteriormente herdam o bloqueio do pai. O bloqueio mais restritivo na herança tem precedência.

Ao contrário do controlo de acesso baseado em funções, usar os bloqueios de gestão para aplicar uma restrição em todos os utilizadores e funções. Para saber mais sobre como definir permissões para utilizadores e funções, veja [controlo de acesso baseado em funções do Azure](../role-based-access-control/role-assignments-portal.md).

Bloqueios de Gestor de recursos aplicam-se apenas às operações que ocorrem no plano de gestão, que consiste em operações enviadas para `https://management.azure.com`. Os bloqueios não restringem como recursos executam suas próprias funções. As alterações de recursos são restritas, mas as operações de recursos não estão restritas. Por exemplo, um bloqueio só de leitura numa base de dados SQL impede-o de eliminem ou modifiquem o banco de dados, mas ele não o impede de criar, atualizar ou eliminar dados na base de dados. Transações de dados são permitidas porque essas operações não são enviadas para `https://management.azure.com`.

Aplicando **só de leitura** pode levar a resultados inesperados, uma vez que algumas operações que parecem ler operações, na verdade, exigem ações adicionais. Por exemplo, colocar uma **só de leitura** bloqueio numa conta de armazenamento impede que todos os utilizadores de listar as chaves. A lista de operação de chaves é tratada por meio de um pedido POST porque as chaves retornadas estão disponíveis para operações de escrita. Por outro exemplo, colocar uma **só de leitura** bloqueio num recurso de serviço de aplicações impede que o Visual Studio Server Explorer de apresentar ficheiros para o recurso porque essa interação requer acesso de escrita.

## <a name="who-can-create-or-delete-locks-in-your-organization"></a>Quem pode criar ou eliminar bloqueios na sua organização
Para criar ou eliminar bloqueios de gestão, tem de ter acesso ao `Microsoft.Authorization/*` ou `Microsoft.Authorization/locks/*` ações. Das funções incorporadas, apenas **Proprietário** e **Administrador de Acesso dos Utilizadores** têm acesso a essas ações.

## <a name="portal"></a>Portal
[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="template"></a>Modelo
O exemplo seguinte mostra um modelo que cria um plano do serviço de aplicações, um web site e um bloqueio no web site. O tipo de recurso do bloqueio é o tipo de recurso do recurso para bloquear e **/fornecedores/bloqueios**. O nome do bloqueio é criado pela concatenação com o nome do recurso **/Microsoft.Authorization/** e o nome do bloqueio.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "hostingPlanName": {
            "type": "string"
        }
    },
    "variables": {
        "siteName": "[concat('ExampleSite', uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "apiVersion": "2016-09-01",
            "type": "Microsoft.Web/serverfarms",
            "name": "[parameters('hostingPlanName')]",
            "location": "[resourceGroup().location]",
            "sku": {
                "tier": "Free",
                "name": "f1",
                "capacity": 0
            },
            "properties": {
                "targetWorkerCount": 1
            }
        },
        {
            "apiVersion": "2016-08-01",
            "name": "[variables('siteName')]",
            "type": "Microsoft.Web/sites",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
            ],
            "properties": {
                "serverFarmId": "[parameters('hostingPlanName')]"
            }
        },
        {
            "type": "Microsoft.Web/sites/providers/locks",
            "apiVersion": "2016-09-01",
            "name": "[concat(variables('siteName'), '/Microsoft.Authorization/siteLock')]",
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('siteName'))]"
            ],
            "properties": {
                "level": "CanNotDelete",
                "notes": "Site should not be deleted."
            }
        }
    ]
}
```

Para implementar este modelo de exemplo com o PowerShell, utilize:

```azurepowershell-interactive
New-AzResourceGroup -Name sitegroup -Location southcentralus
New-AzResourceGroupDeployment -ResourceGroupName sitegroup -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/lock.json -hostingPlanName plan0103
```

Para implementar este modelo de exemplo com a CLI do Azure, utilize:

```azurecli
az group create --name sitegroup --location southcentralus
az group deployment create --resource-group sitegroup --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/lock.json --parameters hostingPlanName=plan0103
```

## <a name="powershell"></a>PowerShell
O bloqueio é implementada recursos com o Azure PowerShell com o [New-AzResourceLock](/powershell/module/az.resources/new-azresourcelock) comando.

Para bloquear um recurso, forneça o nome do recurso, o respetivo tipo de recurso e o seu nome de grupo de recursos.

```azurepowershell-interactive
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Para bloquear um grupo de recursos, forneça o nome do grupo de recursos.

```azurepowershell-interactive
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup
```

Para obter informações sobre um bloqueio, utilize [Get-AzureRmResourceLock](/powershell/module/az.resources/get-azresourcelock). Para obter todos os bloqueios na sua subscrição, utilize:

```azurepowershell-interactive
Get-AzResourceLock
```

Para obter todos os bloqueios de um recurso, utilize:

```azurepowershell-interactive
Get-AzResourceLock -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup
```

Para obter todos os bloqueios para um grupo de recursos, utilize:

```azurepowershell-interactive
Get-AzResourceLock -ResourceGroupName exampleresourcegroup
```

Para eliminar um bloqueio, utilize:

```azurepowershell-interactive
$lockId = (Get-AzResourceLock -ResourceGroupName exampleresourcegroup -ResourceName examplesite -ResourceType Microsoft.Web/sites).LockId
Remove-AzResourceLock -LockId $lockId
```

## <a name="azure-cli"></a>CLI do Azure

O bloqueio é implementada recursos com a CLI do Azure com o [bloqueio az criar](/cli/azure/lock#az-lock-create) comando.

Para bloquear um recurso, forneça o nome do recurso, o respetivo tipo de recurso e o seu nome de grupo de recursos.

```azurecli
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites
```

Para bloquear um grupo de recursos, forneça o nome do grupo de recursos.

```azurecli
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup
```

Para obter informações sobre um bloqueio, utilize [lista de bloqueio de az](/cli/azure/lock#az-lock-list). Para obter todos os bloqueios na sua subscrição, utilize:

```azurecli
az lock list
```

Para obter todos os bloqueios de um recurso, utilize:

```azurecli
az lock list --resource-group exampleresourcegroup --resource-name examplesite --namespace Microsoft.Web --resource-type sites --parent ""
```

Para obter todos os bloqueios para um grupo de recursos, utilize:

```azurecli
az lock list --resource-group exampleresourcegroup
```

Para eliminar um bloqueio, utilize:

```azurecli
lockid=$(az lock show --name LockSite --resource-group exampleresourcegroup --resource-type Microsoft.Web/sites --resource-name examplesite --output tsv --query id)
az lock delete --ids $lockid
```

## <a name="rest-api"></a>API REST
É possível bloquear recursos implementados com o [API REST para bloqueios de gestão](https://docs.microsoft.com/rest/api/resources/managementlocks). A API REST permite-lhe criar e eliminar bloqueios e obter informações sobre bloqueios existentes.

Para criar um bloqueio, execute:

    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version={api-version}

O âmbito pode ser uma subscrição, grupo de recursos ou recurso. O nome de bloqueio é tudo o que deseja chamar o bloqueio. Para a versão de api, utilize **2015-01-01**.

Na solicitação, incluem um objeto JSON que especifica as propriedades para o bloqueio.

    {
      "properties": {
        "level": "CanNotDelete",
        "notes": "Optional text notes."
      }
    } 

## <a name="next-steps"></a>Passos Seguintes
* Para saber mais sobre como organizar logicamente os recursos, veja [utilizando etiquetas para organizar os recursos](resource-group-using-tags.md)
* Para alterar um recurso reside no qual grupo de recursos, consulte [mover recursos para um novo grupo de recursos](resource-group-move-resources.md)
* Pode aplicar restrições e convenções na sua subscrição com políticas personalizadas. Para obter mais informações, veja [What is Azure Policy?](../azure-policy/azure-policy-introduction.md) (O que é o Azure Policy?).
* Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](/azure/architecture/cloud-adoption-guide/subscription-governance).

