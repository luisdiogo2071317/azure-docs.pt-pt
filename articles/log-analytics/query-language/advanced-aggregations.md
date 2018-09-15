---
title: Advanced agregações em consultas do Log Analytics do Azure | Documentos da Microsoft
description: Descreve algumas das opções mais avançadas de agregação disponíveis para consultas do Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 661ff7c07ba2bb17eb5830b38bb39e1c3e80bb55
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45602913"
---
# <a name="advanced-aggregations-in-log-analytics-queries"></a>Agregações avançadas em consultas do Log Analytics

> [!NOTE]
> Deve efetuar [agregações em consultas do Log Analytics](./aggregations.md) antes de concluir esta lição.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Este artigo descreve algumas das opções mais avançadas de agregação disponíveis para consultas do Log Analytics.

## <a name="generating-lists-and-sets"></a>Geração de listas e conjuntos
Pode usar `makelist` aos dados pivot ordem dos valores numa determinada coluna. Por exemplo, pode querer explorar o local de take de eventos de ordem mais comuns nas suas máquinas. Essencialmente, pode dinamizar os dados a ordem dos EventIDs em cada máquina. 

```KQL
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makelist(EventID) by Computer
```
|Computador|list_EventID|
|---|---|
| COMPUTADOR1 | [704,701,1501,1500,1085,704,704,701] |
| computador2 | [326,105,302,301,300,102] |
| ... | ... |

`makelist` gera uma lista na ordem em que os dados que foi passados para ele. Para ordenar eventos a partir de mais antigo ao mais recente, utilize `asc` na instrução order, em vez de `desc`. 

Também é útil criar uma lista de valores distintos apenas. Isso é chamado de um _definir_ e pode ser gerada com `makeset`:

```KQL
Event
| where TimeGenerated > ago(12h)
| order by TimeGenerated desc
| summarize makeset(EventID) by Computer
```
|Computador|list_EventID|
|---|---|
| COMPUTADOR1 | [704,701,1501,1500,1085] |
| computador2 | [326,105,302,301,300,102] |
| ... | ... |

Como `makelist`, `makeset` também funciona com ordenado de dados e irá gerar as matrizes com base na ordem as linhas que são transmitidas para o mesmo.

## <a name="expanding-lists"></a>Listas de expansão
O inverso da operação da `makelist` ou `makeset` é `mvexpand`, que expande uma lista de valores para separar linhas. Ele pode aumentar em qualquer número de colunas dinâmicas, JSON e de matriz. Por exemplo, pode verificar o *Heartbeat* tabela para enviar dados de computadores que enviaram um heartbeat na última hora de soluções:

```KQL
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, Solutions
```
| Computador | Soluções | 
|--------------|----------------------|
| COMPUTADOR1 | "security", "atualizações", "changeTracking" |
| computador2 | "security", "atualizações" |
| computer3 | "antiMalware", "changeTracking" |
| ... | ... | ... |

Utilize `mvexpand` para mostrar cada valor numa linha separada em vez de uma lista separada por vírgulas:

Heartbeat | onde TimeGenerated > há (1H) | projeto de computador, divisão (soluções, ",") | mvexpand soluções
```
| Computer | Solutions | 
|--------------|----------------------|
| computer1 | "security" |
| computer1 | "updates" |
| computer1 | "changeTracking" |
| computer2 | "security" |
| computer2 | "updates" |
| computer3 | "antiMalware" |
| computer3 | "changeTracking" |
| ... | ... | ... |
```

Em seguida, poderia usar `makelist` novamente agrupar itens em conjunto e, desta vez ver a lista de computadores por solução:

```KQL
Heartbeat
| where TimeGenerated > ago(1h)
| project Computer, split(Solutions, ",")
| mvexpand Solutions
| summarize makelist(Computer) by tostring(Solutions) 
```
|Soluções | list_Computer |
|--------------|----------------------|
| "segurança" | ["Computador1", "computador2"] |
| "atualizações" | ["Computador1", "computador2"] |
| "changeTracking" | ["Computador1", "computer3"] |
| "antiMalware" | ["computer3"] |
| ... | ... |

