---
title: Criar um gateway de rede virtual com redundância de zona em zonas de disponibilidade do Azure - pré-visualização | Microsoft Docs
description: Implemente o Gateway de VPN e ExpressRoute gateways em zonas de disponibilidade - pré-visualização.
services: vpn-gateway
documentationcenter: na
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/28/2018
ms.author: cherylmc
ms.openlocfilehash: c484358bf98f0121cfc3ce270b162b01c75b5b09
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096238"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones---preview"></a>Criar um gateway de rede virtual com redundância de zona em zonas de disponibilidade do Azure - Preview

Pode implementar gateways VPN e ExpressRoute no [zonas de disponibilidade do Azure](../availability-zones/az-overview.md). Isto oferece maior disponibilidade, escalabilidade e resiliência para gateways de rede virtual. Implementar gateways em zonas de disponibilidade do Azure fisicamente e logicamente separa gateways numa região, ao proteger a sua conectividade de rede no local para o Azure de falhas de nível de zona.

Os gateways de zonal e com redundância de zona possuem melhorias de desempenho fundamentais através de gateways de rede virtual regular. Além disso, a criação de um gateway de rede virtual com redundância de zona ou zonal é mais rápida do que criar outros gateways. Em vez de demorar 45 minutos, crie vezes demore aproximadamente 15 minutos, para um gateway ExpressRoute e 19 minutos para que um gateway de VPN.

### <a name="zrgw"></a>Zona redundante gateways

Para implementar automaticamente os gateways de rede virtual através de zonas de disponibilidade, pode utilizar gateways de rede virtual com redundância de zona. Com gateways com redundância de zona, pode harness o SLA de 99,99% de disponibilidade em GA acedam aos serviços fundamentais, dimensionáveis no Azure.

<br>
<br>

