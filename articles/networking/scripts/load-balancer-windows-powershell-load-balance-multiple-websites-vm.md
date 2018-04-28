---
title: Exemplo de Script do PowerShell do Azure - balanceamento de carga de vários sites com o Azure PowerShell | Microsoft Docs
description: Exemplo de Script do PowerShell do Azure - balanceamento de carga de vários sites na mesma máquina virtual
services: load-balancer
documentationcenter: load-balancer
author: georgewallace
manager: timlt
editor: tysonn
tags: ''
ms.assetid: ''
ms.service: load-balancer
ms.devlang: powershell
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/16/2017
ms.author: gwallace
ms.openlocfilehash: 44cb76a09cdaacd1bffd653bae6a5735a81dc6f6
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="load-balance-multiple-websites"></a>Balanceamento de carga de vários sites

Este script de exemplo cria uma rede virtual com duas máquinas virtuais (VM) que são membros de um conjunto de disponibilidade. Um balanceador de carga direciona o tráfego de dois endereços IP separados para as duas VMs. Depois de executar o script, pode implementar o software de servidor web para as VMs e anfitrião vários web sites, cada um com o seu próprio endereço IP.

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/load-balancer/load-balance-multiple-web-sites-vm/load-balance-multiple-web-sites-vm.ps1  "Load balance multiple web sites")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, rede virtual, o Balanceador de carga e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [Novo AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Cria um conjunto para fornecer elevada disponibilidade de disponibilidade do Azure. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Cria uma configuração de sub-rede. Esta configuração é utilizada com o processo de criação de rede virtual. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Cria uma rede virtual. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Cria um endereço IP público. |
| [New-AzureRmLoadBalancerFrontendIpConfig](/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) | Cria uma configuração IP de front-end para um balanceador de carga. |
| [New-AzureRmLoadBalancerBackendAddressPoolConfig](/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) | Cria uma configuração de conjunto de endereços de back-end para um balanceador de carga. |
| [Novo AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Cria uma pesquisa NLB. É utilizada uma pesquisa NLB para monitorizar cada VM no conjunto do NLB. Se qualquer VM ficar inacessível, o tráfego não é encaminhado para a VM. |
| [Novo AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Cria uma regra do NLB. Neste exemplo, é criada uma regra para a porta 80. À medida que o tráfego HTTP chega ao NLB, é encaminhado para a porta 80 de uma das VMs no conjunto do NLB. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer) | Cria um balanceador de carga. |
| [Novo AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/new-azurermnetworkinterfaceipconfig) | Define as funcionalidades avançadas para uma interface de rede virtual. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Cria uma interface de rede. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Cria uma configuração de VM. Esta configuração inclui informações como o nome da VM, sistema operativo e credenciais administrativas. A configuração é utilizada durante a criação da VM. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cria uma máquina virtual. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos no grupo. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Exemplos de script do PowerShell redes adicionais podem ser encontrados no [documentação de descrição geral de funcionamento em rede do Azure](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
