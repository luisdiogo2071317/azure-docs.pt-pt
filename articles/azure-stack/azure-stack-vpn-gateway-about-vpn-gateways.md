---
title: Sobre o gateway VPN para o Azure Stack | Documentos da Microsoft
description: Saiba mais sobre o e configure gateways de VPN que utiliza com o Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 0e30522f-20d6-4da7-87d3-28ca3567a890
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/02/2018
ms.author: sethm
ms.openlocfilehash: 860f1181a84ef87fe0e66c2746b76b800a326354
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496204"
---
# <a name="about-vpn-gateway-for-azure-stack"></a>Sobre o gateway VPN para o Azure Stack

*Aplica-se a: integrados do Azure Stack, sistemas e o Kit de desenvolvimento do Azure Stack*

Antes de pode enviar o tráfego de rede entre a rede virtual do Azure e o seu site no local, tem de criar um gateway de rede virtual para a rede virtual.

Um gateway de VPN é um tipo de gateway de rede virtual que envia o tráfego encriptado através de uma ligação pública. Pode utilizar gateways de VPN para enviar tráfego de forma segura entre uma rede virtual no Azure Stack e uma rede virtual no Azure. Também pode enviar o tráfego de forma segura entre uma rede virtual e a outra rede que está ligada a um dispositivo VPN.

Quando cria um gateway de rede virtual, tem de especificar o tipo de gateway que pretende criar. O Azure Stack oferece suporte a um tipo de gateway de rede virtual, o **Vpn** tipo.

Cada rede virtual pode ter dois gateways de rede virtual, mas apenas um de cada tipo. Dependendo das definições que escolher, pode criar várias ligações para um gateway de VPN individual. Um exemplo é uma configuração de ligação multilocal.

Antes de criar e configurar Gateways de VPN para o Azure Stack, reveja os [considerações sobre o funcionamento em rede do Azure Stack](user/azure-stack-network-differences.md) para saber como as configurações para o Azure Stack são diferentes do Azure.

>[!NOTE]
>No Azure, o débito de largura de banda para o gateway de VPN SKU que escolher tem de ser dividido em todas as ligações que estão ligadas ao gateway. Mas, no Azure Stack, o valor de largura de banda para o SKU de gateway VPN é aplicado a cada recurso de ligação que está ligado ao gateway.
>
> Por exemplo:
> * No Azure, o SKU de Gateway de VPN básico capaz de acomodar aproximadamente 100 Mbps de débito agregado. Se criar duas ligações para esse Gateway de VPN e uma ligação está a utilizar 50 Mbps da largura de banda, em seguida, 50 Mbps está disponível para a ligação a outra.
> * No Azure Stack, *cada* ligação para o SKU de Gateway de VPN básico é alocada 100 Mbps de débito.

## <a name="configuring-a-vpn-gateway"></a>Configurar um Gateway de VPN

Uma ligação de gateway VPN depende de vários recursos que estão configurados com definições específicas. A maioria desses recursos pode ser configurada em separado, mas em alguns casos têm de ser configuradas por uma ordem específica.

### <a name="settings"></a>Definições

As definições que escolheu para cada recurso são essenciais para a criação de uma ligação com êxito.

Para obter informações sobre recursos individuais e as definições para um gateway de VPN, consulte [as definições do gateway de VPN sobre para o Azure Stack](azure-stack-vpn-gateway-settings.md). Este artigo ajuda-o a compreender:

* Tipos de gateway, tipos VPN e tipos de ligação.
* As sub-redes de gateway, gateways de rede local e outras definições de recursos que poderá querer considerar.

### <a name="deployment-tools"></a>Ferramentas de implementação

Pode criar e configurar recursos usando uma ferramenta de configuração, como o portal do Azure. Pode alternar posteriormente para outra ferramenta como o PowerShell para configurar recursos adicionais ou modificar os recursos existentes, quando aplicável. Atualmente, não pode configurar todos os recursos e definições de recursos no portal do Azure. As instruções nos artigos para cada topologia de ligação especificam quando uma ferramenta de configuração especifica é necessária.

## <a name="connection-topology-diagrams"></a>Diagramas de topologia de ligação

É importante saber que existem configurações diferentes disponíveis para as ligações de gateway VPN. Determine quais configurações melhor se adequa às suas necessidades. Nas seções a seguir, pode ver diagramas de topologia e informações sobre as seguintes ligações de gateway VPN:

* O modelo de implementação disponível
* As ferramentas de configuração disponíveis
* Ligações que o levam diretamente para um artigo, se disponível

