---
title: Criar uma imagem personalizada do Azure DevTest Labs a partir de uma VM | Microsoft Docs
description: Saiba como criar uma imagem personalizada no Azure DevTest Labs de uma VM aprovisionada no portal do Azure
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: v-craic
ms.openlocfilehash: ce854024df5c6974873cf3574bf2de282946f6d0
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="create-a-custom-image-from-a-vm"></a>Criar uma imagem personalizada a partir de uma VM

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Instruções passo-a-passo

Pode criar uma imagem personalizada a partir de uma VM aprovisionada e, posteriormente, utilizar essa imagem personalizada para criar VMs idênticas. Os passos seguintes mostram como criar uma imagem personalizada a partir de uma VM:

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecione **Mais serviços**, e, em seguida, selecione **DevTest Labs** na lista.

1. Na lista de laboratórios, selecione o laboratório pretendido.  

1. No painel do laboratório, selecione **meu máquinas de virtuais**.
 
1. No **meu máquinas de virtuais** painel, selecione a VM a partir do qual pretende criar a imagem personalizada.

1. No painel da VM, selecione **criar imagem personalizada (VHD)**.

    ![Criar item de menu de imagem personalizada](./media/devtest-lab-create-template/create-custom-image.png)

1. No **criar imagem** painel, introduza um nome e descrição para a sua imagem personalizada. Estas informações são apresentadas na lista de bases de quando criar uma VM.

    ![Criar painel imagem personalizada](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Selecione se o sysprep foi executado a VM. Se o sysprep não foi executada a VM, especifique se pretende que o sysprep executar quando é criada uma VM a partir desta imagem personalizada.

1. Selecione **OK** quando terminar para criar a imagem personalizada.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Mensagens de blogue relacionados

- [Imagens personalizadas ou fórmulas?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copiar as imagens personalizadas entre Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

##<a name="next-steps"></a>Passos Seguintes

- [Adicionar uma VM para o laboratório](devtest-lab-add-vm.md)
