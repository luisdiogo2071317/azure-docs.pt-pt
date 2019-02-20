---
title: O Azure ExpressRoute Global atingir o cenário de aplicação | Documentos da Microsoft
description: Esta página fornece um cenário de aplicação para alcance Global.
documentationcenter: na
services: networking
author: cherylmc
manager: tracsman
ms.service: expressroute,global reach
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/14/2019
ms.author: cherylmc
ms.openlocfilehash: 2adb8debf641a9b3875c5c386df7a35273f2a258
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56428243"
---
# <a name="expressroute-global-reach-application-scenario"></a>Cenário do aplicativo de alcance Global do ExpressRoute

Para saber mais sobre o alcance Global do ExpressRoute, veja [alcance Global do ExpressRoute][Global Reach]. Neste artigo, vamos examinar um cenário de aplicativo, compare a solução alcance Global do ExpressRoute para algumas outras soluções, configurar o alcance Global para o cenário de exemplo e verificar as ligações. 

##<a name="application-scenario"></a>Cenário do aplicativo

Fabrikam Inc. tem um grande presença física e a implementação do Azure na região E.U.A. Leste. A Fabrikam tem conectividade de back-end entre o local e implementações do Azure através do ExpressRoute. Contoso Ltd. tem uma presença e a implementação do Azure em E.U.A. oeste. A Contoso tiver conectividade de back-end entre o local e implementações do Azure através do ExpressRoute.  

Fabrikam Inc. adquire Contoso Ltd. Seguindo a intercalação de pedidos, a Fabrikam quer interligar as redes. A figura seguinte ilustra o cenário:

 [![1]][1]

As setas tracejadas no meio da figura acima indicam as interconexões de rede necessária. A tabela seguinte mostra a tabela de rotas do primário peering privado do ExpressRoute de Contoso Ltd antes da fusão.

[![2]][2]

A tabela seguinte mostra a tabela de rotas do peering privado primário do ExpressRoute da Fabrikam Inc. antes da fusão.

[![3]][3]

## <a name="traditional-solutions"></a>Soluções tradicionais

### <a name="option-1-interconnect-on-premises-networks"></a>Opção 1: Interligar redes no local

A figura a seguir ilustra esta opção. Conforme mostrado, pode interligar as duas redes no local através de VPN de site a site ou outras opções de conexão de banda larga e gerir todos os o encaminhamento entre as duas entidades. 

[![4]][4]

Esta opção tem as seguintes desvantagens:

- Está forçando entre regional comunicação do Azure para a implementação através de uma rota inferior ao ideal.
- Está a negar o benefício de maior disponibilidade da rede de principal da Microsoft para a comunicação entre regional.
- Está fazendo a responsabilidade de encaminhamento completa para a comunicação entre regional.

### <a name="option-2-expressroute-cross-connectivity-and-interconnect-on-premises-networks"></a>Opção 2: ExpressRoute cross conectividade e interligações redes no local

A figura a seguir ilustra esta opção.

[![5]][5]

Conforme mostrado na figura acima, além disso pode estabelecer conectividade entre os circuitos do ExpressRoute para as VNets regionais cruzadas. A conectividade regional cruzada entre a VNet para os circuitos do ExpressRoute iria permitir que as seguintes comunicações:

- Oeste dos EUA / E.U. a leste VNets para comunicar, respetivamente, com Fabrikam/Contoso redes no local.
- A VNet dos EUA oeste para comunicar com a VNet E.U. a leste.

A interligação entre as duas redes no local ainda é necessária para as redes no local comunicar entre si.

Esta opção permite a comunicação do Azure entre regional para a implementação privada enfrente através do backbone do Microsoft e a comunicação entre a rede no local para ser realizado através da rede externa. No entanto, a principal desvantagem da solução é que tem de estabelecer várias cruzadas regionais ligações, que dificultar a manutenção e solução de problemas. Além disso, a opção não permitem-lhe tirar partido da elevada disponibilidade global estrutura principal da Microsoft para a comunicação entre as duas redes no local.

### <a name="option-3-vnet-peering-and-interconnect-on-premises-networks"></a>Opção 3: O VNet peering e interligações redes no local

A figura a seguir ilustra esta opção. A opção utiliza o VNet peering para a comunicação de VNet entre regional. A opção, conforme ilustrado, está incompleta à medida que ela não resolve a VNet entre regiões para comunicação no local (indicada por meio de duas linhas de seta com pontos no meio). Utilização no local para a ligação no local (como no opção 1) ou o ExpressRoute em várias conectividade (como na opção 2) para o inter-regional para comunicação no local.

[![6]][6]

Esta opção fornece encaminhamento ideal para a comunicação de VNet entre regional. No entanto, ele deixa a desejar se a Fabrikam ou Contoso tiver uma implementação do Azure mais complexa, como um modelo de VNet do hub-and-spoke. Além disso, como as duas opções anteriores, esta opção não permitem-lhe tirar partido da elevada disponibilidade global estrutura principal da Microsoft para a comunicação entre as duas redes no local.

