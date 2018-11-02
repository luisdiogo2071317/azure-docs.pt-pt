---
title: Resolução de problemas de alertas de registo no Azure Monitor
description: Problemas comuns, erros e a resolução para o registo de alerta regras no Azure.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 0e1cb2cdd6270590def11479cc5859d996d84caa
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/01/2018
ms.locfileid: "50749056"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Resolução de problemas de alertas de registo no Azure Monitor  

## <a name="overview"></a>Descrição geral
Este artigo mostra-lhe lidar com problemas comuns vistos durante a configuração de alertas de registo dentro do Azure monitor. E fornecer a solução para perguntas freqüentes sobre a funcionalidade ou configuração de alertas de registo. O termo **alertas de registo** para descrever os alertas em que o sinal é a consulta personalizada com base nos [do Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) ou [Application Insights](../application-insights/app-insights-analytics.md). Saiba mais sobre funcionalidade, terminologia e tipos a partir [alertas - descrição geral de registo](monitor-alerts-unified-log.md).

> [!NOTE]
> Este artigo não considere casos quando a regra de alerta é mostrada como acionados no portal do Azure e a notificação através do grupo de ação associada (s). Para tais casos, consulte detalhes no artigo sobre [grupos de ação](monitoring-action-groups.md).


## <a name="log-alert-didnt-fire"></a>Alerta de registo não são acionados

Próxima detalhada são algumas razões comuns, por que um configurado [regra de alerta de registo no Azure Monitor](alert-log.md) não obter acionada quando visualizado no [alertas do Azure](monitoring-alerts-managing-alert-states.md), quando espera que sejam acionados. 

### <a name="data-ingestion-time-for-logs"></a>Tempo de ingestão de dados para os registos
Trabalha alerta de registo ao executar periodicamente a consulta pelo cliente, com base na [do Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) ou [Application Insights](../application-insights/app-insights-analytics.md). Ambos têm a tecnologia o poder da análise, que processa grandes quantidades de dados de registo e fornece uma funcionalidade no mesmo. Como o serviço Log Analytics envolve muitos terabytes de dados a partir de milhares de clientes e de origens variadas de processamento em todo o mundo – o serviço não está suscetível a tempo de atraso. Para obter mais informações, consulte [tempo de ingestão de dados no Log Analytics](../log-analytics/log-analytics-data-ingestion-time.md).

Para superar o atraso de ingestão de dados que pode ocorrer no Log Analytics ou registos do Application Insights; alerta de registo deve aguardar e tentar após algum tempo quando encontrar dados ainda não são ingeridos para o período de tempo de alerta. Alertas de registo tem um conjunto de tempo de espera aumentar exponencialmente, de modo a certificar-se de que podemos esperar que o tempo necessário para os dados ingeridos pelo Log Analytics. Por conseguinte, se os registos consultados por sua regra de alerta de registo são afetados pela atrasos de ingestão, em seguida, alerta de registo irá acionar apenas depois dos dados estão disponíveis na pós-ingestão de Log Analytics e após o intervalo de tempo exponencial devido ao serviço de alertas de registo repetir várias vezes interina .

