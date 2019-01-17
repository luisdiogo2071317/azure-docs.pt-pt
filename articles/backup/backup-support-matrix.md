---
title: Matriz de suporte de cópia de segurança do Azure
description: Fornece um resumo das definições de suporte e limitações para o serviço de cópia de segurança do Azure.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: overview
ms.date: 01/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: b1fa723863e6485e977e075986c3779efed1e689
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360224"
---
# <a name="azure-backup-support-matrix"></a>Matriz de suporte de cópia de segurança do Azure

Pode utilizar o [serviço de cópia de segurança do Azure](backup-overview.md) faça backup dos dados para a cloud do Microsoft Azure. Este artigo resume as definições de suporte e limitações para cenários de cópia de segurança do Azure e implementações.

## <a name="vault-support"></a>Suporte de cofre

O Azure utiliza de cópias de segurança que dos serviços de recuperação para orquestrar e gerir cópias de segurança e armazenar de segurança dos dados.

**Definição** | **Detalhes**
--- | ---
Número de cofres | Até 500 cofres de serviços de recuperação numa única subscrição.
Máquinas num cofre | Até 1000 VMs do Azure num único cofre.<br/><br/> Até 50 no local, máquinas que executem o agente de cópia de segurança do Azure (agente de serviços de recuperação do Microsoft Azure (MABS)) podem ser registadas num único cofre.
Origem de dados no armazenamento do Cofre | GB 54400 máximo. Não existe nenhum limite para cópias de segurança de VM do Azure.
Cópias de segurança para o Cofre | VMs do Azure: uma vez por dia; As máquinas protegidas pelo DPM/MABS: duas vezes por dia; Máquinas feita diretamente com o agente MARS: três vezes por dia.  
Mover o Cofre | Pode mover cofres dos serviços de recuperação de cópia de segurança entre subscrições e grupos de recursos. [Saiba mais](backup-azure-move-recovery-services-vault.md).
Mover dados entre cofres | Não é possível mover a segurança dos dados entre cofres.
Tipo de replicação de armazenamento | Pode modificar o tipo de replicação de armazenamento (GRS/LRS) para um cofre, antes de cópias de segurança são armazenadas. Depois de iniciar as cópias de segurança no cofre, não é possível modificar o tipo de replicação.



## <a name="on-premises-backup-support"></a>Suporte de cópia de segurança no local 

Eis o que é suportado se quiser fazer uma cópia de segurança de máquinas no local.

