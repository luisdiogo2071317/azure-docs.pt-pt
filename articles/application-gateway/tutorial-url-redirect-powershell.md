---
title: "Criar um gateway de aplicação com o redirecionamento de URL com base no caminho - Azure PowerShell | Microsoft Docs"
description: "Saiba como criar um gateway de aplicação com o URL com base no caminho redirecionado tráfego com o Azure PowerShell."
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/24/2018
ms.author: davidmu
ms.openlocfilehash: 64b077a387bce0dd5c1f34aaca4dfcdda5b65824
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-application-gateway-with-url-path-based-redirection-using-azure-powershell"></a>Criar um gateway de aplicação com o redirecionamento com base no caminho de URL com o Azure PowerShell

Pode utilizar o Azure PowerShell para configurar [regras de encaminhamento com base no URL](application-gateway-url-route-overview.md) quando cria um [gateway de aplicação](application-gateway-introduction.md). Neste tutorial, vai criar conjuntos de back-end utilizando [conjuntos de dimensionamento de máquina virtual](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md). Em seguida, crie as regras de encaminhamento de URL que certifique-se de que o tráfego web é redirecionado para o conjunto de back-end adequado.

Neste artigo, saiba como:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação
> * Adicionar serviços de escuta e regras de encaminhamento
> * Criar conjuntos de dimensionamento de máquina virtual para os conjuntos de back-end

O exemplo seguinte mostra o tráfego do site provenientes das portas 8080 e 8081 e que está a ser direcionado para os mesmo conjuntos de back-end:

![Exemplo de encaminhamento de URL](./media/tutorial-url-redirect-powershell/scenario.png)

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Se optar por instalar e utilizar o PowerShell localmente, este tutorial requer o módulo do Azure PowerShell versão 3.6 ou posterior. Para localizar a versão, execute ` Get-Module -ListAvailable AzureRM` . Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps). Se estiver a executar localmente o PowerShell, também terá de executar o `Login-AzureRmAccount` para criar uma ligação com o Azure.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos é um contentor lógico no qual os recursos do Azure são implementados e geridos. Criar um grupo de recursos do Azure utilizando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).  

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroupAG -Location eastus
```

## <a name="create-network-resources"></a>Criar recursos de rede

Criar as configurações de sub-rede para *myBackendSubnet* e *myAGSubnet* utilizando [New-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). Criar a rede virtual denominada *myVNet* utilizando [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork) com as configurações de sub-rede. E por fim, crie o endereço IP público com o nome *myAGPublicIPAddress* utilizando [New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress). Estes recursos são utilizados para fornecer conectividade de rede para o gateway de aplicação e os respetivos recursos associados.

```azurepowershell-interactive
$backendSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.1.0/24
$agSubnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $backendSubnetConfig, $agSubnetConfig
New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Dynamic
```

## <a name="create-an-application-gateway"></a>Criar um gateway de aplicação

### <a name="create-the-ip-configurations-and-frontend-port"></a>Criar as configurações de IP e porta de front-end

Associar *myAGSubnet* que criou anteriormente para o gateway de aplicação com [New-AzureRmApplicationGatewayIPConfiguration](/powershell/module/azurerm.network/new-azurermapplicationgatewayipconfiguration). Atribuir *myAGPublicIPAddress* para o gateway de aplicação com [New-AzureRmApplicationGatewayFrontendIPConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendipconfig). E, em seguida, pode criar o através da porta HTTP [New-AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/new-azurermapplicationgatewayfrontendport).

```azurepowershell-interactive
$vnet = Get-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Name myVNet
$subnet=$vnet.Subnets[0]
$pip = Get-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Name myAGPublicIPAddress
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

### <a name="create-the-default-pool-and-settings"></a>Criar o conjunto predefinido e as definições

Criar o conjunto predefinido de back-end com o nome *appGatewayBackendPool* para o gateway de aplicação com [New-AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendaddresspool). Configurar as definições para o conjunto de back-end utilizando [New-AzureRmApplicationGatewayBackendHttpSettings](/powershell/module/azurerm.network/new-azurermapplicationgatewaybackendhttpsettings).

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

