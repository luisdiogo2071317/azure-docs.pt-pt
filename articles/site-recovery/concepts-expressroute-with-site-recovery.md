---
title: O ExpressRoute do Azure com o Azure Site Recovery | Microsoft Docs
description: Descreve como utilizar o Azure ExpressRoute com o Azure Site Recovery para a migração e de recuperação após desastre
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 7cc4c84ebae7ade4169f8d85a2d5cc11f1df6f87
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
ms.locfileid: "34071452"
---
# <a name="azure-expressroute-with-azure-site-recovery"></a>ExpressRoute do Azure com o Azure Site Recovery

O Microsoft Azure ExpressRoute permite-lhe expandir as redes no local para a cloud da Microsoft através de uma ligação privada facilitada por um fornecedor de conectividade. Com o ExpressRoute, pode ligar aos serviços cloud da Microsoft, tais como o Microsoft Azure, o Office 365 e o Dynamics 365.

Este artigo descreve como pode utilizar o Azure ExpressRoute com o Azure Site Recovery para a migração e de recuperação após desastre.

## <a name="expressroute-circuits"></a>Circuitos do ExpressRoute

Um circuito ExpressRoute representa uma ligação lógica entre a sua infraestrutura no local e os cloud services da Microsoft através de um fornecedor de conectividade. Pode ordenar vários circuitos do ExpressRoute. Cada circuito pode estar em regiões do idêntica ou diferentes e pode ser ligado ao local através de fornecedores de conectividade diferentes. Saiba mais sobre circuitos ExpressRoute [aqui](../expressroute/expressroute-circuit-peerings.md).

## <a name="expressroute-routing-domains"></a>Domínios de encaminhamento do ExpressRoute

