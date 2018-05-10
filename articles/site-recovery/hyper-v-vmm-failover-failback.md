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
ms.openlocfilehash: f4207b8def3a5cd240b7a3ecdffde34a27f2a833
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/08/2018
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

- Utilize uma ativação pós-falha regular para inesperadas. Quando executa esta ativação pós-falha, a recuperação de sites cria uma VM no site secundário e for ligado, cópia de segurança. Executar a ativação pós-falha contra um ponto de recuperação específico. Pode ocorrer a perda de dados, consoante o ponto de recuperação a que utilizar.
- Uma ativação pós-falha planeada pode ser utilizada para manutenção ou durante a indisponibilidade esperada. Esta opção fornece a perda de dados. Quando é acionada uma ativação pós-falha planeada, as VMs de origem são encerradas. Os dados não sincronizados estão sincronizados e a ativação pós-falha é acionada. 
- 
Este procedimento descreve como executar uma ativação pós-falha regular.


1. Em **Definições** > **Itens replicados** clique na VM > **Ativação Pós-falha**.
2. Em **Ativação pós-falha**, selecione um **Ponto de Recuperação** para o qual fazer a ativação pós-falha. Pode utilizar uma das opções seguintes:
    - **Mais recente** (predefinição): esta opção processa primeiro todos os dados enviados para o Site Recovery. Fornece o RPO mais baixo (objetivo de ponto de recuperação) porque a VM de réplica criada após a ativação pós-falha tem todos os dados que foi replicados para o Site de recuperação quando a ativação pós-falha foi acionada.
    - **Processado mais recentemente**: esta opção faz a ativação pós-falha da VM para o ponto de recuperação mais recente processado pelo Site Recovery. Esta opção proporciona um RTO (Objetivo de Tempo de Recuperação) baixo, porque não é despendido tempo ao processar os dados não processados.
    - **Consistente com a aplicação mais recente**: esta opção faz a ativação pós-falha da VM para o último ponto de recuperação consistente com a aplicação processado pelo Site Recovery. 
3. A chave de encriptação não é relevante neste cenário.
4. Selecione **encerrar a máquina antes de iniciar a ativação pós-falha** se pretender que a recuperação de sites para tentar efetuar um encerramento de VMs de origem antes de acionar a ativação pós-falha. Recuperação de site também irá tentar sincronizar os dados no local que ainda não foi enviados para o site secundário, antes de acionar a ativação pós-falha. Tenha em atenção que a ativação pós-falha continua, mesmo se falha de encerramento. Pode seguir o progresso da ativação pós-falha na página **Trabalhos**.
5. Deve ser capaz de ver a VM na nuvem VMM secundária.
6. Depois de verificar a VM, **consolidar** a ativação pós-falha. São eliminados todos os pontos de recuperação disponíveis.

> [!WARNING]
> **Não cancelar uma ativação pós-falha em curso**: antes do início da ativação pós-falha, a replicação da VM é parada. Se cancelar uma ativação pós-falha que esteja em curso, a mesma para, mas a VM não será replicada outra vez.  


## <a name="reprotect-and-fail-back"></a>Proteja e voltar a falhar

Iniciar a replicação do site secundário para o site primário e haja reativação para o site primário. Depois das VMs estão em execução no site primário novamente, pode replicá-los para o site secundário novamente.  

1. No **definições** > **replicado itens** clique na VM e ativar **inverso replicar**. A VM começa a replicar para o site primário.
2. Clique na VM > **a ativação pós-falha planeada**.
3. No **confirmar a ativação de pós-falha planeadas**, certifique-se a direção de ativação pós-falha (a partir da nuvem VMM secundária) e selecione as localizações de origem e de destino. 
4. No **sincronização de dados**, especifique como deseja sincronizar:
    - **Sincronizar os dados antes da ativação pós-falha (sincronizar apenas alterações de delta)**— esta opção minimiza o período de indisponibilidade VM, porque sincroniza-se sem encerrar a VM. Eis o que faz:
        - Guarda um instantâneo da VM de réplica e copia-o para o anfitrião de Hyper-V principal. A réplica VM mantém em execução.
        - Será encerrado a réplica de VM, para que não existem novas alterações ocorrem não existe. O conjunto final de alterações são transferidas para o site primário, e a VM no site primário é iniciada.
    - **Sincronizar os dados durante a ativação pós-falha apenas (transferência completa)**— Utilize esta opção se tiver sido executar no site secundário durante muito tempo. Esta opção é mais rápida, uma vez que vamos esperar várias alterações de disco e não demora tempo nos cálculos de soma de verificação. Esta opção efetua uma transferência de disco. Também é útil quando a VM principal foi eliminada.
5. A chave de encriptação não é relevante neste cenário.
6. Inicie a ativação pós-falha. Pode seguir o progresso de ativação pós-falha no **tarefas** separador.
7. Se tiver selecionado para sincronizar os dados antes da ativação pós-falha, após a sincronização inicial de dados é efetuada e estará pronto desligar a VM no site secundário de réplica, clique em **tarefas** > nome da tarefa de ativação pós-falha planeada >  **Concluir a ativação pós-falha**. Isto será encerrado a VM secundária, transfere as alterações mais recentes para o site primário e começa a VM principal.
8. Na nuvem principal do VMM, verifique que a VM está disponível.
9. A VM principal está agora num Estado de consolidar pendente. Clique em **consolidação**, para confirmar a ativação pós-falha.
10. Se pretender iniciar a replicação novamente a VM principal para o site secundário, ative **inverso replicar**.


> [!NOTE]
> A replicação inversa replica apenas as alterações que ocorreram desde a réplica VM foi desativada e são enviadas apenas alterações de delta.

## <a name="next-steps"></a>Passos Seguintes
[Reveja o passo](hyper-v-vmm-disaster-recovery.md) para replicar VMs Hyper-V para um site secundário.
