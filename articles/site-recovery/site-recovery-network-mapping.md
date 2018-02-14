---
title: "Sobre o mapeamento da rede para a replicação de VM de Hyper-V com a recuperação de Site | Microsoft Docs"
description: "Configure o mapeamento de rede de Hyper-V replicação da máquina virtual de um datacenter no local para o Azure ou para um site secundário."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 02/07/2018
ms.author: raynew
ms.openlocfilehash: d56f8f5bfb40c1c43090f43e119bf9b98918d6e5
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="about-network-mapping-for-hyper-v-vm-replication"></a>Sobre o mapeamento da rede para a replicação de VM de Hyper-V


Este artigo ajuda-o a compreender e planear a rede mapeamento durante a replicação de VMs de Hyper-V para o Azure ou para um site secundário, utilizando o [serviço Azure Site Recovery](site-recovery-overview.md).

Depois de ler este artigo publique quaisquer comentários na parte inferior deste artigo ou coloque questões técnicas no [fórum de serviços de recuperação do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="network-mapping-for-replication-to-azure"></a>Mapeamento da rede para replicação no Azure

Mapeamento da rede é utilizado quando replicar VMs de Hyper-V (geridos no VMM) para o Azure. Mapas de mapeamento entre as redes VM no servidor VMM de origem de rede e redes do Azure de destino. Mapeamento faz o seguinte:

- **Ligação de rede**— garante que replicadas VMs do Azure estão ligadas à rede mapeada. Todas as máquinas que efetuar a ativação pós-falha na mesma rede podem ligar-se entre si, mesmo a ativação pós-falha nos planos de recuperação diferente.
- **Gateway de rede**— se estiver configurado um gateway de rede na rede do Azure de destino, as VMs podem ligar-se outras máquinas virtuais no local.

Tenha em atenção que:

- Mapear uma rede VM do VMM de origem a uma rede virtual do Azure.
- Após a ativação pós-falha de VMs do Azure na origem de rede serão ligadas à rede virtual de destino mapeadas.
- Novas VMs adicionadas à rede VM de origem estão ligadas à rede Azure mapeada quando ocorre a replicação.
- Se a rede de destino tiver várias sub-redes e um dessas sub-redes tiver o mesmo nome que a sub-rede onde está localizada a máquina virtual de origem, a máquina virtual de réplica liga-se a essa sub-rede de destino após a ativação pós-falha.
- Se não existir nenhuma sub-rede de destino com um nome correspondente, a máquina virtual liga-se à primeira sub-rede da rede.


## <a name="network-mapping-for-replication-to-a-secondary-datacenter"></a>Mapeamento da rede para a replicação para um datacenter secundário

Mapeamento da rede é utilizado quando replicar VMs de Hyper-V (gerido no System Center Virtual Machine Manager (VMM)) para um datacenter secundário. Os mapas de mapeamento de rede entre redes VM num servidor VMM de origem e redes VM no servidor VMM de destino. Mapeamento faz o seguinte:

- **Ligação de rede**— VMs liga-se às redes adequadas depois da ativação pós-falha. A VM de réplica será ligada à rede de destino que está mapeada para a rede de origem.
- **Colocação ideal**— ideal coloca a réplica VMs nos servidores de anfitrião do Hyper-V. As VMs da réplica são colocadas em anfitriões que podem aceder a redes VM mapeadas.
- **Nenhum mapeamento da rede**— se não configurar o mapeamento da rede, as VMs de réplica não será ligada a quaisquer redes VM após a ativação pós-falha.

Tenha em atenção que:

- Mapeamento da rede pode ser configurado entre as redes VM em dois servidores do VMM, ou num único servidor VMM se dois sites são geridas pelo mesmo servidor.
- Quando mapeamento está configurado corretamente e a replicação está ativada, uma VM na localização principal será ligada a uma rede e a réplica na localização de destino será ligada à sua rede mapeadas.
-
- Se a redes tem sido corretamente configurado no VMM, quando seleciona uma rede VM de destino durante o mapeamento da rede, as nuvens de origem do VMM que utilizam a rede VM de origem serão apresentadas juntamente com as redes VM de destino disponíveis em nuvens de destino que são utilizadas para proteção.
- Se a rede de destino tem várias sub-redes e um dessas sub-redes tem o mesmo nome que a sub-rede que está localizada a máquina virtual de origem, em seguida, a máquina virtual de réplica será ligada a essa sub-rede de destino após a ativação pós-falha. Se não existir nenhuma sub-rede de destino com um nome correspondente, a máquina virtual será ligada à primeira sub-rede da rede.



### <a name="example"></a>Exemplo

Eis um exemplo para ilustrar este mecanismo. Vamos organização com duas localizações em Nova Iorque e Chicago.

**Localização** | **Servidor VMM** | **Redes VM** | **Mapeado para**
---|---|---|---
Nova Iorque | VMM-NewYork| VMNetwork1-NewYork | Mapeado para VMNetwork1 Chicago
 |  | VMNetwork2-NewYork | Não mapeados
Chicago | VMM-Chicago| VMNetwork1-Chicago | Mapeado para VMNetwork1 NewYork
 | | VMNetwork1-Chicago | Não mapeados

Neste exemplo:

- Quando é criada uma máquina virtual de réplica para qualquer máquina virtual que está ligada ao VMNetwork1 NewYork, será ligado para VMNetwork1 Chicago.
- Quando é criada uma máquina virtual de réplica para VMNetwork2 NewYork ou VMNetwork2 Chicago, não será ligada a nenhuma rede.

Eis como nuvens VMM são configuradas na nossa organização de exemplo e as redes lógicas associadas as nuvens.

#### <a name="cloud-protection-settings"></a>Definições de proteção de nuvem

**Nuvem protegida** | **Proteção de nuvem** | **Rede lógica (Nova Iorque)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>ND</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>ND</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>As definições de rede lógicas e VM

**Localização** | **Rede lógica** | **Rede VM associado**
---|---|---
Nova Iorque | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

#### <a name="target-network-settings"></a>Definições de rede de destino

Com base nestas definições, quando seleciona a rede VM de destino, a tabela seguinte mostra as escolhas que estarão disponíveis.

**Selecionar** | **Nuvem protegida** | **Proteção de nuvem** | **Rede de destino disponível**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Disponível
 | GoldCloud1 | GoldCloud2 | Disponível
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Não disponível
 | GoldCloud1 | GoldCloud2 | Disponível


Se a rede de destino tem várias sub-redes e um dessas sub-redes tem o mesmo nome que a sub-rede que está localizada a máquina virtual de origem, em seguida, a máquina virtual de réplica será ligada a essa sub-rede de destino após a ativação pós-falha. Se não existir nenhuma sub-rede de destino com um nome correspondente, a máquina virtual será ligada à primeira sub-rede da rede.


#### <a name="failback-behavior"></a>Comportamento de reativação pós-falha

Para ver o que acontece no caso de reativação pós-falha (replicação inversa), vamos assumir que VMNetwork1 NewYork está mapeada para VMNetwork1-Chicago, com as seguintes definições.


**Máquina virtual** | **Ligado à rede VM**
---|---
VM1 | VMNetwork1-Network
VM2 (réplica do VM1) | VMNetwork1-Chicago

Com estas definições, vamos rever o que acontece em alguns cenários possíveis.

**Cenário** | **Outcome**
---|---
Nenhuma alteração nas propriedades de rede de VM-2 após a ativação pós-falha. | VM 1 continua a ser ligado à rede de origem.
Propriedades da rede de VM-2 foram alterados após a ativação pós-falha e está desligado. | VM-1 é desligado.
Propriedades da rede de VM-2 foram alterados após a ativação pós-falha e está ligado à VMNetwork2 Chicago. | Se não está mapeada VMNetwork2 Chicago, VM 1 será desligado.
Mapeamento da rede de VMNetwork1 Chicago é alterado. | VM 1 serão ligadas à rede agora mapeada para VMNetwork1 Chicago.



## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [planear a infraestrutura de rede](site-recovery-network-design.md).
