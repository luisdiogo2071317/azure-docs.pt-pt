---
title: Matriz de suporte para replicar VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery | Microsoft Docs
description: Resume os sistemas operativos suportados e componentes para replicar VMs de VMware e o servidor físico para o Azure utilizando o Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/20/2018
ms.author: raynew
ms.openlocfilehash: 6f2f28b36fdb3751a469d66f242f9fa2119f9ae8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Matriz de suporte para VMware e replicação do servidor físico para o Azure

Este artigo resume componentes suportados e as definições de recuperação após desastre de VMs de VMware no Azure utilizando [do Azure Site Recovery](site-recovery-overview.md).

## <a name="supported-scenarios"></a>Cenários suportados

**Cenário** | **Detalhes**
--- | ---
VMs VMware | Pode efetuar a recuperação de desastre para o Azure para as VMs de VMware no local. Pode implementar este cenário no portal do Azure ou através do PowerShell.
Servidores físicos | Pode efetuar a recuperação após desastre para o Azure para servidores físicos do Windows/Linux no local. Pode implementar este cenário no portal do Azure.

## <a name="on-premises-virtualization-servers"></a>Servidores de virtualização no local

**Servidor** | **Requisitos** | **Detalhes**
--- | --- | ---
VMware | o vCenter Server 6.5, 6.0, ou 5.5 ou vSphere 6.5, 6.0 ou 5.5 | Recomendamos que utilize um servidor vCenter.
Físico | N/A


## <a name="replicated-machines"></a>Máquinas replicadas

A tabela seguinte resume a replicação de suporte para as VMs VMware e servidores físicos. Recuperação de sites suporta a replicação de qualquer carga de trabalho em execução numa máquina com um sistema operativo suportado.

