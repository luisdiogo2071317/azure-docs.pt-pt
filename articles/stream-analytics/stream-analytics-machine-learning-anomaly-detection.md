---
title: Deteção de anomalias no Azure Stream Analytics (pré-visualização)
description: Este artigo descreve como utilizar o Azure Stream Analytics e o Azure Machine Learning em conjunto, para detetar anomalias.
services: stream-analytics
author: dubansal
ms.author: dubansal
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/09/2018
ms.openlocfilehash: e7274e4507d901a209ed5832e98ca630feefda4f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Deteção de anomalias do Azure Stream Analytics

> [!IMPORTANT]
> Esta funcionalidade está em pré-visualização, não recomendamos com cargas de trabalho de produção.

O **AnomalyDetection** operador é utilizado para detetar diferentes tipos de anomalias em fluxos de eventos. Por exemplo, um decréscimo lento na memória livre num período de tempo tempo pode ser indicativa de uma fuga de memória ou o número de pedidos de serviço web que são estável num intervalo poderá aumentar ou diminuir significativamente.  

O operador de AnomalyDetection Deteta três tipos de anomalias: 

* **Alteração do nível de bidirecional**: uma constante de aumentar ou diminuir o nível de valores, ascendente e descendente. Este valor é diferente do picos e dips, que são as alterações instantânea ou curta duração.  

* **Lenta tendência positivo**: um aumento lento a tendência ao longo do tempo.  

* **Lenta tendência negativo**: um decréscimo lento na tendência ao longo do tempo.  

Quando utilizar o operador de AnomalyDetection, tem de especificar o **Limit Duration** cláusula. Esta cláusula Especifica que o intervalo de tempo (até que ponto anterior no histórico do evento atual do) deve ser considerado ao detetar anomalias. Este operador, opcionalmente, pode ser limitado para apenas os eventos que correspondem uma propriedade ou determinadas condições utilizando os **quando** cláusula. Este operador também, opcionalmente, pode processar grupos de eventos em separado com base na chave especificada no **partição por** cláusula. Formação e predição independentemente ocorrerem para cada partição. 

## <a name="syntax-for-anomalydetection-operator"></a>Sintaxe para o operador de AnomalyDetection

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 

