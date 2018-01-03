---
title: "Matriz de suporte para replicação de Hyper-V do Azure | Microsoft Docs"
description: "Resume os requisitos de replicação de Hyper-V para o Azure com o Azure Site Recovery e componentes suportados"
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 5918c56c2b7d01c884bf846e3a7d621b3393bb96
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="support-matrix-for-hyper-v-replication-to-azure"></a>Matriz de suporte para replicação de Hyper-V para o Azure


Este artigo resume os componentes suportados e as definições de recuperação após desastre de VMs de Hyper-V no local para o Azure, utilizando o [do Azure Site Recovery](site-recovery-overview.md) serviço.


## <a name="supported-scenarios"></a>Cenários suportados

**Cenário** | **Detalhes**
--- | --- 
**Hyper-V com o VMM** | Pode efetuar a recuperação de desastre para o Azure para as VMs em execução em anfitriões Hyper-V geridos nos recursos de infraestrutura do System Center Virtual Machine Manager (VMM)<br/><br/> Pode implementar este cenário no portal do Azure ou através do PowerShell.<br/><br/> Quando os anfitriões Hyper-V são geridos pelo VMM, também pode efetuar a recuperação de desastres para um site secundário no local. Leitura [neste artigo](tutorial-vmm-to-vmm.md) para obter mais informações sobre este cenário.
**Hyper-V sem o VMM** | Pode efetuar a recuperação de desastre para o Azure para as VMs em execução em anfitriões Hyper-V que não são geridas pelo VMM.<br/><br/> Pode implementar este cenário no portal do Azure ou através do Powershell. 


## <a name="on-premises-servers"></a>Servidores no local

**Servidor** | **Requisitos** | **Detalhes**
--- | --- | ---
**Hyper-V (em execução sem VMM)** | Windows Server 2016, Windows Server 2012 R2 com as atualizações mais recentes. | Quando configurar um site de Hyper-V no Site Recovery, mistura de anfitriões que executam o Windows Server 2016 e 2012 R2 não é suportada.<br/><br/> Para VMs localizadas num anfitrião com o Windows Server 2016, não é suportada a recuperação para uma localização alternativa.
**Hyper-V (em execução com o VMM)** | O VMM 2016, O VMM 2012 R2 | Se for utilizado o VMM, os anfitriões do Windows Server 2016 deverão ser geridas no VMM 2016.<br/><br/> Atualmente não é suportada uma nuvens do VMM mistura de anfitriões de Hyper-V em execução no Windows Server 2016 e 2012 R2.<br/><br/> Ambientes que incluem uma atualização de um servidor VMM 2012 R2 existente para 2016 não são suportadas.


## <a name="replicated-vms"></a>VMs replicadas


A tabela seguinte resume o suporte VM. Recuperação de sites suporta quaisquer cargas de trabalho em execução num sistema operativo suportado. 

 **Componente** | **Detalhes**
