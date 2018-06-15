---
title: Os dados pessoais do Azure Resource Manager | Microsoft Docs
description: Saiba como gerir os dados pessoais associados com as operações do Azure Resource Manager.
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
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358630"
---
# <a name="manage-personal-data-associated-with-azure-resource-manager"></a>Gerir os dados pessoais associados com o Azure Resource Manager

Para evitar a exposição de informações confidenciais, elimine quaisquer informações pessoais que poderá forneceu em implementações, grupos de recursos ou etiquetas. O Azure Resource Manager proporciona operações que permitem-lhe gerir os dados pessoais que poderá forneceu em implementações, grupos de recursos ou etiquetas.

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="delete-personal-data-in-deployment-history"></a>Eliminar dados pessoais no histórico de implementação

Para implementações, o Gestor de recursos mantém os valores de parâmetros e mensagens de estado no histórico de implementação. Estes valores são mantidas até que elimine a implementação do histórico. Para ver se forneceu dados pessoais estes valores, lista as implementações. Se encontrar dados pessoais, elimine as implementações do histórico.

A lista **implementações** no histórico, utilize:

* [Lista por grupo de recursos](/rest/api/resources/deployments/listbyresourcegroup)
* [Get-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Get-AzureRmResourceGroupDeployment)
* [lista de implementação do grupo de AZ](/cli/azure/group/deployment#az-group-deployment-list)

Para eliminar **implementações** a partir do histórico, utilize:

* [Eliminar](/rest/api/resources/deployments/delete)
* [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroupDeployment)
* [eliminar a implementação do grupo AZ](/cli/azure/group/deployment#az-group-deployment-delete)

## <a name="delete-personal-data-in-resource-group-names"></a>Eliminar dados pessoais em nomes de grupo de recursos

O nome do grupo de recursos mantém até que elimine o grupo de recursos. Para ver se forneceu dados pessoais dos nomes, liste os grupos de recursos. Se encontrar dados pessoais, [mover os recursos](resource-group-move-resources.md) para um novo grupo de recursos e eliminar o grupo de recursos com os dados pessoais no nome.

A lista **grupos de recursos**, utilize:

* [lista](/rest/api/resources/resourcegroups/list)
* [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/Get-AzureRmResourceGroup)
* [lista de grupos de AZ](/cli/azure/group#az-group-list)

Para eliminar **grupos de recursos**, utilize:

* [Eliminar](/rest/api/resources/resourcegroups/delete)
* [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/Remove-AzureRmResourceGroup)
* [az group delete](/cli/azure/group#az-group-delete)

## <a name="delete-personal-data-in-tags"></a>Eliminar dados pessoais nas tags

Valores e nomes de etiquetas mantêm até eliminar ou modificar a etiqueta. Para ver se forneceu dados pessoais nas etiquetas, lista as etiquetas. Se encontrar dados pessoais, elimine as etiquetas.

A lista **etiquetas**, utilize:

* [lista](/rest/api/resources/tags/list)
* [Get-AzureRmTag](/powershell/module/azurerm.tags/get-azurermtag)
* [lista de etiquetas AZ](/cli/azure/tag#az-tag-list)

Para eliminar **etiquetas**, utilize:

* [Eliminar](/rest/api/resources/tags/delete)
* [Remover AzureRmTag](/powershell/module/azurerm.tags/remove-azurermtag)
* [eliminação de tag AZ](/cli/azure/tag#az-tag-delete)

## <a name="next-steps"></a>Passos Seguintes
* Para obter uma descrição geral do Gestor de recursos do Azure, consulte o [que é o Gestor de recursos?](resource-group-overview.md)