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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38740600"
---
Depois de aplicar etiquetas a recursos, pode ver os custos dos recursos com essas etiquetas. Demora algum tempo para a análise de custos mostrar a utilização mais recente, pelo que poderá ainda não ver os custos. Quando os custos estão disponíveis, pode ver os custos dos recursos entre os grupos de recursos na sua subscrição. Os utilizadores devem ter [acesso ao nível de subscrição das informações de faturação](../articles/billing/billing-manage-access.md) para ver os custos.

Para ver os custos por etiqueta no portal, selecione a sua subscrição e selecione **Análise de Custos**.

![Análise de custos](./media/resource-manager-governance-tags-billing/select-cost-analysis.png)

Em seguida, filtre por valor da etiqueta e selecione **Aplicar**.

![Ver custo por etiqueta](./media/resource-manager-governance-tags-billing/view-costs-by-tag.png)

Também pode utilizar as [APIs de Faturação do Azure](../articles/billing/billing-usage-rate-card-overview.md) para ver os custos programaticamente.
