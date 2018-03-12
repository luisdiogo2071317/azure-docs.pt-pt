---
title: "Matriz de suporte para a replicação de VMs de Hyper-V no VMM nuvens para um site secundário com o Azure Site Recovery | Microsoft Docs"
description: "Resume o suporte para replicação de VM de Hyper-V em nuvens VMM para um site secundário com o Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: raynew
ms.openlocfilehash: 767b0e76b73c082ddb75374f51700b85272f713e
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/09/2018
---
# <a name="support-matrix-for-replication-of-hyper-v-vms-to-a-secondary-site"></a>Matriz de suporte para a replicação de VMs de Hyper-V para um site secundário

Este artigo resume o que é suportado quando utiliza o [do Azure Site Recovery](site-recovery-overview.md) serviço para replicar VMs Hyper-V geridas em nuvens do System Center Virtual Machine Manager (VMM) para um site secundário. Se pretender replicar VMs Hyper-V para o Azure, reveja [esta matriz de suporte](hyper-v-azure-support-matrix.md).

> [!NOTE]
> Apenas pode replicar para um site secundário quando os anfitriões Hyper-V são geridos em nuvens VMM.

  

## <a name="host-servers"></a>Servidores de anfitrião

**Sistema operativo** | **Detalhes**
--- | ---
Windows Server 2012 R2 | Servidores tem de executar as atualizações mais recentes.
Windows Server 2016 |  Atualmente não são suportadas nuvens do VMM 2016 com uma mistura de anfitriões de R2 de 2012 e Windows Server 2016.<br/><br/> Atualmente não são suportadas implementações que atualizado a partir do System Center 2012 R2 VMM 2012 R2 para o System Center 2016.


## <a name="replicated-vm-support"></a>Replicado suporte VM

A tabela seguinte resume suporte do sistema operativo para máquinas replicados com a recuperação de Site. Qualquer carga de trabalho pode estar em execução no sistema operativo suportado.

**Versão do Windows** | **Hyper-V (com o VMM)**
--- | ---
Windows Server 2016 | Qualquer sistema operativo de convidado [suportadas pelo Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows) no Windows Server 2016 
Windows Server 2012 R2 | Qualquer sistema operativo de convidado [suportadas pelo Hyper-V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027%28v%3dws.11%29) no Windows Server 2012 R2

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
NFS | N/A
SMB 3.0 |  Sim
SAN (ISCSI) | Sim
Caminho multi (o MPIO) | Sim

### <a name="guest-or-physical-server-storage"></a>Armazenamento de servidor físico ou de convidado

**Configuração** | **Suportado**
--- | --- | 
VMDK |  N/A
VHD/VHDX | Sim (até 16 discos)
Ger 2 VM | Sim
Disco de cluster partilhado | Não
Disco encriptado | Não
UEFI| N/A
NFS | Não
SMB 3.0 | Não
RDM | N/A
Disco > 1 TB | Sim
Volume com o disco repartidos > 1 TB<br/><br/> LVM | Sim
Espaços de armazenamento | Sim
Disco frequente Adicionar/remover | Não
Excluir o disco | Sim
Caminho multi (o MPIO) | Sim

## <a name="vaults"></a>cofres

**Ação** | **Suportado**
--- | --- 
Mover cofres entre grupos de recursos (dentro ou entre subscrições) |  Não
Mover o armazenamento, rede, as VMs do Azure através de grupos de recursos (dentro ou entre subscrições) | Não

## <a name="azure-site-recovery-provider"></a>Fornecedor do Azure Site Recovery

O fornecedor coordena as comunicações entre servidores do VMM. 

**Latest** | **Atualizações**
--- | --- | --- | --- | ---
5.1.19 ([disponível a partir do portal](http://aka.ms/downloaddra) | [Funcionalidades e correções mais recentes](https://support.microsoft.com/kb/3155002)



## <a name="next-steps"></a>Passos Seguintes

[Replicar VMs de Hyper-V em nuvens VMM para um site secundário](tutorial-vmm-to-vmm.md)

