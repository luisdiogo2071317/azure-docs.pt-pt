---
title: Ativação pós-falha e reativação pós-falha de VMs de Hyper-V replicados para o Azure com o Site Recovery | Documentos da Microsoft
description: Saiba como a ativação pós-falha de VMs de Hyper-V para o Azure e a reativação pós-falha para o site no local, com o Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: f758939964045ed373703a211d4cbef00f0e42e7
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/09/2018
ms.locfileid: "37919551"
---
# <a name="failover-and-failback-hyper-v-vms-replicated-to-azure"></a>A ativação pós-falha e reativação pós-falha de VMs de Hyper-V replicados para o Azure

Este tutorial descreve como realizar a ativação pós-falha de uma VM de Hyper-V para o Azure. Depois de ter pós-falha, reativação pós-falha para o seu site no local quando estiver disponível. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Certifique-se de que está em conformidade com as propriedades da VM de Hyper-V para verificar os requisitos do Azure
> * Executar uma ativação pós-falha para o Azure
> * Reativação pós-falha do Azure para o local
> * A replicação inversa VMs no local, para iniciar a replicação novamente para o Azure

Este tutorial é o quinto tutorial de uma série. Parte do princípio de que já concluiu as tarefas nos tutoriais anteriores.    

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Prepara Hyper-V no local](tutorial-prepare-on-premises-hyper-v.md)
3. Configurar a recuperação após desastre para [VMs de Hyper-V](tutorial-hyper-v-to-azure.md), ou para [VMs de Hyper-V geridas em clouds do System Center VMM](tutorial-hyper-v-vmm-to-azure.md)
4. [Executar um teste de recuperação após desastre](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Preparar para a ativação pós-falha e reativação pós-falha

Certificar-se de que existem não existem instantâneos na VM e que a VM no local é desativada durante a reativação pós-falha. Ajuda a garantir a consistência dos dados durante a replicação. Não ative a VM no local durante a reativação pós-falha. 

Ativação pós-falha e reativação pós-falha têm três fases:

1. **Ativação pós-falha do Azure**: VMs de Hyper-V de ativação pós-falha do site no local para o Azure.
2. **Reativação pós-falha para o local**: as VMs do Azure de ativação pós-falha para o seu site no local quando o site no local está disponível. Ele começa a sincronização de dados do Azure para o local e após a conclusão, ele exibirá as VMs no local.  
3. **A replicação inversa VMs no local**: após a falha no local, o inverso replicar as VMs no local para iniciar a replicação-los para o Azure.

## <a name="verify-vm-properties"></a>Verificar as propriedades da VM

Antes da ativação pós-falha, verifique as propriedades da VM e, certifique-se de que a VM cumpre com [requisitos do Azure](hyper-v-azure-support-matrix.md#replicated-vms).

Em **Itens Protegidos**, clique em **Itens Replicados** > VM.

2. No painel **Item replicado**, existe um resumo das informações, do estado de funcionamento e dos pontos de recuperação mais recentes disponíveis da VM. Clique em **Propriedades** para ver mais detalhes.

3. Em **Computação e Rede**, pode modificar o nome do Azure, o grupo de recursos, o tamanho de destino, o [conjunto de disponibilidade](../virtual-machines/windows/tutorial-availability-sets.md) e as definições do disco gerido.

4. Pode ver e modificar as definições de rede, incluindo a rede/sub-rede na qual a VM do Azure será localizada após a ativação pós-falha e o endereço IP que será atribuído à mesma.

5. Em **Discos**, pode ver informações sobre o sistema operativo e os discos de dados na VM.

## <a name="failover-to-azure"></a>Ativação pós-falha do Azure

1. Na **configurações** > **itens replicados**, clique na VM > **ativação pós-falha**.
2. Na **ativação pós-falha**, selecione a **mais recente** ponto de recuperação. 
3. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha**. O site Recovery tenta fazer um encerramento de VMs de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. Pode seguir o progresso da ativação pós-falha na página **Trabalhos**.
4. Depois de verificar a ativação pós-falha, clique em **consolidar**. Elimina todos os pontos de recuperação disponíveis.

> [!WARNING]
> **Não cancelar uma ativação pós-falha em curso**: se cancela em curso, paradas de ativação pós-falha, mas a VM não replicar novamente.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>Reativação pós-falha do Azure VM no local e reversa replicar a VM no local

Operação de reativação pós-falha é basicamente uma ativação pós-falha do Azure para o site no local e na replicação inversa novamente começa a replicar VMs do site no local para o Azure.

1. Na **configurações** > **itens replicados**, clique na VM > **ativação pós-falha planeada**.
2. Na **confirmar a ativação de pós-falha planeadas**, certifique-se a direção da ativação pós-falha (do Azure) e selecione as localizações de origem e de destino.
3. Selecione **sincronizar os dados antes da ativação pós-falha (sincronizar apenas alterações de delta)**. Esta opção minimiza o tempo de inatividade da VM porque a sincronização ocorre sem desligar a VM.
4. Inicie a ativação pós-falha. Pode seguir o progresso de ativação pós-falha no **tarefas** separador.
5. Depois dos dados iniciais é feita a sincronização e está pronto para encerrar a VMs do Azure, clique em **trabalhos** > planeada-ativação pós-falha-tarefa-name > **concluir a ativação pós-falha**. Encerra a VM do Azure, transfere o mais recentes alterações no local e inicia a VM no local.
6. Inicie sessão na VM no local para verificar que está disponível como esperado.
7. A VM no local está agora num **consolidar pendente** estado. Clique em **consolidar**. Elimina as VMs do Azure e os respetivos discos e prepara a VM no local para a replicação inversa.
Para começar a replicar a VM no local para o Azure, ative **inverter replicar**. Aciona a replicação das alterações delta que ocorreram, uma vez que a VM do Azure foi desativada.  
