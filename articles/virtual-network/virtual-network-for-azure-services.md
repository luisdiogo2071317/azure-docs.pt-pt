---
title: Rede virtual para serviços do Azure | Documentos da Microsoft
description: Saiba mais sobre os benefícios de implantar recursos numa rede virtual. Recursos em redes virtuais podem comunicar entre si e recursos no local, sem o tráfego que atravessa a Internet.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 3e31dbce7bd24b3c3bb0f24561464e6303f3908e
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990601"
---
# <a name="virtual-network-integration-for-azure-services"></a>Integração da rede virtual para serviços do Azure

Integração de serviços do Azure a uma rede virtual do Azure permite o acesso privado de instâncias de um serviço implementado na rede virtual.

Pode integrar os serviços do Azure com a rede virtual com as seguintes opções:
- Implementar diretamente instâncias dedicadas do serviço numa rede virtual. As instâncias dedicadas destes serviços podem ser acedidas em privado dentro da rede virtual e de redes no local.
- Ao expandir uma rede virtual para o serviço, através de pontos finais de serviço. Pontos finais de serviço permitem que os recursos de serviço individual a ser protegido para a rede virtual.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Implementar serviços do Azure em redes virtuais

Pode comunicar com os recursos mais do Azure através da Internet por meio de endereços IP públicos. Quando implementar os serviços do Azure numa [rede virtual](virtual-networks-overview.md), pode se comunicar com os recursos de serviço em privado, através de endereços IP privados.

![Serviços implementados numa rede virtual](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Implementação de serviços dentro de uma rede virtual fornece as seguintes capacidades:

- Recursos dentro da rede virtual podem comunicar entre si em privado, através de endereços IP privados. Exemplo, transferência de dados diretamente entre o HDInsight e o SQL Server em execução numa máquina virtual, na rede virtual.
- No local recursos podem aceder a recursos numa rede virtual com endereços IP privados através de um [VPN de Site a Site (Gateway de VPN)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) ou [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Redes virtuais podem estar [em modo de peering](virtual-network-peering-overview.md) para ativar a recursos em redes virtuais para comunicar entre si, através de IP privado de endereços.
- Instâncias de serviço numa rede virtual são totalmente geridas pelo serviço do Azure, para monitorizar o estado de funcionamento das instâncias e fornecer escala necessária, com base na carga.
- Instâncias de serviço são implementadas numa sub-rede dedicada na rede virtual. Entrada e acesso de rede de saída tem de ser aberto por meio [grupos de segurança de rede](security-overview.md#network-security-groups) para a sub-rede, com orientação fornecida pelos serviços.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Serviços que podem ser implementados numa rede virtual

Cada serviço implementado diretamente em rede virtual tem requisitos específicos para o encaminhamento e os tipos de tráfego que tem de ter permissão para dentro e fora de sub-redes. Para obter mais informações, consulte: 
 
- Máquinas virtuais: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ou [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [O Service fabric](../service-fabric/service-fabric-patterns-networking.md?toc=%2fazure%2fvirtual-network%2ftoc.json#existingvnet)
- [Conjuntos de dimensionamento de máquinas virtuais](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Ambiente do Serviço de Aplicações](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [RedisCache](../redis-cache/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Gestão de API](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Gateway de VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Gateway de aplicação (interno)](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Serviço Kubernetes do Azure (AKS)](../aks/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Motor do serviço de contentor do Azure](https://github.com/Azure/acs-engine) com o CNI de rede Virtual do Azure [Plug-in](https://github.com/Azure/acs-engine/tree/master/examples/vnet)
- [Serviços de domínio do Active Directory do Azure](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [O Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)
- [Serviços cloud](https://msdn.microsoft.com/library/azure/jj156091): Rede Virtual (clássico) apenas

Pode implementar um [Balanceador de carga do Azure interno](../load-balancer/load-balancer-internal-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) carregar equilibrar muitos dos recursos na lista anterior. Em alguns casos, o serviço cria e implementa automaticamente um balanceador de carga, quando cria um recurso.

## <a name="service-endpoints-for-azure-services"></a>Pontos finais de serviço para serviços do Azure

Alguns serviços do Azure não não possível implementar em redes virtuais. Pode restringir o acesso a alguns dos recursos de serviço para sub-redes de rede virtual apenas específico, se optar por ativar um ponto de final de serviço de rede virtual. Saiba mais sobre [pontos finais de serviço de rede virtual](virtual-network-service-endpoints-overview.md)e os serviços que, pontos de extremidade podem ser ativados para.

## <a name="virtual-network-integration-across-multiple-azure-services"></a>Integração da rede virtual em vários serviços do Azure

Pode implementar um serviço do Azure numa sub-rede de uma rede virtual e recursos de segurança críticos de serviço a essa sub-rede. Por exemplo, pode implementar o HDInsight numa rede virtual e proteger uma conta de armazenamento para a sub-rede do HDInsight.





