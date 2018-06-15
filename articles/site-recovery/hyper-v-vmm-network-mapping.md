---
title: Sobre o mapeamento da rede para a replicação de VM de Hyper-V (com o VMM) para o Azure com a recuperação de Site | Microsoft Docs
description: Descreve como configurar o mapeamento de rede para a replicação de VMs de Hyper-V geridas em nuvens VMM, com o Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: raynew
ms.openlocfilehash: fa596bf4941ac791fa1bc697399a4591d97ba68f
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
ms.locfileid: "34076371"
---
# <a name="prepare-network-mapping-for-hyper-v-vm-replication-to-azure"></a>Preparar o mapeamento de rede para replicação da VM de Hyper-V para o Azure


Este artigo ajuda-o a compreender e preparar o mapeamento da rede quando replicar VMs de Hyper-V em nuvens do System Center Virtual Machine Manager (VMM) para o Azure ou para um site secundário, utilizando o [do Azure Site Recovery](site-recovery-overview.md) serviço.


## <a name="prepare-network-mapping-for-replication-to-azure"></a>Preparar o mapeamento da rede para replicação no Azure

Quando está a replicar para o Azure, mapas de mapeamento de rede entre redes VM num servidor VMM de origem e redes virtuais do Azure de destino. Mapeamento faz o seguinte:
    -  **Ligação de rede**— garante que replicadas VMs do Azure estão ligadas à rede mapeada. Todas as máquinas que efetuar a ativação pós-falha na mesma rede podem ligar-se entre si, mesmo a ativação pós-falha nos planos de recuperação diferente.
    - **Gateway de rede**— se estiver configurado um gateway de rede na rede do Azure de destino, as VMs podem ligar-se outras máquinas virtuais no local.

Mapeamento da rede funciona da seguinte forma:

- Mapear uma rede VM do VMM de origem a uma rede virtual do Azure.
- Após a ativação pós-falha de VMs do Azure na origem de rede serão ligadas à rede virtual de destino mapeadas.
- Novas VMs adicionadas à rede VM de origem estão ligadas à rede Azure mapeada quando ocorre a replicação.
- Se a rede de destino tiver várias sub-redes e um dessas sub-redes tiver o mesmo nome que a sub-rede onde está localizada a máquina virtual de origem, a máquina virtual de réplica liga-se a essa sub-rede de destino após a ativação pós-falha.
- Se não existir nenhuma sub-rede de destino com um nome correspondente, a máquina virtual liga-se à primeira sub-rede da rede.

## <a name="prepare-network-mapping-for-replication-to-a-secondary-site"></a>Preparar o mapeamento da rede para a replicação para um site secundário

Quando está a replicar para um site secundário, mapas de mapeamento da rede entre redes VM num servidor VMM de origem e redes VM no servidor VMM de destino. Mapeamento faz o seguinte:

- **Ligação de rede**— VMs liga-se às redes adequadas depois da ativação pós-falha. A VM de réplica será ligada à rede de destino que está mapeada para a rede de origem.
- **Colocação de VM ideal**— ideal coloca a réplica VMs nos servidores de anfitrião do Hyper-V. As VMs da réplica são colocadas em anfitriões que podem aceder a redes VM mapeadas.
- **Nenhum mapeamento da rede**— se não configurar o mapeamento da rede, as VMs de réplica não será ligada a quaisquer redes VM após a ativação pós-falha.

Mapeamento da rede funciona da seguinte forma:

- Mapeamento da rede pode ser configurado entre as redes VM em dois servidores do VMM, ou num único servidor VMM se dois sites são geridas pelo mesmo servidor.
- Quando mapeamento está configurado corretamente e a replicação está ativada, uma VM na localização principal será ligada a uma rede e a réplica na localização de destino será ligada à sua rede mapeadas.
- Quando seleciona uma rede VM de destino durante o mapeamento de rede na recuperação de Site, as nuvens de origem do VMM que utilizam a rede VM de origem serão apresentadas juntamente com as redes VM de destino disponíveis em nuvens de destino que são utilizadas para proteção.
- Se a rede de destino tem várias sub-redes e um dessas sub-redes tem o mesmo nome que a sub-rede que está localizada a máquina virtual de origem, em seguida, a VM de réplica será ligada a essa sub-rede de destino após a ativação pós-falha. Se não existir nenhuma sub-rede de destino com um nome correspondente, a VM será ligada à primeira sub-rede da rede.

