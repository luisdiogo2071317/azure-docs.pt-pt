---
title: Como efetuar a reativação a partir do Azure para VMware | Microsoft Docs
description: Após a ativação pós-falha de máquinas virtuais do Azure, pode iniciar uma reativação pós-falha para colocar máquinas virtuais no local. Saiba os passos sobre como falhar novamente.
services: site-recovery
documentationcenter: ''
author: nsoneji
manager: gauravd
editor: ''
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/22/2018
ms.author: nisoneji
ms.openlocfilehash: 50dccf6196b7affd3d21087f851b929d0e850f6d
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/07/2018
ms.locfileid: "29768474"
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Falhar novamente a partir do Azure para um site no local

Este artigo descreve como falhar back máquinas virtuais de máquinas virtuais do Azure para o ambiente de VMware no local. Siga as instruções neste artigo para efetuar a fazer uma cópia a máquinas virtuais VMware ou o Windows/Linux em servidores físicos depois de ter pós-falha no local do site para o Azure com o [ativação pós-falha na recuperação de Site](site-recovery-failover.md) tutorial.

## <a name="prerequisites"></a>Pré-requisitos
- Certifique-se de que leu os detalhes sobre o [diferentes tipos de reativação pós-falha](concepts-types-of-failback.md) e advertências correspondentes.

> [!WARNING]
> Não é possível reativação pós-falha depois de ter um [concluído migração](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), mover uma máquina virtual para outro grupo de recursos ou eliminar a máquina virtual do Azure. Se desativar a proteção da máquina virtual, não é possível reativação pós-falha.

> [!WARNING]
> Um servidor físico do Windows Server 2008 R2 SP1, se protegido e efetuar a ativação pós-falha para o Azure, não é possível efetuar a novamente.

> [!NOTE]
> Se tiver a executar a ativação pós-falha de máquinas virtuais VMware, em seguida, não é possível a reativação pós-falha num anfitrião Hyper-v.


- Antes de continuar, conclua os passos de reproteção para que as máquinas virtuais estão num estado replicado, e pode iniciar uma ativação pós-falha para um site no local. Para obter mais informações, consulte [como voltar a proteger a partir do Azure no local](site-recovery-how-to-reprotect.md).

- Certifique-se de que o vCenter está num estado ligado antes de efetuar uma reativação pós-falha. Caso contrário, desligar discos e ligá-los para a máquina virtual irá falhar.

- Durante a ativação pós-falha para o Azure, o site no local poderá não estar acessível e, por conseguinte, o servidor de configuração pode ser indisponível ou encerramento. Durante a reproteção e a reativação pós-falha, o servidor de configuração no local deve ser em execução e no estado OK ligado. 

- Durante a reativação pós-falha, a máquina virtual tem de existir na base de dados de servidor de configuração ou reativação pós-falha não será concluída com êxito. Por conseguinte, certifique-se de que demorar agendadas regularmente cópias de segurança do seu servidor. Se tiver ocorrido um desastre, terá de restaurar o servidor com o endereço IP original, para reativação pós-falha trabalhar.

- O servidor de destino mestre não deve ter todos os instantâneos antes de acionar reproteção/reativação pós-falha.

## <a name="overview-of-failback"></a>Descrição geral da reativação pós-falha
Depois de ter a pós-falha para o Azure, pode falhar novamente para o site no local, executando os seguintes passos:

1. [Proteja](site-recovery-how-to-reprotect.md) as máquinas virtuais do Azure para que possam iniciar replicar máquinas virtuais de VMware no seu site no local. Como parte deste processo, terá também de:
    1. Configurar um destino principal no local: destino principal do Windows para máquinas virtuais Windows e [destino principal do Linux](site-recovery-how-to-install-linux-master-target.md) para máquinas virtuais do Linux.
    2. Configurar um [servidor de processos](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Iniciar [Proteja](site-recovery-how-to-reprotect.md). Isto irá desativar a máquina virtual no local e sincronizar os dados da máquina virtual do Azure com os discos no local.

1. Assim que estiver a replicar as máquinas virtuais no Azure para o seu site no local, iniciar uma falha através do Azure para o site no local.

1. Depois dos dados falhou novamente, pode voltar a proteger as máquinas de virtuais no local novamente, para que possam iniciar a replicação para o Azure.

Para uma descrição geral, veja o seguinte vídeo sobre como falhar para um site no local.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]


## <a name="steps-to-fail-back"></a>Passos para falhar novamente

