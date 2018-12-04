---
title: Realizar a ativação pós-falha e a reativação pós-falha de VMs IaaS do Azure replicadas numa região secundária do Azure para recuperação após desastre com o serviço Azure Site Recovery.
description: Saiba com realizar a ativação pós-falha e a reativação pós-falha de VMs do Azure replicadas numa região secundária do Azure para recuperação após desastre com o serviço Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/27/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 93928d7eb84ce986c8a9322188183e4c3dd76d99
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847869"
---
# <a name="fail-over-and-fail-back-azure-vms-between-azure-regions"></a>Realizar a ativação pós-falha e a reativação pós-falha de VMs do Azure entre regiões do Azure

O serviço [Azure Site Recovery](site-recovery-overview.md) contribui para a estratégia de recuperação após desastre, através da gestão e orquestração de replicação, ativação pós-falha e reativação pós-falha de máquinas no local e máquinas virtuais (VMs) do Azure.

Este tutorial descreve como realizar a ativação pós-falha numa única VM do Azure para uma região secundária do Azure. Depois de feita a ativação pós-falha, vai fazer a reativação pós-falha para a região primária quando estiver disponível. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Realizar a ativação pós-falha da VM do Azure
> * Voltar a proteger a VM do Azure secundária, para que replique para a região primária
> * Realizar a reativação pós-falha da VM secundária
> * Voltar a proteger a VM primária para a região secundária

> [!NOTE]
> Este tutorial destina-se a orientar o utilizador através de passos para efetuar ativação pós-falha para uma região de destino e com o mínimo de personalização; caso deseje saber mais sobre os vários aspetos associados à ativação pós-falha, incluindo as considerações de rede, a automatização ou a resolução de problemas, veja os documentos em "Procedimentos" para VMs do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- Certifique-se de que concluiu um [teste de recuperação após desastre](azure-to-azure-tutorial-dr-drill.md) para verificar se está tudo a funcionar conforme esperado.
- Verifique as propriedades da VM antes de executar a ativação pós-falha de teste. A VM tem de cumprir os [Requisitos do Azure](azure-to-azure-support-matrix.md#replicated-machine-operating-systems).

## <a name="run-a-failover-to-the-secondary-region"></a>Realize uma ativação pós-falha para a região secundária

1. Nos **Itens replicados**, selecione a VM em que pretende realizar a ativação pós-falha > **Ativação pós-falha**

   ![Ativação pós-falha](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. Em **Ativação pós-falha**, selecione um **Ponto de Recuperação** para o qual irá realizar a ativação pós-falha. Pode utilizar uma das opções seguintes:

   * **Mais recente** (predefinição): esta opção processa todos os dados no serviço do Site Recovery e apresenta o Objetivo do Ponto de Recuperação (RPO) mais baixo.
   * **Processado recentemente** : esta opção reverte a máquina virtual para o ponto de recuperação mais recente que foi processado pelo serviço Site Recovery.
   * **Personalizado**: utilize esta opção para realizar a ativação pós-falha para um ponto de recuperação específico. Esta opção é útil para realizar uma ativação pós-falha de teste.

3. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha** se quiser que o Site Recovery tente encerrar as máquinas virtuais de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe.

4. Siga o progresso da ativação pós-falha na página **Tarefas**.

5. Após a ativação pós-falha, valide a máquina virtual ao iniciar sessão. Se pretender voltar a outro ponto de recuperação para a máquina virtual, então pode utilizar a opção **Alterar ponto de recuperação**.

6. Quando estiver satisfeito com a ativação pós-falha da máquina virtual, pode **Consolidar** a ativação pós-falha.
   Ao consolidar elimina todos os pontos de recuperação disponíveis com o serviço. A opção **Alterar o ponto de recuperação** já não está disponível.

## <a name="reprotect-the-secondary-vm"></a>Voltar a proteger a VM secundária

Após a ativação pós-falha da VM, tem de voltar a protege-la, para que esta replique novamente para a região primária.

1. Certifique-se de que a VM está no estado **Ativação pós-falha consolidada** e verifique se a região primária está disponível, e poderá criar e aceder a novos recursos na mesma.
2. No **Cofre** > **Itens Replicados**, clique com o botão direito do rato na VM em que foi realizada a ativação pós-falha e, em seguida, selecione **Voltar a Proteger**.

   ![Clique com o botão direito do rato para voltar a proteger](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Veja se a direção da proteção, a região secundária para primária, já está selecionada.
3. Veja as informações **Grupo de recursos, Rede, Armazenamento e Conjuntos de disponibilidade**. Quaisquer recursos marcados (novo) são criados como parte da operação de reproteção.
4. Clique em **OK** para acionar uma tarefa de reproteção. Esta tarefa realiza o seeding do site de destino com os dados mais recentes. Em seguida, replica as diferenças para a região primária. A VM está agora num estado protegido.

## <a name="fail-back-to-the-primary-region"></a>Realizar a reativação pós-falha para a região primária

Depois de voltar a proteger as VMs, pode realizar a reativação pós-falha para a região primária, conforme precisar. Para tal, configure uma ativação pós-falha da região secundária para a primária, conforme descrito neste artigo.
