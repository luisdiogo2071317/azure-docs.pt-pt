---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 87b9bd74fc59c86bf177e45c18bfc4e104d9c996
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
Para adicionar duas etiquetas a um grupo de recursos, utilize:

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Vamos imaginar que pretende adicionar uma etiqueta de terceira. Sempre que aplicar etiquetas a um recurso ou grupo de recursos, as etiquetas existentes nesse recurso ou grupo de recursos são substituídas. Para adicionar uma nova etiqueta sem perder as etiquetas existentes, tem de obter as etiquetas existentes, adicione uma nova etiqueta e volte a aplicar a coleção de etiquetas:

```powershell
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags += @{Project="Documentation"}
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Recursos não herdam as etiquetas do grupo de recursos. Atualmente, o grupo de recursos tem três etiquetas, mas os recursos não tem quaisquer etiquetas. Para aplicar a todas as etiquetas de um grupo de recursos para os respetivos recursos e manter as etiquetas existentes em recursos que não são duplicados, utilize o seguinte script:

```powershell
$group = Get-AzureRmResourceGroup myResourceGroup
if ($group.Tags -ne $null) {
    $resources = $group | Find-AzureRmResource
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        foreach ($key in $group.Tags.Keys)
        {
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }
        $resourcetags += $group.Tags
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

Em alternativa, pode aplicar etiquetas do grupo de recursos para os recursos sem manter as etiquetas existentes:

```powershell
$g = Get-AzureRmResourceGroup -Name myResourceGroup
Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Combinar uma única tag de vários valores, utilize uma cadeia JSON.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Para remover todas as etiquetas, passa uma tabela hash vazio.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```