![gráfico de gateways redunant de zona](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Zonal gateways

Para implementar gateways numa zona específica, pode utilizar zonal gateways. Quando implementar um gateway zonal, ambas as instâncias do gateway são implementadas no mesmo horário de disponibilidade.

<br>
<br>

![gráfico de zonal gateways](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>SKUs de Gateway

Gateways com redundância de zona e zonal tem de utilizar o novo SKUs de gateway. Uma vez que [inscrever na pré-visualização](#enroll), verá o novo gateway de rede virtual SKUs em todas as regiões do Azure AZ. Estes SKUs são semelhantes de SKUs correspondentes para o ExpressRoute e Gateway de VPN, exceto que, são específicas para gateways com redundância de zona e zonal.

O novo gateway SKUs são:

### <a name="vpn-gateway"></a>Gateway de VPN

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>SKUs IP público

Zona redundante gateways e zonal gateways dependem o recurso IP público do Azure *padrão* SKU. A configuração do recurso de IP público do Azure determina se o gateway que implementar com redundância de zona, ou zonal. Se criar um recurso IP público com um *básico* SKU, o gateway não terão qualquer redundância de zona e os recursos de gateway será regionais.

### <a name="pipzrg"></a>Zona redundante gateways

Quando cria um público endereço IP com o **padrão** SKU de IP público sem especificar uma zona, o comportamento difere dependendo se o gateway é um gateway de VPN, ou como um gateway do ExpressRoute. 

* Para um gateway VPN, as instâncias de dois gateway serão implementadas em qualquer 2 fora estes três (s) zonas fornecer redundância de zona. 
* Para um gateway do ExpressRoute, uma vez que pode existir mais de duas instâncias, o gateway pode abranger em todas as três zonas.

### <a name="pipzg"></a>Zonal gateways

Quando cria um público endereço IP com o **padrão** SKU de IP público e especifique a zona (1, 2 ou 3), todas as instâncias de gateway serão implementadas no mesmo horário.

### <a name="piprg"></a>Gateways regionais

Quando cria um público endereço IP com o **básico** SKU de IP público, o gateway está implementado como um gateway regional e não tem qualquer redundância de zona, incorporada no gateway.

## <a name="before-you-begin"></a>Antes de começar

Pode utilizar o PowerShell instalado localmente no seu computador ou a Shell de nuvem do Azure. Se optar por instalar e utilizar o PowerShell localmente, esta funcionalidade requer a versão mais recente do módulo do PowerShell.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Utilizar o PowerShell localmente

Se estiver a utilizar PowerShell localmente no seu computador, em vez de através da Shell de nuvem, tem de instalar o módulo do PowerShell 6.1.1 ou superior. Para verificar a versão do PowerShell que tenha instalado, utilize o seguinte comando:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="enroll"></a>1. Inscrever na pré-visualização

Antes de poder configurar um gateway com redundância de zona ou zonal, deve primeiro inscrever a subscrição na pré-visualização. Assim que a sua subscrição tiver sido aprovisionada, será iniciado ver o novo gateway SKUs em todas as regiões do Azure AZ. 

Certifique-se de que iniciou a sessão na sua conta do Azure e estiver a utilizar a subscrição que pretende que a lista de permissões para esta pré-visualização. Utilize o seguinte exemplo para inscrever:

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName AllowVMSSVirtualNetworkGateway -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

Utilize o seguinte comando para verificar se a funcionalidade 'AllowVMSSVirtualNetworkGateway' está registada com a sua subscrição:

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
```

O resultado será semelhante para este exemplo:

![aprovisionado](./media/create-zone-redundant-vnet-gateway/verifypreview.png)

## <a name="variables"></a>2. Declarar as variáveis

Os valores utilizados para os passos de exemplo são listados abaixo. Além disso, alguns dos exemplos utilizam variáveis declaradas dentro os passos. Se estiver a utilizar estes passos no seu próprio ambiente, não se esqueça de substituir estes valores pelos seus. Quando especificar a localização, certifique-se de que a região que especificou é suportada. Para obter mais informações, consulte o [FAQ](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="configure"></a>3. Criar a rede virtual

Crie um grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Crie uma rede virtual

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzureRmVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>4. Adicionar a sub-rede do gateway

A sub-rede do gateway contém os endereços IP reservados que utilizam os serviços de gateway de rede virtual. Utilize os exemplos seguintes para adicionar e configurar uma sub-rede de gateway:

Adicione a sub-rede do gateway.

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Defina a configuração de sub-rede de gateway da rede virtual.

```azurepowershell-interactive
$getvnet | Set-AzureRmVirtualNetwork
```
## <a name="publicip"></a>5. Solicitar um endereço IP público
 
Neste passo, escolha as instruções que se aplicam ao gateway que pretende criar. A seleção das zonas para implementar os gateways depende as zonas especificadas para o endereço IP público.

### <a name="ipzoneredundant"></a>Para gateways com redundância de zona

Solicitar um endereço IP público com um **padrão** PublicIpaddress SKU e não especifique qualquer zona. Neste caso, o endereço IP público padrão criado será um IP público com redundância de zona.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>Para zonal gateways

Solicitar um endereço IP público com um **padrão** PublicIpaddress SKU. Especifique a zona (1, 2 ou 3). Todas as instâncias de gateway serão implementadas nesta zona.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Para gateways regionais

Solicitar um endereço IP público com um **básico** PublicIpaddress SKU. Neste caso, o gateway está implementado como um gateway regional e não tem qualquer redundância de zona, incorporada no gateway. As instâncias de gateway são criadas em zonas, respetivamente.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>6. Criar a configuração de IP

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>7. Criar o gateway

Crie o gateway de rede virtual.

>[!NOTE]
>Neste momento, não é possível especificar o SKU de gateway. O SKU não será automaticamente por predefinição para ErGw1AZ para o ExpressRoute e VpnGw1AZ para Gateway de VPN.
>

### <a name="for-expressroute"></a>Para o ExpressRoute

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Para o Gateway VPN

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="feedback"></a>Como os seus comentários

Agradecemos os seus comentários. Envie um e-mail para aznetworkgateways@microsoft.com reporte quaisquer problemas ou os seus comentários (positivo ou negativo) para os gateways com redundância de zona e zonal VPN e Expressroute. Inclua o nome da sua empresa no ["]" na linha de assunto. Também inclua o ID de subscrição, se estiverem a comunicar um problema.

## <a name="faq"></a>FAQ

### <a name="how-do-i-sign-up-for-the-preview"></a>Como posso inscrever-me para a pré-visualização?

Pode [inscrever](#enroll) utilizando os comandos do PowerShell neste artigo.

### <a name="what-will-change-when-i-enroll"></a>O que será alterado quando inscrever?

A perspetiva, durante a pré-visualização, pode implementar gateways de com redundância de zona. Isto significa que todas as instâncias dos gateways serão implementadas através de zonas de disponibilidade do Azure e cada zona de disponibilidade é um domínio de falhas e de atualização diferentes. Isto torna o seus gateways mais fiável, disponíveis e resilientes a falhas de zona.

### <a name="what-regions-are-available-for-the-preview"></a>Que regiões estão disponíveis para a pré-visualização?

Com redundância de zona e zonal gateways estão disponíveis em regiões público/Azure de produção.

### <a name="will-i-be-billed-for-participating-in-this-preview"></a>Será posso faturado por participar nesta pré-visualização?

Será não faturado para gateways de durante a pré-visualização. No entanto, não há nenhum SLA ligado à sua implementação. Estamos muito interessados em hearing os seus comentários.

> [!NOTE]
> Para gateways do ExpressRoute, o gateway não é cobrados/cobrados. No entanto, o circuito propriamente dito (não o gateway) será cobrado.

### <a name="what-regions-are-available-for-me-to-try-this-in"></a>Que regiões estão disponíveis para me permitir experimente este no?

A pré-visualização pública está disponível em regiões EUA Central e França Central (regiões do Azure que têm as zonas de disponibilidade geralmente disponível). Doravante, iremos irá disponibilizar os Gateways com redundância de zona para si noutras regiões público do Azure.

### <a name="can-i-change-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Pode alterar os meus gateways de rede virtual existente para gateways com redundância de zona ou zonal?

Migrar os seus gateways de rede virtual existente para gateways com redundância de zona ou zonal não é atualmente suportada. No entanto, pode eliminar o gateway existente e volte a criar um gateway com redundância de zona ou zonal.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Pode implementar gateways VPN e Express Route na mesma rede virtual?

Coexistência de gateways VPN e Express Route na mesma rede virtual é suportada durante a pré-visualização pública. No entanto, tenha em atenção os seguintes requisitos e limitações:

* Reserva uma /27 intervalo de endereços IP para a sub-rede do gateway.
* Zona-redundante/zonal gateways do Expressroute só podem coexistir com zona-redundante/zonal gateways de VPN.
* Implemente o gateway Expressroute zona-redundante/zonal antes de implementar o gateway VPN zona-redundante/zonal.
* Um gateway Expressroute zona-redundante/zonal pode ser ligado a, no máximo, 4 circuitos.

## <a name="next-steps"></a>Passos Seguintes

Agradecemos os seus comentários. Envie um e-mail para aznetworkgateways@microsoft.com reporte quaisquer problemas ou os seus comentários (positivo ou negativo) para os gateways com redundância de zona e zonal VPN e Expressroute. Inclua o nome da sua empresa no ["]" na linha de assunto. Também inclua o ID de subscrição, se estiverem a comunicar um problema.
