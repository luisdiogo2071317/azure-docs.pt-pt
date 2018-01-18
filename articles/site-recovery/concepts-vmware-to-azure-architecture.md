---
title: "VMware para a arquitetura da replicação do Azure no Azure Site Recovery | Microsoft Docs"
description: "Este artigo fornece uma descrição geral da arquitetura de utilizada quando replicar VMs de VMware no local para o Azure com o serviço do Azure Site Recovery e componentes"
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: raynew
ms.openlocfilehash: 7999f23d167c6e8a7bcaf3a817e0cd2e80a1d649
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware para a arquitetura da replicação do Azure

Este artigo descreve a arquitetura e processos utilizados quando replicar, efetuar a ativação pós-falha e recuperar máquinas virtuais VMware (VMs) entre um site de VMware no local e o Azure, utilizando o [do Azure Site Recovery](site-recovery-overview.md) serviço.


## <a name="architectural-components"></a>Componentes da arquitetura

A seguinte tabela e o gráfico fornecem uma vista detalhada dos componentes utilizados para replicação de VMware no Azure.  

**Componente** | **Requisito** | **Detalhes**
--- | --- | ---
**Azure** | Uma subscrição do Azure, uma conta de armazenamento do Azure e uma rede do Azure. | Os dados replicados de VMs no local são armazenados na conta de armazenamento. As VMs do Azure são criadas com os dados replicados quando executar a uma falha ao longo no local para o Azure. As VMs do Azure ligam-se à rede virtual do Azure quando são criadas.
**Máquina do servidor de configuração** | Um único computador local. Recomendamos a que executá-la como uma VM do VMware que pode implementar a partir de um modelo transferido do OVF.<br/><br/> A máquina executa todos os componentes do Site Recovery no local, incluindo o servidor de configuração, o servidor de processos e o servidor de destino principal. | **Servidor de configuração**: coordena as comunicações entre no local e o Azure e gere a replicação de dados.<br/><br/> **Servidor de processos**: instalado por predefinição no servidor de configuração. Recebe dados de replicação, otimiza-as com colocação em cache, compressão e encriptação e envia-a para o armazenamento do Azure. O servidor de processos também instala o serviço de mobilidade em VMs que pretende replicar, e efetua a descoberta automática de máquinas no local. À medida que cresça a implementação, pode adicionar servidores de processos adicionais, em separado para processar maiores volumes de tráfego de replicação.<br/><br/>  **Servidor de destino mestre**: instalado por predefinição no servidor de configuração. -Processa dados de replicação durante a reativação pós-falha a partir do Azure. Para grandes implementações, pode adicionar um servidor de destino mestre adicionais, em separado para reativação pós-falha.
**Servidores de VMware** | VMs de VMware alojadas em servidores de ESXi do vSphere no local. Recomendamos que um servidor vCenter para gerir os anfitriões. | Durante a implementação da recuperação de sites, adicione servidores VMware para o Cofre dos serviços de recuperação.
**Máquinas replicadas** | O serviço de mobilidade está instalado em cada VM de VMware, replicar. | Recomendamos a que permitir a instalação automática do servidor de processos. Em alternativa pode instalar manualmente o serviço ou utilize um método de implementação automática, tais como o System Center Configuration Manager.

**Arquitetura de VMware para o Azure**

