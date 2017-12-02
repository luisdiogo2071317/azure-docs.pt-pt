---
title: Acerca do VPN gateway para a pilha do Azure | Microsoft Docs
description: Saiba mais sobre e configure os gateways de VPN que utilizar com a pilha do Azure.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/01/2017
ms.author: brenduns
ms.openlocfilehash: ba9642d8c51f57623aded44b84d7127334806bc1
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/02/2017
---
# <a name="about-vpn-gateway-for-azure-stack"></a>Acerca do VPN gateway para a pilha do Azure
*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*


Antes de poder enviar o tráfego de rede entre a rede virtual do Azure e o seu site no local, tem de criar um gateway de rede virtual para a rede virtual.

Um gateway de VPN é um tipo de gateway de rede virtual que envia o tráfego encriptado através de uma ligação pública. Pode utilizar gateways de VPN para enviar o tráfego de forma segura entre uma rede virtual na pilha do Azure e uma rede virtual no Azure ou entre uma rede virtual e a outra rede que está ligada a um dispositivo VPN.

Quando cria um gateway de rede virtual, tem de especificar o tipo de gateway que pretende criar. Pilha do Azure suporta um tipo de gateway de rede virtual: o tipo de 'Vpn'.

Cada rede virtual pode ter dois gateways de rede virtual, mas apenas um de cada tipo. Dependendo das definições que escolher, pode criar várias ligações para um gateway de VPN individual. Um exemplo disto é uma configuração de ligação multilocal.

> [!NOTE]
> No Azure, o débito de largura de banda para o SKU de Gateway de VPN que escolher tem de ser dividido por todas as ligações que estão ligadas à mesma.  Na pilha do Azure, o valor de largura de banda para o SKU de Gateway de VPN é aplicado a cada recurso de ligação que está ligado ao mesmo.     

> Por exemplo, no Azure, o SKU de Gateway de VPN básico pode acomodar aproximadamente 100 Mbps de débito agregado.  Se criar duas ligações para este Gateway de VPN e uma ligação está a utilizar 50 Mbps da largura de banda, em seguida, 50 Mbps está disponível para a outra ligação.   

> Na pilha do Azure, *cada* ligação para o SKU de Gateway de VPN básico obtém alocada 100 Mbps de débito.

## <a name="configuring-a-vpn-gateway"></a>Configurar um Gateway de VPN
Uma ligação de gateway de VPN depende de vários recursos que estão configurados com definições específicas. A maior parte dos recursos podem ser configurados em separado, embora estes, em alguns casos, devam ser configurados numa determinada ordem.

### <a name="settings"></a>Definições
As definições que escolheu para cada recurso são essenciais para a criação de uma ligação com êxito. Para obter informações sobre recursos individuais e as definições do Gateway de VPN, consulte [definições do gateway de VPN sobre para Azure pilha](azure-stack-vpn-gateway-settings.md). Pode encontrar informações para ajudar a compreender os tipos de gateway, tipos de VPN, tipos de ligação, sub-redes de gateway, gateways de rede local e várias outras definições de recursos que pretende considerar.

### <a name="deployment-tools"></a>Ferramentas de implementação
Pode começar por criar e configurar recursos utilizando uma ferramenta de configuração, como o portal do Azure. Mais tarde, pode decidir mudar para outra ferramenta, como o PowerShell, para configurar recursos adicionais ou modificar os recursos existentes, quando aplicável. Atualmente, não pode configurar todos os recursos e definições de recursos no portal do Azure. As instruções nos artigos para cada topologia de ligação especificam quando uma ferramenta de configuração especifica é necessária.

## <a name="connection-topology-diagrams"></a>Diagramas de topologia de ligação
É importante saber que existem configurações diferentes disponíveis para as ligações de gateway VPN. Deve determinar qual das configurações se adequa melhor às suas necessidades. Nas secções abaixo, pode ver informações e diagramas de topologias relativos às ligações de gateways de VPN seguintes. As secções seguintes contêm tabelas que listam:

- O modelo de implementação disponível
- As ferramentas de configuração disponíveis
- Ligações que o levam diretamente para um artigo, se disponível

Os diagramas e as descrições das secções seguintes podem ajudar a selecionar uma topologia de ligação para corresponder aos seus requisitos. Os diagramas mostram as principais topologias de linha de base, mas é possível criar configurações mais complexas utilizando os diagramas como orientação.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Rede Site a Site e Multilocal (túnel VPN IPsec/IKE)
### <a name="site-to-site"></a>Site a Site
Uma ligação de gateway de VP de Site a Site (S2S) é uma ligação através do túnel VPN IPsec/IKE (S2S ou IKEv1). Este tipo de ligação requer um dispositivo VPN localizado no local que tenha um endereço IP público atribuído ao mesmo e não se encontre protegido por um NAT. As ligações S2S podem ser utilizadas para configurações em vários locais e híbridas.    
![Exemplo de configuração da ligação de VPN de site a site](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Multilocal
Este tipo de ligação é uma variação da ligação Rede de VPNs. Cria mais de uma ligação de VPN a partir do gateway de rede virtual, ligando, geralmente, a vários sites no local. Quando trabalha com várias ligações, tem de utilizar um tipo de VPN RouteBased (conhecido como gateway dinâmico ao trabalhar com VNets clássicas). Uma vez que cada rede virtual só pode ter um gateway de VPN, todas as ligações através do mesmo partilham a largura de banda disponível. Isto é frequentemente designado como ligação “multilocal”.   
![Exemplo de ligação Multilocal de Gateway de VPN do Azure](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)



## <a name="gateway-skus"></a>SKUs de gateway
Quando cria um gateway de rede virtual para a pilha do Azure, especificar o SKU de gateway que pretende utilizar. São suportados os seguintes SKUs do VPN gateway:
- Básica
- Standard
- HighPerformance

Quando seleciona um gateway superior SKU, como padrão sobre o Basic ou HighPerformance através de padrão ou Basic, mais CPUs e largura de banda de rede atribuídos ao gateway. Como resultado, o gateway pode suportar um maior débito de rede para a rede virtual.

Pilha do Azure não suporta o gateway de UltraPerformance SKU, que é utilizada exclusivamente com o Expressroute.

Quando selecionar um SKU, considere o seguinte:
- Pilha do Azure não suporta gateways baseado na política.
- Border Gateway Protocol (BGP) não é suportada no SKU básico.
- Gateway de VPN do ExpressRoute coexistentes configurações não são suportadas na pilha do Azure
- As ligações do Gateway de VPN S2S do modo ativo/ativo podem ser configuradas apenas no SKU HighPerformance.

## <a name="estimated-aggregate-throughput-by-sku"></a>Débito agregado estimado por SKU
A tabela seguinte mostra os tipos de gateways e o débito agregado estimado pelo SKU do gateway.

|   | Débito do VPN Gateway *(1)* |Gateway de VPN máximo de túneis IPsec |
|-------|-------|-------|
|**SKU básico** ***(2)***    | 100 Mbps  | 10    |
|**Standard SKU**       | 100 Mbps  | 10    |
|**SKU de elevado desempenho** | 200 Mbps    | 30    |
***(1)***  Débito do VPN não é um débito garantido para ligações em vários locais através da Internet. É a medida de débito mais alta possível.  
***(2)***  BGP não é suportado para o SKU básico.

## <a name="next-steps"></a>Passos seguintes
Saiba mais sobre [definições para gateways de VPN](azure-stack-vpn-gateway-settings.md) para pilha do Azure.