## <a name="handling-missing-bins"></a>Manipulação de discretizações em falta
Um aplicativo útil de `mvexpand` é a necessidade de preencher valores predefinidos para os contentores em falta. Por exemplo, suponha que estava procurando o tempo de atividade de uma determinada máquina ao explorar o heartbeat. Também queira ver a origem do heartbeat que está a ser o _categoria_ coluna. Normalmente, usaríamos um simples resumir instrução da seguinte forma:

```KQL
Heartbeat
| where TimeGenerated > ago(12h)
| summarize count() by Category, bin(TimeGenerated, 1h)
```
| Categoria | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Agente Direto | 2017-06-06T17:00:00Z | 15 |
| Agente Direto | 2017-06-06T18:00:00Z | 60 |
| Agente Direto | 2017-06-06T20:00:00Z | 55 |
| Agente Direto | 2017-06-06T21:00:00Z | 57 |
| Agente Direto | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |

Nessa época resulta no entanto o bucket associado "2017-06-06T19:00:00Z" está em falta porque não existem quaisquer dados de heartbeat durante essa hora. Utilize o `make-series` função para atribuir um valor predefinido para registos vazios. Esta ação irá gerar uma linha para cada categoria com duas colunas de matriz extra, um para valores e outro para registos de tempo correspondente:

```KQL
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
```

| Categoria | count_ | TimeGenerated |
|---|---|---|
| Agente Direto | [15,60,0,55,60,57,60...] | ["2017-06-06T17:00:00.0000000Z","2017-06-06T18:00:00.0000000Z","2017-06-06T19:00:00.0000000Z","2017-06-06T20:00:00.0000000Z","2017-06-06T21:00:00.0000000Z",...] |
| ... | ... | ... |

O terceiro elemento do *count_* matriz é 0, conforme o esperado e não existe um carimbo de correspondência de "2017-06-06T19:00:00.0000000Z" no _TimeGenerated_ matriz. Esse formato de matriz é difícil de ler, no entanto. Uso `mvexpand` para expandir as matrizes e produzir o mesmo formato de saída gerados por `summarize`:

```KQL
Heartbeat
| make-series count() default=0 on TimeGenerated in range(ago(1d), now(), 1h) by Category 
| mvexpand TimeGenerated, count_
| project Category, TimeGenerated, count_
```
| Categoria | TimeGenerated | count_ |
|--------------|----------------------|--------|
| Agente Direto | 2017-06-06T17:00:00Z | 15 |
| Agente Direto | 2017-06-06T18:00:00Z | 60 |
| Agente Direto | 2017-06-06T19:00:00Z | 0 |
| Agente Direto | 2017-06-06T20:00:00Z | 55 |
| Agente Direto | 2017-06-06T21:00:00Z | 57 |
| Agente Direto | 2017-06-06T22:00:00Z | 60 |
| ... | ... | ... |



## <a name="narrowing-results-to-a-set-of-elements-let-makeset-toscalar-in"></a>Os resultados para um conjunto de elementos de restrição: `let`, `makeset`, `toscalar`, `in`
Um cenário comum é selecionar os nomes de algumas entidades específicas com base num conjunto de critérios e, em seguida, filtrar um conjunto de dados diferente para baixo para esse conjunto de entidades. Por exemplo pode localizar os computadores que são conhecidos por ter atualizações em falta e identificar IPs que estes computadores destacados para:


```KQL
let ComputersNeedingUpdate = toscalar(
    Update
    | summarize makeset(Computer)
    | project set_Computer
);
WindowsFirewall
| where Computer in (ComputersNeedingUpdate)
```

## <a name="next-steps"></a>Passos Seguintes

Consulte outras lições para utilizar a linguagem de consulta do Log Analytics:

- [Operações de cadeia de caracteres](string-operations.md)
- [Operações de data e hora](datetime-operations.md)
- [Funções de agregação](aggregations.md)
- [Agregações avançadas](advanced-aggregations.md)
- [Estruturas de dados e JSON](json-data-structures.md)
- [Associações](joins.md)
- [Gráficos](charts.md)