---
title: Exemplo de script do PowerShell do Azure - encaminhar o tráfego através de uma aplicação virtual de rede | Microsoft Docs
description: Azure PowerShell script de exemplo - encaminhar o tráfego através de uma aplicação virtual de rede de firewall.
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
ms.openlocfilehash: 766d800a7875536b729f564a3d9948e5598de6aa
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="route-traffic-through-a-network-virtual-appliance"></a>Encaminhar o tráfego através de uma aplicação virtual de rede

Este script de exemplo cria uma rede virtual com as sub-redes de front-end e back-end. Também cria uma VM com o reencaminhamento IP está ativado para encaminhar o tráfego entre as duas sub-redes. Depois de executar o script pode implementar o software de rede, como uma aplicação de firewall, para a VM.

Pode executar o script a partir do Azure [nuvem Shell](https://shell.azure.com/powershell), ou a partir de uma instalação local do PowerShell. Se utilizar o PowerShell localmente, este script requer o AzureRM PowerShell versão do módulo 5.4.1 ou posterior. Para localizar a versão instalada, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo


[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, VM e todos os recursos relacionados:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```
## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma rede virtual e os grupos de segurança de rede. Cada comando nas seguintes ligações de tabela para a documentação específica do comando:

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Cria uma rede virtual do Azure e a sub-rede do front-end. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Cria o back-end e sub-redes da rede de Perímetro. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Cria um endereço IP público para aceder a VM a partir da internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Cria uma interface de rede virtual e ativar o reencaminhamento de IP para o mesmo. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Cria um grupo de segurança de rede (NSG). |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Cria regras do NSG que permitem que as portas HTTP e HTTPS de entrada para a VM. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Associa os NSGs e tabelas de rotas para sub-redes. |
| [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Cria uma tabela de rota para todas as rotas. |
| [New-AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Cria rotas para encaminhar o tráfego entre sub-redes e a internet através da VM. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cria uma máquina virtual e anexa a NIC ao mesmo. Este comando também especifica a imagem de máquina virtual para utilizar e credenciais administrativas. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Elimina um grupo de recursos e todos os recursos que nele contidos. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Exemplos de script do PowerShell de rede virtual adicionais podem ser encontrados na [exemplos do PowerShell de rede Virtual](../powershell-samples.md).