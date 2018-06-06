---
title: Reproteção efetuada a ativação pós-falha de VMs do Azure para a região principal do Azure com o Azure Site Recovery | Microsoft Docs
description: Descreve como voltar a proteger as VMs do Azure numa região secundária, após a ativação pós-falha de uma região primária, utilizando o Azure Site Recovery.
services: site-recovery
author: rajani-janaki-ram
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 05/31/2018
ms.author: rajanaki
ms.openlocfilehash: 6a57226f81e626e620033750c58d5998d8e7e894
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34716589"
---
# <a name="reprotect-failed-over-azure-vms-to-the-primary-region"></a>Reproteção efetuada a ativação pós-falha de VMs do Azure para a região primária


Quando lhe [efetuar a ativação pós-falha](site-recovery-failover.md) VMs do Azure de uma região para a utilização de outro [do Azure Site Recovery](site-recovery-overview.md), o arranque de VMs de cópia de segurança na região secundária, num Estado não protegido. Se falhar novamente as VMs para a região primária, terá de fazer o seguinte:

- Proteja as VMs na região secundária, de modo a que são iniciados replicar a região primária. 
- Depois de concluir só e estiver a replicar as VMs, pode efetuá-los através da secundário, a região primária.

> [!WARNING]
> Se já [migrados](migrate-overview.md#what-do-we-mean-by-migration) máquinas a partir de principal para a região secundária, mover a VM para outro grupo de recursos ou eliminado a VM do Azure, não é possível voltar a proteger a VM ou efetuar a pós-falha.


## <a name="prerequisites"></a>Pré-requisitos
1. A ativação pós-falha da VM do primário para a região secundária tem de ser consolidada.
2. O site de destino principal deve estar disponível e deverá conseguir aceder ou criar recursos nessa região.

## <a name="reprotect-a-vm"></a>Voltar a proteger uma VM

1. No **cofre** > **replicado itens**, com o botão direito a ativação pós-falha da VM e selecione **voltar a proteger**. A direção deve mostrar de secundária ao principal. 

  ![Voltar a proteger](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Reveja os conjuntos de disponibilidade, armazenamento, rede e grupo de recursos. Em seguida, clique em **OK**. Se existirem quaisquer recursos marcados como novo, estes são criados como parte do processo de só.
3. A tarefa implementa o site de destino com os dados mais recentes. Após a que, replicação de diferenças ocorre. Em seguida, pode ativação pós-falha para o site primário. Pode selecionar a conta de armazenamento ou a rede que pretende utilizar durante a voltar a proteger, utilizando a opção de personalizar.

  ![Personalizar a opção](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

### <a name="customize-reprotect-settings"></a>Personalizar as definições de reproteção

Pode personalizar as seguintes propriedades do destino VMe durante só.

![Personalizar](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Propriedade |Notas  |
|---------|---------|
|Grupo de recursos de destino     | Modificar o grupo de recursos de destino em que a VM ser criada. Como parte de uma só, a VM de destino foi eliminado. Pode escolher um novo grupo de recursos no qual pretende criar a VM após a ativação pós-falha.        |
|Rede virtual de destino     | A rede de destino não pode ser alterada durante a tarefa de reproteção. Para alterar a rede, Refazer o mapeamento da rede.         |
|Armazenamento de destino (VM secundário não utiliza discos geridos)     | Pode alterar a conta de armazenamento que utiliza a VM após a ativação pós-falha.         |
|Réplica geridas discos (secundária VM utiliza discos geridos)    | Recuperação de sites cria discos de réplica gerido na região primária para espelhar discos geridos a VM secundário.         | 
|Armazenamento de cache     | Pode especificar uma conta de armazenamento de cache a utilizar durante a replicação. Por predefinição, é possível criar uma nova conta de armazenamento de cache, se não existir.         |
|Conjunto de Disponibilidade     |Se a VM com a região secundária faz parte de um conjunto de disponibilidade, pode escolher um conjunto de disponibilidade para a VM de destino na região primária. Por predefinição, a recuperação de Site tenta localizar o conjunto na região primária de disponibilidade existente e utilizá-lo. Durante a personalização, pode especificar um novo conjunto de disponibilidade.         |


### <a name="what-happens-during-reprotection"></a>O que acontece durante só?

Por predefinição ocorre o seguinte:

1. É criada uma conta de armazenamento de cache na região primária
2. Se a conta de armazenamento de destino (a original conta do storage na região primária) não existe, é criado um novo. O nome da conta de armazenamento atribuído é o nome da conta de armazenamento utilizado pela VM secundária, o sufixo com "asr".
3. Se a VM utiliza discos geridos, as réplica geridas discos são criados na região primária para armazenar os dados replicados a partir de discos da VM secundário. 
4. Se o conjunto de disponibilidade de destino não existe, uma nova é criada como parte da tarefa de reproteção, se necessário. Se tiver personalizado as definições de, em seguida, o conjunto selecionado é utilizado.

Quando acionar uma tarefa de reproteção e o destino da que VM existe, ocorre o seguinte:

1. Os componentes necessários são criados como parte de reproteção. Se já existirem, são reutilizados.
2. O lado do destino que VM é desativada se estiver em execução.
3. O disco VM de lado de destino é copiado pela recuperação de sites para um contentor, como um blob de seed.
4. O lado do destino VM, em seguida, é eliminado.
5. A origem atual é utilizar o blob de seed lado VM (secundário) para replicar. Isto garante que apenas deltas são replicadas.
6. As alterações principais entre o disco de origem e o blob de seed são sincronizadas. Esta operação pode demorar algum tempo a concluir.
7. Depois de concluída a tarefa de reproteção, a replicação de diferenças começa e cria um ponto de recuperação de acordo com a política de replicação.
8. Depois da tarefa de reproteção for bem sucedida, a VM aciona um estado protegido.

## <a name="next-steps"></a>Passos Seguintes

Depois da VM estiver protegida, pode iniciar uma ativação pós-falha. A ativação pós-falha será encerrado a VM com a região secundária e cria e inicia VM na região primária, com algum período de indisponibilidade breves. Recomendamos que escolha um período de tempo em conformidade e que execute uma ativação pós-falha de teste, mas a iniciar uma ativação pós-falha completa para o site primário. [Saiba mais](site-recovery-failover.md) sobre a ativação pós-falha.

