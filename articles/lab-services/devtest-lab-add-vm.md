---
title: Adicionar uma VM a um laboratório no Azure DevTest Labs | Documentos da Microsoft
description: Saiba como adicionar uma máquina virtual a um laboratório no Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2018
ms.author: spelluru
ms.openlocfilehash: 9ddf44ef933270c08b42f67387866cd7a3b34719
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004084"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Adicionar uma VM a um laboratório no Azure DevTest Labs
Se já tiver [criado a sua primeira VM](devtest-lab-create-first-vm.md), provavelmente fez isso de um pré-carregados [imagem do marketplace](devtest-lab-configure-marketplace-images.md). Agora, se pretender adicionar VMs subsequentes para seu laboratório, pode também escolher uma *base* ou seja, um um [imagem personalizada](devtest-lab-create-template.md) ou uma [fórmula](devtest-lab-manage-formulas.md). Este tutorial explica-lhe utilizar o portal do Azure para adicionar uma VM a um laboratório no DevTest Labs.

Este artigo também mostra como gerir os artefactos para uma VM no seu laboratório.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Passos para adicionar uma VM a um laboratório no Azure DevTest Labs
1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
1. Na lista de laboratórios, selecione o laboratório em que pretende criar a VM.  
1. O laboratório **descrição geral** painel, selecione **+ adicionar**.  

    ![Adicionar botão VM](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Sobre o **Vyberte bázi** painel, selecione uma base para a VM.
1. Sobre o **Máquina Virtual** painel, introduza um nome para a nova máquina virtual no **nome da Máquina Virtual** caixa de texto.

    ![Painel da VM de laboratório](./media/devtest-lab-add-vm/devtestlab-lab-vm-blade.png)

1. Introduza um **nome de utilizador** que é concedido privilégios de administrador na máquina virtual.  
1. Se pretender utilizar uma palavra-passe armazenada numa [do Azure key vault](devtest-lab-store-secrets-in-key-vault.md), selecione **utilizar um segredo guardado**e especifique um valor de chave que corresponda ao seu segredo (palavra-passe). Caso contrário, introduza uma palavra-passe no campo texto da etiqueta **escreva um valor**. Para saber mais sobre a guardar segredos num cofre de chaves e utilizá-las durante a criação de recursos do laboratório, consulte [Store segredos no Azure Key Vault](devtest-lab-store-secrets-in-key-vault.md).
1. O **tipo de disco da Máquina Virtual** determina o tipo de disco de armazenamento é permitido para as máquinas virtuais no laboratório.
2. Selecione **tamanho da Máquina Virtual** e selecione um dos itens predefinidos que especificam os núcleos de processador, o tamanho de RAM e o tamanho de disco rígido da VM para criar.
3. Selecione **artefactos** - e, na lista de artefactos - selecionar e configurar os artefatos que pretende adicionar à imagem de base.
    **Nota:** se estiver familiarizado com o DevTest Labs ou configurar artefactos, consulte a [adicionar um artefato existente a uma VM](#add-an-existing-artifact-to-a-vm) secção e, em seguida, volte aqui quando terminar.
4. Selecione **definições avançadas** para configurar opções de rede e opções de expiração da VM. 

   Para definir uma opção de expiração, selecione o ícone de calendário para especificar uma data em que a VM será automaticamente eliminada.  Por predefinição, a VM nunca irá expirar. 
1. Se quiser exibir ou copiar o modelo Azure Resource Manager, consulte a [modelo Azure Resource Manager guardar](#save-azure-resource-manager-template) secção e voltar aqui quando terminar.
1. Selecione **criar** para adicionar a VM especificada para o laboratório.
1. O painel de laboratório apresenta o estado da criação da VM - primeiro como **Creating**, em seguida, como **em execução** depois da VM foi iniciada.

> [!NOTE]
> [Adicionar uma VM reclamável](devtest-lab-add-claimable-vm.md) mostra como tornar a VM reclamáveis para que fique disponível para utilização por qualquer utilizador no laboratório.
>
>

## <a name="add-an-existing-artifact-to-a-vm"></a>Adicionar um artefato existente a uma VM
Ao criar uma VM, pode adicionar artefactos existentes. Cada um deles inclui artefatos do repositório de artefactos de laboratórios DevTest público, bem como artefatos que criou e adicionado ao seu próprio repositório de artefactos.

* O Azure DevTest Labs *artefactos* permitem-lhe especificar *ações* que são executadas quando a VM está aprovisionada, como executar scripts do Windows PowerShell, executar os comandos de Bash e instalação de software.
* Artefactos *parâmetros* permitem-lhe personalizar o artefacto para o seu cenário

Para saber como criar artefactos, consulte o artigo [Aprenda a criar seu próprio artefactos para utilização com o DevTest Labs](devtest-lab-artifact-author.md).

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
1. Na lista de laboratórios, selecione o laboratório que contém a VM com a qual pretende trabalhar.  
1. Selecione **minhas máquinas virtuais**.
1. Selecione a VM pretendida.
1. Selecione **gerir artefactos**. 
1. Selecione **aplicam-se de artefactos**.
1. Sobre o **aplicam-se de artefactos** painel, selecione o artefacto que pretende adicionar à VM.
1. Sobre o **adicionar artefacto** painel, introduza os valores dos parâmetros necessários e quaisquer parâmetros opcionais que precisa.  
1. Selecione **Add** para adicionar o artefacto e voltar para o **aplicam-se de artefactos** painel.
1. Continue a adicionar os artefactos conforme necessário para a sua VM.
1. Depois de adicionar os artefactos, pode [alterar a ordem na qual são executados os artefactos](#change-the-order-in-which-artifacts-are-run). Também pode aceder novamente à [ver ou modificar um artefato](#view-or-modify-an-artifact).
1. Quando tiver terminado a adição de artefatos, selecione **aplicar**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Alterar a ordem na qual são executados os artefactos
Por predefinição, as ações dos artefactos são executadas na ordem em que são adicionados à VM. Os passos seguintes mostram como alterar a ordem na qual são executados os artefactos.

1. Na parte superior a **aplicam-se de artefactos** painel, selecione a ligação que indica o número de artefactos que foram adicionados à VM.
   
    ![Número de artefactos adicionado à VM](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Sobre o **selecionado artefactos** painel, arraste e largue os artefactos para a ordem pretendida. **Nota:** se tiver problemas ao arrastar o artefacto, certifique-se de que está sendo arrastado do lado esquerdo do artefacto. 
1. Selecione **OK** quando terminar.  

## <a name="view-or-modify-an-artifact"></a>Ver ou modificar um artefacto
Os passos seguintes mostram como ver ou modificar os parâmetros de um artefato:

1. Na parte superior a **aplicam-se de artefactos** painel, selecione a ligação que indica o número de artefactos que foram adicionados à VM.
   
    ![Número de artefactos adicionado à VM](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. Sobre o **selecionado artefactos** painel, selecione o artefacto que pretende visualizar ou editar.  
1. Sobre o **adicionar artefacto** painel, faça qualquer necessárias alterações e selecione **OK** para fechar o **adicionar artefacto** painel.
1. Selecione **OK** para fechar a **selecionado artefactos** painel.

## <a name="save-azure-resource-manager-template"></a>Guardar o modelo Azure Resource Manager
Um modelo Azure Resource Manager fornece uma forma declarativa de definir uma implementação repetível. Os passos seguintes explicam como pode guardar o modelo Azure Resource Manager para a VM que está sendo criada.
Depois de guardar, pode utilizar o modelo Azure Resource Manager para [implementar novas VMs com o Azure PowerShell](../azure-resource-manager/resource-group-overview.md#template-deployment).

1. Sobre o **Máquina Virtual** painel, selecione **ver o modelo de ARM**.
2. Sobre o **modelo de exibição do Azure Resource Manager** painel, selecione o texto de modelo.
3. Copie o texto selecionado para a área de transferência.
4. Selecione **OK** para fechar a **painel de visualização do Azure Resource Manager modelo**.
5. Abra um editor de texto.
6. Cole o texto do modelo da área de transferência.
7. Guarde o ficheiro para utilização posterior.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Passos Seguintes
* Quando a VM tiver sido criada, pode ligar à VM selecionando **Connect** no painel da VM.
* Saiba como [criar artefactos personalizados para a sua VM de laboratórios DevTest](devtest-lab-artifact-author.md).
* Explore os [Galeria de modelos de início rápido do DevTest Labs do Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
