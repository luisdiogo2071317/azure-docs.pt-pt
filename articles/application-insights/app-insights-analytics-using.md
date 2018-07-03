---
title: Utilizar a análise - a ferramenta de pesquisa poderosa do Azure Application Insights | Documentos da Microsoft
description: 'Utilizar a análise, a ferramenta de pesquisa poderosas de diagnóstico do Application Insights. '
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: c3b34430-f592-4c32-b900-e9f50ca096b3
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 07/02/2018
ms.reviewer: danha
ms.author: mbullwin
ms.openlocfilehash: aa86e2f3b1fb147ab167c948475a5207693143c2
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37341567"
---
# <a name="using-analytics-in-application-insights"></a>Utilizar a análise no Application Insights
[Análise](app-insights-analytics.md) é a funcionalidade de pesquisa avançada de [Application Insights](app-insights-overview.md). Essas páginas descrevem a linguagem de consulta do Log Analytics.

* **[Assista ao vídeo introdutório](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Faça o test drive Analytics nos nossos dados simulados](https://analytics.applicationinsights.io/demo)**  se a aplicação não está a enviar dados para o Application Insights ainda.

## <a name="open-analytics"></a>Abrir o Analytics
A partir do recurso de raiz da sua aplicação no Application Insights, clique em Analytics.

![Abrir portal.azure.com, abra o recurso do Application Insights e clique em Analytics.](./media/app-insights-analytics-using/001.png)

O tutorial inline dá-lhe algumas idéias sobre o que pode fazer.

Há uma [visita guiada mais extensiva aqui](app-insights-analytics-tour.md).

## <a name="query-your-telemetry"></a>Consulta de telemetria
### <a name="write-a-query"></a>Escrever uma consulta
![Exibição de esquema](./media/app-insights-analytics-using/150.png)

Começar com os nomes de qualquer uma das tabelas exibidas à esquerda (ou o [intervalo](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/range-operator) ou [União](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/union-operator) operadores). Uso `|` para criar um pipeline de [operadores](https://docs.loganalytics.io/docs/Learn/References/Useful-operators). 

IntelliSense pede-lhe com os operadores e os elementos de expressão que pode utilizar. Clique no ícone de informações (ou prima CTRL + espaço) para obter uma descrição mais longa e exemplos sobre como utilizar cada elemento.

Consulte a [tour de idioma do Analytics](app-insights-analytics-tour.md) e [referência de linguagem](app-insights-analytics-reference.md).

### <a name="run-a-query"></a>Executar uma consulta
![Execução de uma consulta](./media/app-insights-analytics-using/130.png)

1. Pode usar as quebras de linha única numa consulta.
2. Coloque o cursor no interior ou no fim da consulta que pretende executar.
3. Verifique o intervalo de tempo da sua consulta. (Pode alterá-la ou substituí-la, incluindo os seus próprios [ `where...timestamp...` ](https://docs.loganalytics.io/docs/Learn/Tutorials/Date-and-time-operations) cláusula na sua consulta.)
3. Clique em Ir para executar a consulta.
4. Não coloque linhas em branco na sua consulta. Pode ter várias consultas separadas num separador de consulta, separando-as com as linhas em branco. Apenas a consulta que contém o cursor é executado.

### <a name="save-a-query"></a>Guardar uma consulta
![A guardar uma consulta](./media/app-insights-analytics-using/140.png)

1. Guarde o ficheiro de consulta atual.
2. Abra um ficheiro de consulta guardada.
3. Crie um novo arquivo de consulta.

## <a name="see-the-details"></a>Consulte os detalhes
Expanda qualquer linha de resultados para ver a lista completa de propriedades. Pode expandir ainda mais qualquer propriedade que é um valor estruturado - por exemplo, as dimensões personalizadas ou a pilha de listagem numa exceção.

![Expanda uma linha](./media/app-insights-analytics-using/070.png)

## <a name="arrange-the-results"></a>Dispor os resultados
Pode ordenar, filtrar, paginar e agrupar os resultados devolvidos da sua consulta.

> [!NOTE]
> Classificação, agrupamento e filtragem no navegador não executam novamente a sua consulta. Eles apenas reorganizar os resultados que foram devolvidos pela última consulta. 
> 
> Para executar essas tarefas no servidor antes dos resultados são devolvidos, escrever a consulta com o [ordenação](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator), [resumir](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) e [onde](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) operadores.
> 
> 

Escolha as colunas que pretende ver, arrastar cabeçalhos de coluna para reorganizá-las e redimensionar colunas arrastando os limites estão.

![Dispor as colunas](./media/app-insights-analytics-using/030.png)

### <a name="sort-and-filter-items"></a>Ordenar e filtrar itens
Ordene os resultados clicando no cabeçalho de uma coluna. Clique novamente para ordenar a outra forma e clique numa terceira vez para reverter para a ordem original devolvido pela consulta.

Utilize o ícone de filtro para limitar a pesquisa.

![Ordenar e filtrar colunas](./media/app-insights-analytics-using/040.png)

### <a name="group-items"></a>Itens de grupo
Para ordenar por mais de uma coluna, utilize o agrupamento. Primeiro ativá-la e, em seguida, arrastar cabeçalhos de coluna para o espaço acima da tabela.

![Grupo](./media/app-insights-analytics-using/060.png)

### <a name="missing-some-results"></a>Falta alguns resultados?

Se acha que não esteja a ver todos os resultados esperados, há duas razões possíveis.

* **Filtro de intervalo de tempo**. Por predefinição, verá apenas os resultados das últimas 24 horas. Existe um filtro automático que limita o intervalo de resultados que são obtidos a partir de tabelas de origem. 

    No entanto, pode alterar o intervalo de tempo filtro, utilizando o menu pendente.

    Ou pode substituir o intervalo automático, incluindo os seus [ `where  ... timestamp ...` cláusula](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator) na sua consulta. Por exemplo:

    `requests | where timestamp > ago('2d')`

* **Limite de resultados**. Existe um limite de 10 mil linhas nos resultados devolvidos a partir do portal. Um aviso mostra se exceder o limite. Se isto acontecer, classificar os resultados da tabela não sempre mostra-lhe todos os primeiros ou últimos resultados reais. 

    É boa prática evitar atingir o limite. Utilize o filtro de intervalo de tempo ou utilize operadores como:

  * [100 principais por timestamp](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator) 
  * [tirar 100](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/take-operator)
  * [resumir ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) 
  * [onde timestamp > ago(3d)](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator)

(Mais de 10 mil linhas quer? Considere a utilização [exportação contínua](app-insights-export-telemetry.md) em vez disso. Analytics foi concebido para análise, em vez de ao obter dados não processados.)

## <a name="diagrams"></a>Diagramas
Selecione o tipo de diagrama, que gostaria de ter:

![Selecione um tipo de diagrama](./media/app-insights-analytics-using/230.png)

Se tiver várias colunas de tipos corretos, pode escolher o x e eixos y e uma coluna de dimensões para dividir os resultados por.

Por predefinição, os resultados são apresentados inicialmente como uma tabela e selecione o diagrama manualmente. Mas pode usar o [processar diretiva](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) no final de uma consulta para selecionar um diagrama.

### <a name="analytics-diagnostics"></a>Diagnóstico de análise


No Pego, se houver um pico ou passo nos seus dados, poderá ver um ponto de realçado na linha. Isto indica que o diagnóstico de análise identificou uma combinação de propriedades que filtrar a alteração repentina. Clique no ponto para obter mais detalhes no filtro e, para ver a versão filtrada. Isso pode ajudá-lo a identificar o que causou a alteração. 

[Saiba mais sobre o diagnóstico de análise](app-insights-analytics-diagnostics.md)


![Diagnóstico de análise](./media/app-insights-analytics-using/analytics-diagnostics.png)

## <a name="pin-to-dashboard"></a>Afixar ao dashboard
Pode afixar um diagrama ou tabela para um dos seus [dashboards partilhados](app-insights-dashboards.md) -basta clicar o pin. 

![Clique no pin](./media/app-insights-analytics-using/pin-01.png)

Isso significa que, quando monta um dashboard para ajudar a monitorizar o desempenho ou a utilização dos seus serviços web, pode incluir análises muito complexas em conjunto com as outras métricas. 

Pode afixar uma tabela ao dashboard, se ela tem quatro ou menos colunas. São apresentadas apenas as sete linhas superiores.

### <a name="dashboard-refresh"></a>Atualização do dashboard
O gráfico afixado ao dashboard é atualizado automaticamente ao executar novamente a consulta aproximadamente a cada hora. Também pode clicar no botão atualizar.

### <a name="automatic-simplifications"></a>Simplificações automática

Algumas simplificações aplicadas a um gráfico quando afixá-la a um dashboard.

**Restrição de tempo:** consultas são automaticamente limitadas para os últimos 30 dias. O efeito é o mesmo como se a sua consulta inclui `where timestamp > ago(30d)`.

**Restrição de contagem de discretização:** se exibir um gráfico que tem muitas das discretizações discretas (normalmente, um gráfico de barras), as caixas de menos preenchidas automaticamente são agrupadas num único "outros" bin. Por exemplo, esta consulta:

    requests | summarize count_search = count() by client_CountryOrRegion

fica assim no Analytics:

![Gráfico com cauda longa](./media/app-insights-analytics-using/pin-07.png)

mas quando afixá-la a um dashboard, ele fica assim:

![Gráfico com contentores limitados](./media/app-insights-analytics-using/pin-08.png)

## <a name="export-to-excel"></a>Exportar para Excel
Depois de executar uma consulta, pode transferir um ficheiro. csv. Clique em **exportar, Excel**.

## <a name="export-to-power-bi"></a>Exportar para o Power BI
Coloque o cursor numa consulta e escolha **exportar, Power BI**.

![Exportar do Analytics ao Power BI](./media/app-insights-analytics-using/240.png)

Execute a consulta no Power BI. Pode defini-lo para atualizar com base numa agenda.

Com o Power BI, pode criar dashboards que reúnem dados de uma ampla variedade de origens.

[Saiba mais sobre a exportação para o Power BI](app-insights-export-power-bi.md)

## <a name="deep-link"></a>Ligação avançada

Obtenha uma ligação sob **exportação, ligação de partilha** que pode enviar para outro utilizador. Desde que o utilizador tiver [acesso ao seu grupo de recursos](app-insights-resources-roles-access-control.md), a consulta será aberto na IU do Analytics.

(Na ligação, é apresentado o texto da consulta após "? q =", comprimidos gzip e com codificação base 64. Poderia escrever código para gerar ligações avançadas que fornece aos utilizadores. No entanto, a forma recomendada para executar a análise de código é usando o [REST API](https://dev.applicationinsights.io/).)


## <a name="automation"></a>Automatização

Utilize o [API do REST de acesso de dados](https://dev.applicationinsights.io/) para executar consultas de análise. [Por exemplo](https://dev.applicationinsights.io/apiexplorer/query?appId=DEMO_APP&apiKey=DEMO_KEY&query=requests%0A%7C%20where%20timestamp%20%3E%3D%20ago%2824h%29%0A%7C%20count) (com o PowerShell):

```PS
curl "https://api.applicationinsights.io/beta/apps/DEMO_APP/query?query=requests%7C%20where%20timestamp%20%3E%3D%20ago(24h)%7C%20count" -H "x-api-key: DEMO_KEY"
```

Ao contrário da análise de interface do Usuário, a API REST não adiciona automaticamente qualquer limitação timestamp às suas consultas. Não se esqueça de adicionar seu próprio cláusula where, para evitar que as respostas enormes.



## <a name="import-data"></a>Importar dados

Pode importar dados de um ficheiro CSV. Um uso típico consiste em importar dados estáticos, que pode aderir com tabelas a partir da sua telemetria. 

Por exemplo, se os utilizadores autenticados são identificados na sua telemetria por um alias ou o id oculto, pode importar uma tabela que mapeia aliases para nomes reais. Executar uma associação sobre a telemetria de pedido, é possível identificar os utilizadores pelos respetivos nomes real nos relatórios de análise.

### <a name="define-your-data-schema"></a>Na definição do esquema de dados

1. Clique em **configurações** (no canto superior esquerdo) e, em seguida **origens de dados**. 
2. Adicione uma origem de dados, siga as instruções. Será solicitado a fornecer uma amostra dos dados, que devem incluir, pelo menos, 10 linhas. Em seguida, corrigir o esquema.

Isso define uma origem de dados, que, em seguida, pode utilizar para importar tabelas individuais.

### <a name="import-a-table"></a>Importar uma tabela

1. Abra sua definição da origem de dados a partir da lista.
2. Clique em "Carregar" e siga as instruções para carregar a tabela. Isso envolve uma chamada para uma API REST e, portanto, é mais fácil automatizar. 

A tabela está agora disponível para utilização em consultas de análise. Irá aparecer na análise 

### <a name="use-the-table"></a>Utilize a tabela

Vamos supor que a definição de origem de dados é chamada `usermap`, e que tem dois campos, `realName` e `user_AuthenticatedId`. O `requests` tabela também tem um campo chamado `user_AuthenticatedId`, por isso, é fácil juntá-las:

```AIQL

    requests
    | where notempty(user_AuthenticatedId) | take 10
    | join kind=leftouter ( usermap ) on user_AuthenticatedId 
```
A tabela resultante de pedidos de tem uma coluna adicional, `realName`.

### <a name="import-from-logstash"></a>Importar do LogStash

Se usar [LogStash](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html), pode utilizar a análise para consultar os seus registos. Utilize o [Plug-in que encaminha os dados para análise](https://github.com/Microsoft/logstash-output-application-insights). 

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