> [!IMPORTANT]
> Antes de iniciar a reativação pós-falha, certifique-se de que concluiu que as máquinas virtuais. As máquinas virtuais devem estar num estado protegido e o respetivo estado de funcionamento deve ser **OK**. Para voltar a proteger as máquinas virtuais, leia [como voltar a proteger](site-recovery-how-to-reprotect.md).

1. Na página de itens replicados, selecione a máquina virtual e faça duplo clique nele para selecionar **ativação pós-falha não planeada**.
2. No **confirmar ativação pós-falha**, certifique-se a direção de ativação pós-falha (a partir do Azure) e, em seguida, selecione o ponto de recuperação (mais recente ou para a mais recente aplicação consistente) que pretende utilizar para a ativação pós-falha. O ponto consistente de aplicação é atrás do ponto mais recente no tempo e faz com que alguma perda de dados.
3. Durante a ativação pós-falha, a recuperação de Site encerra as máquinas virtuais no Azure. Depois de verificar que esse reativação pós-falha concluída conforme esperado, pode verificar que as máquinas virtuais do Azure foi encerradas.
4. **Consolidar** necessárias para remover a máquina virtual de ativação pós-falha Azure.Right clique o item protegido e, em seguida, clique em **consolidar**. Uma tarefa irá remover a ativação pós-falha de máquinas virtuais no Azure.


## <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Para o ponto de recuperação pode posso falhar novamente as máquinas virtuais?

Durante a reativação pós-falha, tem duas opções para falhar novamente o plano de recuperação da máquina virtual /.

- Se selecionar o ponto de processamento mais recente no tempo, todas as máquinas virtuais serão ativadas pós-falha para o ponto mais recente no tempo. Caso, não há um grupo de replicação no plano de recuperação, em seguida, cada máquina virtual do grupo de replicação serão ativadas pós-falha para o respetivo independente ponto mais recente no tempo.

    Não pode falhar novamente uma máquina virtual até ter, pelo menos, um ponto de recuperação. Não pode falhar novamente um plano de recuperação até que todas as respetivas máquinas virtuais tenha, pelo menos, um ponto de recuperação.

> [!NOTE]
> Um ponto de recuperação mais recente é um ponto de recuperação consistentes com falhas.

- Se selecionar o ponto de recuperação consistente da aplicação, recupera uma reativação pós-falha única máquina virtual para o respetivo ponto de recuperação consistentes com aplicações disponíveis mais recente. No caso de um plano de recuperação com um grupo de replicação, cada grupo de replicação recupera para o respetivo ponto de recuperação disponíveis comuns.
Pontos de recuperação consistentes com aplicações podem ser atrás no tempo e poderá haver perda de dados.

## <a name="what-happens-to-vmware-tools-post-failback"></a>O que acontece a reativação pós-falha de post de ferramentas do VMware?

Em caso de uma máquina virtual do Windows, o Azure Site Recovery desativa as ferramentas do VMware durante a ativação pós-falha. Durante a reativação pós-falha da máquina virtual Windows, as ferramentas do VMware são novamente ativadas. 


## <a name="reprotect-from-on-premises-to-azure"></a>Voltar a proteger no local para o Azure
Após a conclusão da reativação pós-falha e poderá ter de consolidação iniciadas são eliminadas recuperadas máquinas virtuais no Azure. Agora, a máquina virtual está novamente no site no local, mas não será possível protegê-lo. Para começar a replicar para o Azure novamente, efetue o seguinte:

1. No **cofre** > **definição** > **replicado itens**, selecione as máquinas virtuais que falharam novamente e, em seguida, clique em  **Voltar a proteger**.
2. Atribua o valor do servidor de processos que tem de ser utilizada para enviar dados para o Azure.
3. Clique em **OK** para iniciar a tarefa de reproteção.

> [!NOTE]
> Depois de uma máquina de virtual no local é iniciada, demora algum tempo para o agente registar novamente para o servidor de configuração (até 15 minutos). Durante este período, reproteção falha e devolve uma mensagem de erro a indicar que o agente não está instalado. Aguarde alguns minutos e, em seguida, tente reproteção novamente.

## <a name="next-steps"></a>Passos Seguintes

Após a conclusão da tarefa de reproteção, a máquina virtual está a replicar para o Azure e pode efetuar um [ativação pós-falha](site-recovery-failover.md) para mover as máquinas virtuais para o Azure novamente.


