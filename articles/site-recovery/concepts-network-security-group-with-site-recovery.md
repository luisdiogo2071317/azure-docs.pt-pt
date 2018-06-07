---
title: Grupos de segurança com o Azure Site Recovery de rede | Microsoft Docs
description: Descreve como utilizar grupos de segurança de rede com o Azure Site Recovery para a migração e de recuperação após desastre
services: site-recovery
documentationcenter: ''
author: mayanknayar
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 05/21/2018
ms.author: manayar
ms.openlocfilehash: 2b35578e2dc49cd47689f62fc47c5341ea8767a4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655758"
---
# <a name="network-security-groups-with-azure-site-recovery"></a>Grupos de segurança de rede com o Azure Site Recovery

Grupos de segurança de rede são utilizados para limitar o tráfego de rede para recursos numa rede virtual. A [grupo de segurança de rede (NSG)](../virtual-network/security-overview.md#network-security-groups) contém uma lista de regras de segurança que permitem ou negam o tráfego de rede de entrada ou saída com base na origem ou endereço IP de destino, porta e protocolo.

No modelo de implementação Resource Manager, os NSGs podem ser associados a sub-redes ou interfaces de rede individuais. Quando um NSG é associado a uma sub-rede, as regras são aplicadas a todos os recursos ligados à mesma. Tráfego mais pode ser restringido ao também associar um NSG a interfaces de rede individuais dentro de uma sub-rede que já tem um NSG associado.

Este artigo descreve como pode utilizar grupos de segurança de rede com o Azure Site Recovery.

## <a name="using-network-security-groups"></a>Utilizar grupos de segurança de rede

Uma sub-rede individual pode ter zero ou um, associados NSG. Também pode ter uma interface de rede individuais zero ou um, associados NSG. Por isso, pode ter eficazmente restrição tráfego dupla para uma máquina virtual ao associar um NSG pela primeira vez a uma sub-rede e, em seguida, outra NSG a interface de rede da VM. Depende a aplicação de regras do NSG neste caso, a direção do tráfego e a prioridade das regras de segurança aplicada.

Considere um exemplo simples com uma máquina virtual da seguinte forma:
-   A máquina virtual é colocada dentro de **Contoso sub-rede**.
-   **Contoso sub-rede** está associado **sub-rede NSG**.
-   A interface de rede VM é associada adicionalmente **VM NSG**.

![NSG com a recuperação de Site](./media/concepts-network-security-group-with-site-recovery/site-recovery-with-network-security-group.png)

Neste exemplo, para o tráfego de entrada, o NSG de sub-rede é avaliado pela primeira vez. Qualquer tráfego permitido através da sub-rede NSG, em seguida, é avaliado por NSG de VM. A reversão é aplicável para tráfego de saída, com o NSG de VM que está a ser avaliado pela primeira vez. Qualquer tráfego permitido através de NSG de VM, em seguida, é avaliado por NSG de sub-rede.

Isto permite às aplicações de regra de segurança granular. Por exemplo, pode querer permitir acesso de entrada de internet para algumas VMs de aplicação (por exemplo, o front-end VMs) com uma sub-rede, mas restringir o acesso de entrada de internet para as outras VMs (como base de dados e outra VMs de back-end). Neste caso, pode ter uma regra mais lenient no NSG sub-rede, permitindo que o tráfego de internet e restringir o acesso a VMs específicas por negar o acesso na VM NSG. O mesmo pode ser aplicado para tráfego de saída.

Quando configurar essas configurações de NSG, certifique-se de que as prioridades corretas são aplicadas para o [regras de segurança](../virtual-network/security-overview.md#security-rules). As regras são processadas por ordem de prioridade, sendo os números mais baixos processados antes dos mais elevados, uma vez que têm uma prioridade superior. Quando o tráfego corresponder a uma regra, o processamento para. Como resultado, qualquer regra que exista com prioridades inferiores (números mais elevados) e que tenham os mesmos atributos das regras com prioridades superiores não é processada.

É possível que não esteja sempre ciente de que os grupos de segurança de rede estão aplicados, quer à interface de rede, quer à sub-rede. Pode verificar as regras de agregação aplicadas a uma interface de rede, visualizando o [regras de segurança eficaz](../virtual-network/virtual-network-network-interface.md#view-effective-security-rules) para uma interface de rede. Também pode utilizar o [Certifique-se de fluxo IP](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md) capacidade no [observador de rede de Azure](../network-watcher/network-watcher-monitoring-overview.md) para determinar se a comunicação é permitida para ou a partir de uma interface de rede. A ferramenta indica se é permitida a comunicação e que regra de segurança de rede permite ou nega o tráfego.

## <a name="on-premises-to-azure-replication-with-nsg"></a>No local para a replicação do Azure com o NSG

O Azure Site Recovery permite a recuperação após desastre e a migração para o Azure para on-premises [máquinas virtuais Hyper-V](hyper-v-azure-architecture.md), [máquinas virtuais VMware](vmware-azure-architecture.md), e [servidores físicos](physical-azure-architecture.md). Para todos os no local para cenários do Azure, os dados de replicação são enviados e armazenados numa conta do Storage do Azure. Durante a replicação, que não paga quaisquer custos de máquina virtual. Quando executar uma ativação pós-falha para o Azure, a recuperação de sites cria automaticamente máquinas virtuais do IaaS do Azure.

Assim que as VMs foram criadas após a ativação pós-falha para o Azure, podem ser utilizados NSGs para limitar o tráfego de rede para a rede virtual e VMs. Recuperação de sites não cria NSGs como parte da operação de ativação pós-falha. Recomendamos que crie o NGSs Azure necessários antes de iniciar a ativação pós-falha. Pode, em seguida, associar NSGs para efetuar a ativação pós-falha de VMs automaticamente durante a ativação pós-falha, utilizando scripts de automatização com a recuperação de Site poderosa [planos de recuperação](site-recovery-create-recovery-plans.md).

Por exemplo, se a configuração de VM pós-falha é semelhante a [cenário de exemplo](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) detalhadas acima:
-   Pode criar **Contoso VNet** e **Contoso sub-rede** como parte da DR planeamento no destino da região do Azure.
-   Também pode criar e configurar ambos **sub-rede NSG** , bem como **VM NSG** como parte do mesmo DR planeamento.
-   **Sub-rede NSG** pode, em seguida, ser imediatamente associado **Contoso sub-rede**, dado que o NSG e a sub-rede já estão disponíveis.
-   **VM NSG** pode ser associado com VMs durante a ativação pós-falha com planos de recuperação.

Depois dos NSGs são criados e configurados, recomenda-se em execução um [ativação pós-falha de teste](site-recovery-test-failover-to-azure.md) verificar convertidos em script associações de NSG e conectividade VM de pós-falha.

## <a name="azure-to-azure-replication-with-nsg"></a>Replicação do Azure para o Azure com o NSG

O Azure Site Recovery permite a recuperação de desastre do [máquinas virtuais do Azure](azure-to-azure-architecture.md). Ao ativar a replicação para as VMs do Azure, a recuperação de sites pode criar a réplica redes virtuais (incluindo sub-redes e sub-redes do gateway) na região de destino e criar os mapeamentos necessários entre a origem e redes virtuais de destino. Também pode pré-criar redes de lado de destino e sub-redes e utilizar o mesmo durante a ativação da replicação. Recuperação de sites não criar VMs no antes da região do Azure de destino para [ativação pós-falha](azure-to-azure-tutorial-failover-failback.md).

Para replicação de VM do Azure, certifique-se de que as regras do NSG na origem de região do Azure permitem [conectividade de saída](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) para tráfego de replicação. Também pode testar e verifique se estas regras necessárias através deste [exemplo de configuração de NSG](azure-to-azure-about-networking.md#example-nsg-configuration).

Recuperação de sites não crie ou replicar NSGs como parte da operação de ativação pós-falha. Recomendamos que crie os NGSs necessárias no destino região do Azure antes de iniciar a ativação pós-falha. Pode, em seguida, associar NSGs para efetuar a ativação pós-falha de VMs automaticamente durante a ativação pós-falha, utilizando scripts de automatização com a recuperação de Site poderosa [planos de recuperação](site-recovery-create-recovery-plans.md).

Considerar a [cenário de exemplo](concepts-network-security-group-with-site-recovery.md#using-network-security-groups) descrito anteriormente:
-   Recuperação de sites pode criar as réplicas de **Contoso VNet** e **Contoso sub-rede** no destino região do Azure quando a replicação está ativada para a VM.
-   Pode criar as réplicas pretendidas **sub-rede NSG** e **VM NSG** (com o nome, por exemplo, **NSG de sub-rede de destino** e **NSG de VM de destino**, respetivamente) no destino região do Azure, permitindo quaisquer regras adicionais necessárias na região de destino.
-   **NSG de sub-rede de destino** , em seguida, pode ser imediatamente associados à sub-rede de região de destino, dado que o NSG e a sub-rede já estão disponíveis.
-   **NSG de VM de destino** pode ser associado com VMs durante a ativação pós-falha com planos de recuperação.

Depois dos NSGs são criados e configurados, recomenda-se em execução um [ativação pós-falha de teste](azure-to-azure-tutorial-dr-drill.md) verificar convertidos em script associações de NSG e conectividade VM de pós-falha.

## <a name="next-steps"></a>Passos Seguintes
-   Saiba mais sobre [grupos de segurança de rede](../virtual-network/security-overview.md#network-security-groups).
-   Saiba mais sobre o NSG [regras de segurança](../virtual-network/security-overview.md#security-rules).
-   Saiba mais sobre [regras de segurança eficaz](../virtual-network/diagnose-network-traffic-filter-problem.md) para um NSG.
-   Saiba mais sobre [planos de recuperação](site-recovery-create-recovery-plans.md) para automatizar a ativação pós-falha de aplicação.
