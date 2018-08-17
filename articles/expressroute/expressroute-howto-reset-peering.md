---
title: Repor peerings do ExpressRoute do Azure | Documentos da Microsoft
description: Como desactivar e activar peeerings de um circuito do ExpressRoute.
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: charwen
ms.openlocfilehash: b8c9bc1944e9ed0281616062a84958c953d08694
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180899"
---
# <a name="reset-expressroute-peerings"></a>Repor peerings do ExpressRoute

Este artigo descreve como desactivar e activar peerings de circuito do ExpressRoute com o PowerShell. Quando desativa um peering, a sessão de BGP sobre a ligação primária e a ligação secundária do seu circuito do ExpressRoute será encerrada. Perderá conectividade através deste peering para a Microsoft. Quando ativa um peering, irá ser recuperada a sessão de BGP sobre a ligação primária e a ligação secundária do seu circuito do ExpressRoute. Irá recuperar a conectividade através deste peering para a Microsoft. Pode ativar e desativar o Peering da Microsoft e o Peering privado do Azure num circuito do ExpressRoute independentemente. Quando o primeiro de configurar os peerings no seu circuito do ExpressRoute, os peerings estão ativados por predefinição. 

Existem alguns cenários em que talvez ache útil a repor o seu peerings do ExpressRoute.
* Teste seu design de recuperação após desastre e implementação. Por exemplo, tem dois circuitos do ExpressRoute. Pode desativar os peerings de um circuito e forçar o tráfego de rede para a ativação pós-falha para o outro circuito.
* Ative bidirecional reencaminhamento de deteção (BFD) no Peering privado do Azure do seu circuito do ExpressRoute. BFD está ativada por predefinição, se o seu circuito do ExpressRoute é criado após 1 de Agosto de 2018. Se o seu circuito foi criado antes disso, BFD não foi ativada. Pode ativar BFD desativando o peering e reativar a ele. É importante observar que BFD é apenas suportado no Peering privado do Azure.


## <a name="reset-a-peering"></a>Repor um peering

1. Instale a versão mais recente dos cmdlets do PowerShell do Azure Resource Manager. Para obter mais informações, veja [nstall and configure Azure PowerShell (Instalar e configurar o Azure PowerShell)](/powershell/azure/install-azurerm-ps).

2. Abra a consola do PowerShell com privilégios elevados e ligue-se à sua conta. Utilize o exemplo seguinte para o ajudar na ligação:

  ```powershell
  Connect-AzureRmAccount
  ```
3. Se tiver múltiplas subscrições do Azure, verifique as subscrições da conta.

  ```powershell
  Get-AzureRmSubscription
  ```
4. Especifique a subscrição que pretende utilizar.

  ```powershell
  Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
  ```
5. Execute os seguintes comandos para recuperar o seu circuito do ExpressRoute.

  ```powershell
  $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
  ```
6. Identifique o peering que pretende desactivar ou activar. *Peerings* é uma matriz. No exemplo a seguir, Peerings [0] é o Peering privado do Azure e Peering da Microsoft Peerings [1].

  ```powershell
Name                             : ExpressRouteARMCircuit
ResourceGroupName                : ExpressRouteResourceGroup
Location                         : westus
Id                               : /subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
Etag                             : W/"cd011bef-dc79-49eb-b4c6-81fb6ea5d178"
ProvisioningState                : Succeeded
Sku                              : {
                                     "Name": "Standard_MeteredData",
                                     "Tier": "Standard",
                                     "Family": "MeteredData"
                                   }
CircuitProvisioningState         : Enabled
ServiceProviderProvisioningState : Provisioned
ServiceProviderNotes             :
ServiceProviderProperties        : {
                                     "ServiceProviderName": "Coresite",
                                     "PeeringLocation": "Los Angeles",
                                     "BandwidthInMbps": 50
                                   }
ServiceKey                       : ########-####-####-####-############
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "10.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "10.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 789,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "NotConfigured",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     },
                                     {
                                       "Name": "MicrosoftPeering",
                                       "Etag": "W/\"cd011bef-dc79-49eb-b4c6-81fb6ea5d178\"",
                                       "Id": "/subscriptions/########-####-####-####-############/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit/peerings/MicrosoftPeering",
                                       "PeeringType": "MicrosoftPeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 123,
                                       "PrimaryPeerAddressPrefix": "3.0.0.0/30",
                                       "SecondaryPeerAddressPrefix": "3.0.0.4/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 345,
                                       "MicrosoftPeeringConfig": {
                                         "AdvertisedPublicPrefixes": [
                                           "3.0.0.3/32"
                                         ],
                                         "AdvertisedCommunities": [],
                                         "AdvertisedPublicPrefixesState": "ValidationNeeded",
                                         "CustomerASN": 0,
                                         "LegacyMode": 0,
                                         "RoutingRegistryName": "NONE"
                                       },
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : []
AllowClassicOperations           : False
GatewayManagerEtag               :
  ```
7. Execute os seguintes comandos para alterar o estado do peering.

  ```powershell
  $ckt.Peerings[0].State = "Disabled"
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
  ```
O peering deve estar num Estado que definir. 

## <a name="next-steps"></a>Passos Seguintes
Se precisar de ajuda para resolver um problema do ExpressRoute, veja os artigos seguintes:
* [Verificar a conectividade do ExpressRoute](expressroute-troubleshooting-expressroute-overview.md)
* [Resolução de problemas de desempenho da rede](expressroute-troubleshooting-network-performance.md)
