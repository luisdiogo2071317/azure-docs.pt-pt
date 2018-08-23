---
title: Com o portal de pesquisa de registos no Log Analytics do Azure | Documentos da Microsoft
description: Este artigo inclui um tutorial que descreve como criar pesquisas de registos e analisar dados armazenados na sua área de trabalho do Log Analytics com o portal de pesquisa de registos.  O tutorial inclui a execução de algumas consultas simples para devolver diferentes tipos de dados e analisar os resultados.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/15/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: e97bab36a9a1a18cab1d45abfa451139323b2320
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42055969"
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Criar pesquisas de registos no Azure Log Analytics com o portal de pesquisa de registos

Este artigo inclui um tutorial que descreve como criar pesquisas de registos e analisar dados armazenados na sua área de trabalho do Log Analytics com o portal de pesquisa de registos.  O tutorial inclui a execução de algumas consultas simples para devolver diferentes tipos de dados e analisar os resultados.  Ele se concentra em recursos no portal da pesquisa de registos para modificar a consulta, em vez de modificá-la diretamente.  Para obter detalhes sobre como editar a consulta diretamente, consulte a [referência de linguagem de consulta](https://go.microsoft.com/fwlink/?linkid=856079).

Para criar pesquisas no portal do Advanced Analytics em vez do portal de pesquisa de registos, consulte [introdução ao Portal do Analytics](https://go.microsoft.com/fwlink/?linkid=856587).  Ambos os portais de usam a mesma linguagem de consulta para acessar os mesmos dados na área de trabalho do Log Analytics.

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial pressupõe que já tem uma área de trabalho do Log Analytics pelo menos uma origem ligada que gera dados para as consultas analisar.  

- Se não tiver uma área de trabalho, pode criar uma grátis, utilizando o procedimento em [começar com uma área de trabalho do Log Analytics](log-analytics-get-started.md).
- Ligar, pelo menos, um [agente do Windows](log-analytics-windows-agent.md) ou uma [agente Linux](log-analytics-linux-agents.md) à área de trabalho.  

## <a name="open-the-log-search-portal"></a>Abrir o portal da Pesquisa de Registos
Comece por abrir o portal do Registo de Pesquisas. 

1. Abra o portal do Azure.
2. Navegue para o Log Analytics e selecione a sua área de trabalho.
3. Selecione **pesquisa de registos**.

![Botão pesquisa de registos](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a>Criar uma pesquisa simples
A forma mais rápida de obter alguns dados com os quais trabalhar é utilizar uma pesquisa simples que devolve todos os registos numa tabela.  Se tiver clientes Windows ou Linux ligados à sua área de trabalho, terá dados na tabela Event (Windows) ou Syslog (Linux).

Escreva uma das consultas seguintes na caixa de pesquisa e clique no botão “search” (“pesquisar”).  

```
Event
```
```
Syslog
```

São devolvidos dados na vista de lista predefinida e pode ver o número total de registos que foram devolvidos.

![Consulta simples](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Só são apresentadas as primeiras propriedades de cada registo.  Clique em **show more** (mostrar mais) para apresentar todas as propriedades de um determinado registo.

![Detalhes do registo](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-the-time-scope"></a>Definir o âmbito de tempo
Todos os registos recolhidos pelo Log Analytics tem um **TimeGenerated** propriedade que contém a data e hora em que o registo foi criado.  Uma consulta no portal da pesquisa de registos só devolve registos com um **TimeGenerated** dentro do âmbito de tempo que é apresentado no lado esquerdo do ecrã.  

Pode alterar o filtro de tempo ao selecionar o menu suspenso ou modificando o controlo de deslize.  O controlo de deslize apresenta um gráfico de barras que mostra o número relativo de registos para cada segmento de tempo dentro do intervalo.  Este segmento irá variar consoante o período.

O âmbito de tempo predefinido é **1 dia**.  Alterar este valor para **7 dias**, e deve aumentar o número total de registos.

![Âmbito de tempo de data](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Filtrar os resultados da consulta
No lado esquerdo do ecrã está disponível o painel de filtro, que lhe permite adicionar filtragem à consulta sem modificá-la diretamente.  Várias propriedades de registos devolvidos são apresentadas com os respetivos valores de dez melhores com o maior número de registo.

Se estiver a trabalhar com **Event**, selecione a caixa de verificação junto a **Error** (Erro), em **EVENTLEVELNAME**.   Se estiver a trabalhar com **Syslog**, selecione a caixa de verificação junto a **Error**, em **SEVERITYLEVEL**.  Desta forma, a consulta é alterada para uma das seguintes, de modo a limitar os resultados a eventos de erro.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Filtro](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Adicione propriedades ao painel do filtro ao selecionar **Add to filters** (Adicionar aos filtros) no menu de propriedades de um dos registos.

![Adicionar ao menu de filtro](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Pode definir o mesmo filtro ao selecionar **Filter** (Filtro) no menu de propriedades de um registo com o valor pelo qual quer filtrar.  

Tem apenas o **filtro** opção para as propriedades com os respetivos nomes a azul.  Estes são campos *pesquisável* que são indexados para condições de pesquisa.  Os campos a cinzento são campos *de texto livre pesquisáveis* que só têm a opção **Show references** (Mostrar referências).  Esta opção devolve registos que tenham esse valor em qualquer propriedade.

![Menu de filtro](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Pode agrupar os resultados de uma propriedade individual ao selecionar a opção **Group by** (Agrupar por) no menu do registo.  Desta forma, é adicionado o operador [summarize](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) (resumir) à sua consulta, o qual apresenta os resultados num gráfico.  Pode agrupar mais de uma propriedade, mas terá de editar a consulta diretamente.  Selecione o menu do registo junto à propriedade **Computer** e selecione **Group by 'Computer'** (Agrupar por “Computer”).  

![Agrupar por computador](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Trabalhar com os resultados
O portal do Registo de Pesquisas tem várias funcionalidades para trabalhar com os resultados de uma consulta.  Pode ordenar, filtrar e agrupar resultados para analisar os dados sem modificar a consulta em si.  Os resultados das consultas não são ordenados por predefinição.

Para ver os dados numa forma de tabela, que oferece opções adicionais para filtrar e ordenar, clique em **Table** (Tabela).  

![Vista de tabela](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Clique na seta junto a um registo para ver os detalhes do mesmo.

![Ordenar os resultados](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Para ordenar por qualquer campo, clique no cabeçalho da respetiva coluna.

![Ordenar os resultados](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Filtre os resultados por um valor específico na coluna ao clicar no botão de filtro e indicar uma condição de filtro.

![Filtrar os resultados](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Agrupe numa coluna ao arrastar o cabeçalho da mesma para a parte superior dos resultados.  Pode agrupar por vários campos ao arrastar múltiplas colunas para a parte superior.

![Agrupar os resultados](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Trabalhar com dados de desempenho
Os dados de desempenho dos agentes Windows e Linux são armazenados na área de trabalho do Log Analytics, na tabela **Perf**.  Registos de desempenho mais semelhante a qualquer outro registo e podemos escrever uma consulta simples que devolve todos os registos de desempenho, tal com com eventos.

```
Perf
```

![Dados de desempenho](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

No entanto, devolver milhões de registos para todos os objetos e contadores de desempenho não é muito útil.  Pode utilizar os mesmos métodos que utilizou acima para filtrar os dados ou escrever a seguinte consulta diretamente na caixa de pesquisa de registos.  Esta ação devolve apenas registos de utilização do processador dos computadores Windows e Linux.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Utilização do processador](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Isto limita os dados a um contador específico, mas ainda não os põe numa forma que seja particularmente útil.  Pode apresentar os dados num gráfico de linhas, mas primeiro tem de agrupá-los por Computer e TimeGenerated.  Para agrupar em vários campos, tem de modificar a consulta diretamente, por isso, modifique-a para o seguinte.  Esta alteração utiliza a função [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) na propriedade **CounterValue** para calcular o valor médio ao longo de cada hora.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Gráfico de dados de desempenho](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Agora que os dados estão devidamente agrupados, pode apresentá-los num gráfico visual ao adicionar o operador [render](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator).  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Gráfico de linhas](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre a linguagem de consulta do Log Analytics na [introdução ao Portal do Analytics](https://go.microsoft.com/fwlink/?linkid=856079).
- Percorrer um tutorial com o [portal da análise avançada](https://go.microsoft.com/fwlink/?linkid=856587) que permite-lhe executar as mesmas consultas e acessar os mesmos dados que o portal de pesquisa de registos.
