---
title: Consultas de introdução do Azure Resource Graph
description: Utilize o Azure Resource Graph para executar algumas consultas de introdução.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: bcc6c6e9d38948cb4be09c9f52049790ccd5ac7e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227032"
---
# <a name="starter-resource-graph-queries"></a>Consultas de introdução do Azure Resource Graph

O primeiro passo para compreender as consultas com Azure Resource Graph é obter uma compreensão básica da [Linguagem de Consulta](../concepts/query-language.md). Se ainda não estiver familiarizado com o [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md), recomenda-se que reveja os conceitos básicos para compreender como compor pedidos para os recursos que está a procurar.

Vamos examinar as seguintes consultas de introdução:

> [!div class="checklist"]
> - [Contar recursos do Azure](#count-resources)
> - [Listar recursos ordenados por nome](#list-resources)
> - [Mostrar todas as máquinas virtuais ordenadas por nome em ordem descendente](#show-vms)
> - [Mostrar as primeiras cinco máquinas virtuais por nome e por tipo de SO](#show-sorted)
> - [Contar máquinas virtuais por tipo de SO](#count-os)
> - [Mostrar recursos que contêm armazenamento](#show-storage)
> - [Listar todos os endereços IP públicos](#list-publicip)
> - [Contar recursos que tenham endereços IP configurados por subscrição](#count-resources-by-ip)
> - [Listar recursos com um valor de etiqueta específico](#list-tag)
> - [Listar todas as contas de armazenamento com um valor de etiqueta específico](#list-specific-tag)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free) antes de começar.

## <a name="language-support"></a>Suporte de idiomas

A CLI do Azure (por meio de uma extensão) e o Azure PowerShell (por meio de um módulo) suportam o Azure Resource Graph. Antes de executar qualquer uma das seguintes consultas, verifique se o ambiente está pronto. Veja [CLI do Azure](../first-query-azurecli.md#add-the-resource-graph-extension) e [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) para obter os passos para instalar e validar o seu ambiente shell escolhido.

## <a name="count-resources"></a>Contar recursos do Azure

Esta consulta devolve o número de recursos do Azure que existem nas subscrições às quais tem acesso. Também é uma boa consulta a utilizar para confirmar que a shell escolhida tem os componentes apropriados do Azure Resource Graph instalados e em condições de funcionamento.

```Query
summarize count()
```

```azurecli-interactive
az graph query -q "summarize count()"
```

```powershell
Search-AzureRmGraph -Query "summarize count()"
```

## <a name="list-resources"></a>Listar recursos ordenados por nome

Sem se limitar a qualquer tipo de recurso ou de propriedade correspondente específica, esta consulta devolve apenas o **nome**, o **tipo** e a **localização** dos recursos do Azure, mas utiliza `order by` para os ordenar pela propriedade **nome** em ordem (`asc`) ascendente.

```Query
project name, type, location
| order by name asc
```

```azurecli-interactive
az graph query -q "project name, type, location | order by name asc"
```

```powershell
Search-AzureRmGraph -Query "project name, type, location | order by name asc"
```

## <a name="show-vms"></a>Mostrar todas as máquinas virtuais ordenadas por nome em ordem descendente

Em vez de obter todos os recursos do Azure, se quiséssemos apenas uma lista de máquinas virtuais (do tipo `Microsoft.Compute/virtualMachines`), poderíamos fazer corresponder a propriedade **tipo** nos resultados.
À semelhança da consulta anterior, `desc` altera `order by` para ser descendente. O `=~` na correspondência de tipo informa o Resource Graph para não ser sensível a maiúsculas e minúsculas.

```Query
project name, location, type
| where type =~ 'Microsoft.Compute/virtualMachines'
| order by name desc
```

```azurecli-interactive
az graph query -q "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

```powershell
Search-AzureRmGraph -Query "project name, location, type| where type =~ 'Microsoft.Compute/virtualMachines' | order by name desc"
```

## <a name="show-sorted"></a>Mostrar as primeiras cinco máquinas virtuais por nome e por tipo de SO

Esta consulta vai utilizar `limit` para obter apenas cinco registos correspondentes, ordenados por nome. O tipo do recurso do Azure é `Microsoft.Compute/virtualMachines`. `project` informa o Azure Resource Graph quais as propriedades a incluir.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| project name, properties.storageProfile.osDisk.osType
| top 5 by name desc
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | project name, properties.storageProfile.osDisk.osType | top 5 by name desc"
```

## <a name="count-os"></a>Contar máquinas virtuais por tipo de SO

Aproveitando a consulta anterior, estamos ainda limitados pelos recursos do Azure do tipo `Microsoft.Compute/virtualMachines`, mas já não há limitação sobre o número de registros devolvidos.
Em vez disso, utilizamos `summarize` e `count()` para definir como agrupar e agregar os valores pela propriedade, que, neste exemplo, é `properties.storageProfile.osDisk.osType`. Para obter um exemplo do aspeto desta cadeia de caracteres no objeto completo, veja [explorar recursos – deteção de máquinas virtuais](../concepts/explore-resources.md#virtual-machine-discovery).

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| summarize count() by tostring(properties.storageProfile.osDisk.osType)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)"
```

Uma forma diferente de escrever a mesma consulta é `extend` a uma propriedade e dar-lhe um nome temporário para utilizar dentro da consulta, neste caso, **so**. **so** é, em seguida, utilizado por `summarize` e `count()` como no exemplo anterior.

```Query
where type =~ 'Microsoft.Compute/virtualMachines'
| extend os = properties.storageProfile.osDisk.osType
| summarize count() by tostring(os)
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Compute/virtualMachines' | extend os = properties.storageProfile.osDisk.osType | summarize count() by tostring(os)"
```

> [!NOTE]
> Esteja ciente de que, embora `=~` permita a correspondência não sensível a maiúsculas e minúsculas, a utilização de propriedades (como **properties.storageProfile.osDisk.osType**) na consulta requer que a utilização de maiúsculas/minúsculas esteja correta. Se a propriedade tiver uma utilização incorreta das maiúsculas/minúsculas, poderá ainda devolver um valor, mas o agrupamento ou o resumo estarão incorretos.

## <a name="show-storage"></a>Mostrar recursos que contêm armazenamento

Em vez de definir explicitamente o tipo a corresponder, esta consulta de exemplo vai procurar qualquer recurso do Azure que `contains` a palavra **armazenamento**.

```Query
where type contains 'storage' | distinct type
```

```azurecli-interactive
az graph query -q "where type contains 'storage' | distinct type"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'storage' | distinct type"
```

## <a name="list-publicip"></a>Listar todos os endereços IP públicos

À semelhança da consulta anterior, encontra tudo o que era de um tipo que contém a palavra **publicIPAddresses**. Esta consulta expande esse padrão para excluir resultados em que **properties.ipAddress** é nulo, para devolver apenas **properties.ipAddress** e para `limit` os resultados aos 100 principais. Poderá ter de evitar as aspas consoante a shell escolhida.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| project properties.ipAddress
| limit 100
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | project properties.ipAddress | limit 100"
```

## <a name="count-resources-by-ip">Contar recursos que tenham endereços IP configurados por subscrição</a>

Com a consulta do exemplo anterior e ao adicionar `summarize` e `count()`, podemos obter uma lista por subscrição dos recursos com endereços IP configurados.

```Query
where type contains 'publicIPAddresses' and properties.ipAddress != ''
| summarize count () by subscriptionId
```

```azurecli-interactive
az graph query -q "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

```powershell
Search-AzureRmGraph -Query "where type contains 'publicIPAddresses' and properties.ipAddress != '' | summarize count () by subscriptionId"
```

## <a name="list-tag"></a>Listar recursos com um valor de etiqueta específico

Podemos limitar os resultados por propriedades que não sejam do tipo de recurso do Azure, como uma etiqueta. Neste exemplo, está a filtrar recursos do Azure com um nome de etiqueta de **Ambiente** que têm um valor de **Interno**.

```Query
where tags.environment=~'internal'
| project name
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name"
```

```powershell
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name"
```

Caso tenha sido necessário indicar também quais as etiquetas que o recurso tinha e os seus valores, este exemplo poderia ser expandido, ao adicionar a propriedade **etiquetas** à palavra-chave `project`.

```Query
where tags.environment=~'internal'
| project name, tags
```

```azurecli-interactive
az graph query -q "where tags.environment=~'internal' | project name, tags"
```

```powershell
Search-AzureRmGraph -Query "where tags.environment=~'internal' | project name, tags"
```

## <a name="list-specific-tag"></a>Listar todas as contas de armazenamento com um valor de etiqueta específico

Combinar a capacidade de filtro do exemplo anterior com a utilização da filtragem por tipo de recurso do Azure por propriedade **tipo** pode limitar a nossa pesquisa para tipos específicos de recursos do Azure com um nome de etiqueta e um valor específicos.

```Query
where type =~ 'Microsoft.Storage/storageAccounts'
| where tags['tag with a space']=='Custom value'
```

```azurecli-interactive
az graph query -q "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

```powershell
Search-AzureRmGraph -Query "where type =~ 'Microsoft.Storage/storageAccounts' | where tags['tag with a space']=='Custom value'"
```

> [!NOTE]
> Este exemplo utiliza `==` para a correspondência em vez do `=~` condicional. `==` é uma correspondência sensível a maiúsculas e minúsculas.

## <a name="next-steps"></a>Passos seguintes

- Saber mais sobre a [linguagem de consulta](../concepts/query-language.md)
- Aprender a [explorar recursos](../concepts/explore-resources.md)
- Ver exemplos de [Consultas avançadas](advanced.md)