---
title: Etiqueta de recursos do Azure para a organização lógica | Microsoft Docs
description: Mostra como aplicar etiquetas para organizar os recursos do Azure para faturação e gestão.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 003a78e5-2ff8-4685-93b4-e94d6fb8ed5b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: AzurePortal
ms.devlang: na
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: tomfitz
ms.openlocfilehash: 8c828bb49548adfdb02ed6fb1611eb405ebf4ff2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34602929"
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Utilizar etiquetas para organizar os recursos do Azure

[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="powershell"></a>PowerShell

Os exemplos neste artigo requerem a versão 6.0 ou posterior do Azure PowerShell. Se não tiver versão 6.0 ou posterior, [atualizar a versão](/powershell/azure/install-azurerm-ps).

Para ver as etiquetas existentes de um *grupo de recursos*, utilize:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Este script devolve o formato seguinte:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Para ver as etiquetas existentes de um *recurso que tem um ID de recurso específico*, utilize:

```powershell
(Get-AzureRmResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

Ou para ver as etiquetas existentes de um *recurso que tem um nome e grupo de recurso específicos*, utilize:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Para obter *grupos de recursos com uma etiqueta específica*, utilize:

```powershell
(Get-AzureRmResourceGroup -Tag @{ Dept="Finance" }).ResourceGroupName
```

Para obter *recursos com uma etiqueta específica*, utilize:

```powershell
(Get-AzureRmResource -Tag @{ Dept="Finance"}).Name
```

Para obter *recursos que têm um nome de tag específica*, utilize:

```powershell
(Get-AzureRmResource -TagName Dept).Name
```

Sempre que aplicar etiquetas a um recurso ou grupo de recursos, as etiquetas existentes nesse recurso ou grupo de recursos são substituídas. Por conseguinte, tem de utilizar uma abordagem diferente, que tenha em conta se o recurso ou grupo de recursos tem etiquetas existentes.

Para adicionar etiquetas a um *grupo de recursos que não tenha etiquetas*, utilize:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Para adicionar etiquetas a um *grupo de recursos que tenha etiquetas existentes*, obtenha-as, adicione a etiqueta nova e volte a aplicar as etiquetas:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

Para adicionar etiquetas a um *recurso que não tenha etiquetas*, utilize:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

Para adicionar etiquetas a um *recurso que tenha etiquetas*, utilize:

```powershell
$r = Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.Tags.Add("Status", "Approved") 
Set-AzureRmResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Para aplicar todas as etiquetas de um grupo de recursos para todos os respetivos recursos, *sem reter etiquetas existentes nos recursos*, utilize o script seguinte:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups)
{
    Get-AzureRmResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Para aplicar todas as etiquetas de um grupo de recursos para todos os respetivos recursos, *retendo etiquetas existentes nos recursos que não são duplicados*, utilize o script seguinte:

```powershell
$group = Get-AzureRmResourceGroup "examplegroup"
if ($group.Tags -ne $null) {
    $resources = Get-AzureRmResource -ResourceGroupName $group.ResourceGroupName
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzureRmResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Para remover todas as etiquetas, transmita uma tabela hash vazia:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```

## <a name="azure-cli"></a>CLI do Azure

Para ver as etiquetas existentes de um *grupo de recursos*, utilize:

```azurecli
az group show -n examplegroup --query tags
```

Este script devolve o formato seguinte:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

Ou, para ver as etiquetas existentes para um *recursos que tem um grupo de recurso, tipo e nome*, utilize:

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Quando criar ciclos através de uma coleção de recursos, pode querer mostrar o ID de recurso por recurso. Um exemplo completo é mostrado posteriormente neste artigo. Para ver as etiquetas existentes de um *recurso que tem um ID de recurso específico*, utilize:

```azurecli
az resource show --id <resource-id> --query tags
```

Para obter grupos de recursos que tenham uma tag específica, utilize `az group list`:

```azurecli
az group list --tag Dept=IT
```

Para obter todos os recursos que tenham uma etiqueta específica e o valor, utilize `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Sempre que aplicar etiquetas a um recurso ou grupo de recursos, as etiquetas existentes nesse recurso ou grupo de recursos são substituídas. Por conseguinte, tem de utilizar uma abordagem diferente, que tenha em conta se o recurso ou grupo de recursos tem etiquetas existentes.

Para adicionar etiquetas a um *grupo de recursos que não tenha etiquetas*, utilize:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

Para adicionar etiquetas a um *recurso que não tenha etiquetas*, utilize:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Para adicionar etiquetas a um recurso que já tenha etiquetas, obter as etiquetas existentes, reformatar esse valor e volte a aplicar as etiquetas existentes e novas: 

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Para aplicar todas as etiquetas de um grupo de recursos para todos os respetivos recursos, *sem reter etiquetas existentes nos recursos*, utilize o script seguinte:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    az resource tag --tags $t --id $resid
  done
done
```

Aplicar todas as etiquetas de um grupo de recursos para os respetivos recursos e *etiquetas existentes nos recursos de manter*, utilize o seguinte script:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done
done
```

## <a name="templates"></a>Modelos

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>API REST

O portal do Azure e o PowerShell utilizam ambos o [API de REST do Resource Manager](https://docs.microsoft.com/rest/api/resources/) em segundo plano. Se precisar de integrar a etiquetagem para outro ambiente, pode obter etiquetas utilizando **obter** no ID de recurso e atualizar o conjunto de etiquetas utilizando um **PATCH** chamada.

## <a name="tags-and-billing"></a>As etiquetas e faturação

Pode utilizar etiquetas para agrupar os dados de faturação. Por exemplo, se estiver a executar várias VMs para diferentes organizações, utilize as etiquetas para utilização do grupo pelo centro de custos. Também pode utilizar etiquetas para categorizar os custos pelo ambiente de tempo de execução, tais como a utilização de faturação para VMs em execução no ambiente de produção.

Pode obter informações sobre etiquetas através de [utilização de recursos do Azure e RateCard APIs](../billing/billing-usage-rate-card-overview.md) ou o ficheiro de valores separados por vírgulas (CSV) de utilização. Transferir o ficheiro de utilização do [portal de contas do Azure](https://account.windowsazure.com/) ou [EA portal](https://ea.azure.com). Para obter mais informações sobre acesso programático para as informações de faturação, consulte [obter informações acerca do consumo de recursos do Microsoft Azure](../billing/billing-usage-rate-card-overview.md). Para operações de REST API, consulte [referência de API de REST de faturação do Azure](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c).

Quando transferir a utilização de CSV para serviços que suportam etiquetas com faturação, as etiquetas são apresentadas no **etiquetas** coluna. Para obter mais informações, consulte [compreender a fatura do Microsoft Azure](../billing/billing-understand-your-bill.md).

![Consulte as etiquetas na faturação](./media/resource-group-using-tags/billing_csv.png)

## <a name="next-steps"></a>Passos Seguintes

* Pode aplicar restrições e convenções na sua subscrição através da utilização de políticas personalizadas. Uma política que definir poderão exigir que todos os recursos têm um valor para uma tag específica. Para obter mais informações, consulte [o que é a política do Azure?](../azure-policy/azure-policy-introduction.md)
* Para uma introdução ao utilizar o Azure PowerShell quando estiver a implementar recursos, consulte [utilizar o Azure PowerShell com o Azure Resource Manager](powershell-azure-resource-manager.md).
* Para uma introdução ao utilizar a CLI do Azure quando estiver a implementar recursos, consulte [utilizando a CLI do Azure para Mac, Linux e Windows com o Azure Resource Manager](xplat-cli-azure-resource-manager.md).
* Para uma introdução ao utilizar o portal, consulte [no portal do Azure para gerir os recursos do Azure](resource-group-portal.md).  
* Para obter documentação de orientação sobre como as empresas podem utilizar o Resource Manager para gerir subscrições de forma eficaz, consulte [Azure enterprise scaffold - prescriptive subscription governance (Andaime empresarial do Azure - governação de subscrições prescritivas)](/azure/architecture/cloud-adoption-guide/subscription-governance).
