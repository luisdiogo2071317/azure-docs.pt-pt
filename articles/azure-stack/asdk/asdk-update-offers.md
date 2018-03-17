---
title: "Neste tutorial, irá aprender a ofertas de pilha do Azure e os planos de atualização | Microsoft Docs"
description: Este artigo descreve como visualizar e modificar as ofertas de pilha do Azure e planos existentes.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 3b5d2ab5e924f578f5838d11b0d2058aacf67dec
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-update-offers-and-plans"></a>Tutorial: atualização oferece e planos
Como um operador de pilha do Azure, criar planos que contêm as quotas aplicáveis para os seus utilizadores subscrever e serviços pretendidos. Estes *base planos* contêm os serviços de núcleos para oferecidos aos seus utilizadores e só pode ter um plano base por oferta. Se necessitar de modificar a sua oferta, pode utilizar *planos de suplemento* que lhe permitem modificar o plano para expandir o computador, armazenamento, ou quotas inicialmente fornecidas com o plano de base de rede. 

Apesar de combinar tudo num único plano pode ser ideal em alguns casos, poderá ter uma base de planear e oferecer serviços adicionais utilizando planos de suplemento. Por exemplo, pode optar por oferecem IaaS services como parte de um plano de base, com todos os serviços de PaaS tratados como planos de suplemento. Planos também podem ser utilizados para controlar o consumo de recursos no seu ambiente ASDK limitado. Por exemplo, se pretender que os utilizadores para serem mindful da respetiva utilização de recursos, pode ter um plano de base relativamente pequeno (consoante os serviços necessários) e como os utilizadores aceder a capacidade, podem ser alertados que já tenha consumido a alocação de recursos com base no seu plano atribuído. A partir daí, os utilizadores podem selecionar um plano de suplemento disponíveis para obter recursos adicionais. 

> [!NOTE]
> Quando um utilizador adiciona um plano de suplemento para uma subscrição de oferta existente, os recursos adicionais pode demorar até uma hora apresentada. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um plano de suplemento 
> * Subscrever o plano de suplemento

## <a name="create-an-add-on-plan"></a>Criar um plano de suplemento
**Planos de suplemento** são criados, modificando uma oferta existente.

1. Iniciar sessão para o [portal do Azure pilha](https://adminportal.local.azurestack.external) como um administrador da nuvem.
2. Siga os mesmos passos utilizados anteriormente para [criar um plano de base](asdk-offer-services.md) para criar um novo plano oferece serviços que não foram fornecidos anteriormente. Neste exemplo, serão incluídos no plano de serviços do Cofre de chaves (keyvault).
3. No portal do administrador, clique em **oferece** e, em seguida, selecione a oferta para ser atualizado com um plano de suplemento.

   ![](media/asdk-update-offers/1.PNG)

4.  Desloque-se na parte inferior das propriedades da oferta e selecione **planos de suplemento**. Clique em **Adicionar**.
   
    ![](media/asdk-update-offers/2.PNG)

5. Selecione o plano para adicionar. Neste exemplo, o plano é chamado **plano do Cofre de chave**e, em seguida, clique em **selecione** para adicionar o plano para a oferta. Deverá receber uma notificação que o plano foi adicionado com êxito para a oferta.
   
    ![](media/asdk-update-offers/3.PNG)

6. Reveja a lista de suplemento listados planos incluídos com a oferta para verificar que o suplemento do novo plano.
   
    ![](media/asdk-update-offers/4.PNG)

## <a name="subscribe-to-the-add-on-plan"></a>Subscrever o plano de suplemento
Terá de iniciar sessão no portal de utilizador de pilha do Azure para expandir uma subscrição de pilha do Azure existente com um plano de suplemento.

Utilize estes passos para detetar recursos adicionais que foi disponibilizados pela operadora de rede de pilha do Azure e adicione um plano de suplemento para uma oferta que anteriormente deverá subscrever.

1. Inicie sessão no [portal de utilizador](https://portal.local.azurestack.external).
2. Para localizar a subscrição para expandir com o plano de suplemento, clique em **mais serviços**, clique em **subscrições**, em seguida, selecione a sua subscrição.
   
    ![](media/asdk-update-offers/5.PNG)

3.  Na descrição geral subscrição, clique em **adicionar plano**.
   
    ![](media/asdk-update-offers/6.PNG)

4. No painel do plano de adicionar, selecione o plano de suplemento para adicionar à subscrição. Neste exemplo, **plano do Cofre de chave** está selecionada. Em seguida, deverá receber uma notificação que o plano de suplemento foi adquirido com êxito e que terá de atualizar o portal para aceder à subscrição atualizada.
   
    ![](media/asdk-update-offers/7.PNG)

5. Por fim, reveja os planos de suplemento associados à subscrição para se certificar de que o plano de suplemento foi adicionado com êxito.
   
    ![](media/asdk-update-offers/8.PNG)


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um plano de suplemento 
> * Subscrever o plano de suplemento

> [!div class="nextstepaction"]
> [Criar uma máquina virtual a partir de um modelo](asdk-create-vm-template.md)

