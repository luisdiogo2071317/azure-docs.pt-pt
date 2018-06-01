---
title: Migrar VMs do Azure entre regiões do Azure com o Azure Site Recovery | Microsoft Docs
description: Utilize o Azure Site Recovery para migrar VMs IaaS do Azure de uma região do Azure para outra.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 05/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 0460d595bc70ec09d492221485749ece32ec07df
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
ms.locfileid: "34209713"
---
# <a name="migrate-azure-vms-to-another-region"></a>Migrar VMs do Azure para outra região

Além de utilizar o serviço [Azure Site Recovery](site-recovery-overview.md) para gerir e orquestrar a recuperação após desastre de máquinas locais e VMs do Azure para fins de continuidade do negócio e recuperação após desastre (BCDR), também pode utilizar o Site Recovery para gerir a migração de VMs do Azure para uma região secundária. Para migrar VMs do Azure, ative a replicação para as mesmas e realize a ativação pós-falha a partir da região primária para a região secundária da sua preferência.

Este tutorial mostra como migrar as VMs do Azure para outra região. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Crie um cofre dos Serviços de recuperação
> * Ativar replicação para uma VM
> * Executar uma ativação pós-falha para migrar a VM

Este tutorial pressupõe que já tem uma subscrição do Azure. Se não tiver, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.

>[!NOTE]
>
> A replicação do Site Recovery para VMs do Azure está atualmente em pré-visualização.



## <a name="prerequisites"></a>Pré-requisitos

- Certifique-se de que tem VMs do Azure na região do Azure a partir do qual pretende migrar.
- Certifique-se de que compreende a [arquitetura e os componentes do cenário](azure-to-azure-architecture.md).
- Veja os [requisitos e limitações de suporte](azure-to-azure-support-matrix.md).



## <a name="before-you-start"></a>Antes de começar

Antes de configurar a replicação, conclua estes passos.


### <a name="verify-target-resources"></a>Verificar recursos de destino

1. Certifique-se de que a sua subscrição do Azure permite criar VMs na região de destino utilizada para recuperação após desastre. Contacte o suporte para ativar a quota necessária.

2. Certifique-se de que a sua subscrição tem recursos suficientes para suportar as VMs com tamanhos que correspondem às VMs de origem. O Site Recovery escolhe o mesmo tamanho ou o tamanho mais próximo possível da VM de destino.


### <a name="verify-account-permissions"></a>Verificar permissões de conta

Se acabou de criar a conta gratuita do Azure, então é o administrador da sua subscrição. Se não for o administrador da subscrição, combine com o administrador para atribuir as permissões que precisa. Para ativar a replicação para uma nova VM, tem de ter:

1. Permissões para criar uma VM nos recursos do Azure. A função incorporada "Contribuinte de Máquina Virtual" tem estas permissões, que incluem:
    - Permissão para criar uma VM no grupo de recursos selecionado
    - Permissão para criar uma VM na rede virtual selecionada
    - Permissão para escrever na conta de armazenamento selecionada

2. Também precisa de permissão para gerir as operações do Azure Site Recovery. A função "Contribuinte do Site Recovery" tem todas as permissões precisas para gerir as operações do Azure Site Recovery num cofre dos Serviços de Recuperação.


### <a name="verify-vm-outbound-access"></a>Verificar acesso de saída da VM

1. Certifique-se de que não está a utilizar um proxy de autenticação para controlar a conectividade de rede para as VMs que pretende migrar. 
2. Para efeitos deste tutorial, iremos partir do princípio de que as VMs que pretende migrar podem aceder à Internet e não estão a utilizar um proxy de firewall para controlar o acesso de saída. Se assim for, verifique os requisitos [aqui](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

### <a name="verify-vm-certificates"></a>Verificar os certificados da VM

Verifique se todos os certificados de raiz mais recentes estão presentes nas VMs do Azure que pretende migrar. Se os certificados de raiz mais recentes não estiverem presentes, não poderá registar a VM no Site Recovery, devido a restrições de segurança.

- Para VMs do Windows, instale todas as atualizações mais recentes do Windows na VM, para que todos os certificados de raiz fidedigna fiquem na máquina. Num ambiente desligado, siga o padrão do Windows Update e os processos de atualização de certificados da sua organização.
- Para VMs do Linux, siga a documentação de orientação fornecida pelo distribuidor do Linux, para obter os certificados de raiz fidedigna mais recentes e a lista de revogação de certificados na VM.



## <a name="create-a-vault"></a>Criar um cofre

Crie o cofre em qualquer região, exceto na região de origem.

1. Inicie sessão no [portal do Azure](https://portal.azure.com) > **Serviços de Recuperação**.
2. Clique em **Criar um recurso** > **Monitorização e Gestão** > **Backup e Site Recovery**.
3. No **Nome**, especifique o nome amigável **ContosoVMVault**. Se tiver mais do que uma subscrição, selecione a que for adequada.
4. Crie um grupo de recursos **ContosoRG**.
5. Selecione uma região do Azure. Para verificar as regiões suportadas, veja a disponibilidade geográfica em [Detalhes dos Preços do Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Para aceder rapidamente ao cofre a partir do dashboard, clique em **Afixar ao dashboard** e clique em **Criar**.

   ![Novo cofre](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

O cofre novo é adicionado ao **Dashboard** em **Todos os recursos** e na página principal **Cofres dos Serviços de Recuperação**.






## <a name="select-the-source"></a>Selecionar a origem

1. Nos cofres dos Serviços de Recuperação, clique em **ConsotoVMVault** > **+Replicar**.
2. Em **Origem**, selecione **Azure**.
3. Em **Localização de origem**, selecione a região do Azure de origem onde as VMs estão atualmente a ser executadas.
4. Selecione o modelo de implementação do Resource Manager. Em seguida, selecione o **Grupo de recursos de origem**.
5. Clique em **OK** para guardar as definições.


## <a name="enable-replication-for-azure-vms"></a>Ativar a replicação das VMs do Azure

O Site Recovery obtém uma lista das VMs associadas à subscrição e o grupo de recursos.


1. No portal do Azure, clique em **Máquinas virtuais**.
2. Selecione a VM que pretende migrar. Em seguida, clique em **OK**.
3. Nas **Definições**, clique em **Recuperação após desastre**.
4. Em **Configurar recuperação após desastre** > **Região de destino**, selecione a região de destino para a qual irá replicar.
5. Para este tutorial, aceite as outras predefinições.
6. Clique em **Ativar replicação**. Esta ação inicia uma tarefa para ativar a replicação para a VM.

    ![ativar replicação](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="run-a-failover"></a>Executar uma ativação pós-falha

1. Em **Definições** > **Itens replicados**, clique na máquina e, em seguida, clique em **Ativação pós-falha**.
2. Em **Ativação pós-falha**, selecione **Mais recente**. A definição da chave de encriptação não é relevante para este cenário.
3. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha**. O Site Recovery tenta encerrar a VM de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. Pode seguir o progresso da ativação pós-falha na página **Tarefas**.
4. Certifique-se de que a VM do Azure é apresentada no Azure conforme esperado.
5. Em **Itens replicados**, clique com o botão direito do rato na VM > **Consolidar**. Isto conclui o processo de migração,
6. Quando a consolidação terminar, clique em **Desativar Replicação**.  Isto para a replicação para a VM.



## <a name="next-steps"></a>Passos seguintes

Neste tutorial, migrou uma VM do Azure para uma região diferente do Azure. Agora pode configurar a recuperação após desastre para a VM migrada.

> [!div class="nextstepaction"]
> [Configurar a recuperação após desastre após a migração](azure-to-azure-quickstart.md)

