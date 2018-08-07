---
title: Uma apresentação através da análise do Application Insights do Azure | Documentos da Microsoft
description: Breves exemplos de todas as consultas principais no Analytics, a ferramenta de pesquisa poderosa do Application Insights.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: mbullwin
ms.openlocfilehash: 470779f80e998c3908cf28328cfb415d98c5e06c
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579260"
---
# <a name="a-tour-of-analytics-in-application-insights"></a>Uma visita guiada da análise no Application Insights
[Análise](app-insights-analytics.md) é a funcionalidade de pesquisa avançada de [Application Insights](app-insights-overview.md). Essas páginas descrevem a linguagem de consulta do Log Analytics.

* **[Assista ao vídeo introdutório](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Faça o test drive Analytics nos nossos dados simulados](https://analytics.applicationinsights.io/demo)**  se a aplicação não está a enviar dados para o Application Insights ainda.
* **[Roteiro para SQL-usuários](https://aka.ms/sql-analytics)**  traduz as linguagens mais comuns.

Vamos dar uma passagem de algumas consultas básicas para começar.

## <a name="connect-to-your-application-insights-data"></a>Ligar aos seus dados do Application Insights
Abrir o Analytics a partir da sua aplicação [painel de descrição geral](app-insights-dashboards.md) no Application Insights:

![Abrir portal.azure.com, abra o recurso do Application Insights e clique em Analytics.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsiodocslanguage-referencetabular-operators-show-me-n-rows"></a>[Tirar](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators): mostre-me n linhas
Pontos de dados que operações de utilizador (normalmente, pedidos HTTP recebidos pela sua aplicação web) de registo são armazenados numa tabela chamada `requests`. Cada linha é um ponto de dados de telemetria recebido a partir do SDK do Application Insights na sua aplicação.

Vamos começar examinando algumas linhas de exemplo da tabela:

![resultados](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Coloque o cursor em algum lugar na instrução antes de clicar em Go. Pode dividir uma instrução sobre mais de uma linha, mas não a coloque linhas em branco numa instrução. Linhas em branco são uma forma conveniente de manter várias consultas separadas na janela.
>
>

Escolher colunas, arraste-as, grupo por colunas e filtrar:

![Clique em colunas a seleção no canto superior direito de resultados](./media/app-insights-analytics-tour/030.png)

Expanda qualquer item para ver os detalhes:

![Escolha a tabela e utilizar colunas de configurar](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Clique no cabeçalho de uma coluna para reordenar os resultados disponíveis no navegador da web. Mas lembre-se de que para um conjunto de resultados grande, o número de linhas transferidas para o navegador é limitado. Classificação desta forma simplesmente classifica o conjunto de resultados retornados e sempre não mostra os itens de maiores ou menores reais. Para ordenar os itens de forma fiável, utilize o `top` ou `sort` operador.
>
>

## <a name="query-across-applications"></a>Consultar entre aplicativos
Se pretender combinar dados de várias aplicações do Application Insights, utilize o **aplicação** palavra-chave para especificar o aplicativo com o nome da tabela.  Esta consulta combina os pedidos a partir de dois aplicativos diferentes usarem o **União** comando.


```AIQL

    union app('fabrikamstage').requests, app('fabrikamprod').requests
    
```

## <a name="tophttpsdocsloganalyticsiodocslanguage-referencetabular-operatorstop-operator-and-sorthttpsdocsloganalyticsiodocslanguage-referencetabular-operatorssort-operator"></a>[Parte superior](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator) e [ordenação](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator)
`take` é útil para obter um exemplo rápido de um resultado, mas mostra linhas da tabela sem nenhuma ordem específica. Para obter uma vista ordenada, utilize `top` (para obter um exemplo) ou `sort` (através da tabela inteira).

Mostre-me as primeiras n linhas, ordenadas por uma coluna específica:

```AIQL

    requests | top 10 by timestamp desc
```

* *Sintaxe:* maioria dos operadores tem parâmetros de palavra-chave como `by`.
* `desc` = a ordem decrescente, `asc` = ascendente.

![](./media/app-insights-analytics-tour/260.png)

`top...` é uma forma de alto desempenho mais dizer `sort ... | take...`. Podemos poderia ter escrito:

```AIQL

    requests | sort by timestamp desc | take 10
```

O resultado seria o mesmo, mas executaria um pouco mais lenta. (Também poderia escrever `order`, que é um alias de `sort`.)

Os cabeçalhos de coluna na vista de tabela também podem ser utilizados para ordenar os resultados na tela. Mas, obviamente, se já usou `take` ou `top` para recuperar apenas parte de uma tabela, clique no cabeçalho de coluna será apenas reordenar os registros de recuperar.

## <a name="wherehttpsdocsloganalyticsiodocslanguage-referencetabular-operatorswhere-operator-filtering-on-a-condition"></a>[Onde](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator): filtragem numa condição

Vamos ver apenas pedidos que devolveu um código de resultado específico:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

O `where` operador utiliza uma expressão booleana. Aqui estão alguns pontos importantes sobre os mesmos:

* `and`, `or`: Operadores booleanos
* `==`, `<>`, `!=` : igual e não igual a
* `=~`, `!~` : cadeia de maiúsculas e minúsculas igual e não é igual. Há muitos operadores de comparação de cadeia de caracteres mais.

<!---Read all about [scalar expressions]().--->

### <a name="find-unsuccessful-requests"></a>Localizar pedidos sem êxito

Converter um valor de cadeia de caracteres para um número inteiro para utilizar superior-que comparação:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a>Hora

Por predefinição, as suas consultas estão restritas para as últimas 24 horas. Mas pode alterar este intervalo:

![](./media/app-insights-analytics-tour/change-time-range.png)

Substituir o intervalo de tempo escrevendo qualquer consulta que menciona `timestamp` numa cláusula where. Por exemplo:

```AIQL

    // What were the slowest requests over the past 3 days?
    requests
    | where timestamp > ago(3d)  // Override the time range
    | top 5 by duration
```

A funcionalidade de intervalo de tempo é equivalente a uma cláusula "where" inserida após cada menção de uma das tabelas de origem.

`ago(3d)` significa "há três dias". Outras unidades de tempo incluem horas (`2h`, `2.5h`), minutos (`25m`) e segundos (`10s`).

Outros exemplos:

```AIQL

    // Last calendar week:
    requests
    | where timestamp > startofweek(now()-7d)
        and timestamp < startofweek(now())
    | top 5 by duration

    // First hour of every day in past seven days:
    requests
    | where timestamp > ago(7d) and timestamp % 1d < 1h
    | top 5 by duration

    // Specific dates:
    requests
    | where timestamp > datetime(2016-11-19) and timestamp < datetime(2016-11-21)
    | top 5 by duration

    // Between specific day/time range
    requests
    | where timestamp > datetime(2018-05-17T17:06:19.892Z) and timestamp <= datetime(2018-05-18T17:06:19.892Z)
    | where duration > 0

```

[Datas e horas referência](https://docs.loganalytics.io/docs/Language-Reference/Data-types/datetime).


## <a name="projecthttpsdocsloganalyticsiodocslanguage-referencetabular-operatorsproject-operator-select-rename-and-compute-columns"></a>[Projeto](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator): selecionar, mudar o nome e colunas de computação
Uso [ `project` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator) para encontrar apenas as colunas que pretende:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

Também pode mudar o nome de colunas e definir novos:

```AIQL

    requests
    | top 10 by timestamp desc
    | project  
            name,
            response = resultCode,
            timestamp,
            ['time of day'] = floor(timestamp % 1d, 1s)
```

![Resultado](./media/app-insights-analytics-tour/270.png)

* Os nomes das colunas podem incluir espaços ou símbolos se eles estão entre parênteses, como este: `['...']` ou `["..."]`
* `%` é o módulo operador habitual.
* `1d` (que é um dígito, em seguida, um tinha ") é um intervalo de tempo literal que significa um dia. Aqui estão alguns mais literais de timespan: `12h`, `30m`, `10s`, `0.01s`.
* `floor` (alias `bin`) arredonda um valor para baixo até ao múltiplo mais próximo do valor base que indicar. Então, `floor(aTime, 1s)` Arredonda um tempo para baixo para o segundo mais próximo.

As expressões podem incluir todos os operadores usuais (`+`, `-`,...), e há uma variedade de funções úteis.

## <a name="extend"></a>Expansão
Se pretender apenas adicionar colunas para os já existentes, utilize [ `extend` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

Usando [ `extend` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator) é menos detalhada do que [ `project` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator) se pretende manter todas as colunas existentes.

### <a name="convert-to-local-time"></a>Converter a hora local

Carimbos de data / sempre é em formato UTC. Portanto, se estiver na Costa do Pacífico-nos e é Inverno, a hora local é-8 horas da UTC, poderá assim:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```

## <a name="summarizehttpsdocsloganalyticsiodocslanguage-referencetabular-operatorssummarize-operator-aggregate-groups-of-rows"></a>[Resumir](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator): agregar os grupos de linhas
`Summarize` aplica-se uma determinada *função de agregação* através de grupos de linhas.

Por exemplo, o tempo a aplicação web demora a responder a um pedido é comunicado no campo `duration`. Vamos ver o tempo de resposta médio para todos os pedidos:

![](./media/app-insights-analytics-tour/410.png)

Ou poderia separamos o resultado em pedidos de nomes diferentes:

![](./media/app-insights-analytics-tour/420.png)

`Summarize` recolhe os pontos de dados no fluxo em grupos para o qual o `by` cláusula avalia igualmente. Cada valor no `by` expressão - cada nome de operação exclusivos no exemplo acima - resulta numa linha na tabela de resultados.

Em alternativa, pode agrupar os resultados por hora do dia:

![](./media/app-insights-analytics-tour/430.png)

Observe como estamos a utilizar o `bin` função (também conhecido como `floor`). Se usássemos apenas `by timestamp`, todas as linhas de entrada acabaria no seu próprio grupo pequeno. Para qualquer valor de escalar contínua, como tempos ou números, temos de dividir o intervalo contínuo num número gerenciável de valores discretos. `bin` -que é apenas o familiar de arredondamento pendente `floor` funcionar – é a forma mais fácil fazer isso.

Podemos usar a mesma técnica para reduzir a intervalos de cadeias de caracteres:

![](./media/app-insights-analytics-tour/440.png)

Tenha em atenção que pode utilizar `name=` para definir o nome de uma coluna de resultado, nas expressões de agregação ou a-cláusula by.

## <a name="counting-sampled-data"></a>Contagem de diversas amostras de dados
`sum(itemCount)` é a agregação recomendada para a contagem de eventos. Em muitos casos, itemCount = = 1, para que a função simplesmente conta o número de linhas no grupo. Mas quando [amostragem](app-insights-sampling.md) está em operação, apenas uma fração dos eventos originais são mantidas como pontos de dados no Application Insights, para que para cada ponto de dados, ver, existem `itemCount` eventos.

Por exemplo, se amostragem descarta 75% de eventos originais, em seguida, o itemCount = = 4 nos registos de retida - ou seja, para cada registo retido, houve quatro registos originais.

Amostragem adaptável faz com que itemCount ser mais alto durante períodos quando a aplicação está a ser utilizada intensivamente.

Por conseguinte resumindo itemCount dá uma boa estimativa do número original de eventos.

![](./media/app-insights-analytics-tour/510.png)

Há também um `count()` agregação (e uma operação de contagem), para casos em que realmente quiser contar o número de linhas num grupo.

Há diversas [funções de agregação](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions).

## <a name="charting-the-results"></a>Os resultados de criação de gráficos
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Por predefinição, os resultados apresentam como uma tabela:

![](./media/app-insights-analytics-tour/225.png)

Podemos fazer melhor do que a exibição de tabela. Vamos examinar os resultados na vista de gráfico com o vertical barra opção:

![Clique em gráfico, em seguida, selecione o gráfico de barras Vertical e atribuir x e y dos eixos](./media/app-insights-analytics-tour/230.png)

Tenha em atenção que, embora nós não classificar os resultados por hora (como pode ver na apresentação da tabela), a exibição de gráfico mostra sempre datetimes na ordem correta.


## <a name="timecharts"></a>Timecharts
Mostra o número de eventos aqui é a cada hora:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Selecione a opção de exibição de gráfico:

![Pego](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a>Várias séries
Várias expressões no `summarize` cláusula cria várias colunas.

Várias expressões no `by` cláusula cria várias linhas, uma para cada combinação de valores.

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Tabela de pedidos por hora e localização](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a>Segmentar um gráfico por dimensões
Se o gráfico de uma tabela que tem uma coluna de cadeia de caracteres e uma coluna numérica, a cadeia de caracteres pode servir para dividir os dados numéricos numa série separada de pontos. Se houver mais de uma coluna de cadeia de caracteres, pode escolher as colunas a utilizar como a discriminadora.

![Segmentar um gráfico de análise](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Taxa de devolução

Converta um valor booleano para uma cadeia de caracteres para utilizá-lo como uma discriminadora:

```AIQL

    // Bounce rate: sessions with only one page view
    requests
    | where notempty(session_Id)
    | where tostring(operation_SyntheticSource) == "" // real users
    | summarize pagesInSession=sum(itemCount), sessionEnd=max(timestamp)
               by session_Id
    | extend isbounce= pagesInSession == 1
    | summarize count()
               by tostring(isbounce), bin (sessionEnd, 1h)
    | render timechart
```

### <a name="display-multiple-metrics"></a>Apresentar a várias métricas
Se uma tabela que tem mais do que uma coluna numérica, além do período de tempo do gráfico pode apresentar qualquer combinação deles.

![Segmentar um gráfico de análise](./media/app-insights-analytics-tour/110.png)

Tem de selecionar **Split não** antes de poder selecionar várias colunas numéricas. Não é possível dividir por uma coluna de cadeia de caracteres ao mesmo tempo, como a exibição de mais do que uma coluna numérica.

## <a name="daily-average-cycle"></a>Ciclo de média diário
Como variar utilização ao longo do dia médio?

Contagem pedidos quando o módulo de um dia, posicionados em horas:

```AIQL

    requests
    | where timestamp > ago(30d)  // Override "Last 24h"
    | where tostring(operation_SyntheticSource) == "" // real users
    | extend hour = bin(timestamp % 1d , 1h)
          + datetime("2016-01-01") // Allow render on line chart
    | summarize event_count=sum(itemCount) by hour
```

![Gráfico de linhas de horas num dia médio](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> Observe que temos atualmente converter as horas de duração para datetimes para exibir num gráfico de linhas.
>
>

## <a name="compare-multiple-daily-series"></a>Comparar várias séries de diários
Como uso variar com a hora do dia em diferentes países?

```AIQL

     requests  
     | where timestamp > ago(30d)  // Override "Last 24h"
     | where tostring(operation_SyntheticSource) == "" // real users
     | extend hour= floor( timestamp % 1d , 1h)
           + datetime("2001-01-01")
     | summarize event_count=sum(itemCount)
       by hour, client_CountryOrRegion
     | render timechart
```

![Dividir por client_CountryOrRegion](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Desenhar uma distribuição
Como muitas sessões existem extensões diferentes?

```AIQL

    requests
    | where timestamp > ago(30d) // override "Last 24h"
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sessionDuration = max_timestamp - min_timestamp
    | where sessionDuration > 1s and sessionDuration < 3m
    | summarize count() by floor(sessionDuration, 3s)
    | project d = sessionDuration + datetime("2016-01-01"), count_
```

A última linha é necessário para converter para datetime. Atualmente, o eixo x de um gráfico é apresentado como um valor de escalar apenas se for um datetime.

O `where` cláusula exclui da sessões (sessionDuration = = 0) e define o comprimento do eixo x.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsiodocslanguage-referenceaggregation-functionspercentiles"></a>[Percentis](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/percentiles())
Os intervalos de durações abrangem percentagens diferentes de sessões?

Utilize a consulta acima, mas substitua a última linha:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s)
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
```

Removemos também o limite superior no where cláusula, para obter os números corretos, incluindo todas as sessões com mais de um pedido:

![Resultado](./media/app-insights-analytics-tour/180.png)

Onde podemos ver que:

* 5% de sessões têm uma duração inferior a 3 minutos 34s;
* menos de 36 minutos; da última 50% de sessões
* 5% de sessões últimos mais de 7 dias

Para obter uma análise detalhada separada para cada país, apenas nos tem que trazer a coluna de client_CountryOrRegion em separado através de ambos resumir operadores:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id, client_CountryOrRegion
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s), client_CountryOrRegion
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
      by client_CountryOrRegion
```

![](./media/app-insights-analytics-tour/190.png)

## <a name="join"></a>Associar
Temos acesso a várias tabelas, incluindo pedidos e exceções.

Para localizar as exceções relacionadas a um pedido que devolveu uma resposta de falha, pode associar as tabelas no `operation_Id`:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


É boa prática utilizar `project` para selecionar apenas as colunas, precisamos antes de executar a associação.
Nas cláusulas mesmo, renomeamos a coluna timestamp.

## <a name="lethttpsdocsloganalyticsiodocslanguage-referencequery-statementslet-statement-assign-a-result-to-a-variable"></a>[Permitir que](https://docs.loganalytics.io/docs/Language-Reference/Query-statements/Let-statement): atribuir um resultado a uma variável

Utilize `let` separar as partes da expressão anterior. Os resultados permanecem inalterados em:

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> O cliente de análise, não coloque linhas em branco entre as partes da consulta. Certifique-se executar todos.
>

Utilize `toscalar` para converter uma célula de tabela única para um valor:

```AIQL
let topCities =  toscalar (
   requests
   | summarize count() by client_City 
   | top n by count_ 
   | summarize makeset(client_City));
requests
| where client_City in (topCities(3)) 
| summarize count() by client_City;
```


### <a name="functions"></a>Funções

Uso *permitem* para definir uma função:

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a>Acesso a objetos aninhados
Os objetos aninhados podem ser acessados facilmente. Por exemplo, no fluxo de exceções pode ver objetos estruturados, como este:

![Resultado](./media/app-insights-analytics-tour/520.png)

Pode nivelamento-lo ao selecionar as propriedades que está interessado em:

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

Tenha em atenção que terá de converter o resultado para o tipo apropriado.


## <a name="custom-properties-and-measurements"></a>Propriedades personalizadas e medidas
Se seu aplicativo anexa [dimensões personalizadas (propriedades) e medições personalizadas](app-insights-api-custom-events-metrics.md#properties) aos eventos, em seguida, verá-os na `customDimensions` e `customMeasurements` objetos.

Por exemplo, se a sua aplicação inclui:

```csharp

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2.d2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

Para extrair estes valores no Analytics:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast to expected type
```

Para verificar se uma dimensão personalizada é de um tipo específico:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

### <a name="special-characters"></a>Carateres especiais

Para os identificadores com carateres especiais ou palavras-chave idiomáticas nos respetivos nomes, tem de aceder aos mesmos através de `['` e `']` ou a utilizar `["` e `"]`.

```AIQL

    customEvents
    | extend p2d2 = customDimensions.['p2.d2'], ...
```

[Regras de nomenclatura do identificador de referência](https://docs.loganalytics.io/docs/Learn/References/Naming-principles)

## <a name="dashboards"></a>Dashboards
Pode afixar os resultados num dashboard para reunir todos os seus mais importantes gráficos e tabelas.

* [Dashboard partilhado do Azure](app-insights-dashboards.md#share-dashboards): clique no ícone de pin. Antes de fazer isso, tem de ter um dashboard partilhado. No portal do Azure, abra ou criar um dashboard e clique em partilha.
* [Dashboard do Power BI](app-insights-export-power-bi.md): clique em exportar, consulta do Power BI. Uma vantagem essa alternativa é que pode exibir sua consulta juntamente com outros resultados entre uma vasta gama de origens.

## <a name="combine-with-imported-data"></a>Combine com dados importados

Relatórios de análise combinam bem com o dashboard, mas às vezes queira traduzir os dados a um formulário mais fácil de entender. Por exemplo, suponha que os utilizadores autenticados são identificados na telemetria por um alias. Gostaria de mostrar seus nomes real nos resultados. Para tal, precisa de um ficheiro CSV que mapeia desde os aliases para os nomes reais.

Pode importar um ficheiro de dados e utilizá-la tal como com qualquer uma das tabelas padrão (pedidos, exceções e assim por diante). Ou consultá-los por conta própria ou associe-a com outras tabelas. Por exemplo, se tiver uma tabela chamada usermap e ela tem colunas `realName` e `userId`, em seguida, pode usá-lo para traduzir o `user_AuthenticatedId` na telemetria de pedido:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get the realName field from the usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

Para importar uma tabela, no painel de esquema, em **outras origens de dados**, siga as instruções para adicionar uma nova origem de dados, através do carregamento de uma amostra dos seus dados. Em seguida, pode utilizar esta definição para carregar tabelas.

A funcionalidade de importação está atualmente em pré-visualização, verá inicialmente uma ligação "Contacte-nos" em "Outras origens de dados". Utilize esta opção para se inscrever para o programa de pré-visualização e a ligação, em seguida, irá ser substituída por um botão "Adicionar nova origem de dados".


## <a name="tables"></a>Tabelas
O fluxo de telemetria recebido da sua aplicação está acessível através de várias tabelas. O esquema das propriedades disponíveis para cada tabela está visível à esquerda da janela.

### <a name="requests-table"></a>Tabela de pedidos
Pedidos de HTTP de contagem à sua aplicação web e segmente por nome da página:

![Pedidos de contagem segmentados por nome](./media/app-insights-analytics-tour/analytics-count-requests.png)

Localize os pedidos que não obedeçam a maior parte:

![Pedidos de contagem segmentados por nome](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Tabela de eventos personalizados
Se usar [trackevent ()](app-insights-api-custom-events-metrics.md#trackevent) para enviar os seus próprios eventos, pode lê-los desta tabela.

Vejamos um exemplo em que o código da aplicação contém estas linhas:

```csharp

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

Apresenta a frequência desses eventos:

![Taxa de exibição de eventos personalizados](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Extrair os eventos medidas e dimensões:

![Taxa de exibição de eventos personalizados](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Tabela de métricas personalizadas
Se estiver a utilizar [trackmetric ()](app-insights-api-custom-events-metrics.md#trackmetric) para enviar os seus próprios valores de métrica, encontrará os resultados no **customMetrics** stream. Por exemplo:  

![Métricas personalizadas no analytics do Application Insights](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> Na [Explorador de métricas](app-insights-metrics-explorer.md), todas as medições personalizadas anexadas a qualquer tipo de telemetria são apresentados em conjunto no painel de métricas, juntamente com métricas enviados usando `TrackMetric()`. Mas no Analytics, medições personalizadas ainda estão ligadas a qualquer tipo de telemetria eram transportados no - eventos ou pedidos e assim por diante, enquanto métricas enviadas pelo TrackMetric são apresentadas no seu próprio fluxo.
>
>

### <a name="performance-counters-table"></a>Tabela de contadores de desempenho
[Contadores de desempenho](app-insights-performance-counters.md) Mostrar métricas do sistema básico para a sua aplicação, tais como CPU, memória e utilização da rede. Pode configurar o SDK para enviar contadores adicionais, incluindo seus próprios contadores personalizados.

O **performanceCounters** esquema expõe a `category`, `counter` nome, e `instance` nome de cada contador de desempenho. Os nomes de instâncias do contador só são aplicáveis a alguns contadores de desempenho e normalmente indicam o nome do processo ao qual está associado a contagem. A telemetria de cada aplicativo, verá apenas os contadores para essa aplicação. Por exemplo, para ver quais contadores estão disponíveis:

![Contadores de desempenho na análise do Application Insights](./media/app-insights-analytics-tour/analytics-performance-counters.png)

Para obter um gráfico de memória disponível durante o período selecionado:

![Pego de memória no analytics do Application Insights](./media/app-insights-analytics-tour/analytics-available-memory.png)

Como outra telemetria **performanceCounters** também tem uma coluna `cloud_RoleInstance` indica que a identidade da máquina de anfitrião no qual a aplicação está em execução. Por exemplo, para comparar o desempenho da sua aplicação em diferentes computadores:

![Desempenho segmentados por instância de função no Application Insights analytics](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Tabela de exceções
[Exceções comunicado pela sua aplicação](app-insights-asp-net-exceptions.md) estão disponíveis nesta tabela.

Para localizar o pedido HTTP que seu aplicativo estava a processar quando a exceção foi gerada, Junte-se no operation_Id:

![Junte-se de exceções com pedidos no operation_Id](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Tabela de tempos de browser
`browserTimings` Mostra os dados de carregamento de página recolhidos nos browsers dos seus utilizadores.

[Configure a sua aplicação para a telemetria do lado do cliente](app-insights-javascript.md) para ver estas métricas.

O esquema inclui [métricas que indica os comprimentos das diferentes estágios do processo de carregamento de página](app-insights-javascript.md#page-load-performance). (Não indicam o período de tempo que aos utilizadores ler uma página).  

Mostrar popularities de páginas diferentes e carregar vezes para cada página:

![Tempos de carregamento de página no Analytics](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a>Tabela de resultados de disponibilidade
`availabilityResults` Mostra os resultados da sua [testes web](app-insights-monitor-web-app-availability.md). Cada execução de seus testes em cada localização de teste é relatada separadamente.

![Tempos de carregamento de página no Analytics](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Tabela de dependências
Contém os resultados das chamadas que torna a sua aplicação para bases de dados e REST APIs e outra chama para TrackDependency(). Também inclui as chamadas AJAX feitas a partir do browser.

Chamadas AJAX do navegador:

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

Chamadas de dependência do servidor:

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

Resultados de dependência do lado do servidor sempre mostrarem `success==False` se o agente de informações da aplicação não está instalado. No entanto, os outros dados estão corretos.

### <a name="traces-table"></a>Tabela de rastreios
Contém a telemetria enviada pela sua aplicação com tracktrace (,) ou [outras arquiteturas de registo](app-insights-asp-net-trace-logs.md).

## <a name="video"></a>Vídeo 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

Consultas avançadas:

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a>Passos Seguintes
* [Referência de linguagem de análise](app-insights-analytics-reference.md)
* [Roteiro para SQL-usuários](https://aka.ms/sql-analytics) traduz as linguagens mais comuns.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
