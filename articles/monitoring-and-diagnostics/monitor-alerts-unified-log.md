---
title: "Registo de alertas no Monitor do Azure - alertas (pré-visualização) | Microsoft Docs"
description: "Acionar mensagens de correio eletrónico, notificações, é possível chamar URLs de Web sites (webhooks) ou automatização quando são satisfeitas as condições de consulta complexas que especificar para alertas do Azure (pré-visualização)."
author: msvijayn
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: f7457655-ced6-4102-a9dd-7ddf2265c0e2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2018
ms.author: vinagara
ms.openlocfilehash: 438776e7f0885dbdb0d66ccdd18d854e14beb299
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="log-alerts-in-azure-monitor---alerts-preview"></a>Registo de alertas no Monitor do Azure - alertas (pré-visualização)
Este artigo fornece detalhes de regras de alerta como em projetos de consultas de análise nos alertas do Azure (pré-visualização) e descreve as diferenças entre os diferentes tipos de regras de alerta de registo.

Atualmente alertas do Azure (pré-visualização), suporta registar alertas em consultas de [Log Analytics do Azure](../log-analytics/log-analytics-tutorial-viewdata.md) e [Application Insights](../application-insights/app-insights-cloudservices.md#view-azure-diagnostic-events).

> [!WARNING]

> Atualmente, o alerta de registo nos alertas do Azure (pré-visualização) não suporta consultas de área de trabalho em vários locais ou em várias aplicações.

Além disso, os utilizadores podem perfect as consultas na plataforma de análise escolhidas no Azure e, em seguida, *importá-los para utilização nos alertas (pré-visualização) ao guardar a consulta*. Passos a seguir:
- Para o Application Insights: Portal da análise de preferida, validar a consulta e os respetivos resultados. Em seguida, guarde com nome exclusivo para *consultas partilhadas*.
- Para análise de registos: Aceda a pesquisa de registo, validar a consulta e os respetivos resultados. Utilize, em seguida, guarde com nome exclusivo para qualquer categoria.

Em seguida, quando [criar um alerta de registo em alertas (pré-visualização)](monitor-alerts-unified-usage.md), consulte consulta guardada listada como tipo de sinal **registo (consulta guardada)**; conforme ilustrado no exemplo abaixo: ![consulta guardada importado para alertas](./media/monitor-alerts-unified/AlertsPreviewResourceSelectionLog-new.png)

> [!NOTE]
> Utilizar **registo (consulta guardada)** resulta na importação a alertas. Por conseguinte, quaisquer alterações efetuadas após em análise não serão possível reflective em regras de alerta guardadas e vice-versa.

## <a name="log-alert-rules"></a>Regras de alerta de registo

Os alertas são criados pelo alertas do Azure (pré-visualização) automaticamente executar consultas de registo em intervalos regulares.  Se os resultados da consulta registo corresponderem aos critérios específicos, em seguida, é criado um registo de alerta. A regra, em seguida, pode executar automaticamente uma ou mais ações para notificá-lo do alerta ou da invocação do outro processo, como o envio de dados para aplicações externas utilizando proativamente [webhook baseadas em json](monitor-alerts-unified-log-webhook.md)com [ação grupos](monitoring-action-groups.md). Diferentes tipos de regras de alerta utilizam lógica diferente para efetuar esta análise.

Regras de alerta são definidas pelos seguintes detalhes:

- **Inicie sessão consulta**.  A consulta que é executada sempre que a regra de alerta é acionado.  Os registos devolvidos por esta consulta são utilizados para determinar se é criado um alerta.
- **Janela de tempo**.  Especifica o intervalo de tempo para a consulta.  A consulta devolve apenas os registos que foram criados dentro deste intervalo da hora atual.  Janela de tempo pode ser qualquer valor entre 5 minutos e 1440 minutos ou 24 horas. Por exemplo, se a janela de tempo é definida como 60 minutos e a consulta é executada em 1:15 PM, é devolvido apenas os registos criados entre 12:15 PM e 1:15 PM.
- **Frequência**.  Especifica a frequência com que a consulta deve ser executada. Pode ser qualquer valor entre 5 minutos e 24 horas. Deve ser igual ou menor do que a janela de tempo.  Se o valor é maior do que a janela de tempo, em seguida, o risco de registos que está a ser omitidos.<br>Por exemplo, considere uma janela de tempo de 30 minutos e uma frequência de 60 minutos.  Se a consulta é executada em 1:00, devolve registos entre 12:30 e 1:00 PM.  Da próxima vez que executar a consulta é 2:00 quando devolvam registos entre 1:30 e 2:00.  Nunca deverá ser avaliados de quaisquer registos criados entre 1:00 e 1:30.
- **Threshold**.  Os resultados da pesquisa de registo são avaliados para determinar se deve ser criado um alerta.  O limiar é diferente para os diferentes tipos de regras de alertas.

Cada regra de alerta no Log Analytics é um dos dois tipos.  Cada um destes tipos é descrita detalhadamente nas secções que se seguem.

- **[Número de resultados](#number-of-results-alert-rules)**. Alerta único criada quando os registos de número devolvidos pela pesquisa de registo excedem um número especificado.
- **[Medida de métrica](#metric-measurement-alert-rules)**.  Alerta criada para cada objeto no resultado da pesquisa de registo com valores que excedem o limiar especificado.

As diferenças entre os tipos de regra de alerta são os seguintes.

- * * Número de regras de alerta de resultados cria sempre um tempo de alerta único **medida métrica** regra de alerta cria um alerta para cada objeto que é superior ao limiar.
- **Número de resultados** regras de alerta criar um alerta quando o limiar for excedido uma única vez. **Medida de métrica** regras de alertas podem criar um alerta quando o limiar for excedido um determinado número de vezes ao longo de um intervalo de tempo específico.

## <a name="number-of-results-alert-rules"></a>Número de regras de alerta de resultados
**Número de resultados** regras de alerta criar um único alerta quando o número de registos devolvidos pela consulta pesquisa excede o limiar especificado. Este tipo de regra de alerta é ideal para trabalhar com eventos como registos de eventos do Windows, o Syslog, WebApp resposta e registos personalizados.  Poderá pretender criar um alerta quando um evento de erro específico é criado ou quando são criados vários eventos de erro dentro de uma janela de tempo específico.

**Limiar**: O limiar para um * * número de regras de alerta de resultados é maior ou menor do que um valor específico.  Se o número de registos devolvidos pela pesquisa de registo corresponde este critério, é criado um alerta.

Alerta sobre um único evento, defina o número de resultados para maior que 0 e procurar a ocorrência de um único evento que foi criada desde a última vez que a consulta foi executada. Algumas aplicações podem iniciar um erro de ocasional necessariamente não deve emitir um alerta.  Por exemplo, a aplicação pode repetir o processo que criou o evento de erro e, em seguida, êxito da próxima vez.  Neste caso, não poderá criar o alerta, a menos que são criados vários eventos dentro de uma janela de tempo específico.  

Em alguns casos, poderá pretender criar um alerta na ausência de um evento.  Por exemplo, um processo pode registar eventos regulares para indicar que está a funcionar corretamente.  Se não registar um estes eventos dentro de uma janela de tempo específico, deverá ser criado um alerta.  Neste caso, deverá definir o limiar **inferior a 1**.

### <a name="example"></a>Exemplo
Considere um cenário em que pretende saber quando a aplicação baseada na web fornece uma resposta a utilizadores com o código de 500 (ou seja) erro interno do servidor. Poderá criar uma regra de alerta com os seguintes detalhes:  
**Consulta:** pedidos | onde resultCode = = "500"<br>
**Janela de tempo:** 30 minutos<br>
**Frequência de alerta:** cinco minutos<br>
**Valor de limiar:** excelente que 0<br>

Em seguida, alerta iria executar a consulta a cada 5 minutos, com 30 minutos de dados - procurar qualquer registo em que o código de resultado foi 500. Se for encontrado um até esse registo, este é acionado o alerta e o acionador de ação configurada.

## <a name="metric-measurement-alert-rules"></a>Regras de alerta de métrica de medida

**Medida de métrica** regras de alerta criar um alerta para cada objeto numa consulta com um valor que excede um limiar especificado.  Têm as seguintes diferenças distintas de **número de resultados** regras de alertas.

**A função de agregação**: determina o cálculo que é executado e potencialmente uma numérica campo a agregar.  Por exemplo, **existente** devolve o número de registos da consulta, **avg(CounterValue)** devolve a média do campo CounterValue ao longo do intervalo.

> [!NOTE]

> Função de agregação na consulta tem de ser chamado/com o nome: AggregatedValue e forneça um valor numérico. 


**Campo de grupo**: é criado um registo com um valor de agregados para cada instância deste campo e pode ser gerado um alerta para cada.  Por exemplo, se pretendesse gerar um alerta para cada computador, teria de utilizar **por computador**   

> [!NOTE]

> Para a medida métrica regras de alertas que são baseadas no Application Insights, pode especificar o campo para agrupar os dados. Para tal, utilize o **agregado no** opção na definição da regra.   

**Intervalo**: define o intervalo de tempo durante o qual os dados são agregados.  Por exemplo, se tiver especificado **cinco minutos**, seria possível criar um registo para cada instância do campo Grupo agregada em intervalos de 5 minutos durante a janela de tempo especificada para o alerta.
> [!NOTE]
> Função de Reciclagem tem de ser utilizada na consulta. Também se intervalos de tempo unequal são produzidos de janela de tempo através da utilização da função de reciclagem - alerta em vez disso, utilizará bin_at função em vez disso, para garantir que não há um ponto de fixo

**Limiar**: O limiar para regras de alerta de métrica de medida é definido por um valor de agregação e um número de violações.  Se qualquer ponto de dados na pesquisa de registo exceder este valor, tem considerado uma violação.  Se o número de falhas para qualquer objeto no resultado excede o valor especificado, é criado um alerta para esse objeto.

#### <a name="example"></a>Exemplo
Considere um cenário em que pretendia um alerta se a qualquer computador excedeu a utilização do processador de 90% três vezes mais de 30 minutos.  Poderá criar uma regra de alerta com os seguintes detalhes:  

**Consulta:** desempenho | onde ObjectName = = "Processador" e CounterName = = "% de tempo do processador" | resumir AggregatedValue = avg(CounterValue) por bin (TimeGenerated, 5 m), do computador<br>
**Janela de tempo:** 30 minutos<br>
**Frequência de alerta:** cinco minutos<br>
**Valor de agregação:** excelente a 90<br>
**Alerta de Acionador com base na:** Total de falhas maior 5<br>

A consulta criaria um valor médio para cada computador em intervalos de 5 minutos.  Esta consulta iria ser executada a cada 5 minutos para os dados recolhidos através de 30 minutos anteriores.  Dados de exemplo são mostrados abaixo para três computadores.

![Resultados da consulta de exemplo](../log-analytics/media/log-analytics-alerts/metrics-measurement-sample-graph.png)

Neste exemplo, seriam possível criar alertas separadas para srv02 e srv03, uma vez que estes infringido o limiar de 90% 3 vezes durante a janela de tempo.  Se o **alerta acionador com base na:** foram alterados para **Consecutive** , em seguida, seria possível criar um alerta apenas para srv03, uma vez que infringido-o limiar para três amostras consecutivas.


## <a name="next-steps"></a>Passos Seguintes
* Compreender [ações de Webhook para alertas de registo](monitor-alerts-unified-log-webhook.md)
* [Obter uma descrição geral dos alertas do Azure (pré-visualização)](monitoring-overview-unified-alerts.md)
* Saiba mais sobre [através de alertas do Azure (pré-visualização)](monitor-alerts-unified-usage.md)
* Saiba mais sobre [Application Insights](../application-insights/app-insights-analytics.md)
* Saiba mais sobre [Log Analytics](../log-analytics/log-analytics-overview.md).    
