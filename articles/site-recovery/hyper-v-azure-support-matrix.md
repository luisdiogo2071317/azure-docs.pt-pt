---
title: Matriz de suporte para replicação de Hyper-V no Azure | Microsoft Docs
description: Resume os requisitos de replicação de Hyper-V para o Azure com o Azure Site Recovery e componentes suportados
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/06/2018
ms.author: raynew
ms.openlocfilehash: d2c637dc742ee854c7787cf7cd883930c4eaa8bc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>Matriz de suporte para replicação de Hyper-V para o Azure


Este artigo resume as definições para a recuperação de desastre de VMs de Hyper-V no local para o Azure e componentes suportados utilizando [do Azure Site Recovery](site-recovery-overview.md).


## <a name="supported-scenarios"></a>Cenários suportados

**Cenário** | **Detalhes**
--- | --- 
Hyper-V com o Virtual Machine Manager | Pode efetuar a recuperação de desastre para o Azure para as VMs em execução em anfitriões Hyper-V que são geridos nos recursos de infraestrutura do System Center Virtual Machine Manager.<br/><br/> Pode implementar este cenário no portal do Azure ou através do PowerShell.<br/><br/> Quando os anfitriões de Hyper-V são geridos pelo Virtual Machine Manager, também pode efetuar a recuperação de desastres para um site secundário no local. Para obter mais informações sobre este cenário, leia [neste tutorial](tutorial-vmm-to-vmm.md).
Hyper-V sem o Virtual Machine Manager | Pode efetuar a recuperação de desastre para o Azure para as VMs em execução em anfitriões Hyper-V que não são geridos pelo Virtual Machine Manager.<br/><br/> Pode implementar este cenário no portal do Azure ou através do PowerShell. 


## <a name="on-premises-servers"></a>Servidores no local

**Servidor** | **Requisitos** | **Detalhes**
--- | --- | ---
Hyper-V (em execução sem o Virtual Machine Manager) | Windows Server 2016, com as atualizações mais recentes do Windows Server 2012 R2 | Quando configurar um site de Hyper-V no Site Recovery, mistura de anfitriões que executam o Windows Server 2016 e 2012 R2 não é suportada.<br/><br/> Para VMs localizadas num anfitrião com o Windows Server 2016, não é suportada a recuperação para uma localização alternativa.
Hyper-V (em execução com o Virtual Machine Manager) | O Virtual Machine Manager 2016, o Virtual Machine Manager 2012 R2 | Se for utilizado o Virtual Machine Manager, os anfitriões do Windows Server 2016 deverão ser geridas do Virtual Machine Manager 2016.<br/><br/> Atualmente não é suportada uma nuvem do Virtual Machine Manager mistura de anfitriões de Hyper-V em execução no Windows Server 2016 e 2012 R2.<br/><br/> Ambientes que incluem uma atualização de um servidor existente do Virtual Machine Manager 2012 R2 para 2016 não são suportadas.


## <a name="replicated-vms"></a>VMs replicadas


A tabela seguinte resume o suporte VM. Recuperação de sites suporta quaisquer cargas de trabalho em execução num sistema operativo suportado. 

 **Componente** | **Detalhes**
