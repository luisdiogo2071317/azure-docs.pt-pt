---
title: O que há de novo no Azure Site Recovery | Documentos da Microsoft
description: Fornece um resumo dos novos recursos introduzidos no Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 01/28/2019
ms.author: raynew
ms.openlocfilehash: fef6dc7937b2c83e67a281c69267bdf29216793d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55199142"
---
# <a name="whats-new-in-site-recovery"></a>O que há de novo no Site Recovery

O [do Azure Site Recovery](site-recovery-overview.md) serviço é atualizado e melhorado de forma contínua. Para ajudar a manter-se atualizado, este artigo fornece informações sobre as versões mais recentes, novos recursos e novo conteúdo. Esta página é atualizada regularmente.

Se tiver sugestões para recursos de recuperação de Site, Adoraríamos [ouvir os seus comentários](https://feedback.azure.com/forums/256299-site-recovery).

## <a name="q1-2019"></a>2019 P1

### <a name="linux-support"></a>Apoio Técnico para Linux

[Coletânea de 32](https://support.microsoft.com/help/4485985/update-rollup-32-for-azure-site-recovery) oferece uma atualização para os agentes de recuperação de Site e fornecedores. As atualizações adiciona suporte para Linux da seguinte forma:

- **Recuperação após desastre de VMs do Azure**: Estação de trabalho do Red Hat 6/7; novas versões de kernel para Ubuntu, Debian e SUSE.
- **Recuperação após desastre de servidores de VMs de VMware/físico para o Azure**: Red Hat Enterprise Linux a 7,6; Estação de trabalho do Red Hat 6/7; Oracle Linux 6.10/7,6 novas versões de kernel Ubuntu, Debian e SUSE.



## <a name="q4-2018"></a>2018 P4

## <a name="pricing-calculator-for-azure-vm-disaster-recovery"></a>Calculadora de preços para recuperação após desastre de VM do Azure

Recuperação após desastre para VMs do Azure incorre em custos de licenciamento de VM e os custos de armazenamento e rede. O Azure fornece um [Calculadora de preços](https://aka.ms/a2a-cost-estimator) para ajudar a descobrir esses custos. Recuperação de sites fornece agora uma [exemplos de preços estimativa](https://aka.ms/a2a-cost-estimator) que os preços de uma implementação de exemplo com base numa aplicação de três camadas com seis VMs com discos de HDD padrão 12 e 6 discos de Premium SSD. O exemplo presumem uma alteração de dados de 10 GB por dia para standard e 20 GB para premium. Para a sua implementação específica, pode alterar as variáveis de estimar os custos. Pode especificar o número de VMs, o número e tipo de discos geridos e o total de dados esperado altera a frequência esperada entre as VMs. Além disso, pode aplicar um fator de compactação para estimar os custos de largura de banda. [Leitura](https://azure.microsoft.com/blog/know-exactly-how-much-it-will-cost-for-enabling-dr-to-your-azure-vm/) o anúncio.

### <a name="support-for-azure-vms-in-zones"></a>Suporte para VMs do Azure em zonas

As zonas de disponibilidade do Azure são localizações físicas únicas dentro de uma região do Azure. Cada zona é constituída por um ou mais datacenters equipados com energia, refrigeração e redes. Agora pode ativar a replicação para uma VM do Azure e definir o destino para a ativação pós-falha para uma única instância VM, uma VM num conjunto de disponibilidade ou uma VM numa zona de disponibilidade. A definição não tem impacto na replicação. [Leitura](https://azure.microsoft.com/blog/disaster-recovery-of-zone-pinned-azure-virtual-machines-to-another-region/) o anúncio.
 
### <a name="disaster-recovery-for-encrypted-vms"></a>Recuperação após desastre para VMs encriptadas

Site Recovery suporta VMs do Azure encriptado com encriptação de disco do Azure (ADE) com a aplicação do Azure AD. [Saiba mais](azure-to-azure-how-to-enable-replication-ade-vms.md).

### <a name="disaster-recovery-for-vms-using-accelerated-networking"></a>Recuperação após desastre para VMs com o accelerated networking

Funcionamento em rede acelerado ativar Virtualização de e/s de raiz única (SR-IOV) para uma VM, melhorando o desempenho do sistema de rede. Quando ativa a replicação para uma VM do Azure, o Site Recovery Deteta se o funcionamento em rede acelerado está ativado. Se é, depois de ativação pós-falha configura automaticamente o Site Recovery accelerated networking da VM do Azure, de réplica de destino para ambos [Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell#enable-accelerated-networking-on-existing-vms) e [Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms). [Saiba mais](azure-vm-disaster-recovery-with-accelerated-networking.md).

### <a name="automatic-updates-for-the-mobility-service-extension"></a>Atualizações automáticas para a extensão de serviço de mobilidade

Recuperação de site adicionado uma opção para as atualizações automáticas para a extensão de serviço de mobilidade. A extensão de serviço de mobilidade está instalada em cada VM do Azure replicadas pelo Site Recovery. Quando ativa a replicação, selecione se pretende permitir a recuperação de sites gerir atualizações para a extensão. As atualizações não requerem um reinício VM e não afetam os replicação. [Saiba mais](azure-to-azure-autoupdate.md).

### <a name="support-for-standard-ssd-disks"></a>Suporte para discos SSD standard

Azure introduzido [unidades de estado sólido padrão (SSD)](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd) discos para fornecer uma solução de armazenamento económico para aplicações, tais como servidores web que precisam de um desempenho consistente, mas que não tenham alto de IOPS de disco. Combinam estes elementos de SSD disks premium e standard HDD. Site Recovery fornece recuperação após desastre para VMs do Azure com o disco Standard SSD. Por predefinição, o tipo de disco é mantido após a ativação pós-falha para a região de destino.

### <a name="support-for-azure-storage-firewall"></a>Suporte para firewall de armazenamento do Azure

Pode proteger contas de armazenamento do Azure para um conjunto específico de redes, Ativando a regras de firewall para a conta. Configurar contas de armazenamento para negar o tráfego de redes internas e a internet por padrão, em seguida, conceder acesso ao tráfego de VNets específico. Recuperação de sites suporta a replicação para as VMs com discos não geridos nas contas de armazenamento firewa habilitado para uma região secundária. Na região de destino, para discos não geridos, pode selecionar as contas de armazenamento com firewalls ativadas. Também pode restringir o acesso à conta de armazenamento de cache ao restringir o acesso de rede para apenas rede no qual as VMs de origem estão localizados. Tenha em atenção que tem [permitir o acesso](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) para serviços Microsoft fidedignos.

## <a name="q3-2018"></a>2018 P3 

### <a name="linux-support"></a>Apoio Técnico para Linux

#### <a name="update-rollup-28"></a>O Update rollup 28

[Coletânea de 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) oferece uma atualização para os agentes de recuperação de Site e fornecedores. As atualizações adiciona suporte para Linux da seguinte forma:

- **Recuperação após desastre de VMs do Azure**: Red Hat Enterprise Linux 6.10; CentOS 6.10; VMs baseadas em Linux utilizar o estilo de partição de tabela (GPT) do GUID partição no modo de compatibilidade de BIOS legado são agora suportados.
- **Recuperação após desastre de servidores de VMs de VMware/físico para o Azure**: Red Hat Enterprise Linux 6.10; CentOS 6.10; VMs baseadas em Linux utilizar o estilo de partição de tabela (GPT) do GUID partição no modo de compatibilidade de BIOS legado são agora suportados.

#### <a name="update-rollup-27"></a>O Update rollup 27

[Coletânea de 28](https://support.microsoft.com/help/4460079/update-rollup-28-for-azure-site-recovery) oferece uma atualização para os agentes de recuperação de Site e fornecedores. As atualizações adiciona suporte para Linux da seguinte forma:

- **Recuperação após desastre de VMs do Azure**: Red Hat Enterprise Linux 7.5
- **Recuperação após desastre de servidores de VMs de VMware/físico para o Azure**: SUSE Linux Enterprise Server 12, Red Hat Enterprise Linux 7.5

### <a name="support-for-azure-vms-running-on-windows-server-2016"></a>Suporte para VMs do Azure em execução no Windows Server 2016

VMs do Azure em execução no Windows Server 2016 podem ser replicadas entre regiões do Azure com o Azure Site Recovery.

### <a name="support-for-azure-vms-running-storage-spaces-direct"></a>Suporte para VMs do Azure com espaços de armazenamento direto

[Espaços de armazenamento direto](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview) (disponível no Windows Server 2016 e posteriores) agrupa as unidades num agrupamento de armazenamento e, em seguida, usa a capacidade do conjunto para criar espaços de armazenamento. Espaços de armazenamento podem ser utilizados numa VM autónoma numa [cluster de convidados de VMs do Azure](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-in-vm) com armazenamento local em cada nó de cluster ou armazenamento partilhado no cluster.

## <a name="next-steps"></a>Passos Seguintes

Mantenha-se atualizado com nossas atualizações sobre o [Azure atualiza as](https://azure.microsoft.com/updates/?product=site-recovery) página.


