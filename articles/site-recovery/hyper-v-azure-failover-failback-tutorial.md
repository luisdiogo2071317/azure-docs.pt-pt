---
title: Ativação pós-falha e falhar fazer uma cópia de VMs de Hyper-V replicado para o Azure com a recuperação de Site | Microsoft Docs
description: Saiba como efetuar a ativação pós-falha de VMs de Hyper-V para o Azure e haja reativação para o site no local, com o Azure Site Recovery
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 06/20/2018
ms.author: raynew
ms.openlocfilehash: 6a34187a87c6ecda461357a1c2fc8747ddf4b056
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294297"
---
# <a name="failover-and-failback-hyper-v-vms-replicated-to-azure"></a>Ativação pós-falha e a reativação pós-falha VMs de Hyper-V replicado para o Azure

Este tutorial descreve como a ativação pós-falha de uma VM de Hyper-V para o Azure. Depois que tiver a ativação pós-falha, reativação pós-falha, para o seu site no local quando está disponível. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Certifique-se de que as propriedades de VM de Hyper-V para verificar em conformidade com os requisitos do Azure
> * Executar uma ativação pós-falha para o Azure
> * A reativação pós-falha a partir do Azure no local
> * A replicação inversa no local VMs, iniciar a replicação novamente para o Azure

Este tutorial é o quinto tutorial uma série. Pressupõe que já concluiu as tarefas nos tutoriais anteriores.    

1. [Preparar o Azure](tutorial-prepare-azure.md)
2. [Prepara Hyper-V no local](tutorial-prepare-on-premises-hyper-v.md)
3. Configurar a recuperação após desastre para [VMs de Hyper-V](tutorial-hyper-v-to-azure.md), ou para [VMs de Hyper-V geridas em nuvens VMM do System Center](tutorial-hyper-v-vmm-to-azure.md)
4. [Executar um teste de recuperação após desastre](tutorial-dr-drill-azure.md)

## <a name="prepare-for-failover-and-failback"></a>Preparar para ativação pós-falha e a reativação pós-falha

Certifique-se de que existem não existem instantâneos da VM e que a VM no local é desativada durante a reativação pós-falha. Ajuda a garantir a consistência dos dados durante a replicação. Não ative a VM no local durante a reativação pós-falha. 

Ativação pós-falha e a reativação pós-falha tem três fases:

1. **Ativação pós-falha para o Azure**: VMs de Hyper-V de ativação pós-falha do site no local para o Azure.
2. **A reativação pós-falha no local**: as VMs do Azure de ativação pós-falha para o seu site no local, quando estiver disponível. Começa a replicar VMs de VMs de Hyper-V no local. Após a sincronização inicial de dados, as VMs do Azure de ativação pós-falha para o site no local.  
3. **A replicação inversa no local VMs**: depois de dados falhou novamente, inversa replicar as VMs no local para iniciar a replicação-los para o Azure.

## <a name="verify-vm-properties"></a>Verificar as propriedades da VM

Antes da ativação pós-falha, verifique as propriedades da VM e, certifique-se de que a VM cumprem com [requisitos do Azure](hyper-v-azure-support-matrix.md#replicated-vms).

1. No **itens protegidos**, clique em **itens replicados** >< VM-name >.

2. No **replicados item** painel, reveja as informações de VM, o estado de funcionamento e os pontos de recuperação disponível mais recentes. Clique em **Propriedades** para ver mais detalhes.
     - No **computação e rede**, pode modificar as definições de VM e definições de rede, incluindo a rede/sub-rede na qual a VM do Azure estarão localizada após a ativação pós-falha e o endereço IP que serão atribuídas ao mesmo. Discos geridos não são suportados para reativação pós-falha a partir do Azure para o Hyper-V.
      - Em **Discos**, pode ver informações sobre o sistema operativo e os discos de dados na VM.

## <a name="failover-to-azure"></a>Ativação pós-falha para o Azure

1. Em **Definições** > **Itens replicados** clique na VM > **Ativação Pós-falha**.
2. No **ativação pós-falha** selecionar o **mais recente** ponto de recuperação. 
3. Selecione **Encerrar a máquina antes de iniciar a ativação pós-falha**. Recuperação de site tenta efetuar um encerramento de VMs de origem antes de acionar a ativação pós-falha. A ativação pós-falha continua, mesmo que o encerramento falhe. Pode seguir o progresso da ativação pós-falha na página **Trabalhos**.
4. Depois de verificar a ativação pós-falha, clique em **consolidar**. Elimina todos os pontos de recuperação disponíveis.

> [!WARNING]
> **Não cancele uma ativação pós-falha em curso**: Se cancelar em curso, deixa de ativação pós-falha, mas a VM não irá replicar novamente.

## <a name="failback-azure-vm-to-on-premises-and-reverse-replicate-the-on-premises-vm"></a>VM do Azure de reativação pós-falha no local e inversa replicar a VM no local

Operação de reativação pós-falha é basicamente uma ativação pós-falha do Azure para o site no local e na replicação inversa novamente começa a replicar as VMs do site no local para o Azure.

1. No **definições** > **replicado itens**, clique na VM > **ativação pós-falha planeada**.
2. No **confirmar a ativação de pós-falha planeadas**, certifique-se a direção de ativação pós-falha (a partir do Azure) e selecione as localizações de origem e de destino.
3. Selecione **sincronizar os dados antes da ativação pós-falha (sincronizar apenas alterações de delta)**. Esta opção minimiza o tempo de inatividade VM porque sincroniza-se sem encerrar a VM.
4. Inicie a ativação pós-falha. Pode seguir o progresso de ativação pós-falha no **tarefas** separador.
5. Depois dos dados iniciais é efetuada a sincronização e estará pronto encerrar o VMs do Azure, clique em **tarefas** > a ser planeada a ativação pós-falha-tarefa-nome-> **concluir a ativação pós-falha**. Será encerrado a VM do Azure, transfere a mais recente alterações no local e começa a VM no local.
6. Inicie sessão para a VM no local para verificar que se está disponível como esperado.
7. A VM no local agora está a ser um **consolidar pendente** estado. Clique em **consolidar**. Elimina as VMs do Azure e os respetivos discos e prepara a VM no local para a replicação inversa.
Iniciar a replicação da VM no local para o Azure, ativar **inverso replicar**. Aciona a replicação das alterações de delta que ocorreram desde que a VM do Azure foi desativada.  
