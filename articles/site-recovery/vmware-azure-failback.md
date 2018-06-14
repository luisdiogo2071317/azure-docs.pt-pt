---
title: Falhar novamente a partir do Azure para VMware com o Azure Site Recovery | Microsoft Docs
description: Após a ativação pós-falha de máquinas virtuais do Azure, pode iniciar uma reativação pós-falha para colocar máquinas virtuais no local. Saiba os passos sobre como falhar novamente.
services: site-recovery
author: nsoneji
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 03/05/2018
ms.author: nisoneji
ms.openlocfilehash: 8f580fa40bade2bf586dd116729881b249bbba88
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
ms.locfileid: "29944012"
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Falhar novamente a partir do Azure para um site no local

Este artigo descreve como falhar back máquinas virtuais de máquinas virtuais do Azure para um ambiente de VMware no local. Siga as instruções neste artigo para efetuar a fazer uma cópia a máquinas virtuais VMware ou o Windows/Linux em servidores físicos depois de ter pós-falha no local do site para o Azure com o [ativação pós-falha no Azure Site Recovery](site-recovery-failover.md) tutorial.

## <a name="prerequisites"></a>Pré-requisitos
- Certifique-se de que leu os detalhes sobre o [diferentes tipos de reativação pós-falha](concepts-types-of-failback.md) e advertências correspondentes.

