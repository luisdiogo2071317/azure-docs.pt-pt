---
title: Acerca do Azure Migrate | Microsoft Docs
description: Disponibiliza uma descrição geral do serviço do Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 10/23/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 1aeaff486ed9f7cb99ea0b95451fc765344847a1
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50086680"
---
# <a name="about-azure-migrate"></a>Acerca do Azure Migrate

O serviço do Azure Migrate avalia as cargas de trabalho no local para migração para o Azure. O serviço avalia a adequabilidade da migração de máquinas no local, faz o dimensionamento com base no desempenho e oferece estimativas de custos para executar máquinas no local no Azure. Se estiver a considerar migrações lift-and-shift ou estiver nas primeiras etapas da migração, este serviço é para si. Após a avaliação, pode utilizar serviços como o [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) e o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) para migrar as máquinas para o Azure.

## <a name="why-use-azure-migrate"></a>Porquê utilizar o Azure Migrate?

O Azure Migrate ajuda-o a:

- **Avaliar a preparação do Azure**: avaliar se as suas máquinas no local são adequadas para execução no Azure.
- **Obter recomendações de tamanho**: obtenha recomendações de tamanho para as VMs do Azure, com base no histórico de desempenho das VMs no local.
- **Estimar custos mensais**: obtenha custos estimados para executar máquinas no local no Azure.  
- **Migrar com confiança elevada**: visualize as dependências de máquinas no local para criar grupos de computadores que vai avaliar e migrar em conjunto.

## <a name="current-limitations"></a>Limitações atuais

