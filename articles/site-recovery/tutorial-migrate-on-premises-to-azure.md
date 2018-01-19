---
title: "Migrar máquinas no local para o Azure com o Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como migrar as máquinas no local para o Azure, utilizando o Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ee9397406cbca21d8bd53019d9daac5a037f508c
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/18/2018
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migrar máquinas no local para o Azure

Além de utilizar o [do Azure Site Recovery](site-recovery-overview.md) serviço para gerir e orquestrar recuperação após desastre de máquinas no local e as VMs do Azure para fins de continuidade do negócio e recuperação após desastre (BCDR), também pode utilizar o Site Recuperação para gerir a migração de máquinas no local para o Azure.


Este tutorial mostra como migrar VMs no local e servidores físicos para o Azure. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Selecione um objetivo de replicação
> * Configurar o ambiente de origem e de destino
> * Configurar uma política de replicação
> * Ativar a replicação
> * Executar uma migração de teste para se certificar de que tudo está a funcionar conforme esperado
> * Executar uma ativação pós-falha única para o Azure

Este é o terceiro tutorial uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. Preparar no local [VMware](tutorial-prepare-on-premises-vmware.md) ou servidores de Hyper-V.

Antes de começar, é útil rever o [VMware](concepts-vmware-to-azure-architecture.md) ou [Hyper-V](concepts-hyper-v-to-azure-architecture.md) arquiteturas para recuperação após desastre.


## <a name="prerequisites"></a>Pré-requisitos

Exportados pelo paravirtualized controladores de dispositivos não são suportados.


## <a name="create-a-recovery-services-vault"></a>Criar um cofre dos Serviços de Recuperação 

1. Inicie sessão no [portal do Azure](https://portal.azure.com) > **Serviços de Recuperação**.
2. Clique em **Novo** > **Monitorização e Gestão** > **Backup e Site Recovery**.
3. No **nome**, especifique o nome amigável **ContosoVMVault**. Se tiver mais do que uma subscrição, selecione a adequado.
4. Criar um grupo de recursos **ContosoRG**.
5. Selecione uma região do Azure. Para verificar as regiões suportadas, veja a disponibilidade geográfica em [Detalhes dos Preços do Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Para aceder rapidamente ao Cofre a partir do dashboard, clique em **afixar ao dashboard** e, em seguida, clique em **criar**.

   ![Novo cofre](./media/tutorial-migrate-on-premises-to-azure/onprem-to-azure-vault.png)

O novo cofre é adicionado ao **Dashboard** em **todos os recursos**e no principal **cofres dos serviços de recuperação** página.



## <a name="select-a-replication-goal"></a>Selecione um objetivo de replicação

Selecione o que pretende replicar e para onde.
1. Clique em **cofres dos serviços de recuperação** > cofre.
2. No Menu de recursos, clique em **recuperação de Site** > **preparar infraestrutura** > **objetivo de proteção**.
3. No **objetivo de proteção**, selecione de que pretende migrar.
    - **VMware**: selecione **para o Azure** > **Sim, com o VMWare vSphere hipervisor**.
    - **Máquina física**: selecione **para o Azure** > **não virtualizados/outras**.
    - **Hyper-V**: selecione **para o Azure** > **Sim, com o Hyper-V**. Se as VMs de Hyper-V são geridas pelo VMM, selecione **Sim**.


## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

- [Configurar](tutorial-vmware-to-azure.md#set-up-the-source-environment) o ambiente de origem para as VMs de VMware.
- [Configurar](tutorial-physical-to-azure.md#set-up-the-source-environment) o ambiente de origem para servidores físicos.
- [Configurar](tutorial-hyper-v-to-azure.md#set-up-the-source-environment) o ambiente de origem para as VMs de Hyper-V.

## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e certifique-se de recursos de destino.

1. Clique em **Preparar a infraestrutura** > **Destino** e selecione a subscrição do Azure que pretende utilizar.
2. Especifique o modelo de implementação Resource Manager.
3. A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.

## <a name="set-up-a-replication-policy"></a>Configurar uma política de replicação

- [Configurar uma política de replicação](tutorial-vmware-to-azure.md#create-a-replication-policy) para VMware VMs.
- [Configurar uma política de replicação](tutorial-physical-to-azure.md#create-a-replication-policy) para servidores físicos.
- [Configurar uma política de replicação](tutorial-hyper-v-to-azure.md#set-up-a-replication-policy) para VMs de Hyper-V.


## <a name="enable-replication"></a>Ativar a replicação

- [Ativar a replicação](tutorial-vmware-to-azure.md#enable-replication) para VMware VMs.
- [Ativar a replicação](tutorial-physical-to-azure.md#enable-replication) para servidores físicos.
- [Ativar a replicação](tutorial-hyper-v-to-azure.md#enable-replication) para VMs de Hyper-V.


## <a name="run-a-test-migration"></a>Executar uma migração de teste

Executar um [ativação pós-falha de teste](tutorial-dr-drill-azure.md) para o Azure, certificar-se de que tudo está a funcionar conforme esperado.


## <a name="migrate-to-azure"></a>Migrar para o Azure

Execute uma ativação pós-falha para as máquinas que pretende migrar.

1. No **definições** > **replicado itens** clique na máquina > **ativação pós-falha**.
2. No **ativação pós-falha** selecionar um **ponto de recuperação** para efetuar a ativação pós-falha. Selecione o ponto de recuperação mais recente.
3. A definição de chave de encriptação não é relevante para este cenário.
4. Selecione **encerrar a máquina antes de iniciar a ativação pós-falha**. Recuperação do site tentará efetuar um encerramento de máquinas virtuais de origem antes de acionar a ativação pós-falha. Ativação pós-falha continua, mesmo se falha de encerramento. Pode seguir o progresso de ativação pós-falha no **tarefas** página.
5. Certifique-se de que a VM do Azure é apresentada no Azure conforme esperado.
6. No **replicado itens**, clique com o botão direito a VM > **concluir a migração**. Isto termina o processo de migração, deixa de replicação para a VM e deixa de faturação de recuperação de Site para a VM.

    ![Concluir a migração](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Não cancele uma ativação pós-falha em curso**: replicação de VM está parada antes de começa a ativação pós-falha. Se cancelar uma ativação pós-falha em curso, deixa de ativação pós-falha, mas a VM não irá replicar novamente.

Em alguns cenários, a ativação pós-falha requer processamento adicional, que demora cerca de oito a dez minutos a concluir. É possível que repare já tempos de ativação pós-falha para servidores físicos, VMware Linux máquinas, VMs de VMware que não tenham o permite o serviço DHCP e as VMs de VMware que não têm os seguintes controladores de arranque de teste: storvsc, vmbus, storflt, intelide, atapi.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial migradas VMs no local para VMs do Azure. Agora pode configurar a recuperação após desastre para as VMs do Azure.

> [!div class="nextstepaction"]
> [Configurar a recuperação após desastre](site-recovery-azure-to-azure-after-migration.md) para VMs do Azure após a migração a partir de um site no local.
