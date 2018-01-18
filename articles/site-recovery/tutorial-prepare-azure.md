---
title: "Criar recursos para utilização com o Azure Site Recovery | Microsoft Docs"
description: "Saiba como preparar o Azure para a replicação das máquinas no local utilizando o serviço do Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 63290127b298efced14ad34e9223840f3229f046
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="prepare-azure-resources-for-replication-of-on-premises-machines"></a>Preparar os recursos do Azure para a replicação de máquinas no local

O [do Azure Site Recovery](site-recovery-overview.md) serviço contribui para a sua estratégia de recuperação (BCDR) de continuidade e desastre negócio ao manter as suas aplicações empresariais a cópia de segurança e em execução durante falhas planeadas e não. Recuperação de site gere e orquestra a recuperação de desastre de máquinas no local e máquinas virtuais do Azure (VMs), incluindo a replicação, ativação pós-falha e recuperação.

Este tutorial mostra como preparar os componentes do Azure quando pretende replicar VMs no local (Hyper-V ou VMware) ou servidores físicos Windows/Linux para o Azure. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Certifique-se de que a conta possui permissões de replicação
> * Crie uma conta do Storage do Azure.
> * Defina uma rede do Azure. Quando as VMs do Azure são criadas após a ativação pós-falha, estes estão associados a esta rede do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="verify-account-permissions"></a>Verifique as permissões de conta

Se acabou de criar a conta gratuita do Azure, em seguida, é o administrador da sua subscrição. Se não tiver o administrador de subscrição, trabalhar com o administrador atribuir as permissões que precisa. Para ativar a replicação para uma nova máquina virtual, tem de ter:

- Permissão para criar uma VM no grupo de recursos selecionado
- Permissão para criar uma VM na rede virtual selecionada
- Permissão para escrever na conta de armazenamento selecionada

A função incorporada 'Contribuinte de Máquina Virtual' com estas permissões. Também necessita de permissão para gerir as operações do Azure Site Recovery. A função 'Contribuinte de recuperação do Site' tem todas as permissões necessárias para gerir as operações de recuperação de Site num cofre dos serviços de recuperação.

## <a name="create-a-storage-account"></a>Criar uma conta do Storage

Imagens de máquinas replicadas são guardadas no armazenamento do Azure. As VMs do Azure são criadas do armazenamento, quando a ativação pós-falha no local para o Azure.

1. No [portal do Azure](https://portal.azure.com) menu, clique em **novo** -> **armazenamento** -> **conta de armazenamento**.
2. No **criar conta de armazenamento**, introduza um nome para a conta. Para estes tutoriais utilizamos o nome **contosovmsacct1910171607**. O nome tem de ser exclusivo no Azure e de ter entre 3 e 24 carateres, com apenas letras minúsculas e números.
3. Utilize o **Resource Manager** modelo de implementação.
4. Selecione **fins gerais** > **padrão**. Não selecione o armazenamento de Blobs.
5. Selecionar a predefinição **RA-GRS** para a redundância de armazenamento.
6. Selecione a subscrição na qual pretende criar a nova conta do Storage.
7. Especifique um novo grupo de recursos. Um grupo de recursos do Azure é um contentor lógico no qual os recursos do Azure são implementados e geridos. Para estes tutoriais, utilizamos o nome **ContosoRG**.
8. Selecione a localização geográfica para a conta do Storage. A conta de armazenamento tem de ser na mesma região que o Cofre dos serviços de recuperação. Para estes tutoriais, utilizamos o **Europa Ocidental** região.

   ![create-storageacct](media/tutorial-prepare-azure/create-storageacct.png)

9. Clique em **Criar** para criar a conta do Storage.

## <a name="create-a-vault"></a>Criar um cofre

1. No menu do portal do Azure, clique em **novo** > **de monitorização e gestão** >
    **cópia de segurança e recuperação de Site**.
2. Em **Nome**, especifique um nome amigável para identificar o cofre. Para este tutorial utilizamos **ContosoVMVault**.
3. Selecione o grupo de recursos existente com o nome **contosoRG**.
4. Especificar a região do Azure **Europa Ocidental**, que estamos a utilizar neste conjunto de tutoriais.
5. Para aceder rapidamente ao Cofre a partir do dashboard, clique em **afixar ao dashboard** > **criar**.

   ![Novo cofre](./media/tutorial-prepare-azure/new-vault-settings.png)

   O novo cofre é apresentado no **Dashboard** > **todos os recursos**e no principal **cofres dos serviços de recuperação** página.

## <a name="set-up-an-azure-network"></a>Configurar uma rede do Azure

Quando as VMs do Azure criadas a partir de armazenamento após a ativação pós-falha, se estiver a associado a esta rede.

1. No [portal do Azure](https://portal.azure.com) menu, clique em **novo** > **redes** >
    **rede Virtual**
2. Deixe **Resource Manager** selecionado como o modelo de implementação. Gestor de recursos é o modelo de implementação preferenciais.
   - Especifique um nome de rede. O nome tem de ser exclusivo dentro do grupo de recursos do Azure. Iremos utilizar o nome **ContosoASRnet**
   - Utilize o grupo de recursos existente **contosoRG**.
   - Especifique a rede endereço intervalo 10.0.0.0/24.
   - Para este tutorial, não precisa de uma sub-rede.
   - Selecione a subscrição na qual pretende criar a rede.
   - Selecione a localização **Europa Ocidental**. A rede tem de estar na mesma região que o cofre de Serviços de Recuperação.
3. Clique em **Criar**.

   ![create-network](media/tutorial-prepare-azure/create-network.png)

   A rede virtual demora alguns segundos a criar. Depois de criado, vê-lo no dashboard do portal do Azure.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Preparar a infraestrutura do VMware no local para a recuperação após desastre para o Azure](tutorial-prepare-on-premises-vmware.md)
