---
title: Através do ExpressRoute com recuperação após desastre de máquina virtual do Azure | Documentos da Microsoft
description: Descreve como utilizar o Azure ExpressRoute com o Azure Site Recovery para recuperação após desastre de máquina virtual do Azure
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: manayar
ms.openlocfilehash: 73514b524f554affb9730ba63ccd608491497af2
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920475"
---
# <a name="using-expressroute-with-azure-virtual-machine-disaster-recovery"></a>Através do ExpressRoute com recuperação após desastre de máquina virtual do Azure

O Microsoft Azure ExpressRoute permite-lhe expandir as redes no local para a cloud da Microsoft através de uma ligação privada facilitada por um fornecedor de conectividade. Este artigo descreve como pode utilizar o ExpressRoute com o Site Recovery para recuperação após desastre de máquinas virtuais do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que compreende:
-   ExpressRoute [circuitos](../expressroute/expressroute-circuit-peerings.md)
-   ExpressRoute [domínios de encaminhamento](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains)
-   Máquina virtual do Azure [arquitetura da replicação](azure-to-azure-architecture.md)
-   [Configurar a replicação](azure-to-azure-tutorial-enable-replication.md) para máquinas virtuais do Azure
-   [Efetuar a ativação pós-falha](azure-to-azure-tutorial-failover-failback.md) máquinas virtuais do Azure

## <a name="expressroute-and-azure-virtual-machine-replication"></a>Replicação da máquina virtual do Azure e ExpressRoute

Quando proteger máquinas virtuais do Azure com o Site Recovery, dados de replicação são enviados para uma conta de armazenamento do Azure ou a réplica do disco gerido no destino região do Azure, dependendo se o seu Azure virtual máquinas utilização [Azure Managed Disks](../virtual-machines/windows/managed-disks-overview.md). Embora os pontos de extremidade de replicação são públicos, o tráfego de replicação para a replicação de VM do Azure, por predefinição, atravessa a Internet, independentemente de qual a região do Azure a rede virtual de origem existe na.

