---
title: Criar uma oferta no Azure Stack | Documentos da Microsoft
description: Como um administrador da nuvem, saiba como criar uma oferta para os seus utilizadores no Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: efemmano
ms.openlocfilehash: 4ccff997c7e9f29aafc6966730ab36dfcf72ca9f
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077345"
---
# <a name="create-an-offer-in-azure-stack"></a>Criar uma oferta no Azure Stack

[Oferece](azure-stack-key-features.md) são grupos de um ou mais planos que os fornecedores apresentam aos utilizadores para comprar ou subscrever. Este documento mostra como criar uma oferta que inclua o [plano que criou](azure-stack-create-plan.md). Esta oferta fornece aos subscritores a capacidade de definir a segurança de máquinas virtuais.

1. Inicie sessão no portal de administrador do Azure Stack (https://adminportal.local.azurestack.external) e selecione **+ criar um recurso** > **inquilino ofertas + planos** > **oferecem**.

   ![Criar uma oferta](media/azure-stack-create-offer/image01.png)
  
2. Sob **nova oferta**, introduza um **nome a apresentar** e um **nome do recurso**e, em **grupo de recursos**, selecione **Create novos** ou **utilizar existente**. O nome a apresentar é o nome amigável para a oferta. Este nome amigável é a única informação sobre a oferta que os utilizadores veem quando eles subscrevem uma oferta. Utilize um nome intuitivo que ajuda os utilizadores a compreender o que vem com a oferta. Apenas o administrador pode ver o Nome do Recurso. Trata-se do nome que o administrador utiliza para trabalhar com a oferta como um recurso do Azure Resource Manager.

   ![Nova oferta](media/azure-stack-create-offer/image01a.png)
  
3. Selecione **Planos Base** para abrir o **planear**. Selecione os planos que pretende incluir na oferta e, em seguida, escolha **selecione**. Para criar a oferta, selecione **criar**.

   ![Selecionar plano](media/azure-stack-create-offer/image02.png)
  
4. Depois de criar a oferta, pode alterar seu estado. Ofertas têm de ser feitas *público* para os utilizadores obter a vista completa quando eles subscrevem. Ofertas podem ser:

   - **Público**: visíveis para os utilizadores.
   - **Privada**: apenas visíveis para os administradores da cloud. Esta definição é útil durante a elaboração de plano ou à oferta, ou se o administrador da nuvem deseja [criar cada subscrição para os utilizadores](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
   - **Fora de Utilização**: fechados a novos subscritores. Pode utilizar o administrador da nuvem desativados para evitar futuras subscrições, mas deixe os assinantes atuais afetada.

   > [!TIP]  
   > As alterações para a oferta não são imediatamente visíveis ao utilizador. Para ver as alterações, os utilizadores poderão ter de terminar e iniciar sessão novamente para o portal de utilizador para ver a nova oferta.

   Na descrição geral para a oferta, selecione **estado de acessibilidade**. Escolha o estado de que pretende utilizar (por exemplo, **pública**) e, em seguida, selecione **guardar**.
 
     ![Escolha o Estado](media/azure-stack-create-offer/change-stage-1807.png)

     Como alternativa, selecione **alterar estado** e, em seguida, escolha um Estado.

    ![Selecione o estado de acessibilidade](media/azure-stack-create-offer/change-stage-select-1807.png)

   > [!NOTE]
   > Também pode utilizar o PowerShell para criar o padrão de ofertas, planos e quotas. Para obter mais informações, consulte [módulo do PowerShell do Azure Stack 1.4.0](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.4.0).

## <a name="next-steps"></a>Passos Seguintes

- [Criar subscrições](azure-stack-subscribe-plan-provision-vm.md)
- [Aprovisionar uma máquina virtual](azure-stack-provision-vm.md)
