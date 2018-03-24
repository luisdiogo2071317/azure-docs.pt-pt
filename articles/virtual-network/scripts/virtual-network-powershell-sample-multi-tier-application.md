---
title: Script do PowerShell do Azure de exemplo - criar uma rede para aplicações de várias camadas | Microsoft Docs
description: Script do PowerShell do Azure de exemplo - criar uma rede virtual para aplicações de várias camadas.
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
ms.openlocfilehash: d393752c899274c3af845e8a52a965c19ac81dc4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="create-a-network-for-multi-tier-applications"></a>Criar uma rede para aplicações de várias camadas

Este script de exemplo cria uma rede virtual com as sub-redes de front-end e back-end. O tráfego para a sub-rede do front-end é limitado para HTTP e SSH, enquanto o tráfego para a sub-rede de back-end está limitado a MySQL, porta 3306. Depois de executar o script, tem duas máquinas virtuais, um em cada sub-rede que pode implementar MySQL software e do servidor web.

Pode executar o script a partir do Azure [nuvem Shell](https://shell.azure.com/powershell), ou a partir de uma instalação local do PowerShell. Se utilizar o PowerShell localmente, este script requer o AzureRM PowerShell versão do módulo 5.4.1 ou posterior. Para localizar a versão instalada, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo


[!code-azurepowershell-interactive[main](../../../powershell_scripts/virtual-network/virtual-network-multi-tier-application/virtual-network-multi-tier-application.ps1  "Virtual network for multi-tier application")]

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
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Cria uma rede virtual do Azure e a sub-rede do front-end. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Cria uma sub-rede de back-end. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Cria um endereço IP público para aceder a VM a partir da Internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Cria interfaces de rede virtual e anexa-las a sub-redes de front-end e back-end à rede virtual. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) | Cria grupos de segurança (NSG) que estão associados às sub-redes de front-end e back-end de rede. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) |Cria regras do NSG que permitem ou bloquear portas específicas para sub-redes específicas. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cria máquinas virtuais e anexa um NIC para cada VM. Este comando também especifica a imagem de máquina virtual para utilizar e credenciais administrativas. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Elimina um grupo de recursos e todos os recursos que nele contidos. |

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

Exemplos de script do PowerShell de rede virtual adicionais podem ser encontrados na [exemplos do PowerShell de rede Virtual](../powershell-samples.md).