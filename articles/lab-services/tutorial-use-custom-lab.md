---
title: Aceder a um laboratório personalizado no Azure DevTest Labs | Microsoft Docs
description: Neste tutorial, vai aceder ao laboratório criado com o Azure DevTest Labs, reclamar máquinas virtuais, utilizá-las e, em seguida, anular a reclamação.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/09/2018
ms.author: spelluru
ms.openlocfilehash: ce4522673bac56f73944413d102b7cb36cf93f30
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-access-a-custom-lab-in-azure-devtest-labs"></a>Tutorial: Aceder a um laboratório personalizado no Azure DevTest Labs
Neste tutorial, vai utilizar o laboratório personalizado criado no [Tutorial: Criar um laboratório personalizado](tutorial-create-custom-lab.md).

Neste tutorial, irá realizar as seguintes ações:

> [!div class="checklist"]
> * Reclamar uma máquina virtual (VM) no laboratório personalizado
> * Ligar à VM
> * Anular reclamação da VM

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="access-the-lab"></a>Aceder ao laboratório

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. Selecione **Todos os recursos** no menu esquerdo. 
3. Selecione **DevTest Labs** para tipo de recurso. 
4. Selecione o laboratório. 

    ![Selecionar o laboratório](./media/tutorial-use-custom-lab/search-for-select-custom-lab.png)

## <a name="claim-a-vm"></a>Reclamar uma VM

1. Na lista de **Máquinas de virtuais reclamáveis**, selecione **...** (reticências) e, em seguida, **Reclamar máquina**.

    ![Reclamar máquina virtual](./media/tutorial-use-custom-lab/claim-virtual-machine.png)
1. Confirme que vê a VM na lista **Minhas máquinas virtuais**.

    ![Minha máquina virtual](./media/tutorial-use-custom-lab/my-virtual-machines.png)

## <a name="connect-to-the-vm"></a>Ligar à VM

1. Selecione a VM na lista. Verá a **página Máquina Virtual** da sua VM. Selecione **Ligar** na barra de ferramentas.

    ![Conectar à máquina virtual](./media/tutorial-use-custom-lab/connect-button.png)
2. Guarde o ficheiro **RDP** transferido no disco rígido e utilize-o para ligar à máquina virtual. Especifique o nome de utilizador e a palavra-passe mencionados quando a VM foi criada na secção anterior. 

## <a name="unclaim-the-vm"></a>Anular reclamação da VM
Depois de concluir a utilização da VM, anule a reclamação dela ao seguir estes passos: 

1. Na página da máquina virtual, selecione **Anular reclamação** na barra de ferramentas. 

    ![Anular reclamação da VM](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. A VM é encerrada antes de anular a reclamação dela. 

    ![Estado Anular reclamação](./media/tutorial-use-custom-lab/unclaim-status.png) 
1. Depois de terminar a operação de anulação da reclamação, veja a VM na lista de **Máquinas virtuais reclamáveis** na parte inferior. 
    
## <a name="next-steps"></a>Passos seguintes
Este tutorial mostrou como aceder e utilizar um laboratório personalizado criado com o Azure DevTest Labs. Para obter mais informações sobre como aceder e utilizar VMs num laboratório personalizado, veja 

> [!div class="nextstepaction"]
> [Como: Utilizar VMs num laboratório personalizado](devtest-lab-add-vm.md)

