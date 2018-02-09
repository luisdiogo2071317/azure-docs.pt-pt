---
title: Subscrever uma oferta na pilha do Azure | Microsoft Docs
description: "Criar subscrições para ofertas na pilha do Azure"
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 7f3f8683-ef09-4838-92ed-41f2fddbbbed
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 9b35b497c264fab2b8352eda82fe6d4e1fc274e9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="create-subscriptions-to-offers-in-azure-stack"></a>Criar subscrições de ofertas na pilha do Azure

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

Depois de [criar uma oferta](azure-stack-create-offer.md), os utilizadores necessitam de uma subscrição para esse oferta antes de poderem utilizar.   
- Como um operador da nuvem, pode criar uma subscrição para um utilizador a partir do portal do administrador.  Subscrições que cria podem ser para ofertas de públicas e privadas.
- Como um utilizador de inquilino, pode subscrever uma oferta pública ao utilizar o portal de utilizador.  

As secções seguintes fornecem orientações para os operadores da nuvem, ao criar subscrições para utilizadores e como subscrever uma oferta como um utilizador.

## <a name="create-a-subscription-as-a-cloud-operator"></a>Criar uma subscrição como um operador da nuvem
Os operadores da nuvem podem utilizar o portal de administração para criar uma subscrição para uma oferta para um utilizador.  Pode criar subscrições para membros do seu próprio inquilino de diretório.  Quando [vários inquilinos](azure-stack-enable-multitenancy.md) está ativada, também pode criar subscrições para os utilizadores em inquilinos do adicionais.

Pode criar subscrições para ofertas de públicas e privadas.  Se não pretender que os seus inquilinos a criar as seus próprios subscrições, faça todas as suas ofertas privada e, em seguida, criar subscrições em nome de inquilinos. Esta abordagem é comum quando integrar a pilha do Azure com os sistemas de catálogo de serviço ou de faturação externa.

Depois de criar uma subscrição para um utilizador, esse utilizador pode iniciar sessão no portal de utilizador e irá encontrar que são subscrito para a oferta.  

### <a name="to-create-the-subscription-for-a-user"></a>Para criar a subscrição para um utilizador
1.  No portal de administração, aceda a **subscrições de utilizador.**
2.  Selecione **adicionar** para abrir o **nova subscrição de utilizador** painel. Aqui tem de especificar os seguintes detalhes:  

  a. **Nome a apresentar** – um nome amigável para identificar a subscrição que aparece como o *nome de subscrição de utilizador*.

  b. **Utilizador** – Especifique um utilizador de um inquilino do diretório disponível para esta subscrição. O nome de utilizador é apresentado como *proprietário*.  O formato do nome de utilizador depende da sua solução de identidade. Por exemplo:   

     -  **Azure AD:***&lt;user1 > @&lt;contoso.onmicrosoft.com >* 

     -   **AD FS:***&lt;user1 > @&lt;azurestack.local >*      

  c.    **Inquilino do** – selecione o inquilino de diretório em que a conta de utilizador pertence. Se não tiver ativado a vários inquilinos, apenas o inquilino de diretório local está disponível.

3.  Selecione **oferecem** para abrir o **oferece** painel e, em seguida, escolha um **oferecem** para esta subscrição. Porque está a criar uma subscrição para um utilizador, pode selecionar uma oferta privada ou pública.

4.  Selecione **criar** para criar a subscrição. O **subscrições de utilizador** painel agora apresenta a nova subscrição.  Mais tarde, quando o utilizador inicia sessão no portal de utilizador, pode ver detalhes sobre esta subscrição.

### <a name="to-make-an-add-on-plan-available"></a>Para disponibilizar um plano de suplemento  
Um operador da nuvem pode adicionar um plano de suplemento para uma subscrição em qualquer altura criada anteriormente:   
1.  No portal de administração aceda a **mais serviços** > **subscrições de utilizador**e, em seguida, selecione a subscrição que pretende alterar.   

2.  Selecione **suplementos** > **adicionar** para abrir o **adicionar plano** painel.  

3.  Selecione o plano que pretende adicionar como um suplemento.  A consola, em seguida, apresenta os planos associados à subscrição.




## <a name="create-a-subscription-as-a-user"></a>Criar uma subscrição como um utilizador
Como um utilizador, pode iniciar sessão portal de utilizador para localizar e subscrever ofertas públicas e planos de suplemento do seu inquilino do directory (organização). Quando o ambiente de pilha do Azure suporta [vários inquilinos](azure-stack-enable-multitenancy.md) pode subscrever para ofertas de um inquilino do diretório remoto.

### <a name="to-subscribe-to-an-offer"></a>Para subscrever uma oferta
1. [Inicie sessão no](azure-stack-connect-azure-stack.md) no portal de utilizador de pilha do Azure (https://portal.local.azurestack.external) e clicar em **obter uma subscrição**.

   ![Obter uma subscrição](media/azure-stack-subscribe-plan-provision-vm/image01.png)
2. No **nome a apresentar** campo, escreva um nome para a sua subscrição, clique em **oferecem**, clique das ofertas no **escolher uma oferta** painel e, em seguida, clique em  **Criar**.

   ![Criar uma oferta](media/azure-stack-subscribe-plan-provision-vm/image02.png)
3. Para ver a subscrição que criou, clique em **mais serviços**, clique em **subscrições**, em seguida, clique em sua nova subscrição.  

Depois de subscrever uma oferta, atualize o portal para ver quais os serviços fazem parte da nova subscrição.

### <a name="to-subscribe-to-an-add-on-plan"></a>Para subscrever um plano de suplemento
Se uma oferta tiver um plano de suplemento, pode adicionar esse plano para a sua subscrição em qualquer altura.  

1. No portal de utilizador, selecione **mais serviços** > **subscrições**e, em seguida, selecione a subscrição que pretende alterar.

2. Selecione **adicionar plano** botão e, em seguida, selecione o plano de suplemento quiser. Se **adicionar plano** está não disponível, em seguida, existem não planos de suplemento para a oferta associados a essa subscrição.



## <a name="next-steps"></a>Passos Seguintes
[Aprovisionar uma máquina virtual](azure-stack-provision-vm.md)
