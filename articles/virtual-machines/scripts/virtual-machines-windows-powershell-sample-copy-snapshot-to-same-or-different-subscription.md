---
title: Exemplo do Script do Azure PowerShell – Copiar (mover) o instantâneo de um disco gerido para uma subscrição idêntica ou diferente | Microsoft Docs
description: Exemplo do Script do Azure PowerShell – Copiar (mover) o instantâneo de um disco gerido para uma subscrição idêntica ou diferente
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 2905747b6ece93f018bd6ef67fae1b23ec9c9d5d
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/18/2018
ms.locfileid: "41918670"
---
# <a name="copy-snapshot-of-a-managed-disk-in-same-subscription-or-different-subscription-with-powershell"></a>Copiar o instantâneo de um disco gerido na mesma subscrição ou numa subscrição diferente com a CLI

Este script cria uma cópia de um instantâneo na mesma subscrição ou numa subscrição diferente. Utilize este script para mover um instantâneo para uma subscrição diferente para a retenção de dados. Armazenar instantâneos numa subscrição diferente protege-o contra a eliminação acidental de instantâneos na sua subscrição principal. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um instantâneo na subscrição de destino com o ID do instantâneo de origem. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmSnapshotConfig](/powershell/module/azurerm.compute/New-AzureRmSnapshotConfig) | Cria a configuração de instantâneo que é utilizada para a criação do instantâneo. Inclui o Id de recurso do instantâneo principal e a localização que é a mesma que o instantâneo principal.  |
| [New-AzureRmSnapshot](/powershell/module/azurerm.compute/New-AzureRmDisk) | Cria um instantâneo com a configuração de instantâneo, o nome do instantâneo e o nome do grupo de recursos transmitidos como parâmetros. |


## <a name="next-steps"></a>Passos seguintes

[Criar uma máquina virtual a partir de um instantâneo](./virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Para obter mais informações sobre o módulo do Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Pode ver exemplos adicionais de scripts do PowerShell da máquina virtual na [Documentação da VM Windows do Azure](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).