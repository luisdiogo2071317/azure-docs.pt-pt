---
title: Recolher e analisar registos de atividade do Azure no Log Analytics | Microsoft Docs
description: Pode utilizar a solução de registos de atividade do Azure para analisar e procurar o registo de atividade do Azure nas suas subscrições do Azure.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 0b05dc17fc7ba567bf633c25a080fbf56903935c
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130331"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics"></a>Recolher e analisar registos de atividade do Azure na análise de registos

![Símbolo de registos de atividade do Azure](./media/log-analytics-activity/activity-log-analytics.png)

A solução de análise de registos de atividade ajuda a analisar e procurar o [registo de atividade do Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) nas suas subscrições do Azure. O registo de atividade do Azure é um registo que disponibiliza informações aprofundadas operações executadas nos recursos nas suas subscrições. O registo de atividade era anteriormente conhecido como *registos de auditoria* ou *registos operacionais* , uma vez que os relatórios de eventos para as suas subscrições.

Utilizar o registo de atividade, poderá determinar o *que*, *quem*, e *quando* para quaisquer operações (PUT, POST, DELETE) efetuadas para os recursos na sua subscrição de escrita. Também pode compreender o estado das operações e outras propriedades relevantes. O registo de atividade inclui as operações de leitura (GET) ou as operações para recursos que utilizam o modelo de implementação clássica.

Quando os registos de atividade do Azure se liga à análise de registos, pode:

- Analise os registos de atividade com vistas predefinidos
- Analisar e registos de pesquisa e a atividade de várias subscrições do Azure
- Mantenha os registos de atividade para mais de 90 dias<sup>1</sup>
- Correlacionar os registos de atividade com outro Azure plataforma e dados de aplicações
- Ver as atividades operacionais agregadas pelo Estado
- Ver tendências de atividades a acontecer em cada um dos seus serviços do Azure
- Relatório sobre as alterações de autorização em todos os seus recursos do Azure
- Identificar problemas de estado de funcionamento de serviço ou falha afetar os recursos
- Utilize a procura de registo para correlacionar as atividades dos utilizadores, as operações de dimensionamento automático, as alterações de autorização e funcionamento de serviço para as métricas ou outros registos do seu ambiente

<sup>1</sup>por predefinição, análise de registos mantém os seus registos de atividade do Azure para 90 dias, mesmo que estejam no escalão gratuito. Em alternativa, se tiver uma definição de retenção de área de trabalho de inferior a 90 dias. Se a sua área de trabalho tiver retenção é maior do que 90 dias, os registos de atividade são mantidos com base no período de retenção da sua área de trabalho.

Análise de registos recolhe registos de atividade gratuitas e armazena os registos de 90 dias gratuitamente. Se armazenar os registos para mais de 90 dias, será cobrado custo de retenção de dados para os dados armazenados já mais do que 90 dias.

Quando estiver no escalão de preço gratuito, registos de atividade não se aplicam ao seu consumo diário de dados.

## <a name="connected-sources"></a>Origens ligadas

Ao contrário da maioria das outras soluções de análise de registos, os dados não estão recolhidos para registos de atividade por agentes. Todos os dados utilizados pela solução inclui diretamente a partir do Azure.

| Origem Ligada | Suportadas | Descrição |
| --- | --- | --- |
| [Agentes do Windows](log-analytics-windows-agent.md) | Não | A solução não recolhe informações de agentes do Windows. |
| [Agentes do Linux](log-analytics-linux-agents.md) | Não | A solução não recolhe informações de agentes Linux. |
| [Grupo de gestão do SCOM](log-analytics-om-agents.md) | Não | A solução não recolhe informações de agentes num grupo de gestão do SCOM ligado. |
| [Conta de armazenamento do Azure](log-analytics-azure-storage.md) | Não | A solução não recolhe informações do armazenamento do Azure. |

## <a name="prerequisites"></a>Pré-requisitos

- Para aceder às informações de registo de atividade do Azure, tem de ter uma subscrição do Azure.

## <a name="configuration"></a>Configuração

Execute os seguintes passos para configurar a solução de análise de registos de atividade para as áreas de trabalho.

