---
title: Criar e gerir VMs reclamáveis num laboratório no Azure DevTest Labs | Documentos da Microsoft
description: Saiba como adicionar uma máquina virtual reclamáveis a um laboratório no Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2018
ms.author: spelluru
ms.openlocfilehash: 3bfb674fa66f0701a099d237f4e760453c7b6a6e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232132"
---
# <a name="create-and-manage-claimable-vms-in-azure-devtest-labs"></a>Criar e gerir VMs reclamáveis no Azure DevTest Labs
Adicionar uma VM reclamável a um laboratório de forma semelhante a como [adicionar uma VM padrão](devtest-lab-add-vm.md) – num *base* ou seja, seja um [imagem personalizada](devtest-lab-create-template.md), [fórmula](devtest-lab-manage-formulas.md) , ou [imagem do Marketplace](devtest-lab-configure-marketplace-images.md). Este tutorial explica-lhe utilizar o portal do Azure para adicionar uma VM reclamável a um laboratório no DevTest Labs e mostra os processos de um utilizador que se segue para reclamar e removerá os a VM.

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Passos para adicionar uma VM reclamável a um laboratório no Azure DevTest Labs
1. Inicie sessão no [portal do Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
1. Na lista de laboratórios, selecione o laboratório em que pretende criar a VM reclamável.  
1. O laboratório **descrição geral** painel, selecione **+ adicionar**.  

    ![Adicionar botão VM](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Sobre o **Vyberte bázi.** área, selecione uma base para a VM.
1. Na **Máquina Virtual** painel, introduza um nome para a nova máquina virtual no **nome da Máquina Virtual** caixa de texto.

    ![Painel da VM de laboratório](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Introduza um **nome de utilizador** que é concedido privilégios de administrador na máquina virtual.  
1. Se pretender utilizar uma palavra-passe armazenada no seu [arquivo de segredos](https://azure.microsoft.com/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store), selecione **utilizar um segredo guardado**e especifique um valor de chave que corresponda ao seu segredo (palavra-passe). Caso contrário, introduza uma palavra-passe no campo texto da etiqueta **escreva um valor**.
1. O **tipo de disco da Máquina Virtual** determina o tipo de disco de armazenamento é permitido para as máquinas virtuais no laboratório.
1. Selecione **tamanho da Máquina Virtual** e selecione um dos itens predefinidos que especificam os núcleos de processador, o tamanho de RAM e o tamanho de disco rígido da VM para criar.
1. Selecione **artefactos** e, na lista de artefatos, selecionar e configurar os artefatos que pretende adicionar à imagem de base. Se estiver familiarizado com o DevTest Labs ou configurar artefactos, consulte a [adicionar um artefato existente a uma VM](devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) secção e, em seguida, volte aqui quando terminar.
1. Selecione **definições avançadas** para configurar opções de rede e opções de expiração da VM. Sob **opções de afirmação**, escolha **Sim** para tornar a máquina reclamáveis.

  ![Escolha esta opção garantir a VM reclamáveis.](./media/devtest-lab-add-vm/devtestlab-claim-VM-option.png)

1. Se quiser exibir ou copiar o modelo Azure Resource Manager, consulte a [modelo Azure Resource Manager guardar](devtest-lab-add-vm.md#save-azure-resource-manager-template) secção e voltar aqui quando terminar.
1. Selecione **criar** para adicionar a VM especificada para o laboratório.

   O estado da criação da VM é apresentado, primeiro como **Creating**, em seguida, como **em execução** depois da VM foi iniciada.

> [!NOTE]
> Se implementar VMs do laboratório através de [modelos Azure Resource Manager](devtest-lab-create-environment-from-arm.md), pode criar VMs reclamáveis definindo a **allowClaim** propriedade como true na secção de propriedades.
>
>

## <a name="using-a-claimable-vm"></a>Utilizar uma VM reclamável

Um utilizador pode reivindicar qualquer VM na lista de "Máquinas de virtuais Reclamáveis" efetuando um dos seguintes passos:

* Na lista de "Máquinas de virtuais Reclamáveis" na parte inferior do painel de "Descrição geral" do laboratório, faça duplo clique das VMs na lista e escolha **máquina de afirmação**.

 ![Pedir uma VM reclamável específica.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* Na parte superior do painel "Descrição geral", escolha **afirmação qualquer**. Uma máquina virtual aleatória é atribuída a partir da lista de VMs reclamáveis.

 ![Qualquer VM reclamável do pedido.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Depois de uma VM de afirmações de um utilizador, é atrasado em sua lista de "Minhas máquinas virtuais" e não se encontra reclamáveis por qualquer outro utilizador.

## <a name="unclaim-a-vm"></a>Adicione uma VM

Quando um usuário concluído com uma VM solicitada e quiser torná-la disponível para outra pessoa, eles podem retornar a VM solicitada para a lista de máquinas de virtuais reclamáveis efetuando um dos seguintes passos:

- Na lista de "Minhas máquinas virtuais", com o botão direito em uma das VMs na lista – ou selecione nas respetivas reticências (.....) – e escolha **Unclaim**.

  ![Adicione uma VM na lista de VM.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM2.png)

- Na lista de "Minhas máquinas virtuais", selecione uma VM para abrir o painel de gestão, em seguida, selecione **Unclaim** da barra de menus superior.

  ![Adicione uma VM no painel de gestão da VM.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM.png)

Quando um utilizador unclaims uma VM, já não tem quaisquer permissões para essa VM de laboratório específica.

### <a name="transferring-the-data-disk"></a>Transferir o disco de dados
Se uma VM reclamável tem um disco de dados anexado a ele e um utilizador unclaims-lo, o disco de dados mantém-se com a VM. Quando outro usuário, em seguida, as afirmações que a VM, que o utilizador novo afirmações o disco de dados, bem como a VM.

Isso é conhecido como "Transferir o disco de dados". O disco de dados, em seguida, fica disponível na lista do novo usuário **Moje datové disky** para os mesmos gerir.

![Removerá os discos de dados.](./media/devtest-lab-add-vm/devtestlab-unclaim-datadisks.png)



## <a name="next-steps"></a>Passos Seguintes
* Depois de criado, pode ligar à VM selecionando **Connect** no seu painel de gestão.
* Explore os [Galeria de modelos de início rápido do Azure Resource Manager do DevTest Labs](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
