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
ms.date: 06/07/2018
ms.author: brenduns
ms.openlocfilehash: e5b96a9464bf4d0e3b69d2f635da32c6648ce793
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247522"
---
# <a name="create-an-offer-in-azure-stack"></a>Criar uma oferta no Azure Stack

[Oferece](azure-stack-key-features.md) são grupos de um ou mais planos que Fornecedores aos utilizadores para comprar ou subscrever. Este documento mostra como criar uma oferta, que inclui o [plano que criou](azure-stack-create-plan.md). Esta oferta proporciona a capacidade de definir a segurança das máquinas virtuais de subscritores.

1. Inicie sessão no portal de administrador a pilha do Azure (https://adminportal.local.azurestack.external) e selecione **novo** > **inquilino oferece + planos** > **oferecem**.

   ![Criar uma oferta](media/azure-stack-create-offer/image01.png)
  
2. Em **oferecem novo**, introduza um **nome a apresentar** e um **nome do recurso**e, em **grupo de recursos**, selecione **criar novo** ou **utilizar existente**. O nome de apresentação é o nome amigável para a oferta. Este nome amigável é as únicas informações sobre a oferta que os utilizadores veem quando subscrever uma oferta. Utilize um nome intuitivo que ajuda os utilizadores a compreender o que é fornecido com a oferta. Apenas o administrador pode ver o Nome do Recurso. Trata-se do nome que o administrador utiliza para trabalhar com a oferta como um recurso do Azure Resource Manager.

   ![Nova oferta](media/azure-stack-create-offer/image01a.png)
  
3. Selecione **Base planos** para abrir o **planear**. Selecione os planos de que pretende incluir na oferta e, em seguida, escolha **selecione**. Para criar a oferta, selecione **criar**.

   ![Selecione o plano](media/azure-stack-create-offer/image02.png)
  
4. Depois de criar a oferta, pode alterar o estado. As ofertas têm de ser efetuadas *pública* para os utilizadores obter a vista completa, quando estes subscrever. Ofertas podem ser:

   - **Pública**: visíveis para os utilizadores.
   - **Privada**: apenas visíveis para os administradores de nuvem. Esta definição é útil ao drafting o plano ou a oferta, ou se pretender que o administrador da nuvem para [criar cada subscrição para os utilizadores](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Fora de Utilização**: fechados a novos subscritores. Pode utilizar o administrador da nuvem desativado impedir futuras subscrições, mas deixe atuais subscritores afetados.

   > [!TIP]  
   > As alterações para a oferta não são imediatamente visíveis ao utilizador. Para ver as alterações, os utilizadores poderão ter de terminar e iniciar sessão novamente para o portal de utilizador para ver a oferta de novo.

   Para alterar o estado da oferta:

   - **Versão 1803 e posterior**:  
     Na descrição geral para a oferta, selecione **Estado acessibilidade**. Escolha o estado de que pretende utilizar (por exemplo, *pública*) e, em seguida, selecione **guardar**.
 
     ![Selecione o estado de acessibilidade](media/azure-stack-create-offer/change-state.png)

     Como alternativa, depois de aceder a uma oferta pode aceder à **oferecem definições**. Selecione **Estado acessibilidade** de alteração do Estado.

   - **Antes de versão 1803**:  
     Selecione **todos os recursos**, procure oferta novo e, em seguida, selecione a nova oferta. Selecione **alteração de estado**e, em seguida, selecione **pública**.

   > [!NOTE]
   > Também pode utilizar o PowerShell para criar ofertas de predefinido, esquemas e quotas. Para obter mais informações, consulte [Leia-me do administrador de serviços do Azure pilha](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).

## <a name="next-steps"></a>Passos Seguintes

- [Criar subscrições](azure-stack-subscribe-plan-provision-vm.md)
- [Aprovisionar uma máquina virtual](azure-stack-provision-vm.md)
