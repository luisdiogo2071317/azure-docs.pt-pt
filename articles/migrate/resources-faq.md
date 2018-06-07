---
title: Migrar do Azure - perguntas mais frequentes (FAQ) | Microsoft Docs
description: Endereços perguntas mais frequentes sobre migrar do Azure
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: article
ms.date: 06/06/2018
ms.author: snehaa
ms.openlocfilehash: b18d2cecfd7556ad3f05d0f63435d16bc29ebab1
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825267"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Migrar do Azure - perguntas mais frequentes (FAQ)

Este artigo inclui perguntas mais frequentes sobre como migrar do Azure. Se tiver quaisquer consultas adicionais depois de ler este artigo, publique-los no [fórum do Azure migrar](http://aka.ms/AzureMigrateForum).

## <a name="general"></a>Geral

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Como é migrar Azure diferente do Azure Site Recovery?

Migrar do Azure é um serviço de avaliação que o ajuda a detetar as cargas de trabalho no local e planeie a migração para o Azure. [O Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure), juntamente com a ser uma solução de recuperação após desastre, ajuda-o a migrar cargas de trabalho no local para as VMs IaaS do Azure. 

### <a name="how-is-azure-migrate-different-from-azure-site-recovery-deployment-planner"></a>Como é migrar Azure diferente do Planeador de implementação do Azure Site Recovery?

Migrar do Azure é uma ferramenta de planeamento da migração e Planeador de implementação do Azure Site Recovery é uma recuperação após desastre (DR) planeamento ferramenta.

**A migração de VMware para Azure**: Se pretender migrar as cargas de trabalho no local para o Azure, utilize migrar do Azure para planeamento da migração. Migrar do Azure avalia cargas de trabalho no local e fornece orientação, insights e mecanismos para ajudá-lo a migrar para o Azure. Assim que estiver pronto com o seu plano de migração, pode utilizar serviços como o Azure Site Recovery e o serviço de migração de base de dados do Azure para a migração das máquinas para o Azure.

**Migração do Hyper-V para o Azure**: Migrar do Azure atualmente suporta apenas a avaliação de máquinas virtuais VMware para a migração para o Azure. Suporte para o Hyper-V está no plano para a migração do Azure. Provisório, pode utilizar o planeador de implementação de ASR. Depois do suporte de Hyper-V está ativado no Azure migrar, pode utilizar o Azure migrar para o planeamento de migração de cargas de trabalho do Hyper-V.

**Recuperação após desastre do VMware/Hyper-V para o Azure**: Se pretender efetuar a recuperação de desastre (DR) no Azure através do Azure Site Recovery (ASR), utilize o planeador de implementação de ASR para DR planeamento. A ASR implementação Planner efetua uma avaliação profunda, a ASR específico do seu ambiente no local. Fornece recomendações que são necessários para ASR para operações de DR com êxito, tais como a replicação, ativação pós-falha das máquinas virtuais.  

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>A migrar do Azure necessita de servidor para detetar num ambiente VMware vCenter?

Sim, a migrar do Azure requer o vCenter Server para detetar num ambiente VMware. Não suporta a deteção de anfitriões ESXi que não são geridos pelo vCenter Server.

## <a name="discovery"></a>Deteção

### <a name="what-data-is-collected-by-azure-migrate"></a>Que dados são recolhidos por migrar do Azure?

Migrar do Azure suporta dois tipos de deteção, a deteção baseada no dispositivo e a deteção baseada no agente.
A deteção de aplicação recolhe metadados sobre as VMs no local, a lista completa de metadados recolhidos pelo dispositivo de está indicada abaixo:

**Dados de configuração da VM**
- Nome a apresentar VM (no vCenter)
- Caminho de inventário VM (anfitrião/cluster/pasta no vCenter)
- Endereço IP
- Endereço MAC
- Sistema operativo
- Número de núcleos, discos, NICs
- Tamanho da memória, tamanhos de disco

**Dados de desempenho da VM**
- Utilização da CPU
- Utilização de memória
- Para cada disco ligados à VM:
  - Débito de leitura de discos
  - Débito de escrita do disco
  - Operações por segundo de leitura de discos
  - Operações por segundo de escrita do disco
- Para cada adaptador de rede ligado à VM:
  - Rede no
  - Limite de rede

A deteção baseada no agente é uma opção disponível sobre a deteção baseada no dispositivo e ajuda os clientes [visualizar dependências](how-to-create-group-machine-dependencies.md) das VMs no local. Os agentes de dependência recolher detalhes, como, IP de FQDN, SO, endereço, endereço MAC, os processos em execução dentro da VM e as ligações de TCP de entrada/saída da VM. A deteção baseada no agente é opcional e pode optar por não instalar os agentes se não pretender visualizar as dependências das VMs.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Onde está os dados recolhidos armazenada e como tempo?

Os dados recolhidos pelo dispositivo de recoletor são armazenados na localização do Azure que especificou ao criar o projeto de migração. Os dados é guardada seguramente numa subscrição Microsoft e serão eliminados quando o utilizador o elimine o projeto do Azure migrar.

Para visualização de dependência, se instalar agentes nas VMs, os dados recolhidos pelos agentes de dependência são armazenados nos E.U.A. numa área de trabalho do OMS criada na subscrição do utilizador. Estes dados são eliminados depois do utilizador elimina a área de trabalho do OMS no respetiva ou a subscrição. [Saiba mais](https://docs.microsoft.com/azure/migrate/concepts-dependency-visualization).

### <a name="how-does-the-collector-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Forma como o recoletor comunicam com o vCenter Server e o serviço Azure migrar?

A aplicação de recoletor liga ao vCenter Server (porta 443) utilizando as credenciais fornecidas pelo utilizador no dispositivo de. Consulta o vCenter Server utilizando o VMware PowerCLI para recolher os metadados sobre as VMs gerido pelo vCenter Server. Recolhe os dados de configuração sobre as VMs (núcleos, memória, discos, etc. da NIC), bem como o histórico de desempenho de cada VM para o último mês de um do vCenter Server. Os metadados recolhidos, em seguida, é enviado para o serviço Azure migrar (através da internet através de https) para avaliação. [Saiba mais](concepts-collector.md)

### <a name="is-the-data-encrypted-at-rest-and-while-in-transit"></a>É os dados são encriptados em descanso e em trânsito?

Sim, os dados recolhidos são encriptados em descanso tanto em trânsito. Os metadados recolhidos pelo dispositivo de forma segura é enviado para o serviço Azure migrar através da internet através de https. Os metadados recolhidos são armazenados na [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest) e na [blob storage do Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) numa subscrição do Microsoft e são encriptados em pausa.

Os dados recolhidos pelos agentes de dependência também é encriptado em trânsito (canal seguro https) e estão armazenados numa área de trabalho de análise de registos na subscrição do utilizador. Também são encriptado em descanso.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Como podem a detetar um ambiente de multi-inquilino no Azure migrar?

Se tiver um ambiente que é partilhado entre inquilinos e não pretende detetar as VMs de um inquilino na subscrição outro inquilino, pode utilizar o campo de âmbito no dispositivo de recoletor para definir o âmbito de deteção. Se os inquilinos estão a partilhar anfitriões, criar uma credencial que tem acesso só de leitura para apenas as VMs que pertencem ao inquilino específico e, em seguida, utilize esta credencial no dispositivo de recoletor e especifique o âmbito como o anfitrião para efetuar a deteção. Em alternativa, também pode criar pastas no vCenter Server (Digamos Pasta1 para tenant1 e folder2 para tenant2), sob o anfitrião partilhado, mover as VMs para tenant1 para Pasta1 e tenant2 para folder2 e, em seguida, definir o âmbito de deteções no recoletor em conformidade ao especificar a pasta adequada.

### <a name="how-many-virtual-machines-can-be-discovered-in-a-single-migration-project"></a>Número de máquinas virtuais podem ser detetado num projeto único migração?

Pode detetar máquinas virtuais de 1500 num projeto migração único. Se tiver máquinas mais no seu ambiente no local, [mais](how-to-scale-assessment.md) sobre como pode detetar um ambiente grande no Azure migrar.

## <a name="dependency-visualization"></a>Visualização de dependência

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>É necessário pagar para utilizar a funcionalidade de visualização de dependência?

O Azure Migrate está disponível sem custos adicionais. Saiba mais sobre os preços do Azure Migrate [aqui](https://azure.microsoft.com/pricing/details/azure-migrate/).

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Pode utilizar uma área de trabalho existente para visualização de dependência?

Migrar do Azure não suporta a utilização de uma área de trabalho existente para visualização de dependência, no entanto, o Microsoft Monitoring Agent (MMA) suporta multi homing e permite-lhe enviar dados para várias áreas de trabalho. Por isso, se já tiver os agentes implementados e configurados para uma área de trabalho, pode tirar partido multi homing no agente MMA e configurá-lo para a área de trabalho de migração do Azure (para além da área de trabalho existente) e torná-lo a trabalhar. [Aqui](https://blogs.technet.microsoft.com/msoms/2016/05/26/oms-log-analytics-agent-multi-homing-support/) é um blogue na forma como pode ativar a multi homing num agente MMA.

## <a name="next-steps"></a>Passos Seguintes

- Leia o [descrição geral da migração do Azure](migrate-overview.md)
- Saiba como pode [detetar e avaliar](tutorial-assessment-vmware.md) num ambiente VMware
