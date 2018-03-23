---
title: Azure folha de referência do idioma de consulta de análise de registos | Microsoft Docs
description: Este artigo fornece assistência sobre a transição para o novo idioma de consulta de análise de registos, se já estiver familiarizado com o idioma de legado.
services: operations-management-suite
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2017
ms.author: bwren
ms.openlocfilehash: 9c487ab33859ae453a0074ef0344f61de19c7b4d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="transitioning-to-azure-log-analytics-new-query-language"></a>Transição para o novo idioma de consulta de análise de registos do Azure
Análise de registos recentemente implementado um novo idioma de consulta.  Este artigo fornece assistência sobre a transição neste idioma para análise de registos, se já estiver familiarizado com o idioma de legado e ainda precisa de algumas assistência.

## <a name="resources"></a>Recursos


## <a name="language-converter"></a>Conversor de idioma

Se estiver familiarizado com o idioma de consulta de análise de registos legado, é a forma mais fácil para criar a mesma consulta num novo idioma a utilizar o conversor de idioma instalado no portal de registo de pesquisa para a sua área de trabalho é convertida.  Utilizar o conversor é tão simple como escrever uma consulta legada na caixa de texto superior e, em seguida, clicando em **converter**.  Ou pode clicar no botão de procura para executar a consulta ou copiar e colar para utilizá-lo algures pessoa.

![Conversor de idioma](media/log-analytics-log-search-upgrade/language-converter.png)


## <a name="resources"></a>Recursos
O [site de documentação para o idioma de consulta de análise do registo](https://docs.loganalytics.io) tem todos os recursos tem de ficar previsto num novo idioma.  Isto inclui tutoriais, exemplos e uma referência de linguagem concluída.


## <a name="cheat-sheet"></a>Nota

A tabela seguinte fornece uma comparação entre uma variedade de consultas comuns para comandos equivalentes entre o idioma de consulta novo e legado no Log Analytics do Azure.

| Descrição | Legado | novo |
|:--|:--|:--|
| Todas as tabelas de pesquisa      | erro | procurar "error" (não sensível confidencial) |
| Selecione os dados da tabela | Type=Event |  Evento |
|                        | Tipo de evento de = &#124; selecione EventID de origem, o registo de eventos, | Evento &#124; projeto EventID de origem, o registo de eventos, |
|                        | Type=Event &#124; top 100 | Evento &#124; demorar 100 |
| Comparação de cadeias      | Type=Event Computer=srv01.contoso.com   | Event &#124; where Computer == "srv01.contoso.com" |
|                        | Type=Event Computer=contains("contoso") | Evento &#124; onde o computador contiver "contoso" (não sensível confidencial)<br>Evento &#124; onde computador contains_cs "Contoso" (sensível às maiúsculas e) |
|                        | Type=Event Computer=RegEx("@contoso@")  | Evento &#124; onde o computador com regex ". *contoso*" |
| Comparação de data        | Tipo = eventos TimeGenerated > 1DAYS agora | Evento &#124; onde TimeGenerated > ago(1d) |
|                        | Type=Event TimeGenerated>2017-05-01 TimeGenerated<2017-05-31 | Evento &#124; onde TimeGenerated entre (datetime(2017-05-01). datetime(2017-05-31)) |
| Comparação booleana     | Type=Heartbeat IsGatewayInstalled=false  | Heartbeat \| onde IsGatewayInstalled = = false |
| Ordenar                   | Tipo de evento de = &#124; ordenar asc do computador, desc de registo de eventos, EventLevelName asc | Evento \| ordenar por computador asc, desc de registo de eventos, EventLevelName asc |
| Distintos               | Tipo de evento de = &#124; eliminação de duplicados computador \| selecionar computador | Evento &#124; resumir por computador, o registo de eventos |
| Expandir colunas         | Tipo = desempenho CounterName = "% tempo do processador" &#124; EXPANDA if(map(CounterValue,0,50,0,1),"HIGH","LOW") como utilização | Desempenho &#124; onde CounterName = = "% de tempo do processador" \| expandir a utilização = iff (CounterValue > 50, "Alta", "Baixa") |
| Agregação            | Tipo de evento de = &#124; medir existente como contagem por computador | Evento &#124; resumir contagem = existente pelo computador |
|                                | Tipo = desempenho ObjectName = processador CounterName = "% de tempo do processador" &#124; medir avg(CounterValue) por intervalo de computador 5 minutos | Desempenho &#124; onde ObjectName = = "Processador" e CounterName = = "% de tempo do processador" &#124; resumir avg(CounterValue) por computador, bin (TimeGenerated, 5min) |
| Agregação de limite | Tipo = eventos &#124; medir existente pelo computador &#124; principais 10 | Event &#124; summarize AggregatedValue = count() by Computer &#124; limit 10 |
| União                  | Tipo = eventos ou tipo = Syslog | União evento Syslog |
| Associar                   | Tipo = NetworkMonitoring &#124; associação interna AgentIP (tipo = Heartbeat) ComputerIP | NetworkMonitoring &#124; join kind=inner (search Type == "Heartbeat") on $left.AgentIP == $right.ComputerIP |



## <a name="next-steps"></a>Passos Seguintes
- Veja uma [tutorial sobre como escrever consultas](https://go.microsoft.com/fwlink/?linkid=856078) utilizando a linguagem de consulta de novo.
- Consulte o [referência de linguagem de consulta](https://go.microsoft.com/fwlink/?linkid=856079) para obter detalhes sobre todos os comandos, operadores e funções para o novo idioma de consulta.  
