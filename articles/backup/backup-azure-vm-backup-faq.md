---
title: Cópia de segurança VM do Azure FAQ
description: Respostas a perguntas comuns sobre como funcionam as cópias de segurança de VMs do Azure, as limitações e o que ocorre quando há alterações a políticas
services: backup
author: trinadhk
manager: shreeshd
keywords: cópia de segurança de vm do azure, restauro de vm do azure, política de cópias de segurança
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: trinadhk
ms.openlocfilehash: ba77ec34e7887f676ea3df101e87c1ea80fceec5
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414799"
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Perguntas sobre o serviço Azure VM Backup
Este artigo tem respostas a perguntas comuns para o ajudar a compreender rapidamente os componentes do Azure VM Backup. Em algumas das respostas, existem ligações para os artigos que incluem informação abrangente. Também pode publicar perguntas sobre o serviço de Backup do Azure no [fórum de debate](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Configurar a cópia de segurança
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Os cofres dos Serviços de Recuperação suportam VMs clássicas ou VMs no Resource Manager? <br/>
Os cofres dos Serviços de Recuperação suportam ambos os modelos.  Pode fazer backup de uma VM clássica ou uma VM do Resource Manager para um cofre dos serviços de recuperação.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>Que configurações não são suportadas pelo Azure VM backup?
Leia o artigo [sistemas operativos suportados](backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup) e [cópia de segurança de limitações de VM](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Por que motivo não vejo a minha VM no assistente de configuração de cópias de segurança?
No Assistente de cópia de segurança de configurar, cópia de segurança do Azure só apresenta VMs que são:
  * Ainda não estejam protegidas pode verificar o estado de cópia de segurança de uma VM ao aceder ao painel da VM e a verificar o estado de cópia de segurança no Menu de definições. Saiba mais sobre como [Verificar o estado da cópia de segurança de uma VM](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-operations-menu)
  * Pertençam à mesma região que a VM

## <a name="backup"></a>Cópia de segurança
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>As tarefas de cópias de segurança a pedido seguem a mesma agenda de retenção que as cópias de segurança agendadas?
Não. Deve especificar o período de retenção para uma tarefa de cópia de segurança a pedido. Por predefinição, são retido durante 30 dias quando acionada a partir do portal.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Ativei recentemente o Azure Disk Encryption em algumas VMs. As minhas cópias de segurança vão continuar a funcionar?
Tem de dar permissões ao serviço Azure Backup para aceder ao Key Vault. Pode conceder estas permissões no PowerShell com os passos mencionados na secção *Enable Backup* (Ativar Cópia de Segurança) da documentação do [PowerShell](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Migrei discos de uma VM para discos geridos. As minhas cópias de segurança vão continuar a funcionar?
Sim, as cópias de segurança funcionam sem problemas e sem a necessidade de reconfigurar a cópia de segurança.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>A minha VM está a ser encerrado. Será uma demanda ou de um trabalho de cópia de segurança agendado?
Sim. Mesmo quando uma máquina é encerrada cópias de segurança funcionam e o ponto de recuperação está marcado como falhas consistente. Para obter mais detalhes, consulte a secção de consistência de dados em [neste artigo](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)

### <a name="can-i-cancel-an-in-progress-backup-job"></a>Pode cancelar uma tarefa de cópia de segurança em curso?
Sim. Pode cancelar a tarefa de cópia de segurança se ele está na fase de "Criar o instantâneo". **Não é possível cancelar uma tarefa, se a transferência de dados de instantâneos está em curso**.

### <a name="i-enabled-resource-group-lock-on-my-backed-up-managed-disk-vms-will-my-backups-continue-to-work"></a>Eu habilitei o bloqueio do grupo de recursos em minhas VMs com cópia de segurança de discos geridos. As minhas cópias de segurança vão continuar a funcionar?
Se o usuário bloqueia o grupo de recursos, o serviço de cópia de segurança não é possível eliminar os pontos de restauro mais antigos. Devido a isso novas cópias de segurança começam a falhar porque não existe um limite máximo 18 de pontos de restauro impostas a partir do back-end. Se as cópias de segurança estão a falhar com um erro interno após o bloqueio RG, siga estes [coleção de ponto de passos para remover o restauro](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-portal-created-by-backup-service).

### <a name="does-backup-policy-take-daylight-saving-timedst-into-account"></a>Política de cópia de segurança demora Time(DST) da guardar a hora de Verão em conta?
Não. Lembre-se de que data e hora no seu computador local é apresentado na sua hora local e com a tendência atual do horário de Verão. Portanto, o tempo configurado para cópias de segurança agendadas pode ser diferente da sua hora local devido ao horário de Verão.

### <a name="maximum-of-how-many-data-disks-can-i-attach-to-a-vm-to-be-backed-up-by-azure-backup"></a>Máximo de quantos discos de dados podem ligar a uma VM a cópia de segurança pelo Azure Backup?
O Azure Backup suporta agora a cópia de segurança de máquinas virtuais com até 32 discos. Para obter suporte de disco 32, [atualização para a pilha de cópia de segurança do Azure VM V2](backup-upgrade-to-vm-backup-stack-v2.md). Haverá obter suporte para todas as VMs de ativar a proteção a partir de 24 de Setembro de 2018.

### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>Suporte de cópia de segurança do Azure Standard SSD geridos pelo disco?
O Azure Backup suporta [Standard SSD Managed Disks](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/), um novo tipo de armazenamento durável para máquinas virtuais do Microsoft Azure. É suportado para discos geridos num [pilha de cópia de segurança do Azure VM V2](backup-upgrade-to-vm-backup-stack-v2.md).

## <a name="restore"></a>Restauro
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Como posso decidir entre o restauro de discos e o restauro de VMs completo?
Pense o restauro de VMS completo do Azure como uma opção de criação rápida. Restaure VM opção alterações os nomes dos discos, os contentores utilizados por esses discos, endereços IP públicos e os nomes de interface de rede. A alteração é necessária para manter a exclusividade de recursos criados durante a criação da VM. Mas não adicionará a VM ao conjunto de disponibilidade.

Utilize os discos de restauro para:
  * Personalizar a que é criada a partir do ponto na configuração de tempo, como alterar o tamanho da VM
  * Adicionar configurações, que não estão presentes no momento da cópia de segurança
  * Controlar a convenção de nomenclatura para os recursos que são criados
  * Adicionar a VM ao conjunto de disponibilidade
  * Para qualquer outra configuração que pode ser obtido utilizando apenas a definição de PowerShell ou uma modelo declarativo

### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>Pode utilizar cópias de segurança de VM de disco não gerido para restaurar depois de atualizar meus discos para os managed disks?
Sim, pode usar as cópias de segurança realizadas antes de migrar discos a partir de não gerido para gerido. Por predefinição, o trabalho de VM de restauração criará uma VM com discos não geridos. Pode utilizar a funcionalidade de discos de restauro restaurar discos e utilizá-los para criar uma VM em discos geridos.

### <a name="what-is-the-procedure-to-restore-a-vm-to-a-restore-point-taken-before-the-conversion-from-unmanaged-to-managed-disks-was-done-for-a-vm"></a>O que é o procedimento para restaurar uma VM para um ponto de restauro colocado antes da conversão de não gerenciado para os managed disks era feita para uma VM?
Neste cenário, por predefinição, tarefa de VM de restauro irá criar uma VM com discos não geridos. Para criar uma VM com discos geridos:
1. [Restaurar para discos não geridos](tutorial-restore-disk.md#restore-a-vm-disk)
2. [Converta os discos restaurados em discos geridos](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk)
3. [Criar uma VM com discos geridos](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk) <br>
Para cmdlets do PowerShell, consulte [aqui](backup-azure-vms-automation.md#restore-an-azure-vm).

### <a name="can-i-restore-the-vm-if-my-vm-is-deleted"></a>Pode restaurar a VM se a minha VM é eliminado?
Sim. Ciclo de vida da VM e seu item de cópia de segurança correspondente são diferentes. Portanto, mesmo que a VM, pode aceder à correspondente item no cofre dos serviços de recuperação de cópia de segurança e acione um restauro através de um dos pontos de recuperação.

## <a name="manage-vm-backups"></a>Gerir cópias de segurança de VMs
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>O que acontece quando altero uma política de cópias de segurança numa ou em várias VMs?
Quando uma nova política é aplicada ou mais VMS, agenda e a retenção da nova política é seguida. Se a retenção for estendida, os pontos de recuperação existentes serão marcados para que estejam em conformidade com a política nova. Se a retenção for reduzida, serão marcados para eliminação na tarefa de limpeza seguinte e, posteriormente, eliminados.

### <a name="how-can-i-move-a-vm-enrolled-in-azure-backup-between-resource-groups"></a>Como posso mover que uma VM inscrito na cópia de segurança do Azure entre grupos de recursos?
Siga os passos abaixo para mover com êxito a VM de cópia de segurança para o grupo de recursos de destino
1. Temporariamente parar cópia de segurança e manter os dados de cópia de segurança
2. Mova a VM para o grupo de recursos de destino
3. Voltar a proteger com o mesmo/novo cofre

Os utilizadores podem restaurar a partir dos pontos de restauro disponíveis criados antes da operação de movimentação.
