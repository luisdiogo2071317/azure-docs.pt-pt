---
title: Explore os custos do Azure com a análise de custo | Documentos da Microsoft
description: Este artigo ajuda-o a utilizar a análise de custos para explorar e analisar os seus custos organizacionais do Azure.
services: billing
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 08/16/2018
ms.topic: conceptual
ms.service: billing
manager: dougeby
ms.custom: ''
ms.openlocfilehash: db06956e01ed38ce1f8ee0ce92526be16733fd73
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42818129"
---
# <a name="explore-and-analyze-costs-with-cost-analysis"></a>Explore e analise os custos com a análise de custos

Antes de poder controlar corretamente e otimizar os custos do Azure, precisa entender em que os custos teve origem dentro da sua organização. Também é útil saber quanto dinheiro seus serviços de custos e para oferecer suporte a quais ambientes e sistemas. Visibilidade sobre a gama completa de custos é fundamental compreender com precisão os padrões de gastos organizacionais, o que podem ser utilizados para impor a mecanismos de controle de custos, como orçamentos.

Neste artigo, vai utilizar análise de custos para explorar e analisar os custos da sua organização. Ver os custos agregados pela sua organização para compreender onde os custos são acumulados e identificar tendências de gastos. Ver os custos acumulados ao longo do tempo para fazer uma estimativa mensal, trimestral ou anualmente, mesmo as tendências em relação a um orçamento de custos. Um orçamento ajuda a cumprir as restrições financeiras ou ver os custos de diários ou mensais para isolar irregularidades gastos. E, pode transferir dados do relatório atual para análise ou utilizar num sistema externo.

## <a name="requirements"></a>Requisitos

Análise de custos estão disponível para todos os clientes do Enterprise Agreement (EA). Tem de ter acesso de leitura para, pelo menos, um dos seguintes âmbitos para ver os dados de custo.

- EA do Azure (inscrição) de conta de faturação
- Subscrição de EA do Azure
- Grupo de recursos de subscrição do Azure EA

## <a name="review-costs-in-cost-analysis"></a>Rever os custos de análise de custos

Para rever os custos com a análise de custo, abra o portal do Azure, em seguida, navegue até **gestão de custos + faturação** &gt; **contas de faturação** &gt; selecione seu EA conta defaturação&gt; sob a gestão de custos, selecione **análise de custo**.

A vista de análise de custo inicial inclui as seguintes áreas:

**Total** – mostra o custo total para o mês atual.

**Orçamento** – mostra o planeada limite de gastos para o âmbito selecionado, se disponível.

**Custo acumulado** – mostra o total acumulados gastos diários, começando do início do mês. Se tiver [criado um orçamento](billing-cost-management-budget-scenario.md#create-the-azure-budget) para a sua faturação conta ou subscrição, é possível ver rapidamente a tendência de gastos em relação ao orçamento. Coloque o cursor sobre uma data para o custo acumulado para esse dia.

**(Anel) gráficos dinâmicos** – fornecem tabelas dinâmicas dinâmicas. Elas dividem-se para baixo o custo por um conjunto comum de propriedades padrão total. Os gráficos mostram o máximo para o menor custo acumulado para o mês atual. Pode alterar os gráficos dinâmicos em qualquer altura, selecionando um pivô diferente. Por predefinição, os custos são divididos por serviço (categoria do medidor), localização (região) e âmbito subordinado (por exemplo, contas de inscrição de contas de faturação, grupos de recursos nas subscrições e recursos em grupos de recursos).

![Vista inicial de análise de custos](./media/billing-cost-analysis/cost-analysis-01.png)



## <a name="customizing-cost-views"></a>Personalizar vistas de custo

A exibição padrão fornece respostas rápidas a perguntas comuns, como:

- Quanto passo?
- Irá permanecer dentro do meu orçamento?

No entanto, há muitos casos em que precisa que uma análise mais aprofundada. Personalização é iniciado na parte superior da página, com a seleção de data.

Por predefinição, análise de custos mostra os dados para o mês atual. Utilize o Seletor de datas para alternar rapidamente para o último mês, este mês, este trimestre de calendário, este ano de calendário ou um intervalo de datas personalizadas à sua escolha. Selecionar último mês é a forma mais rápida para analisar a sua fatura do Azure mais recente e facilmente reconciliar encargos. As opções de trimestre e ano atuais ajudam a controlar os custos face aos orçamentos de longo prazo. Em alternativa, pode selecionar um intervalo de datas mais refinadas ou mais abrangente, a partir de um único dia para os últimos sete dias ou qualquer coisa, até um ano antes do mês atual.

![Seletor de data](./media/billing-cost-analysis/date-selector.png)

Também por predefinição, de custos mostra analysis **acumulado** os custos. Os custos acumulados incluem todos os custos para cada dia, além de dias anteriores, para uma visão constantemente em expansão dos seus custos acumulados, diárias. Esta vista está otimizada para mostrar como tendências em relação a um orçamento para o intervalo de tempo selecionado.

Também há a **diária** vista. Ele mostra os custos para cada dia e não mostra uma tendência de crescimento. A vista diária é otimizada para mostrar o irregularidades como pico de custos ou dip do dia a dia. Quando seleciona um orçamento, a vista diária também mostra uma estimativa de seu orçamento diário possível aparência. Se os seus custos diários são consistentemente acima do orçamento de diário estimado, em seguida, pode esperar que vai exceder seu orçamento mensal. O orçamento de diário estimado é simplesmente um meio para ajudar a visualizar o seu orçamento num nível inferior. Quando tiver flutuações de custos diários, em seguida, a comparação de orçamento diário estimado para o seu orçamento mensal é menos precisa.

![Vista de diária](./media/billing-cost-analysis/daily-view.png)

Pode **Agrupar por** para selecionar uma categoria de grupo para alterar os dados apresentados no gráfico de área total superior. Agrupamento permite-lhe ver rapidamente como seus gastos foi categorizado pelo tipo de recurso. Eis uma vista dos custos de serviço do Azure para uma visão do mês passado.

![Vista de acumulado diária agrupada](./media/billing-cost-analysis/grouped-daily-accum-view.png)

Gráficos dinâmicos estão sob a vista Total superior. Utilize-os para ver as vistas de diferentes de agrupamento e filtragem de categorias. Ao selecionar qualquer categoria de grupo, é o conjunto completo de dados para a vista total na parte inferior da exibição. Eis um exemplo para grupos de recursos.

![Dados completa para a vista atual](./media/billing-cost-analysis/full-data-set.png)

### <a name="download-cost-analysis-data"></a>Transferir dados de análise de custos

Quando **transferir** informações de análise de custo, um ficheiro CSV é gerado para todos os dados atualmente apresentados no portal do Azure. Se tiver aplicado filtros ou de agrupamento, em seguida, qual estão incluídos no ficheiro. Alguns dados subjacentes para o gráfico de Total superior ativamente não for apresentado no portal do também estão incluídos no ficheiro CSV.

## <a name="next-steps"></a>Passos Seguintes

Ver outros [a faturação do Azure e a documentação de gestão de custos](billing-cost-management-budget-scenario.md).