--- | ---
Configuração de VM | As VMs que são replicados para o Azure tem de cumprir [requisitos do Azure](#failed-over-azure-vm-requirements).
Sistema operativo convidado | Qualquer SO convidado [suportado pelo Azure](https://technet.microsoft.com/library/cc794868.aspx).<br/><br/> Não é suportado do Windows Server 2016 nano for apresentado.




## <a name="hyper-v-network-configuration"></a>Configuração de rede Hyper-V

**Componente** | **Hyper-V com o Virtual Machine Manager** | **Hyper-V sem o Virtual Machine Manager**
--- | --- | ---
Rede do anfitrião: agrupamento NIC | Sim
Rede do anfitrião: VLAN | Sim
Rede do anfitrião: IPv4 | Sim
Rede do anfitrião: IPv6 | Não
Rede VM do convidado: agrupamento NIC | Não
Rede VM do convidado: IPv4 | Sim
Rede VM do convidado: IPv6 | Não
Rede VM do convidado: IP estático (Windows) | Sim
Rede VM do convidado: IP estático (Linux) | Não
Rede VM do convidado: vários NICs | Sim



## <a name="azure-vm-network-configuration-after-failover"></a>Configuração de rede VM do Azure (após a ativação pós-falha)

**Componente** | **Hyper-V com o Virtual Machine Manager** | **Hyper-V sem o Virtual Machine Manager**
--- | --- | ---
Azure ExpressRoute | Sim | Sim
ILB | Sim | Sim
ELB | Sim | Sim
Traffic Manager do Azure | Sim | Sim
Vários NICs | Sim | Sim
IP Reservado | Sim | Sim
IPv4 | Sim | Sim
Manter o endereço IP de origem | Sim | Sim
Pontos finais de serviço de rede Virtual do Azure<br/><br/> (Firewalls de armazenamento do azure e redes virtuais) | Não | Não


## <a name="hyper-v-host-storage"></a>Armazenamento de anfitrião Hyper-V

**Armazenamento** | **Hyper-V com o Virtual Machine Manager** | **Hyper-V sem o Virtual Machine Manager**
--- | --- | --- | ---
NFS | ND | ND
SMB 3.0 | Sim | Sim
SAN (ISCSI) | Sim | Sim
Caminho multi (o MPIO). Testar com:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM para CLARiiON | Sim | Sim

## <a name="hyper-v-vm-guest-storage"></a>Armazenamento de convidado de VM de Hyper-V

**Armazenamento** | **Hyper-V com o Virtual Machine Manager** | **Hyper-V sem o Virtual Machine Manager**
--- | --- | ---
VMDK | ND | ND
VHD/VHDX | Sim | Sim
Geração 2 VM | Sim | Sim
EFI/UEFI| Sim | Sim
Disco de cluster partilhado | Não | Não
Disco encriptado | Não | Não
NFS | ND | ND
SMB 3.0 | Não | Não
RDM | ND | ND
Disco > 1 TB | Sim, 4,095 GB | Sim, 4,095 GB
Disco: setor de lógico e físico de 4K | Não suportado: Ger 1/Gen 2 | Não suportado: Ger 1/Gen 2
Disco: lógico de 4K e do setor físico de 512 bytes | Sim |  Sim
Volume com o disco repartido > 1 TB<br/><br/> Gestão de volumes lógica (LVM) | Sim | Sim
Espaços de armazenamento | Sim | Sim
Disco frequente Adicionar/remover | Não | Não
Excluir o disco | Sim | Sim
Caminho multi (o MPIO) | Sim | Sim

## <a name="azure-storage"></a>Storage do Azure

**Componente** | **Hyper-V com o Virtual Machine Manager** | **Hyper-V sem o Virtual Machine Manager**
--- | --- | ---
Armazenamento localmente redundante | Sim | Sim
Armazenamento georredundante | Sim | Sim
Armazenamento georredundante com acesso de leitura | Sim | Sim
Armazenamento de acesso esporádico | Não | Não
Armazenamento frequente| Não | Não
Blobs de bloco | Não | Não
Encriptação de Inativos (SSE)| Sim | Sim
Armazenamento Premium | Sim | Sim
Serviço de importação/exportação | Não | Não
Rede Virtual serviço pontos finais do Azure (firewalls de armazenamento do Azure e redes virtuais) no destino para a conta de armazenamento de cache utilizado para dados de replicação | Não | Não


## <a name="azure-compute-features"></a>Funcionalidades de computação do Azure

**Funcionalidade** | **Hyper-V com o Virtual Machine Manager** | **Hyper-V sem o Virtual Machine Manager**
--- | --- | ---
Conjuntos de disponibilidade | Sim | Sim
HUB | Sim | Sim  
Managed disks | Sim, para ativação pós-falha.<br/><br/> Reativação pós-falha dos discos geridos não é suportada. | Sim, para ativação pós-falha.<br/><br/> Reativação pós-falha dos discos geridos não é suportada.

## <a name="azure-vm-requirements"></a>Requisitos de VM do Azure

VMs no local que replicam para o Azure tem de cumprir os requisitos de VM do Azure resumidos nesta tabela.

**Componente** | **Requisitos** | **Detalhes**
--- | --- | ---
Sistema operativo convidado | Recuperação de sites suporta todos os sistemas operativos que são [suportado pelo Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | A verificação de pré-requisitos falha se não suportado.
Arquitetura do sistema operativo convidado | 64 bits | A verificação de pré-requisitos falha se não suportado.
Tamanho do disco do sistema operativo | Até 2.048 GB para as VMs de geração 1.<br/><br/> 300 GB para as VMs de geração 2.  | A verificação de pré-requisitos falha se não suportado.
Contagem de discos do sistema operativo | 1 | A verificação de pré-requisitos falha se não suportado.
Contagem de discos de dados | 16 ou menos  | A verificação de pré-requisitos falha se não suportado.
Tamanho VHD do disco de dados | 4,095 GB | A verificação de pré-requisitos falha se não suportado.
Placas de rede | São suportados vários adaptadores |
VHD partilhado | Não suportado | A verificação de pré-requisitos falha se não suportado.
Disco FC | Não suportado | A verificação de pré-requisitos falha se não suportado.
Formato de disco rígido | VHD <br/><br/> VHDX | Recuperação de sites converte automaticamente VHDX VHD quando efetuar a ativação pós-falha para o Azure. Quando falhar no local, as máquinas virtuais continuam a utilizar o formato VHDX.
BitLocker | Não suportado | O BitLocker tem de ser desativado antes de ativar a replicação para uma VM.
o nome da VM | Entre 1 e 63 carateres. Limitado a letras, números e hífenes. O nome da VM tem de começar e terminar com uma letra ou um número. | Atualize o valor nas propriedades da VM no Site Recovery.
Tipo VM | Geração 1<br/><br/> Geração 2 – Windows | Geração 2 VMs com um tipo de disco de SO de basic (que inclui um ou dois volumes de dados formatados como VHDX) e inferior a 300 GB de espaço em disco são suportadas.<br></br>As VMs do Linux geração 2 não são suportadas. [Saiba mais](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Ações de Cofre de serviços de recuperação

**Ação** |  **Hyper-V com o Virtual Machine Manager** | **Hyper-V sem o Virtual Machine Manager**
--- | --- | --- 
Mover o Cofre entre grupos de recursos<br/><br/> Dentro e entre subscrições | Não | Não 
Mover o armazenamento, rede, as VMs do Azure através de grupos de recursos<br/><br/> Dentro e entre subscrições | Não | Não 


## <a name="provider-and-agent"></a>Fornecedor e agente

Para certificar-se de que a implementação é compatível com as definições neste artigo, certifique-se de que está a executar o fornecedor mais recente e as versões do agente.

**Nome** | **Descrição** | **Detalhes**
--- | --- | --- | --- | ---
Fornecedor do Azure Site Recovery | Coordena a comunicação entre servidores no local e o Azure <br/><br/> Hyper-V com o Virtual Machine Manager: instalado nos servidores do Virtual Machine Manager<br/><br/> Sem o Gestor de Máquina Virtual de Hyper-V: instaladas em anfitriões Hyper-V| Versão mais recente: 5.1.2700.1 (disponível a partir do portal do Azure)<br/><br/> [Funcionalidades e correções mais recentes](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Agente dos serviços de recuperação do Microsoft Azure | Coordena a replicação entre VMs de Hyper-V e o Azure<br/><br/> Instalado nos servidores de Hyper-V no local (com ou sem o Virtual Machine Manager) | Agente mais recente disponível no portal






## <a name="next-steps"></a>Passos Seguintes
Saiba como [preparar Azure](tutorial-prepare-azure.md) para recuperação após desastre de VMs de Hyper-V no local. 
