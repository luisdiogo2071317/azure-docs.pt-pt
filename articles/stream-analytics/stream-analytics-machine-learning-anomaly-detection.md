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
ms.openlocfilehash: 3cd9b5a2bfed49ee712b89040477389ba9ea7715
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389637"
---
# <a name="anomaly-detection-in-azure-stream-analytics"></a>Deteção de anomalias no Azure Stream Analytics

> [!IMPORTANT]
> Esta funcionalidade está no processo a ser preterido, mas será substituída por novas funções. Para obter mais informações, visite o [oito novos recursos do Azure Stream Analytics](https://azure.microsoft.com/blog/eight-new-features-in-azure-stream-analytics/) postagem de blog.

O **AnomalyDetection** operador é utilizado para detetar diferentes tipos de anomalias em fluxos de eventos. Por exemplo, uma diminuição lenta memória livre durante muito tempo pode dever-se de um vazamento de memória ou o número de pedidos de serviço web que estão estáveis num intervalo pode aumentar ou reduzir drasticamente.  

O operador de AnomalyDetection Deteta três tipos de anomalias: 

* **Alterações no nível de bidirecionais**: uma constante de aumentar ou diminuir o nível de valores, ascendente e descendente. Este valor é diferente do picos e descidas, o que são alterações instantâneas ou curta duração.  

* **Lento tendência positiva**: um aumento lento a tendência ao longo do tempo.  

* **Lento tendência negativo**: uma diminuição lenta a tendência ao longo do tempo.  

Ao utilizar o operador de AnomalyDetection, tem de especificar o **Limit Duration** cláusula. Essa cláusula Especifica que o intervalo de tempo (até que ponto voltar no histórico do evento atual) deve ser considerado quando detetar anomalias. Este operador, opcionalmente, pode ser limitado a apenas os eventos que correspondem a uma determinada propriedade ou condição, utilizando o **quando** cláusula. Este operador opcionalmente, também pode processar dos grupos de eventos separadamente com base na chave especificada no **particionar por** cláusula. Formação e predição independentemente ocorrerem para cada partição. 

## <a name="syntax-for-anomalydetection-operator"></a>Sintaxe para o operador de AnomalyDetection

`ANOMALYDETECTION(<scalar_expression>) OVER ([PARTITION BY <partition key>] LIMIT DURATION(<unit>, <length>) [WHEN boolean_expression])` 

**Exemplo de utilização**  

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id > 100) FROM input`

### <a name="arguments"></a>Argumentos

* **scalar_expression** -a expressão escalar sobre o qual é executada a deteção de anomalias. Valores permitidos para este parâmetro incluem o número de vírgula flutuante ou tipos de dados de Bigint ou retorno um (valor escalar único). A expressão com carateres universais **\*** não é permitida. Expressão escalar não pode conter outras funções de análise ou funções externas. 

* **partition_by_clause** – a `PARTITION BY <partition key>` cláusula divide a aprendizagem e formação em partições separadas. Em outras palavras, um modelo separado seria usado por valor de `<partition key>` e eventos únicos com esse valor poderia ser usados para aprendizagem e formação nesse modelo. Por exemplo, a seguinte consulta trens e pontua um leitura em relação a outras leituras de apenas o sensor da mesma:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

* **cláusula limit_duration** `DURATION(<unit>, <length>)` -Especifica o intervalo de tempo (até que ponto voltar no histórico do evento atual) deve ser considerado quando detetar anomalias. Ver [DATEDIFF](https://msdn.microsoft.com/azure/stream-analytics/reference/datediff-azure-stream-analytics) para uma descrição detalhada de unidades suportadas e respetivas abreviaturas. 

* **when_clause** -Especifica uma condição booliana para os eventos considerado o cálculo de deteção de anomalias.

### <a name="return-types"></a>Tipos de retorno

O operador de AnomalyDetection devolve um registo que contém todas as pontuações de três como sua saída. As propriedades associadas com os diferentes tipos de detectores de anomalias são:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Para extrair os valores individuais sem o registo, utilize o **GetRecordPropertyValue** função. Por exemplo:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) > 3.25` 

Anomalias de um tipo é detectada quando um das pontuações de anomalias ultrapassar um limiar. O limiar pode ser qualquer número de vírgula flutuante > = 0. O limite é uma compensação entre confidencialidade e a confiança. Por exemplo, um limiar mais baixo seria fazer deteção mais suscetível a alterações e gerar mais alertas, ao passo que um limiar mais elevado poderia tornar deteção menos confidencial e mais confiantes mas mascarar alguns anomalias. O valor de limiar exato a utilizar depende do cenário. Não existe nenhum limite superior, mas o intervalo recomendado é 3.25 a 5. 

