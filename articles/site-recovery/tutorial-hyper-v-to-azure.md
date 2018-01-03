---
title: "Configurar a recuperação após desastre para as VMs de Hyper-V no local (sem VMM) para o Azure com o Azure Site Recovery | Microsoft Docs"
description: "Saiba como configurar a recuperação após desastre de VMs de Hyper-V no local (sem VMM) para o Azure com o serviço do Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31`/2017
ms.author: raynew
ms.openlocfilehash: 633c14bd25bc8a1419196b2c76ca94c26db68991
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Configurar a recuperação de desastre de VMs de Hyper-V no local para Azure

O [do Azure Site Recovery](site-recovery-overview.md) serviço contribui para a sua estratégia de recuperação após desastre, gerir e da orquestração de replicação, ativação pós-falha e a reativação pós-falha de máquinas no local e máquinas de virtuais (VMs) do Azure.

Este tutorial mostra como configurar a recuperação após desastre de VMs de Hyper-V no local para o Azure. O tutorial é relevante para as VMs de Hyper-V que não são geridos pelo System Center Virtual Machine Manager (VMM). Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Selecione a origem de replicação e de destino.
> * Configure o ambiente de replicação de origem, incluindo os componentes da recuperação de Site no local e o ambiente de replicação de destino.
> * Crie uma política de replicação.
> * Ative a replicação para uma VM.

Este é o terceiro tutorial uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Preparar no local Hyper-V](tutorial-prepare-on-premises-hyper-v.md)

Antes de começar, é útil [rever a arquitetura](concepts-hyper-v-to-azure-architecture.md) para este cenário de recuperação após desastre.

## <a name="select-a-replication-goal"></a>Selecione um objetivo de replicação


1. No **todos os serviços** > **cofres dos serviços de recuperação**, clique no nome do cofre foi preparados no tutorial anterior, **ContosoVMVault**.
2. No **introdução**, clique em **recuperação de Site**. Em seguida, clique em **preparar infraestrutura**
3. No **objetivo de proteção** > **onde estão as máquinas localizadas**, selecione **no local**.
4. No **onde pretende replicar as máquinas**, selecione **para o Azure**.
5. No **são as máquinas virtualizadas**, selecione **Sim, com o Hyper-V**.
6. No **está a utilizar o System Center VMM**, selecione **não**. Em seguida, clique em **OK**.

    ![Objetivo de replicação](./media/tutorial-hyper-v-to-azure/replication-goal.png)

## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Para configurar o ambiente de origem, adicionar anfitriões Hyper-V para um site de Hyper-V, transferir e instalar o fornecedor do Azure Site Recovery e agente dos serviços de recuperação do Azure e registar o site Hyper-V no cofre. 

1. No **preparar infraestrutura**, clique em **origem**.
2. Clique em **+ Site Hyper-V**e especifique o nome do site foi criada no tutorial anterior, **ContosoHyperVSite**.
3. Clique em **+ servidor Hyper-V**.
4. Transfira o ficheiro de configuração do fornecedor.
5. Transferir a chave de registo do cofre. Isto tem quando executar a configuração do fornecedor. A chave é válida durante cinco dias depois de gerá-la.

    ![Transfira o fornecedor](./media/tutorial-hyper-v-to-azure/download.png)
    

### <a name="install-the-provider"></a>Instale o fornecedor

Execute o ficheiro de configuração do fornecedor (AzureSiteRecoveryProvider.exe) em cada anfitrião Hyper-V que adicionou à **ContosoHyperVSite** site. A configuração instala o fornecedor do Azure Site Recovery e agente dos serviços de recuperação, em cada anfitrião de Hyper-V.

1. No Assistente de configuração do Azure Site Recovery Provider > **Microsoft Update**, optar por utilizar o Microsoft Update para procurar as atualizações do fornecedor.
2. No **instalação**, aceite a localização de instalação predefinida para o fornecedor e o agente e, em **instalar**.
3. Após a instalação, no Assistente de registo do Microsoft Azure Site Recovery > **as definições do cofre**, clique em **procurar**e, em **ficheiro de chave**, selecione a chave do Cofre de ficheiros que transferidos. 
4. Especifique a subscrição do Azure Site Recovery, o nome do cofre (**ContosoVMVault**) e o site Hyper-V (**ContosoHyperVSite**) ao qual pertence o servidor de Hyper-V.
5. No **as definições de Proxy**, selecione **ligar diretamente ao Azure Site Recovery sem um proxy**.
6. No **registo**, depois do servidor está registado no cofre, clique em **concluir**.

Os metadados do servidor de Hyper-V são obtidos pelo Azure Site Recovery e o servidor é apresentado no **infraestrutura de recuperação de Site** > **anfitriões Hyper-V**. Esta operação pode demorar até 30 minutos.


## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e certifique-se de recursos de destino. 

1. Clique em **preparar a infraestrutura** > **destino**.
2. Selecione a subscrição e o grupo de recursos **ContosoRG**, no qual as VMs do Azure serão criadas após a ativação pós-falha.
3. Selecione o **Gestor de recursos "** modelo de implementação.

A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.


## <a name="set-up-a-replication-policy"></a>Configurar uma política de replicação

1. Clique em **preparar a infraestrutura** > **as definições de replicação** > **+ criar e associar**.
2. No **criar e associar política**, especifique um nome de política, **ContosoReplicationPolicy**.
3. Deixe as predefinições e clique em **OK**.
    - **Copiar frequência** indica que delta dados (após a replicação inicial) serão replicadas a cada cinco minutos.
    - **Retenção do ponto de recuperação** indica que o windows de retenção para cada ponto de recuperação será dois duas horas.
    - **Frequência de instantâneos consistentes com aplicação** indica que os pontos de recuperação que contêm os instantâneos consistentes da aplicação serão criados a cada hora.
    - **Hora de início da replicação inicial**, indica que a replicação inicial será imediatamente iniciada.
4. Após a criação da política, clique em **OK**. Quando cria uma nova política está associada automaticamente o site Hyper-V especificado (**ContosoHyperVSite**)

    ![Política de replicação](./media/tutorial-hyper-v-to-azure/replication-policy.png)


## <a name="enable-replication"></a>Ativar a replicação


1. No **replicar aplicação**, clique em **origem**. 
2. No **origem**, selecione o **ContosoHyperVSite** site. Em seguida, clique em **OK**.
3. No **destino**, certifique-se do Azure como o destino, a subscrição do cofre e o **Resource Manager** modelo de implementação.
4. Selecione o **contosovmsacct1910171607** conta de armazenamento no tutorial anterior para os dados replicados, foi criada e a **ContosoASRnet** rede, na qual as VMs do Azure estarão localizadas após a ativação pós-falha.
5. No **máquinas virtuais** > **selecione**, selecione a VM que pretende replicar. Em seguida, clique em **OK**.

 Pode controlar o progresso do **ativar proteção** ação no **tarefas** > **as tarefas de recuperação de Site**. Depois do **finalizar proteção** tarefa é concluída, a replicação inicial está concluída e a máquina virtual está preparada para ativação pós-falha.

## <a name="next-steps"></a>Passos Seguintes
[Executar um teste de recuperação após desastre](tutorial-dr-drill-azure.md)