Um serviço de escuta é obrigatório para ativar o gateway de aplicação encaminhar o tráfego adequadamente a um conjunto de back-end. Neste tutorial, vai criar vários serviços de escuta. O serviço de escuta básico primeiro espera o tráfego do URL de raiz. Outros serviços de escuta esperam tráfego em URLs específicos, tais como *http://52.168.55.24:8080/imagens/* ou *http://52.168.55.24:8081/vídeo/*.

Criar um serviço de escuta com o nome *defaultListener* utilizando [New-AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/new-azurermapplicationgatewayhttplistener) com a configuração de front-end e porta de front-end que criou anteriormente. Uma regra é necessária para o serviço de escuta saber que conjunto de back-end a utilizar para tráfego de entrada. Criar uma regra básica com o nome *rule1* utilizando [New-AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/new-azurermapplicationgatewayrequestroutingrule).

```azurepowershell-interactive
$defaultlistener = New-AzureRmApplicationGatewayHttpListener `
  -Name defaultListener `
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

Agora que criou os recursos de suporte necessários, especifique os parâmetros para o gateway de aplicação com o nome *myAppGateway* utilizando [New-AzureRmApplicationGatewaySku](/powershell/module/azurerm.network/new-azurermapplicationgatewaysku)e, em seguida, crie-o utilizando [ Novo-AzureRmApplicationGateway](/powershell/module/azurerm.network/new-azurermapplicationgateway).

```azurepowershell-interactive
$sku = New-AzureRmApplicationGatewaySku `
  -Name Standard_Medium `
  -Tier Standard `
  -Capacity 2
New-AzureRmApplicationGateway `
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

### <a name="add-backend-pools-and-ports"></a>Adicionar conjuntos back-end e portas

Pode adicionar conjuntos back-end para o gateway de aplicação utilizando [adicionar AzureRmApplicationGatewayBackendAddressPool](/powershell/module/azurerm.network/add-azurermapplicationgatewaybackendaddresspool). Neste exemplo, *imagesBackendPool* e *videoBackendPool* são criados. Adicionar a porta de front-end para os conjuntos utilizando [adicionar AzureRmApplicationGatewayFrontendPort](/powershell/module/azurerm.network/add-azurermapplicationgatewayfrontendport). Em seguida, submeter as alterações para o gateway de aplicação com [Set-AzureRmApplicationGateway](/powershell/module/azurerm.network/set-azurermapplicationgateway).

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
Add-AzureRmApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name imagesBackendPool 
Add-AzureRmApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name videoBackendPool
Add-AzureRmApplicationGatewayFrontendPort `
  -ApplicationGateway $appgw `
  -Name bport `
  -Port 8080
Add-AzureRmApplicationGatewayFrontendPort `
  -ApplicationGateway $appgw `
  -Name rport `
  -Port 8081
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

## <a name="add-listeners-and-rules"></a>Adicionar serviços de escuta e regras

### <a name="add-listeners"></a>Adicionar serviços de escuta

Adicionar serviços de escuta com o nome *backendListener* e *redirectedListener* que são necessárias para encaminhar o tráfego utilizando [adicionar AzureRmApplicationGatewayHttpListener](/powershell/module/azurerm.network/add-azurermapplicationgatewayhttplistener).

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendPort = Get-AzureRmApplicationGatewayFrontendPort `
  -ApplicationGateway $appgw `
  -Name bport
$redirectPort = Get-AzureRmApplicationGatewayFrontendPort `
  -ApplicationGateway $appgw `
  -Name rport
