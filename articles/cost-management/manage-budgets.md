---
title: Gerir orçamentos na gestão de custo do Azure | Microsoft Docs
description: Este artigo ajuda-o a criar e gerir orçamentos na gestão de custo.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/25/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 9d6bf29909393846ec17a1bcc210fb989efd7f99
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938439"
---
# <a name="manage-budgets"></a>Gerir orçamentos

Definição de cópia de segurança orçamentos orçamento base e alerta ajuda a melhorar a governação de nuvem e accountability. Este artigo ajuda-o a criar orçamentos e começar a geri-los na gestão de custo rapidamente.

Quando tiver uma conta de empresa ou \\<ServerName>\SMS_<SITECODE>\HOTFIX\<KB, pode utilizar a estrutura de entidade de custo hierárquica atribuir mensal quotas de orçamento unidades empresariais diferentes, departamentos ou qualquer outra entidade de custo. Quando tiver uma conta de Premium, pode utilizar a funcionalidade de gestão de atribuição, o que é então aplicada aos seus despesas integralmente na nuvem. Todos os orçamentos são atribuídos manualmente.

Com base na orçamentos atribuídos, pode definir alertas de limiar com base em percentagem do seu orçamento é consumido e definir a gravidade de cada limite.

Relatórios de orçamento mostram a atribuição de atribuído. Os utilizadores podem ver quando os seus gastos através de, em ou no par com o respetivo consumo ao longo do tempo. Quando seleciona **campos de Mostrar/ocultar** na parte superior de um relatório de orçamento, pode ver o custo, orçamento, custo acumulado ou atribuição total.

## <a name="create-budgets"></a>Criar orçamentos

Quando cria um orçamento, pode defini-la para o ano fiscal e aplica-se para uma entidade específica.

Para criar um orçamento e atribua-a uma entidade:

1. Navegue para **custos** &gt; **a gestão de custos** &gt; **orçamento**.
2. Na página Gestão de atribuição, sob **entidades**, selecione a entidade onde pretende criar a atribuição.
3. No ano orçamento, selecione o ano em que pretende criar a atribuição.
4. Para cada mês, defina um valor de atribuição. Quando tiver terminado, clique em **guardar**.
Neste exemplo, a atribuição mensal de Junho de 2018 está definida para $135,000. A atribuição total do ano é $1,615,000.00.
![Criar uma atribuição](./media/manage-budgets/set-budget.png)


Para importar um ficheiro para a atribuição de anual:

1. Em **ações**, selecione **exportar** para transferir um modelo CSV em branco para utilizar como a base para a atribuição.
2. Preencha o ficheiro CSV com as entradas de orçamento e guarde-o localmente.
3. Em **ações**, selecione **importação**.
4. Selecione o ficheiro guardado e, em seguida, clique em **OK**.

Para exportar em seu orçamento concluído como um ficheiro CSV, **ações**, selecione **exportar** para transferir o ficheiro.

## <a name="view-budget-in-reports"></a>Atribuição de vista nos relatórios

Quando concluído, o seu orçamento é apresentado na maioria dos relatórios de custo em **custos** &gt; **análise de custos** e no vs custo. Relatório de orçamento ao longo do tempo. Também pode agendar relatórios com base nos limiares de orçamento utilizando **ações**.

Eis um exemplo do relatório de análise de custos. Mostra o orçamento total e o custo por tipos de utilização e carga de trabalho desde o início do ano.

![Relatório de análise de custos de exemplo com atribuição](./media/manage-budgets/cost-analysis-budget-example.png)

Neste exemplo, suponha a data atual for 22 de Junho. O custo de Junho de 2018 é $71,611.28 em comparação com a atribuição de $135,000 mensal. O custo é muito inferior a atribuição de mensal porque ainda existem oito dias dos gastos antes do fim do mês.

Outra forma de ver o relatório está a observar custo acumulado vs seu orçamento. Para ver os custos de acumulado em **campos de Mostrar/ocultar**, selecione **custo acumulado** e **orçamento Total**. Eis um exemplo que mostra o custo acumulado desde o início do ano.

![Acumulado orçamento](./media/manage-budgets/accumulated-budget.png)

Algum tempo no futuro do custo acumulado poderá exceder os seu orçamento. Mais facilmente pode ver que se alterar a vista do gráfico para o _linha_ tipo.

![Atribuição de apresentado no gráfico de linhas](./media/manage-budgets/budget-line.png)

## <a name="create-budget-alerts-for-a-filter"></a>Criar alertas de orçamento para um filtro

No exemplo anterior, pode ver que o custo acumulado approached a atribuição. Pode criar alertas de atribuição automática de modo a que está a notificado quando gastos abordagens ou excede o seu orçamento. Basicamente, o alerta é um relatório agendado com um limiar. Métricas de limiar de alerta de orçamento incluem:

- Custo restantes vs orçamento – para especificar um limiar de valor de moeda
- Percentagem de custo vs orçamento – para especificar um limiar de valor de percentagem

Vamos ver um exemplo.

No vs custo. Atribuição ao longo do relatório de tempo, clique em **ações** e, em seguida, selecione **agendar relatórios**. No separador do limiar, selecione uma métrica de limiar. Por exemplo, **orçamento de vs de percentagem de custo**. Selecione um tipo de alerta e introduza um valor de percentagem da atribuição. Se pretender ser notificado apenas uma vez, selecione **número de alertas consecutivos** e, em seguida, escreva _1_. Clique em **Guardar**.

![Alerta de atribuição](./media/manage-budgets/budget-alert.png)

## <a name="next-steps"></a>Passos Seguintes

- Se ainda não tiver concluído o primeiro tutorial para a gestão de custo, leia-lo no [rever os custos de utilização e](https://docs.microsoft.com/en-us/azure/cost-management/tutorial-review-usage).
- Saiba mais sobre o [relatórios disponíveis no custo gestão](use-reports.md).
