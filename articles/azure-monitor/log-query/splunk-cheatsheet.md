---
title: Splunk ao Log Analytics do Azure | Documentos da Microsoft
description: Para ajudá-lo para os utilizadores que estão familiarizados com Splunk em aprender a linguagem de consulta do Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/21/2018
ms.author: bwren
ms.openlocfilehash: 61f0cff661c79f994a5b3c20646996f617a31b7e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/04/2018
ms.locfileid: "52883175"
---
# <a name="splunk-to-log-analytics"></a>Splunk ao Log Analytics

Este artigo destina-se para ajudar os utilizadores que estão familiarizados com Splunk em aprender a linguagem de consulta do Log Analytics. Comparações diretas são feitas entre os dois para compreender as principais diferenças e também as semelhanças em que pode aproveitar seu conhecimento existente.

## <a name="structure-and-concepts"></a>Estrutura e conceitos

A tabela seguinte compara as estruturas de dados e de conceitos entre Splunk e o Log Analytics.

 | Conceito  | Splunk | Log Analytics |  Comentário
 | --- | --- | --- | ---
 | Unidade de implementação  | cluster |  cluster |  O log Analytics permite arbitrário cruzada consultas de cluster. Splunk, não. |
 | Caches de dados |  registos  |  Políticas de colocação em cache e retenção |  Controla o período e a colocação em cache ao nível dos dados. Esta definição diretamente afeta o desempenho das consultas e o custo da implementação. |
 | Partição lógica de dados  |  índice  |  base de dados  |  Permite a separação lógica dos dados. As duas implementações permitem uniões e a associação entre estas partições. |
 | Metadados de eventos estruturados | N/A | tabela |  Splunk não tem o conceito exposto para o idioma de procura de metadados de eventos. Log Analytics tem o conceito de uma tabela, que tem colunas. Cada instância de eventos é mapeada para uma linha. |
 | Registo de dados | event | linha |  Apenas a mudança de terminologia. |
 | Atributo de registo de dados | Campo |  Coluna |  No Log Analytics, está predefinida como parte da estrutura de tabela. No Splunk, cada evento tem seu próprio conjunto de campos. |
 | Tipos | tipo de dados |  tipo de dados |  Tipos de dados do log Analytics são mais explícitos como eles são definidos nas colunas. Ambos têm a capacidade de trabalhar dinamicamente com tipos de dados e praticamente equivalente conjunto de tipos de dados incluindo suporte JSON. |
 | Consulta e pesquisa  | procurar | consulta |  Conceitos são essencialmente o mesmo entre o Log Analytics e Splunk. |
 | Tempo de ingestão de eventos | Hora do sistema | ingestion_time() |  No Splunk, cada evento obtém um carimbo de sistema da hora em que o evento foi indexado. No Log Analytics, pode definir uma política denominada ingestion_time que expõe uma coluna de sistema que pode ser referenciada por meio da função de ingestion_time(). |

## <a name="functions"></a>Funções

A seguinte tabela especifica as funções no Log Analytics que são equivalentes às funções de Splunk.

|Splunk | Log Analytics |Comentário
|---|---|---
|strcat | strcat()| (1) |
|dividir  | split() | (1) |
|IF     | IFF()   | (1) |
|tonumber | todouble()<br>tolong()<br>toint() | (1) |
|superior<br>Inferior |toupper()<br>tolower()|(1) |
| substituir | replace() | (1)<br> Tenha também em atenção que, embora `replace()` assume três parâmetros em ambos os produtos, os parâmetros são diferentes. |
| substr | substring() | (1)<br>Observe também que Splunk usa índices baseado num. O log Analytics notas de índices com base em zero. |
| tolower |  tolower() | (1) |
| toupper | toupper() | (1) |
| Correspondência | coincide com regex |  (2)  |
| RegEx | coincide com regex | No Splunk, `regex` é um operador. No Log Analytics, é um operador relacional. |
| searchmatch | == | No Splunk, `searchmatch` permite pesquisar na cadeia exata.
| aleatório | rand()<br>rand(n) | Função do Splunk devolve um número de 0 até 2<sup>31</sup>-1. Do log Analytics devolve um número entre 0,0 e 1,0, ou se um parâmetro fornecido, entre 0 e n-1.
| agora | now() | (1)
| relative_time | ToTimeSpan() | (1)<br>No Log Analytics, o equivalente do Splunk relative_time (datetimeVal, offsetVal) é datetimeVal + totimespan(offsetVal).<br>Por exemplo, <code>search &#124; eval n=relative_time(now(), "-1d@d")</code> torna-se <code>...  &#124; extend myTime = now() - totimespan("1d")</code>.

