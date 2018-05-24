---
title: Acerca do Azure Migrate | Microsoft Docs
description: Disponibiliza uma descrição geral do serviço do Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 05/03/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 869d426bc76d725cead9bedc73800a20531bb9ed
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="about-azure-migrate"></a>Acerca do Azure Migrate

O serviço do Azure Migrate avalia as cargas de trabalho no local para migração para o Azure. O serviço avalia a adequabilidade de migração de máquinas no local, o dimensionamento com base no desempenho e oferece estimativas de custos para executar as suas máquinas no local no Azure. Se estiver a considerar migrações lift-and-shift ou estiver nas primeiras etapas da migração, este serviço é para si. Após a avaliação, pode utilizar serviços como o [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) e o [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) para migrar as máquinas para o Azure.

## <a name="why-use-azure-migrate"></a>Porquê utilizar o Azure Migrate?

O Azure Migrate ajuda-o a:

- **Avaliar a preparação do Azure**: avaliar se as suas máquinas no local são adequadas para execução no Azure.
- **Obter recomendações de tamanho**: obtenha recomendações de tamanho para as VMs do Azure, com base no histórico de desempenho das VMs no local.
- **Estimar custos mensais**: obtenha custos estimados para executar máquinas no local no Azure.  
- **Migrar com confiança elevada**: visualize as dependências de máquinas no local para criar grupos de computadores que vai avaliar e migrar em conjunto.

## <a name="current-limitations"></a>Limitações atuais

