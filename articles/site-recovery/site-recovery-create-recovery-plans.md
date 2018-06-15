---
title: Criar e personalizar planos de recuperação para ativação pós-falha e recuperação no Azure Site Recovery | Microsoft Docs
description: Saiba como criar e personalizar planos de recuperação no Azure Site Recovery. Este artigo descreve como efetuar a ativação pós-falha e recuperação VMs e servidores físicos.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/21/2018
ms.author: raynew
ms.openlocfilehash: e02672ea76eada2d660b20f91c4417019d4efc97
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
ms.locfileid: "30189839"
---
# <a name="create-and-customize-recovery-plans"></a>Criar e personalizar planos de recuperação

Este artigo descreve como criar e personalizar um plano de recuperação no [do Azure Site Recovery](site-recovery-overview.md). Antes de começar, [mais](recovery-plan-overview.md) sobre os planos de recuperação.

## <a name="create-a-recovery-plan"></a>Criar um plano de recuperação

1. No cofre dos serviços de recuperação, selecione **planos de recuperação (recuperação de sites)** > **+ plano de recuperação**.
2. No **criar plano de recuperação**, especifique um nome para o plano.
3. Escolha uma origem e de destino com base nas máquinas no plano e selecione **Resource Manager** do modelo de implementação. A localização de origem tem de ter máquinas que estão ativadas para ativação pós-falha e recuperação. 

   **Ativação pós-falha** | **origem** | **Target** 
   --- | --- | ---
   Azure para o Azure | Região do Azure |Região do Azure
   VMware para Azure | Servidor de configuração | Azure
   Máquinas físicas no Azure | Servidor de configuração | Azure   
   Hyper-V gerida pelo VMM para o Azure  | Nome a apresentar do VMM | Azure
   Hyper-V sem o VMM para o Azure | Nome de site Hyper-V | Azure
   VMM para o VMM |Nome amigável do VMM | Nome a apresentar do VMM 

   > [!NOTE]
   > Um plano de recuperação pode conter máquinas com a mesma origem e destino. VMware e VMs de Hyper-V geridas pelo VMM não podem estar no mesmo plano. VMs de VMware e servidores físicos podem estar no mesmo plano, em que a origem é um servidor de configuração.

2. No **selecione os itens de máquinas virtuais**, selecione os computadores (ou grupo de replicação) que pretende adicionar ao plano. Em seguida, clique em **OK**.
    - Grupo predefinido (grupo 1) são adicionadas máquinas no plano. Após a ativação pós-falha, todas as máquinas neste grupo iniciam em simultâneo.
    - Pode selecionar apenas as máquinas estão a ser as localizações de origem e de destino que especificou. 
1. Clique em **OK** para criar o plano.

## <a name="add-a-group-to-a-plan"></a>Adicione um grupo para um plano

Criar grupos adicionais e adicionar máquinas para diferentes grupos para que pode especificar o comportamento diferentes de forma grupo por grou. Por exemplo, pode especificar quando máquinas num grupo devem iniciar após a ativação pós-falha ou especifique ações personalizadas por grupo.

1. No **planos de recuperação**, faça duplo clique o plano > **personalizar**. Por predefinição, depois de criar um plano de todas as máquinas que adicionou ao mesmo estão localizadas no predefinido grupo 1.
2. Clique em **+ grupo**. Por predefinição, um novo grupo é numerado pela ordem em que é adicionado. Pode ter até sete grupos.
3. Selecione a máquina que pretende mover para o novo grupo, clique em **grupo alteração**e, em seguida, selecione o novo grupo. Em alternativa, faça duplo clique o nome do grupo > **item protegido**e adicionar computadores ao grupo. Um computador ou grupo de replicação só pode pertencer a um grupo num plano de recuperação.


## <a name="add-a-script-or-manual-action"></a>Adicionar uma ação de script ou manual

Pode personalizar um plano de recuperação através da adição de um script ou a ação manual. Tenha em atenção que:

- Se estiver a replicar para o Azure pode integrar runbooks de automatização do Azure para o seu plano de recuperação. [Saiba mais](site-recovery-runbook-automation.md).
- Se estiver a replicar VMs de Hyper-V geridas pelo VMM do System Center, pode criar um script no servidor do VMM no local e incluí-la no plano de recuperação.
- Quando adiciona um script, adiciona um novo conjunto de ações para o grupo. Por exemplo, é criado um conjunto de pré-passos de para o grupo 1 com o nome *grupo 1: pré-passos*. Todos os pré-passos de estão listados dentro deste conjunto. Pode adicionar um script no site primário, apenas se tiver um servidor VMM implementado.
- Se adicionar uma ação manual, quando executa o plano de recuperação, interrompe o momento em que a ação manual que inserido. Uma caixa de diálogo pede-lhe para especificar que a ação manual foi concluída.
- Para criar um script no servidor do VMM, siga as instruções em [neste artigo](hyper-v-vmm-recovery-script.md).
- Scripts podem ser aplicados durante a ativação pós-falha para o site secundário e durante a reativação pós-falha do site secundário para o site primário. Suporte depende do seu cenário de replicação:
    
    **Cenário** | **Ativação pós-falha** | **Reativação pós-falha**
    --- | --- | --- 
    Azure para o Azure  | Runbook | Runbook
    VMware para o Azure | Runbook | ND 
    Hyper-V com o VMM para o Azure | Runbook | Script
    Site Hyper-V para o Azure | Runbook | ND
    VMM para o VMM secundário | Script | Script

1. No plano de recuperação, clique no passo para a qual devem ser adicionados a ação e especificar quando a ação deve ocorrer: um. Se pretender que a ação para ocorrer antes das máquinas no grupo são iniciadas após a ativação pós-falha, selecione **adicionar pré-ação**.
    b. Se pretender que a ação para ocorrer após as máquinas no início do grupo após a ativação pós-falha, selecione **post adicionar ação**. Para mover a posição da ação, selecione o **mover para cima** ou **mover para baixo** botões.
2. No **inserir ação**, selecione **Script** ou **ação Manual**.
3. Se pretender adicionar uma ação manual, efetue o seguinte procedimento"um. Escreva um nome para a ação e o tipo instruções de ação. A pessoa que está a executar a ativação pós-falha irão ver estas instruções.
    b. Especifique se pretende adicionar a ação manual para todos os tipos de ativação pós-falha (teste, ativação pós-falha, a ativação pós-falha planeada (se relevante)). Em seguida, clique em **OK**.
4. Se pretender adicionar um script, efetue o seguinte: um. Se estiver a adicionar um script do VMM, selecione **ativação pós-falha para o script VMM**e, no **caminho do Script** escreva o caminho relativo para a partilha. Por exemplo, se a partilha está localizada em \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, especifique o caminho: \RPScripts\RPScript.PS1.
    b. Se estiver a adicionar uma automatização do Azure run book, especifique o **conta de automatização do Azure** em que o runbook está localizado e selecione as adequadas **o Script do Runbook do Azure**.
5. Execute uma ativação pós-falha do plano de recuperação para garantir que o script funciona conforme esperado.

## <a name="watch-a-video"></a>Ver um vídeo

Ver um vídeo que demonstra como criar um plano de recuperação.


> [!VIDEO https://www.youtube.com/embed/1KUVdtvGqw8]

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre [executar as ativações pós-falha](site-recovery-failover.md).  

    
