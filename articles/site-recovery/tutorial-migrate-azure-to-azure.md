---
title: "Migrar VMs do Azure entre regiões do Azure utilizando o Azure Site Recovery | Microsoft Docs"
description: "Utilize o Azure Site Recovery para migrar VMs IaaS do Azure de uma região do Azure para outro."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.openlocfilehash: e7b925d2daed11ee4e070cda6bcbd4a3511d9c17
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/10/2018
---
# <a name="migrate-azure-vms-to-another-region"></a>Migrar VMs do Azure para outra região

Além de utilizar o [do Azure Site Recovery](site-recovery-overview.md) serviço para gerir e orquestrar recuperação após desastre de máquinas no local e as VMs do Azure para fins de continuidade do negócio e recuperação após desastre (BCDR), também pode utilizar o Site Recuperação para gerir a migração de VMs do Azure para uma região secundária. Para migrar as VMs do Azure, pode ativar a replicação para os mesmos e falha-los da região primária para a região secundária da sua preferência.

Este tutorial mostra como migrar as VMs do Azure para noutra região. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um cofre dos serviços de recuperação
> * Ativar a replicação para uma VM
> * Executar uma ativação pós-falha para migrar a VM

Este tutorial presumes que já tiver uma subscrição do Azure. Se não, criar um [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.

>[!NOTE]
>
> Replicação de recuperação de site para as VMs do Azure está atualmente em pré-visualização.



## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, terá de VMs do Azure numa região do Azure que pretende migrar. Além disso, existem várias definições, deverá verificar antes de começar.


### <a name="verify-target-resources"></a>Certifique-se de recursos de destino

1. Certifique-se de que a sua subscrição do Azure permite-lhe criar VMs na região de destino utilizada para recuperação após desastre. Contacte o suporte para ativar a quota necessária.

2. Certifique-se a que sua subscrição tem recursos suficientes para suportar as VMs com tamanhos correspondem as VMs de origem. Recuperação de site escolhe o mesmo tamanho ou o tamanho mais próximo possível da VM de destino.


### <a name="verify-account-permissions"></a>Verifique as permissões de conta

Se acabou de criar a conta gratuita do Azure é o administrador da sua subscrição. Se não tiver o administrador de subscrição, trabalhar com o administrador atribuir as permissões que precisa. Para ativar a replicação para uma nova VM, tem de ter:

1. Permissões para criar uma VM em recursos do Azure. A função incorporada 'Contribuinte de Máquina Virtual' com estas permissões, que incluem:
    - Permissão para criar uma VM no grupo de recursos selecionado
    - Permissão para criar uma VM na rede virtual selecionada
    - Permissão para escrever na conta de armazenamento selecionada

2. Também necessita de permissão para gerir as operações do Azure Site Recovery. A função 'Contribuinte de recuperação do Site' tem todas as permissões necessárias para gerir as operações de recuperação de Site num cofre dos serviços de recuperação.


### <a name="verify-vm-outbound-access"></a>Verificar o acesso de saída de VM

1. Certifique-se que não estiver a utilizar um proxy de autenticação para controlar a conectividade de rede para as VMs que pretende migrar. 
2. Para efeitos deste tutorial, iremos partem do princípio de que as VMs que pretende migrar pode aceder à internet e não estiver a utilizar um proxy de firewall para controlar o acesso de saída. Se estiver, verifique os requisitos [aqui](azure-to-azure-tutorial-enable-replication.md#configure-outbound-network-connectivity).

### <a name="verify-vm-certificates"></a>Verificar os certificados VM

Verifique se todos os certificados de raiz mais recentes estão presentes nas VMs do Azure que pretende migrar. Se os certificados de raiz mais recentes não estão, a VM não é possível registar a recuperação de sites, devido a restrições de segurança.

- Para VMs do Windows, instale todas as atualizações mais recentes do Windows na VM, para que todos os certificados de raiz fidedigna no computador. Num ambiente desligado, a seguir o padrão de Windows Update e os processos de atualização de certificados para a sua organização.
- Para VMs com Linux, siga as orientações fornecidas pelo distribuidor do Linux, para obter os certificados de raiz fidedigna mais recentes e a lista de revogação de certificados na VM.



## <a name="create-a-vault"></a>Criar um cofre

Crie o cofre em qualquer região, exceto a região de origem.

1. Inicie sessão no [portal do Azure](https://portal.azure.com) > **Serviços de Recuperação**.
2. Clique em **Novo** > **Monitorização e Gestão** > **Backup e Site Recovery**.
3. No **nome**, especifique o nome amigável **ContosoVMVault**. Se tiver mais do que uma subscrição, selecione a adequado.
4. Criar um grupo de recursos **ContosoRG**.
5. Selecione uma região do Azure. Para verificar as regiões suportadas, veja a disponibilidade geográfica em [Detalhes dos Preços do Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Para aceder rapidamente ao Cofre a partir do dashboard, clique em **afixar ao dashboard** e, em seguida, clique em **criar**.

   ![Novo cofre](./media/tutorial-migrate-azure-to-azure/azure-to-azure-vault.png)

O novo cofre é adicionado ao **Dashboard** em **todos os recursos**e no principal **cofres dos serviços de recuperação** página.






## <a name="select-the-source"></a>Selecione a origem

1. No cofres dos serviços de recuperação, clique em **ConsotoVMVault** > **+ replicar**.
2. No **origem**, selecione **Azure - pré-visualização**.
3. No **localização de origem**, selecione a origem de região do Azure onde as VMs estão a ser executados.
4. Selecione o modelo de implementação Resource Manager. Em seguida, selecione o **grupo de recursos de origem**.
5. Clique em **OK** para guardar as definições.


## <a name="enable-replication-for-azure-vms"></a>Ativar a replicação para as VMs do Azure

Recuperação de site obtém uma lista de VMs associados à subscrição e grupo de recursos.


1. No portal do Azure, clique em **máquinas virtuais**.
2. Selecione a VM que pretende migrar. Em seguida, clique em **OK**.
3. No **definições**, clique em **recuperação após desastre (pré-visualização)**.
4. No **configurar a recuperação de desastre** > **região de destino** selecione a região de destino para a qual irá replicar.
5. Para este tutorial, aceite as outras predefinições.
6. Clique em **ativar a replicação**. Esta ação inicia uma tarefa para ativar a replicação para a VM.

    ![Ativar a replicação](media/tutorial-migrate-azure-to-azure/settings.png)

>[!NOTE]
  >
  > Atualmente, a replicação de VMs do Azure com discos geridos não é suportada. 

## <a name="run-a-failover"></a>Executar uma ativação pós-falha

1. No **definições** > **replicado itens**, clique na máquina e, em seguida, clique em **ativação pós-falha**.
2. No **ativação pós-falha**, selecione **mais recente**. A definição de chave de encriptação não é relevante para este cenário.
3. Selecione **encerrar a máquina antes de iniciar a ativação pós-falha**. Recuperação de site tenta encerre a VM de origem antes de acionar a ativação pós-falha. Ativação pós-falha continua, mesmo se falha de encerramento. Pode seguir o progresso de ativação pós-falha no **tarefas** página.
4. Certifique-se de que a VM do Azure é apresentada no Azure conforme esperado.
5. No **replicado itens**, clique com o botão direito a VM > **concluir a migração**. Esta ação termina o processo de migração e deixa de replicação para a VM.



## <a name="next-steps"></a>Passos Seguintes

Neste tutorial migradas uma VM do Azure para uma região do Azure diferente. Agora pode configurar a recuperação de desastres para a VM migrada.

> [!div class="nextstepaction"]
> [Configurar a recuperação após desastre após a migração](azure-to-azure-quickstart.md)

