---
title: Utilizar o portal de registo de pesquisa no Log Analytics do Azure | Microsoft Docs
description: Este artigo inclui um tutorial que descreve como criar registo pesquisas e analisar os dados armazenados na sua área de trabalho de análise de registos com o portal de registo de pesquisa.  O tutorial inclui a execução de algumas consultas simples para devolver diferentes tipos de dados e analisar os resultados.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 37213012e817f0fae21a47a4334a519bbbca206b
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="create-log-searches-in-azure-log-analytics-using-the-log-search-portal"></a>Criar registo pesquisas na análise de registos do Azure utilizando o portal de registo de pesquisa

> [!NOTE]
> Este artigo descreve o portal de registo de pesquisa na análise de registos do Azure utilizando a linguagem de consulta de novo.  Pode saber mais sobre o novo idioma e obter o procedimento para atualizar a sua área de trabalho em [atualizar a sua área de trabalho do Log Analytics do Azure para a pesquisa do novo registo](log-analytics-log-search-upgrade.md).  
>
> Se a sua área de trabalho não foi atualizada para o novo idioma de consulta, devem consultar a [localizar dados através de pesquisas de registo na análise de registos](log-analytics-log-searches.md) para obter informações sobre a versão atual do portal de registo de pesquisa.

Este artigo inclui um tutorial que descreve como criar registo pesquisas e analisar os dados armazenados na sua área de trabalho de análise de registos com o portal de registo de pesquisa.  O tutorial inclui a execução de algumas consultas simples para devolver diferentes tipos de dados e analisar os resultados.  Concentra-se nas funcionalidades no portal de registo de pesquisa para modificar a consulta, em vez de diretamente a modificar.  Para obter detalhes sobre a editar diretamente a consulta, consulte o [referência de linguagem de consulta](https://go.microsoft.com/fwlink/?linkid=856079).

Para criar pesquisas no portal da análise avançada em vez do portal de pesquisa de registo, consulte [começar a utilizar o Portal da análise](https://go.microsoft.com/fwlink/?linkid=856587).  Ambos os portais de utilizam a mesma linguagem de consulta para aceder aos mesmos dados, na área de trabalho de análise de registos.

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que já tem uma área de trabalho de análise de registos com pelo menos uma origem ligada que gera dados para as consultas analisar.  

- Se não tiver uma área de trabalho, pode criar um livre utilizando o procedimento em [começar com uma área de trabalho de análise de registos](log-analytics-get-started.md).
- Ligar pelo menos, um [agente do Windows](log-analytics-windows-agent.md) ou um [agente Linux](log-analytics-linux-agents.md) à área de trabalho.  

## <a name="open-the-log-search-portal"></a>Abrir o portal da Pesquisa de Registos
Comece por abrir o portal do Registo de Pesquisas. 

1. Abra o portal do Azure.
2. Navegue até à análise de registos e selecione a sua área de trabalho.
3. Selecione **Iniciar pesquisa**.

![Botão de procura de registo](media/log-analytics-log-search-log-search-portal/log-search-button.png)

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
Cada registo recolhido pelo Log Analytics tem uma **TimeGenerated** propriedade que contém a data e hora em que o registo foi criado.  Uma consulta no portal de registo de pesquisa devolve apenas os registos com um **TimeGenerated** dentro do âmbito de tempo que é apresentado no lado esquerdo do ecrã.  

Pode alterar o filtro de tempo, selecionando a lista pendente ou modificando o controlo de deslize.  O controlo de deslize apresenta um gráfico de barras que mostra o número de registos para cada segmento de tempo dentro do intervalo relativo.  Este segmento irá variar consoante o intervalo.

O âmbito de tempo predefinido é **1 dia**.  Altere este valor para **7 dias**, e deve aumentar o número total de registos.

![Data hora âmbito](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-the-query"></a>Filtrar os resultados da consulta
No lado esquerdo do ecrã está disponível o painel de filtro, que lhe permite adicionar filtragem à consulta sem modificá-la diretamente.  São apresentadas várias propriedades de registos devolvidos com os respetivos valores de dez principais com maior número de registo.

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

Só tem do **filtro** opção para propriedades com os respetivos nomes azul.  Estes são campos *pesquisável* que são indexados para condições de pesquisa.  Os campos a cinzento são campos *de texto livre pesquisáveis* que só têm a opção **Show references** (Mostrar referências).  Esta opção devolve registos que tenham esse valor em qualquer propriedade.

![Menu de filtro](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Pode agrupar os resultados de uma propriedade individual ao selecionar a opção **Group by** (Agrupar por) no menu do registo.  Desta forma, é adicionado o operador [summarize](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) (resumir) à sua consulta, o qual apresenta os resultados num gráfico.  Pode agrupar mais de uma propriedade, mas terá de editar a consulta diretamente.  Selecione o menu de registo junto a **computador** propriedade e selecione **Agrupar por 'Computador'**.  

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
Os dados de desempenho dos agentes Windows e Linux são armazenados na área de trabalho do Log Analytics, na tabela **Perf**.  Registos de desempenho mais semelhante a qualquer outro registo e iremos pode escrever uma consulta simples que devolve todos os registos de desempenho, tal como com eventos.

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

- Saiba mais sobre o idioma de consulta de análise de registos em [começar a utilizar o Portal da análise](https://go.microsoft.com/fwlink/?linkid=856079).
- Percorrer um tutorial, utilizando o [portal da análise avançadas](https://go.microsoft.com/fwlink/?linkid=856587) que lhe permite executar as mesmas consultas e aceder os mesmos dados que o portal de registo de pesquisa.
