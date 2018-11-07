---
title: Criar a sua primeira VM num laboratório no Azure DevTest Labs | Documentos da Microsoft
description: Saiba como criar a sua primeira máquina virtual num laboratório no Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: fbc5a438-6e02-4952-b654-b8fa7322ae5f
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 93ce9feaf52282b9477d49eaf270d6d89dca7811
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51232533"
---
# <a name="create-your-first-vm-in-a-lab-in-azure-devtest-labs"></a>Criar a sua primeira VM num laboratório no Azure DevTest Labs

Ao acessar o DevTest Labs inicialmente e pretende criar a sua primeira VM, provavelmente farão isso usando um pré-carregados [imagem do base marketplace](devtest-lab-configure-marketplace-images.md). Mais tarde no, também poderá escolher entre uma [imagem personalizada e uma fórmula](devtest-lab-add-vm.md) durante a criação de mais VMs. 

Este tutorial explica-lhe utilizar o portal do Azure para adicionar a sua primeira VM a um laboratório no DevTest Labs.

## <a name="steps-to-add-your-first-vm-to-a-lab-in-azure-devtest-labs"></a>Passos para adicionar a sua primeira VM a um laboratório no Azure DevTest Labs
1. Inicie sessão no [portal do Azure](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
1. Na lista de laboratórios, selecione o laboratório em que pretende criar a VM.  
1. O laboratório **descrição geral** painel, selecione **+ adicionar**.  

    ![Adicionar botão VM](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)

1. Sobre o **Vyberte bázi.** painel, selecione um mercado de imagens para a VM.
1. Sobre o **Máquina Virtual** painel, introduza um nome para a nova máquina virtual no **nome da Máquina Virtual** caixa de texto.

    ![Painel da VM de laboratório](./media/devtest-lab-add-vm/devtestlab-lab-add-first-vm.png)

1. Introduza um **nome de utilizador** que é concedido privilégios de administrador na máquina virtual.  
1. Introduza uma palavra-passe no campo texto da etiqueta **escreva um valor**.
1. O **tipo de disco da Máquina Virtual** determina o tipo de disco de armazenamento é permitido para as máquinas virtuais no laboratório.
1. Selecione **tamanho da Máquina Virtual** e selecione um dos itens predefinidos que especificam os núcleos de processador, o tamanho de RAM e o tamanho de disco rígido da VM para criar.
1. Selecione **artefactos** - e, na lista de artefactos - selecionar e configurar os artefatos que pretende adicionar à imagem de base.
    **Nota:** se estiver familiarizado com o DevTest Labs ou configurar artefactos, consulte a [adicionar um artefato existente a uma VM](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) secção e, em seguida, volte aqui quando terminar.
1. Selecione **criar** para adicionar a VM especificada para o laboratório.

   O painel de laboratório apresenta o estado da criação da VM - primeiro como **Creating**, em seguida, como **em execução** depois da VM foi iniciada.

## <a name="next-steps"></a>Passos Seguintes
* Quando a VM tiver sido criada, pode ligar à VM selecionando **Connect** no painel da VM.
* Confira [adicionar uma VM a um laboratório](devtest-lab-add-vm.md) para informações mais completas sobre como adicionar VMs subsequentes no seu laboratório.
* Explore os [Galeria de modelos de início rápido do DevTest Labs do Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates).
