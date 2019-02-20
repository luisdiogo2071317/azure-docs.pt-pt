---
title: Matriz de suporte de cópia de segurança do Azure
description: Fornece um resumo das definições de suporte e limitações para o serviço de cópia de segurança do Azure.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: raynew
ms.openlocfilehash: ff4ee1d88bd13e647d0f6218d7e9c9b2c57a5a01
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/20/2019
ms.locfileid: "56429575"
---
# <a name="azure-backup-support-matrix"></a>Matriz de suporte de cópia de segurança do Azure

Pode utilizar o [serviço de cópia de segurança do Azure](backup-overview.md) faça backup dos dados para a cloud do Microsoft Azure. Este artigo resume as definições de suporte gerais e limitações para cenários de cópia de segurança do Azure e implementações.

Outras matrizes de suporte estão disponíveis:

[Matriz de suporte](backup-support-matrix-iaas.md) para cópia de segurança de VM do Azure [matriz de suporte](backup-support-matrix-mabs-dpm.md) para utilizar o System Center DPM/Microsoft Azure Backup server (MABS) de cópia de segurança [matriz de suporte](backup-support-matrix-mars-agent.md) para backup usando o Microsoft Azure Agente de recuperação de serviços (MARS)

## <a name="vault-support"></a>Suporte de cofre

O Azure utiliza de cópias de segurança que dos serviços de recuperação para orquestrar e gerir cópias de segurança e armazenar de segurança dos dados.