- Atualmente, pode avaliar máquinas virtuais (VMs) do VMware no local apenas para migração para VMs do Azure. As VMs do VMware têm de ser geridas pelo vCenter Server (versão 5.5, 6.0 ou 6.5).
- O suporte para Hyper-V está no nosso plano. Até lá, recomendamos que utilize o [Planeador de Implementações do Azure Site Recovery](http://aka.ms/asr-dp-hyperv-doc) para planear a migração de cargas de trabalho do Hyper-V.
- Pode detetar até 1500 VMs numa única deteção e até 1500 VMs num único projeto. Além disso, pode avaliar até 1500 VMs numa única avaliação.
- Só pode criar um projeto do Azure Migrate na região E.U.A. Centro-Oeste ou Leste. No entanto, esta limitação não afeta a possibilidade de planear a sua migração para outra localização de destino do Azure. A localização do projeto de migração é utilizada apenas para armazenar os metadados detetados a partir do ambiente no local.
- O Azure Migrate só suporta discos geridos para avaliação de migrações.


## <a name="what-do-i-need-to-pay-for"></a>O que vou pagar?

Saiba mais sobre os preços do Azure Migrate [aqui](https://azure.microsoft.com/en-in/pricing/details/azure-migrate/).


## <a name="whats-in-an-assessment"></a>Novidades nas avaliações

Uma avaliação ajuda-o a identificar a adequabilidade do Azure de VMs no local, obter recomendações de tamanho adequado e estimativas de custo para executar as VMs no Azure. As avaliações podem ser personalizadas com base nas necessidades, ao alterar as propriedades da avaliação. Seguem-se as propriedades que são consideradas durante a criação de uma avaliação.

**Propriedade** | **Detalhes**
--- | ---
**Localização de destino** | A localização do Azure para a qual pretende migrar.<br/><br/>Atualmente, o Azure Migrate suporta 30 regiões, incluindo Alemanha Central, Ásia Oriental, Canadá Central, Coreia Central, EUA Central, EUA Centro-Norte, EUA Centro-Oeste, EUA Centro-Sul, EUA Leste, EUA Leste 2, EUA Oeste, EUA Oeste 2, Europa do Norte, Europa Ocidental, Índia Central, Índia Ocidental, Leste da Austrália, Leste da China, Leste do Canadá, Leste do Japão, Nordeste da Alemanha, Norte da China, Oeste do Japão, Oeste do Reino Unido, Sudeste Asiático, Sudeste da Austrália, Sul da Coreia, Sul da Índia, Sul do Brasil, Sul do Reino Unido, US Gov – Arizona, US Gov – Texas e US Gov – Virginia. Por predefinição, a localização de destino está definida para E.U.A. Oeste 2.
**Redundância do armazenamento** | O tipo de [redundância de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-redundancy) que as VMs do Azure vão utilizar após a migração. A predefinição é o Armazenamento Localmente Redundante (LRS). Tenha em atenção que o Azure Migrate suporta apenas avaliações baseadas em discos geridos e os discos geridos suportam apenas LRS, por conseguinte, a propriedade tem atualmente apenas a opção de LRS.
**Critérios de dimensionamento** | Os critérios a serem utilizados pelo Azure Migrate para encontrar VMs de tamanho adequado para o Azure. Pode fazer o dimensionamento com base no *histórico de desempenho* das VMs no local ou dimensionar as VMs *como no local* para o Azure, sem considerar o histórico de desempenho. O valor predefinido é baseado no dimensionamento do desempenho.
**Planos de preços** | Para cálculos de custo, uma avaliação considera se tem garantia de software e é elegível para o [Benefício Híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Também considera as [Ofertas do Azure](https://azure.microsoft.com/support/legal/offer-details/) em que pode estar inscrito e permite-lhe indicar quaisquer descontos (%) específicos de subscrições, que pode acumular relativamente à oferta.
**Escalão de preço** | Pode especificar o [escalão de preço (Básico/Standard)](../virtual-machines/windows/sizes-general.md) para as VMs de destino do Azure. Por exemplo, se estiver a planear a migração de um ambiente de produção, deve considerar o escalão Standard, que oferece baixa latência às VMs, mas pode ser mais dispendioso. Por outro lado, se tiver um ambiente de Dev/Test, deve considerar o escalão Básico que tem VMs com latência superior e custos reduzidos. Por predefinição, é utilizado o escalão [Standard](../virtual-machines/windows/sizes-general.md).
**Histórico de desempenho** | Por predefinição, o Azure Migrate avalia o desempenho das máquinas no local através do histórico de desempenho do último dia, com um valor de percentil de 95%. Pode modificar estes valores nas propriedades de avaliação.
**Série das VMs** | Pode especificar a série das VMs que gostaria de considerar para redimensionamento. Por exemplo, se tiver um ambiente de produção que não pretende migrar para VMs da série A no Azure, poderá excluir a série A da lista ou da série e o redimensionamento será realizado apenas na série selecionada.  
**Fator de conforto** | O Azure Migrate considera uma memória intermédia (fator de conforto) durante a avaliação. Esta memória intermédia é aplicada em cima dos dados de utilização das VMs (CPU, memória, disco e rede). O fator de conforto dá conta de problemas como utilização sazonal, histórico de desempenho breve e prováveis aumentos na utilização futura.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização resulta, normalmente, numa VM de 2 núcleos. No entanto, com um fator de conforto de 2,0 x, o resultado é uma VM de 4 núcleos. A definição de conforto predefinida é 1,3x.


## <a name="how-does-azure-migrate-work"></a>Como funciona o Azure Migrate?

1.  Cria um projeto do Azure Migrate.
2.  O Azure Migrate utiliza uma VM no local, denominada aplicação recoletora, para detetar informações sobre as suas máquinas no local. Para criar a aplicação, transfira um ficheiro de configuração no formato Open Virtualization Appliance (.ova) e importe-o como VM no seu vCenter Server no local.
3.  Ao ligar-se à VM através da ligação de consola no vCenter Server, especifique uma nova palavra-passe para a VM durante a ligação e, em seguida, execute a aplicação de recoletor na VM para iniciar a deteção.
4.  O recoletor utiliza cmdlets do VMware PowerCLI para recolher os metadados da VM. A deteção é feita sem agentes e não instala nada nos anfitriões do VMware nem nas VMs. Os metadados recolhidos incluem informações da VM (núcleos, memória, discos, tamanhos de discos e adaptadores de rede). Também recolhe dados de desempenho das VMs, incluindo utilização da CPU e de memória, IOPS de disco, débito de disco (MBps) e saída de rede (MBps).
5.  Os metadados são enviados para o projeto do Azure Migrate. Pode vê-los no portal do Azure.
6.  Para efeitos da avaliação, agrupa as VMs detetadas em grupos. Por exemplo, pode agrupar VMs que executem a mesma aplicação. Para um agrupamento mais preciso, pode utilizar a visualização de dependências para ver as dependências de uma máquina específica ou para todas as máquinas de um grupo e refinar o grupo.
7.  Assim que o grupo é formado, crie uma avaliação para o grupo.
8.  Depois de concluída a avaliação, pode vê-la no portal ou transferi-la no formato Excel.



  ![Arquitetura do Azure Planner](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Quais são os requisitos de porta?

A tabela resume as portas necessárias para as comunicações do Azure Migrate.

|Componente          |Para comunicar com     |Porta necessária  |Razão   |
|-------------------|------------------------|---------------|---------|
|Recoletor          |Serviço do Azure Migrate   |TCP 443        |O recoletor liga-se ao serviço através da porta SSL 443|
|Recoletor          |vCenter Server          |Predefinição 9443   | Por predefinição, o recoletor liga-se ao vCenter Server na porta 9443. Se o servidore escutar noutra porta, esta deve ser configurada como porta de saída na VM do recoletor. |
|VM no local     | Área de trabalho do Log Analytics          |[TCP 443](../log-analytics/log-analytics-windows-agent.md) |O agente MMA utiliza a TCP 443 para ligar ao Log Analytics. Só precisa desta porta se estiver a utilizar a funcionalidade de visualização de dependências e a instalar o MMA (Microsoft Monitoring Agent). |



## <a name="what-happens-after-assessment"></a>O que acontece após a avaliação?

Depois de avaliar as máquinas no local para migração com o serviço do Azure Migrate, pode utilizar algumas ferramentas para realizá-la:

- **Azure Site Recovery**: pode utilizar o Azure Site Recovery para migrar para o Azure, da seguinte forma:
  - Prepare os recursos do Azure, incluindo uma subscrição do Azure, uma rede virtual do Azure e uma conta de armazenamento.
  - Prepare os seus servidores VMware no local para a migração. Verifique os requisitos de suporte do VMware para o Site Recovery, prepare os servidores VMware para deteção e prepare a instalação do serviço Site Recovery Mobility nas VMs que vai migrar.
  - Configure a migração. Vai configurar um cofre dos Serviços de Recuperação, as definições de migração de origem e de destino, uma política de replicação e vai ativar a replicação. Pode executar um teste de recuperação após desastre para confirmar que a migração das VMs para o Azure está a funcionar corretamente.
  - Execute uma ativação pós-falha para migrar máquinas no local para o Azure.
  - [Saiba mais](../site-recovery/tutorial-migrate-on-premises-to-azure.md) no tutorial de migração do Site Recovery.

- **Azure Database Migration**: se as suas máquinas no local estiverem a executar uma base de dados, como SQL Server, MySQL ou Oracle, pode utilizar o serviço do Azure Database Migration para migrá-las para o Azure. [Saiba mais](https://azure.microsoft.com/campaigns/database-migration/).



## <a name="next-steps"></a>Passos seguintes
[Siga o tutorial](tutorial-assessment-vmware.md) para criar uma avaliação para uma VM do VMware no local.