**Machine** | **Localização** | **Fazer cópia de segurança** | **Funcionalidades**
--- | --- | --- | ---
**Windows físicos/virtuais (nenhum servidor de cópia de segurança)** | Ficheiros, pastas, estado do sistema | Cópia de segurança para o Cofre dos serviços de recuperação | Cópia de segurança três vezes por dia.<br/><br/> Nenhuma cópia de segurança com suporte para a aplicação.<br/><br/> Restaure o ficheiro, pasta, volume.
**Linux físicos/virtuais (nenhum servidor de cópia de segurança)** | A cópia de segurança não suportada.
**Físicos/virtuais com o DPM** | Ficheiros, pastas, volumes, o estado do sistema, os dados da aplicação. | Cópia de segurança para o DPM (para um disco ligado localmente para o servidor DPM ou em banda.<br/><br/> O DPM, em seguida, efetua cópias de segurança para o cofre. | Instantâneos de deteção de aplicação<br/><br/> Granularidade de completa para cópia de segurança e recuperação.<br/><br/> Linux suportada para VMs (Hyper-V/VMware).<br/><br/>. Oracle não suportada.
**Físico virtual com o MABS** | Ficheiros, pastas, volumes, o estado do sistema, os dados da aplicação. | Cópia de segurança para o MABS (para um disco ligado localmente para o servidor do MABS. Não é suportada em banda<br/><br/> MABS, em seguida, efetua cópias de segurança no cofre. | Instantâneos de deteção de aplicação<br/><br/> Granularidade de completa para cópia de segurança e recuperação.<br/><br/> Linux suportada para VMs (Hyper-V/VMware).<br/><br/>. Oracle não suportada.


## <a name="azure-vms"></a>VMs do Azure

### <a name="azure-vm-limits"></a>Limites VM do Azure

**Limite** | **Detalhes**
--- | ---
Discos de dados VM do Azure | Limite de 16.
Tamanho do disco de dados VM do Azure | Disco individual pode ser até 4095 GB.


### <a name="azure-vm-backup-options"></a>Opções de cópia de segurança de VM do Azure

Eis o que é suportado se quiser fazer uma cópia de segurança de VMs do Azure.

**Machine** | **Localização** | **Fazer cópia de segurança** | **Funcionalidades**
--- | --- | --- | ---
**VMs do Azure (nenhum servidor de cópia de segurança)** | Ficheiros, pastas, estado do sistema | Cópia de segurança para o cofre. | Cópia de segurança uma vez por dia.<br/><br/> Aplicação com suporte para cópia de segurança para VMs do Windows, cópia de segurança consistente com ficheiros para VMs do Linux. Pode configurar a consistência de aplicação para computadores Linux utilizando scripts personalizados.<br/><br/> Restaure VM/disco.<br/><br/> Não é possível criar cópias de segurança numa VM do Azure para uma localização no local.
**VM do Azure com o DPM** | Ficheiros, pastas, volumes, o estado do sistema, os dados da aplicação. | Cópia de segurança para o DPM em execução no Azure (para um disco ligado localmente para o servidor DPM. Banda não é suportada.<br/><br/> O DPM, em seguida, efetua cópias de segurança para o cofre. | Instantâneos de deteção de aplicação<br/><br/> Granularidade de completa para cópia de segurança e recuperação.<br/><br/> Linux suportada para VMs (Hyper-V/VMware).<br/><br/>. Oracle não suportada.
**VM do Azure com o MABS** | Ficheiros, pastas, volumes, o estado do sistema, os dados da aplicação. | Cópia de segurança para o MABS em execução no Azure (para um disco ligado localmente para o servidor do MABS). Não é suportada em banda<br/><br/> MABS, em seguida, efetua cópias de segurança no cofre. | Instantâneos de deteção de aplicação<br/><br/> Granularidade de completa para cópia de segurança e recuperação.<br/><br/> Linux suportada para VMs (Hyper-V/VMware).<br/><br/>. Oracle não suportada.





## <a name="linux-backup-support"></a>Suporte de cópia de segurança do Linux

Eis o que é suportado se quiser fazer uma cópia de segurança de máquinas do Linux.

**Cópia de segurança** | **Linux (aprovado pelo Azure)**
--- | --- 
**Máquina do Linux no local (sem o DPM ou MABS)**. | Não. O agente de MARS só pode ser instalado em máquinas do Windows. 
**VM do Azure (sem o DPM ou MABS)** | A utilizar a cópia de segurança consistente com a aplicação [scripts personalizados](backup-azure-linux-app-consistent.md).<br/><br/> Recuperação ao nível do ficheiro.<br/><br/> Restaure ao criar uma VM a partir de um ponto de recuperação ou disco.
**No local. o computador/Azure VM com o DPM** | Cópia de segurança consistente com ficheiros de VMs de Convidado Linux no Hyper-V e no VMWare<br/><br/> Restauro de VMs de Convidado do Linux do Hyper-V e do VMWare</br></br> Cópia de segurança consistente com ficheiros não está disponível para VMs do Azure
**Máquina/Azure VM com o MABS locais** | Cópia de segurança consistente com ficheiros de VMs de Convidado Linux no Hyper-V e no VMWare<br/><br/> Restaurar a VM de Hyper-V e VMs de convidado de Linux do VMWare</br></br> Ficheiro de cópia de segurança consistente não está disponível para VMs do Azure.

## <a name="disk-support"></a>Suporte de disco

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

Cópia de segurança suporta a compactação do tráfego de cópia de segurança, conforme resumido na tabela abaixo. Tenha em atenção que:

- Para VMs do Azure, a extensão da VM lê os dados diretamente a partir da conta de armazenamento do Azure através da rede de armazenamento, pelo que não é necessário comprimir este tráfego.
- Se estiver a utilizar o DPM ou MABS, é possível compactar os dados antes de ele cópia de segurança para DPM/MABS, para poupar largura de banda. 

**Machine** | **Comprimir para o MABS/DPM (TCP)** | **Comprimir (HTTPS) para o Cofre**
--- | --- | ---
Máquinas do Windows sem DPM/MABS no local | ND | Sim
VMs do Azure | ND | ND
VMs no local/Azure com o DPM | ![Sim][green] | ![Sim][green]
VMs no local/Azure com o MABS | ![Sim][green] | ![Sim][green]



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

- [Fazer cópia de segurança de VMs do Azure](backup-azure-arm-vms-prepare.md)
- [Cópia de segurança diretamente a máquinas Windows](tutorial-backup-windows-server-to-azure.md), sem um servidor de cópia de segurança.
- [Configurar o MABS](backup-azure-microsoft-azure-backup.md) para cópia de segurança para o Azure e, em seguida, de cópia de segurança de cargas de trabalho para o MABS.
- [Configurar o DPM](backup-azure-dpm-introduction.md) para cópia de segurança para o Azure e, em seguida, de cópia de segurança de cargas de trabalho para o DPM.

[green]: ./media/backup-support-matrix/green.png
[yellow]: ./media/backup-support-matrix/yellow.png
[red]: ./media/backup-support-matrix/red.png
