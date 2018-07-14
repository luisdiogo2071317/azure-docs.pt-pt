---
title: Pedidos de aumentar a quota de vCPU de Gestor de recursos do Azure | Documentos da Microsoft
description: Pedidos de aumento de quota de vCPU de Gestor de recursos do Azure
author: ganganarayanan
ms.author: gangan
ms.date: 6/13/2018
ms.topic: article
ms.service: microsoft-docs
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 7456785815dbefb2436713814965d90ba0e789ee
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/13/2018
ms.locfileid: "39037271"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Pedidos de aumento de quota de vCPU de Gestor de recursos

Quotas de vCPU de Gestor de recursos são aplicadas ao nível de região e o nível de família de SKU.
Obter mais informações sobre como as quotas são aplicadas no [subscrição do Azure e limites do serviço](http://aka.ms/quotalimits) página.
Para obter mais informações sobre famílias de SKU, pode comparar o custo e desempenho no [preços de máquinas virtuais](http://aka.ms/pricingcompute) página.

Para pedir um aumento, siga as instruções abaixo para criar um pedido de suporte através do Azure "utilização + quotas" painel disponível no Portal do Azure. 

## <a name="request-quota-increase-at-subscription-level"></a>Pedir aumento da quota ao nível da subscrição

1. Partir https://portal.azure.com, selecione **subscrições**.

   ![Subscrições](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Selecione a subscrição que tem uma quota maior.

   ![Selecionar subscrição](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Selecione **utilização + quotas**

   ![Selecione a utilização e quotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. No canto superior direito, selecione **pedir aumento**.

   ![Pedir aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Passo: 1 – Selecione **núcleos** como o tipo de cotação. 

   ![Preencha o formulário](./media/resource-manager-core-quotas-request/forms.png)
   
6. Passo: 2 – implementação selecione modelar como "Resource Manager" e selecione uma localização.

    ![Painel de problema de quota](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Selecione as famílias de SKU que exigem um aumento.

    ![Séries SKU selecionadas](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. Introduza nos novos limites de que pretende na subscrição.

    ![Pedido de quota de novo SKU](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Para remover uma linha, desmarque o SKU no menu pendente família SKU ou clique no ícone de rejeição "x".
Depois de introduzir a quota pretendida para cada família SKU, clique em "Seguinte", na página de passo de problema para continuar com a criação do pedido de suporte.

