---
title: Com o ExpressRoute com a recuperação de desastre de máquina virtual do Azure | Microsoft Docs
description: Descreve como utilizar o Azure ExpressRoute com o Azure Site Recovery para a recuperação de desastre de máquina virtual do Azure
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/11/2018
ms.author: manayar
ms.openlocfilehash: 44ecbcc51cb53f4d7b68f5c5e24e7d81c5a4208c
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
---
# <a name="using-expressroute-with-azure-virtual-machine-disaster-recovery"></a>Com o ExpressRoute com a recuperação de desastre de máquina virtual do Azure

O Microsoft Azure ExpressRoute permite-lhe expandir as redes no local para a cloud da Microsoft através de uma ligação privada facilitada por um fornecedor de conectividade. Este artigo descreve como pode utilizar o ExpressRoute com a recuperação de Site para a recuperação de desastre de máquinas virtuais do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que compreende:
-   ExpressRoute [circuitos](../expressroute/expressroute-circuit-peerings.md)
-   ExpressRoute [domínios de encaminhamento](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains)
-   Máquina virtual do Azure [arquitetura de replicação](azure-to-azure-architecture.md)
-   [Configurar a replicação](azure-to-azure-tutorial-enable-replication.md) máquinas virtuais do Azure
-   [Efetuar a ativação pós-falha](azure-to-azure-tutorial-failover-failback.md) máquinas virtuais do Azure

## <a name="expressroute-and-azure-virtual-machine-replication"></a>Replicação da máquina virtual do Azure e ExpressRoute

Quando proteger máquinas virtuais do Azure com a recuperação de Site, replicação são enviados dados para uma conta de armazenamento do Azure ou a réplica gerido disco no destino região do Azure, dependendo se o seu Azure virtual máquinas utilize [discos geridos Azure](../virtual-machines/windows/managed-disks-overview.md). Embora os pontos finais de replicação são públicos, tráfego de replicação para a replicação de VM do Azure, por predefinição, não atravessar da Internet, independentemente da que região do Azure a rede virtual de origem existe.

