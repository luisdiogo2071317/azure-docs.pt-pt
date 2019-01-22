---
title: Exemplo de script do Azure PowerShell - encaminhar o tráfego através de uma aplicação virtual de rede | Microsoft Docs
description: Exemplo de script do Azure PowerShell - encaminhar o tráfego através de uma aplicação virtual de rede de firewall.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/20/2018
ms.author: jdial
ms.openlocfilehash: ca7374eea99428b7431242f89e7cef53f760d7d5
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54423930"
---
# <a name="route-traffic-through-a-network-virtual-appliance-script-sample"></a>Encaminhar o tráfego através de um exemplo de script da aplicação virtual de rede

Este script de exemplo cria uma rede virtual com as sub-redes de front-end e back-end. Também cria uma VM com o reencaminhamento de IP ativado para encaminhar o tráfego entre as duas sub-redes. Depois de executar o script pode implementar o software de rede, como uma aplicação de firewall, na VM.

Pode executar o script a partir do [Azure Cloud Shell](https://shell.azure.com/powershell) ou a partir de uma instalação local do PowerShell. Se utilizar o PowerShell localmente, este script requer a versão 5.4.1 ou posterior do módulo AzureRM PowerShell. Para localizar a versão instalada, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/azurerm/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo


[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/route-traffic-through-nva/route-traffic-through-nva.ps1 "Route traffic through a network virtual appliance")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```
## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma rede virtual e grupos de segurança de rede. Cada comando na tabela seguinte liga à documentação específica do comando:

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)  | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Cria uma rede e sub-rede virtual de front-end do Azure. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Crias as sub-redes de back-end e DMZ. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Cria um endereço IP público para aceder à VM a partir da Internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Cria uma interface de rede virtual e ativa o reencaminhamento de IP para a mesma. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Cria um grupo de segurança de rede (NSG). |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Cria regras de NSG que permitem as portas HTTP e HTTPS de entrada para a VM. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig)| Associa os NSGs e as tabelas de rotas às sub-redes. |
| [New-AzureRmRouteTable](/powershell/module/azurerm.network/new-azurermroutetable)| Cria uma tabela de rotas para todas as rotas. |
| [New-AzureRMRouteConfig](/powershell/module/azurerm.network/new-azurermrouteconfig)| Cria rotas para encaminhar o tráfego entre as sub-redes e a Internet através da VM. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cria uma máquina virtual e anexa o NIC à mesma. Este comando também especifica a imagem da máquina virtual a utilizar e as credenciais administrativas. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup)  | Elimina um grupo de recursos e todos os recursos contidos no mesmo. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

Pode ver exemplos adicionais de scripts do PowerShell de rede virtual em [Exemplos do PowerShell de rede virtual](../powershell-samples.md).
