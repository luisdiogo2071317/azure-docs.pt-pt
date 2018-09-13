---
title: Reimplementar uma VM num laboratório no Azure DevTest Labs | Documentos da Microsoft
description: Saiba como voltar a implementar uma máquina virtual (mover de um nó do Azure para outro) no Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8460f09e-482f-48ba-a57a-c95fe8afa001
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: spelluru
ms.openlocfilehash: 273b0f1105d8b71b90a06e2627e201b97f12a754
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35649991"
---
# <a name="redeploy-a-vm-in-a-lab-in-azure-devtest-labs"></a>Reimplementar uma VM num laboratório no Azure DevTest Labs
Se não conseguir ligar a uma máquina virtual (VM) num laboratório através de uma ligação de ambiente de trabalho remoto, volte a implementar a VM e tente novamente o conencting a ele. Quando voltar a implementar uma VM, DevTest Labs muda a VM a partir do nó no qual está em execução para um novo nó dentro da infraestrutura do Azure. Em seguida, inicia a VM, mantendo a todas as suas opções de configuração e os recursos associados. Desta forma, poupa recursos o tempo gasto na solução de problemas de sua conexão de área de trabalho remoto ou o acesso a aplicações para VMs baseadas em Windows no laboratório. 

## <a name="steps-to-redeploy-a-vm-in-a-lab"></a>Passos para voltar a implementar uma VM num laboratório 
Para Reimplementar uma VM num laboratório no Azure DevTest Labs, siga os passos seguintes: 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
3. Na lista de laboratórios, selecione o laboratório que inclui a VM que pretende voltar a implementar.  
4. No painel esquerdo, selecione **minhas máquinas de virtuais**. 
5. Na lista de VMs, selecione uma VM.
6. Na página da Máquina Virtual para a sua VM, selecione **Reimplementar** sob **operações** no menu à esquerda.

    ![Voltar a implementar](media/devtest-lab-redeploy-vm/redeploy.png)
7. Ler as informações na página e selecione **Reimplementar** botão. 9. Verificar o estado da operação de reimplementação na **notificações** janela.

    ![Implementar novamente o Estado](media/devtest-lab-redeploy-vm/redeploy-status.png)

## <a name="next-steps"></a>Passos Seguintes
Saiba como redimensionar uma VM no Azure DevTest Labs, consulte [redimensionar uma VM](devtest-lab-resize-vm.md).