> [!WARNING]
> Não pode falhar novamente depois de ter um [concluído migração](migrate-overview.md#what-do-we-mean-by-migration), mover uma máquina virtual para outro grupo de recursos ou eliminar a máquina virtual do Azure. Se desativar a proteção da máquina virtual, não pode falhar novamente.

> [!WARNING]
> Um servidor físico do Windows Server 2008 R2 SP1, se protegido e efetuar a ativação pós-falha para o Azure, não é possível efetuar a novamente.

> [!NOTE]
> Se tiverem falhado através de máquinas virtuais VMware, não pode falhar para um anfitrião Hyper-V.


- Antes de continuar, conclua os passos de reproteção para que as máquinas virtuais estão num estado replicado e pode iniciar uma ativação pós-falha para um site no local. Para obter mais informações, consulte [como voltar a proteger a partir do Azure no local](vmware-azure-reprotect.md).

- Certifique-se de que o vCenter está num estado ligado antes de efetuar uma reativação pós-falha. Caso contrário, desligar discos e ligá-los para a máquina virtual irá falhar.

- Durante a ativação pós-falha para o Azure, o site no local pode não estar acessível, e o servidor de configuração pode estar indisponível ou encerrado para baixo. Durante a reproteção e a reativação pós-falha, o servidor de configuração no local deve ser em execução e no estado OK ligado. 

- Durante a reativação pós-falha, a máquina virtual tem de existir na base de dados de servidor de configuração ou reativação pós-falha não será concluída com êxito. Certifique-se de que demorar agendadas regularmente cópias de segurança do seu servidor. Se ocorrer um desastre, terá de restaurar o servidor com o endereço IP original para a reativação pós-falha trabalhar.

- O servidor de destino mestre não deve ter todos os instantâneos antes de acionar reproteção/reativação pós-falha.

## <a name="overview-of-failback"></a>Descrição geral da reativação pós-falha
Depois de ter a pós-falha para o Azure, pode falhar novamente para o site no local, executando os seguintes passos:

1. [Proteja](vmware-azure-reprotect.md) as máquinas virtuais do Azure para que possam iniciar replicar máquinas virtuais de VMware no seu site no local. Como parte deste processo, terá também de:

    * Configure um destino principal no local. Utilizar um destino principal do Windows para máquinas virtuais Windows e uma [destino principal do Linux](vmware-azure-install-linux-master-target.md) para máquinas virtuais do Linux.
    * Configurar um [servidor de processos](vmware-azure-set-up-process-server-azure.md).
    * Iniciar [Proteja](vmware-azure-reprotect.md) para desligar a máquina virtual no local e sincronizar os dados da máquina virtual do Azure com os discos no local.

2. Depois de replicar as máquinas virtuais no Azure para o seu site no local, inicie uma ativação pós-falha do Azure para o site no local.

3. Depois dos dados falhar novamente, pode voltar a proteger as máquinas de virtuais no local novamente para que possam iniciar a replicação para o Azure.

Para uma descrição geral, veja o seguinte vídeo sobre como falhar para um site no local:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Passos para falhar novamente

> [!IMPORTANT]
> Antes de iniciar a reativação pós-falha, certifique-se de que terminou a que as máquinas virtuais. As máquinas virtuais devem estar num estado protegido e o respetivo estado de funcionamento deve ser **OK**. Para voltar a proteger as máquinas virtuais, leia [como voltar a proteger](vmware-azure-reprotect.md).

1. Na página de itens replicados, selecione a máquina virtual. Faça duplo clique nele para selecionar **ativação pós-falha não planeada**.
2. No **confirmar ativação pós-falha**, certifique-se a direção de ativação pós-falha (a partir do Azure). Em seguida, selecione o ponto de recuperação (mais recente ou para a mais recente aplicação consistente) que pretende utilizar para a ativação pós-falha. O ponto consistente de aplicação é atrás do ponto mais recente no tempo e faz com que alguma perda de dados.
3. Durante a ativação pós-falha, a recuperação de Site encerra as máquinas virtuais no Azure. Depois de verificar que reativação pós-falha concluída conforme esperado, pode verificar que as máquinas virtuais no Azure encerrada.
4. **Consolidar** é necessária para remover a máquina virtual de efetuada a ativação pós-falha do Azure. Clique no item protegido e, em seguida, selecione **consolidar**. Uma tarefa remove as máquinas virtuais de efetuada a ativação pós-falha no Azure.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Para o ponto de recuperação pode posso falhar novamente as máquinas virtuais?

Durante a reativação pós-falha, tem duas opções para falhar novamente o plano de recuperação da máquina virtual /.

- Se selecionar o ponto de processamento mais recente no tempo, todas as máquinas virtuais ativadas pós-falha para o ponto mais recente no tempo. Se existir um grupo de replicação no plano de recuperação, cada máquina virtual do grupo de replicação pós-falha para o respetivo ponto mais recente independente no tempo.

  Não pode falhar novamente uma máquina virtual até ter, pelo menos, um ponto de recuperação. Não pode falhar novamente um plano de recuperação até que todas as respetivas máquinas virtuais tenha, pelo menos, um ponto de recuperação.

  > [!NOTE]
  > Um ponto de recuperação mais recente é um ponto de recuperação consistentes com falhas.

- Se selecionar o ponto de recuperação consistentes com aplicações, recupera uma reativação pós-falha única máquina virtual para o respetivo ponto de recuperação consistentes com aplicações disponíveis mais recente. No caso de um plano de recuperação com um grupo de replicação, cada grupo de replicação recupera para o respetivo ponto de recuperação disponíveis comuns.
Pontos de recuperação consistentes com aplicações podem ser atrás no tempo e poderá haver perda de dados.

## <a name="what-happens-to-vmware-tools-post-failback"></a>O que acontece a reativação pós-falha de post de ferramentas do VMware?

No caso de uma máquina virtual do Windows, a recuperação de sites desativa as ferramentas do VMware durante a ativação pós-falha. Durante a reativação pós-falha da máquina virtual Windows, as ferramentas do VMware são reenabled. 


## <a name="reprotect-from-on-premises-to-azure"></a>Voltar a proteger no local para o Azure
Após a conclusão da reativação pós-falha e ter iniciado consolidação, são eliminadas recuperadas máquinas virtuais no Azure. Agora, a máquina virtual está novamente no site no local, mas não será possível protegê-lo. Para começar a replicar para o Azure novamente, efetue o seguinte:

1. Selecione **cofre** > **definição** > **replicado itens**, selecione as máquinas virtuais que não foi possível voltar e, em seguida, selecione  **Voltar a proteger**.
2. Introduza o valor do servidor de processos que tem de ser utilizada para enviar dados para o Azure.
3. Selecione **OK** para iniciar a tarefa de reproteção.

> [!NOTE]
> Depois de uma máquina de virtual no local é iniciada, demora algum tempo para o agente registar novamente para o servidor de configuração (até 15 minutos). Durante este período, reproteção falha e devolve uma mensagem de erro a indicar que o agente não está instalado. Aguarde alguns minutos e, em seguida, tente reproteção novamente.

## <a name="next-steps"></a>Passos Seguintes

Após a conclusão da tarefa de reproteção, os são replicados máquina virtual para o Azure e podem fazer um [ativação pós-falha](site-recovery-failover.md) para mover as máquinas virtuais para o Azure novamente.


