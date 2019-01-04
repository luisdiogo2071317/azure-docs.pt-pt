---
ms.assetid: ''
title: Configurar o Azure Key Vault firewalls e redes virtuais - Azure Key Vault
description: Instruções passo a passo para configurar o Cofre de chaves firewalls e redes virtuais
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: conceptual
ms.workload: identity
ms.date: 01/02/2019
ms.author: ambapat
ms.openlocfilehash: 09a19b92a496650f94be208d4f463f1fb3fa4256
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2019
ms.locfileid: "54001860"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Configurar o Azure Key Vault firewalls e redes virtuais

Este artigo fornece instruções passo a passo para configurar o Azure Key Vault firewalls e redes virtuais para restringir o acesso ao seu Cofre de chaves. O [pontos finais de serviço de rede virtual para o Key Vault](key-vault-overview-vnet-service-endpoints.md) permitem-lhe restringir o acesso a uma rede virtual especificada e um conjunto de intervalos de endereços IPv4 (protocolo IP versão 4).

> [!IMPORTANT]
> Depois de regras de firewall estão em vigor, os utilizadores apenas podem executar o Key Vault [plano de dados](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) permitidas de operações quando os pedidos têm origem nos intervalos de endereços IPv4 ou as redes virtuais. Isso também se aplica a aceder ao Cofre de chaves do portal do Azure. Embora os usuários podem navegar para um cofre de chaves do portal do Azure, eles poderão não conseguir listar chaves, segredos ou certificados se os respetivos computadores cliente não está na lista de permitidos. Isso também afeta o Seletor de Cofre de chave por outros serviços do Azure. Os utilizadores poderão ver a lista de cofres de chaves, mas não listar as chaves, se as regras de firewall impedem os respetivos computadores cliente.

## <a name="use-the-azure-portal"></a>Utilizar o portal do Azure

Eis como configurar o Cofre de chaves firewalls e redes virtuais com o portal do Azure:

1. Navegue para o Cofre de chaves que pretende proteger.
2. Selecione **Firewalls e redes virtuais**.
3. Sob **autorizar o acesso**, selecione **redes selecionadas**.
4. Para adicionar redes virtuais existentes para firewalls e regras de rede virtual, selecione **+ adicionar redes virtuais existentes**.
5. No novo painel apresentado, selecione a subscrição, redes virtuais e sub-redes que pretende permitir o acesso a este Cofre de chaves. Se a redes virtuais e sub-redes que selecionou não tiverem pontos finais de serviço ativados, confirme que pretende ativar pontos finais de serviço e selecione **ativar**. Poderá demorar até 15 minutos para entrar em vigor.
6. Sob **redes IP**, adicionar intervalos de endereços IPv4, escrevendo intervalos de endereços IPv4 no [notação CIDR (Classless Inter-domain Routing)](https://tools.ietf.org/html/rfc4632) ou endereços IP individuais.
7. Selecione **Guardar**.

Pode também adicionar novas redes virtuais e sub-redes e, em seguida, ativar pontos finais de serviço para o acabada de criar redes virtuais e sub-redes, selecionando **+ Adicionar nova rede virtual**. Em seguida, siga as instruções.

## <a name="use-the-azure-cli-20"></a>Utilizar CLI 2.0 do Azure

Eis como configurar o Cofre de chaves firewalls e redes virtuais, utilizando a CLI 2.0 do Azure:

1. [Instalar a CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) e [iniciar sessão](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Listar regras de rede virtual disponível. Se ainda não definir quaisquer regras para este Cofre de chaves, a lista estará vazia.
   ```azurecli
   az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
   ```

3. Ative um ponto de extremidade de serviço para o Cofre de chaves numa rede virtual existente e a sub-rede.
   ```azurecli
   az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
   ```

4. Adicione uma regra de rede para uma rede virtual e uma sub-rede.
   ```azurecli
   subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
   az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
   ```

5. Adicione um intervalo de endereços IP do qual permitir o tráfego.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Se este Cofre de chaves deve ser acessível a todos os serviços confiáveis, defina `bypass` para `AzureServices`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. Ativar as regras de rede, definindo a ação predefinida `Deny`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

## <a name="use-azure-powershell"></a>Utilizar o Azure PowerShell

Eis como configurar o Cofre de chaves firewalls e redes virtuais com o PowerShell:

1. Instalar a versão mais recente [do Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps), e [iniciar sessão](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Listar regras de rede virtual disponível. Se não tiver definido a quaisquer regras para este Cofre de chaves, a lista estará vazia.
   ```PowerShell
   (Get-AzureRmKeyVault -VaultName "mykeyvault").NetworkAcls
   ```

3. Ative o ponto final de serviço para o Key Vault numa rede virtual existente e a sub-rede.
   ```PowerShell
   Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzureRmVirtualNetwork
   ```

4. Adicione uma regra de rede para uma rede virtual e uma sub-rede.
   ```PowerShell
   $subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
   Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
   ```

5. Adicione um intervalo de endereços IP do qual permitir o tráfego.
   ```PowerShell
   Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Se este Cofre de chaves deve ser acessível a todos os serviços confiáveis, defina `bypass` para `AzureServices`.
   ```PowerShell
   Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. Ativar as regras de rede, definindo a ação predefinida `Deny`.
   ```PowerShell
   Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```

## <a name="references"></a>Referências

* Os comandos da CLI 2.0 do Azure: [az keyvault-regra de rede](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Cmdlets do Azure PowerShell: [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [AzureRmKeyVaultNetworkRule adicione](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule), [AzureRmKeyVaultNetworkRuleSet de atualização](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Passos Seguintes

* [Pontos finais de serviço de rede virtual para o Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Proteger o seu cofre de chaves](key-vault-secure-your-key-vault.md)