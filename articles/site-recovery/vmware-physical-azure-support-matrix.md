---
title: Matriz de suporte para replicar VMs de VMware e servidores físicos para o Azure com o Azure Site Recovery | Documentos da Microsoft
description: Resume os sistemas operativos suportados e componentes para replicar VMware VMs e de servidor físico para o Azure com o Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: eecf608a712e6a29180b797bbef491501653dfd6
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006483"
---
# <a name="support-matrix-for-vmware-and-physical-server-replication-to-azure"></a>Matriz de suporte para o VMware e de replicação de servidor físico para o Azure

Este artigo resume os componentes suportados e configurações para recuperação após desastre de VMs de VMware para o Azure, utilizando [do Azure Site Recovery](site-recovery-overview.md).

## <a name="replication-scenario"></a>Cenário de replicação

**Cenário** | **Detalhes**
--- | ---
VMs VMware | Replicação de VMs de VMware no local para o Azure. Pode implementar este cenário no portal do Azure ou utilizando [PowerShell](vmware-azure-disaster-recovery-powershell.md).
Servidores físicos | Replicação de servidores físicos de Windows/Linux no local para o Azure. Pode implementar este cenário no portal do Azure.

## <a name="on-premises-virtualization-servers"></a>Servidores de virtualização no local

**Servidor** | **Requisitos** | **Detalhes**
--- | --- | ---
VMware | o vCenter Server 6.7, 6.5, 6.0, ou 5.5 ou vSphere 6.7, 6.5, 6.0 ou 5.5 | Recomendamos que utilize um servidor vCenter.<br/><br/> Recomendamos que os anfitriões vSphere e servidores vCenter estão localizados na mesma rede que o servidor de processos. Por predefinição os componentes de servidor de processo é executado no servidor de configuração,, esta será a rede na qual configurou o servidor de configuração, a menos que configurar um servidor de processos dedicados.
Físico | N/A

## <a name="site-recovery-configuration-server"></a>Servidor de configuração do site Recovery

O servidor de configuração é uma máquina no local que executa os componentes do Site Recovery, incluindo o servidor de configuração, o servidor de processos e o servidor de destino mestre. Para a replicação de VMware é configurar o servidor de configuração com todos os requisitos, através de um modelo do OVF para criar uma VM de VMware. Para a replicação de servidor físico, configure a máquina do servidor de configuração manualmente.

**Componente** | **Requisitos**
--- |---
Núcleos de CPU | 8
RAM | 16 GB
Número de discos | 3 discos<br/><br/> Discos incluem o disco de SO, disco de cache do servidor de processo e unidade de retenção para a reativação pós-falha.
Espaço livre em disco | 600 GB de espaço necessário para a cache do servidor de processo.
Espaço livre em disco | 600 GB de espaço necessário para a unidade de retenção.
Sistema operativo  | Windows Server 2012 R2 ou Windows Server 2016 |
Região do sistema operativo | Inglês (en-us)
PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") deve ser instalado.
Funções do Windows Server | Não ative: <br> - Active Directory Domain Services <br>- Serviços de Informação da Internet <br> - Hyper-V |
Políticas de grupo| Não ative: <br> -Impedi o acesso ao prompt de comando. <br> -Impedi o acesso a ferramentas de edição do registo. <br> -Lógica para anexos de ficheiros de fidedignidade. <br> – Ative a execução do Script. <br> [Saiba mais](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)|
IIS | Certifique-se de que:<br/><br/> -Não é necessário um Web site predefinido do preexistente <br> -Permitir [autenticação anónima](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Ativar [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) definição  <br> -Não tiverem preexistente de Web site/aplicação em escuta na porta 443<br>
Tipo NIC | VMXNET3 (quando implementado como uma VM VMware)
Tipo de endereço IP | Estático
Portas | 443 utilizado para a orquestração de canal de controlo)<br>9443 utilizado para o transporte de dados

## <a name="replicated-machines"></a>Máquinas replicadas

Recuperação de sites suporta a replicação de qualquer carga de trabalho em execução numa máquina.

