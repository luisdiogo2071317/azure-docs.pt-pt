---
title: "Matriz de suporte para replicação de VMs de VMware ou servidores físicos para um site VMware secundário com o Azure Site Recovery | Microsoft Docs"
description: "Resume o suporte para a replicação do servidor VMware/físico a um site secundário com o Azure Site Recovery"
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: raynew
ms.openlocfilehash: b51a4573ad7a8461b7261f08d94639d2030492d9
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-replication-of-vmware-vms-and-physical-servers-to-a-secondary-site"></a>Matriz de suporte para replicação de VMs de VMware e servidores físicos para um site secundário

Este artigo resume o que é suportado quando utiliza o [do Azure Site Recovery](site-recovery-overview.md) serviço para replicar VMs de VMware ou Windows/Linux servidores físicos para um site VMware secundário.

- Se pretender replicar VMs de VMware ou servidores físicos para o Azure, reveja [esta matriz de suporte](vmware-physical-azure-support-matrix.md).
- Se pretender replicar VMs Hyper-V para um site secundário, reveja [esta matriz de suporte](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Replicação de VMs de VMware no local e servidores físicos é fornecida pelo InMage Scout. InMage Scout está incluído na subscrição do serviço Azure Site Recovery.


## <a name="host-servers"></a>Servidores de anfitrião

**Sistema operativo** | **Detalhes**
--- | ---
servidor vCenter | vCenter 5.5, 6.0 e 6.5<br/><br/> Se executar 6.0 ou 6.5, tenha em atenção que apenas 5.5 funcionalidades são suportadas.


## <a name="replicated-vm-support"></a>Replicado suporte VM

A tabela seguinte resume suporte do sistema operativo para máquinas replicados com a recuperação de Site. Qualquer carga de trabalho pode estar em execução no sistema operativo suportado.

**Sistema operativo** | **Detalhes**
--- | ---
Windows Server | 64 bits do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 com, pelo menos, SP1.
Linux | Red Hat Enterprise Linux 6.7, 6.8, 6.9, 7.1, 7.2 <br/><br/> Centos 6.5, 6.6, 6.7, 6.8, 6.9, 7.0, 7.1, 7.2 <br/><br/> Oracle Enterprise Linux 6.4, 6.5, 6.8 com o kernel compatível do Red Hat ou Unbreakable Enterprise Kernel versão 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 11 SP4 


## <a name="linux-machine-storage"></a>Armazenamento de máquina do Linux

Apenas máquinas Linux com o armazenamento seguinte podem ser replicadas:

- Ficheiro de sistema (EXT3 ETX4, ReiserFS, XFS).
- Mapeador de software MultiPath-dispositivo.
- Gestor de volume (LVM2).
- Servidores físicos com o armazenamento de controlador do HP CCISS não são suportados.
- O sistema de ficheiros ReiserFS só é suportado em SUSE Linux Enterprise Server 11 SP3.

## <a name="network-configuration---hostguest-vm"></a>Configuração de rede - VM do anfitrião/convidado

**Configuração** | **Suportado**  
--- | --- 
Anfitrião - agrupamento NIC | Sim 
Host - VLAN | Sim 
Host - IPv4 | Sim 
Host - IPv6 | Não 
Convidado de VM - agrupamento NIC | Não
Convidado VM - IPv4 | Sim
VM - IPv6 convidado | Não
Endereço de IP estático de VM de Gues - Windows/Linux- | Sim
VM convidado - vários NICs | Sim


## <a name="storage"></a>Armazenamento

### <a name="host-storage"></a>Armazenamento de anfitrião

**Armazenamento (anfitrião)** | **Suportado** 
--- | --- 
NFS | Sim 
SMB 3.0 | N/A 
SAN (ISCSI) | Sim 
Caminho multi (o MPIO) | Sim 

### <a name="guest-or-physical-server-storage"></a>Armazenamento de servidor físico ou de convidado

**Configuração** | **Suportado** 
--- | --- 
VMDK | Sim 
VHD/VHDX | N/A 
Ger 2 VM | N/A 
Disco de cluster partilhado | Sim 
Disco encriptado | Não 
UEFI| Sim 
NFS | Não 
SMB 3.0 | Não 
RDM | Sim 
Disco > 1 TB | Sim 
Volume com o disco repartidos > 1 TB<br/><br/> LVM | Sim 
Espaços de armazenamento | Não 
Disco frequente Adicionar/remover | Sim 
Excluir o disco | Sim 
Caminho multi (o MPIO) | N/A 

## <a name="vaults"></a>cofres

**Ação** | **Suportado** 
--- | --- 
Mover cofres entre grupos de recursos (dentro ou entre subscrições) | Não 
Mover o armazenamento, rede, as VMs do Azure através de grupos de recursos (dentro ou entre subscrições) | Não 

## <a name="mobility-service-and-updates"></a>Serviço de mobilidade e atualizações

O serviço de mobilidade coordena a replicação entre servidores do VMware no local ou servidores físicos e o site secundário. Quando configurar a replicação, deve certificar-se de que tem a versão mais recente do serviço de mobilidade e outros componentes.

**Atualização** | **Detalhes** 
--- | --- 
Scout atualizações | [Saiba mais sobre e transferir](/vmware-physical-secondary-disaster-recovery.md#updates) as atualizações mais recentes do Scout | Atualizações Scout são cumulativas.
Atualizações de componentes | Scout atualizações incluem atualizações para todos os componentes, incluindo o servidor do RX, servidor de configuração, processo e servidores de destino mestre, servidores de vContinuum e servidores de origem que pretende proteger.<br/><br/> [Saiba mais](vmware-physical-secondary-disaster-recovery.md#download-and-install-component-updates).


## <a name="next-steps"></a>Passos Seguintes

Transferir o [Guia do utilizador InMage Scout](https://aka.ms/asr-scout-user-guide)

- [Replicar VMs de Hyper-V em nuvens VMM para um site secundário](tutorial-vmm-to-vmm.md)
- [Replicar VMs VMware e servidores físicos para um site secundário](tutorial-vmware-to-vmware.md)