**Exemplo de utilização**  

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`

### <a name="arguments"></a>Argumentos

* **scalar_expression** -a expressão escalar durante o qual a deteção de anomalias é executada. Valores permitidos para este parâmetro incluem o número de vírgula flutuante ou de dados Bigint tipos de retorno ou um único () valor escalar. A expressão com carateres universais **\*** não é permitida. Expressão escalar não pode conter outras funções analíticas ou funções externas. 

* **partition_by_clause** - `PARTITION BY <partition key>` cláusula divide a aprendizagem e a formação em partições separadas. Por outras palavras, um modelo separado seria utilizado por valor `<partition key>` e apenas eventos com que o valor seriam utilizados para formação esse modelo e de aprendizagem. Por exemplo, a seguinte consulta trains e pontua um leitura contra outras as leituras de apenas o sensor mesmo:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

* **cláusula limit_duration** `DURATION(<unit>, <length>)` -Especifica o intervalo de tempo (até que ponto anterior no histórico do evento atual do) deve ser considerado ao detetar anomalias. Consulte [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics) para uma descrição detalhada do unidades suportadas e as respetivas abreviaturas. 

* **when_clause** -Especifica uma condição booleana para os eventos considerado o cálculo de deteção de anomalias.

### <a name="return-types"></a>Tipos de retorno

O operador de AnomalyDetection devolve um registo que contém todas as três pontuações como o resultado. As propriedades associadas os diferentes tipos de detetores anomalias são:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Para extrair os valores fora do registo individuais, utilize o **GetRecordPropertyValue** função. Por exemplo:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 

Foi detetada uma anomalias de um tipo quando um das pontuações de anomalias atravesse um limiar. O limiar pode ser qualquer número de vírgula flutuante > = 0. O limiar é um compromisso entre sensibilidade e o confiança. Por exemplo, um limiar mais baixo seria efetuar deteção mais confidenciais para as alterações e gerar alertas mais, enquanto que um limiar mais elevado pode tornar deteção menos sensível e mais certeza mas mascarar algumas anomalias. O valor de limiar exato a utilizar depende do cenário. Não existe nenhum limite superior, mas o intervalo recomendado é de 3.25 5. 

O valor 3.25 mostrado no exemplo é apenas um ponto de partida sugerido. Otimizar o valor ao executar as operações no seu próprio conjunto de dados e observe o valor de saída até chegar um limiar tolerable.

## <a name="anomaly-detection-algorithm"></a>Algoritmo de deteção de anomalias

* Operador de AnomalyDetection utiliza um **learning não supervisionado** abordagem em que não assume qualquer tipo de distribuição nos eventos. Em geral, os dois modelos são mantidos em paralelo em qualquer momento, em que uma delas é utilizada para classificação e o outro está preparado em segundo plano. Os modelos de deteção de anomalias são preparados com dados da sequência atual, em vez de utilizar um mecanismo fora de banda. A quantidade de dados utilizados para formação depende o d de tamanho de janela especificado pelo utilizador dentro do parâmetro Limit Duration. Cada modelo termina obter preparado com base no d para 2d visão de eventos. É recomendado ter pelo menos 50 eventos em cada janela para obter os melhores resultados. 

* Utiliza o operador de AnomalyDetection **a deslizante semântica de janela** para preparar e eventos de pontuação de modelos. O que significa que cada evento é avaliado para anomalias e é devolvido um modelo de pontuação. A classificação é uma indicação de que o nível de confiança de que anomalias. 

* Operador de AnomalyDetection fornece um **garantia repetibilidade** sempre o mesmo entrada produz a mesma classificação, independentemente do resultado da tarefa a hora de início. A hora de início da saída de tarefa representa a hora em que o primeiro evento de saída é produzido pela tarefa. É definido pelo utilizador para a hora atual, um valor personalizado ou a última hora de saída (se a tarefa tivesse produziu uma saída anteriormente). 

### <a name="training-the-models"></a>Os modelos de preparação 

Como avança ser de tempo, modelos são preparados com dados diferentes. Para fazer sentido das pontuações, ajuda a compreender o mecanismo de subjacente através do qual os modelos são preparados. Aqui, **t<sub>0</sub>**  é o **hora de início do resultado da tarefa** e **d** é o **tamanho da janela** da duração de limite parâmetro. Partem do princípio de que a hora é dividida entre cópias de segurança **saltos de tamanho d**, a partir do `01/01/0001 00:00:00`. São utilizados os seguintes passos para preparar o modelo e Pontuar os eventos:

* Quando inicia uma tarefa de cópia de segurança, lê dados começando tempo t<sub>0</sub> – 2d.  
* Quando o tempo atinge o salto seguinte, um novo modelo M1 é criado e começa a obter preparado.  
* Quando o tempo avança pela outro salto, um novo modelo M2 é criado e começa a obter preparado.  
* Quando o tempo atinge t<sub>0</sub>, M1 é efetuada Active Directory e respetiva classificação começa a obter debitado.  
* No próximo salto, três coisas acontecer em simultâneo:  

  * M1 já não é necessário e são rejeitado.  
  * M2 tem foi suficientemente preparada, pelo que é utilizado para classificação.  
  * Um novo modelo M3 é criado e começa a obter preparado em segundo plano.  

* Este ciclo repete-se para cada hop, onde o modelo de Active Directory é eliminado, muda para o modelo de paralelo e inicie um modelo de terceiro em segundo plano de preparação. 

Diagrammatically, os passos de ter o seguinte aspeto: 

![Modelos de formação](media/stream-analytics-machine-learning-anomaly-detection/training_model.png)

|**Modelo** | **Hora de início de formação** | **Tempo para começar a utilizar o modelo de pontuação** |
|---------|---------|---------|
|M1     | 11:20   | 11:33   |
|M2     | 11:30   | 11:40   |
|M3     | 11:40   | 11:50   |

* Modelo M1 inicia formação às 11:20 am, que é o salto seguinte depois de leitura às 11:13 am de iniciar a tarefa. O primeiro ser produzido um resultado de M1 às 11:33 am após formação com 13 minutos de dados. 

* Um novo modelo M2 também inicia formação às 11:30 am, mas o modelo de pontuação não obter utilizado até 11:40 am, que é depois tenha sido preparado com 10 minutos de dados. 

* M3 acompanha o mesmo padrão como M2. 

### <a name="scoring-with-the-models"></a>Com os modelos de classificação 

Ao nível do Machine Learning, o algoritmo de deteção de anomalias calcula um valor de strangeness para cada evento de entrada comparando-o com eventos numa janela do histórico. O cálculo de strangeness é diferente para cada tipo de anomalias.  

Vamos rever a computação strangeness detalhadamente (partem do princípio de um conjunto de windows histórico com eventos existe): 

1. **Alteração de nível de bidirecional:** com base na janela do histórico, um intervalo de funcionamento normal é calculado como [10 de percentil, percentil 90th] isto é, 10 de valor de percentil como o valor de percentil de limite inferior e 90th como o limite superior. Um valor de strangeness para o evento atual é calculado como:  

   - 0, se event_value estiver ao alcance funcionamento normal  
   - Se event_value/90th_percentile, event_value > 90th_percentile  
   - 10th_percentile/event_value, se for o event_value < 10th_percentile  

2. **Tendência positiva lenta:** uma linha de tendência ao longo de valores de eventos na janela do histórico é calculada e a operação de procura uma tendência positiva dentro da linha. O valor de strangeness é calculado como:  

   - Declive, se declive for positivo  
   - 0, caso contrário 

3. **Tendência negativa lenta:** uma linha de tendência ao longo de valores de eventos na janela do histórico é calculada e a operação de procura tendência negativa dentro da linha. O valor de strangeness é calculado como: 

   - Declive, se declive for negativo  
   - 0, caso contrário  

Assim que o valor de strangeness para o evento de entrada é calculado, um valor de martingale é calculado com base no valor strangeness (consulte o [blogue de Machine Learning](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/) para obter detalhes sobre como o valor de martingale é calculado). Este valor martingale é retuned como a classificação de anomalias. O valor de martingale lentamente aumenta em resposta a um valores, que permite que o detector permaneça robusta para alterações esporádicas e reduz a alertas de falsas. Também tem uma propriedade úteis: 

Probabilidade [existe t esse que M<sub>t</sub> > λ] < 1/λ, onde M<sub>t</sub> é o valor de martingale em t instantânea e λ é um valor real. Por exemplo, se existir um alerta quando M<sub>t</sub>> 100, em seguida, a probabilidade de falsos positivos é inferior a 1/100.  

## <a name="guidance-for-using-the-bi-directional-level-change-detector"></a>Orientações para utilizar o nível de bidirecional alterar detector 

A alteração de nível de bidirecional detector pode ser utilizada em cenários como o tráfego de hora rush e de recuperação ou de falha de energia, etc. No entanto, funciona de forma a que depois de um modelo está preparado com determinados dados, outra alteração nível é anómala se e apenas se o novo valor é superior ao limite superior anterior (nível ascendente alterar maiúsculas e minúsculas) ou mais baixo do que o limite inferior anterior (nível de descendente Altere maiúsculas e minúsculas). Por conseguinte, um modelo, deverá ver os valores de dados no intervalo do novo nível (ascendência ou em decadência) não na respetiva janela de formação para os mesmos ser considerado anómalas. 

Os seguintes pontos devem ser considerados ao utilizar este detector: 

1. Quando a série de tempo subitamente vê um aumento ou remover no valor, o operador de AnomalyDetection Deteta-lo. Mas detetar regressar ao normal requer planeamento mais. Se uma série de tempo estava num Estado de repouso antes de anomalias, que podem ser conseguido através da definição de janela de deteção o operador AnomalyDetection no máximo metade do comprimento de anomalias. Este cenário pressupõe que a duração mínima da anomalias pode ser estimada antecedência e há suficiente eventos em que período de tempo para preparar o modelo suficientemente (pelo menos 50 eventos). 

   Tal é apresentado nos números 1 e 2 abaixo utilizando uma alteração de limite superior (a mesma lógica aplica-se a uma alteração de limite inferior). Em ambos os números as waveforms são uma alteração de nível anómala. Linhas de laranja verticais denotam limites de salto e o tamanho de salto é o mesmo que a janela de deteção especificada no operador AnomalyDetection. As linhas verdes indicam o tamanho da janela de formação. Na figura 1, o tamanho de salto é o mesmo que o tempo para que lasts anomalias. Na figura 2, o tamanho de salto é metade de tempo para o qual a anomalias dura. Em todos os casos, é detetada uma alteração ascendente porque o modelo utilizado para classificação foi preparado no dados normais. Mas, com base em como funciona o detector de alteração de nível de bidirecional, tem de excluir os valores normais da janela de preparação utilizada para o modelo que pontua regressar ao normal. Na figura 1, a formação do modelo de pontuação inclui alguns eventos normais, pelo que não é possível detetar a devolver em normal. Mas na figura 2, a formação inclui apenas a parte anómala, que garante que é detetado regressar ao normal. Algo mais pequeno do que metade também funciona para a mesma razão, enquanto que algo maior terminará a cópia de segurança, incluindo um bit dos eventos normais. 

   ![AD com o comprimento de anomalias igual de tamanho de janela](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_anomaly_length.png)

   ![AD com o tamanho da janela é igual a metade do comprimento de anomalias](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_half_anomaly_length.png)

2. Nos casos em que não é possível prever o comprimento de anomalias, este detector funciona, melhor esforço. No entanto, escolher que uma janela de tempo quanto mais estreitos forem limita os dados de formação, que deverá aumentar a probabilidade de detetar regressar ao normal. 

3. No cenário seguinte, a anomalias mais não não detetada como a janela de formação inclui já uma anomalias do mesmo valor elevado. 

   ![Anomalias com o mesmo tamanho de](media/stream-analytics-machine-learning-anomaly-detection/anomalies_with_same_length.png)

## <a name="example-query-to-detect-anomalies"></a>Consulta de exemplo para detetar anomalias 

A seguinte consulta pode ser utilizada para enviar um alerta se for detetada uma anomalias.
Quando o fluxo de entrada não é uniform, o passo de agregação pode ajudar a transformá-los para uma série de hora uniforme. O exemplo utiliza AVG, mas o tipo específico de agregação depende do cenário de utilizador. Além disso, quando uma série de tempo tem em falta irregulares maiores do que a janela de agregação, não sabemos de quaisquer eventos na série de tempo a deteção de anomalias de Acionador (de acordo com a deslizante semântica de janela). Como resultado, pressuposto de uniformidade da é interrompido quando chegam próximo evento. Estas situações, lacunas na série de tempo devem ser preenchidas. Uma abordagem possível consiste em efetuar o último evento para cada janela de salto, conforme mostrado abaixo.

```sql
    WITH AggregationStep AS 
    (
         SELECT
               System.Timestamp as tumblingWindowEnd,

               AVG(value) as avgValue

         FROM input
         GROUP BY TumblingWindow(second, 5)
    ),

    FillInMissingValuesStep AS
    (
          SELECT
                System.Timestamp AS hoppingWindowEnd,

                TopOne() OVER (ORDER BY tumblingWindowEnd DESC) AS lastEvent

         FROM AggregationStep
         GROUP BY HOPPINGWINDOW(second, 300, 5)

    ),

    AnomalyDetectionStep AS
    (

          SELECT
                hoppingWindowEnd,
                lastEvent.tumblingWindowEnd as lastTumblingWindowEnd,
                lastEvent.avgValue as lastEventAvgValue,
                system.timestamp as anomalyDetectionStepTimestamp,

                ANOMALYDETECTION(lastEvent.avgValue) OVER (LIMIT DURATION(hour, 1)) as
                scores

          FROM FillInMissingValuesStep
    )

    SELECT
          alert = 1,
          hoppingWindowEnd,
          lastTumblingWindowEnd,
          lastEventAvgValue,
          anomalyDetectionStepTimestamp,
          scores

    INTO output

    FROM AnomalyDetectionStep

    WHERE

        CAST(GetRecordPropertyValue(scores, 'BiLevelChangeScore') as float) >= 3.25

        OR CAST(GetRecordPropertyValue(scores, 'SlowPosTrendScore') as float) >=
        3.25

       OR CAST(GetRecordPropertyValue(scores, 'SlowNegTrendScore') as float) >=
       3.25
