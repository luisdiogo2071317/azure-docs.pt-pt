---
title: Neste artigo, irá aprender a atualizar os planos e ofertas do Azure Stack | Documentos da Microsoft
description: Este artigo descreve como ver e modificar as ofertas do Azure Stack e planos existentes.
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
ms.date: 07/30/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: a35ba993e6fd1162fa4a18bc0d6bc9351fe7dfa2
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358286"
---
# <a name="azure-stack-add-on-plans"></a>Planos de suplementos do Azure Stack

Como um operador do Azure Stack, criar planos de suplementos para modificar uma [ *plano base* ](azure-stack-create-plan.md) quando pretender oferecer serviços adicionais ou expandir *computador*, *armazenamento* , ou *rede* quotas para além da oferta inicial de planos base. Planos de suplementos modificar o plano de base e são as extensões opcionais que os utilizadores podem optar por subscrever. 

Existem ocasiões combinar tudo num único plano é ideal. Outras vezes poderá ter uma base de planos e ofertas, em seguida, os serviços adicionais ao utilizar planos de suplementos. Por exemplo, poderia optar por oferecer serviços IaaS como parte de um plano base, com todos os serviços de PaaS tratados como planos de suplementos.

Outro motivo para usar os planos de suplementos é ajudar os utilizadores a estar atento a sua utilização de recursos. Para fazer isso, pode começar com um plano base que inclui quotas relativamente pequenas (consoante os serviços necessários). Em seguida, como utilizadores atingem a capacidade, eles seriam ser alertados de que eles consumiu a alocação de recursos com base no respetivo plano atribuído. A partir daí, os utilizadores, em seguida, podem selecionar um plano de suplemento que fornece os recursos adicionais.

> [!NOTE]
> Quando não quiser utilizar um plano de suplemento para expandir uma quota, também pode optar por [editar a configuração original da quota de](azure-stack-quota-types.md#to-edit-a-quota). 

Quando um utilizador adiciona um plano de suplemento a uma subscrição de oferta existente, os recursos adicionais poderiam demorar uma hora a aparecer. 

## <a name="create-an-add-on-plan"></a>Criar um plano de suplemento
Planos de suplementos são criados ao modificar uma oferta existente:

1. Inicie sessão no portal de administrador do Azure Stack como um administrador da nuvem.
2. Siga os passos utilizados para [criar um novo plano base](azure-stack-create-plan.md) para criar um novo plano de oferta de serviços que não foram oferecidos anteriormente. Neste exemplo, serviços de Cofre de chaves (Microsoft. keyvault) serão incluídos no novo plano.
3. No portal do administrador, clique em **oferece** e, em seguida, selecione a oferta para ser atualizado com um plano de suplemento.

   ![](media/create-add-on-plan/1.PNG)

4.  Desloque para baixo das propriedades da oferta e selecione **planos de suplementos**. Clique em **Adicionar**.
   
    ![](media/create-add-on-plan/2.PNG)

5. Selecione o plano para adicionar. Neste exemplo, o plano é designado **plano do Cofre de chave**. Depois de selecionar o plano, clique em **selecione** para adicionar o plano à oferta. Deverá receber uma notificação que o plano foi adicionado com êxito para a oferta.
   
    ![](media/create-add-on-plan/3.PNG)

6. Reveja a lista de suplemento planos incluídos com a oferta para verificar que o suplemento do novo plano listados.
   
    ![](media/create-add-on-plan/4.PNG)

## <a name="next-steps"></a>Passos Seguintes
[Criar uma oferta](azure-stack-create-offer.md)