Um circuito ExpressRoute tem diversos domínios de encaminhamento associados:
-   [Peering privado do Azure](../expressroute/expressroute-circuit-peerings.md#azure-private-peering) - serviços, nomeadamente as máquinas virtuais (IaaS), de computação do Azure e serviços de cloud (PaaS) que são implementados numa rede virtual podem ser ligados através do domínio de peering privado. O domínio de peering privado é considerado uma extensão da sua rede principal fidedigna no Microsoft Azure.
-   [Peering público do Azure](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) -serviços, tais como Web sites, bases de dados SQL e Storage do Azure são disponibilizados no endereços IP públicos. Em privado pode ligar a serviços alojados em endereços IP públicos, incluindo VIPs dos seus serviços em nuvem, através do domínio de encaminhamento de peering público. Peering público foi preterido para criações novo e Peering da Microsoft deve ser utilizada em vez disso, para os serviços do Azure PaaS.
-   [Peering da Microsoft](../expressroute/expressroute-circuit-peerings.md#microsoft-peering) -conectividade ao Microsoft online services (serviços do Office 365, o Dynamics 365 e o Azure PaaS) é através do peering da Microsoft. Peering da Microsoft é o domínio de encaminhamento recomendado para ligar aos serviços do Azure PaaS.

Saiba mais sobre e comparar os domínios de encaminhamento do ExpressRoute [aqui](../expressroute/expressroute-circuit-peerings.md#routing-domain-comparison).

## <a name="on-premises-to-azure-replication-with-expressroute"></a>No local para a replicação do Azure com o ExpressRoute

O Azure Site Recovery permite a recuperação após desastre e a migração para o Azure para on-premises [máquinas virtuais Hyper-V](hyper-v-azure-architecture.md), [máquinas virtuais VMware](vmware-azure-architecture.md), e [servidores físicos](physical-azure-architecture.md). Para todos os no local para cenários do Azure, os dados de replicação são enviados e armazenados numa conta do Storage do Azure. Durante a replicação, que não paga quaisquer custos de máquina virtual. Quando executar uma ativação pós-falha para o Azure, a recuperação de sites cria automaticamente máquinas virtuais do IaaS do Azure.

Recuperação de site replica os dados para uma conta de armazenamento do Azure através de um ponto final público. Para utilizar o ExpressRoute para replicação de recuperação de Site, pode utilizar [peering público](../expressroute/expressroute-circuit-peerings.md#azure-public-peering) ou [peering da Microsoft](../expressroute/expressroute-circuit-peerings.md#microsoft-peering). Peering da Microsoft é o domínio de encaminhamento recomendado para replicação. Depois de máquinas virtuais ou servidores de ativação pós-falha para uma rede virtual do Azure, pode aceder aos mesmos utilizando [peering privado](../expressroute/expressroute-circuit-peerings.md#azure-private-peering). A replicação não é suportada ao longo do peering privado.

O cenário combinado que é representado no diagrama seguinte: ![no local-para o Azure com o ExpressRoute](./media/concepts-expressroute-with-site-recovery/site-recovery-with-expressroute.png)

## <a name="azure-to-azure-replication-with-expressroute"></a>Replicação do Azure para o Azure com o ExpressRoute

O Azure Site Recovery permite a recuperação de desastre do [máquinas virtuais do Azure](azure-to-azure-architecture.md). Dependendo se o seu Azure virtual máquinas utilize [Azure geridos discos](../virtual-machines/windows/managed-disks-overview.md), são enviados dados de replicação para uma conta de armazenamento do Azure ou a réplica gerido disco no destino região do Azure. Embora os pontos finais de replicação são públicos, tráfego de replicação para a replicação de VM do Azure, por predefinição, não atravessar da Internet, independentemente da que região do Azure a rede virtual de origem existe. Pode substituir a rota de sistema predefinida do Azure para o prefixo do endereço 0.0.0.0/0 com um [rotas personalizadas](../virtual-network/virtual-networks-udr-overview.md#custom-routes) e divert tráfego da VM para uma aplicação de virtual de rede no local (NVA), mas esta configuração não é recomendada para a recuperação de Site replicação. Se estiver a utilizar rotas personalizadas, deve [criar um ponto final de serviço de rede virtual](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) no seu virtual de rede para "Armazenamento", para que o tráfego de replicação deixam o limite do Azure.

Para a recuperação de desastre de VM do Azure, por predefinição, o ExpressRoute não é necessário para a replicação. Depois de máquinas virtuais efetuar a ativação pós-falha para o destino da região do Azure, pode aceder aos mesmos utilizando [peering privado](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

Se já estiver a utilizar o ExpressRoute para ligar a partir do seu centro de dados no local para as VMs do Azure na região de origem, pode planear para voltar a estabelecer conectividade do ExpressRoute na região de destino de ativação pós-falha. Pode utilizar o mesmo circuito de ExpressRoute para ligar à região de destino através de uma nova ligação de rede virtual ou utilizar uma ligação para recuperação após desastre e separado circuito do ExpressRoute. Os cenários possíveis diferentes descritos [aqui](azure-vm-disaster-recovery-with-expressroute.md#failover-models-with-expressroute).

Pode replicar máquinas virtuais do Azure a qualquer região do Azure dentro do mesmo cluster geográfico conforme detalhado [aqui](../site-recovery/azure-to-azure-support-matrix.md#region-support). Se a região do Azure de destino que escolheu não estiver na mesma região geopolítica como origem, poderá ter de ativar o ExpressRoute Premium. Para mais detalhes, consulte [localizações do ExpressRoute](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) e [ExpressRoute preços](https://azure.microsoft.com/pricing/details/expressroute/).

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [circuitos ExpressRoute](../expressroute/expressroute-circuit-peerings.md).
- Saiba mais sobre [domínios de encaminhamento do ExpressRoute](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Saiba mais sobre [localizações do ExpressRoute](../expressroute/expressroute-locations.md).
- Saiba mais sobre a recuperação após desastre de [máquinas virtuais do Azure com o ExpressRoute ](azure-vm-disaster-recovery-with-expressroute.md).
