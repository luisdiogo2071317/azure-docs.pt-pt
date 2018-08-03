---
title: Alertas de registo no Azure Monitor
description: E-mails de Acionador, notificações, URLs de Web sites de chamada (webhooks) ou automatização quando estiverem reunidas as condições de consulta de análise que especificar para alertas do Azure.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: fd278ad6865c871ed0a5ed9272c9fadfca0f38db
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440434"
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Alertas de registo no Azure Monitor - alertas 
Este artigo fornece detalhes de alertas de registo são um dos tipos de alertas suportados dentro do novo [alertas do Azure](monitoring-overview-unified-alerts.md) e permitir que os utilizadores utilizem a plataforma de análise do Azure como base para alertas.


Alerta de registo é composta por regras de pesquisa de registos, criadas para [do Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) ou [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events). Os detalhes dos preços para alertas de registo está disponível na [preços do Azure Monitor](https://azure.microsoft.com/en-us/pricing/details/monitor/) página. No faturas do Azure, os alertas de registo são representadas como tipo `microsoft.insights/scheduledqueryrules` com:
- Alertas de registo no Application Insights apresentado com o nome exato do alerta, juntamente com o grupo de recursos e as propriedades do alerta
- Alertas no Log Analytics mostrado com o nome do alerta de registo `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` juntamente com o grupo de recursos e as propriedades do alerta

    > [!NOTE]
    > O nome para pesquisas guardadas tudo, cronogramas e ações criadas com a API de análise de registo tem de ser em minúsculas. Se inválido carateres como `<, >, %, &, \, ?, /` são utilizados – eles serão substituídos por `_` na fatura.

## <a name="log-search-alert-rule---definition-and-types"></a>Log search regra de alerta - definição e tipos

São criadas regras de pesquisa de registos pelos Alertas do Azure para executar automaticamente consultas de registos especificados a intervalos regulares.  Se os resultados da pesquisa de registos corresponderem a critérios específicos, é criado um registo de alerta. A regra pode executar automaticamente uma ou mais ações através dos [Grupos de Ações](monitoring-action-groups.md). 

Regras de pesquisa de registo são definidas pelos seguintes detalhes:
- **Consulta de registo**.  A consulta é executada sempre que a regra de alerta é acionado.  Os registos devolvidos por esta consulta são utilizados para determinar se é criado um alerta. *O Azure Application Insights* consulta também pode incluir [chamadas em várias aplicações](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery), desde que o utilizador tem direitos de acesso para os aplicativos externos. 

    > [!IMPORTANT]
    > Suporte de [cruzada consulta de aplicação para o Application Insights](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery) está em pré-visualização - a funcionalidade limitada para utilizar com 2 ou mais aplicações e experiência do usuário está sujeitas a alterações. Utilização de [cruzada consulta de área de trabalho](https://dev.loganalytics.io/oms/documentation/3-Using-the-API/CrossResourceQuery) e [entre recursos consulta para o Log Analytics](../log-analytics/log-analytics-cross-workspace-search.md) está atualmente **nepodporuje** nos alertas do Azure.

- **Período de tempo**.  Especifica o intervalo de tempo para a consulta. A consulta devolve apenas os registos que foram criados neste intervalo da hora atual. Período de tempo restringe os dados obtidos para a consulta de registo evitar abusos e evita qualquer comando de tempo (como há) utilizado numa consulta de registo. <br>*Por exemplo, se o período de tempo é definido como 60 minutos, e a consulta é executada em 1 às 15:15, apenas os registos criados entre 12:15 e 1 às 15:15 é devolvido ao executar a consulta de registo. Agora, se a consulta de registo utiliza o comando como atrás de tempo (7D), a consulta de log deve ser executada apenas para dados entre 12:15 e 1 às 15:15 - como se existem dados para apenas os últimos 60 minutos. E não durante sete dias de dados conforme especificado na consulta de registo.*
- **Frequência**.  Especifica a frequência com que a consulta deve ser executada. Pode ser qualquer valor entre 5 minutos e 24 horas. Deve ser igual ou menor que o período de tempo.  Se o valor for maior do que o período de tempo, em seguida, corre o risco de registos a ser omitidos.<br>*Por exemplo, considere um período de tempo de 30 minutos e uma frequência de 60 minutos.  Se a consulta é executada às 9:00 com 1, ele devolve registos entre 12:30 e 1 21 horas.  Da próxima vez que a consulta executaria é 2:00 quando ele retornaria registos entre 1:30 e 2:00.  Quaisquer registos criados entre 1:00 e 1:30 nunca seriam avaliados.*
- **Limiar**.  Os resultados da pesquisa de registos são avaliados para determinar se deve ser criado um alerta.  O limiar é diferente para os diferentes tipos de regras de alerta de pesquisa de registo.

Regras de pesquisa de registo, sê-lo para [do Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) ou [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events), podem ter dois tipos. Cada um desses tipos é descrita detalhadamente nas seções a seguir.

- **[Número de resultados](#number-of-results-alert-rules)**. Único alerta criada quando um número especificado de excedem o número de registos devolvido pela pesquisa de registo.
- **[Medida da métrica](#metric-measurement-alert-rules)**.  Alerta criado para cada objeto nos resultados da pesquisa de registo com valores que excedam o limiar especificado.

Seguem-se as diferenças entre os tipos de regra de alerta.

- *Número de resultados* regras de alerta cria sempre um tempo de alerta, único *medida da métrica* regra de alerta cria um alerta para cada objeto que é superior ao limiar.
- *Número de resultados* regras de alerta criar um alerta quando o limiar for excedido uma única vez. *Medida da métrica* regras de alerta podem criar um alerta quando o limiar for ultrapassado um determinado número de vezes ao longo de um intervalo de tempo específico.

### <a name="number-of-results-alert-rules"></a>Número de regras de alerta de resultados
**Número de resultados** regras de alerta criar um único alerta quando o número de registros retornados pela consulta de pesquisa exceder o limiar especificado. Este tipo de regra de alerta é ideal para trabalhar com eventos como logs de eventos do Windows, Syslog, resposta de aplicação Web e os registos personalizados.  Pode querer criar um alerta quando é criado um evento de erro específico ou quando vários eventos de erro são criados num período de tempo específico.

**Limiar**: O limite para um número de regras de alerta de resultados é maior ou menor do que um determinado valor.  Se o número de registos devolvidos pela pesquisa de registos corresponderem este critério, em seguida, é criado um alerta.

Para alertar relativamente a um único evento, defina o número de resultados para maior que 0 e procurar a ocorrência de um único evento que foi criado desde a última vez que a consulta foi executada. Alguns aplicativos podem iniciar um erro de ocasional necessariamente não deve emitir um alerta.  Por exemplo, o aplicativo pode repetir o processo que o evento de erro de criar e, em seguida, ter êxito na próxima vez.  Neste caso, não poderá criar o alerta, a menos que vários eventos são criados num período de tempo específico.  

Em alguns casos, pode querer criar um alerta na ausência de um evento.  Por exemplo, um processo pode iniciar eventos regulares para indicar que está a funcionar corretamente.  Se não iniciar um dos seguintes eventos num período de tempo específico, em seguida, um alerta deve ser criado.  Neste caso, definir o limiar **menos de 1**.

#### <a name="example"></a>Exemplo
Considere um cenário onde deseja saber quando a sua aplicação baseada na web dá uma resposta para os utilizadores com o código de 500 Erro de servidor (ou seja) interno. Poderia criar uma regra de alerta com os seguintes detalhes:  
- **Consulta:** pedidos | onde resultCode = = "500"<br>
- **Período de tempo:** 30 minutos<br>
- **Frequência do alerta:** cinco minutos<br>
- **Valor de limiar:** maior que 0<br>

Em seguida, alerta seria executar a consulta a cada 5 minutos, com 30 minutos de dados, para procurar qualquer registo em que o código de resultado era 500. Se nem um registo é encontrado, ele é acionado o alerta e aciona a ação configurada.

### <a name="metric-measurement-alert-rules"></a>Regras de alerta de medida da métrica

- **Medida da métrica** regras de alerta criar um alerta para cada objeto numa consulta com um valor que excede um limiar especificado.  Eles têm as seguintes diferenças distintas de **número de resultados** regras de alerta.
- **Função de agregação**: determina o cálculo que é executado e, potencialmente, com um numérico campo a agregar.  Por exemplo, **count ()** devolve o número de registos na consulta, **avg(CounterValue)** devolve a média do campo CounterValue ao longo do intervalo. Função de agregação na consulta tem de ser chamado/com o nome: AggregatedValue e fornecer um valor numérico. 
- **Campo de grupo**: é criado um registo com um valor agregado para cada instância deste campo e pode ser gerado um alerta para cada um.  Por exemplo, se quiser gerar um alerta para cada computador, usaria **por computador** 

    > [!NOTE]
    > Medida da métrica regras de alerta que se baseiam no Application Insights, pode especificar o campo para agrupar os dados. Para tal, utilize o **agregada no** opção na definição da regra.   
    
- **Intervalo**: define o intervalo de tempo durante o qual os dados são agregados.  Por exemplo, se especificou **cinco minutos**, teria de ser criado um registo para cada instância do campo Grupo agregada em intervalos de 5 minutos ao longo do período de tempo especificado para o alerta.

    > [!NOTE]
    > Função de posição tem de ser utilizada numa consulta para especificar o intervalo. Como bin() pode resultar em intervalos de tempo desiguais - alerta será convertida automaticamente bin comando ao comando bin_at com um horário apropriado no tempo de execução, para garantir resultados com um ponto de fixo
    
- **Limiar**: O limiar para regras de alerta de medida da métrica é definido por um valor agregado e um número de violações.  Se qualquer ponto de dados na pesquisa de registos exceder este valor, ele é considerado uma violação.  Se o número de falhas para qualquer objeto nos resultados excede o valor especificado, é criado um alerta para esse objeto.

#### <a name="example"></a>Exemplo
Considere um cenário em que queria um alerta se a qualquer computador excedeu a utilização do processador de 90% três vezes mais de 30 minutos.  Poderia criar uma regra de alerta com os seguintes detalhes:  

- **Consulta:** desempenho | onde ObjectName = = "Processador" e CounterName = = "% de tempo do processador" | resumir AggregatedValue = avg(CounterValue) por bin (TimeGenerated, 5 m), computador<br>
- **Período de tempo:** 30 minutos<br>
- **Frequência do alerta:** cinco minutos<br>
- **Agregar valor:** superior a 90<br>
- **Acionar alerta com base em:** Total de casos de violações de maior que 2<br>

A consulta deverá criar um valor médio para cada computador em intervalos de 5 minutos.  Esta consulta seria executada a cada 5 minutos para os dados recolhidos durante os 30 minutos anteriores.  Dados de exemplo são mostrados abaixo para três computadores.

![Resultados da consulta de exemplo](./media/monitor-alerts-unified/metrics-measurement-sample-graph.png)

Neste exemplo, seriam possível criar alertas separadas para srv02 e srv03, uma vez que eles infringido o limiar de 90% 3 vezes ao longo do período de tempo.  Se o **acionar alerta com base em:** foram alterados para **Consecutive** , em seguida, seria possível criar um alerta apenas para srv03, uma vez que ele infringido o limiar para três amostras consecutivas.


## <a name="log-search-alert-rule---creation-and-modification"></a>Log search regra de alerta - criação e modificação

Alerta de registo, bem como a respetiva consisting regra alerta de pesquisa de registo pode ser visualizada, criada ou modificada a partir:
- Portal do Azure
- REST APIs (incluindo através do PowerShell)
- Modelos Azure Resource Manager

### <a name="azure-portal"></a>Portal do Azure
Desde a introdução dos [novos alertas do Azure](monitoring-overview-unified-alerts.md), agora os utilizadores podem gerir todos os tipos de alertas no portal do Azure a partir de um único local e com os passos semelhantes para utilização. Saiba mais sobre [usando novos alertas do Azure](monitor-alerts-unified-usage.md).

Além disso, os utilizadores podem aperfeiçoa as consultas na plataforma de análise de escolha no Azure e, em seguida *importá-los para utilização em alertas ao guardar a consulta*. Passos a seguir:
- *Para o Application Insights*: portal obrigatória para análise, validar a consulta e seus resultados. Em seguida, guarde com o nome exclusivo para o *consultas compartilhadas*.
- *Para o Log Analytics*: obrigatória para pesquisa de registos, validar a consulta e seus resultados. Em seguida, utilize Guardar com o nome exclusivo para qualquer categoria.

Em seguida, quando [criar um alerta de registo nos alertas ](monitor-alerts-unified-usage.md), verá a consulta guardada listada como o tipo de sinal **Log (consulta guardada)**; conforme ilustrado no exemplo abaixo: ![consulta guardada, importado para alertas](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Usando **Log (consulta guardada)** resulta numa importação para alertas. Por conseguinte, quaisquer alterações efetuadas depois no Analytics não será reflexiva em regras de alerta de pesquisa de registo e vice-versa.

### <a name="rest-apis"></a>APIs REST
As APIs fornecidas para o registo de alertas são RESTful e podem ser acedidos através da API de REST do Azure Resource Manager. Por conseguinte, podem ser acedidos através do PowerShell, também outras opções para tirar partido das APIs.

Para obter detalhes, bem como exemplos de como usar a REST API, por gentileza consulte:
- [API de análise de REST de alerta de registo](../log-analytics/log-analytics-api-alerts.md) - para criar e gerir regras de alerta de pesquisa de registo para o Azure Log Analytics
- [O Azure Monitor agendada regras REST API de consulta](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) - para criar e gerir regras de alerta de pesquisa de registo para o Azure Application Insights

### <a name="azure-resource-manager-template"></a>Modelo do Azure Resource Manager
Os utilizadores também podem utilizar a flexibilidade proporcionada pela [do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) para criar e atualizar recursos - para criar ou atualizar os alertas de registo.

Para obter detalhes, bem como exemplos sobre como utilizar modelos do Resource Manager, por gentileza consulte:
- [Guardar a gestão de pesquisa e alertas](monitor-alerts-unified-log-template.md#managing-log-alert-on-log-analytics) para alertas de registo com base no Azure Log Analytics
- [Agendado a regra de consulta](monitor-alerts-unified-log-template.md#managing-log-alert-on-application-insights) para alertas de registo com base no Azure Application Insights
 

## <a name="next-steps"></a>Passos Seguintes
* Compreender [webhooks em alertas de registo no Azure](monitor-alerts-unified-log-webhook.md).
* Saiba mais sobre a nova [alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [Application Insights](../application-insights/app-insights-analytics.md).
* Saiba mais sobre [do Log Analytics](../log-analytics/log-analytics-overview.md).    