$fipconfig = Get-AzureRmApplicationGatewayFrontendIPConfig `
  -ApplicationGateway $appgw
Add-AzureRmApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name backendListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $backendPort
Add-AzureRmApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name redirectedListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $redirectPort  
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-the-default-url-path-map"></a>Adicionar o mapeamento de caminho de URL predefinido

Mapas de caminho de URL Certifique-se de que os URLs específicos são encaminhados para conjuntos back-end específico. Pode criar os mapas de caminho de URL com o nome *imagePathRule* e *videoPathRule* utilizando [New-AzureRmApplicationGatewayPathRuleConfig](/powershell/module/azurerm.network/new-azurermapplicationgatewaypathruleconfig) e [ AzureRmApplicationGatewayUrlPathMapConfig adicionar](/powershell/module/azurerm.network/add-azurermapplicationgatewayurlpathmapconfig).

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$poolSettings = Get-AzureRmApplicationGatewayBackendHttpSettings `
  -ApplicationGateway $appgw `
  -Name myPoolSettings
$imagePool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name imagesBackendPool
$videoPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name videoBackendPool
$defaultPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name appGatewayBackendPool
$imagePathRule = New-AzureRmApplicationGatewayPathRuleConfig `
  -Name imagePathRule `
  -Paths "/images/*" `
  -BackendAddressPool $imagePool `
  -BackendHttpSettings $poolSettings
$videoPathRule = New-AzureRmApplicationGatewayPathRuleConfig `
  -Name videoPathRule `
  -Paths "/video/*" `
  -BackendAddressPool $videoPool `
  -BackendHttpSettings $poolSettings
Add-AzureRmApplicationGatewayUrlPathMapConfig `
  -ApplicationGateway $appgw `
  -Name urlpathmap `
  -PathRules $imagePathRule, $videoPathRule `
  -DefaultBackendAddressPool $defaultPool `
  -DefaultBackendHttpSettings $poolSettings
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-redirection-configuration"></a>Adicionar configuração de redirecionamento

Pode configurar o redirecionamento para o serviço de escuta com [adicionar AzureRmApplicationGatewayRedirectConfiguration](/powershell/module/azurerm.network/add-azurermapplicationgatewayredirectconfiguration).

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendListener = Get-AzureRmApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name backendListener 
$redirectConfig = Add-AzureRmApplicationGatewayRedirectConfiguration `
  -ApplicationGateway $appgw `
  -Name redirectConfig `
  -RedirectType Found `
  -TargetListener $backendListener `
  -IncludePath $true `
  -IncludeQueryString $true
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-the-redirection-url-path-map"></a>Adicionar o mapeamento de caminho do URL de redirecionamento

```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$poolSettings = Get-AzureRmApplicationGatewayBackendHttpSettings `
  -ApplicationGateway $appgw `
  -Name myPoolSettings
$defaultPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -ApplicationGateway $appgw `
  -Name appGatewayBackendPool
$redirectConfig = Get-AzureRmApplicationGatewayRedirectConfiguration `
  -ApplicationGateway $appgw `
  -Name redirectConfig
$redirectPathRule = New-AzureRmApplicationGatewayPathRuleConfig `
  -Name redirectPathRule `
  -Paths "/images/*" `
  -RedirectConfiguration $redirectConfig
Add-AzureRmApplicationGatewayUrlPathMapConfig `
  -ApplicationGateway $appgw `
  -Name redirectpathmap `
  -PathRules $redirectPathRule `
  -DefaultBackendAddressPool $defaultPool `
  -DefaultBackendHttpSettings $poolSettings
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

### <a name="add-routing-rules"></a>Adicionar regras de encaminhamento

As regras de encaminhamento associar os maps de URL com os serviços de escuta que criou. Pode adicionar as regras com o nome *defaultRule* e *redirectedRule* utilizando [adicionar AzureRmApplicationGatewayRequestRoutingRule](/powershell/module/azurerm.network/add-azurermapplicationgatewayrequestroutingrule).


```azurepowershell-interactive
$appgw = Get-AzureRmApplicationGateway `
  -ResourceGroupName myResourceGroupAG `
  -Name myAppGateway
$backendlistener = Get-AzureRmApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name backendListener
$redirectlistener = Get-AzureRmApplicationGatewayHttpListener `
  -ApplicationGateway $appgw `
  -Name redirectedListener
$urlPathMap = Get-AzureRmApplicationGatewayUrlPathMapConfig `
  -ApplicationGateway $appgw `
  -Name urlpathmap
$redirectPathMap = Get-AzureRmApplicationGatewayUrlPathMapConfig `
  -ApplicationGateway $appgw `
  -Name redirectpathmap
Add-AzureRmApplicationGatewayRequestRoutingRule `
  -ApplicationGateway $appgw `
  -Name defaultRule `
  -RuleType PathBasedRouting `
  -HttpListener $backendlistener `
  -UrlPathMap $urlPathMap
