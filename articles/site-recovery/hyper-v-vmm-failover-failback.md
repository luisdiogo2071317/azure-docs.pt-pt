---
title: Ativação pós-falha e falhar fazer uma cópia de VMs de Hyper-V replicado para um Datacenter secundário com a recuperação de Site | Microsoft Docs
description: Saiba como efetuar a ativação pós-falha de VMs de Hyper-V para o site secundário no local e que haja reativação para o site primário, com o Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/02/2018
ms.author: raynew
ms.openlocfilehash: ecb0b9395ce7071442ddf0dd976e1ca57b8be906
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Ativação pós-falha e falhar fazer uma cópia de VMs de Hyper-V foram replicadas para o site secundário no local

O [do Azure Site Recovery](site-recovery-overview.md) serviço gere e orquestra a replicação, ativação pós-falha e a reativação pós-falha de máquinas no local e máquinas de virtuais (VMs) do Azure.

Este artigo descreve como efetuar a ativação pós-falha de uma VM de Hyper-V geridas numa nuvem System Center Virtual Machine Manager (VMM), para um site secundário do VMM. Depois de feita a ativação pós-falha, vai fazer a reativação pós-falha para o seu site no local quando estiver disponível. Neste artigo, vai aprender a:

> [!div class="checklist"]
> * Ativação pós-falha numa VM de Hyper-V a partir de uma nuvem VMM principal a uma nuvem VMM secundária
> * Proteja do site secundário para o site primário e voltar a falhar
> * Opcionalmente, iniciar a replicação do primário para o secundário novamente

## <a name="failover-and-failback"></a>Ativação pós-falha e reativação pós-falha

Ativação pós-falha e a reativação pós-falha tem três fases:

1. **Ativação pós-falha para o site secundário**: falhar máquinas do site primário para o secundário.
2. **Falhar novamente a partir do site secundário**: replicar VMs de secundário do primário para e execute uma ativação pós-falha planeada para falhar novamente.
3. Após a ativação pós-falha planeada, opcionalmente, inicie a replicação do site primário para o secundário novamente.


## <a name="prerequisites"></a>Pré-requisitos

- Certifique-se de que concluiu uma [exercício de recuperação após desastre](hyper-v-vmm-test-failover.md) para verificar que tudo está a funcionar conforme esperado.
- Para concluir a reativação pós-falha, certifique-se de que os servidores primários e secundários do VMM estão ligados a recuperação de sites.



## <a name="run-a-failover-from-primary-to-secondary"></a>Executar uma ativação pós-falha de principal para secundário

Pode executar uma ativação pós-falha planeada ou regular para VMs de Hyper-V.

- Utilize uma ativação pós-falha regular para inesperadas. Quando executa esta ativação pós-falha, a recuperação de sites cria uma VM no site secundário e for ligado, cópia de segurança. Pode ocorrer a perda de dados, dependendo de colocar pendentes os dados que ainda não foram sincronizados.
- Uma ativação pós-falha planeada pode ser utilizada para manutenção ou durante a indisponibilidade esperada. Esta opção fornece a perda de dados. Quando é acionada uma ativação pós-falha planeada, as VMs de origem são encerradas. Os dados não sincronizados estão sincronizados e a ativação pós-falha é acionada. 
- 
Este procedimento descreve como executar uma ativação pós-falha regular.


1. Em **Definições** > **Itens replicados** clique na VM > **Ativação Pós-falha**.
1. Selecione **encerrar a máquina antes de iniciar a ativação pós-falha** se pretender que a recuperação de sites para tentar efetuar um encerramento de VMs de origem antes de acionar a ativação pós-falha. Recuperação de site também irá tentar sincronizar os dados no local que ainda não foi enviados para o site secundário, antes de acionar a ativação pós-falha. Tenha em atenção que a ativação pós-falha continua, mesmo se falha de encerramento. Pode seguir o progresso da ativação pós-falha na página **Trabalhos**.
2. Deve ser capaz de ver a VM na nuvem VMM secundária.
3. Depois de verificar a VM, **consolidar** a ativação pós-falha. São eliminados todos os pontos de recuperação disponíveis.

> [!WARNING]
> **Não cancelar uma ativação pós-falha em curso**: antes do início da ativação pós-falha, a replicação da VM é parada. Se cancelar uma ativação pós-falha que esteja em curso, a mesma para, mas a VM não será replicada outra vez.  


## <a name="reverse-replicate-and-failover"></a>A replicação inversa e ativação pós-falha

Iniciar a replicação do site secundário para o site primário e haja reativação para o site primário. Depois das VMs estão em execução no site primário novamente, pode replicá-los para o site secundário.  

 
1. Clique na VM > clique em **inverso replicar**.
2. Assim que a tarefa estiver concluída, clique na VM > no **ativação pós-falha**, certifique-se a direção de ativação pós-falha (a partir da nuvem VMM secundária) e selecione as localizações de origem e de destino. 
4. Inicie a ativação pós-falha. Pode seguir o progresso de ativação pós-falha no **tarefas** separador.
5. Na nuvem principal do VMM, verifique que a VM está disponível.
6. Se pretender iniciar a replicação novamente a VM principal para o site secundário, clique em **inverso replicar**.

## <a name="next-steps"></a>Passos Seguintes
[Reveja o passo](hyper-v-vmm-disaster-recovery.md) para replicar VMs Hyper-V para um site secundário.
