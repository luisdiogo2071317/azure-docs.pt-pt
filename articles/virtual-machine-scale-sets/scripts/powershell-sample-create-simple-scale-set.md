---
title: Exemplos do Azure PowerShell - Criar um conjunto de dimensionamento de máquinas virtuais básico | Microsoft Docs
description: Exemplos do Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d6417803ae465ec167f1f8980aae84629715c7d7
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
ms.locfileid: "30246292"
---
# <a name="create-a-basic-virtual-machine-scale-set-with-powershell"></a>Criar um conjunto de dimensionamento de máquinas virtuais básico com o PowerShell
Este script cria um conjunto de dimensionamento de máquinas virtuais com o Windows Server 2016. Depois de executar o script, pode aceder às instâncias de VMs através de RDP.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo
[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/simple-scale-set/simple-scale-set.ps1 "Create a simple virtual machine scale set")]

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
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos no grupo. |

## <a name="next-steps"></a>Passos seguintes
Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode encontrar exemplos adicionais do script do conjunto de dimensionamento de máquinas virtuais do PowerShell na [documentação do conjunto de dimensionamento de máquinas virtuais do Azure](../powershell-samples.md).