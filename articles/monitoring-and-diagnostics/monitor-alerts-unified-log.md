---
title: Registar alertas no Monitor do Azure - alertas | Microsoft Docs
description: Acionar mensagens de correio eletrónico, notificações, é possível chamar URLs de Web sites (webhooks) ou automatização quando são satisfeitas as condições de consulta de análise que especificar para alertas do Azure.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 175e512d0bdaa84d5251f4bbdb09aed3aed436f9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34638726"
---
# <a name="log-alerts-in-azure-monitor---alerts"></a>Registo de alertas no Monitor do Azure - alertas 
Este artigo fornece detalhes de alertas de registo são um dos tipos de alertas suportados no novo [alertas do Azure](monitoring-overview-unified-alerts.md) e permitir que os utilizadores utilizem plataforma de análise do Azure como base para alertas... Para obter detalhes de alertas de métrica utilizando os registos, consulte [quase em Tempo Real métrica alertas](monitoring-near-real-time-metric-alerts.md)


Alerta de registo é composta por regras de pesquisa de registo criadas para [Log Analytics do Azure](../log-analytics/log-analytics-tutorial-viewdata.md) ou [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events)


## <a name="log-search-alert-rule---definition-and-types"></a>Registo pesquisa regra de alerta - definições e de tipos

Procurar regras de registo são criadas através de alertas do Azure para executar consultas de registo especificado automaticamente em intervalos regulares.  Se os resultados da consulta registo corresponderem aos critérios específicos, em seguida, é criado um registo de alerta. A regra, em seguida, pode executar automaticamente um ou mais ações utilizando [ação grupos](monitoring-action-groups.md). 