![Componentes](./media/concepts-vmware-to-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Processo de replicação

1.  Preparar os recursos do Azure e componentes no local.
2.  No cofre dos serviços de recuperação, especifique as definições de replicação de origem. Como parte deste processo, configurar o servidor de configuração no local. Para implementar este servidor como uma VM de VMware, pode transferir um modelo OVF preparado e importá-lo para VMware para criar a VM.
3. Especifique as definições de replicação de destino, criar uma política de replicação e ativar a replicação para as VMs de VMware.
4.  Replicar máquinas em conformidade com a política de replicação e uma cópia inicial dos dados VM é replicado para o armazenamento do Azure.
5.  Após a conclusão da replicação inicial, começa a replicação das alterações de delta para o Azure. As alterações registadas relativas a uma máquina são guardadas num ficheiro .hrl.
    - Máquinas comunicam com o servidor de configuração na porta HTTPS 443 entrada para a gestão de replicação.
    - As máquinas enviam dados de replicação para o servidor de processos na porta HTTPS 9443 entrada (podem ser modificadas).
    - O servidor de configuração orquestra a gestão da replicação com o Azure através da porta HTTPS 443 de saída.
    - O servidor de processos recebe os dados das máquinas de origem, otimiza-os, encripta-os e envia-os para o armazenamento do Azure através da porta 443 de saída.
    - Se ativar a consistência multi VM, as máquinas do grupo de replicação comunicam entre si através da porta 20004. A consistência multi-VM é utilizada se agrupar várias máquinas em grupos de replicação que partilhem pontos de consistência de falhas e pontos de consistência de aplicação quando é efetuada a ativação pós-falha dos mesmos. Isto é útil se as máquinas estiverem a executar a mesma carga de trabalho e tiverem de ser consistentes.
6.  O tráfego é replicado para o armazenamento do Azure pontos finais públicos, através da internet. Em alternativa, pode utilizar zure ExpressRoute [peering público](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). A replicação de tráfego através de uma rede de VPNs a partir de um site no local para o Azure não é suportada.


**VMware para o processo de replicação do Azure**

![Processo de replicação](./media/concepts-vmware-to-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Processo de ativação pós-falha e de reativação pós-falha

Depois de configurar a replicação e tiver de executar um exercício de recuperação após desastre (ativação pós-falha de teste) para verificar que tudo está a funcionar conforme esperado, pode executar a ativação pós-falha e reativação pós-falha conforme necessário para.

1. Pode efetuar a ativação pós-falha de um único computador ou, criar planos de recuperação, para efetuar a ativação pós-falha de várias VMs.
2. Quando executar uma ativação pós-falha, as VMs do Azure são criadas a partir dos dados replicados no armazenamento do Azure.
3. Após a acionar a ativação pós-falha inicial, a consolidação-o para iniciar a aceder a carga de trabalho a partir da VM do Azure.

Quando o site no local primário estiver novamente disponível, pode fazer a reativação pós-falha.
1. Terá de configurar uma infraestrutura de reativação pós-falha, incluindo:
    - **Servidor de processo temporário no Azure**: falhar novamente a partir do Azure, configurar uma VM do Azure para atuar como um servidor de processos, para lidar com a replicação a partir do Azure. É possível eliminar esta VM após a conclusão da reativação pós-falha.
    - **Ligação VPN**: para falhar novamente, precisa de uma ligação VPN (ou Azure ExpressRoute) da rede do Azure para o site no local.
    - **Servidor de destino principal separado**: por predefinição, o servidor de destino principal foi instalado com o servidor de configuração, em que a VM, de VMware no local processa a reativação pós-falha. No entanto, se precisar de falhar novamente grandes volumes de tráfego, deve configurar um servidor de destino principal independente no local para esta finalidade.
    - **Política de reativação pós-falha**: para replicar de novo para o site no local, precisa de uma política de reativação pós-falha. Isto foi criado automaticamente quando criou a política de replicação no local para o Azure.
2. Depois dos componentes estão em vigor, a reativação pós-falha ocorre em três fases:
    - Fase 1: Voltar a proteger as VMs do Azure para que o se replicar a partir do Azure para as VMs de VMware no local.
    - Fase 2: Executar uma ativação pós-falha para o site no local.
    - Fase 3: Depois de cargas de trabalho falharam novamente, reativar a replicação para as VMs no local.

**Reativação pós-falha da VMware a partir do Azure**

![Reativação pós-falha](./media/concepts-vmware-to-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Passos Seguintes

Siga [neste tutorial](tutorial-vmware-to-azure.md) para ativar o VMware para a replicação do Azure.