O valor 3.25 mostrado no exemplo é apenas um ponto de partida sugerido. Ajustar o valor ao executar as operações no seu próprio conjunto de dados e observe o valor de saída até chegar um limiar tolerável.

## <a name="anomaly-detection-algorithm"></a>Algoritmo de deteção de anomalias

* Operador de AnomalyDetection utiliza um **learning não supervisionado** abordagem em que ele não assume qualquer tipo de distribuição nos eventos. Em geral, os dois modelos são mantidos em paralelo em qualquer momento, em que uma delas é utilizada para a classificação e a outra é preparada em segundo plano. Os modelos de deteção de anomalias são treinados com os dados de fluxo de atual, em vez de usar um mecanismo fora de banda. A quantidade de dados usadas para treinamento depende o d de tamanho de janela especificado pelo usuário dentro do parâmetro Limit Duration. Cada modelo acaba introdução treinados com base no d para 2d valor de eventos. Recomenda-se de ter pelo menos 50 eventos em cada janela para obter melhores resultados. 

* Utiliza o operador de AnomalyDetection **deslizante semântica de janela** para preparar modelos e eventos de pontuação. Isto significa que cada evento é avaliado para anomalias e uma pontuação é devolvida. A classificação é uma indicação de que o nível de confiança de que anomalias. 

* Operador de AnomalyDetection fornece uma **garantia de capacidade de repetição** sempre a mesma entrada produz a mesma classificação, independentemente do resultado da tarefa de hora de início. A hora de início da saída de tarefa representa a hora em que o primeiro evento de saída é produzido pela tarefa. Ele é definido pelo utilizador para a hora atual, um valor personalizado ou a última hora de saída (se a tarefa tivesse produziu uma saída anteriormente). 

### <a name="training-the-models"></a>Os modelos de treinamento 

À medida que progride de tempo, modelos são treinados com dados diferentes. Dar sentido as pontuações, ele ajuda a compreender o mecanismo subjacente pelo qual os modelos são treinados. Aqui, **t<sub>0</sub>**  é o **hora de início da saída da tarefa** e **1!d** é o **tamanho da janela** da duração de limite parâmetro. Partem do princípio de que o tempo é dividido em **saltos de d de tamanho**, a partir `01/01/0001 00:00:00`. Os passos seguintes são utilizados para preparar o modelo e a pontuação os eventos:

* Quando um trabalho é iniciado, ele lê dados a partir de tempo t<sub>0</sub> – 2d.  
* Quando tempo atinge o próximo salto, um novo modelo M1 é criado e começa a introdução de preparação.  
* Quando a hora avança pelo salto de outro, um novo modelo de M2 é criado e começa a introdução de preparação.  
* Quando for atingida t<sub>0</sub>, M1 é feita Active Directory e sua pontuação inicia a obtenção de saída.  
* No próximo salto, três coisas acontecem em simultâneo:  

  * M1 já não é necessária e é descartado.  
  * M2 foi suficientemente treinado para que ele é usado para a classificação.  
  * Um novo modelo M3 é criado e começa a obter treinados em segundo plano.  

* Este ciclo repete-se para cada salto, onde o modelo de Active Directory é descartado, mudar para o modelo paralelo e começar a preparar um modelo de terceiro em segundo plano. 

Diagrammatically, os passos semelhante ao seguinte: 

![Modelos de formação](media/stream-analytics-machine-learning-anomaly-detection/training_model.png)

|**Modelo** | **Hora de início de treinamento** | **Hora de começar a utilizar a sua pontuação** |
|---------|---------|---------|
|M1     | 11:20   | 11:33   |
|M2     | 11:30   | 11:40   |
|M3     | 11:40   | 11:50   |

* Modelo M1 inicia o treinamento às 11H: 20, que é o salto seguinte depois da tarefa é iniciada a leitura às 11H: 13. A primeira saída é produzida a partir do M1 às 11:33 am após a formação com 13 minutos de dados. 

* Um novo modelo de M2 também começa treinamento às 11h30, mas não se acostumar sua pontuação até 11 40 am, que é depois foi treinado com dez minutos de dados. 

* M3 segue o mesmo padrão de M2. 

### <a name="scoring-with-the-models"></a>Com os modelos de classificação 

Ao nível do Machine Learning, o algoritmo de deteção de anomalias calcula um valor de strangeness para cada evento de entrada comparando-os com eventos numa janela do histórico. A computação strangeness é diferente para cada tipo de anomalias.  

Vamos rever o cálculo de strangeness detalhadamente (partem do princípio de um conjunto do windows históricos com eventos existe): 

