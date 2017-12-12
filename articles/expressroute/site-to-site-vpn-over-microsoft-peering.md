---
title: "Configurar uma VPN de site a site através de peering da Microsoft para Azure ExpressRoute | Microsoft Docs"
description: "Configure a ligação IPsec/IKE para o Azure através de um circuito de peering de ExpressRoute Microsoft com um gateway de VPN de site para site."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/06/2017
ms.author: cherylmc
ms.openlocfilehash: 64203e2cbac1206224f0e0ad8b7d364f19ad0332
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/07/2017
---
# <a name="configure-a-site-to-site-vpn-over-expressroute-microsoft-peering"></a>Configurar uma VPN de site a site através de peering da Microsoft do ExpressRoute

Este artigo ajuda-o a configurar a conectividade encriptada segura entre a rede no local e as redes virtuais (VNets) do Azure através de uma ligação privada do ExpressRoute. A configuração de um túnel seguro através do ExpressRoute permite para intercâmbio de dados com confidencialidade, reprodução anti, autenticidade e integridade.

## <a name="architecture"></a>Arquitetura

Pode tirar partido do peering para estabelecer um túnel VPN IPsec/IKE de site a site entre os selecionado redes no local e as VNets do Azure da Microsoft.

  ![Descrição geral de conectividade](./media/site-to-site-vpn-over-microsoft-peering/IPsecER_Overview.png)

