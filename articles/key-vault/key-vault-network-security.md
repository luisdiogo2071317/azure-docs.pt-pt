---
ms.assetid: ''
title: Configurar o Cofre de chaves do Azure Firewalls e redes virtuais
description: Instruções passo a passo para configurar o Cofre de chaves firewalls e redes virtuais
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/31/2018
ms.author: ambapat
ms.openlocfilehash: c58fc56742c0a11771bdd84e4195e6f476622a3b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345185"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Configurar o Cofre de chaves do Azure Firewalls e redes virtuais

Este guia descreve as instruções passo a passo para configurar o Cofre de chaves firewalls e redes virtuais para restringir o acesso ao seu Cofre de chaves. O [pontos finais de serviço de rede Virtual para o Key Vault](key-vault-overview-vnet-service-endpoints.md) permitem-lhe restringir o acesso ao Cofre de chaves para a rede Virtual especificada e/ou um conjunto de intervalos de endereços IPv4 (protocolo IP versão 4).

> [!IMPORTANT]
> Assim que a firewall e regras de rede virtual estão em vigor, todos os Key Vault [plano de dados](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) operações só podem ser executadas quando o chamador pedidos provêm da rede ou redes virtuais permitidas ou intervalos de endereços IPV4. Isso também se aplica a aceder ao Cofre de chaves a partir do portal do Azure. Enquanto um usuário pode browser para um cofre de chaves a partir do portal do Azure, eles talvez não consiga lista chaves/segredos/certificados se os respetivos computadores cliente não está na lista de permitidos. Isso também afeta o 'selecionador de Cofre de chave' por outros serviços do Azure. Os utilizadores podem ser capazes de ver a lista de cofres de chaves, mas não listar as chaves, se as regras de firewall impedem os respetivos computadores cliente.

## <a name="azure-portal"></a>Portal do Azure

1. Navegue para o Cofre de chaves que pretende proteger.
2. Clique em **Firewalls e redes virtuais**.
3. Clique em **redes selecionadas** sob **autorizar o acesso**.
4. Para adicionar redes virtuais existentes para firewalls e regras de rede virtual, clique em **+ adicionar redes virtuais existentes**.
5. No novo painel que aparece, selecione a subscrição, rede ou redes virtuais e sub-redes que pretende permitir o acesso a este Cofre de chaves. Se a rede ou redes virtuais e sub-redes que selecionou, não têm pontos finais de serviço ativados, verá uma mensagem a indicar, "as redes seguintes não têm enavled de pontos finais de serviço...". Clique em **ativar** depois de confirmar que pretende ativar pontos finais de serviço listadas para a rede ou redes virtuais e sub-redes. Pode demorar até 15 minutos para entrar em vigor.
6. Pode também adicionar nova rede ou de redes virtuais e sub-redes e, em seguida, ativar pontos finais de serviço para a rede ou redes virtuais recém-criado e sub-redes, ao clicar em **+ Adicionar nova rede virtual** e instruções a seguir.
7. Sob **redes IP**, pode adicionar intervalos de endereços IPv4, escrevendo intervalos de endereços IPv4 no [notação CIDR (Classless Inter-domain Routing)](https://tools.ietf.org/html/rfc4632) ou endereços IP individuais.
8. Clique em **Guardar**.

## <a name="azure-cli-20"></a>CLI 2.0 do Azure

1. [Instalar a CLI do Azure 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) e [início de sessão](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Regras de rede virtual disponível de lista, se não tiver definido a quaisquer regras para este Cofre de chaves, a lista estará vazia.
```azurecli
az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
```

3. Ativar o ponto final de serviço para o Cofre de chaves numa rede Virtual existente e a sub-rede
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
```

4. Adicionar uma regra de rede para uma rede virtual e sub-rede
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
```

5. Adicionar intervalo de endereços IP para permitir tráfego a partir de
```azurecli
az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
```

6. Se este Cofre de chaves tem de ser acessível para todos os serviços confiáveis, definir 'Ignorar' para AzureServices
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
```

7. Agora o passo final e importante, ative Diante de regras de rede ao definir a ação predefinida para "Deny"
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
```

## <a name="azure-powershell"></a>Azure PowerShell

1. Instalar a versão mais recente [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) e [início de sessão](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Regras de rede virtual disponível de lista, se não tiver definido a quaisquer regras para este Cofre de chaves, a lista estará vazia.
```PowerShell
(Get-AzureRmKeyVault -VaultName "mykeyvault").NetworkAcls
```

3. Ativar o ponto final de serviço para o Cofre de chaves numa rede Virtual existente e a sub-rede
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzureRmVirtualNetwork
```

4. Adicionar uma regra de rede para uma rede virtual e sub-rede
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
```

5. Adicionar intervalo de endereços IP para permitir tráfego a partir de
```PowerShell
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
```

6. Se este Cofre de chaves tem de ser acessível para todos os serviços confiáveis, definir 'Ignorar' para AzureServices
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
```

7. Agora o passo final e importante, ative Diante de regras de rede ao definir a ação predefinida para "Deny"
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
```

## <a name="references"></a>Referências

* Os comandos da CLI 2.0 do Azure - [az keyvault-regra de rede](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Cmdlets do Azure PowerShell - [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [Add-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule), [ Atualização-AzureRmKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Passos Seguintes

* [Pontos finais de serviço de rede virtual para o Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Proteger o seu cofre de chaves](key-vault-secure-your-key-vault.md)