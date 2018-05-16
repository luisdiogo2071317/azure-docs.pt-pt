---
title: Criar um laboratório personalizado com o Azure DevTest Labs | Microsoft Docs
description: Neste início rápido, vai criar um laboratório personalizado com o Azure DevTest Labs.
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
ms.openlocfilehash: df70322a2d6562fce8da17fde652dd0dfbfcaec4
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-set-up-a-custom-lab-by-using-azure-devtest-labs"></a>Tutorial: Configurar um laboratório personalizado com o Azure DevTest Labs
Neste tutorial, vai criar um laboratório personalizado através do portal do Azure. O administrador do laboratório configura um laboratório numa organização, cria VMs no laboratório e configura as políticas. Os utilizadores do laboratório (por exemplo: o programador e os técnicos de teste) reivindicam as VMs no laboratório, ligam-se a elas e utilizam-nas. 

Neste tutorial, irá realizar as seguintes ações:

> [!div class="checklist"]
> * Criar um laboratório personalizado
> * Adicionar máquinas virtuais (VM) ao laboratório
> * Adicionar um utilizador à função de Utilizador do Laboratório

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="create-a-custom-lab"></a>Criar um laboratório personalizado
Os passos seguintes mostram como utilizar o portal do Azure para criar um laboratório no Azure DevTest Labs. 

1. Inicie sessão no [portal do Azure](https://portal.azure.com).
2. No menu principal no lado esquerdo, selecione **Criar recurso** (na parte superior da lista), aponte para **Ferramentas de programação** e clique em **DevTest Labs**. 

    ![Menu Novo DevTest Lab](./media/tutorial-create-custom-lab/new-custom-lab-menu.png)
1. Na janela **Criar um DevTest Lab**, realize as seguintes ações: 
    1. Em **Nome do laboratório**, introduza um nome para o laboratório. 
    2. Em **Subscrição**, selecione a subscrição na qual pretende criar o laboratório. 
    3. Em **Grupo de recursos**, selecione **Criar novo** e introduza um nome para o grupo de recursos. 
    4. Em **Localização**, selecione a localização/regiãoonde pretende criar o laboratório. 
    5. Selecione **Criar**. 
    6. Selecione **Afixar ao dashboard**. Depois de criar o laboratório, poderá vê-lo no dashboard. 

        ![Criar uma secção de laboratório do DevTest Labs](./media/tutorial-create-custom-lab/create-custom-lab-blade.png)

## <a name="add-a-vm-to-the-lab"></a>Adicionar uma VM ao laboratório

1. Na página **DevTest Lab**, selecione **+ Adicionar** na barra de ferramentas. 

    ![Botão Adicionar](./media/tutorial-create-custom-lab/add-vm-to-lab-button.png)
1. Na página **Escolher uma base**, pesquise com a palavra-chave **Ubuntu** e selecione uma das imagens de base na lista. 
1. Na página **Máquina virtual**, realize as seguintes ações: 
    1. Em **Nome da máquina virtual**, introduza um nome para a máquina virtual. 
    2. Para **Nome de utilizador**, introduza um nome para o utilizador com acesso à máquina virtual. 
    3. Em **Introduzir um valor**, introduza a palavra-passe do utilizador. 
    4. Selecione **Definições avançadas**.
    5. Para **Tornar esta máquina reclamável**, selecione **Sim**.
    6. Confirme que a **contagem de instâncias** está definida como **1**. Se for definida como **2**, serão criadas duas VMs com os nomes: `<base image name>00' and <base image name>01`. Por exemplo: `win10vm00` e `win10vm01`. 
    7. Para fechar a página **Avançado**, clique em **OK**. 
    8. Selecione **Criar**. 

        ![Escolher uma base](./media/tutorial-create-custom-lab/new-virtual-machine.png)
    9. Pode ver o estado da VM na lista de **Máquinas de virtuais reclamáveis**. A criação da máquina virtual pode demorar cerca de 25 minutos. A VM é criada num grupo de recursos do Azure separado, cujo nome começa com o nome do grupo de recursos atual no qual está o laboratório. Por exemplo, se o laboratório estiver em `labrg`, a VM poderá ser criada no grupo de recursos `labrg3988722144002`. 

        ![Estado de criação da VM](./media/tutorial-create-custom-lab/vm-creation-status.png)
1. Uma vez criada a VM, será apresentada na lista de **Máquinas de virtuais reclamáveis**. 

## <a name="add-a-user-to-the-lab-user-role"></a>Adicionar um utilizador à função de Utilizador do Laboratório

1. Selecione **Configuração e políticas** no menu esquerdo. 

    ![Configuração e políticas](./media/tutorial-create-custom-lab/configuration-and-policies-menu.png)
1. Selecione **Controlo de acesso(IAM)** no menu e, em seguida, selecione **+ Adicionar** na barra de ferramentas. 

    ![Controlo de acesso–- botão Adicionar utilizador](./media/tutorial-create-custom-lab/access-control-add.png)
1. Na página **Adicionar permissões**, realize as seguintes ações:
    1. Em **função**, selecione **Utilizador do DevTest Labs**. 
    2. Selecione o **utilizador** que pretende adicionar. 
    3. Selecione **Guardar**.

        ![Adicionar permissões](./media/tutorial-create-custom-lab/add-lab-user.png)
4. Para fechar **Configuração e políticas – Controlo de acesso (IAM)**, selecione **X** no canto direito. 

## <a name="cleanup-resources"></a>Recursos de limpeza
O próximo tutorial mostra como um utilizador do laboratório pode reivindicar e ligar-se a uma VM no laboratório. Se não quiser completar esse tutorial e quiser limpar os recursos criados como parte deste tutorial, siga estes passos: 

1. No portal do Azure, selecione **Grupos de recursos** no menu. 
2. Selecione o grupo de recursos no qual criou o laboratório. 
3. Selecione **Eliminar grupo de recursos** na barra de ferramentas. A eliminação de um grupo de recursos elimina todos os recursos no grupo, incluindo o laboratório. 
4. Repita estes passos para eliminar o grupo de recursos adicional criado por si com o nome `<your resource group name><random numbers>`. Por exemplo: `splab3988722144001`. As VMs são criadas neste grupo de recursos em vez de serem criadas no grupo de recursos no qual existe o laboratório. 

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, criou um laboratório personalizado com uma VM e concedeu acesso ao laboratório a um utilizador. Para saber como aceder ao laboratório enquanto um utilizador do laboratório, avance para o próximo tutorial:

> [!div class="nextstepaction"]
> [Tutorial: Aceder ao laboratório personalizado](tutorial-use-custom-lab.md)