1. **Alteração de nível de bidirecionais:** com base na janela do histórico, um intervalo de funcionamento normal é computado como [10 de percentil, percentil 90th] isto é, de 10 de valor de percentil como o valor de percentil limite inferior e 90, como o limite superior. Um valor de strangeness para o evento atual é computado como:  

   - 0, se event_value estiver ao alcance de funcionamento normal  
   - if event_value/90th_percentile, event_value > 90th_percentile  
   - 10th_percentile/event_value, se for o event_value < 10th_percentile  

2. **Tendência positiva lenta:** uma linha de tendência sobre os valores de eventos na janela do histórico é calculada e a operação de procura de uma tendência positiva dentro da linha. O valor de strangeness é computado como:  

   - Situação perigosa, se slope for positivo  
   - 0, caso contrário 

3. **Tendência de negativa lenta:** uma linha de tendência sobre os valores de eventos na janela do histórico é calculada e a operação de procura de tendência negativa dentro da linha. O valor de strangeness é computado como: 

   - Situação perigosa, se a inclinação é negativa  
   - 0, caso contrário  

Assim que o valor de strangeness para o evento de entrada é computado, um valor de martingale é calculado com base no valor strangeness (consulte a [blogue de Machine Learning](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/) para obter detalhes sobre como é calculado o valor de martingale). Este valor de martingale é ajustado como a classificação de anomalia. O valor de martingale lentamente aumenta em resposta a valores estranhos, que permite o detetor de mantenha a robustez às alterações esporádicos e reduz os falsos alertas. Ele também tem uma propriedade útil: 

Probabilidade [existe t tal esse M<sub>t</sub> > λ] < 1/λ, onde M<sub>t</sub> é o valor de martingale em t instantânea e λ é um valor real. Por exemplo, se existir um alerta quando M<sub>t</sub>> 100, em seguida, a probabilidade de falsos positivos é inferior a 1/100.  

## <a name="guidance-for-using-the-bi-directional-level-change-detector"></a>Detetor de alterar a orientação para usar o nível de bidirecional 

O detetor de alteração de nível de bidirecional pode ser usado em cenários como o tráfego de indisponibilidade e a recuperação ou a hora de ponta de energia, etc. No entanto, ele opera de forma que uma vez é preparado um modelo com certos dados, outra alteração nível é anómala se e apenas se o novo valor é maior do que o limite superior anterior (caso de alteração do nível de ascendente) ou menor do que o limite inferior anterior (nível de descendente Altere caso). Por conseguinte, um modelo não deverá ver valores de dados no intervalo do novo nível (para cima ou para baixo) em sua janela de treinamento para os mesmos para serem considerados como anómalos. 

Os seguintes pontos devem ser considerados ao utilizar este detetor de: 

1. Quando a série de tempo, de repente, vê um aumento ou remover no valor, o operador de AnomalyDetection detectá-lo. Mas a detecção de devolver ao normal requer um planejamento mais. Se era uma série de tempo no estado estável antes de anomalias, o que podem ser conseguido através da definição de janela de deteção da operadora AnomalyDetection pelo menos metade o comprimento da anomalia. Este cenário pressupõe que a duração mínima da anomalia pode ser estimada antes do tempo, e há suficiente nesse período de tempo para preparar o modelo suficientemente (pelo menos 50 eventos). 

   Isso é mostrado nas figuras 1 e 2 abaixo uma alteração de limite superior (a mesma lógica aplica-se a uma alteração de limite inferior). Nas figuras, as formas de ondas são uma alteração de nível anómala. Linhas verticais de cor de laranja denotam os limites de salto e o tamanho de salto é o mesmo que a janela de deteção especificada no operador AnomalyDetection. As linhas verde indicam o tamanho da janela de treinamento. Na figura 1, o tamanho de salto é o mesmo que o tempo para que dura de anomalias. Na figura 2, o tamanho de salto é metade do tempo para o qual a anomalias tem uma duração. Em todos os casos, é detetada uma alteração ascendente porque o modelo utilizado para a classificação foi treinado em dados normais. Mas, com base em como funciona o detetor de alteração de nível de bidirecional, ele tem de excluir os valores normais da janela de treinamento utilizada para o modelo que pontua o retorno como normal. Na figura 1, treinamento do modelo de classificação inclui alguns eventos normais, para que não é possível detetar o retorno como normal. Mas, na figura 2, a formação inclui apenas a parte anómala, o que garante que o retorno normal é detetado. Algo menor do que metade também funciona pela mesma razão, ao passo que nada maior terminarão incluindo um pouco de eventos normais. 

   ![AD com comprimento de anomalias igual de tamanho de janela](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_anomaly_length.png)

   ![AD com o tamanho da janela é igual a metade do comprimento de anomalias](media/stream-analytics-machine-learning-anomaly-detection/windowsize_equal_half_anomaly_length.png)

