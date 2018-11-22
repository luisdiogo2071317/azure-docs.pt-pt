---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 5c2c7b621512be7b81d14b99069d52f4f3aa3f33
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52279845"
---
| Recurso | Limite Predefinido | Limite Máximo |
| --- | --- | --- |
| Recursos por [grupo de recursos](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (por tipo de recurso) |800 |Varia por tipo de recurso |
| Implementações por grupo de recursos no histórico de implementação |800 |800 |
| Recursos por implementação |800 |800 |
| Bloqueios de gestão (por âmbito exclusivo) |20 |20 |
| Número de etiquetas (por recurso ou grupo de recursos) |15 |15 |
| Comprimento de chave de etiqueta |512 |512 |
| Comprimento do valor de etiqueta |256 |256 |


#### <a name="template-limits"></a>Limites do modelo

| Valor | Limite Predefinido | Limite Máximo |
| --- | --- | --- |
| Parâmetros |256 |256 |
| Variáveis |256 |256 |
| Recursos (incluindo o número de cópias) |800 |800 |
| Saídas |64 |64 |
| Expressão de modelo |24,576 carateres |24,576 carateres |
| Recursos em Modelos exportados |200 |200 | 
| Tamanho do modelo |1 MB |1 MB |
| Tamanho do ficheiro de parâmetro |64 KB |64 KB |

Pode exceder alguns limites de modelo ao utilizar um modelo aninhado. Para obter mais informações, consulte [utilizar modelos ligados durante a implantação de recursos do Azure](../articles/azure-resource-manager/resource-group-linked-templates.md). Para reduzir o número de parâmetros, variáveis ou saídas, pode combinar diversos valores num objeto. Para obter mais informações, consulte [objetos como parâmetros](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

Se atingir o limite de 800 implementações por grupo de recursos, elimine as implementações do histórico de que já não são necessários. Pode eliminar as entradas do histórico com [eliminar a implementação do grupo az](/cli/azure/group/deployment#az_group_deployment_delete) para a CLI do Azure, ou [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) no PowerShell. Elimina uma entrada do histórico de implementação não afeta os recursos de implementar. 