---
title: "As definições do gateway de VPN para a pilha do Azure | Microsoft Docs"
description: "Saiba mais sobre as definições para gateways de VPN que utilizar com a pilha do Azure."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: fa8d3adc-8f5a-4b4f-8227-4381cf952c56
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/18/2018
ms.author: brenduns
ms.openlocfilehash: 1eba5df93b461eb22ab8341b4498682957c9298a
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="vpn-gateway-configuration-settings-for-azure-stack"></a>Definições de configuração do gateway VPN do Azure pilha

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Um gateway de VPN é um tipo de gateway de rede virtual que envia o tráfego encriptado entre a rede virtual na pilha do Azure e um gateway VPN remoto. O gateway de VPN remoto pode ser no Azure, um dispositivo no seu centro de dados ou um dispositivo no outro site.  Se existir conectividade de rede entre os dois pontos finais, pode estabelecer uma ligação de VPN de Site a Site (S2S) segura entre as duas redes.

Uma ligação de gateway VPN baseia-se na configuração de vários recursos, cada um dos quais contém as definições configuráveis. As secções neste artigo abordam os recursos e as definições relacionadas com um gateway de VPN para uma rede virtual criado no modelo de implementação do Resource Manager. Pode encontrar descrições e diagramas de topologia para cada solução de ligação no [sobre o Gateway de VPN para Azure pilha](azure-stack-vpn-gateway-about-vpn-gateways.md).

## <a name="vpn-gateway-settings"></a>Definições do gateway de VPN

### <a name="gateway-types"></a>Tipos de gateway
Cada rede virtual do Azure pilha suporta um gateway de rede virtual único, que têm de ser do tipo **Vpn**.  Este suporte difere do Azure, o que suporta os tipos de adicionais.  

Quando estiver a criar um gateway de rede virtual, tem de se certificar de que o tipo de gateway está correto para a sua configuração. Necessita de um gateway de VPN a `-GatewayType Vpn`.

Exemplo:
```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn
-VpnType RouteBased
```

### <a name="gateway-skus"></a>SKUs de gateway
Quando cria um gateway de rede virtual, tem de especificar o SKU de gateway que pretende utilizar. Selecione os SKU que atendem as suas necessidades com base nos tipos de cargas de trabalho, taxas de transferência, funcionalidades e SLA.

>[!NOTE]
> As redes virtuais clássicas devem continuar a usar os SKU antigos. Para obter mais informações sobre os SKU antigos do gateway, consulte [Trabalhar com os SKU do gateway de rede virtual (antigos)](/azure/vpn-gateway/vpn-gateway-about-skus-legacy).

Pilha do Azure oferece os seguintes SKUs de gateway VPN:

|   | Débito do VPN Gateway |Gateway de VPN máximo de túneis IPsec |
|-------|-------|-------|
|**SKU básico**  | 100 Mbps  | 10    |
|**Standard SKU**           | 100 Mbps  | 10    |
|**SKU de elevado desempenho** | 200 Mbps    | 30    |

### <a name="resizing-gateway-skus"></a>O redimensionamento de SKUs de gateway
Pilha do Azure não suporta um redimensionamento de SKUs entre os SKUs legados suportados.

Da mesma forma, a pilha do Azure não suporta um redimensionamento de uma SKUs legados suportados (básico, Standard e HighPerformance), para os SKUs mais recentes suportado pelo Azure (VpnGw1, VpnGw2 e VpnGw3).

### <a name="configure-the-gateway-sku"></a>Configurar o SKU de gateway
#### <a name="azure-stack-portal"></a>Portal de pilha do Azure
Se utilizar o portal de pilha do Azure para criar um gateway de rede virtual do Gestor de recursos, pode selecionar o SKU de gateway com a lista pendente. As opções que são apresentadas com correspondem para o tipo de Gateway e o tipo VPN que selecionar.

