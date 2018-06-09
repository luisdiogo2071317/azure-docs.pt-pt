---
title: Neste artigo, irá aprender a ofertas de pilha do Azure e os planos de atualização | Microsoft Docs
description: Este artigo descreve como visualizar e modificar as ofertas de pilha do Azure e planos existentes.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.custom: mvc
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a84148a3ac31d51ff30cebffab00e5fec8fdaa87
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238507"
---
# <a name="azure-stack-add-on-plans"></a>Planos de suplemento de pilha do Azure
Como um operador de pilha do Azure, criar planos que contêm as quotas aplicáveis para os seus utilizadores subscrever e serviços pretendidos. Estes [ *base planos* ](azure-stack-create-plan.md) contêm os serviços de núcleos para oferecidos aos seus utilizadores e só pode ter um plano base por oferta. Se necessitar de modificar a sua oferta, pode utilizar *planos de suplemento* que lhe permitem modificar o plano para expandir o computador, armazenamento, ou quotas inicialmente fornecidas com o plano de base de rede. 

Apesar de combinar tudo num único plano pode ser ideal em alguns casos, poderá ter uma base de planear e oferecer serviços adicionais utilizando planos de suplemento. Por exemplo, pode optar por oferecem IaaS services como parte de um plano de base, com todos os serviços de PaaS tratados como planos de suplemento. Planos também podem ser utilizados para controlar o consumo de recursos no seu ambiente de pilha do Azure. Por exemplo, se pretender que os utilizadores para serem mindful da respetiva utilização de recursos, pode ter um plano de base relativamente pequeno (consoante os serviços necessários) e como os utilizadores aceder a capacidade, podem ser alertados que já tenha consumido a alocação de recursos com base no seu plano atribuído. A partir daí, os utilizadores podem selecionar um plano de suplemento disponíveis para obter recursos adicionais. 

> [!NOTE]
> Quando um utilizador adiciona um plano de suplemento para uma subscrição de oferta existente, os recursos adicionais pode demorar até uma hora apresentada. 

## <a name="create-an-add-on-plan"></a>Criar um plano de suplemento
Planos de suplemento são criados ao modificar uma oferta existente:

1. Inicie sessão no portal de administrador de pilha do Azure como um administrador da nuvem.
2. Siga os mesmos passos utilizados para [criar um novo plano base](azure-stack-create-plan.md) para criar um novo plano oferece serviços que não foram fornecidos anteriormente. Neste exemplo, serviços do Cofre de chaves (keyvault) serão incluídos no plano de novo.
3. No portal do administrador, clique em **oferece** e, em seguida, selecione a oferta para ser atualizado com um plano de suplemento.

   ![](media/create-add-on-plan/1.PNG)

4.  Desloque-se na parte inferior das propriedades da oferta e selecione **planos de suplemento**. Clique em **Adicionar**.
   
    ![](media/create-add-on-plan/2.PNG)

5. Selecione o plano para adicionar. Neste exemplo, o plano é chamado **plano do Cofre de chave**e, em seguida, clique em **selecione** para adicionar o plano para a oferta. Deverá receber uma notificação que o plano foi adicionado com êxito para a oferta.
   
    ![](media/create-add-on-plan/3.PNG)

6. Reveja a lista de suplemento listados planos incluídos com a oferta para verificar que o suplemento do novo plano.
   
    ![](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Passos Seguintes
[Criar uma oferta](azure-stack-create-offer.md)