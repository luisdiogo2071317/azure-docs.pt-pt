---
title: "Configurar a recuperação após desastre de VMs de Hyper-V (sem VMM) no local para o Azure com o Azure Site Recovery | Microsoft Docs"
description: "Saiba como configurar a recuperação após desastre de VMs de Hyper-V (sem VMM) no local para o Azure com o serviço Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 02/14/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: b946964c162f47a283c37c6eae7e7152e27b6033
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/22/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Configurar a recuperação após desastre de VMs de Hyper-V no local para o Azure

O serviço [Azure Site Recovery](site-recovery-overview.md) contribui para a estratégia de recuperação após desastre, através da gestão e orquestração de replicação, ativação pós-falha e reativação pós-falha de máquinas no local e máquinas virtuais (VMs) do Azure.

Este tutorial mostra como configurar a recuperação após desastre de VMs de Hyper-V no local para o Azure. O tutorial é relevante para as VMs de Hyper-V que não são geridas pelo System Center Virtual Machine Manager (VMM). Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Selecionar a origem e o destino da replicação.
> * Configurar o ambiente de replicação de origem, incluindo os componentes do Site Recovery no local e o ambiente de replicação de destino.
> * Criar uma política de replicação.
> * Ativar a replicação para uma VM.

Este é o terceiro tutorial de uma série. Este tutorial parte do princípio de que já concluiu as tarefas nos tutoriais anteriores:

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Prepara Hyper-V no local](tutorial-prepare-on-premises-hyper-v.md)

Antes de começar, é útil [rever a arquitetura](concepts-hyper-v-to-azure-architecture.md) deste cenário de recuperação após desastre.

## <a name="select-a-replication-goal"></a>Selecionar um objetivo de replicação


1. Em **Todos os Serviços** > **Cofres dos Serviços de Recuperação**, clique no nome do cofre que preparamos no tutorial anterior, **ContosoVMVault**.
2. Em **Introdução**, clique em **Site Recovery**. Em seguida, clique em **Preparar Infraestrutura**
3. Em **Objetivo de proteção** > **Onde estão localizadas as máquinas virtuais**, selecione **No local**.
4. Em **Para onde pretende replicar as máquinas**, selecione **Para o Azure**.
5. Em **As suas máquinas estão virtualizadas?**, selecione **Não**. Em seguida, clique em **OK**.

    ![Objetivo da replicação](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="set-up-the-source-environment"></a>Configurar o ambiente de origem

Para configurar o ambiente de origem, adicione anfitriões Hyper-V a um site Hyper-V, transfira e instale o Fornecedor do Azure Site Recovery e o agente dos Serviços de Recuperação do Azure e registe o site Hyper-V no cofre. 

1. Em **Preparar Infraestrutura**, clique em **Origem**.
2. Clique em **+Site Hyper-V** e especifique o nome do site que criamos no tutorial anterior, **ContosoHyperVSite**.
3. Clique em **+Servidor Hyper-V**.
4. Transfira o ficheiro de configuração do Fornecedor.
5. Transfira a chave de registo do cofre. Precisará dela quando executar a configuração do Fornecedor. A chave é válida durante cinco dias depois de gerá-la.

    ![Transferir o Fornecedor](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Instalar o Fornecedor

Execute o ficheiro de configuração do Fornecedor (AzureSiteRecoveryProvider.exe) em cada anfitrião Hyper-V adicionado ao site **ContosoHyperVSite**. A configuração instala o Fornecedor do Azure Site Recovery e o agente dos Serviços de Recuperação em cada anfitrião Hyper-V.

1. No assistente de Configuração do Fornecedor do Azure Site Recovery > **Microsoft Update**, opte por utilizar o Microsoft Update para procurar atualizações do Fornecedor.
2. Em **Instalação**, aceite a localização de instalação predefinida para o Fornecedor e o agente e, em seguida, clique em **Instalar**.
3. Após a instalação, no Assistente de Registo do Microsoft Azure Site Recovery > **Definições do Cofre**, clique em **Procurar** e, em **Ficheiro de Chave**, selecione o ficheiro de chave do cofre transferido. 
4. Especifique a subscrição do Azure Site Recovery, o nome do cofre (**ContosoVMVault**) e o site Hyper-V (**ContosoHyperVSite**) ao qual pertence o servidor Hyper-V.
5. Em **Definições de Proxy**, selecione **Ligar diretamente ao Azure Site Recovery sem um proxy**.
6. Em **Registo**, após registar o servidor no cofre, clique em **Concluir**.

Os metadados do servidor Hyper-V são obtidos pelo Azure Site Recovery e o servidor é apresentado em **Infraestrutura do Site Recovery** > **Anfitriões Hyper-V**. Esta ação pode demorar até 30 minutos.


## <a name="set-up-the-target-environment"></a>Configurar o ambiente de destino

Selecione e verifique os recursos de destino. 

1. Clique em **Preparar a Infraestrutura** > **Destino**.
2. Selecione a subscrição e o grupo de recursos **ContosoRG**, no qual as VMs do Azure serão criadas após a ativação pós-falha.
3. Selecione o modelo de implementação **Resource Manager"**.

A Recuperação de Sites verifica que tem uma ou mais contas de armazenamento e redes do Azure compatíveis.


## <a name="set-up-a-replication-policy"></a>Configurar uma política de replicação

1. Clique em **Preparar a infraestrutura** > **Definições de Replicação** > **+Criar e associar**.
2. Em **Criar e associar política**, especifique um nome para a política, **ContosoReplicationPolicy**.
3. Mantenha as predefinições e clique em **OK**.
    - **Copiar frequência** indica que os dados delta (após a replicação inicial) serão replicados a cada cinco minutos.
    - **Retenção do ponto de recuperação** indica que as janelas de retenção de cada ponto de recuperação serão de duas horas.
    - **Frequência de instantâneos consistentes com a aplicação** indica que os pontos de recuperação que contêm instantâneos consistentes com a aplicação serão criados a cada hora.
    - **Hora de início da replicação inicial** indica que a replicação inicial será imediatamente iniciada.
4. Após a criação da política, clique em **OK**. Quando cria uma nova política, esta fica automaticamente associada ao site Hyper-V especificado (**ContosoHyperVSite**)

    ![Política de replicação](./media/hyper-v-azure-tutorial/replication-policy.png)


## <a name="enable-replication"></a>Ativar a replicação


1. Em **Replicar aplicação**, clique em **Origem**. 
2. Em **Origem**, selecione o site **ContosoHyperVSite**. Em seguida, clique em **OK**.
3. Em **Destino**, verifique o Azure como o destino, a subscrição do cofre e o modelo de implementação **Resource Manager**.
4. Selecione a conta de armazenamento **contosovmsacct1910171607** que criamos no tutorial anterior para os dados replicados e a rede **ContosoASRnet**, na qual as VMs do Azure estarão localizadas após a ativação pós-falha.
5. Em **Máquinas virtuais** > **Selecionar**, selecione a VM que pretende replicar. Em seguida, clique em **OK**.

 Pode controlar o progresso da ação **Ativar Proteção** em **Tarefas** > **Tarefas do Site Recovery**. Após a conclusão da tarefa **Finalizar Proteção**, a replicação inicial estará concluída e a máquina virtual estará preparada para a ativação pós-falha.

## <a name="next-steps"></a>Passos seguintes
[Executar um teste de recuperação após desastre](tutorial-dr-drill-azure.md)
