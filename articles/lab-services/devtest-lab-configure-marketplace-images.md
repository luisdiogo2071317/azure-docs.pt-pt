---
title: Configurar definições de imagem do Azure Marketplace no Azure DevTest Labs | Microsoft Docs
description: Configurar as imagens do Azure Marketplace podem ser utilizadas ao criar uma VM no Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 90df853eeca89e61fa69e6967218c4c18274ac1b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Configurar definições de imagem do Azure Marketplace no Azure DevTest Labs
DevTest Labs suporta criar VMs baseadas nas imagens do Azure Marketplace, dependendo de como tiver configurado imagens do Azure Marketplace para ser utilizado no laboratório. Este artigo mostra como especificar que, se existirem, as imagens do Azure Marketplace podem ser utilizado ao criar as VMs num laboratório. Isto garante que a equipa de apenas tem acesso a imagens do Marketplace que precisam. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Selecione as imagens do Azure Marketplace são permitidas quando criar uma VM
1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista.
3. Na lista de laboratórios, selecione o laboratório pretendido. 
4. No painel do laboratório, selecione **políticas de configuração e**.
5. Do laboratório **políticas de configuração e** painel em **Bases de máquinas virtuais**, selecione **imagens do Marketplace**.
6. Especifique se pretende que as imagens do Azure Marketplace qualificadas para estar disponível para utilização como base de uma nova VM. Se selecionar **Sim**, em seguida, todos os no Azure Marketplace imagens que cumpram os critérios seguintes são permitidos no laboratório:
   
   * A imagem cria uma única VM, **e**
   * A imagem utiliza o Gestor de recursos do Azure para aprovisionar VMs, **e**
   * A imagem não necessita de comprar um plano de licenciamento extra
     
    Se pretender que não existem imagens para ser permitido ou pretender especificar que imagens podem ser utilizados, selecione **não**.
     
     ![Opção para permitir que todas as imagens do Marketplace ser utilizado como base imagens para VMs](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Se selecionar **não** para o passo anterior, o **permitidos imagens/selecionar todos os** caixa de verificação está ativada. 
   Pode utilizar esta opção juntamente com a caixa de pesquisa para rapidamente selecione ou desmarque todos os itens apresentados na lista.
   * Selecione as imagens do Azure Marketplace que pretende permitir a criação de VM individualmente marcando a caixa de verificação correspondente de cada imagem.
   * Selecione nada na lista se não pretender permitir que qualquer imagens do Azure Marketplace ser utilizado no laboratório.
   
    ![Pode especificar as imagens do Azure Marketplace podem ser utilizadas como base imagens para VMs](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Passos Seguintes
Assim que tiver configurado como imagens do Azure Marketplace são permitidas quando criar uma VM, o passo seguinte consiste em [adicionar uma VM para o laboratório](devtest-lab-add-vm.md).

