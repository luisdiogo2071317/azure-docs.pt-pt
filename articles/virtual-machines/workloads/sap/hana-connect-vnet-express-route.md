---
title: Configuração de conectividade de rede virtual para o SAP HANA no Azure (instâncias grandes) | Documentos da Microsoft
description: Configuração de conectividade de rede virtual para utilizar o SAP HANA no Azure (instâncias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5efdda485e4e1f5013948c6636b267f0d388f4d5
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44164736"
---
# <a name="connecting-a-vnet-to-hana-large-instance-expressroute"></a>Ligar uma VNet para ExpressRoute de instância grande do HANA

À medida que definidos todos os intervalos de endereços IP e agora temos novamente os dados da Microsoft, pode começar a ligar a VNet que criou anteriormente para instâncias grandes do HANA. Depois de criar a VNet do Azure, um gateway do ExpressRoute tem de ser criado na VNet para ligar a VNet para o circuito do ExpressRoute que se liga ao inquilino de cliente no carimbo a instância grande.

> [!NOTE] 
> Este passo pode demorar até 30 minutos a concluir, como o novo gateway é criado na subscrição do Azure designada e, em seguida, ligado à VNet do Azure especificada.

Se já existir um gateway, verifique se é um gateway do ExpressRoute ou não. Caso contrário, o gateway tem de ser eliminado e recriado como um gateway do ExpressRoute. Se já estiver estabelecido um gateway do ExpressRoute, uma vez que a VNet do Azure já está ligada ao circuito do ExpressRoute para conectividade no local, avance para a secção de ligar VNets abaixo.

- Utilizar qualquer um da (novo) [portal do Azure](https://portal.azure.com/), ou o PowerShell para criar um gateway de VPN do ExpressRoute ligado à VNet.
  - Se utilizar o portal do Azure, adicione uma nova **Gateway de rede Virtual** e, em seguida, selecione **ExpressRoute** como o tipo de gateway.
  - Se tiver escolhido em vez disso, o PowerShell, primeiro de transferir e utilizar a versão mais recente [Azure PowerShell SDK](https://azure.microsoft.com/downloads/) para garantir uma experiência ideal. Os comandos seguintes criam um gateway do ExpressRoute. Os textos precedidos por um _$_ são variáveis definidas pelo utilizador que precisam ser atualizados com as suas informações específicas.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

Neste exemplo, foi utilizado o SKU de gateway de alto desempenho. As opções são HighPerformance ou UltraPerformance, como o gateway só SKUs que são suportadas para SAP HANA no Azure (instâncias grandes).

> [!IMPORTANT]
> Para instâncias grandes do HANA do classs de tipo II SKU, a utilização do SKU de Gateway de UltraPerformance é obrigatória.

**Ligar VNets**

Agora que a VNet do Azure tem um gateway do ExpressRoute, que utilize as informações de autorização fornecidas pela Microsoft para ligar o gateway do ExpressRoute para o SAP HANA no circuito de ExpressRoute do Azure (instâncias grandes) criado para este conectividade. Este passo pode ser efetuado utilizando o portal do Azure ou o PowerShell. O portal é recomendada, no entanto instruções do PowerShell são os seguintes. 

- Execute os seguintes comandos para cada gateway de VNet com uma AuthGUID diferente para cada ligação. As duas primeiras entradas mostradas a seguir de script são provenientes as informações fornecidas pela Microsoft. Além disso, o AuthGUID é específico para cada VNet e o seu gateway. Significa que, se pretender adicionar outra VNet do Azure, terá de obter AuthID outro para o seu circuito do ExpressRoute que se liga instâncias grandes do HANA no Azure. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Se pretender ligar o gateway a vários circuitos do ExpressRoute que estão associados a sua subscrição, terá de executar este passo de mais de uma vez. Por exemplo, provavelmente pretende ligar o mesmo Gateway de VNet para o circuito do ExpressRoute que se liga a VNet à sua rede no local.

**Passos seguintes?**

- Consultar [requisitos de rede adicionais para HLI](hana-additional-network-requirements.md).