Add-AzureRmApplicationGatewayRequestRoutingRule `
  -ApplicationGateway $appgw `
  -Name redirectedRule `
  -RuleType PathBasedRouting `
  -HttpListener $redirectlistener `
  -UrlPathMap $redirectPathMap
Set-AzureRmApplicationGateway -ApplicationGateway $appgw
```

## <a name="create-virtual-machine-scale-sets"></a>Criar conjuntos de dimensionamento de máquina virtual

Neste exemplo, crie três conjuntos de dimensionamento de máquina virtual que suportam os três conjuntos de back-end que criou. Os conjuntos de dimensionamento que criar são denominados *myvmss1*, *myvmss2*, e *myvmss3*. Cada conjunto de dimensionamento contém duas instâncias de máquina virtual no qual instalar o IIS. Atribuir a escala definida para o conjunto de back-end, quando configurar as definições de IP.

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
$imagesPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name imagesBackendPool `
  -ApplicationGateway $appgw
$videoPool = Get-AzureRmApplicationGatewayBackendAddressPool `
  -Name videoBackendPool `
  -ApplicationGateway $appgw
for ($i=1; $i -le 3; $i++)
{
  if ($i -eq 1)
  {
     $poolId = $backendPool.Id
  }
  if ($i -eq 2) 
  {
    $poolId = $imagesPool.Id
  }
  if ($i -eq 3)
  {
    $poolId = $videoPool.Id
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
$publicSettings = @{ "fileUris" = (,"https://raw.githubusercontent.com/davidmu1/samplescripts/master/appgatewayurl.ps1"); 
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File appgatewayurl.ps1" }

for ($i=1; $i -le 3; $i++)
{
  $vmss = Get-AzureRmVmss -ResourceGroupName myResourceGroupAG -VMScaleSetName myvmss$i
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

## <a name="test-the-application-gateway"></a>O gateway de aplicação de teste

Pode utilizar [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para obter o endereço IP público do gateway de aplicação. Copie o endereço IP público e, em seguida, cole-o a barra de endereço do seu browser. Tal como *http://52.168.55.24*, *http://52.168.55.24:8080/images/test.htm*, *http://52.168.55.24:8080/video/test.htm*, ou *http:// 52.168.55.24:8081/Images/Test.htm*.

```azurepowershell-interactive
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![URL de base de teste no gateway de aplicação](./media/tutorial-url-redirect-powershell/application-gateway-iistest.png)

Altere o URL para http://&lt;endereço ip&gt;: 8080/video/test.htm, substituindo o IP de endereços para &lt;endereço ip&gt;, e deverá ver algo semelhante ao seguinte exemplo:

![Testar o URL de imagens no gateway de aplicação](./media/tutorial-url-redirect-powershell/application-gateway-iistest-images.png)

Altere o URL para http://&lt;endereço ip&gt;: 8080/video/test.htm, substituindo o IP de endereços para &lt;endereço ip&gt;, e deverá ver algo semelhante ao seguinte exemplo:

![URL de vídeo de teste no gateway de aplicação](./media/tutorial-url-redirect-powershell/application-gateway-iistest-video.png)

Agora, altere o URL para http://&lt;endereço ip&gt;: 8081/images/test.htm, substituindo o IP de endereços para &lt;endereço ip&gt;, e deverá ver o tráfego redirecionado novamente para o conjunto de back-end de imagens em http://&lt;endereço ip&gt;: 8080/imagens.

## <a name="next-steps"></a>Passos Seguintes

Neste artigo, aprendeu como:

> [!div class="checklist"]
> * Configure a rede
> * Criar um gateway de aplicação
> * Adicionar serviços de escuta e regras de encaminhamento
> * Criar conjuntos de dimensionamento de máquina virtual para os conjuntos de back-end

> [!div class="nextstepaction"]
> [Saiba mais sobre o que pode fazer com o gateway de aplicação](application-gateway-introduction.md)