Os diagramas e as descrições das secções seguintes podem ajudá-lo a selecionar uma topologia de ligação para corresponder aos requisitos. Os diagramas mostram as topologias de linha de base principal, mas é possível criar configurações mais complexas utilizando os diagramas como guia.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Rede Site a Site e Multilocal (túnel VPN IPsec/IKE)

### <a name="site-to-site"></a>Site a Site

Uma ligação de gateway de VP de Site a Site (S2S) é uma ligação através do túnel VPN IPsec/IKE (S2S ou IKEv1). Este tipo de ligação requer um dispositivo VPN que encontrarem no local e é atribuído um endereço IP público. Este dispositivo não pode estar localizado atrás de um NAT. As ligações S2S podem ser utilizadas para configurações em vários locais e híbridas.

![Exemplo de configuração de ligação de VPN de site a site](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="multi-site"></a>Multilocal

Este tipo de ligação é uma variação da ligação Rede de VPNs. Cria mais de uma ligação de VPN a partir do gateway de rede virtual, ligando, geralmente, a vários sites no local. Quando trabalha com várias ligações, tem de utilizar um tipo de RouteBased VPN (conhecido como gateway dinâmico ao trabalhar com VNets clássicas.) Uma vez que cada rede virtual só pode ter um gateway de VPN, todas as ligações através do mesmo partilham a largura de banda disponível. Isto é frequentemente designado como ligação “multilocal”.

![Exemplo de ligação Multilocal de Gateway de VPN do Azure](media/azure-stack-vpn-gateway-about-vpn-gateways/vpngateway-multisite-connection-diagram.png)

## <a name="gateway-skus"></a>SKUs de gateway

Quando cria um gateway de rede virtual para o Azure Stack, especificar o SKU de gateway que pretende utilizar. Os SKUs de gateway VPN seguintes são suportados:

* Básica
* Standard
* HighPerformance

Quando seleciona um SKU, como o padrão ao longo do Basic ou HighPerformance de gateway superior ao longo do Standard ou Basic, mais CPUs e largura de banda de rede são alocados para o gateway. Como resultado, o gateway pode suportar um maior débito de rede para a rede virtual.

O Azure Stack não suporta o gateway de UltraPerformance SKU, que é utilizado exclusivamente com o Express Route.

Quando selecionar o SKU, considere o seguinte:

* O Azure Stack não suporta os gateways baseados na política.
* Protocolo BGP (Border Gateway) não é suportada no SKU básico.
* Gateway de VPN do ExpressRoute coexistir configurações não são suportadas no Azure Stack.
* As ligações do Gateway de VPN S2S do modo ativo/ativo podem ser configuradas apenas no SKU HighPerformance.

## <a name="estimated-aggregate-throughput-by-sku"></a>Débito agregado estimado por SKU

A tabela seguinte mostra os tipos de gateways e o débito agregado estimado pelo SKU do gateway.

|   | Débito do Gateway de VPN *(1)* | Gateway de VPN máximo de túneis IPsec *(2)* |
|-------|-------|-------|
|**SKU básico** ***(3)***    | 100 Mbps  | 10    |
|**Standard SKU**       | 100 Mbps  | 10    |
|**SKU de elevado desempenho** | 200 Mbps    | 5 |

**Notas de tabela:**

*Tenha em atenção (1)* -débito de VPN não é um débito garantido para ligações entre locais através da Internet. É a medida de débito mais alta possível.  
*Tenha em atenção (2)* -máximo de túneis é o total por implementação do Azure Stack para todas as subscrições.  
*Tenha em atenção (3)* -encaminhamento de BGP não é suportado para o SKU básico.

>[!NOTE]
>Apenas uma ligação de VPN de Site a Site pode ser criada entre duas implementações do Azure Stack.  Isso é devido a uma limitação na plataforma que permite apenas uma única ligação de VPN para o mesmo endereço IP.  Uma vez que o Azure Stack aproveita o gateway de multi-inquilino que utiliza um IP público único para todos os Gateways de VPN no sistema do Azure Stack, pode haver apenas uma ligação VPN entre dois sistemas do Azure Stack.  Esta limitação aplica-se também à ligação de mais do que um ligações VPN de Site a Site a qualquer Gateway de VPN que utiliza um único endereço IP.  O Azure Stack não permitirá mais do que um recurso de Gateway de rede Local ser criado usando o mesmo endereço IP.


## <a name="next-steps"></a>Passos Seguintes

[Definições de configuração do gateway VPN para o Azure Stack](azure-stack-vpn-gateway-settings.md)
