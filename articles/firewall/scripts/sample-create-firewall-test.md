---
title: Exemplo de script do Azure PowerShell - Criar ambiente de teste do Azure Firewall
description: Exemplo de script do Azure PowerShell - Criar ambiente de teste do Azure Firewall.
services: virtual-network
author: vhorne
ms.service: firewall
ms.devlang: powershell
ms.topic: sample
ms.date: 8/13/2018
ms.author: victorh
ms.openlocfilehash: 63b34b6ddc1809031dc66fb3e41fa4a22d9f4a03
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182795"
---
# <a name="create-an-azure-firewall-test-environment"></a>Criar um ambiente de teste do Azure Firewall

Este exemplo de script cria uma firewall e um ambiente de rede de teste. A rede tem uma VNet, com três sub-redes: *AzureFirewallSubnet*, *ServersSubnet*e *JumpboxSubnet*. ServersSubnet e JumpboxSubnet têm um Windows Server de 2 núcleos em cada um.

A firewall está em AzureFirewallSubnet configurada com uma Coleção de Regras de Aplicação com uma única regra que permite o acesso a www.microsoft.com.

É criada uma rota definida pelo utilizador que aponta o tráfego de rede de ServersSubnet através da firewall, em que são aplicadas as regras de firewall.

Pode executar o script a partir do [Azure Cloud Shell](https://shell.azure.com/powershell) ou a partir de uma instalação local do PowerShell. 

Se executar o PowerShell localmente, este script requer a versão mais recente do módulo AzureRM PowerShell (6.9.0 ou superior). Para localizar a versão instalada, execute `Get-Module -ListAvailable AzureRM`. 

Pode utilizar o `PowerShellGet` se precisar de atualizar, o qual está incorporado no Windows 10 e no Windows Server 2016.

> [!NOTE]
>Outra versão do Windows requer a instalação do `PowerShellGet` antes de poder utilizá-lo. Pode executar `Get-Module -Name PowerShellGet -ListAvailable | Select-Object -Property Name,Version,Path` para determinar se está instalado no seu sistema. Se a saída estiver em branco, tem de instalar a versão mais recente do [Windows Management Framework](https://www.microsoft.com/download/details.aspx?id=54616).

Para obter mais informações, veja [Instalar o Azure PowerShell no Windows com o PowerShellGet](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps?view=azurermps-6.4.0)

Qualquer instalação existente do Azure PowerShell feita com o instalador de Plataforma Web entrará em conflito com a instalação do PowerShellGet e terá de ser removida.

Não se esqueça de que, se estiver a executar o PowerShell localmente, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de exemplo


[!code-azurepowershell-interactive[main](../../../powershell_scripts/firewall/create-fw-test.ps1  "Create a firewall test environment")]

## <a name="clean-up-deployment"></a>Limpar a implementação 

Execute o seguinte comando para remover o grupo de recursos, a VM e todos os recursos relacionados:

```powershell
Remove-AzureRmResourceGroup -Name AzfwSampleScriptEastUS -Force
```

## <a name="script-explanation"></a>Explicação do script

Este script utiliza os seguintes comandos para criar um grupo de recursos, uma rede virtual e grupos de segurança de rede. Cada comando na tabela seguinte liga à documentação específica do comando:

| Comando | Notas |
|---|---|
| [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) | Cria um objeto de configuração de sub-rede |
| [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) | Cria uma rede e sub-rede virtual de front-end do Azure. |
| [New-AzureRmNetworkSecurityRuleConfig](/powershell/module/azurerm.network/new-azurermnetworksecurityruleconfig) | Cria regras de segurança a serem atribuídas a um grupo de segurança de rede. |
| [New-AzureRmNetworkSecurityGroup](/powershell/module/azurerm.network/new-azurermnetworksecuritygroup) |Cria regras do NSG que permitem ou bloquear portas específicas para sub-redes específicas. |
| [Set-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig) | Associa os NSGs a sub-redes. |
| [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress) | Cria um endereço IP público para aceder à VM a partir da Internet. |
| [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) | Cria interfaces de rede virtual e anexa-as a sub-redes de front-end e back-end da rede virtual. |
| [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) | Cria uma configuração de VM. Esta configuração inclui informações como o nome da VM, sistema operativo e credenciais administrativas. A configuração é utilizada durante a criação da VM. |
| [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) | Cria uma máquina virtual. |
|[Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Remove um grupo de recursos e todos os recursos contidos no grupo. |
|[New-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewall?view=azurermps-6.9.0)| Cria um Azure Firewall novo.|
|[Get-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermfirewall?view=azurermps-6.9.0)|Obtém um objeto do Azure Firewall.|
|[New-AzureRmFirewallApplicationRule](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermfirewallapplicationrule?view=azurermps-6.9.0)|Cria uma regra de aplicação do Azure Firewall nova.|
|[Set-AzureRmFirewall](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermfirewall?view=azurermps-6.9.0)|Consolida alterações ao objeto do Azure Firewall.|


## <a name="next-steps"></a>Passos seguintes

Para obter mais informações sobre o Azure PowerShell, veja [Documentação do Azure PowerShell](/powershell/azure/overview).

