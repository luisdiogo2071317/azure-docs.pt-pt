---
title: Exemplos do Azure PowerShell - conjunto de dimensionamento com redundância de zona | Microsoft Docs
description: Exemplos do Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: de0e4e68033559a450d16fe531deff063923f5e0
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38652250"
---
# <a name="create-a-zone-redundant-virtual-machine-scale-set-with-powershell"></a>Criar um conjunto de dimensionamento de máquinas virtuais com redundância de zona com o PowerShell
Este script cria um conjunto de dimensionamento de máquinas virtuais com o Windows Server 2016 em várias Zonas de Disponibilidade. Depois de executar o script, pode aceder à máquina virtual através de RDP.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/create-zone-redundant-scale-set/create-zone-redundant-scale-set.ps1 "Create zone-redundant scale set")]

## <a name="clean-up-deployment"></a>Limpar a implementação
Execute o seguinte comando para remover o grupo de recursos, o conjunto de dimensionamento e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script
Este script utiliza os seguintes comandos para criar a implementação. Cada item na tabela liga a documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvmss) | Cria o conjunto de dimensionamento de máquinas virtuais e todos os recursos de suporte, incluindo a rede virtual, o balanceador de carga e as regras NAT. |
| [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) | Obtém informações sobre um conjunto de dimensionamento de máquinas virtuais. |
| [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension) | Adiciona uma extensão da VM para que o Script Personalizado instale uma aplicação Web básica. |
| [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) | Atualiza o modelo do conjunto de dimensionamento de máquinas virtuais para aplicar a extensão da VM. |
| [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) | Obtém informações sobre o endereço IP público atribuído, utilizado pelo balanceador de carga. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos no grupo. |


## <a name="next-steps"></a>Passos seguintes
Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode encontrar exemplos adicionais do script do conjunto de dimensionamento de máquinas virtuais do PowerShell na [documentação do conjunto de dimensionamento de máquinas virtuais do Azure](../powershell-samples.md).