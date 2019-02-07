---
title: Monitorizar a utilização e gastos com alertas de custo | Documentos da Microsoft
description: Este artigo descreve como o custo alertas Ajuda monitorizar utilização e gastos no Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/05/2019
ms.topic: conceptual
ms.service: cost-management
manager: alavital
ms.custom: ''
ms.openlocfilehash: 813ec8f74371b6ae76ac306aea2c462f0beea1fb
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/06/2019
ms.locfileid: "55774175"
---
# <a name="use-cost-alerts-to-monitor-usage-and-spending"></a>Utilizar alertas de custo para monitorizar a utilização e gastos

Este artigo ajuda-o a compreender e utilizar os alertas de gestão de custos para monitorizar o seu Azure utilização e gastos. Custos de alertas são geradas automaticamente com base em quando os recursos do Azure são consumidos. Alertas mostram todos os Active Directory, gestão de custos e alertas de faturação em conjunto num único local. Quando o consumo de atinge um determinado limite, os alertas são gerados pelo Cost Management. Existem três tipos de alertas de custo: alertas de orçamento, alertas de crédito e alertas de quota de gastos do departamento.

## <a name="budget-alerts"></a>Alertas de orçamento

Alertas de orçamento notificá-lo quando gastos, com base na utilização ou custo, atingirem ou exceder o montante definido no [condição do orçamento de alerta](tutorial-acm-create-budgets.md). Orçamentos de gestão de custos são criados no portal do Azure ou o [Azure-consumo](https://docs.microsoft.com/rest/api/consumption) API. 

No portal do Azure, orçamentos são definidos pelo custo. Utilizar a API de consumo do Azure, orçamentos são definidos pelo custo ou pela utilização de consumo. Alertas de orçamento suportam orçamentos baseados em utilização e custos. Alertas de orçamento são gerados automaticamente sempre que estiverem reunidas as condições de alerta de orçamento. Pode ver todos os alertas de custo no portal do Azure. Sempre que é gerado um alerta, é apresentada nos alertas de custo. Um e-mail de alerta também é enviado para as pessoas na lista de destinatários de alertas do orçamento.

## <a name="credit-alerts"></a>Alertas de crédito

Alertas de crédito notificá-lo quando seus compromissos monetários do crédito do Azure são consumidos. São compromissos monetários para organizações com contratos Enterprise. Crédito alertas são geradas automaticamente a 90% e a 100% do seu saldo de crédito do Azure. Sempre que é gerado um alerta, é refletida nos alertas de custo e no e-mail enviado para os proprietários de conta.

## <a name="department-spending-quota-alerts"></a>Alertas de quota de gastos departamento

Alertas de quota de gastos departamento notificá-lo quando gastos do departamento atingem um limiar fixo da quota. Quotas de gastos são configuradas no portal do EA. Sempre que um limiar for cumprido gera uma mensagem de e-mail para os proprietários de departamento e é apresentado nos alertas de custo. Por exemplo, 50% ou 75% da quota.

## <a name="view-cost-alerts"></a>Alertas da vista de custos

No portal do Azure, clique em **Cost Management + Faturação** na lista de serviços. Em seguida, na lista, no **Cost Management**, selecione **custo alertas**.

![Imagem de exemplo de alertas, mostrado na gestão de custos](./media/cost-mgt-alerts-monitor-usage-spending/budget-alerts-fullscreen.png)

O número total de alertas ativos e ignoradas é apresentada na página de alertas de custo.

Todos os alertas mostram o tipo de alerta. Um alerta de orçamento mostra o motivo por que foi gerado e o nome do orçamento aplica-se. Cada alerta mostra a data em que foi gerado, o estado e o âmbito (subscrição ou gestão de grupo) que o alerta se aplica a.

Possível estado inclui **Active Directory** e **dispensada**. Estado do Active Directory indica que o alerta ainda é relevante. Estado ignorado indica que alguém tiver marcado o alerta para o definir como não mais relevante.

Selecione um alerta na lista para ver os detalhes. Detalhes do alerta mostram mais informações sobre o alerta. Alertas de orçamento incluem um link para o orçamento. Se uma recomendação está disponível para um alerta de orçamento, uma ligação para a recomendação também é mostrada. Orçamento, crédito e alertas de quota de gastos departamento tem uma ligação para analisar em análise de custo em que pode explorar os custos de âmbito do alerta. O exemplo seguinte mostra a gastos para um departamento com detalhes do alerta.

![Imagem de exemplo que mostra gastos para um departamento com detalhes do alerta](./media/cost-mgt-alerts-monitor-usage-spending/dept-spending-selected-with-credits.png)

Ao visualizar os detalhes de um alerta ignorado, pode reativá-la se é necessária qualquer ação manual. A imagem seguinte mostra um exemplo.

![Apresentação de imagem de exemplo dispensar e reativar as opções](./media/cost-mgt-alerts-monitor-usage-spending/Dismiss-reactivate-options.png)

## <a name="see-also"></a>Consulte também

- Se já não tiver criado um orçamento ou definir condições de alerta para um orçamento, execute o [criar e gerir orçamentos](tutorial-acm-create-budgets.md) tutorial.
