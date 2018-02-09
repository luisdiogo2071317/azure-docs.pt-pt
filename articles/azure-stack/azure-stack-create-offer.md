---
title: Criar uma oferta no Azure pilha | Microsoft Docs
description: Como um administrador da nuvem, saiba como criar uma oferta para os utilizadores na pilha do Azure.
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: 96b080a4-a9a5-407c-ba54-111de2413d59
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/06/2018
ms.author: brenduns
ms.openlocfilehash: 2666aacbbaf44ae9b3bcf2df480e835457e6f449
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/09/2018
---
# <a name="create-an-offer-in-azure-stack"></a>Criar uma oferta no Azure Stack

*Aplica-se a: Azure pilha integrado sistemas e Kit de desenvolvimento de pilha do Azure*

[Oferece](azure-stack-key-features.md) são grupos de um ou mais planos que Fornecedores aos utilizadores para comprar ou subscrever. Este documento mostra como criar uma oferta, que inclui o [plano que criou](azure-stack-create-plan.md) no último passo. Esta oferta proporciona a capacidade para aprovisionar as máquinas virtuais de subscritores.

1. Inicie sessão no portal de administrador a pilha do Azure (https://adminportal.local.azurestack.external) > clique **novo** > **inquilino oferece + planos**  >   **Oferecer**.

   ![](media/azure-stack-create-offer/image01.png)
2. No **oferecem novo** painel, preencha **nome a apresentar** e **nome do recurso**e, em seguida, selecione o novo ou existente **grupo de recursos**. O nome de apresentação é o nome amigável para a oferta. Este nome amigável é as únicas informações sobre a oferta que os utilizadores veem quando subscrever. Por conseguinte, não se esqueça de utilizar um nome intuitivo que ajuda o utilizador compreende que vem com a oferta. Apenas o administrador pode ver o Nome do Recurso. Trata-se do nome que o administrador utiliza para trabalhar com a oferta como um recurso do Azure Resource Manager.

   ![](media/azure-stack-create-offer/image01a.png)
3. Clique em **Base planos** para abrir o **planear** painel, selecione os planos de que pretende incluir na oferta e, em seguida, clique em **selecione**. Clique em **Criar** para criar a oferta.

   ![](media/azure-stack-create-offer/image02.png)
4. Clique em **todos os recursos**, procure a sua oferta novo, clique na oferta da nova, clique em **alteração de estado**e, em seguida, clique em **pública**.

   ![](media/azure-stack-create-offer/image03.png)

As ofertas têm de ser efetuadas públicas para os utilizadores obter a vista completa, quando subscrever. Ofertas podem ser:

* **Pública**: visíveis para os utilizadores.
* **Privada**: apenas visíveis para os administradores de nuvem. Útil ao drafting o plano ou a oferta, ou se pretender que o administrador da nuvem para [criar cada subscrição para os utilizadores](azure-stack-subscribe-plan-provision-vm.md#create-a-subscription-as-a-cloud-operator).
* **Fora de Utilização**: fechados a novos subscritores. Pode utilizar o administrador da nuvem desativado impedir futuras subscrições, mas deixe os subscritores atuais inalterados.

As alterações para a oferta não são imediatamente visíveis para o utilizador. Para ver as alterações, poderá ter de terminar sessão/início de sessão para ver a nova subscrição no "seleccionador de subscrição" quando criar grupos de recursos/recursos.

> [!NOTE]
>Também pode criar ofertas predefinido, esquemas e quotas através do PowerShell conforme explicado no [Leia-me do administrador de serviços do Azure pilha](https://github.com/Azure/AzureStack-Tools/tree/master/ServiceAdmin).
>


### <a name="next-steps"></a>Passos Seguintes
[Criar subscrições](azure-stack-subscribe-plan-provision-vm.md)      
[Aprovisionar uma máquina virtual](azure-stack-provision-vm.md)