>[!NOTE]
>Quando configurar a VPN site a site através de Microsoft peering, são-lhe cobrados para o gateway de VPN e de saída VPN. Para obter mais informações, consulte [preços dos gateways de VPN](https://azure.microsoft.com/pricing/details/vpn-gateway).
>
>

Para elevada disponibilidade e redundância, pode configurar vários túneis através de dois pares de MSEE PE de um circuito do ExpressRoute e ativar o balanceamento da carga entre os túneis.

  ![Opções de elevada disponibilidade](./media/site-to-site-vpn-over-microsoft-peering/HighAvailability.png)

Túneis VPN através de peering da Microsoft podem ser terminadas com gateway de VPN, ou utilizando um adequado rede Virtual dispositivo (NVA) disponíveis através do Azure Marketplace. Pode trocar as rotas estaticamente ou dinamicamente através dos túneis encriptados sem exposição a troca de rotas para o peering da Microsoft subjacente. Os exemplos neste artigo, BGP (diferente da sessão de BGP utilizada para criar peering da Microsoft) é utilizada para trocar dinamicamente prefixos através dos túneis encriptados.

>[!IMPORTANT]
>Para o lado no local, normalmente peering da Microsoft termina no DMZ e peering privado termina na zona de rede principal. As duas zonas seriam separadas através de firewalls. Se estiver a configurar o peering da Microsoft exclusivamente para ativar o protocolo de túnel seguro através do ExpressRoute, lembre-se filtrar através de IPs públicos de interesse são obter anunciados através do peering da Microsoft.
>
>

## <a name="workflow"></a>Fluxo de trabalho

1. Configure o peering da Microsoft para o seu circuito do ExpressRoute.
2. Anuncie selecionados Azure regionais prefixos públicos à sua rede no local através do peering da Microsoft.
3. Configurar um gateway VPN e estabelecer túneis IPsec
4. Configure o dispositivo VPN no local.
5. Crie a ligação do IPsec/IKE de site para site.
6. (Opcional) Configure firewalls/filtragem no dispositivo VPN no local.
7. Testar e validar a comunicação de IPsec do circuito ExpressRoute.

## <a name="peering"></a>1. configurar o peering da Microsoft

Para configurar uma ligação de VPN de site a site através do ExpressRoute, terá de tirar partido de peering da Microsoft do ExpressRoute.

* Para configurar um novo circuito do ExpressRoute, começar a utilizar o [pré-requisitos do ExpressRoute](expressroute-prerequisites.md) artigo e, em seguida, [criar e modificar um circuito ExpressRoute](expressroute-howto-circuit-arm.md).

* Se já tiver um circuito do ExpressRoute, mas não dispõe de peering da Microsoft configurado, configurar o peering da Microsoft a utilizar o [criar e modificar o peering de um circuito ExpressRoute](expressroute-howto-routing-arm.md#msft) artigo.

Assim que tiver configurado o circuito e peering da Microsoft, pode visualizar facilmente-la utilizando o **descrição geral** página no portal do Azure.

![circuito](./media/site-to-site-vpn-over-microsoft-peering/ExpressRouteCkt.png)

## <a name="routefilter"></a>2. Configurar filtros de rotas

Um filtro de rota permite-lhe identificar os serviços que pretende consumir através do peering da Microsoft do circuito do ExpressRoute. É, essencialmente, uma lista branca de todos os valores de Comunidades BGP. 

![filtro de rota](./media/site-to-site-vpn-over-microsoft-peering/route-filter.png)

Neste exemplo, a implementação está apenas nos *Azure EUA oeste 2* região. Uma regra de filtro de rota é adicionada ao permitir que apenas o anúncio de Azure EUA oeste 2 regionais prefixos, que tem o valor das Comunidades de BGP *12076:51026*. Especifique os prefixos regionais que pretende permitir selecionando **gerir regra**.

Dentro do filtro de rota, também tem de escolher os circuitos do ExpressRoute para o qual se aplica o filtro de rota. Pode escolher os circuitos do ExpressRoute, selecionando **adicionar circuito**. A ilustração anterior, o filtro de rota é associado ao exemplo circuito ExpressRoute.

### <a name="configfilter"></a>2.1 configurar o filtro de rota

Configure um filtro de rota. Para obter os passos, consulte [configurar filtros de rota para peering da Microsoft](how-to-routefilter-portal.md).

### <a name="verifybgp"></a>2.2 verificar as rotas BGP

Depois de ter com êxito criar peering do circuito de ExpressRoute da Microsoft e associados um filtro de rota com o circuito, pode verificar as rotas BGP receberam MSEEs nos dispositivos PE que são peering com os MSEEs. O comando de verificação varia consoante o sistema operativo dos dispositivos de PE.

#### <a name="cisco-examples"></a>Exemplos da Cisco

Este exemplo utiliza um comando de Cisco IOS-XE. No exemplo, um encaminhamento virtual e reencaminhamento de instância (VRF) é utilizado para isolar o tráfego de peering.

```
show ip bgp vpnv4 vrf 10 summary
```

O seguinte resultado parcial mostra que 68 prefixos foram recebeu o vizinho *.243.229.34 com 12076 o ASN (MSEE):

```
...

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
X.243.229.34    4        12076   17671   17650    25228    0    0 1w4d           68
```

Para ver a lista de prefixos recebidos a partir de vizinho, utilize o seguinte exemplo:

```
sh ip bgp vpnv4 vrf 10 neighbors X.243.229.34 received-routes
```

Para confirmar que estão a receber o conjunto correto de prefixos, pode cross-verificar. Resultado do comando seguinte do Azure PowerShell apresenta os prefixos anunciados através do peering para cada um dos serviços e para cada região do Azure da Microsoft:

```powershell
Get-AzureRmBgpServiceCommunity
```

## <a name="vpngateway"></a>3. Configurar o gateway VPN e de túneis IPsec

Nesta secção, são criados túneis IPsec VPN entre o gateway de VPN do Azure e o dispositivo VPN no local. Os exemplos utilizam dispositivos VPN de Router de serviço de nuvem do Cisco (CSR1000).

O diagrama seguinte mostra a VPN IPsec túneis estabelecidas entre o dispositivo VPN no local 1 e o par de instância de gateway de VPN do Azure. Os túneis IPsec VPN dois estabeleceram entre o dispositivo VPN no local 2 e o par de instância de gateway de VPN do Azure não é ilustrado no diagrama e os detalhes de configuração não estão listados. No entanto, ter adicionais túneis VPN melhora a elevada disponibilidade.

  ![Túneis VPN](./media/site-to-site-vpn-over-microsoft-peering/EstablishTunnels.png)

Ao longo do par de túnel IPsec, é estabelecida uma sessão de eBGP para trocar rotas de rede privada. O diagrama seguinte mostra a sessão de eBGP estabelecida através do par de túnel IPsec:

  ![sessões de eBGP ao longo do par de túnel](./media/site-to-site-vpn-over-microsoft-peering/TunnelBGP.png)

O diagrama seguinte mostra abstracted descrição geral da rede de exemplo:

  ![rede de exemplo](./media/site-to-site-vpn-over-microsoft-peering/OverviewRef.png)

### <a name="about-the-azure-resource-manager-template-examples"></a>Sobre os exemplos de modelo Azure Resource Manager

Nos exemplos, o gateway VPN e terminations de túnel IPsec são configuradas através de um modelo Azure Resource Manager. Se estiver a utilizar modelos do Resource Manager ou, para compreender as noções básicas modelo do Resource Manager, consulte [compreender a estrutura e a sintaxe de modelos Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). O modelo desta secção cria uma greenfield ambiente do Azure (VNet). No entanto, se tiver uma VNet existente, pode referenciá-lo no modelo. Se não estiver familiarizado com as configurações do VPN gateway IPsec/IKE site a site, consulte [criar uma ligação site a site](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md).

>[!NOTE]
>Não é necessário utilizar modelos Azure Resource Manager para criar esta configuração. Pode criar esta configuração utilizando o portal do Azure ou o PowerShell.
>
>

### <a name="variables3"></a>3.1 declarar as variáveis

Neste exemplo, as declarações de variável de corresponder à rede de exemplo. Quando declarar as variáveis, modifique esta secção para refletir o seu ambiente.

* A variável **localAddressPrefix** é uma matriz de endereços IP no local para terminar os túneis IPsec.
* O **gatewaySku** determina o débito da VPN. Para obter mais informações sobre o gatewaySku e vpnType, consulte [as definições de configuração do Gateway de VPN](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku). Para os preços, consulte [preços dos gateways de VPN](https://azure.microsoft.com/pricing/details/vpn-gateway).
* Definir o **vpnType** para **RouteBased**.

```json
"variables": {
  "virtualNetworkName": "SecureVNet",       // Name of the Azure VNet
  "azureVNetAddressPrefix": "10.2.0.0/24",  // Address space assigned to the VNet
  "subnetName": "Tenant",                   // subnet name in which tenants exists
  "subnetPrefix": "10.2.0.0/25",            // address space of the tenant subnet
  "gatewaySubnetPrefix": "10.2.0.224/27",   // address space of the gateway subnet
  "localGatewayName": "localGW1",           // name of remote gateway (on-premises)
  "localGatewayIpAddress": "X.243.229.110", // public IP address of the on-premises VPN device
  "localAddressPrefix": [
    "172.16.0.1/32",                        // termination of IPsec tunnel-1 on-premises 
    "172.16.0.2/32"                         // termination of IPsec tunnel-2 on-premises 
  ],
  "gatewayPublicIPName1": "vpnGwVIP1",    // Public address name of the first VPN gateway instance
  "gatewayPublicIPName2": "vpnGwVIP2",    // Public address name of the second VPN gateway instance 
  "gatewayName": "vpnGw",                 // Name of the Azure VPN gateway
  "gatewaySku": "VpnGw1",                 // Azure VPN gateway SKU
  "vpnType": "RouteBased",                // type of VPN gateway
  "sharedKey": "string",                  // shared secret needs to match with on-premise configuration
  "asnVpnGateway": 65000,                 // BGP Autonomous System number assigned to the VPN Gateway 
  "asnRemote": 65010,                     // BGP Autonmous Syste number assigned to the on-premises device
  "bgpPeeringAddress": "172.16.0.3",      // IP address of the remote BGP peer on-premises
  "connectionName": "vpn2local1",
  "vnetID": "[resourceId('Microsoft.Network/virtualNetworks', variables('virtualNetworkName'))]",
  "gatewaySubnetRef": "[concat(variables('vnetID'),'/subnets/','GatewaySubnet')]",
  "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]",
  "api-version": "2017-06-01"
},
```

### <a name="vnet"></a>3.2 criar rede virtual (VNet)

Se estiver a associação a uma VNet existente com os túneis VPN, pode ignorar este passo.

```json
{
  "apiVersion": "[variables('api-version')]",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "[variables('azureVNetAddressPrefix')]"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "[variables('subnetPrefix')]"
        }
      },
      {
        "name": "GatewaySubnet",
        "properties": {
          "addressPrefix": "[variables('gatewaySubnetPrefix')]"
        }
      }
    ]
  },
  "comments": "Create a Virtual Network with Subnet1 and Gatewaysubnet"
},
```

### <a name="ip"></a>3.3 atribuir endereços IP públicos para instâncias de gateway VPN
 
Atribua um endereço IP público para cada instância de um gateway de VPN.

```json
{
  "apiVersion": "[variables('api-version')]",
  "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('gatewayPublicIPName1')]",
    "location": "[resourceGroup().location]",
    "properties": {
      "publicIPAllocationMethod": "Dynamic"
    },
    "comments": "Public IP for the first instance of the VPN gateway"
  },
  {
    "apiVersion": "[variables('api-version')]",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('gatewayPublicIPName2')]",
    "location": "[resourceGroup().location]",
    "properties": {
      "publicIPAllocationMethod": "Dynamic"
    },
    "comments": "Public IP for the second instance of the VPN gateway"
  },
```

### <a name="termination"></a>3.4 especificar a terminação de túnel VPN no local (gateway de rede local)

Os dispositivos VPN no local são referidos como o **gateway de rede local**. O fragmento json seguinte também especifica os detalhes de elemento de rede BGP remotos:

```json
{
  "apiVersion": "[variables('api-version')]",
  "type": "Microsoft.Network/localNetworkGateways",
  "name": "[variables('localGatewayName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "localNetworkAddressSpace": {
      "addressPrefixes": "[variables('localAddressPrefix')]"
    },
    "gatewayIpAddress": "[variables('localGatewayIpAddress')]",
    "bgpSettings": {
      "asn": "[variables('asnRemote')]",
      "bgpPeeringAddress": "[variables('bgpPeeringAddress')]",
      "peerWeight": 0
    }
  },
  "comments": "Local Network Gateway (referred to your on-premises location) with IP address of remote tunnel peering and IP address of remote BGP peer"
},
```

### <a name="creategw"></a>3.5 criar o gateway VPN

Esta secção do modelo de configura o gateway VPN com as definições necessárias para uma configuração de ativo-ativo. Tenha em consideração os seguintes requisitos:

* Criar o gateway VPN com uma **"RouteBased"** VpnType. Esta definição é obrigatória se pretender ativar o encaminhamento de BGP entre o gateway VPN e a VPN no local.
* Para estabelecer túneis VPN entre as duas instâncias do gateway VPN e um determinado dispositivo no local no modo ativo-ativo, o **"activeActive"** parâmetro está definido como **verdadeiro** no modelo do Resource Manager . Para saber mais sobre os gateways de VPN altamente disponíveis, consulte [conectividade de gateway VPN elevada](../vpn-gateway/vpn-gateway-highlyavailable.md).
* Para configurar sessões de eBGP entre os túneis VPN, tem de especificar dois ASNs diferentes em ambos os lados. É preferível para especificar os números ASN privados. Para obter mais informações, consulte [gateways de descrição geral do BGP e VPN do Azure](../vpn-gateway/vpn-gateway-bgp-overview.md).

```json
{
"apiVersion": "[variables('api-version')]",
"type": "Microsoft.Network/virtualNetworkGateways",
"name": "[variables('gatewayName')]",
"location": "[resourceGroup().location]",
"dependsOn": [
  "[concat('Microsoft.Network/publicIPAddresses/', variables('gatewayPublicIPName1'))]",
  "[concat('Microsoft.Network/publicIPAddresses/', variables('gatewayPublicIPName2'))]",
  "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
],
"properties": {
  "ipConfigurations": [
    {
      "properties": {
        "privateIPAllocationMethod": "Dynamic",
        "subnet": {
          "id": "[variables('gatewaySubnetRef')]"
        },
        "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('gatewayPublicIPName1'))]"
        }
      },
      "name": "vnetGtwConfig1"
    },
    {
      "properties": {
        "privateIPAllocationMethod": "Dynamic",
        "subnet": {
          "id": "[variables('gatewaySubnetRef')]"
        },
        "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('gatewayPublicIPName2'))]"
        }
      },
          "name": "vnetGtwConfig2"
        }
      ],
      "sku": {
        "name": "[variables('gatewaySku')]",
        "tier": "[variables('gatewaySku')]"
      },
      "gatewayType": "Vpn",
      "vpnType": "[variables('vpnType')]",
      "enableBgp": true,
      "activeActive": true,
      "bgpSettings": {
        "asn": "[variables('asnVpnGateway')]"
      }
    },
    "comments": "VPN Gateway in active-active configuration with BGP support"
  },
```

### <a name="ipsectunnel"></a>3.6 estabelecer túneis IPsec

A ação final do script cria túneis IPsec entre o gateway de VPN do Azure e o dispositivo VPN no local.

```json
{
  "apiVersion": "[variables('api-version')]",
  "name": "[variables('connectionName')]",
  "type": "Microsoft.Network/connections",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworkGateways/', variables('gatewayName'))]",
    "[concat('Microsoft.Network/localNetworkGateways/', variables('localGatewayName'))]"
  ],
  "properties": {
    "virtualNetworkGateway1": {
      "id": "[resourceId('Microsoft.Network/virtualNetworkGateways', variables('gatewayName'))]"
    },
    "localNetworkGateway2": {
      "id": "[resourceId('Microsoft.Network/localNetworkGateways', variables('localGatewayName'))]"
    },
    "connectionType": "IPsec",
    "routingWeight": 0,
    "sharedKey": "[variables('sharedKey')]",
    "enableBGP": "true"
  },
  "comments": "Create a Connection type site-to-site (IPsec) between the Azure VPN Gateway and the VPN device on-premises"
  }
```

## <a name="device"></a>4. Configurar o dispositivo VPN no local

O gateway de VPN do Azure é compatível com vários dispositivos VPN de fornecedores diferentes. Para informações de configuração e dispositivos que foram validados para trabalhar com o gateway de VPN, consulte [acerca dos dispositivos VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md).

Quando configurar o seu dispositivo VPN, terá dos seguintes itens:

* Uma chave partilhada. Esta é a mesma chave partilhada que especificar ao criar a ligação de VPN de site para site. Os exemplos utilizam uma chave partilhada básica. Deve gerar uma chave mais complexa para utilizar.
* O endereço IP público do seu gateway VPN. Pode ver o endereço IP público através do portal do Azure, do PowerShell ou da CLI. Para localizar o endereço IP público do seu gateway VPN no portal do Azure, navegue para gateways de rede Virtual, em seguida, clique no nome do seu gateway.

Normalmente, os elementos de eBGP estão ligados diretamente (frequentemente através de uma ligação WAN). No entanto, quando estiver a configurar eBGP através de túneis IPsec VPN através do peering da Microsoft do ExpressRoute, existem vários domínios de encaminhamento entre os elementos de eBGP. Utilize o **ebgp multihop** comando para estabelecer a relação de vizinho de eBGP entre os dois não-diretamente ligado elementos. O número inteiro que se segue ebgp multihop comando Especifica o valor TTL nos pacotes BGP. O comando **máximo caminhos eibgp 2** permite o balanceamento de carga do tráfego entre os dois caminhos BGP.

### <a name="cisco1"></a>Exemplo de CSR1000 Cisco

O exemplo seguinte mostra a configuração para CSR1000 da Cisco numa máquina virtual de Hyper-V, como o dispositivo VPN no local:

```
!
crypto ikev2 proposal az-PROPOSAL
 encryption aes-cbc-256 aes-cbc-128 3des
 integrity sha1
 group 2
!
crypto ikev2 policy az-POLICY
 proposal az-PROPOSAL
!
crypto ikev2 keyring key-peer1
 peer azvpn1
  address 52.175.253.112
  pre-shared-key secret*1234
 !
!
crypto ikev2 keyring key-peer2
 peer azvpn2
  address 52.175.250.191
  pre-shared-key secret*1234
 !
!
!
crypto ikev2 profile az-PROFILE1
 match address local interface GigabitEthernet1
 match identity remote address 52.175.253.112 255.255.255.255
 authentication remote pre-share
 authentication local pre-share
 keyring local key-peer1
!
crypto ikev2 profile az-PROFILE2
 match address local interface GigabitEthernet1
 match identity remote address 52.175.250.191 255.255.255.255
 authentication remote pre-share
 authentication local pre-share
 keyring local key-peer2
!
crypto ikev2 dpd 10 2 on-demand
!
!
crypto ipsec transform-set az-IPSEC-PROPOSAL-SET esp-aes 256 esp-sha-hmac
 mode tunnel
!
crypto ipsec profile az-VTI1
 set transform-set az-IPSEC-PROPOSAL-SET
 set ikev2-profile az-PROFILE1
!
crypto ipsec profile az-VTI2
 set transform-set az-IPSEC-PROPOSAL-SET
 set ikev2-profile az-PROFILE2
!
!
interface Loopback0
 ip address 172.16.0.3 255.255.255.255
!
interface Tunnel0
 ip address 172.16.0.1 255.255.255.255
 ip tcp adjust-mss 1350
 tunnel source GigabitEthernet1
 tunnel mode ipsec ipv4
 tunnel destination 52.175.253.112
 tunnel protection ipsec profile az-VTI1
!
interface Tunnel1
 ip address 172.16.0.2 255.255.255.255
 ip tcp adjust-mss 1350
 tunnel source GigabitEthernet1
 tunnel mode ipsec ipv4
 tunnel destination 52.175.250.191
 tunnel protection ipsec profile az-VTI2
!
interface GigabitEthernet1
 description External interface
 ip address x.243.229.110 255.255.255.252
 negotiation auto
 no mop enabled
 no mop sysid
!
interface GigabitEthernet2
 ip address 10.0.0.1 255.255.255.0
 negotiation auto
 no mop enabled
 no mop sysid
!
router bgp 65010
 bgp router-id interface Loopback0
 bgp log-neighbor-changes
 network 10.0.0.0 mask 255.255.255.0
 network 10.1.10.0 mask 255.255.255.128
 neighbor 10.2.0.228 remote-as 65000
 neighbor 10.2.0.228 ebgp-multihop 5
 neighbor 10.2.0.228 update-source Loopback0
 neighbor 10.2.0.228 soft-reconfiguration inbound
 neighbor 10.2.0.228 filter-list 10 out
 neighbor 10.2.0.229 remote-as 65000    
 neighbor 10.2.0.229 ebgp-multihop 5
 neighbor 10.2.0.229 update-source Loopback0
 neighbor 10.2.0.229 soft-reconfiguration inbound
 maximum-paths eibgp 2
!
ip route 0.0.0.0 0.0.0.0 10.1.10.1
ip route 10.2.0.228 255.255.255.255 Tunnel0
ip route 10.2.0.229 255.255.255.255 Tunnel1
!
```

## <a name="firewalls"></a>5. Configurar a filtragem de dispositivos VPN e as firewalls (opcionais)

Configure a firewall e a filtragem de acordo com os requisitos.

## <a name="testipsec"></a>6. Testar e validar o túnel IPsec

O estado de túneis IPsec pode ser verificado no gateway de VPN do Azure, os comandos do Powershell:

```powershell
Get-AzureRmVirtualNetworkGatewayConnection -Name vpn2local1 -ResourceGroupName myRG | Select-Object  ConnectionStatus,EgressBytesTransferred,IngressBytesTransferred | fl
```

Exemplo de saída:

```powershell
ConnectionStatus        : Connected
EgressBytesTransferred  : 17734660
IngressBytesTransferred : 10538211
```

Para verificar o estado dos túneis nas instâncias do gateway de VPN do Azure de forma independente, utilize o seguinte exemplo:

```powershell
Get-AzureRmVirtualNetworkGatewayConnection -Name vpn2local1 -ResourceGroupName myRG | Select-Object -ExpandProperty TunnelConnectionStatus
```

Exemplo de saída:

```powershell
Tunnel                           : vpn2local1_52.175.250.191
ConnectionStatus                 : Connected
IngressBytesTransferred          : 4877438
EgressBytesTransferred           : 8754071
LastConnectionEstablishedUtcTime : 11/04/2017 17:03:30

Tunnel                           : vpn2local1_52.175.253.112
ConnectionStatus                 : Connected
IngressBytesTransferred          : 5660773
EgressBytesTransferred           : 8980589
LastConnectionEstablishedUtcTime : 11/04/2017 17:03:13
```

Também pode verificar o estado de túnel no dispositivo VPN no local.

Exemplo de CSR1000 Cisco:

```
show crypto session detail
show crypto ikev2 sa
show crypto ikev2 session detail
show crypto ipsec sa
```

Exemplo de saída:

```
csr1#show crypto session detail

Crypto session current status

Code: C - IKE Configuration mode, D - Dead Peer Detection
K - Keepalives, N - NAT-traversal, T - cTCP encapsulation
X - IKE Extended Authentication, F - IKE Fragmentation
R - IKE Auto Reconnect

Interface: Tunnel1
Profile: az-PROFILE2
Uptime: 00:52:46
Session status: UP-ACTIVE
Peer: 52.175.250.191 port 4500 fvrf: (none) ivrf: (none)
      Phase1_id: 52.175.250.191
      Desc: (none)
  Session ID: 3
  IKEv2 SA: local 10.1.10.50/4500 remote 52.175.250.191/4500 Active
          Capabilities:DN connid:3 lifetime:23:07:14
  IPSEC FLOW: permit ip 0.0.0.0/0.0.0.0 0.0.0.0/0.0.0.0
        Active SAs: 2, origin: crypto map
        Inbound:  #pkts dec'ed 279 drop 0 life (KB/Sec) 4607976/433
        Outbound: #pkts enc'ed 164 drop 0 life (KB/Sec) 4607992/433

Interface: Tunnel0
Profile: az-PROFILE1
Uptime: 00:52:43
Session status: UP-ACTIVE
Peer: 52.175.253.112 port 4500 fvrf: (none) ivrf: (none)
      Phase1_id: 52.175.253.112
      Desc: (none)
  Session ID: 2
  IKEv2 SA: local 10.1.10.50/4500 remote 52.175.253.112/4500 Active
          Capabilities:DN connid:2 lifetime:23:07:17
  IPSEC FLOW: permit ip 0.0.0.0/0.0.0.0 0.0.0.0/0.0.0.0
        Active SAs: 2, origin: crypto map
        Inbound:  #pkts dec'ed 668 drop 0 life (KB/Sec) 4607926/437
        Outbound: #pkts enc'ed 477 drop 0 life (KB/Sec) 4607953/437
```

O protocolo de linha na Interface de túnel Virtual (VTI) não é alterado para "até" até IKE fase 2 foi concluída. O seguinte comando verifica a associação de segurança:

```
csr1#show crypto ikev2 sa

IPv4 Crypto IKEv2  SA

Tunnel-id Local                 Remote                fvrf/ivrf            Status
2         10.1.10.50/4500       52.175.253.112/4500   none/none            READY
      Encr: AES-CBC, keysize: 256, PRF: SHA1, Hash: SHA96, DH Grp:2, Auth sign: PSK, Auth verify: PSK
      Life/Active Time: 86400/3277 sec

Tunnel-id Local                 Remote                fvrf/ivrf            Status
3         10.1.10.50/4500       52.175.250.191/4500   none/none            READY
      Encr: AES-CBC, keysize: 256, PRF: SHA1, Hash: SHA96, DH Grp:2, Auth sign: PSK, Auth verify: PSK
      Life/Active Time: 86400/3280 sec

IPv6 Crypto IKEv2  SA

csr1#show crypto ipsec sa | inc encaps|decaps
    #pkts encaps: 177, #pkts encrypt: 177, #pkts digest: 177
    #pkts decaps: 296, #pkts decrypt: 296, #pkts verify: 296
    #pkts encaps: 554, #pkts encrypt: 554, #pkts digest: 554
    #pkts decaps: 746, #pkts decrypt: 746, #pkts verify: 746
```

### <a name="verifye2e"></a>Verifique a conetividade de ponto a ponto entre interior de rede no local e a VNet do Azure

Se os túneis IPsec estão operacionais e que as rotas estáticas estão definidas corretamente, deverá conseguir enviar um ping o endereço IP do elemento BGP remoto de:

```
csr1#ping 10.2.0.228
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.2.0.228, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 5/5/5 ms

#ping 10.2.0.229
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.2.0.229, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 4/5/6 ms
```

### <a name="verifybgp"></a>Verifique as sessões de BGP através de IPsec

No gateway de VPN do Azure, verifique o estado do elemento BGP:

```powershell
Get-AzureRmVirtualNetworkGatewayBGPPeerStatus -VirtualNetworkGatewayName vpnGtw -ResourceGroupName SEA-C1-VPN-ER | ft
```

Exemplo de saída:

```powershell
  Asn ConnectedDuration LocalAddress MessagesReceived MessagesSent Neighbor    RoutesReceived State    
  --- ----------------- ------------ ---------------- ------------ --------    -------------- -----    
65010 00:57:19.9003584  10.2.0.228               68           72   172.16.0.10              2 Connected
65000                   10.2.0.228                0            0   10.2.0.228               0 Unknown  
65000 07:13:51.0109601  10.2.0.228              507          500   10.2.0.229               6 Connected
```

Para verificar a lista de prefixos de rede recebidos através de eBGP a VPN concentrador no local, pode filtrar pela "Origem" do atributo:

```powershell
Get-AzureRmVirtualNetworkGatewayLearnedRoute -VirtualNetworkGatewayName vpnGtw -ResourceGroupName myRG  | Where-Object Origin -eq "EBgp" |ft
```

O resultado de exemplo, o ASN 65010 é o número de sistema autónomo BGP na VPN no local.

```powershell
AsPath LocalAddress Network      NextHop     Origin SourcePeer  Weight
------ ------------ -------      -------     ------ ----------  ------
65010  10.2.0.228   10.1.10.0/25 172.16.0.10 EBgp   172.16.0.10  32768
65010  10.2.0.228   10.0.0.0/24  172.16.0.10 EBgp   172.16.0.10  32768
```

Para ver a lista de rotas anunciadas:

```powershell
Get-AzureRmVirtualNetworkGatewayAdvertisedRoute -VirtualNetworkGatewayName vpnGtw -ResourceGroupName myRG -Peer 10.2.0.228 | ft
```

Exemplo de saída:

```powershell
AsPath LocalAddress Network        NextHop    Origin SourcePeer Weight
------ ------------ -------        -------    ------ ---------- ------
       10.2.0.229   10.2.0.0/24    10.2.0.229 Igp                  0
       10.2.0.229   172.16.0.10/32 10.2.0.229 Igp                  0
       10.2.0.229   172.16.0.5/32  10.2.0.229 Igp                  0
       10.2.0.229   172.16.0.1/32  10.2.0.229 Igp                  0
65010  10.2.0.229   10.1.10.0/25   10.2.0.229 Igp                  0
65010  10.2.0.229   10.0.0.0/24    10.2.0.229 Igp                  0
```

Exemplo CSR1000 de Cisco no local:

```
csr1#show ip bgp neighbors 10.2.0.228 routes
BGP table version is 7, local router ID is 172.16.0.10
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
              t secondary path,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>   10.2.0.0/24      10.2.0.228                             0 65000 i
 r>   172.16.0.1/32    10.2.0.228                             0 65000 i
 r>   172.16.0.2/32    10.2.0.228                             0 65000 i
 r>   172.16.0.3/32   10.2.0.228                             0 65000 i

Total number of prefixes 4
```

A lista de redes anunciado a partir do CSR1000 de Cisco no local para o gateway de VPN do Azure pode ser apresentada utilizando o seguinte comando:

```powershell
csr1#show ip bgp neighbors 10.2.0.228 advertised-routes
BGP table version is 7, local router ID is 172.16.0.10
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
              t secondary path,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>   10.0.0.0/24      0.0.0.0                  0         32768 i
 *>   10.1.10.0/25     0.0.0.0                  0         32768 i

Total number of prefixes 2
```

## <a name="next-steps"></a>Passos seguintes

* [Configurar o Monitor de Desempenho de Rede para o ExpressRoute](how-to-npm.md)

* [Adicionar uma ligação site a site para uma VNet com uma ligação de gateway VPN existente](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)