(1) no Splunk, a função é invocada com o `eval` operador. No Log Analytics, é utilizado como parte da `extend` ou `project`.<br>(2) no Splunk, a função é invocada com o `eval` operador. No Log Analytics, pode ser utilizado com o `where` operador.


## <a name="operators"></a>Operadores

As secções seguintes fornecem exemplos de como utilizar diferentes operadores entre Splunk e o Log Analytics.

> [!NOTE]
> Para fins de exemplo a seguir, o campo de Splunk _regra_ é mapeado para uma tabela no Azure Log Analytics e padrão timestamp é mapeado do Splunk para a análise de registos _ingestion_time()_ coluna.

### <a name="search"></a>Pesquisa
No Splunk, pode omitir o `search` palavra-chave e especifique uma cadeia de caracteres unquoted. No Azure Log Analytics tem de iniciar cada pesquisa com `find`, uma cadeia de caracteres unquoted é um nome de coluna e o valor de pesquisa deve ser uma cadeia de caracteres com aspas simples. 

| |  | |
|:---|:---|:---|
| Splunk | **Pesquisa** | <code>search Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" earliest=-24h</code> |
| Log Analytics | **find** | <code>find Session.Id=="c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time()> ago(24h)</code> |
| | |

### <a name="filter"></a>Filtro
Início de consultas do Log Analytics do Azure a partir de um resultado de tabela definido em que o filtro. No Splunk, a filtragem é a operação padrão no índice atual. Também pode utilizar `where` operador no Splunk, mas não é recomendado.

| |  | |
|:---|:---|:---|
| Splunk | **Pesquisa** | <code>Event.Rule="330009.2" Session.Id="c8894ffd-e684-43c9-9125-42adc25cd3fc" _indextime>-24h</code> |
| Log Analytics | **onde** | <code>Office_Hub_OHubBGTaskError<br>&#124; where Session_Id == "c8894ffd-e684-43c9-9125-42adc25cd3fc" and ingestion_time() > ago(24h)</code> |
| | |


### <a name="getting-n-eventsrows-for-inspection"></a>Obter eventos n/linhas para inspeção 
O Azure Log Analytics também suporta `take` como um alias para `limit`. No Splunk, se os resultados são ordenados, `head` devolve os primeiros resultados n. No Azure Log Analytics, limite não está ordenado, mas devolve as primeiras n linhas que se encontram.

| |  | |
|:---|:---|:---|
| Splunk | **HEAD** | <code>Event.Rule=330009.2<br>&#124; head 100</code> |
| Log Analytics | **Limite** | <code>Office_Hub_OHubBGTaskError<br>&#124; limit 100</code> |
| | |



### <a name="getting-the-first-n-eventsrows-ordered-by-a-fieldcolumn"></a>Obter as primeiras n eventos/linhas ordenadas por uma campo/colunas
Para obter os resultados na parte inferior, Splunk vai utilizar `tail`. No Azure Log Analytics pode especificar a direção de ordenação com `asc`.

| |  | |
|:---|:---|:---|
| Splunk | **HEAD** |  <code>Event.Rule="330009.2"<br>&#124; sort Event.Sequence<br>&#124; head 20</code> |
| Log Analytics | **parte superior** | <code>Office_Hub_OHubBGTaskError<br>&#124; top 20 by Event_Sequence</code> |
| | |




### <a name="extending-the-result-set-with-new-fieldscolumns"></a>Estender o resultado definido com novos campos/colunas
Splunk também tem um `eval` função, que não deve ser comparável com o `eval` operador. Ambas as `eval` operador no Splunk e o `extend` operador no Log Analytics do Azure só suportam funções escalares e operadores aritméticos.

| |  | |
|:---|:---|:---|
| Splunk | **Avaliação** |  <code>Event.Rule=330009.2<br>&#124; eval state= if(Data.Exception = "0", "success", "error")</code> |
| Log Analytics | **Expandir** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend state = iif(Data_Exception == 0,"success" ,"error")</code> |
| | |


### <a name="rename"></a>Mudar o Nome 
O Azure Log Analytics utiliza o mesmo operador para mudar o nome e para criar um novo campo. Splunk tem dois operadores separados, `eval` e `rename`.