**Componente** | **Detalhes**
--- | ---
Definições da máquina | As máquinas que replicam para o Azure tem de cumprir [requisitos do Azure](#azure-vm-requirements).
Sistema operativo Windows | 64 bits do Windows Server 2016 (Server Core, o servidor com experiência de ambiente de trabalho), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 com, pelo menos, SP1. Não é suportado o Windows Server de Nano de 2016.
Sistema operativo Linux | Red Hat Enterprise Linux: 5.2 para 5.11, 6.1 para 6.9, 7.0 e 7,4 <br/><br/>CentOS: 5.2 para 5.11, 6.1 para 6.9, 7.0 e 7,4 <br/><br/>Servidor Ubuntu 14.04 LTS[ (versões de kernel suportado)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Servidor Ubuntu 16.04 LTS[ (versões de kernel suportado)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7/Debian 8<br/><br/>Oracle Enterprise Linux 6.4, 6.5 com o kernel compatível do Red Hat ou Unbreakable Enterprise Kernel versão 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3, SUSE Linux Enterprise Server 11 SP4 <br/><br/>Atualizar máquinas replicadas do SP3 para SP4 não é suportada. Para atualizar, desative a replicação e ativá-la novamente após a atualização.

>[!NOTE]
>
> - Sobre as distribuições do Linux, apenas os cotações kernels que fazem parte da versão/atualização de versão secundária distribuição são suportados.
>
> - Atualizar máquinas protegidas em Linux principais versões de distribuição não é suportada. Para atualizar, desative a replicação, atualize o sistema operativo e, em seguida, ativar a replicação novamente.
>

### <a name="ubuntu-kernel-versions"></a>Versões do Ubuntu kernel


**Versão suportada** | **Versão de serviço de mobilidade de recuperação de sites do Azure** | **Versão de kernel** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic para 3.13.0-121-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic para 3.13.0-128-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic para 3.13.0-132-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic para 3.13.0-137-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-104-generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic para 4.4.0-81-generic,<br/>4.8.0-34-Generic para 4.8.0-56-generic,<br/>4.10.0-14-Generic para 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic para 4.4.0-91-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic para 4.4.0-96-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic para 4.4.0-104-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic |

## <a name="linux-file-systemsguest-storage"></a>Armazenamento de convidado/sistemas de ficheiros do Linux

**Componente** | **Suportado**
--- | ---
sistemas de ficheiros | ext3, ext4, ReiserFS (Suse Linux Enterprise Server apenas), XFS.
Gestor de volumes | LVM2.
Software MultiPath | Mapeador de dispositivo.
Dispositivos de armazenamento Paravirtualized | Os dispositivos exportados por controladores paravirtualizados não são suportados.
Dispositivos de e/s de fila multi bloco | Não suportado.
Servidores físicos com o controlador de armazenamento HP CCISS | Não suportado.
Diretórios | Estes diretórios (se configurar como separados partições /-sistemas de ficheiros) todas têm de estar no mesmo disco do SO no servidor de origem: / (raiz), /boot /usr, /usr/local, /var, /etc/hosts.</br></br> /Boot deve estar na partição de disco e não pode ser um volume LVM.<br/><br/>
Requisitos de espaço livre| 2 GB na partição /root <br/><br/> 250 MB na pasta de instalação
XFSv5 | Funcionalidades de XFSv5 nos sistemas de ficheiros XFS, tais como a soma de verificação de metadados, são suportadas do serviço de mobilidade versão 9.10 onward. Utilize o utilitário de xfs_info para verificar o superblock XFS para a partição. Se ftype estiver definido como 1, em seguida, XFSv5 funcionalidades estão em utilização.



## <a name="network"></a>Rede

**Componente** | **Suportado**
--- | ---
Agrupamento NIC de rede do anfitrião | Suportada para VMware VMs. <br/><br/>Não é suportada para a replicação de máquina física.
Rede de anfitrião VLAN | Sim.
Rede de anfitrião IPv4 | Sim.
Rede do anfitrião IPv6 | Não.
Rede de convidado/servidor agrupamento NIC | Não.
Rede de convidado/servidor IPv4 | Sim.
Rede de convidado/servidor IPv6 | Não.
IP estático da rede do convidado/servidor (Windows) | Sim.
IP estático da rede do convidado/servidor (Linux) | Sim. <br/><br/>As VMs estão configuradas para utilizar DHCP na reativação pós-falha.
Vários NICs de rede de convidado/servidor | Sim.


## <a name="azure-vm-network-after-failover"></a>Rede VM do Azure (após a ativação pós-falha)

**Componente** | **Suportado**
--- | ---
Azure ExpressRoute | Sim
ILB | Sim
ELB | Sim
Traffic Manager do Azure | Sim
Vários NICs | Sim
Endereço IP reservado | Sim
IPv4 | Sim
Manter o endereço IP de origem | Sim
Pontos finais de serviço de rede Virtual do Azure<br/><br/> (Firewalls de armazenamento do azure e redes virtuais) | Não

## <a name="storage"></a>Armazenamento
**Componente** | **Suportado**
--- | ---
Host NFS | Sim para VMware<br/><br/> Não para servidores físicos
SAN (ISCSI) no anfitrião | Sim
Multipath de anfitrião (MPIO) | Sim, testada com Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM para CLARiiON
Convidados/servidor VMDK | Sim
Convidados/servidor EFI/UEFI| Parcial (migração do Azure para o Windows Server 2012 e posteriores em máquinas virtuais VMware apenas) </br></br> Consulte a nota no fim da tabela
Disco de cluster partilhado convidado/servidor | Não
Disco encriptado convidado/servidor | Não
Convidados/servidor NFS | Não
Convidados/servidor SMB 3.0 | Não
Convidados/servidor RDM | Sim<br/><br/> N/d para servidores físicos
Convidados/servidor disco > 1 TB | Sim<br/><br/>4,095 GB
Disco de convidado/servidor com o tamanho de sector físico lógicos e 4 k de 4K | Sim
Disco de convidado/servidor com lógico de 4K e tamanho de setor físico de 512 bytes | Sim
Volume de convidado/servidor com o disco repartido > 4 TB <br><br/>Gestão de volumes lógica (LVM)| Sim
Convidados/servidor - os espaços de armazenamento | Não
Disco de acesso frequente adição/remoção de convidado/servidor | Não
Convidados/servidor - disco de exclusão | Sim
Multipath de convidado/servidor (MPIO) | N/A

> [!NOTE]
> UEFI arrancar em máquinas virtuais VMware com o Windows Server 2012 ou posterior podem ser migrados para o Azure. As seguintes restrições aplicam-se:

> - Apenas uma migração para o Azure é suportada. Não é suportada a reativação pós-falha para o site de VMware no local.
> - O servidor não deve ter mais de quatro partições no disco do SO.
> - Requer a versão do serviço de mobilidade 9.13 ou posterior.
> - Não é suportado para servidores físicos.

## <a name="azure-storage"></a>Storage do Azure

**Componente** | **Suportado**
--- | ---
Armazenamento localmente redundante | Sim
Armazenamento georredundante | Sim
Armazenamento georredundante com acesso de leitura | Sim
Armazenamento de acesso esporádico | Não
Armazenamento frequente| Não
Blobs de bloco | Não
Encriptação de Inativos (encriptação do serviço de armazenamento)| Sim
Armazenamento Premium | Sim
Serviço de importação/exportação | Não
Pontos finais de serviço de rede virtuais<br/><br/> Firewalls de armazenamento e redes virtuais configuradas na conta de armazenamento de cache/armazenamento de destino (utilizada para armazenar dados de replicação) | Não
Contas de armazenamento de v2 para fins gerais (camadas de acesso frequente e esporádico) | Não

## <a name="azure-compute"></a>Computação do Azure

**Funcionalidade** | **Suportado**
--- | ---
Conjuntos de disponibilidade | Sim
HUB | Sim
Managed disks | Sim

## <a name="azure-vm-requirements"></a>Requisitos de VM do Azure

VMs no local que replicam para o Azure tem de cumprir os requisitos de VM do Azure resumidos nesta tabela. Quando a recuperação de sites é executada uma verificação de pré-requisitos, irá falhar se alguns dos requisitos não forem cumpridas.

**Componente** | **Requisitos** | **Detalhes**
--- | --- | ---
Sistema operativo convidado | Certifique-se [sistemas operativos suportados](#replicated machines). | Falha na verificação se não suportado. 
Arquitetura do sistema operativo convidado | 64 bits. | Falha na verificação se não suportado. 
Tamanho do disco do sistema operativo | Até 2.048 GB. | Falha na verificação se não suportado. 
Contagem de discos do sistema operativo | 1 | Falha na verificação se não suportado.  
Contagem de discos de dados | 64 ou menos. | Falha na verificação se não suportado.  
Tamanho do disco de dados | 4,095 GB | Falha na verificação se não suportado. 
Placas de rede | São suportados vários adaptadores. | 
VHD partilhado | Não suportado. | Falha na verificação se não suportado. 
Disco FC | Não suportado. | Falha na verificação se não suportado. 
BitLocker | Não suportado. | O BitLocker tem de ser desativado antes de ativar a replicação para uma máquina. | 
o nome da VM | Entre 1 e 63 carateres.<br/><br/> Limitado a letras, números e hífenes.<br/><br/> O nome do computador tem de começar e terminar com uma letra ou número. |  Atualize o valor nas propriedades da máquina no Site Recovery.


## <a name="vault-tasks"></a>Tarefas do Cofre

**Ação** | **Suportado**
--- | ---
Mover o Cofre entre grupos de recursos<br/><br/> Dentro e entre subscrições | Não
Mover o armazenamento, rede, as VMs do Azure através de grupos de recursos<br/><br/> Dentro e entre subscrições | Não


## <a name="mobility-service"></a>Serviço de mobilidade

**Nome** | **Descrição** | **versão mais recente** | **Detalhes**
--- | --- | --- | --- | ---
Configuração de unificada de recuperação de sites do Azure | Coordena a comunicação entre servidores do VMware no local e o Azure <br/><br/> Instalado nos servidores do VMware no local | 9.12.4653.1 (disponível a partir do portal) | [Funcionalidades e correções mais recentes](https://aka.ms/latest_asr_updates)
Serviço de mobilidade | Coordena a replicação entre servidores de servidores/físico de VMware no local e o site secundário/Azure<br/><br/> Instalado na VM de VMware ou servidores físicos que pretende replicar | 9.12.4653.1 (disponível a partir do portal) | [Funcionalidades e correções mais recentes](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Passos Seguintes
[Saiba como](tutorial-prepare-azure.md) preparar do Azure para a recuperação após desastre de VMs de VMware.