## <a name="global-reach"></a>Alcance Global

Alcance Global do ExpressRoute liga o peering privado de circuitos do ExpressRoute, conforme ilustrado na figura a seguir:

[![7]][7]

Configurar o alcance Global entre dois circuitos do ExpressRoute permite a comunicação privada entre as duas redes no local e a implementação do Azure em duas regiões. Portanto, o alcance Global satisfaz toda a comunicação desejada (indicada por meio de uma linha tracejada da primeira figura deste artigo) através da rede de backbone do Microsoft.

### <a name="configure-global-reach"></a>Configurar o alcance Global

Para saber como configurar o alcance Global do ExpressRoute, veja [configurar o alcance Global][Configure Global Reach]. 

Uma vez que Fabrikam Inc. e Contoso Ltd. integrado do Azure como as empresas separados, os circuitos do ExpressRoute de duas empresas estão em duas diferentes subscrições do Azure. Para criar o alcance Global em subscrições, tem de criar uma autorização no circuito de ExpressRoute que pertencem à Contoso Ltd. e passá-lo para a Fabrikam, Inc. Circuito do ExpressRoute.


Para criar uma autorização para o circuito de ExpressRoute da Contoso, o primeiro início de sessão na conta do Azure da Contoso e selecione a subscrição adequada (se existirem várias subscrições). Os comandos do PowerShell para essas etapas são:

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```
Os passos para criar uma autorização de circuito do ExpressRoute estão listados abaixo:

```powershell
$ckt_2 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_2_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $ckt_2 -Name "Name_for_auth_key"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_2
```

A saída de 'Set-AzureRmExpressRouteCircuit' irá listar o circuito do Expressroute. Tenha em atenção que o ID de peering privado e a chave de autorização que seriam listada até o final da lista. Veja um fragmento de saída de PowerShell de exemplo abaixo:

```powershell
ServiceKey                       : <serviceKey>
Peerings                         : [
                                     {
                                       "Name": "AzurePrivatePeering",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/peerings/AzurePrivatePeering",
                                       "PeeringType": "AzurePrivatePeering",
                                       "State": "Enabled",
                                       "AzureASN": 12076,
                                       "PeerASN": 65020,
                                       "PrimaryPeerAddressPrefix": "192.168.11.16/30",
                                       "SecondaryPeerAddressPrefix": "192.168.11.20/30",
                                       "PrimaryAzurePort": "",
                                       "SecondaryAzurePort": "",
                                       "VlanId": 110,
                                       "ProvisioningState": "Succeeded",
                                       "GatewayManagerEtag": "4",
                                       "LastModifiedBy": "Customer",
                                       "Connections": []
                                     }
                                   ]
Authorizations                   : [
                                     {
                                       "Name": "Auth4-ASH-Cust11",
                                       "Etag": "W/\"78dfeeae-0485-4cda-b5aa-99e780589677\"",
                                       "Id": "/subscriptions/<subscriptionId>/resourceGroups/SEA-Cust11/providers/Microsoft.Network/expressRouteCircuits/SEA-Cust11-ER/authorizations/Auth4-ASH-Cust11",
                                       "AuthorizationKey": "<authorizationKey>",
                                       "AuthorizationUseStatus": "Used",
                                       "ProvisioningState": "Succeeded"
                                     }
                                   ]
