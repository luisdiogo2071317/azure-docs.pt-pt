---
title: Tutorial - criar e gerir orçamentos do Azure | Documentos da Microsoft
description: Este tutorial ajuda plano e uma conta para os custos de serviços do Azure que consumir.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 6c6143cad04178fcafc825d9dae13c1a0620fb93
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47033452"
---
# <a name="tutorial-create-and-manage-azure-budgets"></a>Tutorial: Criar e gerir orçamentos do Azure

Orçamentos no Cost Management ajudá-lo a planejar e responsabilidade de unidade organizacional. Com orçamentos, podem contribuir para os serviços do Azure se consume ou se subscreve durante um período específico. Ajudam a informar outras pessoas sobre seus gastos para gerenciar de forma proativa os custos e para monitorizar a forma como gastos avança ao longo do tempo. Pode ver como gastos avança ao longo do tempo. Quando os limiares de orçamento que criou são excedidos, apenas notificações são acionadas. Nenhum dos seus recursos são afetados e seu consumo não está parado. Pode usar os orçamentos de comparar e controlar a analisar os custos de gastos.

Orçamentos repor automaticamente no final de um período (mensalmente, trimestralmente ou anualmente) para a mesma quantidade de orçamento ao selecionar uma data de expiração no futuro. Porque repostas com a mesma quantidade de orçamento, tem de criar orçamentos separados quando orçada diferem de valores de moeda para períodos futuros.

Os exemplos neste tutorial orientam-criação e edição de um orçamento para uma subscrição do Azure Enterprise Agreement (EA).

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Criar um orçamento no portal do Azure
> * Editar um orçamento

## <a name="prerequisites"></a>Pré-requisitos

Orçamentos estão disponíveis para todos os clientes do EA do Azure. Tem de ter acesso de leitura para uma subscrição do Azure EA para criar e gerir orçamentos. Contas de faturas de EA não são suportadas por orçamentos.

Orçamentos são criados na subscrição ou ao nível do grupo de recursos, individualmente. As seguintes permissões do Azure são suportadas por subscrição para os orçamentos de por utilizador e grupo:

- Proprietário – pode criar, modificar ou eliminar orçamentos para uma subscrição.
- Contribuinte – pode criar, modificar ou eliminar os seus próprios orçamentos. Pode modificar a quantidade de orçamento para orçamentos criados por outros utilizadores.
- Leitor-pode ver os orçamentos de que eles têm permissão para.

## <a name="sign-in-to-azure"></a>Iniciar sessão no Azure

- Inicie sessão no portal do Azure em http://portal.azure.com.

## <a name="create-a-budget-in-the-azure-portal"></a>Criar um orçamento no portal do Azure

Pode criar um orçamento de subscrição do Azure durante um período mensal, trimestral ou anual. O conteúdo de navegação no portal do Azure determina se criar um orçamento para uma subscrição ou para um grupo de recursos.

No portal do Azure, navegue até **gestão de custos + faturação** &gt; **subscrições** &gt; selecionar uma subscrição &gt; **orçamentos**. Neste exemplo, é o orçamento que criar para a subscrição que selecionou.

Depois de criar orçamentos, mostram uma simples exibição de seus gastos atual contra eles.

Clique em **Adicionar**.

![Orçamentos de gestão de custos](./media/tutorial-acm-create-budgets/budgets01.png)

Na **orçamento criar** janela, introduza um nome de orçamento e a quantidade de orçamento. Em seguida, escolha um mensais, trimestrais, ou o período de duração anual. Em seguida, selecione uma data de fim. Orçamentos requerem o limiar, pelo menos, um custo (% do orçamento) e um endereço de e-mail correspondentes. Opcionalmente, pode incluir até cinco limiares e cinco endereços de e-mail num orçamento único.

Eis um exemplo de criação de um orçamento mensal de US $4.500. É gerado um alerta de e-mail quando for atingido 90% do orçamento.

![Exemplo de orçamento mensal](./media/tutorial-acm-create-budgets/monthly-budget01.png)

Quando cria um orçamento trimestral, ele funciona da mesma forma como um orçamento mensal. A diferença é que a quantidade de orçamento para o trimestre é uniformemente dividida entre os três meses do trimestre. Como pode esperar, uma quantidade de orçamento anual é uniformemente dividida entre todos os 12 meses do ano de calendário.

Gastos atuais face aos orçamentos é atualizada sempre que a gestão de custos recebe dados de faturas atualizados. Normalmente, diariamente.

![Gastos face aos orçamentos de atual](./media/tutorial-acm-create-budgets/budgets-current-spending.png)

Depois de criar um orçamento, ela é mostrada na análise de custos. Visualizar o seu orçamento em relação à tendência de gastos é uma das primeiras etapas quando começar a [analisar os custos e despesas](quick-acm-cost-analysis.md).

![Orçamento mostrado na análise de custos](./media/tutorial-acm-create-budgets/cost-analysis.png)

No exemplo anterior, criou um orçamento para uma subscrição. No entanto, também pode criar um orçamento para um grupo de recursos. Se quiser criar um orçamento para um grupo de recursos, navegue para **gestão de custos + faturação** &gt; **subscrições** &gt; selecionar uma subscrição > **recursos grupos** > selecione um grupo de recursos > **orçamentos** > e, em seguida, **Add** um orçamento.

## <a name="edit-a-budget"></a>Editar um orçamento

Dependendo do nível de acesso que tem, pode editar um orçamento para alterar as respetivas propriedades. No exemplo a seguir, algumas das propriedades são só de leitura porque o utilizador tem permissão de contribuinte à subscrição. Atualmente, o **data de expiração** está desativada e não pode ser modificado uma vez definida.

![Editar o orçamento – permissão de contribuinte](./media/tutorial-acm-create-budgets/edit-budget.png)


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Criar um orçamento no portal do Azure
> * Editar um orçamento

Avance para o próximo tutorial para criar uma exportação recorrente para os seus dados de gestão de custos.

> [!div class="nextstepaction"]
> [Criar e gerir os dados exportados](tutorial-export-acm-data.md)
