---
title: Criar um gateway de rede virtual com redundância de zona em zonas de disponibilidade do Azure - pré-visualização | Documentos da Microsoft
description: Implemente gateways do ExpressRoute e Gateway de VPN em zonas de disponibilidade - pré-visualização.
services: vpn-gateway
documentationcenter: na
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: article
ms.date: 07/09/2018
ms.author: cherylmc
ms.openlocfilehash: fa349555a5effd41ca519cbd5a29005203d79543
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952560"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones---preview"></a>Criar um gateway de rede virtual com redundância de zona em zonas de disponibilidade do Azure - pré-visualização

Pode implementar gateways VPN e ExpressRoute no [zonas de disponibilidade do Azure](../availability-zones/az-overview.md). Isso leva a maior disponibilidade, escalabilidade e resiliência a gateways de rede virtual. Implementar gateways em zonas de disponibilidade do Azure, física e logicamente separa gateways dentro de uma região, ao proteger a conectividade da rede no local para o Azure contra falhas de nível de zona.

Gateways com redundância de zona e zonais têm melhorias de desempenho fundamentais sobre gateways de rede virtual regular. Além disso, a criação de um gateway de rede virtual com redundância de zona ou zonal é mais rápido do que criar outros gateways. Em vez de passar 45 minutos, crie vezes demoram aproximadamente 15 minutos para um gateway do ExpressRoute e 19 minutos para um gateway de VPN.

### <a name="zrgw"></a>Gateways com redundância de zona

Para implementar automaticamente os seus gateways de rede virtual em zonas de disponibilidade, pode utilizar gateways de rede virtual com redundância de zona. Com os gateways com redundância de zona, pode aproveitar o SLA de 99,99% de tempo de atividade em disponibilidade geral para aceder aos seus serviços de missão crítica e dimensionáveis no Azure.

<br>
<br>