**Definição** | **Detalhes**
--- | ---
**Cofres de subscrição** | Até 500 cofres de serviços de recuperação numa única subscrição.
**Máquinas num cofre** | Até 1000 VMs do Azure num único cofre.<br/><br/> Até 50 MABS servidores podem ser registados num único cofre.
**Origens de dados no armazenamento do Cofre** | GB 54400 máximo. Não existe nenhum limite para cópias de segurança de VM do Azure.
**Cópias de segurança para o Cofre** | VMs do Azure: uma vez por dia<br/><br/>As máquinas protegidas pelo DPM/MABS: duas vezes por dia<br/><br/> Máquinas feita diretamente com o agente MARS: três vezes por dia. 
**Cópias de segurança entre cofres** | Cópia de segurança é dentro de uma região.<br/><br/> É necessário um cofre em cada região do Azure que contém as VMs que pretende criar cópias de segurança. Não é possível criar cópias de segurança para uma região diferente. 
**Mover o Cofre** | Pode [mover cofres](https://review.docs.microsoft.com/azure/backup/backup-azure-move-recovery-services-vault) em várias subscrições, ou entre grupos de recursos na mesma subscrição.
**Mover dados entre cofres** | Não é possível mover a segurança dos dados entre cofres.
**Modificar o tipo de armazenamento do Cofre** | Pode modificar o tipo de replicação de armazenamento (GRS/LRS) para um cofre, antes de cópias de segurança são armazenadas. Depois de iniciar as cópias de segurança no cofre, não é possível modificar o tipo de replicação.



## <a name="on-premises-backup-support"></a>Suporte de cópia de segurança no local

Eis o que é suportado se quiser fazer uma cópia de segurança de máquinas no local.

**Machine** | **Cópia de segurança** | **Localização** | **Funcionalidades**
--- | --- | --- | ---
**Cópia de segurança direta da máquina do Windows com agente MARS** | Ficheiros, pastas, estado do sistema | Criar cópias de segurança para cofre dos serviços de recuperação | Cópia de segurança três vezes por dia.<br/><br/> Nenhuma cópia de segurança com suporte para a aplicação.<br/><br/> Restaure o ficheiro, pasta, volume.
**Cópia de segurança direta da máquina do Linux com o agente MARS** | A cópia de segurança não suportada.
**Cópia de segurança para DPM** | Ficheiros, pastas, volumes, o estado do sistema, os dados da aplicação. | Criar cópias de segurança para o armazenamento do DPM local. O DPM, em seguida, efetua cópias de segurança para o cofre. | Instantâneos de deteção de aplicação<br/><br/> Granularidade de completa para cópia de segurança e recuperação.<br/><br/> Linux suportada para VMs (Hyper-V/VMware).<br/><br/>. Oracle não suportada.
**Voltar ao MABS** | Ficheiros, pastas, volumes, o estado do sistema, os dados da aplicação. | Criar cópias de segurança para o armazenamento local do MABS. MABS, em seguida, efetua cópias de segurança no cofre. | Instantâneos de deteção de aplicação<br/><br/> Granularidade de completa para cópia de segurança e recuperação.<br/><br/> Linux suportada para VMs (Hyper-V/VMware).<br/><br/>. Oracle não suportada.


## <a name="azure-vm-backup-support"></a>Suporte de cópia de segurança de VM do Azure

### <a name="azure-vm-limits"></a>Limites VM do Azure

**Limite** | **Detalhes**
--- | ---
Discos de dados VM do Azure | Limite de 16.
Tamanho do disco de dados VM do Azure | Disco individual pode ser até 4095 GB.


### <a name="azure-vm-backup-options"></a>Opções de cópia de segurança de VM do Azure

Eis o que é suportado se quiser fazer uma cópia de segurança de VMs do Azure.

**Machine** | **Cópia de segurança** | **Localização** | **Funcionalidades**
--- | --- | --- | ---
**Cópia de segurança do VM do Azure com a extensão de VM** | VM inteira | Cópia de segurança para o Cofre | Extensão instalada quando ativar a cópia de segurança para uma VM.<br/><br/> Cópia de segurança uma vez por dia.<br/><br/> Aplicação com suporte para cópia de segurança para VMs do Windows, cópia de segurança consistente com ficheiros para VMs do Linux. Pode configurar a consistência de aplicação para computadores Linux utilizando scripts personalizados.<br/><br/> Restaure VM/disco.<br/><br/> Não é possível criar cópias de segurança numa VM do Azure para uma localização no local.
**O Azure backup da VM com o agente de MARS** | Ficheiros, pastas | Cópia de segurança para o Cofre | Cópia de segurança três vezes por dia.<br/><br/> O agente de MARS pode executar em conjunto com a extensão de VM, se quiser fazer cópias de segurança de ficheiros/pastas específicas em vez de toda a VM.
**VM do Azure com o DPM** | Ficheiros, pastas, volumes, o estado do sistema, os dados da aplicação. | Criar cópias de segurança para o armazenamento local de VM do Azure com o DPM. O DPM, em seguida, efetua cópias de segurança para o cofre. | Instantâneos de deteção de aplicação<br/><br/> Granularidade de completa para cópia de segurança e recuperação.<br/><br/> Linux suportada para VMs (Hyper-V/VMware).<br/><br/>. Oracle não suportada.
**VM do Azure com o MABS** | Ficheiros, pastas, volumes, o estado do sistema, os dados da aplicação. | Cópia de segurança para o armazenamento local de VM do Azure com o MABS. MABS, em seguida, efetua cópias de segurança no cofre. | Instantâneos de deteção de aplicação<br/><br/> Granularidade de completa para cópia de segurança e recuperação.<br/><br/> Linux suportada para VMs (Hyper-V/VMware).<br/><br/> Oracle não suportada.





## <a name="linux-backup-support"></a>Suporte de cópia de segurança do Linux

Eis o que é suportado se quiser fazer uma cópia de segurança de máquinas do Linux.

**Cópia de segurança** | **Linux (aprovado pelo Azure)**
--- | ---
**Cópia de segurança direta da máquina no local com o Linux**. | Não. O agente de MARS só pode ser instalado em máquinas do Windows.
**Fazer cópias de segurança de VM do Azure em execução no Linux (com a extensão do agente)** | A utilizar a cópia de segurança consistente com a aplicação [scripts personalizados](backup-azure-linux-app-consistent.md).<br/><br/> Recuperação ao nível do ficheiro.<br/><br/> Restaure ao criar uma VM a partir de um ponto de recuperação ou disco.
**Criar cópias de segurança no local ou VM do Azure em execução no Linux com o DPM** | Cópia de segurança consistente com ficheiros de VMs de Convidado Linux no Hyper-V e no VMWare<br/><br/> Restauro de VMs de Convidado do Linux do Hyper-V e do VMWare</br></br> Cópia de segurança consistente com ficheiros não está disponível para VMs do Azure
**Cópia de segurança no local. o computador/Azure VM em execução no Linux com o MABS** | Cópia de segurança consistente com ficheiros de VMs de Convidado Linux no Hyper-V e no VMWare<br/><br/> Restaurar a VM de Hyper-V e VMs de convidado de Linux do VMWare</br></br> Ficheiro de cópia de segurança consistente não está disponível para VMs do Azure.

## <a name="daylight-saving-support"></a>A guardar o suporte da hora de Verão

Cópia de segurança do Azure não suporta o ajuste automático de relógio de alterações de economia de hora de Verão para cópias de segurança de VM do Azure. Modificar as políticas de cópia de segurança manualmente conforme necessário.


## <a name="disk-deduplication-support"></a>Suporte de eliminação de duplicados do disco

Suporte de eliminação de duplicados do disco é o seguinte:
- A eliminação de duplicados do disco é suportado no local ao utilizar o DPM ou MABs para fazer uma cópia de segurança de VMs de Hyper-V a executar o Windows. O Windows Server efetua a eliminação de duplicados de dados (ao nível do anfitrião) em discos rígidos virtuais (VHDs) que estão ligados à máquina virtual como o armazenamento de cópia de segurança.
- A eliminação de duplicados não é suportada no Azure para qualquer componente de cópia de segurança. Quando o System Center DPM e o servidor de cópia de segurança são implementados no Azure, os discos de armazenamento ligados à VM não é possível ter duplicados eliminados.


## <a name="securityencryption-support"></a>Suporte de segurança/encriptação

O Azure Backup suporta a encriptação para dados em trânsito e em repouso:

Tráfego de rede para o Azure:
- Tráfego de cópia de segurança dos servidores para o Cofre dos serviços de recuperação é encriptado com Advanced Encryption Standard 256.
- Dados de cópia de segurança são enviados através de uma ligação HTTPS segura.
- Os dados de cópia de segurança são armazenados no cofre dos serviços de recuperação no formato encriptado.
- O único a ter a frase de acesso para desbloquear estes dados. Microsoft não é possível desencriptar os dados de cópia de segurança em momento algum.
    > [!WARNING]
    > Depois de configurar o cofre, apenas tem acesso à chave de encriptação. A Microsoft nunca mantém uma cópia e não tem acesso à chave. Se a chave está no local incorreto, a Microsoft não é possível recuperar os dados de cópia de segurança.
Segurança de dados:
- Ao fazer backup de VMs do Azure tem de configurar a encriptação *dentro de* a máquina virtual.
- O Azure Backup suporta o Azure Disk Encryption, que utiliza o BitLocker nas máquinas virtuais do Windows e **dm-crypt** nas máquinas virtuais do Linux.
- No back-end, o Azure Backup utiliza a [encriptação do Serviço de Armazenamento do Azure](../storage/common/storage-service-encryption.md), que protege os dados inativos.


**Machine** | **Em trânsito** | **Em repouso**
--- | --- | ---
Máquinas do Windows sem DPM/MABS no local | ![Sim][green] | ![Sim][green]
VMs do Azure | ![Sim][green] | ![Sim][green]
VMs no local/Azure com o DPM | ![Sim][green] | ![Sim][green]
VMs no local/Azure com o MABS | ![Sim][green] | ![Sim][green]



## <a name="compression-support"></a>Suporte de compressão

Cópia de segurança suporta a compactação do tráfego de cópia de segurança, conforme resumido na tabela abaixo. 

- Para VMs do Azure, a extensão da VM lê os dados diretamente a partir da conta de armazenamento do Azure através da rede de armazenamento, pelo que não é necessário comprimir este tráfego.
- Se estiver a utilizar o DPM ou MABS, é possível compactar os dados antes de ele cópia de segurança para DPM/MABS, para poupar largura de banda.

**Machine** | **Comprimir para o MABS/DPM (TCP)** | **Comprimir (HTTPS) para o Cofre**
--- | --- | ---
**Cópia de segurança direta de máquinas do Windows no local** | ND | Sim
**Fazer cópias de segurança de VMs do Azure com a extensão de VM** | ND | ND
* * Criar cópias de segurança em máquinas no local/Azure com o MABS/DPM | ![Sim][green] | ![Sim][green]



## <a name="retention-limits"></a>Limites de retenção

**Definição** | **Limites**
--- | ---
Máximo de pontos de recuperação por instância protegida (máquina/carga de trabalho | 9999
Hora de expiração máximo para um ponto de recuperação | Sem limite
Frequência de cópia de segurança máxima para o DPM/MABS | A cada 15 minutos para o SQL Server<br/><br/> Uma vez a uma hora para outras cargas de trabalho.
Frequência de cópia de segurança máxima para o Cofre | VMs de máquinas/Azure do Windows em execução MARS no local: três por dia<br/><br/> O DPM/MABS: Duas por dia<br/><br/> Cópia de segurança VM do Azure: Uma vez por dia
Retenção do ponto de recuperação | Diariamente, semanalmente, mensalmente, anualmente.
Período de retenção máximo | Depende da frequência de cópia de segurança.
Pontos de recuperação no disco do DPM/MABS | 64 para servidores de ficheiros, 448 para servidores de aplicações.<br/><br/> Pontos de recuperação de banda são ilimitados para o DPM no local.

## <a name="next-steps"></a>Passos Seguintes

- [Matriz de suporte de revisão](backup-support-matrix-iaas.md) para cópia de segurança de VM do Azure.


[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
