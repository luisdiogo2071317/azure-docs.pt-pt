---
title: Criar uma imagem personalizada do Azure DevTest Labs a partir de um ficheiro VHD | Microsoft Docs
description: Saiba como criar uma imagem personalizada no Azure DevTest Labs de um ficheiro VHD utilizando o portal do Azure
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: v-craic
ms.openlocfilehash: d1f1b9948fb591484c107818a01e141932effbba
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>Criar uma imagem personalizada de um ficheiro VHD

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Instruções passo-a-passo

Os seguintes passos guiá-lo a criar uma imagem personalizada de um ficheiro VHD utilizando o portal do Azure:

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.

1. Na lista de laboratórios, selecione o laboratório pretendido.  

1. No painel principal o laboratório, selecione **políticas de configuração e**. 

1. No **políticas de configuração e** painel, selecione **imagens personalizadas**.

1. No **imagens personalizadas** painel, selecione **+ adicionar**.

    ![Adicionar a imagem personalizada](./media/devtest-lab-create-template/add-custom-image.png)

1. Introduza o nome da imagem personalizada. Este nome é apresentado na lista de imagens de base, ao criar uma VM.

1. Introduza a descrição da imagem personalizada. Esta descrição é apresentada na lista de imagens de base, ao criar uma VM.

1. Para **tipo de SO**, selecione **Windows** ou **Linux**.

    - Se selecionar **Windows**, especifique através da caixa de verificação se *sysprep* tiver sido executado na máquina. 
    - Se selecionar **Linux**, especifique através da caixa de verificação se *deprovision* tiver sido executado na máquina. 

1. Selecione um **VHD** no menu pendente. Este é o VHD que será utilizado para criar a nova imagem personalizada. Se necessário, selecione a **carregar um VHD com o PowerShell**.

1. Também pode introduzir um nome do plano, oferta do plano e fabricante de plano se a imagem utilizada para criar a imagem personalizada não está publicada pela Microsoft.

1. Selecione **OK** para criar a imagem personalizada.

Após alguns minutos, a imagem personalizada é criada e estiver armazenada no interior da conta de armazenamento de laboratório. Quando um utilizador do laboratório pretende criar uma nova VM, a imagem está disponível na lista de imagens de base.

![Imagem personalizada disponível na lista de imagens de base](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>Mensagens de blogue relacionados

- [Imagens personalizadas ou fórmulas?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Copiar as imagens personalizadas entre Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Passos Seguintes

- [Adicionar uma VM para o laboratório](./devtest-lab-add-vm.md)
