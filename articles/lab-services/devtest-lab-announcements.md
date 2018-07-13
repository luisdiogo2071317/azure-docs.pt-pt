---
title: Publicar um comunicados para um laboratório no Azure DevTest Labs | Documentos da Microsoft
description: Saiba como adicionar um anúncio a um laboratório no Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 67a09946-4584-425e-a94c-abe57c9cbb82
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 3282a90069ecd119154df492ac6dc366d26b5300
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611210"
---
# <a name="post-an-announcement-to-a-lab-in-azure-devtest-labs"></a>Publicar um anúncio a um laboratório no Azure DevTest Labs

Enquanto administrador de laboratório, pode postar um anúncio personalizado num laboratório existente para notificar os utilizadores sobre as alterações recentes ou adições ao laboratório. Por exemplo, talvez queira informar os utilizadores sobre:

- Novos tamanhos VM disponíveis
- Imagens que estão atualmente inutilizáveis
- Atualizações às políticas de laboratório

Depois de publicado, o anúncio é apresentado na página de descrição geral do laboratório e o usuário pode selecioná-lo para obter mais detalhes.

A funcionalidade de anúncio destina-se a ser utilizado para notificações temporárias.  Pode desativar um anúncio facilmente depois de já não for necessário.

## <a name="steps-to-post-an-announcement-in-an-existing-lab"></a>Etapas para lançar um anúncio num laboratório existente

1. Inicie sessão no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Se necessário, selecione **todos os serviços**e, em seguida, selecione **DevTest Labs** da lista. (Seu laboratório já pode ser mostrado no Dashboard sob **todos os recursos**).
1. Na lista de laboratórios, selecione o laboratório em que pretende publicar um anúncio.  
1. O laboratório **descrição geral** área, selecione **Konfigurace a zásady**.  

    ![Botão de configuração e políticas](./media/devtest-lab-announcements/devtestlab-config-and-policies.png)

1. À esquerda sob **configurações**, selecione **anúncio de laboratório**.

    ![Botão de anúncio de laboratório](./media/devtest-lab-announcements/devtestlab-announcements.png)

1. Para criar uma mensagem para os utilizadores neste laboratório, defina **Enabled** ao **Sim**.

1. Pode introduzir um **data de expiração** para especificar uma data e hora após o qual o anúncio já não é apresentado aos utilizadores. Se não introduzir uma data de expiração, o anúncio permanece até que desabilitá-la.

   > [!NOTE]
   > Após o anúncio, já não é apresentado aos utilizadores, mas ainda existe no **anúncio de laboratório** painel. Pode efetuar edições ao mesmo e reativá-la para ativá-lo novamente.
   >
   >

1. Introduza um **título do anúncio** e o **text Oznámení**.

   O título pode ter até 100 carateres e é mostrado ao usuário na página de descrição geral do laboratório. Se o utilizador selecionar o título, o texto de anúncio é apresentado.

   O texto do comunicado aceita markdown. Como introduzir o texto de anúncio, pode ver a mensagem na área de pré-visualização na parte inferior do ecrã.

    ![Ecrã de anúncio de laboratório para criar a mensagem.](./media/devtest-lab-announcements/devtestlab-post-announcement.png)


1. Selecione **guardar** assim que o anúncio estiver pronto para ser publicado.

Quando já não pretende mostrar este anúncio aos utilizadores de laboratório, volte para o **anúncio de laboratório** página e defina **ativado** para **não**. Se tiver especificado uma data de expiração, o anúncio é desativado automaticamente em que de data e hora.

## <a name="steps-for-users-to-view-an-announcement"></a>Passos para que os utilizadores vejam um anúncio

1. Partir do [portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040), selecione um laboratório.

1. Se o laboratório tem um anúncio publicado para o mesmo, o aviso de informações é mostrado na parte superior da página de descrição geral do laboratório. Este aviso de informações é o título de anúncio que foi especificado quando o anúncio foi criado.

    ![Anúncio de laboratório na página Descrição geral](./media/devtest-lab-announcements/devtestlab-user-announcement.png)

1. O utilizador pode selecionar a mensagem para ver o anúncio completo.

    ![Obter mais informações para o anúncio de laboratório](./media/devtest-lab-announcements/devtestlab-user-announcement-text.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Passos Seguintes
* Se alterar ou definir uma política de laboratório, pode querer publicar um anúncio para informar os utilizadores. [Definir políticas e agendas](devtest-lab-set-lab-policy.md) fornece informações sobre como aplicar restrições e convenções na sua subscrição ao utilizar políticas personalizadas.
* Explore os [Galeria de modelos de início rápido do DevTest Labs do Azure Resource Manager](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