Para recuperação após desastre de VM do Azure, como dados de replicação não saem do limite do Azure, ExpressRoute não é necessário para a replicação. Depois de máquinas virtuais de ativação pós-falha para o destino região do Azure, pode acessá-los usando [peering privado](../expressroute/expressroute-circuit-peerings.md#azure-private-peering).

## <a name="replicating-azure-deployments"></a>Replicar de implementações do Azure

Anterior [artigo](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity), descrito uma configuração simples com uma rede virtual do Azure ligado ao centro de dados do cliente no local através do ExpressRoute. Implementações empresariais típicos têm cargas de trabalho dividir por várias redes virtuais do Azure e um hub central de conectividade estabelece conectividade externa, para a Internet e para implementações no local.

Este exemplo descreve uma topologia hub- and -spoke, que é comum em Implantações corporativas:
-   A implementação tem o **Azure Ásia Oriental** região e o datacenter no local tem uma ligação de circuito do ExpressRoute através de um limite de parceiro em RAE de Hong Kong.
-   Aplicativos são implantados em duas redes virtuais "spoke" – **origem VNet1** com o endereço espaço 10.1.0.0/24 e **origem VNet2** com endereço espaço 10.2.0.0/24.
-   Rede virtual do concentrador, **VNet do Hub de origem**, com o espaço de endereços 10.10.10.0/24 atua como o controlador de chamadas. Todas as comunicações entre sub-redes vai através do hub.
-   Rede virtual do concentrador tem duas sub-redes – **sub-rede da NVA** com o endereço espaço 10.10.10.0/25 e **sub-rede do Gateway** com endereço espaço 10.10.10.128/25.
-   O **sub-rede da NVA** tem uma aplicação de rede virtual com endereço IP 10.10.10.10.
-   O **sub-rede do Gateway** estabeleceu um gateway do ExpressRoute para uma ligação de ExpressRoute que encaminha para o Centro de dados do cliente no local através de um domínio de encaminhamento de Peering privado.
-   Cada rede virtual do spoke está ligado à rede virtual do concentrador e encaminhamento de todas as dentro esta topologia de rede é controlada por meio de tabelas de rota do Azure (UDR). Todo o tráfego de saída de uma VNet para outra VNet ou para o datacenter no local é encaminhado através da NVA.

![No local-para-Azure com o ExpressRoute antes da ativação pós-falha](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-before-failover.png)

### <a name="hub-and-spoke-peering"></a>Hub e spoke peering

O spoke para peering do hub tem a seguinte configuração:
-   Permitir o endereço de rede virtual: ativada
-   Permitir tráfego reencaminhado: ativada
-   Permitir que o trânsito de gateway: desativado
-   Utilize remover gateways: ativada

 ![-Spoke para configuração de peering do hub](./media/azure-vm-disaster-recovery-with-expressroute/spoke-to-hub-peering-configuration.png)

O hub para falava peering tem a seguinte configuração:
-   Permitir o endereço de rede virtual: ativada
-   Permitir tráfego reencaminhado: ativada
-   Permitir que o trânsito de gateway: ativada
-   Utilize remover gateways: desativado

 ![Hub para falava configuração de peering](./media/azure-vm-disaster-recovery-with-expressroute/hub-to-spoke-peering-configuration.png)

### <a name="enabling-replication-for-the-deployment"></a>Ativar a replicação para a implementação

Para a configuração acima, primeira [configurar a recuperação após desastre](azure-to-azure-tutorial-enable-replication.md) para todas as máquinas virtuais com o Site Recovery. Recuperação de sites pode criar redes virtuais (incluindo sub-redes e sub-redes do gateway) de réplica na região de destino e criar os mapeamentos necessários entre as redes virtuais de origem e de destino. Também pode criar previamente as sub-redes e redes de lado de destino e utilizar o mesmo ao ativar a replicação.

Recuperação de sites é replicado não tabelas de rotas, gateways de rede virtual, ligações de gateway de rede virtual, peering de rede virtual, ou qualquer outro sistema de rede recursos ou ligações. Esses e outros recursos não fazem parte dos [processo de replicação](azure-to-azure-architecture.md#replication-process) precisam ser criados durante ou antes da ativação pós-falha e ligados aos recursos relevantes. Pode utilizar o Azure Site Recovery poderosos [planos de recuperação](site-recovery-create-recovery-plans.md) para automatizar a criar e ligar a recursos adicionais com scripts de automatização.

Por predefinição, o tráfego de replicação não deixam o limite do Azure. Normalmente, as implementações da NVA também definem uma rota predefinida (0.0.0.0/0) que força o tráfego de Internet de saída a fluir através da NVA. Neste caso, a aplicação poderá se verá limitada se todo o tráfego de replicação passa através da NVA. O mesmo também se aplica ao usar as rotas predefinidas para encaminhar todo o tráfego de VM do Azure para implementações no local. Recomendamos [criar um ponto de extremidade do serviço de rede virtual](azure-to-azure-about-networking.md#create-network-service-endpoint-for-storage) no seu virtual de rede para "Armazenamento", para que o tráfego de replicação não deixam o limite do Azure.

## <a name="failover-models-with-expressroute"></a>Modelos de ativação pós-falha com o ExpressRoute

Quando as máquinas virtuais do Azure são ativação pós-falha para uma região diferente, a ligação de ExpressRoute existente para a rede virtual de origem não é automaticamente transferida para a rede virtual de destino na região de recuperação. Uma nova ligação é necessário para ligar o ExpressRoute para a rede virtual de destino.

Pode replicar máquinas virtuais do Azure para qualquer região do Azure dentro do mesmo cluster geográfico conforme detalhado [aqui](azure-to-azure-support-matrix.md#region-support). Se a região do Azure de destino escolhido não estiver na mesma região geopolítica como a origem, tem de ativar o ExpressRoute Premium se estiver a utilizar um circuito ExpressRoute único para a conectividade de região de origem e de destino. Para obter mais detalhes, consulte [localizações do ExpressRoute](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) e [preços do ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute/).

### <a name="two-expressroute-circuits-in-two-different-expressroute-peering-locations"></a>Dois circuitos do ExpressRoute em duas localizações de peering de ExpressRoute diferentes
-   Esta configuração é útil se pretender assegurar contra falhas do circuito de ExpressRoute primário e contra desastres regionais em grande escala, que também podem afetar as localizações de peering de ExpressRoute e interromper o seu circuito do ExpressRoute primário.
-   Normalmente o circuito ligado ao ambiente de produção é utilizado como o circuito primário e o circuito secundário é uma segurança contra falhas e, normalmente, de largura de banda inferior. A largura de banda da secundária pode ser aumentada num evento de desastre, quando o elemento secundário deve assumir como principal.
-   Com esta configuração pode estabelecer ligações a partir do seu circuito do ExpressRoute secundário para a rede virtual de destino após a ativação pós-falha ou tem as ligações estabelecidas e está prontas para uma declaração de desastres, reduzindo seu tempo de recuperação geral. Com ligações simultâneas a ambos os principal e redes virtuais de região de destino, certifique-se de que o ambiente encaminhamento usa o circuito secundário e a ligação apenas após a ativação pós-falha.
-   As redes virtuais de origem e de destino para as VMs protegidas com o Site Recovery podem ter os endereços IP idêntica ou diferentes na ativação pós-falha por seus requisitos. Em ambos os casos, as ligações secundárias podem ser estabelecidas antes da ativação pós-falha.

### <a name="two-expressroute-circuits-in-the-same-expressroute-peering-location"></a>Dois circuitos do ExpressRoute na mesma localização de peering de ExpressRoute
-   Com esta configuração, pode assegurar contra falhas do circuito de ExpressRoute primário, mas não contra desastres regionais em grande escala, que pode afetar o localizações de peering do ExpressRoute. Com a última opção, podem ser afetados circuitos primários e secundários.
-   As outras condições para endereços IP e ligações permanecem os mesmos no caso anterior. Pode ter ligações simultâneas entre o datacenter no local para a rede virtual de origem com o circuito primário e para a rede virtual de destino com o circuito secundário. Com ligações simultâneas a ambos os principal e redes virtuais de região de destino, certifique-se de que o ambiente encaminhamento usa o circuito secundário e a ligação apenas após a ativação pós-falha.
-   Não é possível ligar os dois circuitos à mesma rede virtual quando os circuitos são criados na mesma localização de peering.

### <a name="single-expressroute-circuit"></a>Circuito ExpressRoute único
-   Esta configuração não assegurar contra um desastre regional em grande escala, o que poderia afetar a localização de peering do ExpressRoute.
-   Com um único circuito ExpressRoute, não é possível ligar a origem e redes virtuais em simultâneo para o circuito de destino, se o mesmo espaço de endereços IP é utilizado na região de destino.
-   Quando o mesmo espaço de endereços IP é utilizado na região de destino, a ligação de lado de origem deve ser desligada e a ligação de lado do destino estabelecida após esta data. Esta alteração de ligação pode ser colocado em script como parte de um plano de recuperação.
-   Numa falha regional, se a região primária não estiver acessível, a operação desligar poderá falhar. Um período de indisponibilidade poderá afetar a criação da ligação para a região de destino quando o mesmo espaço de endereços IP é utilizado na rede virtual de destino.
-   Se a criação da ligação tem êxito no destino e a região primária recupera mais tarde, pode deparar quedas de pacotes, se duas conexões simultâneas tentam estabelecer ligação ao mesmo espaço de endereço. Para impedir que as quedas de pacote, a conexão principal será encerrada imediatamente. Reativação pós-falha de postagem de máquinas virtuais para a região primária, a lata de ligação primária novamente estabelecer após a desconexão a ligação secundária.
-   Se o espaço de endereço diferente é utilizado na rede virtual de destino, em seguida, pode simultaneamente ligar à origem de e redes virtuais no mesmo circuito do ExpressRoute de destino.

## <a name="recovering-azure-deployments"></a>Recuperação de implementações do Azure
Considere o modelo de ativação pós-falha com dois circuitos do ExpressRoute diferentes em duas localizações de peering diferentes e retenção de endereços IP privados para as máquinas de virtuais do Azure protegidos. Região de recuperação de destino é o Azure Sudeste asiático e uma ligação do circuito de ExpressRoute secundária é estabelecida através de um limite de parceiro em Singapura.

Para automatizar a recuperação de toda a implementação, para replicar máquinas virtuais e redes virtuais, além de outros recursos de rede relevantes e ligações também devem ser criadas. Para o anterior hub- and -spoke topologia de rede adicionais seguintes passos têm de ser realizadas durante ou após a [ativação pós-falha](azure-to-azure-tutorial-failover-failback.md) operação:
1.  Crie o Gateway do ExpressRoute do Azure na rede de virtual do concentrador de região de destino. O Gateway do ExpressRoute é necessário para ligar a rede virtual do concentrador de destino para o circuito do ExpressRoute.
2.  Crie a ligação de rede virtual a partir da rede virtual do hub de destino para o circuito do ExpressRoute de destino.
3.  Configure os peerings de VNet entre o hub a região de destino e redes virtuais indicadas. As propriedades do peering na região de destino serão iguais na região de origem.
4.  Configure as UDRs na VNet do hub e spoke duas VNets. As propriedades do lado do destino UDRs são iguais do lado de origem quando o mesmo IP de através de endereços. Com os endereços IP de destino diferente, as UDRs devem ser modificadas em conformidade.

Os passos acima podem ser colocado em script como parte de um [plano de recuperação](site-recovery-create-recovery-plans.md). Consoante a conectividade de aplicativo e requisitos de tempo de recuperação, os passos acima também podem ser concluídos antes de iniciar a ativação pós-falha.

Publicar a recuperação das máquinas virtuais e a conclusão dos outros passos de conectividade, o ambiente de recuperação será semelhante ao seguinte: ![no local-para-Azure com o ExpressRoute após a ativação pós-falha](./media/azure-vm-disaster-recovery-with-expressroute/site-recovery-with-expressroute-after-failover.png)

Um exemplo de topologia simples para recuperação após desastre de VM do Azure com o circuito ExpressRoute único, com o mesmo IP nas máquinas de virtuais de destino, está detalhado [aqui](site-recovery-retain-ip-azure-vm-failover.md#on-premises-to-azure-connectivity).

## <a name="recovery-time-objective-rto-considerations"></a>Considerações de objetivo de tempo (RTO) de recuperação
Para reduzir o tempo de recuperação geral para a sua implementação, recomendamos, aprovisionamento e implementação de região de destino adicionais [componentes de rede](azure-vm-disaster-recovery-with-expressroute.md#enabling-replication-for-the-deployment) como gateways de rede virtual com antecedência. Um pequeno período de indisponibilidade é associado com a implantação de recursos adicionais, e este período de indisponibilidade pode afetar o tempo de recuperação geral, se não foram responsáveis por durante o planejamento.

Recomendamos a execução regular [testes de recuperação após desastre](azure-to-azure-tutorial-dr-drill.md) protegidos implementações. Um teste valida a estratégia de replicação sem perda de dados ou de indisponibilidade e não afeta o seu ambiente de produção. Também é executar um teste de evita problemas de configuração de última hora que podem afetar negativamente o objetivo de tempo de recuperação. Recomendamos que utilize uma rede de VM do Azure separada para a ativação pós-falha de teste, em vez da rede predefinida que foi configurada quando ativou a replicação.

Se estiver a utilizar um único circuito ExpressRoute, recomendamos que utilize um espaço de endereços IP diferente para a rede virtual de destino para evitar problemas de estabelecimento de ligação durante a desastres regionais. Se utilizar endereços IP diferentes não é viável para o seu ambiente de produção recuperada, a ativação de pós-falha de teste de exploração de recuperação do desastre deve ser feita numa rede de teste separada com endereços IP diferentes, como não é possível ligar duas redes virtuais com sobreposição de IP espaço de endereço para o mesmo circuito do ExpressRoute.

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [circuitos do ExpressRoute](../expressroute/expressroute-circuit-peerings.md).
- Saiba mais sobre [domínios de encaminhamento do ExpressRoute](../expressroute/expressroute-circuit-peerings.md#expressroute-routing-domains).
- Saiba mais sobre [localizações do ExpressRoute](../expressroute/expressroute-locations.md).
- Saiba mais sobre [planos de recuperação](site-recovery-create-recovery-plans.md) para automatizar a ativação pós-falha do aplicativo.