### <a name="incorrect-time-period-configured"></a>Período de hora incorreto configurado
Conforme descrito no artigo [terminologia para alertas de registo](monitor-alerts-unified-log.md#log-search-alert-rule---definition-and-types), período de tempo indicado na configuração especifica o intervalo de tempo para a consulta. A consulta devolve apenas os registos que foram criados neste intervalo de tempo. Período de tempo restringe os dados obtidos para a consulta de registo evitar abusos e evita qualquer comando de tempo (como há) utilizado numa consulta de registo. 
*Por exemplo, se o período de tempo é definido como 60 minutos, e a consulta é executada em 1 às 15:15, apenas os registos criados entre 12:15 e 1 às 15:15 é devolvido ao executar a consulta de registo. Agora, se a consulta de registo utiliza o comando como atrás de tempo (1 dia), a consulta de registo deve ser executada apenas para dados entre 12:15 e 1 às 15:15 - como se existem dados para apenas os últimos 60 minutos. E não durante sete dias de dados conforme especificado na consulta de registo.*

Com base na sua lógica de consulta, verifique se o período de tempo adequado na configuração foi fornecido. Por exemplo, mencionado anteriormente, se o registo de consultar utiliza há (1 dia), conforme mostrado com marcador verde -, em seguida, o período de tempo deve ser definido para 24 horas ou 1440 minutos (como indicado em vermelho) garantir que a consulta fornecida executa corretamente como envisaged.
    ![Período de tempo](./media/monitor-alerts-unified/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Suprimir alertas a opção estiver definida
Conforme descrito no passo 8 deste artigo [criar uma regra de alerta de registo no portal do Azure](alert-log.md#managing-log-alerts-from-the-azure-portal), alerta de registo fornece uma opção Configurar automática supressão da regra de alerta e impedir que a notificação/acionador estipulado período de tempo. Opção de alertas de suppress fará com que o alerta de registo executar ao mesmo tempo que não o acionamento do grupo de ação para o período de tempo especificado na **suprimir alertas** opção e, por conseguinte, o utilizador pode parecer esse alerta não são disparados enquanto na realidade foi suprimida, conforme configurado .
    ![Suprimir alertas](./media/monitor-alerts-unified/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Regra de alerta de medida da métrica está incorreta
Tipo de medida da métrica de regra de alerta de registo é o subtipo de alertas de registo, que têm capacidades especiais, mas por sua vez emprega restrição sobre a sintaxe de consulta de alerta. Alerta de registo de medida da métrica regra requer a saída da consulta de alerta para fornecer uma série de métrica de tempo - uma tabela com períodos de tempo de tamanho igual distintos, juntamente com valores correspondentes de AggregatedValue calculadas. Além disso, os utilizadores podem optar por ter nas variáveis adicionais tabela juntamente com AggregatedValue como computador, o nó, etc. com que os dados na tabela pode ser ordenado.

Por exemplo, suponha que regra de alerta de registo de medida da métrica foi configurada como:
- consulta era: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- período de tempo de seis horas
- limiar de 50
- lógica de alerta de falhas consecutivas três
- Agregação após escolhido como $table

Agora, uma vez que o comando, usamos resuma... por e fornecidas duas variáveis: timestamp & $table; alerta de serviço irá escolher $table para "Agregado com" - basicamente ordenar a tabela de resultados ao campo: $table - conforme mostrado abaixo e, em seguida, examinar a vários AggregatedValue para cada tabela tipo (como availabilityResults), para ver se haveria falhas consecutivas de 3 ou mais.

   ![Execução da consulta medida métrica com vários valores](./media/monitor-alerts-unified/LogMMQuery.png)

Como "Agregadas após" é $table – os dados são ordenados na coluna de $table (como em vermelho); em seguida, podemos de grupo e procurar por tipos de campo "Agregado com" (ou seja) $table – por exemplo: os valores do availabilityResults será considerado como um desenho/entidade (como destacado na cor de laranja). Nesta plotagem de valor/entidade – Serviço de alertas verifica a existência de falhas consecutivas três ocorrendo (como mostra a verde) para a alerta irá obter acionada para o valor de tabela "availabilityResults". Da mesma forma, se em qualquer outro valor de $table se são visualizadas três falhas consecutivas - outra notificação de alertas será acionada para o mesmo; com o serviço de alertas classificar automaticamente os valores num desenho/entidade (como cor de laranja) por vez.

Agora suponha que, medida da métrica regra de alerta de registo foi modificada e a consulta foi `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)` com o restante da configuração restante mesmo que antes de incluir a lógica de alerta de falhas consecutivas três. Opção "Aggregate após" nesse caso será por predefinição: timestamp. Uma vez que apenas um valor é fornecido na consulta para resumir... por timestamp (ou seja) semelhante ao exemplo anterior, no final da execução a saída seria conforme ilustrado abaixo. 

   ![Métrica execução de consultas de medição com o valor único](./media/monitor-alerts-unified/LogMMtimestamp.png)

Como "Agregadas após" é timestamp – os dados são ordenados na coluna timestamp (como em vermelho); em seguida, podemos agrupar por timestamp – por exemplo: os valores de `2018-10-17T06:00:00Z` será considerado como um desenho/entidade (como destacado na cor de laranja). Este valor plotagem/entidade – Serviço de alertas encontrará sem falhas consecutivas que (como a cada valor de timestamp tem apenas uma entrada) e, por conseguinte, alertas nunca irão obter ativadas. Portanto, nesse caso, utilizador ou - tem
- Adicionar uma variável fictícia ou uma variável existente (como $table) a corretamente classificação concluído com o campo "Aggregate após" configurado
- (Ou) reconfigurar a regra de alerta para utilizar a lógica de alerta com base na *total de violação* em vez disso adequadamente
 
## <a name="log-alert-fired-unnecessarily"></a>Alerta de registo acionado desnecessariamente
Próxima detalhada são algumas razões comuns, por que um configurado [regra de alerta de registo no Azure Monitor](alert-log.md) podem ser acionadas quando visualizado no [alertas do Azure](monitoring-alerts-managing-alert-states.md), quando não espera que sejam acionados.

### <a name="alert-triggered-by-partial-data"></a>Alerta acionada por dados parciais
Análise que alimenta o Log Analytics e Application Insights está sujeitos aos atrasos de ingestão e de processamento; devido a que, ao tempo quando é executada a consulta de alerta de registo fornecida - pode haver um caso de dados não está disponível ou apenas alguns dados estejam disponíveis. Para obter mais informações, consulte [tempo de ingestão de dados no Log Analytics](../log-analytics/log-analytics-data-ingestion-time.md).

Dependendo de como a regra de alerta é configurada, pode haver enganássemos firing caso haja nenhum ou apenas dados parciais nos registos no momento da execução de alerta. Nesses casos, recomenda-se que a configuração ou de consulta de alerta é alterada. *Por exemplo, se o registo de regra do alerta está configurada para acionar quando o número de resultados da consulta de análise é inferior a 5; (Digamos) em seguida, quando não existe (zero registo) de dados ou resultados parciais (um registo) obter acionada a regra de alerta. Onde-como após atraso de ingestão, quando mesma consulta é executada no Analytics a consulta com dados completos pode fornecer os resultados como 10 registos.*

### <a name="alert-query-output-misunderstood"></a>Saída de consulta de alerta mal compreendida
Para os alertas de registo a lógica para alertar é fornecida pelo usuário por meio de consulta do analytics. A consulta de análise fornecida pode ser empregadas várias funções matemáticas e grandes quantidades de dados para criar construções específicas. O serviço de alerta executará a consulta do cliente fornecido em intervalos especificados com os dados para o período de tempo especificado; alertas de serviço faz sutil é alterado para consulta fornecido – com base na escolhido o tipo de alerta e o mesmo pode ser testemunhado na secção "Consultar a ser executado" no ecrã de lógica de sinal de configurar, conforme ilustrado abaixo: ![consulta a ser executado](./media/monitor-alerts-unified/LogAlertPreview.png)
 
O que é mostrado na **consulta a ser executado** secção destina-se o registo de alerta service será executado; o utilizador pode executar a consulta declarada como timespan através de [Analytics portal](../log-analytics/log-analytics-log-search-portals.md) ou [API de análise de](https://docs.microsoft.com/en-us/rest/api/loganalytics/) -Se quer saber antes da criação do alerta, que resultado de consulta de alerta pode ser.
 
## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre [alertas de registo nos alertas do Azure](monitor-alerts-unified-log.md)
* Saiba mais sobre [Application Insights](../application-insights/app-insights-analytics.md)
* Saiba mais sobre [do Log Analytics](../log-analytics/log-analytics-overview.md). 