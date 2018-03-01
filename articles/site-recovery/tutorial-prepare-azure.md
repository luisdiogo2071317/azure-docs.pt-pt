---
title: "Criar recursos para utilização com o Azure Site Recovery | Microsoft Docs"
description: "Aprenda a preparar o Azure para a replicação de máquinas no local com o serviço Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: d1aadd6b44d64f0bdb35ea02d628bedfc366ad3c
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Preparar os recursos do Azure para a replicação de máquinas no local

O serviço [Azure Site Recovery](site-recovery-overview.md) contribui para a sua estratégia de continuidade comercial e recuperação após desastre (BCDR) ao manter as suas aplicações empresariais em funcionamento e durante falhas planeadas e não planeadas. O Site Recovery gere e orquestra a recuperação após desastre de computadores no local e máquinas virtuais (VMs) do Azure, incluindo replicação, ativação pós-falha e recuperação.

Este tutorial mostra como preparar os componentes do Azure quando pretende replicar VMs no local (Hyper-V ou VMware) ou servidores físicos Windows/Linux para o Azure. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Certifique-se de que a conta tem permissões de replicação
> * Crie uma conta do Storage do Azure.
> * Configure uma rede do Azure. Quando as VMs do Azure são criadas após a ativação pós-falha, são associadas a esta rede do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="verify-account-permissions"></a>Verificar permissões de conta

Se acabou de criar a conta gratuita do Azure, então é o administrador da sua subscrição. Se não for o administrador da subscrição, combine com o administrador para atribuir as permissões que precisa. Para ativar a replicação para uma nova máquina virtual, tem de ter:

- Permissão para criar uma VM no grupo de recursos selecionado
- Permissão para criar uma VM na rede virtual selecionada
- Permissão para escrever na conta de armazenamento selecionada

A função incorporada "Contribuinte de Máquina Virtual" tem estas permissões. Também precisa de permissão para gerir as operações do Azure Site Recovery. A função "Contribuinte do Site Recovery" tem todas as permissões precisas para gerir as operações do Azure Site Recovery num cofre dos Serviços de Recuperação.

## <a name="create-a-storage-account"></a>Criar uma conta do Storage

As imagens das máquinas replicadas são guardadas no armazenamento do Azure. As VMs do Azure são criadas a partir do armazenamento quando fizer a ativação pós-falha do local para o Azure.

1. No menu do [portal do Azure](https://portal.azure.com), clique em **Nova** -> **Armazenamento** -> **Conta de armazenamento**.
2. Em **Criar conta de armazenamento**, introduza um nome para a conta. Para estes tutoriais, iremos utilizar o nome **contosovmsacct1910171607**. O nome tem de ser exclusivo no Azure e ter entre 3 e 24 carateres compostos apenas por números e letras minúsculas.
3. Utilize o modelo de implementação **Resource Manager**.
4. Selecione **Fins gerais** > **Standard**. Não selecione o armazenamento de blobs.
5. Selecione a predefinição **RA-GRS** para redundância de armazenamento.
6. Selecione a subscrição na qual pretende criar a nova conta do Storage.
7. Especifique um novo grupo de recursos. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Nestes tutoriais, vamos utilizar o nome **ContosoRG**.
8. Selecione a localização geográfica para a conta do Storage. A conta de armazenamento tem de estar na mesma região que o cofre dos Serviços de Recuperação. Nestes tutoriais, vamos utilizar a região **Europa Ocidental**.

   ![create-storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Clique em **Criar** para criar a conta do Storage.

## <a name="create-a-vault"></a>Criar um cofre

1. No portal do Azure, clique em **Criar um recurso** > **Monitorização + Gestão** > **Backup e Site Recovery**.
2. Em **Nome**, especifique um nome amigável para identificar o cofre. Neste tutorial utilizamos **ContosoVMVault**.
3. Selecione o grupo de recursos existente denominado **contosoRG**.
4. Especifique a região do Azure **Europa Ocidental**, que estamos a utilizar neste conjunto de tutoriais.
5. Para aceder rapidamente ao cofre a partir do dashboard, clique em **Afixar ao dashboard** > **Criar**.

   ![Novo cofre](./media/tutorial-prepare-azure/new-vault-settings.png)

   O cofre novo é apresentado no **Dashboard** > **Todos os recursos** e na página principal **Cofres dos Serviços de Recuperação**.

## <a name="set-up-an-azure-network"></a>Configurar uma rede do Azure

Quando as VMs do Azure são criadas a partir do armazenamento após a ativação pós-falha, são associadas a esta rede.

1. No [portal do Azure](https://portal.azure.com), clique em **Criar um recurso** > **Redes** > **Rede virtual**.
2. Deixe o **Resource Manager** selecionado como o modelo de implementação. O modelo de implementação preferencial é o Resource Manager.
   - Especifique um nome de rede. O nome tem de ser exclusivo dentro do grupo de recursos do Azure. Iremos utilizar o nome **ContosoASRnet**
   - Utilize o grupo de recursos existente **contosoRG**.
   - Especifique o intervalo de endereços de rede 10.0.0.0/24.
   - Não precisamos de uma sub-rede neste tutorial.
   - Selecione a subscrição na que vai criar a rede.
   - Selecione a localização **Europa Ocidental**. A rede tem de estar na mesma região que o cofre de Serviços de Recuperação.
3. Clique em **Criar**.

   ![create-network](media/tutorial-prepare-azure/create-network.png)

   A rede virtual demora alguns segundos a ser criada. Depois de estar criada, pode vê-la no dashboard do portal do Azure.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Preparar infraestrutura VMware no local para a recuperação após desastre para o Azure](tutorial-prepare-on-premises-vmware.md)