Recuperação de desastres da VM do Azure, como dados de replicação deixam o limite do Azure, ExpressRoute não é necessário para a replicação. Depois de máquinas virtuais efetuar a ativação pós-falha para o destino da região do Azure, pode aceder aos mesmos utilizando [peering privado](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

## <a name="replicating-azure-deployments"></a>Replicar implementações do Azure

Anterior [artigo](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity), descrito uma configuração simples com uma rede virtual do Azure ligada ao centro de dados do cliente no local através do ExpressRoute. Implementações de típicos de empresas têm cargas de trabalho dividir por várias redes virtuais do Azure e um concentrador central conectividade estabelece a conectividade externa, à Internet e para as implementações no local.

Este exemplo descreve uma topologia hub- and -spoke, que é comum em implementações empresariais:
-   A implementação está no **Azure Oriental** região e o Centro de dados no local tem uma ligação de circuito de ExpressRoute através de um limite de parceiro em RAE de Hong Kong.
-   As aplicações são implementadas através de duas redes virtuais spoke – **origem VNet1** com endereço espaço 10.1.0.0/24 e **origem VNet2** com endereço espaço 10.2.0.0/24.
-   A rede virtual do hub, **origem Hub VNet**, com o espaço de endereços 10.10.10.0/24 age a (m) como. Todas as comunicações entre sub-redes passa através do hub.
-   A rede virtual do hub tem duas sub-redes – **NVA sub-rede** com endereço espaço 10.10.10.0/25 e **sub-rede do Gateway** com endereço espaço 10.10.10.128/25.
-   O **sub-rede NVA** tem uma aplicação virtual de rede com o endereço IP 10.10.10.10.
-   O **sub-rede do Gateway** tem um gateway ExpressRoute ligado a uma ligação de ExpressRoute que encaminha para o Centro de dados do cliente no local através de um domínio de encaminhamento do Peering privado.
-   Cada rede virtual spoke está ligado à rede virtual hub e o encaminhamento de todas as nesta topologia de rede é controlada através de tabelas de rota do Azure (UDR). Todos os tráfego de saída de uma VNet para outra VNet ou para o Centro de dados no local é encaminhado através da NVA.

![No local-para o Azure com o ExpressRoute antes da ativação pós-falha](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

### <a name="hub-and-spoke-peering"></a>Hub- and spoke peering

Spoke para hub peering tem as seguintes configurações:
-   Permitir que o endereço de rede virtual: ativado
-   Permitir tráfego reencaminhado: ativado
-   Permitir que o trânsito do gateway: desativado
-   Utilize remover gateways: ativado

 ![Spoke para configuração do peering hub](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

O hub para spoke peering tem as seguintes configurações:
-   Permitir que o endereço de rede virtual: ativado
-   Permitir tráfego reencaminhado: ativado
-   Permitir que o trânsito do gateway: ativado
-   Utilize remover gateways: desativado

 ![Hub para spoke configuração do peering](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="enabling-replication-for-the-deployment"></a>Ativar a replicação para a implementação

Para o programa de configuração acima, primeiro [configurar a recuperação após desastre](azure-to-azure-tutorial-enable-replication.md) para cada máquina virtual utilizando a recuperação de sites. Recuperação de sites pode criar a réplica redes virtuais (incluindo sub-redes e sub-redes do gateway) na região de destino e criar os mapeamentos necessários entre as redes virtuais de origem e de destino. Também pode pré-criar redes de lado de destino e sub-redes e utilizar o mesmo durante a ativação da replicação.

Recuperação de sites não replicar as tabelas de rotas, gateways de rede virtual, ligações de gateway de rede virtual, peering de rede virtual, ou quaisquer outras redes recursos ou ligações. Estes e outros recursos que não fazem parte do [processo de replicação](azure-to-azure-architecture.md#replication-process) tem de ser criados durante ou antes da ativação pós-falha e ligados aos recursos relevantes. Pode utilizar o Azure Site Recovery powerful [planos de recuperação](site-recovery-create-recovery-plans.md) para automatizar a criação e ligação a recursos adicionais utilizando scripts de automatização.

Por predefinição, o tráfego de replicação não deixe o limite do Azure. Normalmente, as implementações de NVA também definem uma rota predefinida (0.0.0.0/0) que força o tráfego de Internet de saída a passagem a NVA. Neste caso, o dispositivo poderá obter limitado se todo o tráfego de replicação atravessa a NVA. O mesmo também se aplica quando utilizar rotas predefinidas para encaminhamento todo o tráfego de VM do Azure para as implementações no local. Recomendamos [criar um ponto final de serviço de rede virtual](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) no seu virtual de rede para "Armazenamento", para que o tráfego de replicação não deixe o limite do Azure.

## <a name="failover-models-with-expressroute"></a>Modelos de ativação pós-falha com o ExpressRoute

Quando as máquinas virtuais do Azure são a ativação pós-falha numa região diferente, a ligação do ExpressRoute existente para a rede virtual de origem não é transferida automaticamente para a rede virtual de destino na região de recuperação. Uma nova ligação é necessário para estabelecer a ligação de ExpressRoute para a rede virtual de destino.

Pode replicar máquinas virtuais do Azure a qualquer região do Azure dentro do mesmo cluster geográfico conforme detalhado [aqui](azure-to-azure-support-matrix.md#region-support). Se a região do Azure de destino que escolheu não estiver na mesma região geopolítica como origem, tem de ativar o ExpressRoute Premium se estiver a utilizar um circuito ExpressRoute único para a conectividade de região de origem e de destino. Para mais detalhes, consulte [localizações do ExpressRoute](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) e [ExpressRoute preços](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="two-expressroute-circuits-in-two-different-expressroute-peering-locations"></a>Dois circuitos do ExpressRoute em duas localizações de peering de ExpressRoute diferentes
-   Esta configuração é útil se pretender assegurar contra falhas do circuito ExpressRoute primário e contra em grande escala regionais perante desastres, que também podem afetar as localizações de peering de ExpressRoute e perturbar o seu circuito de ExpressRoute primário.
-   Normalmente, o circuito ligado ao ambiente de produção é utilizado como o circuito primário e o circuito secundário é um failsafe e normalmente de menor largura de banda. A largura de banda de secundário pode ser aumentada num evento de desastre, quando o elemento secundário deve assumir como principal.
-   Com esta configuração pode estabelecer ligações do seu circuito de ExpressRoute secundária para a rede virtual de destino após a ativação pós-falha ou tem as ligações estabelecidas e prontas para uma declaração de desastre, reduzindo o tempo de recuperação geral. Com ligações simultâneas a ambos os principais e redes virtuais de região de destino, certifique-se de que o encaminhamento no local utiliza o circuito secundário e ligação apenas após a ativação pós-falha.
-   As redes virtuais de origem e de destino para as VMs protegidas com a recuperação de Site podem ter os endereços IP idêntica ou diferentes na ativação pós-falha pelos requisitos. Em ambos os casos, as ligações secundárias podem ser estabelecidas antes da ativação pós-falha.

### <a name="two-expressroute-circuits-in-the-same-expressroute-peering-location"></a>Dois circuitos do ExpressRoute na mesma localização de peering de ExpressRoute
-   Com esta configuração, pode assegurar contra falhas do circuito ExpressRoute primário, mas não contra desastres inesperados regionais em grande escala, que pode afetar as localizações de peering de ExpressRoute. Com a última opção, podem obter afetados circuitos primários e secundários.
-   As outras condições para endereços IP e ligações permanecerem os mesmos que os no caso de anterior. Pode ter ligações simultâneas do datacenter no local à rede virtual de origem com o circuito primário e para a rede virtual de destino com o circuito secundário. Com ligações simultâneas a ambos os principais e redes virtuais de região de destino, certifique-se de que o encaminhamento no local utiliza o circuito secundário e ligação apenas após a ativação pós-falha.
-   Não é possível ligar os dois circuitos à mesma rede virtual quando circuitos criados na mesma localização de peering.

### <a name="single-expressroute-circuit"></a>Circuito ExpressRoute único
-   Esta configuração não assegurar contra desastres regional em grande escala, que pode afetar a localização de peering do ExpressRoute.
-   Com um único circuito de ExpressRoute, não é possível estabelecer a ligação de origem e destino redes virtuais em simultâneo ao circuito, se for utilizado o mesmo espaço de endereços IP na região de destino.
-   Quando o mesmo espaço de endereços IP é utilizado na região de destino, a ligação de lado de origem deve ser interrompida e estabelecida a ligação de lado de destino após essa data. Esta alteração de ligação pode ser convertidos em script como parte de um plano de recuperação.
-   Uma falha regional, se a região primária está inacessível, poderia efetuar a operação desligar. Essa indisponibilidade pode afetar a criação da ligação para a região de destino ao mesmo espaço de endereços IP é utilizado na rede virtual de destino.
-   Se a criação da ligação é concluída com êxito no destino e a região primária recupera mais tarde, pode enfrentam pacote remoções se duas ligações simultâneas tentam estabelecer ligação ao mesmo espaço de endereços. Para evitar remoções de pacote, a ligação principal deve ser terminada imediatamente. Reativação pós-falha da POST de máquinas virtuais para a região primária, novamente a pode de ligação principal ser estabelecida depois de desligar a ligação secundária.
-   Se o espaço de endereço diferente é utilizado na rede virtual de destino, em seguida, pode em simultâneo ligar à origem de e redes virtuais do mesmo circuito de ExpressRoute de destino.

## <a name="recovering-azure-deployments"></a>Recuperar implementações do Azure
Considere o modelo de ativação pós-falha com dois circuitos do ExpressRoute diferentes em duas localizações de peering diferentes e retenção de endereços IP privados para as máquinas virtuais do Azure protegidas. A região de recuperação de destino é o Azure Sudeste asiático e é estabelecida uma ligação de circuito de ExpressRoute secundária através de um limite de parceiro em Singapura.

Para automatizar a recuperação da implementação completa, para além de replicar máquinas virtuais e redes virtuais, outras ligações de recursos de rede relevantes e tem também de ser criadas. O anterior hub- and -spoke topologia de rede adicionais seguintes passos têm de ser executadas durante ou após o [ativação pós-falha](azure-to-azure-tutorial-failover-failback.md) operação:
1.  Crie o Gateway do ExpressRoute do Azure na rede de virtual do hub de região de destino. É necessário o Gateway do ExpressRoute para ligar a rede virtual do hub de destino para o circuito do ExpressRoute.
2.  Crie a ligação de rede virtual a partir da rede virtual do hub de destino para o destino circuito ExpressRoute.
3.  Configure os peerings de VNet entre hub a região de destino e redes virtuais spoke. As propriedades do peering na região de destino serão as mesmas na região de origem.
4.  Configure os UDRs na hub VNet e a spoke duas VNets. As propriedades do lado do destino UDRs são idênticos no lado de origem quando o mesmo IP endereços. Com os endereços IP de destino diferente, os UDRs devem ser modificados em conformidade.

Os passos acima podem ser colocado em script como parte de um [plano de recuperação](site-recovery-create-recovery-plans.md). Consoante a conectividade de aplicação e os requisitos de tempo de recuperação, os passos acima também podem ser concluídos antes de iniciar a ativação pós-falha.

Após a recuperação das máquinas virtuais e a conclusão dos outros passos de conectividade, o ambiente de recuperação procura da seguinte forma: ![no local-para o Azure com o ExpressRoute após a ativação pós-falha](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)

Um exemplo de topologia simples para recuperação após desastre de VM do Azure com o circuito ExpressRoute único, com o mesmo IP em máquinas virtuais de destino, está detalhado [aqui](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity).

## <a name="recovery-time-objective-rto-considerations"></a>Considerações de objetivo de tempo (RTO) de recuperação
Para reduzir o tempo de recuperação geral para a sua implementação, recomendamos o aprovisionamento e implementar a região de destino adicionais [componentes de rede](azure-vm-disaster-recovery-with-expressroute.md#enabling-replication-for-the-deployment) como gateways de rede virtual com antecedência. Um período de indisponibilidade breves associado implementar recursos adicionais, e este período de indisponibilidade pode afetar o tempo de recuperação geral, caso contrário, tida em consideração durante o planeamento.

Recomendamos a execução regular [simulações de recuperação após desastre](azure-to-azure-tutorial-dr-drill.md) para proteger as implementações. Um exercício valida a estratégia de replicação sem perda de dados ou o período de indisponibilidade e não afeta o ambiente de produção. Também é executar um exercício evita a problemas de configuração do último minuto que podem afetar negativamente o objetivo de tempo de recuperação. Recomendamos que utilize uma rede separada da VM do Azure para ativação pós-falha de teste, em vez da rede predefinida que foi configurada quando ativar a replicação.

Se estiver a utilizar um único circuito ExpressRoute, recomendamos que utilize um espaço de endereços IP diferentes para a rede virtual de destino para evitar problemas de estabelecimento de ligação durante perante desastres regionais. Se não for viável para o seu ambiente de produção recuperada utilizar endereços IP diferentes, deve ser efetuada a ativação de pós-falha de teste de exercício de recuperação do desastre numa rede de teste separada com endereços IP diferentes, não é possível ligar duas redes virtuais com sobreposição de IP espaço de endereço para o mesmo circuito do ExpressRoute.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [circuitos ExpressRoute](../expressroute/expressroute-circuit-peerings.md).
- Saiba mais sobre [domínios de encaminhamento do ExpressRoute](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Saiba mais sobre [localizações do ExpressRoute](../expressroute/expressroute-locations.md).
- Saiba mais sobre [planos de recuperação](site-recovery-create-recovery-plans.md) para automatizar a ativação pós-falha de aplicação.
