---
title: Criar um gateway de aplicação de dimensionamento automático e redundância de zona, com um endereço IP reservado - Azure PowerShell
description: Saiba como criar um gateway de aplicação de dimensionamento automático e redundância de zona, com um endereço IP reservado através do Azure PowerShell.
services: application-gateway
author: amitsriva
ms.service: application-gateway
ms.topic: tutorial
ms.date: 9/26/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 6c54706f45653c43e6b41d0adb3132583079e6b6
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47167535"
---
# <a name="tutorial-create-an-autoscaling-zone-redundant-application-gateway-with-a-reserved-virtual-ip-address-using-azure-powershell"></a>Tutorial: criar um gateway de aplicação de dimensionamento automático e redundância de zona, com um endereço IP virtual reservado através do Azure PowerShell

Este tutorial descreve como criar um Gateway de Aplicação Azure com cmdlets do Azure PowerShell e o modelo de implementação Azure Resource Manager. Este tutorial aborda as diferenças do novo SKU de dimensionamento automático em comparação com o SKU padrão existente. Especificamente, as funcionalidades para suportar o dimensionamento automático, a redundância de zona e as VIPs reservadas (IP estático).

Para obter mais informações sobre o dimensionamento automático e a redundância de zona do gateway de aplicação, veja [Gateway de Aplicação de Dimensionamento Automático e Redundância de Zona (Pré-visualização Pública)](application-gateway-autoscaling-zone-redundant.md).

> [!IMPORTANT]
> O SKU do gateway de aplicação de dimensionamento automático e redundância de zona está atualmente em pré-visualização pública. Esta pré-visualização é disponibilizada sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Algumas funcionalidades poderão não ser suportadas ou poderão ter capacidades limitadas. Veja os [Termos Suplementares de Utilização para Pré-visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) para obter mais informações.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Configurar o parâmetro de configuração de dimensionamento automático
> * Utilizar o parâmetro de zona
> * Utilizar um VIP estático
> * Criar o gateway de aplicação


Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Este tutorial exige que execute o Azure PowerShell localmente. Tem de ter a versão 6.9.0 do módulo Azure PowerShell ou posterior instalada. Executar `Get-Module -ListAvailable AzureRM` para localizar a versão. Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Depois de verificar a versão do PowerShell, execute `Login-AzureRmAccount` para criar uma ligação ao Azure.

## <a name="sign-in-to-your-azure-account"></a>Inicie sessão na sua conta do Azure

```azurepowershell
Connect-AzureRmAccount
Select-AzureRmSubscription -Subscription "<sub name>"
```
## <a name="create-a-resource-group"></a>Criar um grupo de recursos
Crie um grupo de recursos numa das localizações disponíveis.

```azurepowershell
$location = "East US 2"
$rg = "<rg name>"

#Create a new Resource Group
New-AzureRmResourceGroup -Name $rg -Location $location
```

## <a name="create-a-vnet"></a>Criar uma VNet
Crie uma VNet com uma sub-rede dedicada para um gateway de aplicação de dimensionamento automático. Atualmente, apenas um gateway de aplicação de dimensionamento automático pode ser implementado em cada sub-rede dedicada.

```azurepowershell
#Create VNet with two subnets
$sub1 = New-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -AddressPrefix "10.0.0.0/24"
$sub2 = New-AzureRmVirtualNetworkSubnetConfig -Name "BackendSubnet" -AddressPrefix "10.0.1.0/24"
$vnet = New-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg `
       -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $sub1, $sub2
```

## <a name="create-a-reserved-public-ip"></a>Criar um IP público reservado

Especificar o método de alocação de PublicIPAddress como **estático**. Um VIP de gateway de aplicação de dimensionamento automático só pode ser estático. Os IPs dinâmicos não são suportados. Apenas o SKU PublicIpAddress standard é suportado.

```azurepowershell
#Create static public IP
$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP" `
       -location $location -AllocationMethod Static -Sku Standard
