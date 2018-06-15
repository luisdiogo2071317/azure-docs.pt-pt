---
title: Subscrever uma oferta na pilha do Azure | Microsoft Docs
description: Criar subscrições para ofertas na pilha do Azure
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/11/2018
ms.author: brenduns
ms.openlocfilehash: 9153649774a67533649fb62da83a3f50abd592da
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295215"
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Criar subscrições de ofertas na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Depois de [criar uma oferta](azure-stack-create-offer.md), os utilizadores necessitam de uma subscrição para esse oferta antes de poderem utilizar. Existem duas formas que os utilizadores podem obter subscrito para uma oferta:

- Como um operador da nuvem, pode criar uma subscrição para um utilizador a partir do portal do administrador. Subscrições que cria podem ser para ofertas de públicas e privadas.
- Como um utilizador de inquilino, pode subscrever uma oferta pública ao utilizar o portal de utilizador.  

## <a name="create-a-subscription-as-a-cloud-operator"></a>Criar uma subscrição como um operador da nuvem

Os operadores da nuvem podem utilizar o portal de administração para criar uma subscrição para uma oferta para um utilizador.  Pode criar subscrições para membros do seu próprio inquilino de diretório.  Quando [vários inquilinos](azure-stack-enable-multitenancy.md) está ativada, também pode criar subscrições para os utilizadores em inquilinos do adicionais.

Se não pretender que os seus inquilinos a criar as seus próprios subscrições, faça as ofertas privada e, em seguida, criar subscrições para os inquilinos. Esta abordagem é comum quando integrar a pilha do Azure com os sistemas de catálogo de serviço ou de faturação externa.

Depois de criar uma subscrição para um utilizador, pode iniciar sessão no portal de utilizador e ver que está a subscrito para a oferta.  

### <a name="to-create-a-subscription-for-a-user"></a>Para criar uma subscrição para um utilizador

1. No portal de administração, aceda a **subscrições de utilizador.**
2. Selecione **Adicionar**. Em **nova subscrição de utilizador**, introduza as seguintes informações:  

   - **Nome a apresentar** – um nome amigável para identificar a subscrição que aparece como o *nome de subscrição de utilizador*.
   - **Utilizador** – Especifique um utilizador de um inquilino do diretório disponível para esta subscrição. O nome de utilizador é apresentado como *proprietário*.  O formato do nome de utilizador depende da sua solução de identidade. Por exemplo:

     - **Azure AD:***&lt;user1 > @&lt;contoso.onmicrosoft.com >* 

     - **AD FS:***&lt;user1 > @&lt;azurestack.local >* 

   - **Inquilino do** – selecione o inquilino de diretório em que a conta de utilizador pertence. Se ainda não ativou a vários inquilinos, apenas o inquilino de diretório local está disponível.

3. Selecione **oferecem**. Em **oferece**, escolha um **oferecem** para esta subscrição. Porque está a criar a subscrição para um utilizador, selecione **privada** como o estado de acessibilidade.

4. Selecione **criar** para criar a subscrição. Verá que a nova subscrição em **subscrição de utilizador**. Quando o utilizador inicia sessão no portal de utilizador podem ver os detalhes da subscrição.

### <a name="to-make-an-add-on-plan-available"></a>Para disponibilizar um plano de suplemento

Um operador da nuvem pode adicionar um plano de suplemento para uma subscrição em qualquer altura criada anteriormente:

1. No portal de administração, selecione **mais serviços** > **subscrições de utilizador**. Selecione a subscrição que pretende alterar.

2. Selecione **suplementos** e, em seguida, selecione **+ adicionar**.  

3. Em **adicionar plano**, selecione o plano de que pretende como suplemento.

## <a name="create-a-subscription-as-a-user"></a>Criar uma subscrição como um utilizador

Como um utilizador, pode iniciar sessão portal de utilizador para localizar e subscrever ofertas públicas e planos de suplemento para o seu inquilino do directory (organização).

>[!NOTE]
>Se o seu ambiente de pilha do Azure suporta [vários inquilinos](azure-stack-enable-multitenancy.md) também pode subscrever ofertas de um inquilino do diretório remoto.

### <a name="to-subscribe-to-an-offer"></a>Para subscrever uma oferta

1. [Inicie sessão no](azure-stack-connect-azure-stack.md) portal de utilizador de pilha do Azure (https://portal.local.azurestack.external) e selecione **obter uma subscrição**.

   ![Obter uma subscrição](media/azure-stack-subscribe-plan-provision-vm/image01.png)
  
2. Em **obter uma subscrição**, introduza o nome amigável da subscrição no **nome a apresentar**. Selecione **oferecem** e, em **escolher uma oferta**, escolha uma oferta. Selecione **criar** para criar a subscrição.

   ![Criar uma oferta](media/azure-stack-subscribe-plan-provision-vm/image02.png)
  
3. Depois de subscrever uma oferta, atualize o portal para ver quais os serviços fazem parte da nova subscrição.
4. Para ver a subscrição que criou, selecione **mais serviços** e, em seguida, selecione **subscrições**. Selecione a subscrição para ver os detalhes da subscrição.  

### <a name="to-subscribe-to-an-add-on-plan"></a>Para subscrever um plano de suplemento

Se uma oferta tiver um plano de suplemento, pode adicionar esse plano para a sua subscrição em qualquer altura.  

1. No portal de utilizador, selecione **mais serviços** > **subscrições**e, em seguida, selecione a subscrição que pretende alterar. Se existirem quaisquer planos de suplemento disponíveis, **+ adicionar plano** está ativa e se existe um mosaico para **planos de suplemento**.

   >[!NOTE]
   >Se **+ adicionar plano** não está ativo, em seguida, sabemos de nenhum planos de suplemento para a oferta associadas com essa subscrição.

1. Selecione **+ adicionar plano** ou **planos de suplemento** mosaico. Em **planos de suplemento**, selecione o plano de que pretende adicionar.

## <a name="next-steps"></a>Passos Seguintes

[Aprovisionar uma máquina virtual](azure-stack-provision-vm.md)
