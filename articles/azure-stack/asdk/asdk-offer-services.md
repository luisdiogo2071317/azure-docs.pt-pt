---
title: Neste tutorial, vai criar uma oferta de pilha do Azure | Microsoft Docs
description: Saiba como criar uma oferta de pilha do Azure, incluindo planos e quotas.
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
ms.openlocfilehash: 083b5e20b89f22cb8e523926858fe9ffb1441319
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="tutorial-offer-azure-stack-iaas-services"></a>Tutorial: oferecem serviços de IaaS de pilha do Azure
Como um administrador da nuvem de pilha do Azure, pode criar ofertas podem subscrever os seus utilizadores (algumas vezes referidos como inquilinos). Utilizar a subscrição, os utilizadores podem consumir, em seguida, serviços de pilha do Azure.

Este tutorial mostra como criar uma oferta para permitir aos utilizadores criar máquinas virtuais com base na imagem de centro de dados do Windows Server 2016 de pilha do Azure marketplace que carregou no [tutorial anterior](asdk-marketplace-item.md).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar uma oferta
> * Criar um plano
> * Definir quotas
> * Oferta de conjunto para público

Na pilha do Azure, os serviços são fornecidos aos utilizadores através de subscrições, ofertas e planos. Os utilizadores podem subscrever oferece várias. Ofertas podem ter um ou mais planos e planos podem ter um ou mais serviços, conforme mostrado no diagrama seguinte:

![As subscrições, ofertas e planos](media/asdk-offer-services/sop.png)

Como um operador de pilha do Azure oferece serviços, lhe for pedido pela primeira vez para criar a oferta, em seguida, um plano e, finalmente, quotas. Depois de criar uma oferta, os utilizadores de pilha do Azure, em seguida, podem subscrever ofertas através do portal de utilizador.

## <a name="create-an-offer"></a>Criar uma oferta
**Oferece** são grupos de um ou mais planos que os operadores do Azure pilha apresentam aos utilizadores para comprar ou subscrever.

1. Iniciar sessão para o [portal do Azure pilha](https://adminportal.local.azurestack.external) como um administrador da nuvem.

2. Clique em **novo** > **oferece + planos** > **oferecem**.

   ![Nova oferta](media/asdk-offer-services/new-offer.png)

2. No **oferecem novo** secção, preencha **nome a apresentar** e **nome do recurso**e, em seguida, selecione o novo ou existente **grupo de recursos**. O nome a apresentar é pública nome amigável a oferta que os utilizadores veem. Apenas o operador da nuvem, pode ver o nome de recurso, o que é utilizado por administradores para trabalhar com a oferta como um recurso do Azure Resource Manager.

   ![Nome a apresentar](media/asdk-offer-services/offer-display-name.png)


## <a name="create-a-plan"></a>Criar um plano
Depois de introduzir as informações de oferta básico, tem de adicionar pelo menos um plano de base para a oferta. 

**Planos** permitir operadores de pilha do Azure para os serviços de grupo e os respetivos quotas associadas, para ser disponibilizado a utilizadores.

1. Clique em **Base planos**e, no **plano** secção, clique em **adicionar** para adicionar um novo plano para a oferta.

   ![Adicione um plano](media/asdk-offer-services/new-plan.png)

2. No **planear novo** secção, preencha **nome a apresentar** e **nome do recurso**. O nome de apresentação é o nome amigável do plano e que os utilizadores veem. Apenas o operador da nuvem, pode ver o nome de recurso utilizado pelos operadores da nuvem para funcionar com o plano como um recurso do Azure Resource Manager.

   ![Planear o nome a apresentar](media/asdk-offer-services/plan-display-name.png)

3. Em seguida, selecione os serviços incluídos no plano. Para oferecer serviços de IaaS, clique em **serviços**, selecione **Microsoft. Compute**, **Network**, e **Microsoft**e, em seguida, Clique em **selecione**.

   ![Planear os serviços](media/asdk-offer-services/select-services.png)


## <a name="set-quotas"></a>Definir quotas
Agora que a oferta tem um plano que inclui serviços, tem de definir quotas para cada um deles. 

**Quotas** determinar a quantidade de recursos que um utilizador pode consumir para cada serviço incluído no plano de a ser oferecido.

1. Clique em **Quotas**e, em seguida, selecione o serviço para o qual pretende criar uma quota. 

   Para começar, primeiro de criar uma quota para o serviço de computação. Na lista de espaço de nomes, selecione o **Microsoft. Compute** espaço de nomes e, em seguida, clique em **criar nova quota**.
   
   ![Criar nova quota](media/asdk-offer-services/create-quota.png)

2. No **Criar quota** secção, escreva um nome para a quota, defina os parâmetros para a quota pretendidos e clique em **OK**.

   ![Nome de quota](media/asdk-offer-services/quota-properties.png)

3. Selecione a quota que criou para o **Microsoft. Compute** serviço.

   ![Selecione quota](media/asdk-offer-services/set-quota.png)

4. Repita os passos 1 a 3 para definir quotas para os serviços de armazenamento e de rede e, em seguida, clique em OK na secção de Quotas. Em seguida, clique em **OK** no **novo plano** secção para concluir a configuração de plano. 

   ![Todas as quotas definido](media/asdk-offer-services/all-quotas-set.png)

5. Conclua a criação de oferta clicando **criar** na secção de plano. Verá uma notificação quando foi criada com êxito a oferta e estas serão listadas como uma oferta disponível.

   ![Oferta criada](media/asdk-offer-services/offer-complete.png)

## <a name="set-offer-to-public"></a>Oferta de conjunto para público
As ofertas têm de ser efetuadas públicas para os utilizadores para vê-los ao selecionar uma oferta para subscrever. 

Ofertas podem ser:
- **Pública**: visíveis para todos os utilizadores.
- **Privada**: apenas visíveis para os administradores de nuvem. Útil ao drafting o plano ou a oferta, ou se pretender que o administrador da nuvem criar cada subscrição para os utilizadores.
- **Fora de Utilização**: fechados a novos subscritores. Pode utilizar o administrador da nuvem desativado impedir futuras subscrições, mas deixe os subscritores atuais inalterados.

> [!TIP]
> As alterações para a oferta não são imediatamente visíveis para os utilizadores. Para ver as alterações, os utilizadores poderão ter a fim de sessão e voltar a iniciar sessão para o [portal de utilizador](https://portal.local.azurestack.external) para ver a oferta de novo.

Para definir a oferta de novo para público: 

1. No menu do dashboard, clique em **oferece** e, em seguida, clique na oferta que criou.

2. Clique em **Alterar Estado** e, em seguida, em **Público**.

   ![Estado público](media/asdk-offer-services/set-public.png)

3. A oferta estarão agora disponível no portal de utilizador de pilha do Azure.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar uma oferta
> * Criar um plano
> * Definir quotas
> * Oferta de conjunto para público

Avançar para o próximo tutorial para saber como subscrever a oferta que acabou de criar como um utilizador de pilha do Azure.

> [!div class="nextstepaction"]
> [Subscrever uma oferta](asdk-subscribe-services.md)