- Pode avaliar máquinas virtuais (VMs) do VMware no local apenas para migração para VMs do Azure. As VMs do VMware têm de ser geridas pelo vCenter Server (versão 5.5, 6.0 ou 6.5).
- Se quiser avaliar Hyper-VMs e servidores físicos, utilize o [Planeador de Implementações do Azure Site Recovery](http://aka.ms/asr-dp-hyperv-doc) para Hyper-V e as nossas [ferramentas para parceiros](https://azure.microsoft.com/migration/partners/) para máquinas físicas.
- Pode detetar até 1500 VMs numa única deteção e até 1500 VMs num único projeto. Além disso, pode avaliar até 1500 VMs numa única avaliação.
- Se pretender encontrar um ambiente maior, pode dividir a deteção e criar vários projetos. [Saiba mais](how-to-scale-assessment.md). O Azure Migrate suporta até 20 projetos por subscrição.
- O Azure Migrate só suporta discos geridos para avaliação de migrações.
-  Só pode criar projetos do Azure Migrate na geografia Estados Unidos. No entanto, pode planear uma migração para qualquer localização de destino do Azure.
    - Os metadados detetados no ambiente no local são armazenados na região do projeto de migração.
    - Os metadados são armazenados numa das regiões na geografia: E.U.A. Centro-Oeste/E.U.A. Leste.
    - Se utilizar a visualização de dependência com uma área de trabalho do Log Analytics, é criada na mesma região que o projeto.


## <a name="what-do-i-need-to-pay-for"></a>O que vou pagar?

[Saiba mais](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre os preços do Azure Migrate.


## <a name="whats-in-an-assessment"></a>Novidades nas avaliações

As definições da avaliação podem ser personalizadas de acordo com as suas necessidades. As propriedades da avaliação estão resumidas na tabela abaixo.

**Propriedade** | **Detalhes**
--- | ---
**Localização de destino** | A localização do Azure para a qual pretende migrar.<br/><br/>Atualmente, o Azure Migrate suporta 30 regiões. [Veja as regiões](https://azure.microsoft.com/global-infrastructure/services/). Por predefinição, a região de destino está definida como E.U.A. Oeste 2.
**Tipo de armazenamento** | O tipo de discos que quer alocar no Azure. Esta propriedade é aplicável quando o critério de dimensionamento é **como no local**. Pode especificar o tipo de disco de destino como discos geridos premium (a predefinição) ou discos geridos standard. Para o dimensionamento com base no desempenho, a recomendação de tamanho do disco é feita automaticamente com base nos dados de desempenho das VMs.
**Critérios de dimensionamento** | O dimensionamento pode ter por base o **histórico de desempenho** das VMs no local ou **como no local** (a predefinição), sem ter em conta o histórico de desempenho.
**Oferta do Azure** | A [oferta do Azure](https://azure.microsoft.com/support/legal/offer-details/) em que está escrito. O Azure Migrate calcula o custo em conformidade.
**Benefício Híbrido do Azure** | Se tiver o software assurance e for elegível para o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/) com custos reduzidos.
**Instâncias Reservadas** |  Se tem [instâncias reservadas](https://azure.microsoft.com/pricing/reserved-vm-instances/) no Azure. O Azure Migrate calcula o custo em conformidade.
**Tempo de atividade de VM** | O período de tempo durante o qual as VMs vão ser executadas no Azure. Os cálculos de custo são feitos em conformidade.
**Escalão de preço** | O [escalão de preço (básico/standard)](../virtual-machines/windows/sizes-general.md) para as VMs de destino do Azure. Por exemplo, se estiver a planear a migração de um ambiente de produção, pode considerar o escalão standard, que oferece baixa latência às VMs, mas pode ser mais caro. Por outro lado, num ambiente de teste, pode utilizar o escalão básico, que tem latência mais alta e custos mais baixos. Por predefinição, é utilizado o escalão [standard](../virtual-machines/windows/sizes-general.md).
**Histórico de desempenho** | Por predefinição, o Azure Migrate avalia o desempenho das máquinas no local através do histórico de desempenho do último dia, com um valor de percentil de 95%.
**Série das VMs** | A série das VMs utilizada para os cálculos de tamanho. Por exemplo, se tiver um ambiente de produção que não pretende migrar para VMs da série A no Azure, poderá excluir a série A da lista ou da série. O dimensionamento tem por base apenas as séries selecionadas.   
**Fator de conforto** | O Azure Migrate considera uma memória intermédia (fator de conforto) durante a avaliação. Esta memória intermédia é aplicada em cima dos dados de utilização das VMs (CPU, memória, disco e rede). O fator de conforto dá conta de problemas como utilização sazonal, histórico de desempenho breve e prováveis aumentos na utilização futura.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização resulta, normalmente, numa VM de 2 núcleos. No entanto, com um fator de conforto de 2,0 x, o resultado é uma VM de 4 núcleos. A definição de conforto predefinida é 1,3x.


## <a name="how-does-azure-migrate-work"></a>Como funciona o Azure Migrate?

1.  Cria um projeto do Azure Migrate.
2.  O Azure Migrate utiliza uma VM no local, denominada aplicação recoletora, para detetar informações sobre as suas máquinas no local. Para criar a aplicação, transfira um ficheiro de configuração no formato Open Virtualization Appliance (.ova) e importe-o como VM no seu vCenter Server no local.
3. Ligue à VM a partir do VCenter Server e especifique uma palavra-passe nova para a mesma durante a ligação.
4. Execute o recoletor na VM para iniciar a deteção.
5. O recoletor utiliza os cmdlets do VMware PowerCLI para recolher os metadados da VM. A deteção é feita sem agentes e não instala nada nos anfitriões do VMware nem nas VMs. Os metadados recolhidos incluem informações da VM (núcleos, memória, discos, tamanhos de discos e adaptadores de rede). Também recolhe dados de desempenho das VMs, incluindo utilização da CPU e de memória, IOPS de disco, débito de disco (MBps) e saída de rede (MBps).
5.  Os metadados são enviados para o projeto do Azure Migrate. Pode vê-los no portal do Azure.
6.  Para efeitos da avaliação, agrupa as VMs detetadas em grupos. Por exemplo, pode agrupar VMs que executem a mesma aplicação. Para um agrupamento mais preciso, pode utilizar a visualização de dependências para ver as dependências de uma máquina específica ou para todas as máquinas de um grupo e refinar o grupo.
7.  Depois de um grupo ser definido, crie uma avaliação para o mesmo.
8.  Depois de concluída a avaliação, pode vê-la no portal ou transferi-la no formato Excel.

  ![Arquitetura do Azure Migrate](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Quais são os requisitos de porta?

A tabela resume as portas necessárias para as comunicações do Azure Migrate.

Componente | Comunica com |  Detalhes
--- | --- |---
Recoletor  | Serviço do Azure Migrate | O recoletor liga-se ao serviço através da porta SSL 443.
Recoletor | vCenter Server | Por predefinição, o recoletor liga-se ao vCenter Server na porta 443. Se o servidor escutar noutra porta, configure-a como porta de saída na VM do recoletor.
VM no local | Área de trabalho do Log Analytics | [TCP 443] | [O Microsoft Monitoring Agent (MMA)](../log-analytics/log-analytics-windows-agent.md) utiliza a porta TCP 443 para se ligar ao Log Analytics. Só precisa desta porta se estiver a utilizar a visualização de dependências, a qual precisa do agente MMA.


## <a name="what-happens-after-assessment"></a>O que acontece após a avaliação?

Depois de avaliar as máquinas no local, pode utilizar algumas ferramentas para realizar a migração:

- **Azure Site Recovery**: pode utilizar o Azure Site Recovery para migrar para o Azure. Para tal, tem de [preparar os componentes do Azure](../site-recovery/tutorial-prepare-azure.md) de que precisa, incluindo uma conta de armazenamento e uma rede virtual. No local, [prepare o ambiente do VMware](../site-recovery/vmware-azure-tutorial-prepare-on-premises.md). Quando estiver tudo preparado, configure e ative a replicação para o Azure e migre as VMs. [Saiba mais](../site-recovery/vmware-azure-tutorial.md).
- **Azure Database Migration**: se as máquinas no local estiverem a executar uma base de dados, como SQL Server, MySQL ou Oracle, pode utilizar o [Azure Database Migration Service](../dms/dms-overview.md) para migrá-las para o Azure.


## <a name="next-steps"></a>Passos seguintes

- [Siga o tutorial](tutorial-assessment-vmware.md) para criar uma avaliação para uma VM do VMware no local.
- [Veja as perguntas mais frequentes](resources-faq.md) sobre o Azure Migrate.
