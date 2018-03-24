---
title: Exemplo de script do PowerShell do Azure - tráfego de rede de VM de filtro | Microsoft Docs
description: Script do PowerShell do Azure de exemplo - filtrar o tráfego de rede VM de entrada e saído.
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
ms.openlocfilehash: 13089e0194b62a1f2f7a7c592d3ff21af8495dd1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="filter-inbound-and-outbound-vm-network-traffic"></a>Filtrar o tráfego de rede VM de entrada e saído

Este script de exemplo cria uma rede virtual com as sub-redes de front-end e back-end. Tráfego de rede de entrada para a sub-rede do front-end está limitado a HTTP e HTTPS, enquanto o tráfego de saída à internet da sub-rede de back-end não é permitido. Depois de executar o script, terá de uma máquina virtual com dois NICs. Cada NIC está ligado a outra sub-rede.

Pode executar o script a partir do Azure [nuvem Shell](https://shell.azure.com/powershell), ou a partir de uma instalação local do PowerShell. Se utilizar o PowerShell localmente, este script requer o AzureRM PowerShell versão do módulo 5.4.1 ou posterior. Para localizar a versão instalada, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo

[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/filter-network-traffic/filter-network-traffic.ps1  "Filter VM network traffic")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, VM e todos os recursos relacionados:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma rede virtual e os grupos de segurança de rede. Cada comando nas seguintes ligações de tabela para a documentação específica do comando:

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Cria um objeto de configuração de sub-rede |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Cria uma rede virtual do Azure e a sub-rede do front-end. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Cria regras de segurança a ser atribuída a um grupo de segurança de rede. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Cria regras do NSG que permitem ou bloquear portas específicas para sub-redes específicas. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Associa os NSGs a sub-redes. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Cria um endereço IP público para aceder a VM a partir da internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Cria interfaces de rede virtual e anexa-las a sub-redes de front-end e back-end à rede virtual. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Cria uma configuração de VM. Esta configuração inclui informações como o nome VM, sistema operativo e as credenciais administrativas. A configuração é utilizada durante a criação da VM. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Crie uma máquina virtual. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Exemplos de script do PowerShell de rede virtual adicionais podem ser encontrados na [exemplos do PowerShell de rede Virtual](../powershell-samples.md).