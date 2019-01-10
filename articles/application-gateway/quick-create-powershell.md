---
title: Início Rápido - Direcionar tráfego da Web com o Gateway de Aplicação do Azure - Azure PowerShell | Microsoft Docs
description: Saiba como utilizar o Azure PowerShell para criar um Gateway de Aplicação do Azure que direciona o tráfego da Web para máquinas virtuais num conjunto de back-end.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/8/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: cb5a6a21cd6d33316e0560d7641bee99b2102373
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159830"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-powershell"></a>Início rápido: Tráfego da web direto com o Gateway de aplicação do Azure - Azure PowerShell

Este guia de introdução mostra-lhe como utilizar o portal do Azure para criar rapidamente um gateway de aplicação com duas máquinas virtuais no seu conjunto de back-end. Em seguida, vai testá-lo para se certificar de que está a funcionar corretamente. Com o Gateway de aplicação do Azure, direcionar o tráfego de web de aplicação a recursos específicos por: serviços de escuta a atribuir às portas, criação de regras e adicionar recursos a um agrupamento de back-end.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="run-azure-powershell-locally"></a>Executar localmente o Azure PowerShell

Se optar por instalar e utilizar o Azure PowerShell localmente, este tutorial requer a versão do módulo 3.6 ou posterior do Azure PowerShell. 

1. Para localizar a versão, execute `Get-Module -ListAvailable AzureRM`. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). 
2. Para criar uma ligação com o Azure, execute `Login-AzureRmAccount`.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

No Azure, alocar recursos relacionados a um grupo de recursos. Criar um grupo de recursos com o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet da seguinte forma: 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Criar recursos de rede

Crie uma rede virtual para que o gateway de aplicação pode comunicar com outros recursos. Duas sub-redes são criadas neste exemplo: um para o gateway de aplicação e outro para os servidores de back-end. A sub-rede do gateway de aplicação pode conter apenas os gateways de aplicação. Não existem outros recursos são permitidos.

1. Criar as configurações de sub-rede chamando [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig).
2. Criar a rede virtual com as configurações de sub-rede chamando [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork).
3. Criar o endereço IP público ao chamar [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress).

```azurepowershell-interactive
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```
## <a name="create-backend-servers"></a>Criar servidores de back-end

Neste exemplo, vai criar duas máquinas virtuais do Azure utilizar como servidores de back-end para o gateway de aplicação. Também é instalar o IIS nas máquinas virtuais para verificar se o Azure criada com êxito o gateway de aplicação.

### <a name="create-two-virtual-machines"></a>Criar duas máquinas virtuais

1. Crie uma interface de rede com [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). 
2. Crie uma configuração de máquina virtual com [New-AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig).
3. Criar a máquina virtual com [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).

Ao executar o exemplo de código seguinte para criar as máquinas virtuais, o Azure pede-lhe as credenciais. Introduza *azureuser* para o nome de utilizador e *Azure123456!* a palavra-passe:
    
```azurepowershell-interactive
$vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork myVNet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzureRmNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -SubnetId $subnet.Id
  $vm = New-AzureRmVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzureRmVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzureRmVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzureRmVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzureRmVMBootDiagnostics `
    -VM $vm `
    -Disable
  New-AzureRmVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzureRmVMExtension `
    -ResourceGroupName myResourceGroupAG `
    -ExtensionName IIS `
    -VMName myVM$i `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
}
```

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

### <a name="create-the-ip-configurations-and-frontend-port"></a>Criar as configurações de IP e a porta de front-end

1. Uso [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration) para criar a configuração que associa a sub-rede que criou com o gateway de aplicação. 
2. Uso [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig) para criar a configuração que atribui o endereço IP público que criou anteriormente para o gateway de aplicação. 
3. Uso [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport) para atribuir a porta 80 para acessar o gateway de aplicação.

```azurepowershell-interactive
$vnet   = Get-AzureRmVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork myVNet -Name myAGSubnet
$pip    = Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
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

### <a name="create-the-backend-pool"></a>Criar o conjunto de back-end

1. Utilize [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool) para criar o conjunto de back-end para o gateway de aplicação. 
2. Configurar as definições para o conjunto de back-end com [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

```azurepowershell-interactive
$address1 = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic1
$address2 = Get-AzureRmNetworkInterface -ResourceGroupName myResourceGroupAG -Name myNic2
$backendPool = New-AzureRmApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool `
  -BackendIPAddresses $address1.ipconfigurations[0].privateipaddress, $address2.ipconfigurations[0].privateipaddress
$poolSettings = New-AzureRmApplicationGatewayBackendHttpSettings `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 120
```

### <a name="create-the-listener-and-add-a-rule"></a>Criar o serviço de escuta e adicionar uma regra

O Azure requer um serviço de escuta ativar o gateway de aplicação para encaminhar o tráfego adequadamente para o conjunto de back-end. O Azure também requer uma regra para o serviço de escuta de saber qual conjunto de back-end a utilizar para tráfego de entrada. 

1. Criar um serviço de escuta com o [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) com a configuração de front-end e porta de front-end que criou anteriormente. 
2. Utilize [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule) para criar uma regra denominada *rule1*. 

```azurepowershell-interactive
$defaultlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzureRmApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Criar o gateway de aplicação

Agora que criou os recursos de suporte necessários, crie o gateway de aplicação:

1. Uso [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku) para especificar parâmetros para o gateway de aplicação.
2. Uso [New-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway) para criar o gateway de aplicação.

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzureRmApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $backendPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

## <a name="test-the-application-gateway"></a>Testar o gateway de aplicação

Embora o IIS não é necessário para criar o gateway de aplicação, a instalou neste início rápido para verificar se o Azure criada com êxito o gateway de aplicação. Utilize o IIS para testar o gateway de aplicação:

1. Execute [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para obter o endereço IP público do gateway de aplicação. 
2. Copie e cole o endereço IP público na barra de endereço do seu browser. Quando atualizar o navegador, deverá ver o nome da máquina virtual.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Testar o gateway de aplicação](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Limpar recursos

Quando já não precisar dos recursos que criou com o gateway de aplicação, remova o grupo de recursos. Ao remover o grupo de recursos, também remover o gateway de aplicação e todos os respetivos recursos relacionados. 

Para remover o grupo de recursos, chamar o [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) cmdlet da seguinte forma:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Gerir o tráfego da Web com um gateway de aplicação com o Azure PowerShell](./tutorial-manage-web-traffic-powershell.md)

