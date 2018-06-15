---
title: Migrar as máquinas no local para o Azure com o Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como migrar as máquinas no local para o Azure, com o Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 02/27/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 3e82983ff171e638415d6e7cafe0cb034dd978af
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/11/2018
ms.locfileid: "34057160"
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migrar máquinas no local para o Azure

Além de utilizar o serviço [Azure Site Recovery](site-recovery-overview.md) para gerir e orquestrar a recuperação após desastre de máquinas no local e de VMs do Azure para fins de continuidade do negócio e recuperação após desastre (BCDR), também pode utilizar o Site Recovery para gerir a migração de máquinas no local para o Azure.


Este tutorial mostra como migrar VMs no local e servidores físicos para o Azure. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Selecionar um objetivo de replicação
> * Configurar o ambiente de origem e de destino
> * Configurar uma política de replicação
> * Ativar a replicação
> * Executar uma migração de teste para confirmar que está tudo a funcionar conforme o previsto
> * Executar uma ativação pós-falha única para o Azure

Este é o terceiro tutorial de uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. Prepare no [VMware](vmware-azure-tutorial-prepare-on-premises.md) local ou no [Hyper-V] (hyper-v-prepare-on-premises-tutorial.md) servers.

Antes de começar, é útil rever as arquiteturas do [VMware](vmware-azure-architecture.md) ou [Hyper-V](hyper-v-azure-architecture.md) para a recuperação após desastre.


## <a name="prerequisites"></a>Pré-requisitos

Os dispositivos exportados por controladores paravirtualizados não são suportados.


## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 

1. Inicie sessão no [portal do Azure](https://portal.azure.com) > **Serviços de Recuperação**.
2. Clique em **Criar um recurso** > **Monitorização e Gestão** > **Backup e Site Recovery**.
3. No **Nome**, especifique o nome amigável **ContosoVMVault**. Se tiver mais do que uma subscrição, selecione a que for adequada.
4. Crie um grupo de recursos **ContosoRG**.
5. Selecione uma região do Azure. Para verificar as regiões suportadas, veja a disponibilidade geográfica em [Detalhes dos Preços do Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Para aceder rapidamente ao cofre a partir do dashboard, clique em **Afixar ao dashboard** e clique em **Criar**.

   ![Novo cofre](./media/migrate-tutorial-on-premises-azure/onprem-to-azure-vault.png)

O cofre novo é adicionado ao **Dashboard** em **Todos os recursos** e na página principal **Cofres dos Serviços de Recuperação**.



## <a name="select-a-replication-goal"></a>Selecionar um objetivo de replicação

Selecione o que pretende replicar e para onde.
1. Clique em **Cofres dos Serviços de Recuperação** > cofre.
2. No Menu de Recursos, clique em **Site Recovery** > **Preparar Infraestrutura** > **Objetivo de proteção**.
3. Em **Objetivo de proteção**, selecione o que pretende migrar.
    - **VMware**: selecione **Para o Azure** > **Sim, com o VMware vSphere Hypervisor**.
    - **Computador físico**: selecione **Para o Azure** > **Não virtualizado/Outro**.
    - **Hyper-V**: selecione **Para o Azure** > **Sim, com o Hyper-V**. Se as VMs de Hyper-V forem geridas pelo VMM, selecione **Sim**.


## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

- [Configure](vmware-azure-tutorial.md#set-up-the-source-environment) o ambiente de origem das VMs VMware.
- [Configure](physical-azure-disaster-recovery.md#set-up-the-source-environment) o ambiente de origem dos servidores físicos.
- [Configure](hyper-v-azure-tutorial.md#set-up-the-source-environment) o ambiente de origem das VMs de Hyper-V.

## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e verifique os recursos de destino.

1. Clique em **Preparar a infraestrutura** > **Destino** e selecione a subscrição do Azure que pretende utilizar.
2. Especifique o modelo de implementação do Resource Manager.
3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.

## <a name="set-up-a-replication-policy"></a>Configurar uma política de replicação

- [Configure uma política de replicação](vmware-azure-tutorial.md#create-a-replication-policy) para as VMs VMware.
- [Configure uma política de replicação](physical-azure-disaster-recovery.md#create-a-replication-policy) para os servidores físicos.
- [Configure uma política de replicação](hyper-v-azure-tutorial.md#set-up-a-replication-policy) para as VMs de Hyper-V.


## <a name="enable-replication"></a>Ativar a replicação

- [Ative a replicação](vmware-azure-tutorial.md#enable-replication) para as VMs VMware.
- [Ative a replicação](physical-azure-disaster-recovery.md#enable-replication) para os servidores físicos.
- Ative a replicação para as VMs de Hyper-V [com](hyper-v-vmm-azure-tutorial.md#enable-replication) ou [sem VMM](hyper-v-azure-tutorial.md#enable-replication).


## <a name="run-a-test-migration"></a>Executar uma migração de teste

Execute uma [ativação pós-falha de teste](tutorial-dr-drill-azure.md) para o Azure para confirmar que está tudo a funcionar conforme o previsto.


## <a name="migrate-to-azure"></a>Migrar para o Azure

Execute a ativação pós-falha nos computadores que quer migrar.

1. Em **Definições** > **Itens replicados**, clique no computador > **Ativação Pós-falha**.
2. Em **Ativação pós-falha**, selecione um **Ponto de Recuperação** para o qual fazer a ativação pós-falha. Selecione o ponto de recuperação mais recente.
3. A definição da chave de encriptação não é relevante para este cenário.
4. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha**. O Site Recovery tentará fazer um encerramento das máquinas virtuais de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. Pode seguir o progresso da ativação pós-falha na página **Tarefas**.
5. Certifique-se de que a VM do Azure é apresentada no Azure conforme esperado.
6. Em **Itens replicados**, clique com o botão direito do rato na VM > **Concluir a Migração**. Desta forma, o processo de migração é concluído, a replicação para a VM é parada e a faturação do Site Recovery para a VM também é parada.

    ![Concluir a migração](./media/migrate-tutorial-on-premises-azure/complete-migration.png)


> [!WARNING]
> **Não cancele uma ativação pós-falha em curso**: antes do início da ativação pós-falha, a replicação da VM é parada. Se cancelar uma ativação pós-falha que esteja em curso, a mesma para, mas a VM não será replicada outra vez.

Em alguns cenários, a ativação pós-falha requer processamento adicional, que demora cerca de oito a dez minutos a concluir. Poderá reparar em tempos de ativação pós-falha superiores Para servidores físicos, máquinas do Linux do VMware, VMs do VMware que não têm o serviço DHCP ativado e VMs do VMware que não têm os controladores de arranque storvsc, vmbus, storflt, intelide e atapi.


## <a name="next-steps"></a>Passos seguintes

Neste tutorial, migrou VMs no local para VMs do Azure. Agora pode configurar a recuperação após desastre para as VMs do Azure.

> [!div class="nextstepaction"]
> [Configure a recuperação após desastre](azure-to-azure-replicate-after-migration.md) para as VMs do Azure após a migração a partir de um site no local.