2. Em casos em que não é possível prever a duração da anomalia, este detetor opera na melhor esforço. No entanto, escolher que uma janela de tempo mais estreito limita os dados de treinamento, que seria aumentar a probabilidade de detecção de devolver ao normal. 

3. No cenário seguinte, a anomalias de mais tempo não não detetada como a janela de treinamento já inclui uma anomalia do mesmo valor elevado. 

   ![Anomalias com o mesmo tamanho](media/stream-analytics-machine-learning-anomaly-detection/anomalies_with_same_length.png)

## <a name="example-query-to-detect-anomalies"></a>Consulta de exemplo para detetar anomalias 

A seguinte consulta pode ser utilizada para gerar um alerta se for detetada uma anomalia.
Quando o fluxo de entrada não é uniforme, o passo de agregação pode ajudar a transformá-lo numa série de tempo uniforme. O exemplo usa a média, mas o tipo específico de agregação depende do cenário de utilizador. Além disso, quando uma série de tempo tem falhas maiores do que a janela de agregação, existem não são todos os eventos na série de tempo para deteção de anomalias do acionador (de acordo com deslizante semântica de janela). Como resultado, a suposição de que uniformidade é interrompida quando o seguinte evento é recebido. Em tais situações, as lacunas na série de tempo devem ser preenchidas. Uma abordagem possível é tirar o último evento em cada janela de salto, conforme mostrado abaixo.

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

## <a name="performance-guidance"></a>Orientação de desempenho

* Utilize unidades de transmissão em fluxo seis para tarefas. 
* Envie eventos, pelo menos, um segundo afastados.
* Uma consulta de não-particionada de mensagens em fila que está a utilizar o operador de AnomalyDetection pode produzir resultados com uma latência de computação de cerca de 25 ms em média.
* A latência registada por uma consulta particionada varia um pouco com o número de partições, como o número de computações é superior. No entanto, a latência é sobre o mesmo que o caso de não-particionada para um comparável número total de eventos de todas as partições.
* Ao ler os dados de tempo não real, uma grande quantidade de dados é ingerida rapidamente. Estes dados de processamento é atualmente mais lento. A latência em cenários assim, foi encontrada para aumentar de forma linear com o número de pontos de dados na janela, em vez do intervalo de tamanho ou de um evento de janela. Para reduzir a latência em casos não em tempo real, considere a utilização de um tamanho de janela mais pequeno. Em alternativa, considere a seu trabalho a partir da hora atual. Alguns exemplos de latências numa consulta não-particionada: 
    - 60 pontos de dados na janela de deteção podem resultar numa latência de 10 segundos, com um débito de 3 MBps. 
    - Em 600 pontos de dados, a latência pode chegar a cerca de 80 segundos com um débito de 0.4 MBps.

## <a name="limitations-of-the-anomalydetection-operator"></a>Limitações do operador AnomalyDetection 

* Este operador não suporta atualmente a deteção de pico e dip. Picos e descidas são alterações espontânea ou curta duração na série de tempo. 

* Atualmente, este operador não manipula os padrões de sazonalidade. Os padrões de sazonalidade são padrões repetidos nos dados, por exemplo um comportamento de tráfego web diferente durante a semana ou tendências de compras diferentes durante a Black Friday, que não são anomalias, mas um padrão esperado no comportamento. 

* Este operador espera que os da série de tempo de entrada para ser uniforme. Um fluxo de eventos pode ser feito uniforme, agregação ao longo de uma em cascata ou saltos janela. Em cenários em que a lacuna entre os eventos é sempre menor do que a janela de agregação, uma janela em cascata é suficiente para tornar a série de tempo uniforme. Quando o intervalo pode ser maior, as lacunas podem ser preenchidas, repetindo o último valor com uma janela de salto que remonte há. 

## <a name="references"></a>Referências

* [Deteção de anomalias – usando de machine learning para detetar anomalias nos dados de séries de tempo](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/)
* [API de deteção de anomalias de aprendizagem automática](https://docs.microsoft.com/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Deteção de anomalias de série de tempo](https://msdn.microsoft.com/library/azure/mt775197.aspx)

## <a name="next-steps"></a>Passos Seguintes

* [Introdução ao Azure Stream Analytics](stream-analytics-introduction.md)
* [Começar a utilizar o Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Tarefas de escala do Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referência do idioma de consulta do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referência de API do REST de gestão do Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)

