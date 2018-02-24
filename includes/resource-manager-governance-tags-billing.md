---
title: incluir ficheiro
description: incluir ficheiro
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/19/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 7843410043b726526380b2a916d96f414a2decda
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
Depois de aplicar etiquetas a recursos, pode ver os custos para recursos com dessas etiquetas. Demora tempo para análise de custos ver a utilização mais recente, pelo que não poderá ver os custos ainda. Quando estiverem disponíveis os custos, pode ver os custos de recursos entre grupos de recursos na sua subscrição. Os utilizadores devem ter [acesso de nível de subscrição para as informações de faturação](../articles/billing/billing-manage-access.md) para ver os custos.

Para ver os custos por tag no portal, selecione a sua subscrição e selecione **análise de custos**.

![Análise de custo](./media/resource-manager-governance-tags-billing/select-cost-analysis.png)

Em seguida, o valor da etiqueta de filtro e selecione **aplicar**.

![Custo de vista por etiqueta](./media/resource-manager-governance-tags-billing/view-costs-by-tag.png)

Também pode utilizar o [APIs de faturação do Azure](../articles/billing/billing-usage-rate-card-overview.md) programaticamente ver os custos.
