---
title: Tutorial – Previsão de despesas com a Cloudyn no Azure | Microsoft Docs
description: Neste tutorial, saiba como prever gastos através do histórico de dados de utilização e gastos.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/18/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: ''
manager: dougeby
ms.openlocfilehash: e1954ae9fcbb010c66e8a812e3e34477ac2bfc29
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49395229"
---
# <a name="tutorial-forecast-future-spending"></a>Tutorial: prever despesas futuras

A Cloudyn ajuda a prever despesas futuras, através do histórico dos dados de utilização e de despesas. Utilize relatórios do Cloudyn para ver todos os dados de projeção de custo. Os exemplos neste tutorial explicam como rever projeções de custo através dos relatórios. Neste tutorial, ficará a saber como:

> [!div class="checklist"]
> * Prever despesas futuras

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

- Tem de ter uma conta do Azure.
- Tem de ter um registo de avaliação ou uma subscrição paga da Cloudyn.

## <a name="forecast-future-spending"></a>Prever despesas futuras

O Cloudyn inclui relatórios de projeção de custos para ajudar a prever despesas com base na sua utilização ao longo do tempo. O seu principal objetivo é ajudar a garantir que as tendências de custo não excedem as expectativas da sua organização. Os relatórios que utiliza são do Custo Previsto do Mês Atual e do Custo Previsto Anual. Ambos mostram despesas futuras previstas se a sua utilização permanecer relativamente consistente com os últimos 30 dias de utilização.

O relatório de Custo Previsto do Mês Atual mostra os custos dos seus serviços. Utiliza os custos do início do mês e do mês anterior para mostrar o custo previsto. No menu de relatórios na parte superior do portal, clique em **Custos** > **Projeção e Orçamento** > **Custo Previsto do Mês Atual**. A imagem seguinte mostra um exemplo.

![Custo previsto do mês atual](./media/tutorial-forecast-spending/project-month01.png)

No exemplo, pode ver quais os serviços que mais gastam. Os custos do Azure foram inferiores aos custos dos AWS. Se quer ver os detalhes da previsão de custos para VMs do Azure, na lista **Filtrar**, selecione **Azure/VM**.

![Custo previsto do mês atual da VM do Azure](./media/tutorial-forecast-spending/project-month02.png)

Siga os mesmos passos básicos anteriores para ver as previsões do custo mensal para outros serviços em que está interessado.

O relatório do Custo Previsto Anual mostra o custo extrapolado dos seus serviços ao longo dos 12 meses seguintes.

No menu de relatórios na parte superior do portal, clique em **Custos** > **Projeção e Orçamento** > **Custo Previsto Anual**. A imagem seguinte mostra um exemplo.

![Relatório do custo previsto anual](./media/tutorial-forecast-spending/project-annual01.png)

No exemplo, pode ver quais os serviços que mais gastam. Semelhante ao exemplo mensal, os custos do Azure foram inferiores aos custos do AWS. Se quer ver os detalhes da previsão de custos para VMs do Azure, na lista **Filtrar**, selecione **Azure/VM**.

![Custo previsto anual das VMs](./media/tutorial-forecast-spending/project-annual02.png)

Na imagem acima, o custo previsto anual das VMs do Azure é de $ 28.374.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

> [!div class="checklist"]
> * Prever despesas futuras


Avance para o próximo tutorial para saber como gerir os custos com relatórios de alocação e análise de custos e encargos.

> [!div class="nextstepaction"]
> [Gerir os custos com relatórios de alocação e análise de custos e encargos](tutorial-manage-costs.md)
