---
title: Criar um gateway de aplicação com um conjunto de dimensionamento de máquina virtual - Azure PowerShell | Microsoft Docs
description: Saiba como criar um gateway de aplicação com um conjunto com o Azure PowerShell de dimensionamento de máquina virtual.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: victorh
ms.openlocfilehash: ee3ee9d00d1136131f3ddc5776011e8ac4fa4a28
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
---
# <a name="create-an-application-gateway-and-virtual-machine-scale-set-using-azure-powershell"></a>Criar uma aplicação gateway e máquina virtual de conjunto de dimensionamento com o Azure PowerShell

Pode utilizar o Azure PowerShell para criar um [gateway de aplicação](application-gateway-introduction.md) que utiliza um [conjunto de dimensionamento da máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) para servidores back-end. Neste exemplo, o conjunto de dimensionamento contém duas instâncias de máquina virtual que são adicionadas ao conjunto predefinido de back-end do gateway de aplicação.

Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação
> * Criar um conjunto com o conjunto de back-end predefinido de dimensionamento de máquina virtual

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o Azure PowerShell versão do módulo 3,6 ou posterior. Para localizar a versão, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Connect-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Criar um grupo de recursos do Azure utilizando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).  

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Criar recursos de rede 

Configure as sub-redes denominadas *myBackendSubnet* e *myAGSubnet* utilizando [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Criar a rede virtual *myVNet* utilizando [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) com as configurações de sub-rede. E por fim, crie o endereço IP público com o nome *myAGPublicIPAddress* utilizando [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Estes recursos são utilizados para fornecer conectividade de rede para o gateway de aplicação e os respetivos recursos associados.

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

### <a name="create-the-ip-configurations-and-frontend-port"></a>Criar as configurações de IP e porta de front-end

Associar *myAGSubnet* que criou anteriormente para o gateway de aplicação com [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration). Atribuir *myAGPublicIPAddress* para o gateway de aplicação com [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig).

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

### <a name="create-the-backend-pool-and-settings"></a>Criar o conjunto back-end e as definições

Criar o conjunto de back-end com o nome *appGatewayBackendPool* para o gateway de aplicação com [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool). Configurar as definições para os conjuntos de endereços de back-end utilizando [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$defaultPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool 
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-default-listener-and-rule"></a>Criar o serviço de escuta de predefinição e a regra

Um serviço de escuta é obrigatório para ativar o gateway de aplicação encaminhar o tráfego adequadamente para o conjunto de back-end. Neste exemplo, pode criar um serviço de escuta básico que escuta o tráfego do URL de raiz. 

Criar um serviço de escuta com o nome *mydefaultListener* utilizando [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) com a configuração de front-end e porta de front-end que criou anteriormente. Uma regra é necessária para o serviço de escuta saber que conjunto de back-end a utilizar para tráfego de entrada. Criar uma regra básica com o nome *rule1* utilizando [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$defaultlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name mydefaultListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $defaultPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Criar o gateway de aplicação

Agora que criou os recursos de suporte necessários, especifique os parâmetros para o gateway de aplicação com [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku)e, em seguida, crie-o utilizando [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway).

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
$appgw = New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $defaultPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

## <a name="create-a-virtual-machine-scale-set"></a>Criar um conjunto de dimensionamento de máquinas virtuais

Neste exemplo, pode criar um conjunto para fornecer os servidores para o conjunto de back-end no gateway de aplicação de dimensionamento de máquina virtual. Atribuir a escala definida para o conjunto de back-end, quando configurar as definições de IP.

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name appGatewayBackendPool `
  -ApplicationGateway $appgw
$ipConfig = New-AzureRmVmssIpConfig `
  -Name myVmssIPConfig `
  -SubnetId $vnet.Subnets[1].Id `
  -ApplicationGatewayBackendAddressPoolsId $backendPool.Id
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
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword "Azure123456!" `
  -ComputerNamePrefix myvmss
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name myVmssNetConfig `
  -Primary $true `
  -IPConfiguration $ipConfig
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmssConfig
```

### <a name="install-iis"></a>Instalar o IIS

```azurepowershell-interactive
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/vhorne/samplescripts/master/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }
$vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.8 `
  -Setting $publicSettings
Update-AzureRmVmss `
  -ResourceGroupName myResourceGroupAG `
  -Name myvmss `
  -VirtualMachineScaleSet $vmss
```

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

Pode utilizar [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para obter o endereço IP público do gateway de aplicação. Copie o endereço IP público e cole-o na barra de endereço do browser.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![URL de base de teste no gateway de aplicação](./media/tutorial-create-vmss-powershell/tutorial-iistest.png)

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação
> * Criar um conjunto com o conjunto de back-end predefinido de dimensionamento de máquina virtual

Para obter mais informações sobre gateways de aplicação e os recursos associados, avance para os artigos de procedimentos.
