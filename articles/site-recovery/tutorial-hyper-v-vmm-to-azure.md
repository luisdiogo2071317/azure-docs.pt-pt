---
title: "Configurar a recuperação após desastre de VMs de Hyper-V no local em nuvens VMM para o Azure com o Azure Site Recovery | Microsoft Docs"
description: "Saiba como configurar a recuperação após desastre de VMs de Hyper-V no local em nuvens VMM do System Center para o Azure, com o serviço do Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 5f364c6f8a88ad53c12909f0e9f10afcce5ef8af
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Configurar a recuperação após desastre de VMs de Hyper-V no local em nuvens VMM para o Azure

O [do Azure Site Recovery](site-recovery-overview.md) serviço contribui para a sua estratégia de recuperação após desastre, gerir e da orquestração de replicação, ativação pós-falha e a reativação pós-falha de máquinas no local e máquinas de virtuais (VMs) do Azure.

Este tutorial mostra como configurar a recuperação após desastre de VMs de Hyper-V no local para o Azure. O tutorial é relevante para as VMs de Hyper-V que são geridos pelo System Center Virtual Machine Manager (VMM). Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Selecione a origem de replicação e de destino.
> * Configure o ambiente de replicação de origem, incluindo os componentes da recuperação de Site no local e o ambiente de replicação de destino.
> * Configure o mapeamento da rede, para mapeamento entre redes de VM do VMM e redes virtuais do Azure.
> * Criar uma política de replicação
> * Ativar a replicação para uma VM

Este é o terceiro tutorial uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Preparar no local Hyper-V](tutorial-prepare-on-premises-hyper-v.md)

Antes de começar, é útil [rever a arquitetura](concepts-hyper-v-to-azure-architecture.md) para este cenário de recuperação após desastre.



## <a name="select-a-replication-goal"></a>Selecione um objetivo de replicação

1. No **todos os serviços** > **cofres dos serviços de recuperação**, clique no nome do cofre utilizamos nestes tutoriais **ContosoVMVault**.
2. No **introdução**, clique em **recuperação de Site**. Em seguida, clique em **preparar infraestrutura**
3. No **objetivo de proteção** > **onde estão as máquinas localizadas**, selecione **no local**.
4. No **onde pretende replicar as máquinas**, selecione **para o Azure**.
5. No **são as máquinas virtualizadas**, selecione **Sim, com o Hyper-V**.
6. No **está a utilizar o System Center VMM**, selecione **Sim**. Em seguida, clique em **OK**.

    ![Objetivo de replicação](./media/tutorial-hyper-v-vmm-to-azure/replication-goal.png)



## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Quando configurar o ambiente de origem, pode instalar o fornecedor do Azure Site Recovery e agente dos serviços de recuperação do Azure e registar servidores no local no cofre. 

