---
title: Criar um gateway de aplicação que aloja vários sites - Azure PowerShell
description: Saiba como criar um gateway de aplicação que aloja vários sites através do Azure PowerShell.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/13/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 31034d6f2b5f9e73623ad7e81c54bfb75e71851b
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2019
ms.locfileid: "54421358"
---
# <a name="create-an-application-gateway-that-hosts-multiple-web-sites-using-azure-powershell"></a>Criar um gateway de aplicação que aloja vários sites através do Azure PowerShell

Pode utilizar o Azure PowerShell para [configurar o alojamento de vários sites](multiple-site-overview.md) quando cria um [gateway de aplicação](overview.md). Neste tutorial, vai definir conjuntos de endereços de back-end através de conjuntos de dimensionamento de máquinas virtuais. Em seguida, vai configurar os serviços de escuta e as regras com base nos domínios que possui para assegurar que o tráfego Web chega aos servidores adequados nos conjuntos. Este tutorial parte do princípio de que possui vários domínios e utiliza exemplos de *www.contoso.com* e *www.fabrikam.com*.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar a rede
> * Criar um gateway de aplicação
> * Criar serviços de escuta de back-end
> * Criar regras de encaminhamento
> * Criar conjuntos de dimensionamento de máquinas virtuais com conjuntos de back-end
> * Criar um registo CNAME no seu domínio

