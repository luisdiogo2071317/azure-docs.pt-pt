---
title: Criar um pipeline de aprendizagem do Apache Spark - Azure HDInsight | Microsoft Docs
description: Utilize a biblioteca de aprendizagem do Apache Spark para criar pipelines de dados.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: maxluk
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: maxluk
ms.openlocfilehash: 238ab5f940fbea836b75e20b015ae16f22eef3e9
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="create-a-spark-machine-learning-pipeline"></a>Criar um pipeline do Spark machine learning

Biblioteca de aprendizagem do Apache Spark máquina escalável (MLlib) proporciona capacidades de modelação para um ambiente distribuído. O pacote de Spark [ `spark.ml` ](http://spark.apache.org/docs/latest/ml-pipeline.html) é um conjunto de APIs de alto nível incorporados no DataFrames. Estas APIs ajudam a criar e otimizar práticas pipelines de aprendizagem automática.  *Spark aprendizagem* refere-se a esta API com base em MLlib DataFrame, não mais antigo com base em RDD pipeline API.

Um de machine learning pipeline (ML) é um fluxo de trabalho completado combinar os vários algoritmos do machine learning em conjunto. Podem existir vários passos necessários para processar e obter dados, que requerem uma sequência de algoritmos. Pipelines definem as fases e ordenação de uma processo de aprendizagem. No MLlib, as fases de um pipeline são representados por uma sequência de PipelineStages, onde um transformador e um Estimator efetuam tarefas específica.

Um transformador é um algoritmo que transforma um DataFrame para outro, utilizando o `transform()` método. Por exemplo, um transformador de funcionalidade foi ler uma coluna de uma DataFrame, mapeá-lo para outra coluna e um novo DataFrame de saída com a coluna mapeada anexada ao mesmo.

Um Estimator é uma abstração de algoritmos de aprendizagem e é responsável por ajuste ou um conjunto de dados de preparação produzir um transformador. Um Estimator implementa um método denominado `fit()`, que aceita um DataFrame e produz um DataFrame, que é um transformador.

Cada instância sem monitorização de estado de um transformador ou um Estimator tem o seu próprio identificador exclusivo, que é utilizado durante a especificação de parâmetros. Ambos utilizam uma uniform API para especificar estes parâmetros.

## <a name="pipeline-example"></a>Exemplo de pipeline

Para demonstrar uma utilização de um pipeline de ML prática, este exemplo utiliza o exemplo `HVAC.csv` ficheiro de dados que vem previamente carregado no armazenamento de cluster do HDInsight, o Storage do Azure ou para o Data Lake Store predefinida. Para ver os conteúdos do ficheiro, navegue para o `/HdiSamples/HdiSamples/SensorSampleData/hvac` diretório. `HVAC.csv`contém um conjunto de tempo de destino e temperatures reais de AVAC (*aquecimento, ventilação e ar condicionado*) edifícios vários sistemas. O objetivo é para preparar o modelo de dados e produzir uma previsão temperatura para um determinado edifício.

O seguinte código:

1. Define um `LabeledDocument`, qual armazena o `BuildingID`, `SystemInfo` (um sistema identificador e duração) e um `label` (1.0 se a criação for demasiado frequente, 0.0, caso contrário).
2. Cria uma função de analisador personalizado `parseDocument` que demora uma linha (linhas) de dados e determina se o edifício "ativos" comparando a temperatura de destino para a temperatura real.
3. Aplica-se o analisador quando extrair os dados de origem.
4. Cria dados de preparação.

```python
# The data structure (column meanings) of the data array:
# 0 Date
# 1 Time
# 2 TargetTemp
# 3 ActualTemp
# 4 System
# 5 SystemAge
# 6 BuildingID

LabeledDocument = Row("BuildingID", "SystemInfo", "label")

# Define a function that parses the raw CSV file and returns an object of type LabeledDocument
def parseDocument(line):
    values = [str(x) for x in line.split(',')]
    if (values[3] > values[2]):
        hot = 1.0
    else:
        hot = 0.0        

    textValue = str(values[4]) + " " + str(values[5])

    return LabeledDocument((values[6]), textValue, hot)

# Load the raw HVAC.csv file, parse it using the function
data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
training = documents.toDF()
```

Este pipeline de exemplo tem três fases: `Tokenizer` e `HashingTF` (ambas Transformers), e `Logistic Regression` (um Estimator).  Os dados extraídos e analisados o `training` DataFrame flui através do pipeline quando `pipeline.fit(training)` é chamado.

1. A primeira fase, `Tokenizer`, divide o `SystemInfo` coluna de entrada (que consistem os valores de identificador e idade do sistema) para um `words` coluna de saída. Neste novo `words` coluna é adicionada ao DataFrame. 
2. A segunda fase, `HashingTF`, converte a nova `words` coluna em vetores de funcionalidade. Neste novo `features` coluna é adicionada ao DataFrame. Estes primeiro duas fases são Transformers. 
3. A terceira fase `LogisticRegression`, é um Estimator e, por isso, o pipeline chama o `LogisticRegression.fit()` método para produzir um `LogisticRegressionModel`. 

```python
tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
lr = LogisticRegression(maxIter=10, regParam=0.01)

# Build the pipeline with our tokenizer, hashingTF, and logistic regression stages
pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

model = pipeline.fit(training)
```

Para ver o novo `words` e `features` colunas adicionadas pelo `Tokenizer` e `HashingTF` transformers e um exemplo do `LogisticRegression` estimator, execute um `PipelineModel.transform()` método no DataFrame original. Código de produção, o passo seguinte seria possível passem um teste DataFrame para validar a formação.

```python
peek = model.transform(training)
peek.show()

# Outputs the following:
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|BuildingID|SystemInfo|label|   words|            features|       rawPrediction|         probability|prediction|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+
|         4|     13 20|  0.0|[13, 20]|(262144,[250802,2...|[0.11943986671420...|[0.52982451901740...|       0.0|
|        17|      3 20|  0.0| [3, 20]|(262144,[89074,25...|[0.17511205617446...|[0.54366648775222...|       0.0|
|        18|     17 20|  1.0|[17, 20]|(262144,[64358,25...|[0.14620993833623...|[0.53648750722548...|       0.0|
|        15|      2 23|  0.0| [2, 23]|(262144,[31351,21...|[-0.0361327091023...|[0.49096780538523...|       1.0|
|         3|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|         4|     13 28|  0.0|[13, 28]|(262144,[69821,25...|[0.14630166986618...|[0.53651031790592...|       0.0|
|         2|     12 24|  0.0|[12, 24]|(262144,[187043,2...|[-0.0509556393066...|[0.48726384581522...|       1.0|
|        16|     20 26|  1.0|[20, 26]|(262144,[128319,2...|[0.33829638728900...|[0.58377663577684...|       0.0|
|         9|      16 9|  1.0| [16, 9]|(262144,[153779,1...|[-0.0853679939336...|[0.47867095324139...|       1.0|
|        12|       6 5|  0.0|  [6, 5]|(262144,[18659,89...|[0.07513008136562...|[0.51877369045183...|       0.0|
|        15|     10 17|  1.0|[10, 17]|(262144,[64358,25...|[-0.0291988646553...|[0.49270080242078...|       1.0|
|         7|      2 11|  0.0| [2, 11]|(262144,[212053,2...|[0.03678030020834...|[0.50919403860812...|       0.0|
|        15|      14 2|  1.0| [14, 2]|(262144,[109681,2...|[0.06216423725633...|[0.51553605651806...|       0.0|
|         6|       3 2|  0.0|  [3, 2]|(262144,[89074,21...|[0.00565582077537...|[0.50141395142468...|       0.0|
|        20|     19 22|  0.0|[19, 22]|(262144,[139093,2...|[-0.0769288695989...|[0.48077726176073...|       1.0|
|         8|     19 11|  0.0|[19, 11]|(262144,[139093,2...|[0.04988910033929...|[0.51246968885151...|       0.0|
|         6|      15 7|  0.0| [15, 7]|(262144,[77099,20...|[0.14854929135994...|[0.53706918109610...|       0.0|
|        13|      12 5|  0.0| [12, 5]|(262144,[89689,25...|[-0.0519932532562...|[0.48700461408785...|       1.0|
|         4|      8 22|  0.0| [8, 22]|(262144,[98962,21...|[-0.0120753606650...|[0.49698119651572...|       1.0|
|         7|      17 5|  0.0| [17, 5]|(262144,[64358,89...|[-0.0721054054871...|[0.48198145477106...|       1.0|
+----------+----------+-----+--------+--------------------+--------------------+--------------------+----------+

only showing top 20 rows
```

O `model` objeto pode agora ser utilizado para tornar as predições. Para o exemplo completo desta máquina learning aplicação e instruções passo a passo para executá-los, consulte [compilar o Apache Spark aprendizagem automática aplicações no Azure HDInsight](apache-spark-ipython-notebook-machine-learning.md).

## <a name="see-also"></a>Consulte também

* [Ciência de dados utilizando Scala e o Spark no Azure](../../machine-learning/team-data-science-process/scala-walkthrough.md)