| |  | |
|:---|:---|:---|
| Splunk | **Mudar o nome** |  <code>Event.Rule=330009.2<br>&#124; rename Date.Exception as execption</code> |
| Log Analytics | **Expandir** | <code>Office_Hub_OHubBGTaskError<br>&#124; extend exception = Date_Exception</code> |
| | |




### <a name="format-resultsprojection"></a>Formato resultados/projeção
Splunk não parece ter um operador semelhante à `project-away`. Pode usar a interface do Usuário para filtrar os campos ausente.

| |  | |
|:---|:---|:---|
| Splunk | **Tabela** |  <code>Event.Rule=330009.2<br>&#124; table rule, state</code> |
| Log Analytics | **Projeto**<br>**fora do projeto** | <code>Office_Hub_OHubBGTaskError<br>&#124; project exception, state</code> |
| | |



### <a name="aggregation"></a>Agregação
Consulte a [agregações em consultas do Log Analytics](aggregations.md) para as funções de agregação diferente.

| |  | |
|:---|:---|:---|
| Splunk | **Estatísticas** |  <code>search (Rule=120502.*)<br>&#124; stats count by OSEnv, Audience</code> |
| Log Analytics | **resumir** | <code>Office_Hub_OHubBGTaskError<br>&#124; summarize count() by App_Platform, Release_Audience</code> |
| | |



### <a name="join"></a>Associar
Associação no Splunk possui limitações significativas. A subconsulta tem um limite de 10000 resultados (definido no ficheiro de configuração de implementação) e há um número limitado de associação sabores.

| |  | |
|:---|:---|:---|
| Splunk | **aderir** |  <code>Event.Rule=120103* &#124; stats by Client.Id, Data.Alias | Junte-se a Client.Id máx = 0 [pesquisar mais antigo = - 24h Event.Rule="150310.0" Data.Hresult= 2147221040]</code> |
| Log Analytics | **aderir** | <code>cluster("OAriaPPT").database("Office PowerPoint").Office_PowerPoint_PPT_Exceptions<br>&#124; where  Data_Hresult== -2147221040<br>&#124; join kind = inner (Office_System_SystemHealthMetadata<br>&#124; summarize by Client_Id, Data_Alias)on Client_Id</code>   |
| | |



### <a name="sort"></a>Ordenar
No Splunk, classificar em ordem tem de utilizar crescente o `reverse` operador. O Azure Log Analytics também suporta a definição de onde colocar os valores nulos, no início ou no final.

| |  | |
|:---|:---|:---|
| Splunk | **Ordenação** |  <code>Event.Rule=120103<br>&#124; sort Data.Hresult<br>&#124; reverse</code> |
| Log Analytics | **Ordenar por** | <code>Office_Hub_OHubBGTaskError<br>&#124; order by Data_Hresult,  desc</code> |
| | |



### <a name="multivalue-expand"></a>Expanda multivalue
Este é um operador semelhante no Splunk e no Log Analytics.

| |  | |
|:---|:---|:---|
| Splunk | **mvexpand** |  `mvexpand foo` |
| Log Analytics | **mvexpand** | `mvexpand foo` |
| | |




### <a name="results-facets-interesting-fields"></a>Faceta de resultados, interessante campos
No portal do Log Analytics, é exposta somente a primeira coluna. Todas as colunas estão disponíveis por meio da API.

| |  | |
|:---|:---|:---|
| Splunk | **Campos** |  <code>Event.Rule=330009.2<br>&#124; fields App.Version, App.Platform</code> |
| Log Analytics | **facetas** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; facet by App_Branch, App_Version</code> |
| | |




### <a name="de-duplicate"></a>Remover a duplicação
Pode usar `summarize arg_min()` em vez disso, para inverter a ordem dos quais obtém escolhido o registo.

| |  | |
|:---|:---|:---|
| Splunk | **eliminação de duplicados** |  <code>Event.Rule=330009.2<br>&#124; dedup device_id sortby -batterylife</code> |
| Log Analytics | **resumir arg_max()** | <code>Office_Excel_BI_PivotTableCreate<br>&#124; summarize arg_max(batterylife, *) by device_id</code> |
| | |




## <a name="next-steps"></a>Passos Seguintes

- Sempre o processo de uma aula sobre o [escrevem consultas no Log Analytics](get-started-queries.md).