1. Ative a solução Análise do Registo de Atividades do [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) ou com o processo descrito em [Adicionar soluções do Log Analytics da Galeria de Soluções](log-analytics-add-solutions.md).
2. Configure os registos de atividade para aceder à sua área de trabalho de análise de registos.
    1. No portal do Azure, selecione a sua área de trabalho e, em seguida, clique em **registo de atividade do Azure**.
    2. Para cada subscrição, clique no nome da subscrição.  
        ![Adicionar subscrição](./media/log-analytics-activity/add-subscription.png)
    3. No *SubscriptionName* painel, clique em **Connect**.  
        ![Ligar a subscrição](./media/log-analytics-activity/subscription-connect.png)

Se adicionar a solução através do portal do OMS, verá o mosaico seguinte. Inicie sessão no portal do Azure para ligar uma subscrição do Azure a sua área de trabalho.  
![efetuar a avaliação](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-the-solution"></a>Utilizar a solução

Quando adiciona a solução de análise de registos de atividade a sua área de trabalho, o **registos de atividade do Azure** mosaico é adicionado ao seu dashboard da descrição geral. Este mosaico mostra uma contagem do número de registos de actividades do Azure para as subscrições do Azure com a solução de acesso.

![Mosaico de registos de atividade do Azure](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Regista a atividade do Azure de vista

Clique em de **registos de atividade do Azure** mosaico para abrir o **registos de atividade do Azure** dashboard. O dashboard inclui os painéis na tabela seguinte. Cada painel apresenta uma lista com um máximo de 10 itens que correspondem aos critérios do âmbito e do intervalo de tempo especificados. Pode executar uma pesquisa de registos que devolve todos os registos ao clicar em **Ver tudo** na parte inferior do painel ou ao clicar no cabeçalho do painel.

Dados de registo de atividade só é apresentada *depois* que configurou os registos de atividade para ir para a solução de, pelo que não é possível ver os dados antes.

| Painel | Descrição |
| --- | --- |
| Entradas de registo de atividade do Azure | Mostra um gráfico de barras da parte superior entrada de registo de atividade do Azure totais de registos para o intervalo de datas que tiver selecionado e mostra uma lista dos de chamadores de 10 atividade principais. Clique em executar uma pesquisa de registo para o gráfico de barras <code>AzureActivity</code>. Clique num item de autor da chamada para executar uma pesquisa de registo devolver todas as entradas de registo de atividade para esse item. |
| Registos de atividade por Estado | Mostra um gráfico de anel para o estado de registo de atividade do Azure para o intervalo de datas que selecionou. Também mostra uma lista uma lista de registos de dez Estado principais. Clique em gráfico para executar uma pesquisa de registo para <code>AzureActivity &#124; summarize AggregatedValue = count() by ActivityStatus</code>. Clique num item de estado para executar uma pesquisa de registo devolver todas as entradas de registo de atividade para esse registo de estado. |
| Registos de atividade por recurso | Mostra o número total de recursos com registos de atividade e apresenta uma lista de principais dez recursos com registo de contagens de cada recurso. Clique na área total para executar uma pesquisa de registo para <code>AzureActivity &#124; summarize AggregatedValue = count() by Resource</code>, que mostra todos os recursos do Azure disponíveis para a solução. Clique num recurso para executar uma pesquisa de registo devolver todos os registos de atividade para esse recurso. |
| Registos de atividade pelo fornecedor de recursos | Mostra o número total de fornecedores de recursos que produzem a atividade de registo e lista as dez principais. Clique na área total para executar uma pesquisa de registo para <code>AzureActivity &#124; summarize AggregatedValue = count() by ResourceProvider</code>, que mostra todos os fornecedores de recursos do Azure. Clique num fornecedor de recursos para executar uma pesquisa de registo devolver todos os registos de atividade do fornecedor. |

![Dashboard de registos de atividade do Azure](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Passos Seguintes

- Criar um [alerta](log-analytics-alerts-creating.md) quando ocorre uma atividade específica.
- Utilize [pesquisa registo](log-analytics-log-searches.md) para ver informações detalhadas dos registos de atividade.
