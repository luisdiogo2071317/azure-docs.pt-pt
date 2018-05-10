---
title: Adicione uma declaração de suporte internas para um laboratório no Azure DevTest Labs | Microsoft Docs
description: Saiba como publicar uma declaração de suporte internas para um laboratório no Azure DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: c9900333-6c5e-4d7d-b0f4-889015e9550c
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 2d12ca26fb2aa5abddcf44b2e634b2f08b1fb01b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="add-an-internal-support-statement-to-a-lab-in-azure-devtest-labs"></a>Adicione uma declaração de suporte internas para um laboratório no Azure DevTest Labs

Azure DevTest Labs permite-lhe personalizar o seu laboratório com uma instrução de suporte interno que fornece aos utilizadores obter informações sobre o laboratório. Por exemplo, pode fornecer informações de contacto para que um utilizador sabe como contactar o suporte interno quando precisar de ajuda na resolução de problemas ou aceder aos recursos no laboratório. Também pode fornecer ligações para sites internos ou perguntas mais frequentes que pode aceder a sua equipa antes de contactar o suporte.

Uma instrução de suporte interno destina-se a permitem-lhe publicar informações de laboratório que normalmente não irá alterar demasiadas vezes. Para notificar os utilizadores sobre as informações de laboratório que é mais temporário natureza –, tais como atualizações recentes às políticas de laboratório – consulte [anúncio de mensagem num laboratório](devtest-lab-announcements.md).

Pode facilmente desativar ou editar uma declaração de suporte, depois de já não é aplicável.

## <a name="steps-to-add-a-support-statement-to-an-existing-lab"></a>Passos para adicionar uma declaração de suporte a um laboratório existente

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Se necessário, selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista. (O laboratório já pode ser apresentado no Dashboard em **todos os recursos**).
1. Na lista de laboratórios, selecione o laboratório no qual pretende adicionar uma declaração de suporte.  
1. No laboratório de **descrição geral** área selecione **políticas de configuração e**.  

    ![Botão de configuração e políticas](./media/devtest-lab-internal-support-message/devtestlab-config-and-policies.png)

1. No lado esquerdo em **definições**, selecione **suporte interno**.

    ![Botão de suporte interno](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)

1. Para criar uma mensagem de suporte internas para que os utilizadores neste laboratório, defina ativado como **Sim**.

1. No **suporte mensagem** campo, introduza a declaração de suporte interno que pretende para apresentar os utilizadores de laboratório. A mensagem de suporte aceita Markdown. Como introduzir o texto da mensagem, pode ver o **pré-visualização** área na parte inferior do ecrã para ver como a mensagem é apresentada aos utilizadores.

    ![Ecrã de suporte internas para criar a mensagem.](./media/devtest-lab-internal-support-message/devtestlab-add-support-statement.png)


1. Selecione **guardar** assim que a sua declaração de suporte está pronta para publicar.

Se já não pretende mostrar esta mensagem de suporte para os utilizadores de laboratório, volte ao **suporte interno** página e defina **ativado** para **não**.

## <a name="steps-for-users-to-view-the-support-message"></a>Passos para os utilizadores ver a mensagem de suporte

1. Do [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), selecione um laboratório.

1. No laboratório de **descrição geral** área selecione **suporte interno**.  

    ![Botão de suporte interno](./media/devtest-lab-internal-support-message/devtestlab-internal-support.png)


1. Se uma mensagem de suporte é publicada, o utilizador pode vê-la a partir do painel de suporte interno.

    ![Painel de suporte interno que mostra a mensagem de suporte publicada](./media/devtest-lab-internal-support-message/devtestlab-view-suport-statement.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Passos Seguintes
* As declarações de suporte interno, normalmente, são utilizadas para fornecer informações de suporte que não que mudam frequentemente. Também pode aprender como [publicar um anúncio para um laboratório](devtest-lab-announcements.md) para informar os utilizadores de temporária as alterações ou atualizações para o laboratório.
* [Definir políticas e as agendas](devtest-lab-set-lab-policy.md) fornece informações sobre como pode aplicar a outras restrições e convenções sua subscrição através da utilização de políticas personalizadas.