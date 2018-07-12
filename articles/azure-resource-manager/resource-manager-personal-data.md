---
title: Os dados pessoais do Azure Resource Manager | Documentos da Microsoft
description: Saiba como gerir dados pessoais associados a operações do Azure Resource Manager.
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
ms.date: 05/14/2018
ms.author: tomfitz
ms.openlocfilehash: 71928be07080ed14fdcb93f33ea64d2572955b53
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38466029"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Gerir dados pessoais associados com o Azure Resource Manager

Para evitar expor informações confidenciais, elimine quaisquer informações pessoais, que pode ter fornecido em implementações, grupos de recursos ou etiquetas. O Azure Resource Manager proporciona operações que lhe permitem gerir dados pessoais que tenha fornecido em implementações, grupos de recursos ou etiquetas.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Eliminar os dados pessoais no histórico de implementação

Para implementações, o Resource Manager mantém valores de parâmetros e as mensagens de estado no histórico de implementação. Estes valores persistirem até que elimine a implementação do histórico. Para ver se forneceu os dados pessoais nestes valores, liste as implementações. Se localizar dados pessoais, elimine as implementações do histórico.

A lista **implementações** na história, utilize:

* [Lista por grupo de recursos](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Get-AzureRmResourceGroupDeployment)
* [lista de implementação do grupo AZ](/cli/azure/group/deployment#az-group-deployment-list)

Para eliminar **implementações** a partir do histórico, utilize:

* [Eliminar](/rest/api/resources/deployments/delete)
* [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroupDeployment)
* [eliminar a implementação do grupo AZ](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Eliminar dados pessoais em nomes de grupo de recursos

O nome do grupo de recursos persiste até que elimine o grupo de recursos. Para ver se forneceu os dados pessoais nos nomes, liste os grupos de recursos. Se encontrar os dados pessoais [mover os recursos](resource-group-move-resources.md) para um novo grupo de recursos e a eliminar o grupo de recursos com os dados pessoais no nome.

A lista **grupos de recursos**, utilize:

* [Lista](/rest/api/resources/resourcegroups/list)
* [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/Get-AzureRmResourceGroup)
* [lista de grupos de AZ](/cli/azure/group#az-group-list)

Para eliminar **grupos de recursos**, utilize:

* [Eliminar](/rest/api/resources/resourcegroups/delete)
* [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Eliminar dados pessoais em etiquetas

Nomes de etiquetas e valores persistirem até que elimine ou modifique a marca. Para ver se forneceu os dados pessoais nas etiquetas, liste as etiquetas. Se localizar dados pessoais, elimine as etiquetas.

A lista **etiquetas**, utilize:

* [Lista](/rest/api/resources/tags/list)
* [Get-AzureRmTag](/powershell/module/azurerm.tags/get-azurermtag)
* [lista de etiquetas de AZ](/cli/azure/tag#az-tag-list)

Para eliminar **etiquetas**, utilize:

* [Eliminar](/rest/api/resources/tags/delete)
* [Remove-AzureRmTag](/powershell/module/azurerm.tags/remove-azurermtag)
* [eliminação de marca de AZ](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Passos Seguintes
* Para obter uma descrição geral do Azure Resource Manager, consulte o [o que é o Resource Manager?](resource-group-overview.md)