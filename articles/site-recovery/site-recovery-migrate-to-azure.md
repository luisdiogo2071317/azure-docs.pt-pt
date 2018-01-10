---
title: "Sobre a migração no Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como migrar no local e as VMs do Azure utilizando o serviço do Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: 966d532a33626a8fcc3a3b93790d203aadfd81b4
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="about-migration"></a>Sobre a migração

Leia este artigo para uma descrição geral de como o [do Azure Site Recovery](site-recovery-overview.md) serviço ajuda-o a migração de máquinas. 

Eis o que pode migrar através do Site Recovery:

- **Migrar no local para o Azure**: Migrar VMs de Hyper-V no local, as VMs VMware e servidores físicos para o Azure. Após a migração, as cargas de trabalho em execução nas máquinas no local vão estar em execução em VMs do Azure. 
- **Migrar no Azure**: migre VMs do Azure entre regiões do Azure. 
- **Migrar AWS**: migre instâncias do Windows AWS para VMs IaaS do Azure. 


## <a name="what-do-we-mean-by-migration"></a>O que queremos dizer com migração?

Além de utilizar a recuperação de sites para a recuperação de desastre no local e as VMs do Azure, pode utilizar o serviço de recuperação de Site para os migrar. Qual é a diferença?

- Recuperação de desastres, replicar máquinas regulares para o Azure. Quando ocorre uma falha, pode falha as máquinas do site primário para o site secundário do Azure e aceder aos mesmos a partir daí. Quando o site principal esteja novamente disponível, falhar novamente a partir do Azure.
- Para a migração, replicar máquinas no local para o Azure, ou VMs do Azure para uma região secundária. Em seguida, pode falha a VM do site primário para o secundário e concluir o processo de migração. Não há nenhuma reativação pós-falha envolvida.  


## <a name="migration-scenarios"></a>Cenários de migração

**Cenário** | **Detalhes**
--- | ---
**Migrar no local para o Azure** | Pode migrar as VMs de VMware no local, VMs de Hyper-V e servidores físicos para o Azure. Para tal, conclua quase os mesmos passos como faria para recuperação após desastre completa. Basta não falhar máquinas novamente a partir do Azure para o site no local.
**Migrar entre regiões do Azure** | Pode migrar as VMs do Azure de uma região do Azure para outro. Depois de concluída a migração, pode configurar a recuperação após desastre para as VMs do Azure agora na região secundária para a qual migrou.
**Migrar o AWS para o Azure** | Pode migrar instâncias do AWS para VMs do Azure. Recuperação de site trata instâncias AWS, como servidores físicos para fins de migração. 

## <a name="next-steps"></a>Passos Seguintes

- [Migrar máquinas no local para o Azure](tutorial-migrate-on-premises-to-azure.md)
- [Migrar VMs de uma região do Azure para outra](tutorial-migrate-azure-to-azure.md)
- [Migrar o AWS para o Azure](tutorial-migrate-aws-to-azure.md)
