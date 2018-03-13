---
title: "Configurar a replicação para as VMs do Azure no Azure Site Recovery | Microsoft Docs"
description: "Este artigo descreve como configurar a replicação para as VMs do Azure, de uma região do Azure para outro utilizando a recuperação de sites."
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/09/2018
ms.author: asgang
ms.openlocfilehash: 39d81ed6408e5f2c434a4fbaa681efc4c0b19a63
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/12/2018
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Replicar máquinas virtuais do Azure para outra região do Azure


>[!NOTE]
>
> Replicação de recuperação de site para máquinas virtuais do Azure está atualmente em pré-visualização.

Este artigo descreve como ativar a replicação de VMs do Azure, de uma região do Azure para outro.

## <a name="prerequisites"></a>Pré-requisitos

Este artigo pressupõe que já configurou a recuperação de sites para este cenário, conforme descrito no [tutorial do Azure para o Azure](azure-to-azure-tutorial-enable-replication.md). Certifique-se de ter preparado os pré-requisitos e criar o Cofre dos serviços de recuperação.



## <a name="enable-replication"></a>Ativar a replicação

Ative a replicação. Este procedimento assume que a região do Azure primária é Ásia Oriental, e a região secundária Sudeste asiático.

1. No cofre, clique em **+ replicar**.
2. Tenha em atenção os seguintes campos:
    - **Origem**: O ponto de origem das VMs, que neste caso é **Azure**.
    - **Localização de origem**: região do Azure a partir do qual pretende proteger as máquinas virtuais. Nesta ilustração, a localização de origem é 'Oriental'
    - **Modelo de implementação**: modelo de implementação do Azure máquinas de origem.
    - **Grupo de recursos**: O grupo de recursos para o qual as máquinas virtuais de origem pertence. Todas as VMs no grupo de recursos selecionado são apresentadas para proteção no próximo passo.

    ![Ativar a replicação](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. No **máquinas virtuais > Selecione as máquinas virtuais**, clique e selecione cada VM que pretende replicar. Só pode selecionar máquinas para as quais a replicação pode ser ativada. Em seguida, clique em **OK**.
    ![Ativar replicação](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. No **definições**, opcionalmente, pode configurar as definições do site de destino:

    - **Localização de destino**: A localização onde os dados de máquina virtual de origem serão replicados. Consoante a sua localização máquinas selecionadas, a recuperação de sites fornece a lista de regiões de destino adequado. Recomendamos que mantenha a localização de destino a mesma como a localização do Cofre de serviços de recuperação.
    - **Grupo de recursos de destino**: O grupo de recursos para que todos os seus máquinas virtuais replicadas pertence. Por predefinição o Azure Site Recovery cria um novo grupo de recursos na região de destino com um nome que o sufixo "asr". No caso de grupo de recursos criado pelo Azure Site Recovery já existe, é reutilizado. Também pode optar por personalizá-la conforme mostrado na secção abaixo.
    - **Rede Virtual de destino**: por predefinição, a recuperação de sites cria uma nova rede virtual na região de destino com um nome que o sufixo "asr". Esta é mapeada para a sua rede de origem e utilizada para qualquer proteção futura. [Saiba mais](site-recovery-network-mapping-azure-to-azure.md) sobre mapeamento da rede.
    - **As contas de armazenamento de destino**: por predefinição, a recuperação de sites cria uma nova conta de armazenamento de destino mimicking a configuração de armazenamento VM de origem. No caso de conta de armazenamento já existe, é reutilizado.
    - **As contas de armazenamento em cache**: recuperação de sites necessita de conta de armazenamento adicional chamou o armazenamento de cache na região de origem. Todas as alterações a acontecer nas VMs de origem são controladas e enviadas para a conta de armazenamento de cache antes de replicar os para a localização de destino.
    - **Conjunto de disponibilidade**: por predefinição, o Azure Site Recovery cria uma novo conjunto de disponibilidade na região de destino com um nome que o sufixo "asr". No caso de conjunto de disponibilidade criado pelo Azure Site Recovery já existe, é reutilizado.
    - **Política de replicação**: define as definições de recuperação ponto retenção histórico e aplicação consistente frequência de instantâneos. Por predefinição, o Azure Site Recovery cria uma nova política de replicação com as predefinições dos ' 24 horas para retenção do ponto de recuperação e ' 60 minutos para a frequência de instantâneos consistentes da aplicação.

    ![Ativar a replicação](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Personalizar a recursos de destino

Pode modificar as definições do destino predefinido utilizadas pela recuperação de sites.

1. Clique em **personalizar:** para modificar as predefinições:
    - No **grupo de recursos de destino**, selecione o grupo de recursos na lista de todos os grupos de recurso na localização de destino da subscrição.
    - No **rede virtual de destino**, selecione a rede de uma lista de todos os a rede virtual na localização de destino.
    - No **do conjunto de disponibilidade**, pode adicionar as definições do conjunto de disponibilidade para a VM, se não estiverem parte de um conjunto de disponibilidade na região de origem.
    - No **contas de armazenamento de destino**, selecione a conta que pretende utilizar.

        ![Ativar a replicação](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Clique em **criar o recurso de destino** > **ativar replicação**.
3. Depois das VMs estão ativadas para replicação, pode verificar o estado de funcionamento VM em **replicado itens**

>[!NOTE]
>Durante a replicação inicial, o estado pode demorar algum tempo para atualizar, sem progresso. Clique em de **atualizar** botão, para obter o estado mais recente.
>

# <a name="next-steps"></a>Passos Seguintes

[Saiba mais](site-recovery-test-failover-to-azure.md) sobre como executar uma ativação pós-falha de teste.