**Componente** | **Detalhes**
--- | ---
Definições do computador | As máquinas que replicam para o Azure tem de cumprir [requisitos do Azure](#azure-vm-requirements).
Sistema de operativo do Windows | 64-bit Windows Server 2016 (Server Core, servidor com experiência de área de trabalho), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 com em, pelo menos, SP1.</br></br>  [Windows Server 2008 com pelo menos o SP2 - 32 bits e 64 bits](migrate-tutorial-windows-server-2008.md) (apenas para migração). </br></br> * *O servidor Nano do Windows 2016 não é suportado.*
Sistema operativo Linux | Red Hat Enterprise Linux: 5.2 para 5.11, 6.1 para 6.9, 7.0 e 7,4 <br/><br/>CentOS: 5.2 para 5.11, 6.1 para 6.9, 7.0 e 7,4 <br/><br/>Servidor de Ubuntu 14.04 LTS[ (versões de kernel de suportadas)](#ubuntu-kernel-versions)<br/><br/>Servidor de Ubuntu 16.04 LTS[ (versões de kernel de suportadas)](#ubuntu-kernel-versions)<br/><br/>Debian 7/Debian 8[ (versões de kernel de suportadas)](#debian-kernel-versions)<br/><br/>Oracle Enterprise Linux 6.4, 6.5 com o kernel compatível do Red Hat ou Indestrutíveis Enterprise Kernel versão 3 (UEK3) <br/><br/>SUSE Linux Enterprise Server 11 SP3 SP4 do SUSE Linux Enterprise Server 11 <br/><br/>Atualizar máquinas replicadas do SP3 para SP4 não é suportada. Para atualizar, desative a replicação e ativá-la novamente após a atualização.

>[!NOTE]
>
> - Nas distribuições do Linux, apenas os kernels das ações que fazem parte da versão/atualização do podverze distribuição são suportados.
>
> - A atualizar máquinas protegidas em Linux principais versões de distribuição não é suportada. Para atualizar, desative a replicação, atualize o sistema operativo e, em seguida, ative novamente a replicação.
>
> - Servidores a executar o Red Hat Enterprise Linux 5.2 5.11 ou CentOS 5.2 para 5.11 devem ter os componentes de Services(LIS) de integração do Linux instalados para que as máquinas efetuar o arranque no Azure.

### <a name="ubuntu-kernel-versions"></a>Versões de kernel do Ubuntu


**Versão suportada** | **Versão do serviço de mobilidade de recuperação de sites do Azure** | **Versão de kernel** |
--- | --- | --- |
14.04 LTS | 9.17 | 3.13.0-24-Generic para 3.13.0-149-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-127-generic |
14.04 LTS | 9.16 | 3.13.0-24-Generic para 3.13.0-144-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-119-generic |
14.04 LTS | 9.15 | 3.13.0-24-Generic para 3.13.0-144-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-119-generic |
14.04 LTS | 9.14 | 3.13.0-24-Generic para 3.13.0-142-generic,<br/>3.16.0-25-Generic para 3.16.0-77-generic,<br/>3.19.0-18-Generic para 3.19.0-80-generic,<br/>4.2.0-18-Generic para 4.2.0-42-generic,<br/>4.4.0-21-Generic para 4.4.0-116-generic |
|||
16.04 LTS | 9.17 | 4.4.0-21-Generic para 4.4.0-127-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-43-generic |
16.04 LTS | 9.16 | 4.4.0-21-Generic para 4.4.0-119-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-38-generic |
16.04 LTS | 9.15 | 4.4.0-21-Generic para 4.4.0-119-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-38-generic |
16.04 LTS | 9.14 | 4.4.0-21-Generic para 4.4.0-116-generic,<br/>4.8.0-34-Generic para 4.8.0-58-generic,<br/>4.10.0-14-Generic para 4.10.0-42-generic,<br/>4.11.0-13-Generic para 4.11.0-14-generic,<br/>4.13.0-16-Generic para 4.13.0-36-generic |


### <a name="debian-kernel-versions"></a>Versões do debian kernel


**Versão suportada** | **Versão do serviço de mobilidade de recuperação de sites do Azure** | **Versão de kernel** |
--- | --- | --- |
Debian 7 | 9.17 | 3.2.0-4-AMD64 para 3.2.0-6-amd64, 3.16.0-0.bpo.4-amd64 |
Debian 7 | 9.14, 9.15, 9.16 | 3.2.0-4-AMD64 para 3.2.0-5-amd64, 3.16.0-0.bpo.4-amd64 |
|||
Debian 8 | 9.17 | 3.16.0-4-AMD64 para 3.16.0-6-amd64, 4.9.0-0.bpo.4-amd64 para 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.16 | 3.16.0-4-AMD64 para 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 para 4.9.0-0.bpo.6-amd64 |
Debian 8 | 9.14, 9.15 | 3.16.0-4-AMD64 para 3.16.0-5-amd64, 4.9.0-0.bpo.4-amd64 para 4.9.0-0.bpo.5-amd64 |


## <a name="linux-file-systemsguest-storage"></a>Armazenamento de convidado/sistemas de ficheiros do Linux

**Componente** | **Suportado**
--- | ---
Sistemas de ficheiros | ext3, ext4, XFS.
Gestor de volumes | LVM2.
Dispositivos de armazenamento Paravirtualizados | Os dispositivos exportados por controladores paravirtualizados não são suportados.
Dispositivos de e/s de fila multi bloco | Não suportado.
Servidores físicos com o controlador de armazenamento de HP CCISS | Não suportado.
Diretórios | Esses diretórios (se definido como separados partições /-sistemas de ficheiros) todos têm de estar no mesmo disco de SO no servidor de origem: / (raiz), /boot, /usr, /usr/local., /var, /etc.</br></br> /Boot deve estar numa partição de disco e não ser um volume LVM.<br/><br/>
Requisitos de espaço livre| 2 GB na partição /root <br/><br/> 250 MB na pasta de instalação
XFSv5 | Funcionalidades de XFSv5 em sistemas de ficheiros XFS, por exemplo, soma de verificação de metadados, são suportadas a partir do serviço de mobilidade versão 9.10 ou superior. Utilize o utilitário de xfs_info para verificar o superblock XFS para a partição. Se ftype é definido como 1, em seguida, XFSv5 funcionalidades estão em utilização.



## <a name="network"></a>Rede

**Componente** | **Suportado**
--- | ---
Rede de anfitrião, o agrupamento NIC | Suportada para VMs de VMware. <br/><br/>Não é suportada para a replicação da máquina física.
Rede de anfitrião VLAN | Sim.
Rede de anfitrião IPv4 | Sim.
Rede do anfitrião IPv6 | Não.
Rede de convidado/servidor agrupamento NIC | Não.
Rede de convidado/servidor IPv4 | Sim.
Rede de convidado/servidor IPv6 | Não.
IP estático da rede do servidor/convidado (Windows) | Sim.
IP estático da rede do servidor/convidado (Linux) | Sim. <br/><br/>As VMs estão configuradas para utilizar DHCP na reativação pós-falha.
Várias NICs de rede de convidado/servidor | Sim.


## <a name="azure-vm-network-after-failover"></a>Rede VM do Azure (após a ativação pós-falha)

**Componente** | **Suportado**
--- | ---
Azure ExpressRoute | Sim
ILB | Sim
ELB | Sim
Traffic Manager do Azure | Sim
Multi-NIC | Sim
Endereço IP reservado | Sim
IPv4 | Sim
Manter o endereço IP de origem | Sim
Pontos finais de serviço de rede Virtual do Azure<br/> (sem firewalls de armazenamento do Azure) | Sim
Redes Aceleradas | Não

## <a name="storage"></a>Armazenamento
**Componente** | **Suportado**
--- | ---
Anfitrião NFS | Sim para VMware<br/><br/> Não para os servidores físicos
SAN (iSCSI/FC) do anfitrião | Sim
Anfitrião de vSAN | Sim para VMware<br/><br/> N/d para servidores físicos
Multipath i de anfitrião (MPIO) | Sim, testado com o Microsoft DSM, EMC PowerPath 5.7 SP4, EMC PowerPath DSM para CLARiiON
Volumes virtuais do anfitrião (VVols) | Sim para VMware<br/><br/> N/d para servidores físicos
VMDK/servidor convidado | Sim
Servidor de convidado/EFI/UEFI| Parcial (migração do Azure para o Windows Server 2012 e posteriores VMware as máquinas virtuais apenas) </br></br> Consulte a nota no final da tabela
Disco de cluster partilhado do servidor/convidado | Não
Disco de convidado/servidor encriptado | Não
Convidado/servidor NFS | Não
O SMB 3.0 de servidor/convidado | Não
Convidado/servidor RDM | Sim<br/><br/> N/d para servidores físicos
Servidor/convidado disco > 1 TB | Sim<br/><br/>Até 4095 GB
Disco de convidado/servidor com o tamanho de setor físico de k lógicos e 4 de 4K | Sim
Disco de convidado/servidor com lógico de 4K e o tamanho de setor físico de 512 bytes | Sim
Volume de convidado/servidor com o disco repartido > 4 TB <br><br/>Gestão de volumes lógicos (LVM)| Sim
Servidor/convidado - espaços de armazenamento | Não
Disco de acesso frequente Adicionar/Remover/servidor de convidado | Não
Servidor/convidado - excluir o disco | Sim
Multipath i de convidado/servidor (o MPIO) | Não

> [!NOTE]
> UEFI de arranque máquinas de virtuais de VMware com o Windows Server 2012 ou posterior podem ser migrados para o Azure. As seguintes restrições aplicam-se:

> - Apenas uma migração para o Azure é suportada. Não é suportada a reativação pós-falha para o site de VMware no local.
> - O servidor não deve ter mais de quatro partições no disco do SO.
> - Requer a versão de serviço de mobilidade 9.13 ou posterior.
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
Encriptação em repouso (encriptação do serviço de armazenamento)| Sim
Armazenamento Premium | Sim
Serviço de importação/exportação | Não
Firewalls de armazenamento do Azure para redes virtuais configuradas na conta de armazenamento de cache/armazenamento de destino (usada para armazenar dados de replicação) | Não
Contas de armazenamento para fins gerais v2 (escalões esporádico e frequentes) | Não

## <a name="azure-compute"></a>Computação do Azure

**Funcionalidade** | **Suportado**
--- | ---
Conjuntos de disponibilidade | Sim
HUB | Sim
Managed disks | Sim

## <a name="azure-vm-requirements"></a>Requisitos de VM do Azure

As VMs no local que replica para o Azure tem de cumprir os requisitos de VM do Azure resumidos nesta tabela. Quando a recuperação de Site é executada uma verificação de pré-requisitos, ele falhará se alguns dos requisitos não forem cumpridas.

**Componente** | **Requisitos** | **Detalhes**
--- | --- | ---
Sistema operativo convidado | Certifique-se [sistemas operativos suportados](#replicated-machines) para replicar máquinas. | Falha na verificação se não suportado.
Arquitetura de sistema operativo convidado | 64 bits. | Falha na verificação se não suportado.
Tamanho de disco do sistema operativo | Até 2048 GB. | Falha na verificação se não suportado.
Número de discos do sistema operativo | 1 | Falha na verificação se não suportado.  
Número de discos de dados | 64 ou menos. | Falha na verificação se não suportado.  
Tamanho do disco de dados | Até 4095 GB | Falha na verificação se não suportado.
Placas de rede | São suportados vários adaptadores. |
VHD partilhado | Não suportado. | Falha na verificação se não suportado.
Disco FC | Não suportado. | Falha na verificação se não suportado.
BitLocker | Não suportado. | O BitLocker tem de ser desativado antes de ativar a replicação para uma máquina. |
o nome da VM | Entre 1 e 63 carateres.<br/><br/> Limitado a letras, números e hífenes.<br/><br/> O nome do computador deve começar e terminar com uma letra ou número. |  Atualize o valor nas propriedades da máquina no Site Recovery.


## <a name="vault-tasks"></a>Tarefas de cofre

**Ação** | **Suportado**
--- | ---
Mover o Cofre entre grupos de recursos<br/><br/> Dentro e entre subscrições | Não
Mover o armazenamento, rede, as VMs do Azure entre grupos de recursos<br/><br/> Dentro e entre subscrições | Não


## <a name="mobility-service"></a>Serviço de mobilidade

**Nome** | **Descrição** | **Versão mais recente** | **Detalhes**
--- | --- | --- | --- | ---
Configuração unificada do Azure Site Recovery | Coordena as comunicações entre servidores de VMware no local e o Azure <br/><br/> Instalado em servidores de VMware no local | 9.12.4653.1 (disponível a partir do portal) | [Funcionalidades e correções mais recentes](https://aka.ms/latest_asr_updates)
Serviço de mobilidade | Coordena a replicação entre servidores de servidores/físico de VMware no local e o site do Azure/secundária<br/><br/> Instalado na VM de VMware ou servidores físicos que pretende replicar | 9.12.4653.1 (disponível a partir do portal) | [Funcionalidades e correções mais recentes](https://aka.ms/latest_asr_updates)


## <a name="next-steps"></a>Passos Seguintes
[Saiba como](tutorial-prepare-azure.md) para preparar o Azure para recuperação após desastre de VMs de VMware.