Procurar regras de registo são definidas pelos seguintes detalhes:
- **Inicie sessão consulta**.  A consulta que é executada sempre que a regra de alerta é acionado.  Os registos devolvidos por esta consulta são utilizados para determinar se é criado um alerta. *Azure Application Insights* consulta também pode incluir [chamadas entre aplicações](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery), desde que o utilizador tem direitos de acesso as aplicações externas. 

    > [!IMPORTANT]
    > Suppport de [cruzada consulta de aplicação para o Application Insights](https://dev.applicationinsights.io/ai/documentation/2-Using-the-API/CrossResourceQuery) está em pré-visualização - a funcionalidade limitada para utilizar com 2 ou mais aplicações e experiência de utilizador está sujeita a alterações. Utilização de [cruzada área de trabalho consulta](https://dev.loganalytics.io/oms/documentation/3-Using-the-API/CrossResourceQuery) e [entre recursos consulta de análise de registos](../log-analytics/log-analytics-cross-workspace-search.md) está atualmente **não suportado** nos alertas do Azure.

- **Período de tempo**.  Especifica o intervalo de tempo para a consulta. A consulta devolve apenas os registos que foram criados neste intervalo da hora atual. Período de tempo restringe os dados alcançados para a consulta de registo impedir que abuso e evita a qualquer comando de tempo (como há) utilizado numa consulta do registo. <br>*Por exemplo, se o período de tempo é definido como 60 minutos e a consulta é executada em 1:15 PM, apenas os registos criados entre 12:15 PM e 1:15 PM é devolvido para executar a consulta de registo. Agora, se a consulta de registo utiliza a hora de comando como há (7d), a registo seria possível executar a consulta apenas para dados entre 12:15 PM e 1:15 PM - como se existem dados para apenas os últimos 60 minutos. E não para dados conforme especificado na consulta de registo de sete dias.*
- **Frequência**.  Especifica a frequência com que a consulta deve ser executada. Pode ser qualquer valor entre 5 minutos e 24 horas. Deve ser igual ou menor do que o período de tempo.  Se o valor é maior do que o período de tempo, em seguida, o risco de registos que está a ser omitidos.<br>*Por exemplo, considere um período de tempo de 30 minutos e uma frequência de 60 minutos.  Se a consulta é executada em 1:00, devolve registos entre 12:30 e 1:00 PM.  Da próxima vez que executar a consulta é 2:00 quando devolvam registos entre 1:30 e 2:00.  Nunca deverá ser avaliados de quaisquer registos criados entre 1:00 e 1:30.*
- **Limiar**.  Os resultados da pesquisa de registo são avaliados para determinar se deve ser criado um alerta.  O limiar é diferente para os diferentes tipos de regras de alerta de pesquisa de registo.

Procurar regras de registo seja para [Log Analytics do Azure](../log-analytics/log-analytics-tutorial-viewdata.md) ou [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events), podem ser de dois tipos. Cada um destes tipos é descrita detalhadamente nas secções que se seguem.

- **[Número de resultados](#number-of-results-alert-rules)**. Alerta único criada quando os registos de número devolvidos pela pesquisa de registo excedem um número especificado.
- **[Medida de métrica](#metric-measurement-alert-rules)**.  Alerta criada para cada objeto no resultado da pesquisa de registo com valores que excedem o limiar especificado.

As diferenças entre os tipos de regra de alerta são os seguintes.

- *Número de resultados* regras de alerta cria sempre um tempo de alerta, único *medida métrica* regra de alerta cria um alerta para cada objeto que é superior ao limiar.
- *Número de resultados* regras de alerta criar um alerta quando o limiar for excedido uma única vez. *Medida de métrica* regras de alertas podem criar um alerta quando o limiar for excedido um determinado número de vezes ao longo de um intervalo de tempo específico.

### <a name="number-of-results-alert-rules"></a>Número de regras de alerta de resultados
**Número de resultados** regras de alerta criar um único alerta quando o número de registos devolvidos pela consulta pesquisa excede o limiar especificado. Este tipo de regra de alerta é ideal para trabalhar com eventos como registos de eventos do Windows, o Syslog, WebApp resposta e registos personalizados.  Poderá pretender criar um alerta quando um evento de erro específico é criado ou quando são criados vários eventos de erro durante um período de tempo específico.

**Limiar**: O limiar para um número de regras de alerta de resultados for maior ou menor do que um valor específico.  Se o número de registos devolvidos pela pesquisa de registo corresponde este critério, é criado um alerta.

Alerta sobre um único evento, defina o número de resultados para maior que 0 e procurar a ocorrência de um único evento que foi criada desde a última vez que a consulta foi executada. Algumas aplicações podem iniciar um erro de ocasional necessariamente não deve emitir um alerta.  Por exemplo, a aplicação pode repetir o processo que criou o evento de erro e, em seguida, êxito da próxima vez.  Neste caso, não poderá criar o alerta, a menos que são criados vários eventos durante um período de tempo específico.  

Em alguns casos, poderá pretender criar um alerta na ausência de um evento.  Por exemplo, um processo pode registar eventos regulares para indicar que está a funcionar corretamente.  Se não registar um estes eventos num período de tempo específico, deverá ser criado um alerta.  Neste caso, deverá definir o limiar **inferior a 1**.

#### <a name="example"></a>Exemplo
Considere um cenário em que pretende saber quando a aplicação baseada na web fornece uma resposta a utilizadores com o código de 500 (ou seja) erro interno do servidor. Poderá criar uma regra de alerta com os seguintes detalhes:  
- **Consulta:** pedidos | onde resultCode = = "500"<br>
- **Período de tempo:** 30 minutos<br>
- **Frequência de alerta:** cinco minutos<br>
- **Valor de limiar:** excelente que 0<br>

Em seguida, alerta iria executar a consulta a cada 5 minutos, com 30 minutos de dados - procurar qualquer registo em que o código de resultado foi 500. Se for encontrado um até esse registo, é acionado o alerta e aciona uma ação configurada.

### <a name="metric-measurement-alert-rules"></a>Regras de alerta de métrica de medida

- **Medida de métrica** regras de alerta criar um alerta para cada objeto numa consulta com um valor que excede um limiar especificado.  Têm as seguintes diferenças distintas de **número de resultados** regras de alertas.
- **A função de agregação**: determina o cálculo que é executado e potencialmente uma numérica campo a agregar.  Por exemplo, **existente** devolve o número de registos da consulta, **avg(CounterValue)** devolve a média do campo CounterValue ao longo do intervalo. Função de agregação na consulta tem de ser chamado/com o nome: AggregatedValue e forneça um valor numérico. 
- **Campo de grupo**: é criado um registo com um valor de agregados para cada instância deste campo e pode ser gerado um alerta para cada.  Por exemplo, se pretendesse gerar um alerta para cada computador, teria de utilizar **por computador** 

    > [!NOTE]
    > Para a medida métrica regras de alertas que são baseadas no Application Insights, pode especificar o campo para agrupar os dados. Para tal, utilize o **agregado no** opção na definição da regra.   
    
- **Intervalo**: define o intervalo de tempo durante o qual os dados são agregados.  Por exemplo, se tiver especificado **cinco minutos**, seria possível criar um registo para cada instância do campo Grupo agregada em intervalos de 5 minutos durante o período de tempo especificado para o alerta.

    > [!NOTE]
    > A função de Reciclagem tem de ser utilizada na consulta para especificar o intervalo. Como bin() pode resultar em intervalos de tempo unequal - alerta será convertido automaticamente bin comando ao comando bin_at com momento adequado em runtime, para garantir resultados com um ponto de fixo
    
- **Limiar**: O limiar para regras de alerta de métrica de medida é definido por um valor de agregação e um número de violações.  Se qualquer ponto de dados na pesquisa de registo exceder este valor, tem considerado uma violação.  Se o número de falhas para qualquer objeto no resultado excede o valor especificado, é criado um alerta para esse objeto.

#### <a name="example"></a>Exemplo
Considere um cenário em que pretendia um alerta se a qualquer computador excedeu a utilização do processador de 90% três vezes mais de 30 minutos.  Poderá criar uma regra de alerta com os seguintes detalhes:  

- **Consulta:** desempenho | onde ObjectName = = "Processador" e CounterName = = "% de tempo do processador" | resumir AggregatedValue = avg(CounterValue) por bin (TimeGenerated, 5 m), do computador<br>
- **Período de tempo:** 30 minutos<br>
- **Frequência de alerta:** cinco minutos<br>
- **Valor de agregação:** excelente a 90<br>
- **Alerta de Acionador com base na:** Total de falhas maior que 2<br>

A consulta criaria um valor médio para cada computador em intervalos de 5 minutos.  Esta consulta iria ser executada a cada 5 minutos para os dados recolhidos através de 30 minutos anteriores.  Dados de exemplo são mostrados abaixo para três computadores.

![Resultados da consulta de exemplo](./media/monitor-alerts-unified/metrics-measurement-sample-graph.png)

Neste exemplo, seriam possível criar alertas separadas para srv02 e srv03, uma vez que estes infringido o limiar de 90% 3 vezes durante o período de tempo.  Se o **alerta acionador com base na:** foram alterados para **Consecutive** , em seguida, seria possível criar um alerta apenas para srv03, uma vez que infringido-o limiar para três amostras consecutivas.


## <a name="log-search-alert-rule---creation-and-modification"></a>Registo pesquisa regra de alerta - criação e modificação

Alerta de registo, bem como o respetiva consisting registo pesquisa a regra de alerta pode ser visualizada, criada ou modificada a partir de:
- Portal do Azure
- RESTO das APIs (incluindo através do PowerShell)
- Modelos Azure Resource Manager

### <a name="azure-portal"></a>Portal do Azure
Desde a introdução do [novos alertas do Azure](monitoring-overview-unified-alerts.md), agora, os utilizadores podem gerir todos os tipos de alertas no portal do Azure a partir de uma única localização e passos semelhantes. Saiba mais sobre [utilizando novos alertas de Azure](monitor-alerts-unified-usage.md).

Além disso, os utilizadores podem perfect as consultas na plataforma de análise escolhidas no Azure e, em seguida, *importá-los para utilização nos alertas ao guardar a consulta*. Passos a seguir:
- *Para o Application Insights*: portal da análise de preferida, validar a consulta e os respetivos resultados. Em seguida, guarde com nome exclusivo para *consultas partilhadas*.
- *Para análise de registos*: aceda a pesquisa de registo, validar a consulta e os respetivos resultados. Utilize, em seguida, guarde com nome exclusivo para qualquer categoria.

Em seguida, quando [criar um alerta de registo em alertas ](monitor-alerts-unified-usage.md), consulte consulta guardada listada como tipo de sinal **registo (consulta guardada)**; conforme ilustrado no exemplo abaixo: ![consulta guardada importado para alertas](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Utilizar **registo (consulta guardada)** resulta na importação a alertas. Por conseguinte, quaisquer alterações efetuadas após em análise não serão possível reflective em regras de alerta de pesquisa de registo e vice-versa.

### <a name="rest-apis"></a>APIs REST
APIs fornecido para o registo de alertas são RESTful e podem ser acedidos através da API de REST do Azure Resource Manager. Por conseguinte, pode ser acedido através do PowerShell, bem como outras opções de tirar partido das APIs.

Para obter detalhes, bem como exemplos sobre como utilizar a REST API, consulte:
- [API de REST de alerta de análise de registo](../log-analytics/log-analytics-api-alerts.md) - para criar e gerir regras de alerta de pesquisa de registo para análise de registos do Azure
- [Azure Monitor agendada consulta regras de API REST](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) - para criar e gerir regras de alerta de pesquisa de registo para o Azure Application Insights

### <a name="azure-resource-manager-template"></a>Modelo do Azure Resource Manager
Os utilizadores também podem utilizar a flexibilidade fornecida pelo [do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) para criar e atualizar recursos - para criar ou atualizar os alertas de registo.

Para obter detalhes, bem como exemplos sobre como utilizar os modelos do Resource Manager, consulte:
- [Guardar pesquisa e alertas gestão](monitor-alerts-unified-log-template.md#managing-log-alert-on-log-analytics) para alertas de registo com base na análise de registos do Azure
- [Agendar a regra de consulta](monitor-alerts-unified-log-template.md#managing-log-alert-on-application-insights) para alertas de registo com base no Azure Application Insights
 

## <a name="next-steps"></a>Passos Seguintes
* Compreender [registar alertas no Azure](monitor-alerts-unified-log-webhook.md).
* Saiba mais sobre a nova [alertas do Azure](monitoring-overview-unified-alerts.md).
* Saiba mais sobre [Application Insights](../application-insights/app-insights-analytics.md).
* Saiba mais sobre [Log Analytics](../log-analytics/log-analytics-overview.md).    