```

## <a name="retrieve-details"></a>Obter os detalhes

Obtenha os detalhes do grupo de recursos, sub-rede e IP num objeto local para criar os detalhes de configuração do IP de gateway da aplicação.

```azurepowershell
$resourceGroup = Get-AzureRmResourceGroup -Name $rg
$publicip = Get-AzureRmPublicIpAddress -ResourceGroupName $rg -name "AppGwVIP"
$vnet = Get-AzureRmvirtualNetwork -Name "AutoscaleVNet" -ResourceGroupName $rg
$gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "AppGwSubnet" -VirtualNetwork $vnet
```
## <a name="configure-application-gateway-infrastructure"></a>Configurar a infraestrutura do gateway da aplicação
Configure a configuração do IP, a configuração do IP de front-end, o conjunto de back-end, as definições de http, o certificado, a porta, o serviço de escuta e a regra num formato idêntico ao do Gateway de Aplicação Standard existente. O novo SKU segue o mesmo modelo de objeto do SKU Standard.

```azurepowershell
$ipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name "IPConfig" -Subnet $gwSubnet
$fip = New-AzureRmApplicationGatewayFrontendIPConfig -Name "FrontendIPCOnfig" -PublicIPAddress $publicip
$pool = New-AzureRmApplicationGatewayBackendAddressPool -Name "Pool1" `
       -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "SSLPort" -Port 443
$fp02 = New-AzureRmApplicationGatewayFrontendPort -Name "HTTPPort" -Port 80

$securepfxpwd = ConvertTo-SecureString -String "scrap" -AsPlainText -Force
$sslCert01 = New-AzureRmApplicationGatewaySslCertificate -Name "SSLCert" -Password $securepfxpwd `
            -CertificateFile "D:\Networking\ApplicationGateway\scrap.pfx"
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "SSLListener" `
             -Protocol Https -FrontendIPConfiguration $fip -FrontendPort $fp01 -SslCertificate $sslCert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "HTTPListener" `
             -Protocol Http -FrontendIPConfiguration $fip -FrontendPort $fp02

$setting = New-AzureRmApplicationGatewayBackendHttpSettings -Name "BackendHttpSetting1" `
          -Port 80 -Protocol Http -CookieBasedAffinity Disabled
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule1" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener01 -BackendAddressPool $pool
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "Rule2" -RuleType basic `
         -BackendHttpSettings $setting -HttpListener $listener02 -BackendAddressPool $pool
```

## <a name="specify-autoscale"></a>Especificar o dimensionamento automático

Agora pode especificar a configuração do dimensionamento automático para o gateway de aplicação. São suportados dois tipos de configuração de dimensionamento automático:

- **Modo de capacidade fixo**. Neste modo, o gateway de aplicação não dimensiona automaticamente e opera numa capacidade de Unidade de Escala fixa.

   ```azurepowershell
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```
- **Modo de dimensionamento automático**. Neste modo, o gateway de aplicação é dimensionado automaticamente com base no padrão de tráfego da aplicação.

   ```azurepowershell
   $autoscaleConfig = New-AzureRmApplicationGatewayAutoscaleConfiguration -MinCapacity 2
   $sku = New-AzureRmApplicationGatewaySku -Name Standard_v2 -Tier Standard_v2
   ```

## <a name="create-the-application-gateway"></a>Criar o gateway de aplicação

Crie o Gateway de Aplicação e inclua as zonas de redundância. 

A configuração de zonas é suportada apenas nas regiões onde as Zonas do Azure estão disponíveis. Em regiões onde as Zonas do Azure não estão disponíveis, não deve ser utilizado o parâmetro de zona. Um gateway de aplicação também pode ser implementado numa zona única, em duas zonas ou em todas as três zonas. O PublicIPAddress para um gateway de aplicação de zona única deve estar vinculado à mesma zona. Para um gateway de aplicação com redundância de duas ou três zonas, o PublicIPAddress tem de ter também redundância de zona, pelo que nenhuma zona é especificada.

```azurepowershell
$appgw = New-AzureRmApplicationGateway -Name "AutoscalingAppGw" -Zone 1,2,3 `
  -ResourceGroupName $rg -Location $location -BackendAddressPools $pool `
  -BackendHttpSettingsCollection $setting -GatewayIpConfigurations $ipconfig `
  -FrontendIpConfigurations $fip -FrontendPorts $fp01, $fp02 `
  -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 `
  -Sku $sku -sslCertificates $sslCert01 -AutoscaleConfiguration $autoscaleConfig
```

## <a name="test-the-application-gateway"></a>Testar o gateway de aplicação

Utilize [Get-AzureRmPublicIPAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) para obter o endereço IP público do gateway de aplicação. Copie o endereço IP público ou o nome DNS e cole-o na barra de endereço do browser.

`Get-AzureRmPublicIPAddress -ResourceGroupName $rg -Name AppGwVIP`

## <a name="clean-up-resources"></a>Limpar recursos
Primeiro, explore os recursos que foram criados com o gateway de aplicação e, em seguida, quando já não for necessário, pode utilizar o comando `Remove-AzureRmResourceGroup` para remover o grupo de recursos, o gateway de aplicação e todos os recursos relacionados.

`Remove-AzureRmResourceGroup -Name $rg`

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Utilizar um VIP estático
> * Configurar o parâmetro de configuração de dimensionamento automático
> * Utilizar o parâmetro de zona
> * Criar o gateway de aplicação

> [!div class="nextstepaction"]
> [Criar um gateway de aplicação com regras de encaminhamento com base no caminho de URL](./tutorial-url-route-powershell.md)
