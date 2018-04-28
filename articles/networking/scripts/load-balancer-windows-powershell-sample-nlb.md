---
title: Exemplo de Script do PowerShell do Azure - carga equilibrar o tráfego para VMs de elevada disponibilidade | Microsoft Docs
description: Exemplo de Script do PowerShell do Azure - carga equilibrar o tráfego para VMs de elevada disponibilidade
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
ms.openlocfilehash: 61d65cbdcf7867cc7a2a225648dc8b1ab7137bcb
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
---
# <a name="load-balance-traffic-to-vms-for-high-availability"></a>Tráfego de balanceamento de carga para as VMs para elevada disponibilidade

Este script de exemplo cria tudo necessárias para executar várias máquinas virtuais do Windows que estão configuradas no altamente disponível e a configuração com balanceamento de carga. Depois de executar o script, terá três máquinas virtuais, associadas a um Conjunto de Disponibilidade do Azure e acessíveis através de um Balanceador de Carga do Azure.

Se for preciso, instale o Azure PowerShell com a instrução que se encontra no [Guia do Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/) e, em seguida, execute `Connect-AzureRmAccount` para criar uma ligação ao Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-nlb/create-vm-nlb.ps1 "Quick Create VM")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma máquina virtual, um conjunto de disponibilidade, um balanceador de carga e todos os recursos relacionados. Cada comando na tabela liga à documentação específica do comando.

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Cria uma configuração de sub-rede. Esta configuração é utilizada com o processo de criação de rede virtual. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Cria uma rede e sub-rede virtual do Azure. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)  | Cria um endereço IP público com um endereço IP estático e um nome DNS associado. |
| [New-AzureRmLoadBalancer](/powershell/module/azurerm.network/new-azurermloadbalancer)  | Cria um balanceador de carga do Azure. |
| [Novo AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) | Cria uma sonda do Balanceador de carga. Uma sonda do Balanceador de carga é utilizada para monitorizar cada VM no conjunto de Balanceador de carga. Se qualquer VM ficar inacessível, o tráfego não é encaminhado para a VM. |
| [Novo AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) | Cria uma regra de Balanceador de carga. Neste exemplo, é criada uma regra para a porta 80. Como o tráfego HTTP chega ao balanceador de carga, é encaminhado para a porta 80 uma das VMs no conjunto de Balanceador de carga. |
| [Novo AzureRmLoadBalancerInboundNatRuleConfig](/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) | Cria uma regra de tradução de endereços de rede (NAT) do Balanceador de carga.  As regras NAT de mapeiam uma porta de Balanceador de carga para uma porta numa VM. Neste exemplo, é criada uma regra NAT para o tráfego SSH para cada VM no conjunto de Balanceador de carga.  |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Cria um grupo de segurança de rede (NSG), que é um limite de segurança entre a Internet e a máquina virtual. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Cria uma regra NSG para permitir tráfego de entrada. Neste exemplo, a porta 22 está aberta para o tráfego SSH. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Cria a placa da rede virtual e liga-a à rede virtual, à sub-rede e ao NSG. |
| [Novo AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset) | Cria um conjunto de disponibilidade. Os conjuntos de disponibilidade garantem o tempo de atividade das aplicações ao propagar as máquinas virtuais nos recursos físicos de modo a que, se ocorrer uma falha, o conjunto completo não seja afetado. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Cria uma configuração de VM. Esta configuração inclui informações como o nome da VM, sistema operativo e credenciais administrativas. A configuração é utilizada durante a criação da VM. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm)  | Cria a máquina virtual e liga-a à placa de rede, à rede virtual, à sub-rede e ao NSG. Este comando também especifica a imagem de máquina virtual a ser utilizada e as credenciais administrativas.  |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Elimina um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Exemplos de script do PowerShell redes adicionais podem ser encontrados no [documentação de descrição geral de funcionamento em rede do Azure](../powershell-samples.md?toc=%2fazure%2fnetworking%2ftoc.json).