```

## <a name="performance-guidance"></a>Guia de desempenho

* Utilize unidades de transmissão em fluxo seis para tarefas. 
* Envie eventos, pelo menos, um segundo, à excepção.
* Uma consulta não particionadas que está a utilizar o operador de AnomalyDetection pode produzir resultados com uma latência de cálculo de cerca de 25 ms em média.
* A latência teve por uma consulta particionada varia ligeiramente com o número de partições, como o número de cálculos é superior. No entanto, a latência é sobre os mesmos que as maiúsculas e minúsculas não particionadas para um comparável número total de eventos em todas as partições.
* Ao ler os dados de não-em tempo real, uma grande quantidade de dados é ingerida rapidamente. Estes dados de processamento é atualmente mais lento. Foi encontrada a latência de tais cenários para aumentar a forma linear com o número de pontos de dados na janela em vez do intervalo de tamanho ou evento de janela. Para reduzir a latência em casos de não-em tempo real, considere utilizar um tamanho mais pequeno da janela. Em alternativa, considere a sua tarefa a partir da hora atual. Alguns exemplos de latências numa consulta não particionadas: 
    - 60 pontos de dados na janela de deteção podem resultar numa latência de 10 segundos com um débito de 3 MBps. 
    - Em 600 pontos de dados, a latência pode aceder aos cerca de 80 segundos com um débito de 0.4 MBps.

## <a name="limitations-of-the-anomalydetection-operator"></a>Limitações do operador AnomalyDetection 

* Este operador atualmente não suporta a deteção de pico de pedidos e dip. Picos e dips são spontaneous ou curta duração alterações na série de tempo. 

* Atualmente, este operador não processa padrões de sazonalidade. Padrões de sazonalidade são repetidos padrões nos dados, por exemplo um comportamento de tráfego web diferente durante a semana ou de diferentes tendências compras durante negra sexta-feira, que não são anomalias mas um padrão esperado no comportamento. 

* Este operador espera que a série de tempo de entrada ser uniforme. Um fluxo de eventos pode ser efetuado uniform por agregar através de uma em cascata ou hopping janela. Em cenários em que o intervalo entre eventos sempre é menor do que a janela de agregação, uma janela em cascata é suficiente para efetuar a série de tempo uniforme. Quando o intervalo pode ser maior, as lacunas podem ser preenchidas repetindo o último valor utilizando uma janela de salto. 

## <a name="references"></a>Referências

* [Deteção de anomalias – Using do machine learning detetar anomalias nos dados de séries de tempo](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [Deteção de anomalias API de aprendizagem](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Deteção de anomalias de séries de tempo](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

