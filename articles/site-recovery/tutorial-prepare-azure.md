---
title: "Criar recursos para utilização com o Azure Site Recovery | Microsoft Docs"
description: "Aprenda a preparar o Azure para a replicação de máquinas no local com o Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 2f6ff1d30eef1fe34e55457d9bdd4295804ec16a
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Preparar os recursos do Azure para a replicação de máquinas no local

 O [Azure Site Recovery](site-recovery-overview.md) contribui para a sua estratégia de continuidade comercial e recuperação após desastre (BCDR) ao manter as suas aplicações empresariais em funcionamento durante falhas planeadas e não planeadas. O Site Recovery gere e orquestra a recuperação após desastre de computadores no local e máquinas virtuais (VMs) do Azure, incluindo replicação, ativação pós-falha e recuperação.

Este tutorial mostra como preparar os componentes do Azure quando pretende replicar VMs no local (Hyper-V ou VMware) ou servidores físicos Windows/Linux para o Azure. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Certifique-se de que a conta tem permissões de replicação.
> * Crie uma conta do Storage do Azure.
> * Configure uma rede do Azure. Quando as VMs do Azure são criadas após a ativação pós-falha, são associadas a esta rede do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no [portal do Azure](http://portal.azure.com).

## <a name="verify-account-permissions"></a>Verificar permissões de conta

Se acabou de criar a conta gratuita do Azure, é o administrador da sua subscrição. Se não for o administrador da subscrição, combine com o administrador para lhe atribuir as permissões de que precisa. Para ativar a replicação para uma nova máquina virtual, tem de ter permissões para:

- Criar uma VM no grupo de recursos selecionado.
- Criar uma VM na rede virtual selecionada.
- Escrever na conta de armazenamento selecionada.

A função incorporada Contribuinte de Máquina Virtual tem estas permissões. Também precisa de permissão para gerir as operações do Site Recovery. A função Contribuinte do Site Recovery tem todas as permissões necessárias para gerir as operações do Azure Site Recovery num cofre dos Serviços de Recuperação.

## <a name="create-a-storage-account"></a>Criar uma conta do Storage

As imagens das máquinas replicadas são guardadas no armazenamento do Azure. As VMs do Azure são criadas a partir do armazenamento quando fizer a ativação pós-falha do local para o Azure.

1. No menu [portal do Azure](https://portal.azure.com), selecione **Novo** > **Armazenamento** > **Conta de armazenamento**.
2. Em **Criar conta de armazenamento**, introduza um nome para a conta. Nestes tutoriais, utilize o nome **contosovmsacct1910171607**. O nome tem de ser exclusivo no Azure e ter entre 3 e 24 carateres compostos apenas por números e letras minúsculas.
3. Em **Modelo de implementação**, selecione **Resource Manager**.
4. Em **Tipo de conta**, selecione **Fins gerais**. Em **Desempenho**, selecione **Padrão**. Não selecione o armazenamento de blobs.
5. Em **Replicação**, selecione o **Armazenamento georredundante com acesso de leitura** predefinido para redundância de armazenamento.
6. Em **Subscrição**, selecione a subscrição na qual pretende criar a nova conta de armazenamento.
7. Em **Grupo de recursos**, introduza um novo grupo de recursos. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Nestes tutoriais, utilize o nome **ContosoRG**.
8. Em **Localização**, selecione a localização geográfica para a conta de armazenamento. A conta de armazenamento tem de estar na mesma região que o cofre dos Serviços de Recuperação. Nestes tutoriais, utilize a região **Europa Ocidental**.

   ![Criar uma conta do Storage](media/tutorial-prepare-azure/create-storageacct.png)

9. Selecione **Criar** para criar a conta de armazenamento.

## <a name="create-a-vault"></a>Criar um cofre

1. No portal do Azure, selecione **Criar um recurso** > **Monitorização + Gestão** > **Backup e Site Recovery**.
2. Em **Nome**, introduza um nome amigável para identificar o cofre. Neste tutorial, utilize **ContosoVMVault**.
3. Em **Grupo de recursos**, selecione o grupo de recursos existente com o nome **contosoRG**.
4. Em **Localização**, especifique a região do Azure **Europa Ocidental** que é utilizada neste conjunto de tutoriais.
5. Para aceder rapidamente ao cofre a partir do dashboard, selecione **Afixar ao dashboard** > **Criar**.

   ![Criar um novo cofre](./media/tutorial-prepare-azure/new-vault-settings.png)

   O novo cofre é apresentado em **Dashboard** > **Todos os recursos** e na página principal **Cofres dos Serviços de Recuperação**.

## <a name="set-up-an-azure-network"></a>Configurar uma rede do Azure

Quando as VMs do Azure são criadas a partir do armazenamento após a ativação pós-falha, são associadas a esta rede.

1. No [portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Redes** > **Rede virtual**.
2. Deixe o **Resource Manager** selecionado como o modelo de implementação. O modelo de implementação preferencial é o Resource Manager. Em seguida, siga estes passos:

   a. Em **Nome**, introduza um nome de rede. O nome tem de ser exclusivo dentro do grupo de recursos do Azure. Utilize o nome **ContosoASRnet**.

   b. Em **Grupo de recursos**, utilize o grupo de recursos existente **contosoRG**.

   c. Em **Intervalo de endereços**, introduza o intervalo de endereços de rede **10.0.0.0/24**.

   d. Neste tutorial, não precisa de uma sub-rede.

   e. Em **Subscrição**, selecione a subscrição na qual vai criar a rede.

   f. Em **Localização**, selecione **Europa Ocidental**. A rede tem de estar na mesma região que o cofre de Serviços de Recuperação.

3. Selecione **Criar**.

   ![Criar uma rede virtual](media/tutorial-prepare-azure/create-network.png)

   A rede virtual demora alguns segundos a ser criada. Depois de estar criada, pode vê-la no dashboard do portal do Azure.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Preparar infraestrutura VMware no local para a recuperação após desastre para o Azure](tutorial-prepare-on-premises-vmware.md)