![gráfico de gateways de zona redunant](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Gateways zonais

Para implementar gateways numa zona específica, utilize gateways zonais. Quando implementa um gateway zonal, ambas as instâncias do gateway são implementadas na mesma zona de disponibilidade.

<br>
<br>

![gráfico de zonal gateways](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>SKUs de Gateway

Gateways com redundância de zona e zonais tem de utilizar os novos SKU de gateway. Uma vez que [inscrever na pré-visualização do](#enroll), verá o novo gateway de rede virtual SKUs em todas as regiões do Azure AZ. Essas SKUs são semelhantes para os SKUs correspondentes para o ExpressRoute e Gateway de VPN, exceto pelo fato de são específicas para gateways com redundância de zona e zonais.

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

Gateways com redundância de zona e zonais gateways contam com o recurso IP público do Azure *padrão* SKU. A configuração do recurso de IP público do Azure determina se o gateway que implementar com redundância de zona, ou zonais. Se criar um recurso IP público com um *básica* SKU, o gateway não terá nenhuma redundância de zona e os recursos de gateway serão regionais.

### <a name="pipzrg"></a>Gateways com redundância de zona

Quando cria um através de endereços IP público a **padrão** SKU de IP público sem especificar uma zona, o comportamento difere consoante se o gateway é um gateway de VPN ou um gateway do ExpressRoute. 

* Para um gateway VPN, as instâncias de dois gateway serão implementadas em qualquer 2 fora essas três zonas para fornecer redundância de zona. 
* Para um gateway do ExpressRoute, uma vez que pode haver mais de duas instâncias, o gateway pode ser distribuídas em todas as três zonas.

### <a name="pipzg"></a>Gateways zonais

Quando cria um através de endereços IP público a **padrão** SKU de IP público e especificar a zona (1, 2 ou 3), todas as instâncias de gateway serão implementadas na mesma zona.

### <a name="piprg"></a>Gateways regionais

Quando cria um através de endereços IP público a **básica** SKU de IP público, o gateway é implementado como um gateway regional e não tem nenhuma redundância de zona, criada para o gateway.

## <a name="before-you-begin"></a>Antes de começar

Pode utilizar o PowerShell instalado localmente no seu computador ou o Azure Cloud Shell. Se optar por instalar e utilizar o PowerShell localmente, esta funcionalidade requer a versão mais recente do módulo do PowerShell.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Para utilizar o PowerShell localmente

Se estiver a utilizar o PowerShell localmente no seu computador, em vez de utilizar o Cloud Shell, tem de instalar o módulo de PowerShell 6.1.1 ou superior. Para verificar a versão do PowerShell que tenha instalado, utilize o seguinte comando:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Se precisar de atualizar, veja [Install Azure PowerShell module (Instalar o módulo do Azure PowerShell)](/powershell/azure/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="enroll"></a>1. Inscrever-se na pré-visualização

Antes de poder configurar um gateway com redundância de zona ou zonal, deve primeiro inscrever sua subscrição na pré-visualização do. Assim que a sua subscrição tiver sido aprovisionada, começa a ver o novo gateway SKUs em todas as regiões do Azure AZ. 

Certifique-se de que tem sessão iniciada na sua conta do Azure e estiver a utilizar a subscrição que pretende que a lista de permissões para esta pré-visualização. Utilize o seguinte exemplo de inscrição:

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName AllowVMSSVirtualNetworkGateway -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

Utilize o seguinte comando para verificar se a funcionalidade de 'AllowVMSSVirtualNetworkGateway' está registrada com a sua subscrição:

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
```

O resultado será semelhante a este exemplo:

![aprovisionado](./media/create-zone-redundant-vnet-gateway/verifypreview.png)

## <a name="variables"></a>2. Declarar as variáveis

Os valores utilizados para obter os passos de exemplo estão listados abaixo. Além disso, alguns dos exemplos utilizam variáveis declaradas dentro os passos. Se estiver a utilizar estes passos em seu próprio ambiente, certifique-se de que substitua estes valores pelos seus próprios. Ao especificar a localização, certifique-se de que a região a que especificar é suportada. Para obter mais informações, consulte a [FAQ](#faq).

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

A sub-rede do gateway contém os endereços IP reservados que utilizam os serviços do gateway de rede virtual. Utilize os exemplos seguintes para adicionar e defina uma sub-rede de gateway:

Adicione a sub-rede do gateway.

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Defina a configuração de sub-rede de gateway para a rede virtual.

```azurepowershell-interactive
$getvnet | Set-AzureRmVirtualNetwork
```
## <a name="publicip"></a>5. Solicitar um endereço IP público
 
Neste passo, escolha as instruções que se aplicam ao gateway que pretende criar. A seleção das zonas para implementar os gateways depende as zonas especificadas para o endereço IP público.

### <a name="ipzoneredundant"></a>Para gateways com redundância de zona

Peça um endereço IP público com um **padrão** PublicIpaddress SKU e não especificar qualquer zona. Neste caso, o endereço IP público Standard criado será um IP público com redundância de zona.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>Para gateways zonais

Peça um endereço IP público com um **padrão** PublicIpaddress SKU. Especifica a zona (1, 2 ou 3). Todas as instâncias de gateway serão implementadas nesta zona.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Para gateways regionais

Peça um endereço IP público com um **básica** PublicIpaddress SKU. Neste caso, o gateway é implementado como um gateway regional e não tem nenhuma redundância de zona, criada para o gateway. As instâncias de gateway são criadas em qualquer zonas, respectivamente.

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

### <a name="for-expressroute"></a>Para o ExpressRoute

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Para o Gateway VPN

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="feedback"></a>Como fornecer comentários

Agradecemos os seus comentários. Envie um e-mail para aznetworkgateways@microsoft.com para reportar quaisquer problemas ou fornecer comentários (positivo ou negativo) para os gateways VPN e o Express Route com redundância de zona e zonais. Inclua o nome da sua empresa em "[]" na linha de assunto. Também incluem o seu ID de subscrição se está a relatar um problema.

## <a name="faq"></a>FAQ

### <a name="how-do-i-sign-up-for-the-preview"></a>Como me inscrevo para a pré-visualização?

Pode [inscrever o respetivo](#enroll) utilizando os comandos do PowerShell neste artigo.

### <a name="what-will-change-when-i-enroll"></a>O que será alterado se inscrever?

Da sua perspectiva, durante a pré-visualização, pode implementar seus gateways com redundância de zona. Isso significa que todas as instâncias dos gateways serão implementadas em zonas de disponibilidade do Azure, e cada zona de disponibilidade é um domínio de falha e de atualização diferente. Isso faz com que os gateways mais confiável, disponível e resiliente a falhas de zona.

### <a name="can-i-use-the-azure-portal"></a>Pode utilizar o portal do Azure?

Sim, pode utilizar o portal do Azure para a pré-visualização. No entanto, terá de inscrever com o PowerShell ou não será possível utilizar o portal durante a pré-visualização.

### <a name="what-regions-are-available-for-the-preview"></a>Que regiões estão disponíveis para a pré-visualização?

Com redundância de zona e zonais gateways estão disponíveis nas regiões públicas do Azure/de produção.

### <a name="will-i-be-billed-for-participating-in-this-preview"></a>Feita a cobrança para participar nesta pré-visualização?

Não vai ser Taxado dos seus gateways durante a pré-visualização. No entanto, não existe nenhum SLA anexado à sua implementação. Estamos muito interessados em ouvir os seus comentários.

> [!NOTE]
> Para gateways do ExpressRoute, o gateway não é faturados/cobrado. No entanto, o circuito em si (não o gateway) será faturado.

### <a name="what-regions-are-available-for-me-to-try-this-in"></a>Que regiões estão disponíveis para mim experimentar isso em?

A pré-visualização pública está disponível nas regiões E.U.A. Central e o Centro de França (regiões do Azure que têm zonas de disponibilidade em disponibilidade geral). Daqui em diante, faremos os Gateways com redundância de zona disponíveis para em outras regiões públicas do Azure.

### <a name="can-i-change-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>Posso alterar meu gateways de rede virtual existente para gateways com redundância de zona ou zonais?

Migrar os seus gateways de rede virtual existente para gateways com redundância de zona ou zonais não é atualmente suportada. No entanto, pode eliminar o gateway existente e voltar a criar um gateway com redundância de zona ou zona.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>Pode implementar gateways VPN e o Express Route na mesma rede virtual?

Coexistência de gateways VPN e o Express Route na mesma rede virtual é suportada durante a pré-visualização pública. No entanto, tenha em atenção os seguintes requisitos e limitações:

* Reserva um /27 intervalo de endereços IP para a sub-rede do gateway.
* Zona-redundantes/zonal gateways de Express Route apenas podem coexistir com gateways de VPN de zona-redundantes/zonais.
* Implemente o gateway do Express Route zona-redundantes/zonal antes de implementar o gateway de VPN zona-redundantes/zonais.
* Pode ser ligado um zona-redundantes/zonal gateway do Express Route para, no máximo, 4 circuitos.

## <a name="next-steps"></a>Passos Seguintes

Agradecemos os seus comentários. Envie um e-mail para aznetworkgateways@microsoft.com para reportar quaisquer problemas ou fornecer comentários (positivo ou negativo) para os gateways VPN e o Express Route com redundância de zona e zonais. Inclua o nome da sua empresa em "[]" na linha de assunto. Também incluem o seu ID de subscrição se está a relatar um problema.