## <a name="example"></a>Exemplo

Eis um exemplo para ilustrar este mecanismo. Vamos organização com duas localizações em Nova Iorque e Chicago.

**Localização** | **Servidor VMM** | **Redes VM** | **Mapeado para**
---|---|---|---
Nova Iorque | VMM-NewYork| VMNetwork1-NewYork | Mapeado para VMNetwork1 Chicago
 |  | VMNetwork2-NewYork | Não mapeados
Chicago | O VMM Chicago| VMNetwork1-Chicago | Mapeado para VMNetwork1 NewYork
 | | VMNetwork2-Chicago | Não mapeados

Neste exemplo:

- Quando uma réplica que é criada a VM para qualquer VM que está ligada ao VMNetwork1 NewYork, será ligado para VMNetwork1 Chicago.
- Quando uma réplica que é criada a VM para VMNetwork2 NewYork ou VMNetwork2 Chicago, não será ligada a nenhuma rede.

Eis como nuvens VMM são configuradas na nossa organização de exemplo e as redes lógicas associadas as nuvens.

### <a name="cloud-protection-settings"></a>Definições de proteção de nuvem

**Nuvem protegida** | **Proteção de nuvem** | **Rede lógica (Nova Iorque)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>ND</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>ND</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

### <a name="logical-and-vm-network-settings"></a>As definições de rede lógicas e VM

**Localização** | **Rede lógica** | **Rede VM associado**
---|---|---
Nova Iorque | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2Chicago | VMNetwork2-Chicago

### <a name="target-network-settings"></a>Definições de rede de destino

Com base nestas definições, quando seleciona a rede VM de destino, a tabela seguinte mostra as escolhas que estarão disponíveis.

**Selecionar** | **Nuvem protegida** | **Proteção de nuvem** | **Rede de destino disponível**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Disponível
 | GoldCloud1 | GoldCloud2 | Disponível
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Não disponível
 | GoldCloud1 | GoldCloud2 | Disponível


Se a rede de destino tem várias sub-redes e um dessas sub-redes tem o mesmo nome que a sub-rede que está localizada a máquina virtual de origem, em seguida, a máquina virtual de réplica será ligada a essa sub-rede de destino após a ativação pós-falha. Se não existir nenhuma sub-rede de destino com um nome correspondente, a máquina virtual será ligada à primeira sub-rede da rede.


### <a name="failback-behavior"></a>Comportamento de reativação pós-falha

Para ver o que acontece no caso de reativação pós-falha (replicação inversa), vamos assumir que VMNetwork1 NewYork está mapeada para VMNetwork1-Chicago, com as seguintes definições.


**VM** | **Ligado à rede VM**
---|---
VM1 | VMNetwork1-Network
VM2 (réplica do VM1) | VMNetwork1-Chicago

Com estas definições, vamos rever o que acontece em alguns cenários possíveis.

**Cenário** | **Resultado**
---|---
Nenhuma alteração nas propriedades de rede de VM-2 após a ativação pós-falha. | VM 1 continua a ser ligado à rede de origem.
Propriedades da rede de VM-2 foram alterados após a ativação pós-falha e está desligado. | VM-1 é desligado.
Propriedades da rede de VM-2 foram alterados após a ativação pós-falha e está ligado à VMNetwork2 Chicago. | Se não está mapeada VMNetwork2 Chicago, VM 1 será desligado.
Mapeamento da rede de VMNetwork1 Chicago é alterado. | VM 1 serão ligadas à rede agora mapeada para VMNetwork1 Chicago.



## <a name="next-steps"></a>Passos Seguintes

- [Saiba mais sobre](hyper-v-vmm-networking.md) endereçamento IP após a ativação pós-falha para um site secundário do VMM.
- [Saiba mais sobre](concepts-on-premises-to-azure-networking.md) endereçamento IP após a ativação pós-falha para o Azure.