--- | ---
Configuração de VM | As VMs que são replicados para o Azure tem de cumprir [requisitos do Azure](#failed-over-azure-vm-requirements).
Sistema operativo convidado | Qualquer SO convidado [suportado pelo Azure](https://technet.microsoft.com/library/cc794868.aspx).<br/><br/> Não é suportado do Windows Server 2016 nano for apresentado.




## <a name="hyper-v-network-configuration"></a>Configuração de rede Hyper-V

**Componente** | **Hyper-V com o VMM** | **Hyper-V sem o VMM**
--- | --- | ---
O agrupamento de rede: NIC de anfitrião | Sim
Rede de anfitrião: VLAN | Sim
Rede de anfitrião: IPv4 | Sim
Rede de anfitrião: IPv6 | Não
VM do convidado o agrupamento NIC da rede: | Não
Rede de VM do convidado: IPv4 | Sim
Rede de VM do convidado: IPv6 | Não
IP de rede: estática de VM do convidado (Windows) | Sim
IP de rede: estática de VM do convidado (Linux) | Não
Rede de VM do convidado: várias-NIC | Sim



## <a name="azure-vm-network-configuration-after-failover"></a>Configuração de rede VM do Azure (após a ativação pós-falha)

**Componente** | **Hyper-V com o VMM** | **Hyper-V sem o VMM**
--- | --- | ---
ExpressRoute | Sim | Sim
ILB | Sim | Sim
ELB | Sim | Sim
Gestor de Tráfego | Sim | Sim
Vários NICs | Sim | Sim
IP Reservado | Sim | Sim
IPv4 | Sim | Sim
Manter o endereço IP de origem | Sim | Sim
Pontos finais do serviço VNET<br/><br/> (Firewalls de armazenamento do azure e redes virtuais) | Não | Não


## <a name="hyper-v-host-storage"></a>Armazenamento de anfitrião Hyper-V

**Armazenamento** | **Hyper-V com o VMM** | Hyper-V sem o VMM
--- | --- | --- | ---
NFS | ND | ND
SMB 3.0 | Sim | Sim
SAN (ISCSI) | Sim | Sim
Caminho multi (o MPIO). Testar com:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM para CLARiiON | Sim | Sim

## <a name="hyper-v-vm-guest-storage"></a>Armazenamento de convidado de VM de Hyper-V

**Armazenamento** | **Hyper-V com o VMM** | Hyper-V sem o VMM
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
Disco > 1 TB | Sim, 4095 GB | Sim, 4095 GB
Disco: setor de lógico e físico de 4K | Não suportado: Ger 1/Gen 2 | Não suportado: Ger 1/Gen 2
Disco: lógico de 4K e do setor físico de 512 bytes | Sim |  Sim
Volume com o disco repartidos > 1 TB<br/><br/> Gestão de volumes LVM lógica | Sim | Sim
Espaços de armazenamento | Sim | Sim
Disco frequente Adicionar/remover | Não | Não
Excluir o disco | Sim | Sim
Caminho multi (o MPIO) | Sim | Sim

## <a name="azure-storage"></a>Storage do Azure

**Componente** | **Hyper-V com o VMM** | **Hyper-V sem VMM)**
--- | --- | ---
LRS | Sim | Sim
GRS | Sim | Sim
RA-GRS | Sim | Sim
Armazenamento de acesso esporádico | Não | Não
Armazenamento frequente| Não | Não
Blobs de bloco | Não | Não
Encriptação rest(SSE)| Sim | Sim
Armazenamento Premium | Sim | Sim
Serviço de importação/exportação | Não | Não
VNET pontos finais de serviço (firewalls de armazenamento do Azure e as VNETs) no destino para a conta de armazenamento de cache utilizado para dados de replicação | Não | Não


## <a name="azure-compute-features"></a>Funcionalidades de computação do Azure

**Funcionalidade** | **Hyper-V com o VMM** | **Hyper-V sem o VMM**
--- | --- | ---
Conjuntos de disponibilidade | Sim | Sim
HUB | Sim | Sim  
Managed disks | Sim, para ativação pós-falha<br/><br/> Reativação pós-falha dos discos geridos não é suportada | Sim, para ativação pós-falha<br/><br/> Reativação pós-falha dos discos geridos não é suportada

## <a name="azure-vm-requirements"></a>Requisitos de VM do Azure

VMs no local que replicam para o Azure tem de cumprir os requisitos de VM do Azure resumidos nesta tabela.

**Componente** | **Requisitos** | **Detalhes**
--- | --- | ---
**Sistema operativo convidado** | Recuperação de sites suporta todos os sistemas operativos que são [suportado pelo Azure](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx).  | Verificação de pré-requisitos irá falhar se não suportado.
**Arquitetura do sistema operativo convidado** | 64 bits | Verificação de pré-requisitos irá falhar se não suportado.
**Tamanho do disco do sistema operativo** | 2048 GB para as VMs de geração 1.<br/><br/> 300 GB para as VMs de geração 2.  | Verificação de pré-requisitos irá falhar se não suportado.
**Contagem de discos do sistema operativo** | 1 | Verificação de pré-requisitos irá falhar se não suportado.
**Contagem de discos de dados** | 16 ou menos  | Verificação de pré-requisitos irá falhar se não suportado.
**Tamanho VHD do disco de dados** | 4095 GB | Verificação de pré-requisitos irá falhar se não suportado.
**Adaptadores de rede** | São suportados vários adaptadores |
**VHD partilhado** | Não suportado | Verificação de pré-requisitos irá falhar se não suportado.
**Disco FC** | Não suportado | Verificação de pré-requisitos irá falhar se não suportado.
**Formato de disco rígido** | VHD <br/><br/> VHDX | Recuperação de sites converte automaticamente VHDX VHD quando efetuar a ativação pós-falha para o Azure. Quando falhar no local as máquinas virtuais continuam a utilizar o formato VHDX.
**BitLocker** | Não suportado | O BitLocker tem de ser desativado antes de ativar a replicação para uma VM.
**Nome da VM** | Entre 1 e 63 carateres. Restritas a letras, números e hífenes. O nome da VM tem de começar e terminar com uma letra ou número. | Atualize o valor nas propriedades da VM no Site Recovery.
**Tipo VM** | Geração 1<br/><br/> Geração 2 – Windows | Geração 2 VMs com um tipo de disco de SO de basic (que inclui um ou dois volumes de dados formatados como VHDX) e inferior a 300 GB de espaço em disco são suportadas.<br></br>As VMs do Linux geração 2 não são suportadas. [Saiba mais](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)|

## <a name="recovery-services-vault-actions"></a>Ações de Cofre de serviços de recuperação

**Ação** |  **Hyper-V com o VMM** | **Hyper-V sem o VMM**
--- | --- | --- 
Mover o Cofre entre grupos de recursos<br/><br/> Dentro e entre subscrições | Não | Não 
Mover o armazenamento, rede, as VMs do Azure através de grupos de recursos<br/><br/> Dentro e entre subscrições | Não | Não 


## <a name="provider-and-agent"></a>O fornecedor e agente

Para certificar-se de que a implementação é compatível com as definições neste artigo, certifique-se de que está a executar o fornecedor mais recente e as versões do agente.

**Nome** | **Descrição** | **Detalhes**
--- | --- | --- | --- | ---
**Fornecedor do Azure Site Recovery** | Coordena a comunicação entre servidores no local e o Azure <br/><br/> Hyper-V com o VMM: instalado nos servidores VMM<br/><br/> Hyper-V sem o VMM: instaladas em anfitriões Hyper-V| Versão mais recente: 5.1.2700.1 (disponível a partir do portal)<br/><br/> [Funcionalidades e correções mais recentes](https://aka.ms/latest_asr_updates)
**Agente de serviços de recuperação do Azure (MARS) da Microsoft** | Coordena a replicação entre VMs de Hyper-V e o Azure<br/><br/> Instalado nos servidores de Hyper-V no local (com ou sem VMM) | Agente mais recente disponível no portal






## <a name="next-steps"></a>Passos Seguintes
Saiba como [preparar Azure](tutorial-prepare-azure.md) para recuperação após desastre de VMs de Hyper-V no local. 