#### <a name="powershell"></a>PowerShell
O exemplo do PowerShell seguinte especifica o GatewaySku - como VpnGw1.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig -GatewaySku VpnGw1
-GatewayType Vpn -VpnType RouteBased
```
### <a name="connection-types"></a>Tipos de ligação
No modelo de implementação Resource Manager, cada configuração exige um tipo de ligação de gateway de rede virtual específico. Os valores disponíveis do PowerShell do Resource Manager para - ConnectionType são:

- IPsec

No exemplo do PowerShell seguinte, é criada uma ligação S2S que requer o tipo de ligação IPsec.  

```PowerShell
New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg
-Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local
-ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'
```

### <a name="vpn-types"></a>Tipos de VPN
Quando criar o gateway de rede virtual para uma configuração de gateway VPN, tem de especificar um tipo de VPN. O tipo de VPN que escolher depende da topologia de ligação que pretende criar.  Também pode dependem de um tipo de VPN no hardware que está a utilizar. Configurações de S2S requerem um dispositivo VPN. Alguns dispositivos VPN só suportam um determinado tipo VPN.

> [!IMPORTANT]  
> Neste momento, a pilha do Azure só suporta o tipo de VPN de baseada na rota. Se o dispositivo suporta apenas as VPNs baseadas de política, ligações a esses dispositivos do Azure pilha não são suportadas.

- **PolicyBased**: *(suportado pelo Azure, mas não através da pilha do Azure)* VPNs baseadas em políticas encriptam e direcionam os pacotes através de túneis IPsec com as políticas de IPsec que estão configuradas com as combinações de prefixos de endereços entre a rede no local e a VNet de pilha do Azure. Normalmente, a política (ou o seletor de tráfego), é definido como uma lista de acesso na configuração do dispositivo VPN.

- **RouteBased**: RouteBased VPNs utilizam "rotas" no reencaminhamento IP ou na tabela de encaminhamento para direcionar os pacotes para as respetivas interfaces de túnel. As interfaces de túnel, em seguida, encriptam ou desencriptam os pacotes dentro e fora dos túneis. A política (ou o Seletor de tráfego) para as VPNs de RouteBased estão configuradas como qualquer a qualquer (ou carateres universais). O valor para um tipo de RouteBased VPN é RouteBased.

O exemplo do PowerShell seguinte especifica o - VpnType como RouteBased. Quando estiver a criar um gateway, tem de confirmar se o -VpnType está correto para a sua configuração.

```PowerShell
New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
-Location 'West US' -IpConfigurations $gwipconfig
-GatewayType Vpn -VpnType RouteBased
```
### <a name="gateway-requirements"></a>Requisitos do gateway
A tabela seguinte lista os requisitos para gateways de VPN.

| |Gateway de VPN PolicyBased básico | Gateway de VPN RouteBased básico | Gateway de VPN RouteBased padrão | Gateway de VPN RouteBased elevado desempenho|
|--|--|--|--|--|
| **Conetividade site a Site (conetividade S2S)** | Não suportado | Configuração de RouteBased VPN | Configuração de RouteBased VPN | Configuração de RouteBased VPN |
| **Método de autenticação**  | Não suportado | Chave pré-partilhada para a conetividade S2S  | Chave pré-partilhada para a conetividade S2S  | Chave pré-partilhada para a conetividade S2S  |   
| **Número máximo de ligações S2S**  | Não suportado | 10 | 10| 30|
|**Suporte de encaminhamento ativo (BGP)** | Não suportado | Não suportado | Suportadas | Suportadas |

### <a name="gateway-subnet"></a>Sub-rede de gateway
Antes de criar um gateway de VPN, terá de criar uma sub-rede de gateway. A sub-rede do gateway contém os endereços IP que utilizam os serviços e VMs de gateway de rede virtual. Ao criar o gateway de rede virtual, VMs de gateway são implementadas para a sub-rede do gateway e configuradas com as definições do gateway VPN necessárias. Não implemente há mais alguma coisa (por exemplo, VMs adicionais) para a sub-rede do gateway. A sub-rede do gateway tem de ser o nome "GatewaySubnet" para funcionar corretamente. A sub-rede do gateway de atribuição de nome "GatewaySubnet" permite que a pilha do Azure para identificar a sub-rede para implementar as VMs de gateway de rede virtual e os serviços.

Quando cria a sub-rede do gateway, especifica o número de endereços IP que a sub-rede contém. Os endereços IP na sub-rede de gateway são atribuídos às VMs de gateway e os serviços de gateway. Algumas configurações requerem mais endereços IP do que outras. Observe as instruções para a configuração que pretende criar e certifique-se de que a sub-rede do gateway que pretende criar cumpre os requisitos. Além disso, pode querer Certifique-se que à sub-rede do gateway contém endereços IP suficientes para acomodar possíveis configurações adicionais futuras. Embora seja possível criar uma sub-rede do gateway tão pequena como/29, recomendamos que crie uma sub-rede de gateway de/28 ou superior (/ 28, / 27, /26 etc.). Dessa forma, se adicionar a funcionalidade no futuro, não dispõe que fechar as suas gateway, em seguida, elimine e recrie a sub-rede do gateway para permitir mais endereços IP.

O exemplo de Gestor de recursos do PowerShell seguinte mostra uma sub-rede de gateway com o nome GatewaySubnet. Pode ver que a notação CIDR Especifica/27, que permite obter endereços IP suficientes para a maioria das configurações de que existe atualmente.

```PowerShell
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27
```

> [!IMPORTANT]
> Ao trabalhar com sub-redes de gateway, evite associar um grupo de segurança de rede (NSG) às mesmas. A associação de um grupo de segurança de rede a esta sub-rede poderá fazer com que o gateway de VPN deixe de funcionar conforme esperado. Para obter mais informações sobre grupos de segurança de rede, consulte [que é um grupo de segurança de rede?](/azure/virtual-network/virtual-networks-nsg).

### <a name="local-network-gateways"></a>Gateways de rede local
Ao criar uma configuração de gateway VPN no Azure, o gateway de rede local representa com frequência a sua localização no local. Na pilha do Azure, representa todos os dispositivos remotos de VPN que se encontra fora da pilha do Azure.  Isto pode ser um dispositivo VPN no seu centro de dados, um centro de dados remoto ou um Gateway de VPN no Azure.

Dê um nome, o endereço IP público do dispositivo VPN, de gateway de rede local e especificar os prefixos de endereços que estão na localização no local. Azure analisa os prefixos de endereço de destino para o tráfego de rede, consulta a configuração que especificou para o gateway de rede local e encaminha os pacotes em conformidade.

O exemplo do PowerShell seguinte cria um novo gateway de rede local:

```PowerShell
New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg
-Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'
```
Por vezes, terá de modificar as definições do gateway de rede local. Por exemplo, quando adiciona ou modifica o intervalo de endereços, ou se o endereço IP do dispositivo VPN é alterado. Consulte [modificar as definições do gateway de rede local com o PowerShell](/azure/vpn-gateway/vpn-gateway-modify-local-network-gateway).

## <a name="ipsecike-parameters"></a>Parâmetros IPsec/IKE
Quando configurar uma ligação VPN na pilha do Azure, terá de configurar a ligação em ambas as extremidades.  Se estiver a configurar uma ligação VPN entre a pilha do Azure e um dispositivo de hardware, como um comutador ou um router, que está a atuar como um Gateway de VPN, o que o dispositivo poderá pedir-lhe o definições adicionais.

Ao contrário do Azure, o que suporta vários ofertas como um iniciador e um dispositivo de resposta, a pilha do Azure suporta apenas uma oferta.

###  <a name="ike-phase-1-main-mode-parameters"></a>Parâmetros de IKE Fase 1 (Modo Principal)
| Propriedade              | Valor|
|-|-|
| Versão do IKE           | IKEv2 |
|Grupo Diffie-Hellman   | Grupo 2 (1024 bits) |
| Método de Autenticação | Chave Pré-partilhada |
|Algoritmos de Encriptação e Hash | AES256, SHA256 |
|Duração de SA (Tempo)     | 28 800 segundos|

### <a name="ike-phase-2-quick-mode-parameters"></a>Parâmetros de IKE Fase 2 (Modo Rápido)
| Propriedade| Valor|
|-|-|
|Versão do IKE |IKEv2 |
|Encriptação e hash algoritmos (encriptação)     | GCMAES256|
|Encriptação e hash algoritmos (autenticação) | GCMAES256|
|Duração de SA (Tempo)  | segundos 14,400 |
|Duração de SA (Bytes) | 819,200       |
|Perfect Forward Secrecy (PFS) |PFS2048 |
|Deteção de Elemento Inutilizado | Suportadas|  