AllowClassicOperations           : False
GatewayManagerEtag               : 4
```

Com o ID de peering e a chave de autorização, pode criar o alcance Global em circuito do ExpressRoute da Fabrikam. Início de sessão na conta do Azure da Fabrikam. Se houver mais de uma subscrição, selecione a subscrição adequada.

Alcance global cria um conjunto com redundância de ligações ponto a ponto entre os dois pares MSEE. Para ligações ponto a dois ponto, tem de especificar/29 prefixo de endereço (para o exemplo em execução vamos utilizar 192.168.11.64/29). Utilize os seguintes comandos para criar a ligação alcance Global:

```powershell
$ckt_1 = Get-AzureRmExpressRouteCircuit -Name "Your_circuit_1_name" -ResourceGroupName "Your_resource_group"
Add-AzureRmExpressRouteCircuitConnectionConfig -Name "Your_connection_name" -ExpressRouteCircuit $ckt_1 -PeerExpressRouteCircuitPeering "circuit_2_private_peering_id" -AddressPrefix "__.__.__.__/29" -AuthorizationKey "########-####-####-####-############"
Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt_1
```

Assim que os comandos acima são executados, irá demorar alguns minutos para criar as ligações redundantes de alcance Global.

### <a name="verify-global-reach"></a>Verifique se o alcance Global

A tabela seguinte mostra a tabela de rotas do primário peering privado do ExpressRoute da Contoso Ltd depois de configurar o alcance Global.

[![8]][8]

A tabela seguinte mostra a tabela de rotas do primário peering privado do ExpressRoute da Fabrikam Inc. depois de configurar o alcance Global.

[![9]][9]

Nas tabelas acima, podemos ver todos os prefixos de "Rede" de destino esperado e apropriado "próximo SALTO" estão listado.

Acima pode ver o painel de "Tabela de rota Get", que pode ser acedido no portal do Azure em "Peering privado" de um circuito do ExpressRoute. Também pode listar uma tabela de rota do ExpressRoute com o seguinte comando do PowerShell:

```powershell
Get-AzExpressRouteCircuitRouteTable -DevicePath 'primary' -ExpressRouteCircuitName "Your_circuit_name" -PeeringType AzurePrivatePeering -ResourceGroupName "Your_resource_group"
```

Para ver a tabela de rotas do MSEE secundário, substitua "principal" com a palavra-chave "secondary" no comando acima.

Vamos também verificar a conectividade de plano de dados de ping de destino diferente a partir de redes diferentes. Os pings de três seguintes verificar a conectividade de plano de dados para a rede no local da Contoso, Contoso Azure VNet, e a VNet do Azure Fabrikam da Fabrikam rede no local.


    C:\Users\FabrikamUser>ping 10.1.11.10
    
    Pinging 10.1.11.10 with 32 bytes of data:
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    Reply from 10.1.11.10: bytes=32 time=69ms TTL=122
    
    Ping statistics for 10.1.11.10:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 69ms, Maximum = 69ms, Average = 69ms
    
    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=82ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=81ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=124
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 82ms, Average = 78ms
    
    C:\Users\FabrikamUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=2ms TTL=125
    Reply from 10.10.11.4: bytes=32 time=3ms TTL=125
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 2ms, Maximum = 3ms, Average = 2ms


Os seguintes dois pings Verifique a conectividade de plano de dados para a VNet do Azure de Contoso e Fabrikam Azure VNet da rede no local de Contoso.

    C:\Users\ContosoUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=6ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=5ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 5ms, Maximum = 6ms, Average = 5ms
    
    C:\Users\ContosoUser>ping 10.10.11.4
    
    Pinging 10.10.11.4 with 32 bytes of data:
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=74ms TTL=124
    Reply from 10.10.11.4: bytes=32 time=73ms TTL=124
    
    Ping statistics for 10.10.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 73ms, Maximum = 74ms, Average = 73ms

O ping seguinte verifica a conetividade de plano de dados à VNet do Azure de Contoso da Fabrikam Azure VNet.

    C:\Users\FabrikamUser>ping 10.17.11.4
    
    Pinging 10.17.11.4 with 32 bytes of data:
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=77ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=76ms TTL=125
    Reply from 10.17.11.4: bytes=32 time=78ms TTL=125
    
    Ping statistics for 10.17.11.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 76ms, Maximum = 78ms, Average = 77ms

## <a name="optimization-for-latency-sensitive-traffic"></a>Otimização para o tráfego de confidenciais de latência

Alcance global encaminha o tráfego através de dispositivos do Microsoft Edge. Para o cenário específico considerado neste artigo, pode conseguir mais encaminhamento ideal entre as duas VNets, permitindo que o VNet peering entre eles. Da mesma forma podem alcançar mais encaminhamento ideal entre as duas redes no local através de um fornecedor de serviço, que pode fornecer uma ligação WAN mais direta entre os sites. Em tais cenários, pode usar o alcance Global encaminhamento como uma opção de back-falha para essas ligações. 

## <a name="next-steps"></a>Passos Seguintes

Alcance global é implementada numa base de país a país. Para ver se o alcance Global está disponível nos países em que pretende, consulte [alcance Global do ExpressRoute][Global Reach].

<!--Image References-->
[1]: ./media/expressroute-globalreach-applicationscenario/PreMergerScenario.png "cenário da aplicação"
[2]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Premerger.png "tabela de rota do ExpressRoute de Contoso antes da intercalação de pedidos"
[3]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Premerger.png "tabela de rota do ExpressRoute da Fabrikam antes da intercalação de pedidos"
[4]: ./media/expressroute-globalreach-applicationscenario/S2SVPN-Solution.png "interconectados redes no local"
[5]: ./media/expressroute-globalreach-applicationscenario/ExR-XConnect-Solution.png "ExpressRoute cruzada ligar"
[6]: ./media/expressroute-globalreach-applicationscenario/VNet-peering-Solution.png "o VNet peering"
[7]: ./media/expressroute-globalreach-applicationscenario/GlobalReach-Solution.png "alcance global"
[8]: ./media/expressroute-globalreach-applicationscenario/ContosoExR-RT-Postmerger.png "tabela de rota do ExpressRoute da Contoso com alcance Global"
[9]: ./media/expressroute-globalreach-applicationscenario/FabrikamExR-RT-Postmerger.png "tabela de rota do Fabrikam ExpressRoute com alcance Global"

<!--Link References-->
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach





