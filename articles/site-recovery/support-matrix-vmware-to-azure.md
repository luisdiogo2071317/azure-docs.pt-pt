---
title: "Matriz de suporte do Azure Site Recovery para replicar VMs de VMware e servidores físicos para o Azure | Microsoft Docs"
description: "Resume os sistemas operativos suportados e componentes para replicar VMware VMs para o Azure com o serviço do Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 01/11/2018
ms.author: raynew
ms.openlocfilehash: ead133318d8660e8b8f4b3e9c5dddb6d75878b19
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Matriz de suporte para VMware e replicação do servidor físico para o Azure


Este artigo resume componentes suportados e as definições de recuperação após desastre de VMs de VMware para o Azure, utilizando o [do Azure Site Recovery](site-recovery-overview.md) serviço.


## <a name="supported-scenarios"></a>Cenários suportados

**Cenário** | **Detalhes** 
--- | --- 
**VMs de VMware** | Pode efetuar a recuperação de desastre para o Azure para as VMs de VMware no local. Pode implementar este cenário no portal do Azure ou através do PowerShell.
**Servidores físicos** | Pode efetuar a recuperação após desastre para o Azure para servidores físicos do Windows/Linux no local. Pode implementar este cenário no portal do Azure.

## <a name="on-premises-virtualization-servers"></a>Servidores de virtualização no local

**Servidor** | **Requisitos** | **Detalhes**
--- | --- | ---
**VMware** | vCenter Server 6.5, 6.0 ou 5.5 ou vSphere 6.5, 6.0, 5.5 | Recomendamos que utilize um servidor vCenter
**Servidores físicos** | ND


## <a name="replicated-machines"></a>Máquinas replicadas

A tabela seguinte resume o suporte de replicação para máquinas. Recuperação de sites suporta a replicação de qualquer carga de trabalho em execução numa máquina com um sistema operativo suportado.

**Componente** | **Detalhes**
--- | ---
Configuração da máquina | As máquinas que replicam para o Azure tem de cumprir [requisitos do Azure](#failed-over-azure-vm-requirements).
Sistema operativo da máquina (Windows) | 64 bits do Windows Server 2016 (Server Core, o servidor com experiência de ambiente de trabalho)\*, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 com, pelo menos, SP1
Sistema operativo da máquina (Linux) | Red Hat Enterprise Linux: 5.2 para 5.11, 6.1 para 6.9, 7.0 e 7,4 <br/><br/>CentOS: 5.2 para 5.11, 6.1 para 6.9, 7.0 e 7,4 <br/><br/>Servidor Ubuntu 14.04 LTS[ (versões de kernel suportado)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Servidor Ubuntu 16.04 LTS[ (versões de kernel suportado)](#supported-ubuntu-kernel-versions-for-vmwarephysical-servers)<br/><br/>Debian 7 <br/><br/>Debian 8<br/><br/>Oracle Enterprise Linux 6.4, 6.5 com o kernel compatível do Red Hat ou Unbreakable Enterprise Kernel versão 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3 <br/><br/>SUSE Linux Enterprise Server 11 SP4 <br/>(Não é suportada a atualização de replicar máquinas do SLES 11 SP3 para SLES 11 SP4. Se uma máquina replicada tiver sido atualizada do SLES 11SP3 para SLES 11 SP4, terá de desativar a replicação e proteger a máquina post novamente a atualização.)

>[!NOTE]
>
> - Sobre as distribuições do Linux, são suportados apenas os cotações kernels que fazem parte da versão secundária versão/atualização da distribuição.
>
> - Atualizações entre versões principais de uma distribuição Linux num Azure Site Recovery proteger a máquina virtual VMware ou servidor físico não é suportado. Ao atualizar o sistema operativo em versões principais (por exemplo 6.* CentOS para CentOS 7.*), desative a replicação da máquina, atualize o sistema operativo na máquina e, em seguida, ativar a replicação novamente.
>

### <a name="ubuntu-kernel-versions"></a>Versões do Ubuntu kernel


**Versão suportada** | **Versão do serviço de mobilidade** | **Versão de kernel** |
--- | --- | --- |
14.04 LTS | 9.10 | 3.13.0-24-Generic para 3.13.0-121-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-81-generic |
14.04 LTS | 9.11 | 3.13.0-24-Generic para 3.13.0-128-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-91-generic |
14.04 LTS | 9.12 | 3.13.0-24-Generic para 3.13.0-132-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-96-generic |
14.04 LTS | 9.13 | 3.13.0-24-Generic para 3.13.0-137-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-104-generic |
16.04 LTS | 9.10 | 4.4.0-21-Generic para 4.4.0-81-generic,<br/>4.8.0-34-Generic para 4.8.0-56-generic,<br/>4.10.0-14-Generic para 4.10.0-24-generic |
16.04 LTS | 9.11 | 4.4.0-21-Generic para 4.4.0-91-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-32-generic |
16.04 LTS | 9.12 | 4.4.0-21-Generic para 4.4.0-96-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-35-generic |
16.04 LTS | 9.13 | 4.4.0-21-Generic para 4.4.0-104-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic |

## <a name="linux-file-systemsguest-storage-configurations"></a>Configurações de armazenamento do Linux ficheiro sistemas/convidado

**Componente** | **Suportado**
--- | ---
sistemas de ficheiros | ext3, ext4, ReiserFS (Suse Linux Enterprise Server apenas), XFS
Gestor de volumes | LVM2
Software MultiPath | Mapeador de dispositivo
Dispositivos de armazenamento Paravirtualized | Exportados pelo paravirtualized controladores de dispositivos não são suportados.
Dispositivos de e/s de fila multi bloco | Não suportado.
Servidores físicos com o controlador de armazenamento HP CCISS | Não suportado.
Diretórios | Estes diretórios (se configurar como separados partições /-sistemas de ficheiros) têm de estar todos no mesmo disco do SO no servidor de origem: / (raiz), /boot /usr, /usr/local, /var, /etc/hosts </br></br>  Se / volume (raiz) é um volume LVM, em seguida, tem de residir numa partição separada no mesmo disco e não pode ser um volume LVM /boot.<br/><br/>
XFSv5 | São suportadas as funcionalidades de XFSv5 em sistemas de ficheiros XFS como metadados soma de verificação da versão 9.10 do serviço de mobilidade e superior. Utilize o utilitário de xfs_info para verificar o superblock XFS para a partição. Se ftype estiver definido como 1, em seguida, XFSv5 funcionalidades estão em utilização.



## <a name="network"></a>Rede

**Componente** | **Suportado** 
--- | --- 
Agrupamento de NIC de rede do anfitrião | Suportada para VMware VMs <br/><br/>Não é suportada para a replicação de máquina física
Rede de anfitrião VLAN | Sim 
Rede de anfitrião IPv4 | Sim 
Rede do anfitrião IPv6 | Não 
Agrupamento de NIC de rede de convidado/servidor | Não 
Rede de convidado/servidor IPv4 | Sim 
Rede de convidado/servidor IPv6 | Não 
IP estático da rede do convidado/servidor (Windows) | Sim 
IP estático da rede do convidado/servidor (Linux) | Sim <br/><br/>As VMs estão configuradas para utilizar DHCP na reativação pós-falha  
Vários NICs de rede de convidado/servidor | Sim 


## <a name="azure-vm-network-after-failover"></a>Rede VM do Azure (após a ativação pós-falha)

**Componente** | **Suportado** 
--- | --- 
ExpressRoute | Sim 
ILB | Sim 
ELB | Sim 
Gestor de Tráfego | Sim 
Vários NICs | Sim 
Endereço IP reservado | Sim 
IPv4 | Sim 
Manter o endereço IP de origem | Sim 
Pontos finais do serviço de rede virtual<br/><br/> (Firewalls de armazenamento do azure e as VNETs) | Não 


## <a name="storage"></a>Armazenamento


**Componente** | **Suportado** 
--- | --- 
Host NFS | Sim para VMware<br/><br/> Não para servidores físicos 
SAN (ISCSI) no anfitrião | Sim
Caminho de anfitrião multi (o MPIO) | Sim - testada com: Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM para CLARiiON
Convidados/servidor VMDK | Sim 
Convidados/servidor EFI/UEFI| Parcial (migração do Azure para o Windows Server 2012 e apenas mais tarde.) </br></br> * * Consulte a nota no fim da tabela.
Disco de cluster partilhado convidado/servidor | Não 
Disco encriptado convidado/servidor | Não 
Convidados/servidor NFS | Não 
Convidados/servidor SMB 3.0 | Não
Convidados/servidor RDM | Sim<br/><br/> N/d para servidores físicos 
Convidados/servidor disco > 1 TB | Sim<br/><br/>4095 GB 
Disco de convidado/servidor com o tamanho de sector físico lógicos e 4 k de 4K | Sim
Disco de convidado/servidor com lógico de 4K e tamanho de setor físico de 512 bytes | Sim 
Volume de convidado/servidor com o disco repartidos > 1 TB<br/><br/> Convidados/servidor de gestão de Volume LVM lógica - os espaços de armazenamento | Nenhum disco de acesso frequente adição/remoção de convidado/servidor | Nenhum convidado/servidor - excluir o disco | Sim multi-path convidado/servidor (MPIO) | N/D

> [!NOTE]
> * * UEFI arrancar em máquinas virtuais VMware ou servidores físicos com o Windows Server 2012 ou posterior, que podem ser migrados para o Azure. As seguintes restrições aplicam-se.
> - Apenas uma migração para o Azure é suportada. Reativação pós-falha para o site de VMware no local não suportada.
> - O servidor não deve ter mais de 4 partições no disco do SO.
> - Requer a versão do serviço de mobilidade de recuperação de Site do Azure 9.13 ou posterior.


## <a name="azure-storage"></a>Storage do Azure

**Componente** | **Suportado** 
--- | --- 
LRS | Sim 
GRS | Sim 
RA-GRS | Sim 
Armazenamento de acesso esporádico | Não 
Armazenamento frequente| Não 
Blobs de bloco | Não 
Encriptação rest(SSE)| Sim 
Armazenamento Premium | Sim 
Serviço de importação/exportação | Não 
Pontos finais do serviço de rede virtual<br/><br/> Firewalls de armazenamento do Azure e as VNETs configuradas na conta de armazenamento de cache/armazenamento de destino (utilizada para armazenar dados de replicação) | Não 
Contas de armazenamento de V2 para fins gerais (camada ambos frequente e esporádico) | Não 


## <a name="azure-compute"></a>Computação do Azure

**Featuree** | **Suportado** 
--- | --- 
Conjuntos de disponibilidade | Sim 
HUB | Sim   
Managed disks | Sim 

## <a name="azure-vm-requirements"></a>Requisitos de VM do Azure

VMs no local que replicam para o Azure tem de cumprir os requisitos de VM do Azure resumidos nesta tabela.

**Componente** | **Requisitos** | **Detalhes**
--- | --- | ---
**Sistema operativo convidado** | Certifique-se [sistemas operativos suportados](#replicated machines) | A verificação de pré-requisitos falha se não suportado.
**Arquitetura do sistema operativo convidado** | 64 bits | Verificação de pré-requisitos falha se não suportado
**Tamanho do disco do sistema operativo** | 2048 GB | Verificação de pré-requisitos falha se não suportado
**Contagem de discos do sistema operativo** | 1 | Verificação de pré-requisitos irá falhar se não suportado.
**Contagem de discos de dados** | 64 ou menos se estiver a replicar **VMs de VMware para Azure**; 16 ou menos se estiver a replicar **VMs de Hyper-V para o Azure** | Verificação de pré-requisitos irá falhar se não suportado
**Tamanho VHD do disco de dados** | 4095 GB | Verificação de pré-requisitos falha se não suportado
**Adaptadores de rede** | São suportados vários adaptadores | 
**VHD partilhado** | Não suportado | Verificação de pré-requisitos falha se não suportado
**Disco FC** | Não suportado | Verificação de pré-requisitos falha se não suportado
**Formato de disco rígido** | VHD <br/><br/> VHDX | Embora VHDX não é atualmente suportado no Azure, a recuperação de sites converte automaticamente VHDX VHD, quando efetuar a ativação pós-falha para o Azure. Quando falhar no local as máquinas virtuais continuam a utilizar o formato VHDX.
**Bitlocker** | Não suportado | O BitLocker tem de ser desativado antes de ativar a replicação para uma máquina.
**Nome da VM** | 1 - 63 carateres.<br/><br/> Restritas a letras, números e hífenes.<br/><br/> O nome do computador tem de começar e terminar com uma letra ou número. | Atualize o valor nas propriedades da máquina no Site Recovery.
**Tipo VM** | Geração 1<br/><br/> Geração 2 – Windows | Geração 2 VMs com um tipo de disco de SO de basic (que inclui um ou dois volumes de dados formatados como VHDX) e inferior a 300 GB de espaço em disco são suportadas.<br></br>As VMs do Linux geração 2 não são suportadas. [Saiba mais](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="vault-tasks"></a>Tarefas do Cofre

**Ação** | **Suportado** 
--- | --- 
Mover o Cofre entre grupos de recursos<br/><br/> Dentro e entre subscrições | Não 
Mover o armazenamento, rede, as VMs do Azure através de grupos de recursos<br/><br/> Dentro e entre subscrições | Não 


## <a name="mobility-service"></a>Serviço de Mobilidade

**Nome** | **Descrição** | **Versão mais recente** | **Detalhes**
--- | --- | --- | --- | ---
* * Do azure Site Recovery Unified configuração * * | Coordena a comunicação entre servidores do VMware no local e o Azure <br/><br/> Instalado nos servidores do VMware no local | 9.12.4653.1 (disponível a partir do portal) | [Funcionalidades e correções mais recentes](https://aka.ms/latest_asr_updates)
**Serviço de mobilidade** | Coordena a replicação entre servidores de servidores/físico de VMware no local e o site secundário/Azure<br/><br/> Instalado na VM de VMware ou servidores físicos que pretende replicar  | 9.12.4653.1 (disponível a partir do portal) | [Funcionalidades e correções mais recentes](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Passos Seguintes
[Saiba como](tutorial-prepare-azure.md) preparar do Azure para a recuperação após desastre de VMs de VMware.
