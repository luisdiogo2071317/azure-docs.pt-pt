---
title: Replicar uma VM do Azure noutra região do Azure
description: Este início rápido fornece os passos necessários para replicar uma VM do Azure numa região do Azure para uma região diferente.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: quickstart
ms.date: 10/19/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 623c45cbc82139090d893b8501c7d363c4443d67
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49456189"
---
# <a name="replicate-an-azure-vm-to-another-azure-region"></a>Replicar uma VM do Azure noutra região do Azure

O serviço [Azure Site Recovery](site-recovery-overview.md) contribui para a sua estratégia de continuidade comercial e recuperação após desastre (BCDR) ao manter as suas aplicações empresariais em funcionamento e durante falhas planeadas e não planeadas. O Site Recovery gere e orquestra a recuperação após desastre de computadores no local e máquinas virtuais (VMs) do Azure, incluindo replicação, ativação pós-falha e recuperação.

Este início rápido descreve como replicar uma VM do Azure numa região diferente do Azure.

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

> [!NOTE]
> Este artigo destina-se a orientar um novo utilizador através da experiência do Azure Site Recovery com as opções predefinidas e a personalização mínima. Se quiser saber mais sobre as várias definições que podem ser personalizadas, veja [o tutorial para permitir a replicação das VMs do Azure](azure-to-azure-tutorial-enable-replication.md)

## <a name="log-in-to-azure"></a>Iniciar sessão no Azure

Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="enable-replication-for-the-azure-vm"></a>Ativar a replicação para a VM do Azure

1. No portal do Azure, clique em **Máquinas virtuais** e selecione a VM que pretende replicar.
2. Nas **Operações**, clique em **Recuperação após desastre**.
3. Em **Configurar recuperação após desastre** > **Região de destino**, selecione a região de destino para a qual irá replicar.
4. Para este Início Rápido, aceite as outras predefinições.
5. Clique em **Ativar replicação**. Esta ação inicia uma tarefa para ativar a replicação para a VM.

    ![ativar replicação](media/azure-to-azure-quickstart/enable-replication1.png)

## <a name="verify-settings"></a>Verificar as definições

Após a tarefa de replicação ser concluída, pode ver o estado da replicação, modificar a replicação e testar a implementação.

1. No menu VM, clique em **Recuperação após desastre**.
2. Pode verificar o estado de funcionamento da replicação, os pontos de recuperação que foram criados e as regiões de origem e destino num mapa.

   ![Estado de replicação](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Limpar recursos

A VM na região primária para de replicar quando desativa a replicação na mesma:

- As definições de replicação da origem são limpas automaticamente.
- A faturação do Site Recovery para a VM também para.

Pare a replicação da seguinte forma:

1. Selecione a VM.
2. Na **Recuperação após desastre**, clique em **Desativar Replicação**.

   ![Desativar a replicação](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Passos seguintes

Neste início rápido, replicou uma única VM numa região secundária. Agora pode explorar mais opções e tentar replicar um conjunto de VMs do Azure com um plano de recuperação.

> [!div class="nextstepaction"]
> [Configurar a recuperação após desastre para VMs do Azure](azure-to-azure-tutorial-enable-replication.md)
