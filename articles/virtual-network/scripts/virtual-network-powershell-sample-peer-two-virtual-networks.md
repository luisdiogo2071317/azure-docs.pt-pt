---
title: Exemplo de script do PowerShell do Azure - redes virtuais ponto a ponto dois | Microsoft Docs
description: Exemplo de script do PowerShell do Azure - redes virtuais do elemento de rede dois
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: c0efdf759a0bdb87de4dc8ff9566a8e817503c5e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="peer-two-virtual-networks"></a>Elemento duas redes virtuais

Este script cria e liga duas redes virtuais na mesma região através da rede do Azure. Depois de executar o script, irá criar um peering entre duas redes virtuais.

Pode executar o script a partir do Azure [nuvem Shell](https://shell.azure.com/powershell), ou a partir de uma instalação local do PowerShell. Se utilizar o PowerShell localmente, este script requer o AzureRM PowerShell versão do módulo 5.4.1 ou posterior. Para localizar a versão instalada, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/peer-two-virtual-networks/peer-two-virtual-networks.ps1 "Peer two networks")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, VM e todos os recursos relacionados:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma máquina virtual e todos os recursos relacionados. Cada comando nas seguintes ligações de tabela para a documentação específica de comando:

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. | 
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)| Cria uma rede e sub-rede virtual do Azure. |
| [Add-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering) | Cria um peering entre duas redes virtuais.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Exemplos de script do PowerShell de rede virtual adicionais podem ser encontrados na [exemplos do PowerShell de rede Virtual](../powershell-samples.md).