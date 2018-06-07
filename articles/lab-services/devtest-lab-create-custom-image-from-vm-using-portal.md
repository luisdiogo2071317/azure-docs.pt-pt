---
title: Criar uma imagem personalizada do Azure DevTest Labs a partir de uma VM | Microsoft Docs
description: Saiba como criar uma imagem personalizada no Azure DevTest Labs de uma VM aprovisionada no portal do Azure
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 22f1579b2df2acdc736ed4c1d5cee64d096c320a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635921"
---
# <a name="create-a-custom-image-from-a-vm"></a>Criar uma imagem personalizada a partir de uma VM

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Instruções passo-a-passo

Pode criar uma imagem personalizada a partir de uma VM aprovisionada e, posteriormente, utilizar essa imagem personalizada para criar VMs idênticas. Os passos seguintes mostram como criar uma imagem personalizada a partir de uma VM:

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.

1. Na lista de laboratórios, selecione o laboratório pretendido.  

1. No painel principal o laboratório, selecione **meu máquinas de virtuais**.
 
1. No **meu máquinas de virtuais** painel, selecione a VM a partir do qual pretende criar a imagem personalizada.

1. No painel de gestão da VM, selecione **criar imagem personalizada (VHD)**.

    ![Criar item de menu de imagem personalizada](./media/devtest-lab-create-template/create-custom-image.png)

1. No **imagem personalizada** painel, introduza um nome e descrição para a sua imagem personalizada. Estas informações são apresentadas na lista de bases de quando criar uma VM. A imagem personalizada incluirá o disco de SO e todos os discos de dados ligados à máquina virtual.

    ![Criar painel imagem personalizada](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Selecione se o sysprep foi executado a VM. Se o sysprep não foi executada a VM, especifique se pretende que o sysprep executar quando é criada uma VM a partir desta imagem personalizada.

1. Selecione **OK** quando terminar para criar a imagem personalizada.

Após alguns minutos, a imagem personalizada é criada e estiver armazenada no interior da conta de armazenamento de laboratório. Quando um utilizador do laboratório pretende criar uma nova VM, a imagem está disponível na lista de imagens de base.

![Imagem personalizada disponível na lista de imagens de base](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Mensagens de blogue relacionados

- [Imagens personalizadas ou fórmulas?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copiar as imagens personalizadas entre Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Passos Seguintes

- [Adicionar uma VM para o laboratório](devtest-lab-add-vm.md)
