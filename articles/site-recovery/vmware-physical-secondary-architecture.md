---
title: "Arquitetura de replicação do servidor VMware/físico no Azure Site Recovery | Microsoft Docs"
description: "Este artigo fornece uma descrição geral da arquitetura de utilizada quando replicar no local as VMs VMware ou servidores físicos Windows/Linux para um site VMware secundário com o Azure Site Recovery e componentes"
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: 97a990aa3ed9043280888900d8fc7b604b6c22b5
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="vmware-vmphysical-server-to-vmware-replication-architecture"></a>Servidor VM de VMware/físico à arquitetura de replicação de VMware

Este artigo descreve a arquitetura e processos utilizados quando replicar, efetuar a ativação pós-falha e recuperar máquinas de virtuais de VMware no local (VMs) ou servidores físicos Windows/Linux para uma secundária VMware site a utilizar [do Azure Site Recovery](site-recovery-overview.md).


## <a name="architectural-components"></a>Componentes da arquitetura

**Área** | **Componente** | **Detalhes**
--- | --- | ---
**Azure** | Este cenário é implementado através de InMage Scout. | Para obter o InMage Scout, precisa de uma subscrição do Azure.<br/><br/> Depois de criar um cofre dos Serviços de Recuperação, transfere o InMage Scout e instala as atualizações mais recentes para configurar a implementação.
**Servidor de processos** | Localizado no site primário | O servidor de processos é implementado para lidar com a colocação em cache, a compressão e a otimização de dados.<br/><br/> Ele também faz a instalação por push do Agente Unified nas máquinas que pretende proteger.
**Servidor de configuração** | Localizado no site secundário | O servidor de configuração gere, configura e monitoriza a implementação, utilizando o site de gestão ou a consola do vContinuum.
**Servidor de vContinuum** | Opcional. Instalado na mesma localização que o servidor de configuração. | Fornece uma consola de gestão e monitorização do seu ambiente protegido.
**Servidor de destino mestre** | Localizado no site secundário | O servidor de destino mestre contém dados replicados. Recebe dados do servidor de processos, cria uma máquina de réplica no site secundário e detém os pontos de retenção de dados.<br/><br/> O número de servidores de destino principal de que precisa, depende do número de máquinas que estiver a proteger.<br/><br/> Se pretender que haja reativação para o site primário, precisa também que aí exista um servidor de destino principal. O Agente Unified é instalado neste servidor.
**VMware ESX/ESXi e servidor vCenter** |  As VMs são alojadas em anfitriões ESX/ESXi. Os anfitriões são geridos com um servidor vCenter | Precisa de uma infraestrutura do VMware para replicar VMs de VMware.
**VMs/servidores físicos** |  Agente Unificado instalado nas VMs VMware e servidores físicos que pretende replicar. | O agente atua como um fornecedor de comunicação entre todos os componentes.

### <a name="replication-process"></a>Processo de replicação

1. Configure servidores de componentes em cada site (configuração, processo, destino principal) e instale o agente Unified nas máquinas que pretende replicar.
2. Após a replicação inicial, o agente em cada máquina envia alterações de replicação delta para o servidor de processos.
3. O servidor de processos otimiza os dados e transfere-o para o servidor de destino principal no site secundário. O servidor de configuração gere o processo de replicação.

**Figura 6: Replicação de VMware para VMware**

![VMware para VMware](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="next-steps"></a>Passos Seguintes

[Configurar](vmware-physical-secondary-disaster-recovery.md) recuperação após desastre de VMs de VMware e servidores físicos para um site secundário.
