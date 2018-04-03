---
title: Criar uma oferta no Azure pilha | Microsoft Docs
description: Como um administrador da nuvem, saiba como criar uma oferta para os utilizadores na pilha do Azure.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2018
ms.author: brenduns
ms.openlocfilehash: 6a59d0c8144492ef907e5c395f05e4e8a16678a5
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/03/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Criar uma oferta no Azure Stack

[Oferece](azure-stack-key-features.md) são grupos de um ou mais planos que Fornecedores aos utilizadores para comprar ou subscrever. Este documento mostra como criar uma oferta, que inclui o [plano que criou](azure-stack-create-plan.md) no último passo. Esta oferta proporciona a capacidade para aprovisionar as máquinas virtuais de subscritores.

1. Inicie sessão no portal de administrador a pilha do Azure (https://adminportal.local.azurestack.external) > clique **novo** > **inquilino oferece + planos** > **oferecem**.

   ![](media/azure-stack-create-offer/image01.png)
2. No **oferecem novo** painel, preencha **nome a apresentar** e **nome do recurso**e, em seguida, selecione o novo ou existente **grupo de recursos**. O nome de apresentação é o nome amigável para a oferta. Este nome amigável é as únicas informações sobre a oferta que os utilizadores veem quando subscrever. Por conseguinte, não se esqueça de utilizar um nome intuitivo que ajuda o utilizador compreende que vem com a oferta. Apenas o administrador pode ver o Nome do Recurso. Trata-se do nome que o administrador utiliza para trabalhar com a oferta como um recurso do Azure Resource Manager.

   ![](media/azure-stack-create-offer/image01a.png)
3. Clique em **Base planos** para abrir o **planear** painel, selecione os planos de que pretende incluir na oferta e, em seguida, clique em **selecione**. Clique em **Criar** para criar a oferta.

   ![](media/azure-stack-create-offer/image02.png)
4. Depois de criar a oferta, pode alterar o estado. As ofertas têm de ser efetuadas *pública* para os utilizadores obter a vista completa, quando estes subscrever. Ofertas podem ser:
   - **Pública**: visíveis para os utilizadores.
   - **Privada**: apenas visíveis para os administradores de nuvem. Útil ao drafting o plano ou a oferta, ou se pretender que o administrador da nuvem para [criar cada subscrição para os utilizadores](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Fora de Utilização**: fechados a novos subscritores. Pode utilizar o administrador da nuvem desativado impedir futuras subscrições, mas deixe os subscritores atuais inalterados.

   > [!TIP]  
   > As alterações para a oferta não são imediatamente visíveis para o utilizador. Para ver as alterações, os utilizadores poderão ter a fim de sessão e início de sessão novamente para o portal de utilizador para ver a oferta de novo. 

   Para alterar o estado da oferta: 

   - **Versão 1803 e posterior**:  
     No painel de descrição geral para a oferta, clique em **Estado acessibilidade**, selecione o estado de que pretende utilizar, como *pública*e, em seguida, clique em **guardar**. 
 
     ![Selecione o estado de acessibilidade](media/azure-stack-create-offer/change-state.png) 

     Em alternativa, depois de aceder a uma oferta pode goto **oferecem definições**e, em seguida, selecione **Estado acessibilidade** de alteração do Estado. 

   - **Antes de versão 1803**:  
     Clique em **todos os recursos**, procure a sua oferta novo, clique na oferta da nova, clique em **alteração de estado**e, em seguida, clique em **pública**.

  
   > [!NOTE] 
   > Também pode utilizar o PowerShell para criar ofertas de predefinido, esquemas e quotas. Para obter mais informações, consulte [Leia-me do administrador de serviços do Azure pilha](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
   >


### <a name="next-steps"></a>Passos Seguintes
[Criar subscrições](azure-stack-subscribe-plan-provision-vm.md)      
[Aprovisionar uma máquina virtual](azure-stack-provision-vm.md)
