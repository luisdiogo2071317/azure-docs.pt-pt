---
title: Dashboards e navegação no Azure Application Insights | Documentos da Microsoft
description: Crie vistas de seus gráficos-chave APM e consultas.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 39b0701b-2fec-4683-842a-8a19424f67bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 8161fda80b5fa498f9321371c9ad2c8a2d97441a
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962424"
---
# <a name="navigation-and-dashboards-in-the-application-insights-portal"></a>Navegação e Dashboards no portal do Application Insights
Depois de ter [configurar o Application Insights no seu projeto](app-insights-overview.md), dados de telemetria sobre o desempenho e a utilização da sua aplicação serão apresentado no recurso do Application Insights do seu projeto no [portal do Azure](https://portal.azure.com).

## <a name="find-your-telemetry"></a>Encontrar a telemetria
Inicie sessão para o [portal do Azure](https://portal.azure.com) e navegue para o recurso do Application Insights que criou para a sua aplicação.

![Clique em Procurar, selecione o Application Insights, em seguida, a aplicação.](./media/app-insights-dashboards/00-start.png)

O painel de descrição geral (página) para a sua aplicação mostra um resumo das métricas de diagnóstico de chave da sua aplicação e é um gateway para outros recursos do portal.

![Principais rotas para ver a telemetria](./media/app-insights-dashboards/010-oview.png)

Pode personalizar qualquer um dos gráficos e grades e afixá-los a um dashboard. Dessa forma, pode reunir a telemetria de chave a partir de diferentes aplicações num central dashboard.

## <a name="dashboards"></a>Dashboards
A primeira coisa que vê depois de iniciar sessão para o [portal do Microsoft Azure](https://portal.azure.com) é um dashboard. Aqui, pode reunir os gráficos que são mais importantes para si por todos os recursos do Azure, incluindo telemetria a partir [do Azure Application Insights](app-insights-overview.md).

![Um dashboard personalizado.](./media/app-insights-dashboards/31.png)

1. **Navegue para recursos específicos** , tais como a sua aplicação no Application Insights: utilizar a barra à esquerda.
2. **Voltar ao dashboard atual**, ou mudar a outras vistas recentes: Utilize o menu de lista pendente na parte superior esquerda.
3. **Alternar dashboards**: Utilize o menu de lista pendente o título do dashboard
4. **Criar, editar e partilhar dashboards** na barra de ferramentas da dashboard.
5. **Editar dashboard**: coloque o cursor sobre um mosaico e, em seguida, utilize a barra superior para mover, personalizar ou removê-lo.

## <a name="add-to-a-dashboard"></a>Adicionar a um dashboard
Quando estiver olhando um painel ou conjunto de gráficos, que é particularmente interessante, pode afixar uma cópia do mesmo ao dashboard. Verá sua próxima vez em que retornar lá.

![Para afixar um gráfico, coloque o cursor sobre ela e, em seguida, clique em "..." no cabeçalho.](./media/app-insights-dashboards/33.png)

1. Afixar gráfico ao dashboard. Uma cópia do gráfico é apresentado no dashboard.
2. Afixar todo o painel ao dashboard, é apresentado no dashboard como um mosaico que pode clicar sucessivamente.
3. Clique em canto superior esquerdo para voltar ao dashboard atual. Em seguida, pode utilizar o menu pendente para regressar à vista atual.

Tenha em atenção que os gráficos são agrupados em mosaicos: um mosaico pode conter mais de um gráfico. Afixar o mosaico inteiro ao dashboard.

O gráfico é atualizado automaticamente com uma frequência que depende do intervalo de tempo do gráfico:

* Intervalo de tempo até uma hora: atualizar a cada 5 minutos
* Intervalo de tempo de 1 a 24 horas: atualizar a cada 15 minutos
* Intervalo superior a 24 horas de tempo: (intervalo de tempo) / 60.

### <a name="pin-any-query-in-analytics"></a>Afixar qualquer consulta do Analytics
Também pode [afixar Analytics](../log-analytics/query-language/get-started-analytics-portal.md) gráficos para um [partilhado](#share-dashboards-with-your-team) dashboard. Isto permite-lhe a adição de gráficos de qualquer consulta arbitrária juntamente com as métricas standard. 

Os resultados são automaticamente recalculados a cada hora. Clique no ícone de atualização no gráfico para recalcular imediatamente. (Atualizar o navegador não recalcular.)

## <a name="adjust-a-tile-on-the-dashboard"></a>Ajustar um mosaico no dashboard
Depois de um mosaico no dashboard, é possível ajustar isso.

![Coloque o cursor sobre um gráfico para editá-lo.](./media/app-insights-dashboards/36.png)

1. Adicione um gráfico ao mosaico.
2. Defina a métrica, a dimensão de grupo e o estilo (tabela, graph) de um gráfico.
3. Arraste entre o diagrama para aplicar zoom no; Clique no botão de desfazer para repor o período de tempo; definir propriedades de filtro para os gráficos no mosaico.
4. Definir o título do mosaico.

Mosaicos afixados a partir de painéis de Explorador de métricas têm mais opções de edição que mosaicos afixados a partir de um painel de descrição geral.

O mosaico original que tenha afixado não é afetado pelas edições.

## <a name="switch-between-dashboards"></a>Alternar entre dashboards
Pode guardar a mais do que um dashboard e alternar entre elas. Quando afixar um gráfico ou um painel, estes serão adicionados ao dashboard atual.

![Para alternar entre dashboards, clique o Dashboard e selecione um dashboard guardado. Para criar e guardar um novo dashboard, clique em novo. Reorganizar, clique em Editar.](./media/app-insights-dashboards/32.png)

Por exemplo, pode ter um dashboard para apresentar o ecrã inteiro no ambiente de equipe e outro para o desenvolvimento geral.

No dashboard, é apresentado um painel como um mosaico: clique nele para aceder ao painel. Um gráfico replica o gráfico na localização original.

![Clique num mosaico para abrir o painel representa](./media/app-insights-dashboards/35.png)

## <a name="share-dashboards"></a>Partilhar dashboards
Depois de criar um dashboard, pode partilhá-lo com outros utilizadores.

![No cabeçalho de dashboard, clique em partilha](./media/app-insights-dashboards/41.png)

Saiba mais sobre [funções e controlo de acesso](app-insights-resources-roles-access-control.md).

## <a name="create-dashboards-programmatically"></a>Criar dashboards programaticamente
Pode automatizar a criação de dashboard usando [do Azure Resource Manager](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards-create-programmatically) e um simple editor de JSON.

## <a name="app-navigation"></a>Navegação de aplicação
O painel de descrição geral é o gateway para obter mais informações sobre a sua aplicação.

* **Qualquer gráfico ou mosaico** - clique em qualquer mosaico ou do gráfico para ver mais detalhes sobre o que é apresentado.

### <a name="overview-blade-buttons"></a>Botões do painel de descrição geral
![Barra de navegação superior do painel de descrição geral](./media/app-insights-dashboards/app-overview-top-nav.png)

* [**Explorador de métricas** ](app-insights-metrics-explorer.md) -criar seus próprios gráficos de desempenho e utilização.
* [**Pesquisa** ](app-insights-diagnostic-search.md) - investigar instâncias específicas de eventos, tais como pedidos, exceções, ou rastreios de registo.
* [**Análise** ](app-insights-analytics.md) -consultas eficientes através de sua telemetria.
* **Intervalo de tempo** -ajustar a amplitude exibida por todos os gráficos no painel.
* **Eliminar** -eliminar o recurso do Application Insights para esta aplicação. Deve também remova os pacotes do Application Insights a partir do código de aplicação, ou editar a [chave de instrumentação](app-insights-create-new-resource.md#copy-the-instrumentation-key) na sua aplicação para direcionar a telemetria para um recurso do Application Insights diferente.

### <a name="essentials-tab"></a>Separador Essentials
* [Chave de instrumentação](app-insights-create-new-resource.md#copy-the-instrumentation-key) -identifica o recurso de aplicação.

### <a name="app-navigation-bar"></a>Barra de navegação de aplicação
![Barra de navegação esquerda](./media/app-insights-dashboards/app-left-nav-bar.png)

* **Descrição geral** -regressar ao painel de descrição geral da aplicação.
* **Registo de atividades** -alertas e eventos administrativos do Azure.
* [**Controlo de acesso** ](app-insights-resources-roles-access-control.md) -fornecem acesso aos membros da Equipe e outras pessoas.
* [**As etiquetas** ](../azure-resource-manager/resource-group-using-tags.md) -utilizar etiquetas para agrupar a sua aplicação com outras pessoas.

INVESTIGAR

* [**Mapa da aplicação** ](app-insights-app-map.md) -Active Directory mapa que mostra os componentes do seu aplicativo, derivada das informações de dependência.
* [**Deteção inteligente** ](app-insights-proactive-diagnostics.md) -rever os alertas de desempenho recentes.
* [**Live Stream** ](app-insights-live-stream.md) – um fixo de conjunto de métricas de quase imediata, útil ao implementar uma nova compilação ou a depurar.
* [**Disponibilidade / testes Web** ](app-insights-monitor-web-app-availability.md) -enviar pedidos regulares para a aplicação web a partir em todo o world.*
* [**Falhas de desempenho** ](app-insights-web-monitor-performance.md) -exceções, taxas de falhas e tempos de resposta para pedidos à sua aplicação e para pedidos a partir da sua aplicação para [dependências](app-insights-asp-net-dependencies.md).
* [**Desempenho** ](app-insights-web-monitor-performance.md) -tempo de resposta, os tempos de resposta de dependência.
* [Servidores](app-insights-web-monitor-performance.md) -contadores de desempenho. Disponível se [instalar o Monitor de estado](app-insights-monitor-performance-live-website-now.md).
* **Browser** -página vista e o desempenho de AJAX. Disponível se [instrumentar suas páginas da web](app-insights-javascript.md).
* **Utilização** -contagens de sessões, utilizadores e visualizações de página. Disponível se [instrumentar suas páginas da web](app-insights-javascript.md).

CONFIGURAR

* **Introdução ao** -inline tutorial.
* **Propriedades** -chave de instrumentação, a subscrição e a id de recurso.
* [Alertas](app-insights-alerts.md) -configuração de alerta de métrica.
* [A exportação contínua](app-insights-export-telemetry.md) -configurar a exportação de telemetria para o armazenamento do Azure.
* [Testes de desempenho](app-insights-monitor-web-app-availability.md#performance-tests) -configurar uma carga sintética no seu Web site.
* [Preços e quota](app-insights-pricing.md) e [amostragem de ingestão](app-insights-sampling.md).
* **Acesso à API** -crie [anotações da versão](app-insights-annotations.md) e para a API de acesso de dados.
* [**Itens de trabalho** ](app-insights-diagnostic-search.md#create-work-item) -ligar a um sistema de controlo, para que possa criar bugs ao inspecionar o telemetria de trabalho.

DEFINIÇÕES

* [**Bloqueia** ](../azure-resource-manager/resource-group-lock-resources.md) -bloquear recursos do Azure
* [**Script de automação** ](app-insights-powershell.md) -exportar uma definição do recurso do Azure, para que possa utilizar como um modelo para criar novos recursos.


## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Passos Seguintes

|  |  |
| --- | --- |
| [Explorador de métricas](app-insights-metrics-explorer.md)<br/>Métricas de filtrar e segmentar |![Exemplo de pesquisa](./media/app-insights-dashboards/64.png) |
| [Pesquisa de diagnóstico](app-insights-diagnostic-search.md)<br/>Localizar e Inspecione os eventos, eventos relacionados e criar bugs |![Exemplo de pesquisa](./media/app-insights-dashboards/61.png) |
| [Análise](app-insights-analytics.md)<br/>Poderosa linguagem de consultas |![Exemplo de pesquisa](./media/app-insights-dashboards/63.png) |