1. No **preparar infraestrutura**, clique em **origem**.
2. Em **Preparar a origem**, clique em **+ VMM** para adicionar um servidor VMM. No **Adicionar servidor**, verifique se **servidor VMM do System Center** aparece no **tipo de servidor**.
3. Transferir o instalador do Microsoft Azure Site Recovery Provider.
4. Transferir a chave de registo do cofre. Isto tem quando executar a configuração do fornecedor. A chave é válida durante cinco dias depois de gerá-la.
5. Transferir o agente dos serviços de recuperação.

    ![Transferência](./media/tutorial-hyper-v-vmm-to-azure/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Instalar o Fornecedor no servidor VMM

1. No Assistente de configuração do Azure Site Recovery Provider > **Microsoft Update**, optar por utilizar o Microsoft Update para procurar as atualizações do fornecedor.
2. No **instalação**, aceite a localização de instalação predefinida para o fornecedor e, em **instalar**. 
3. Após a instalação, no Assistente de registo do Microsoft Azure Site Recovery > **as definições do cofre**, clique em **procurar**e, em **ficheiro de chave**, selecione a chave do Cofre de ficheiros que transferidos.
4. Especifique a subscrição do Azure Site Recovery e o nome do cofre (**ContosoVMVault**). Especifique um nome amigável para o servidor do VMM, a identificá-lo no cofre.
5. No **as definições de Proxy**, selecione **ligar diretamente ao Azure Site Recovery sem um proxy**.
6. Aceite a localização predefinida para o certificado que é utilizado para encriptar dados. Serão possível desencriptar dados encriptados ao efetuar a ativação pós-falha.
7. No **sincronizar metadados da nuvem**, selecione **sincronizar metadados da nuvem para o portal de recuperação de Site**. Esta ação só deverá ocorrer uma vez em cada servidor. Em seguida, clique em **registar**.
8. Depois do servidor está registado no cofre, clique em **concluir**.

Após a conclusão de registo, os metadados do servidor são obtidos pelo Azure Site Recovery e o servidor VMM é apresentado na **infraestrutura de recuperação de Site**.

### <a name="install-the-recovery-services-agent"></a>Instalar o agente dos serviços de recuperação

Instale o agente em cada anfitrião de Hyper-V que contenha VMs que pretende replicar.

1. No Assistente de configuração do agente de recuperação serviços Microsoft Azure > **verificação de pré-requisitos**, clique em **seguinte**. Os pré-requisitos em falta serão instalados automaticamente.
2. No **definições de instalação**, aceite a localização de instalação e a localização da cache. A unidade de cache necessita de, pelo menos, 5 GB de armazenamento. Recomendamos que uma unidade com 600 GB ou mais de espaço livre. Em seguida, clique em **Instalar**.
3. No **instalação**, quando a instalação estiver concluída, clique em **fechar** para concluir o assistente.

    ![Instalar agente](./media/tutorial-hyper-v-vmm-to-azure/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

1. Clique em **preparar a infraestrutura** > **destino**.
2. Selecione a subscrição e o grupo de recursos (**ContosoRG**) no qual as VMs do Azure serão criadas após a ativação pós-falha.
3. Selecione o **Gestor de recursos "** modelo de implementação.

A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.


## <a name="configure-network-mapping"></a>Configurar o mapeamento da rede

1. Em **Infraestrutura do Site Recovery** > **Mapeamentos da Rede** > **Mapeamento da Rede**, clique no ícone **+ Mapeamento da Rede**.
2. No **adicionar mapeamento da rede**, selecione o servidor do VMM de origem. Selecione **Azure** como destino.
3. Verifique a subscrição e o modelo de implementação, após a ativação pós-falha.
4. No **rede de origem**, selecione a rede VM de origem no local.
5. No **rede de destino**, selecione a rede Azure no qual as VMs da réplica do Azure estarão localizada quando são criados após a ativação pós-falha. Em seguida, clique em **OK**.

    ![Mapeamento da rede](./media/tutorial-hyper-v-vmm-to-azure/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Configurar uma política de replicação

1. Clique em **preparar a infraestrutura** > **as definições de replicação** > **+ criar e associar**.
2. No **criar e associar política**, especifique um nome de política, **ContosoReplicationPolicy**.
3. Deixe as predefinições e clique em **OK**.
    - **Copiar frequência** indica que delta dados (após a replicação inicial) serão replicadas a cada cinco minutos.
    - **Retenção do ponto de recuperação** indica que o windows de retenção para cada ponto de recuperação será dois duas horas.
    - **Frequência de instantâneos consistentes com aplicação** indica que os pontos de recuperação que contêm os instantâneos consistentes da aplicação serão criados a cada hora.
    - **Hora de início da replicação inicial**, indica que a replicação inicial será imediatamente iniciada.
    - **Encriptar dados armazenados no Azure** -predefinição **desativar** definição indica que Inativos dados no Azure não estão encriptados.
4. Após a criação da política, clique em **OK**. Quando cria uma nova política, automaticamente fica associada à nuvem do VMM.

## <a name="enable-replication"></a>Ativar a replicação

1. No **replicar aplicação**, clique em **origem**. 
2. No **origem**, selecione a nuvem do VMM. Em seguida, clique em **OK**.
3. No **destino**, certifique-se do Azure como o destino, a subscrição do cofre e selecione o **Resource Manager** modelo.
4. Selecione o **contosovmsacct1910171607** conta de armazenamento e o **ContosoASRnet** rede do Azure.
5. No **máquinas virtuais** > **selecione**, selecione a VM que pretende replicar. Em seguida, clique em **OK**.

 Pode controlar o progresso do **ativar proteção** ação no **tarefas** > **as tarefas de recuperação de Site**. Depois do **finalizar proteção** tarefa é concluída, a replicação inicial está concluída e a VM está preparada para ativação pós-falha.


## <a name="next-steps"></a>Passos Seguintes
[Executar um teste de recuperação após desastre](tutorial-dr-drill-azure.md)