![Exemplo de encaminhamento multilocal](./media/tutorial-multiple-sites-powershell/scenario.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o módulo do Azure PowerShell versão 3.6 ou posterior. Para localizar a versão, execute ` Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/azurerm/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Crie um grupo de recursos do Azure com [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).  

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Criar recursos de rede

Crie as configurações de sub-rede com [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Crie a rede virtual com [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) com as configurações de sub-rede. Por fim, crie o endereço IP público com [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Estes recursos são utilizados para fornecer conectividade de rede ao gateway de aplicação e aos respetivos recursos associados.

```azurepowershell-interactive
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24

$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24

$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig

$pip = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

### <a name="create-the-ip-configurations-and-frontend-port"></a>Criar as configurações de IP e a porta de front-end

Associe a sub-rede que criou anteriormente ao gateway de aplicação com [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration). Atribua o endereço IP público ao gateway de aplicação com [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig).

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$subnet=$vnet.Subnets[0]

$gipconfig = New-AzureRmApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet

$fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip

$frontendport = New-AzureRmApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pools-and-settings"></a>Criar os conjuntos de back-end e as definições

Crie o primeiro conjunto de endereços de back-end para o gateway de aplicação com [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool). Configure as definições do conjunto com [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$contosoPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name contosoPool

$fabrikamPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name fabrikamPool

$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listeners-and-rules"></a>Criar os serviços de escuta e as regras

São necessários serviços de escuta para permitir ao gateway de aplicação encaminhar o tráfego adequadamente para os conjuntos de endereços de back-end. Neste tutorial, vai criar dois serviços de escuta para os seus dois domínios. Neste exemplo, os serviços de escuta são criados para os domínios de *www.contoso.com* e *www.fabrikam.com*.

Crie o primeiro serviço de escuta com [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) com a configuração e a porta de front-end que criou anteriormente. É necessária uma regra para o serviço de escuta saber qual o conjunto de back-end a utilizar para o tráfego de entrada. Crie uma regra básica denominada *contosoRule* com [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$contosolistener = New-AzureRmApplicationGatewayHttpListener `
  -Name contosoListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport `
  -HostName "www.contoso.com"

$fabrikamlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name fabrikamListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport `
  -HostName "www.fabrikam.com"

$contosoRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name contosoRule `
  -RuleType Basic `
  -HttpListener $contosoListener `
  -BackendAddressPool $contosoPool `
  -BackendHttpSettings $poolSettings

$fabrikamRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name fabrikamRule `
  -RuleType Basic `
  -HttpListener $fabrikamListener `
  -BackendAddressPool $fabrikamPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Criar o gateway de aplicação

Agora que criou os recursos de apoio necessários, especifique os parâmetros do gateway de aplicação com [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) e, em seguida, crie-o com [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway).

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2

$appgw = New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $contosoPool, $fabrikamPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $contosoListener, $fabrikamListener `
  -RequestRoutingRules $contosoRule, $fabrikamRule `
  -Sku $sku
```

## <a name="create-virtual-machine-scale-sets"></a>Criar conjuntos de dimensionamento de máquinas virtuais

Neste exemplo, vai criar dois conjuntos de dimensionamento de máquinas virtuais que suportam os dois conjuntos de back-end que criou. Os conjuntos de dimensionamento que criar são denominados *myvmss1* e *myvmss2*. Cada conjunto de dimensionamento contém duas instâncias de máquina virtual onde vai instalar o IIS. Pode atribuir o conjunto de dimensionamento ao conjunto de back-end quando configurar as definições de IP.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet

$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway

$contosoPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name contosoPool `
  -ApplicationGateway $appgw

$fabrikamPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name fabrikamPool `
  -ApplicationGateway $appgw

for ($i=1; $i -le 2; $i++)
{
  if ($i -eq 1) 
  {
    $poolId = $contosoPool.Id
  }
  if ($i -eq 2)
  {
    $poolId = $fabrikamPool.Id
  }

  $ipConfig = New-AzureRmVmssIpConfig `
    -Name myVmssIPConfig$i `
    -SubnetId $vnet.Subnets[1].Id `
    -ApplicationGatewayBackendAddressPoolsId $poolId

  $vmssConfig = New-AzureRmVmssConfig `
    -Location eastus `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

  Set-AzureRmVmssStorageProfile $vmssConfig `
    -ImageReferencePublisher MicrosoftWindowsServer `
    -ImageReferenceOffer WindowsServer `
    -ImageReferenceSku 2016-Datacenter `
    -ImageReferenceVersion latest
    -OsDiskCreateOption FromImage

  Set-AzureRmVmssOsProfile $vmssConfig `
    -AdminUsername azureuser `
    -AdminPassword "Azure123456!" `
    -ComputerNamePrefix myvmss$i

  Add-AzureRmVmssNetworkInterfaceConfiguration `
    -VirtualMachineScaleSet $vmssConfig `
    -Name myVmssNetConfig$i `
    -Primary $true `
    -IPConfiguration $ipConfig

  New-AzureRmVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmssConfig
}
```

### <a name="install-iis"></a>Instalar o IIS

```azurepowershell-interactive
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/application-gateway/iis/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

for ($i=1; $i -le 2; $i++)
{
  $vmss = Get-AzureRmVmss `
    -ResourceGroupName myResourceGroupAG `
    -VMScaleSetName myvmss$i

  Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings

  Update-AzureRmVmss `
    -ResourceGroupName myResourceGroupAG `
    -Name myvmss$i `
    -VirtualMachineScaleSet $vmss
}
```

## <a name="create-cname-record-in-your-domain"></a>Criar registo CNAME no seu domínio

Depois de criar o gateway de aplicação com o respetivo endereço IP público, pode obter o endereço DNS e utilizá-lo para criar um registo CNAME no seu domínio. Pode utilizar [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para obter o endereço DNS do gateway de aplicação. Copie o valor *fqdn* de DNSSettings e utilize-o como o valor do registo CNAME que criar. Não é recomendada a utilização de registos A, uma vez que o VIP pode ser alterado no reinício do gateway de aplicação.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

## <a name="test-the-application-gateway"></a>Testar o gateway de aplicação

Introduza o nome de domínio na barra de endereço do seu browser. Como, por exemplo, http://www.contoso.com.

![Testar o site contoso no gateway de aplicação](./media/tutorial-multiple-sites-powershell/application-gateway-iistest.png)

Altere o endereço para o outro domínio, e deverá ver algo semelhante ao seguinte exemplo:

![Testar o site fabrikam no gateway de aplicação](./media/tutorial-multiple-sites-powershell/application-gateway-iistest2.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando já não forem necessários, remova o grupo de recursos, o gateway de aplicação e todos os recursos relacionados, com [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup).

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configurar a rede
> * Criar um gateway de aplicação
> * Criar serviços de escuta de back-end
> * Criar regras de encaminhamento
> * Criar conjuntos de dimensionamento de máquinas virtuais com conjuntos de back-end
> * Criar um registo CNAME no seu domínio

> [!div class="nextstepaction"]
> [Criar um gateway de aplicação com regras de encaminhamento com base no caminho de URL](./tutorial-url-route-powershell.md)