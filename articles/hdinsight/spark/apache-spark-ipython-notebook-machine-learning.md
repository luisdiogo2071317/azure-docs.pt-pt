---
title: "Criar aplicações de aprendizagem máquina Apache Spark no Azure HDInsight | Microsoft Docs"
description: "Instruções passo a passo sobre como criar aplicações de aprendizagem do Apache Spark no HDInsight Spark clusters utilizando o bloco de notas do Jupyter"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: jgao
ms.openlocfilehash: 74dcd368d8696df26c5ad294c5657161fbe7f408
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/25/2018
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Criar aplicações de aprendizagem máquina Apache Spark no Azure HDInsight

Saiba como criar um Apache Spark de machine learning aplicação utilizando um cluster do Spark no HDInsight. Este artigo mostra como utilizar o bloco de notas do Jupyter disponível com o cluster para criar e testar esta aplicação. A aplicação utiliza os dados de HVAC.csv de exemplo que está disponíveis em todos os clusters por predefinição.

[MLlib](https://spark.apache.org/docs/1.1.0/mllib-guide.html) biblioteca de aprendizagem do Spark máquina dimensionável constituídas por comuns e utilitários de algoritmos de aprendizagem, incluindo classificação, regressão, clustering, filtragem de colaboração, redução dimensionalidade, bem como subjacente primitivos de otimização.

**Pré-requisitos:**

Tem de ter o seguinte:

* Um cluster do Apache Spark no HDInsight. Para obter instruções, consulte [clusters do Apache Spark criar no Azure HDInsight](apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Compreender o conjunto de dados

Os seguintes dados mostram a temperatura de destino e a temperatura real de algumas edifícios que têm sistemas de AVAC instalado. O **sistema** coluna representa o ID de sistema e o **SystemAge** coluna representa o número de anos, o sistema de AVAC foi implementado em criar. Utilizar os dados neste tutorial para prever se um edifício vai ser hotter ou colder com base na temperatura de destino, com um ID de sistema e a idade do sistema.

![Instantâneo dos dados utilizados por exemplo, o Spark machine learning](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "instantâneo dos dados utilizados por exemplo, o Spark machine learning")

O ficheiro de dados, **HVAC.csv**, está localizado em **\HdiSamples\HdiSamples\SensorSampleData\hvac** em todos os clusters do HDInsight.

## <a name="app"></a>Escrever uma aplicação de aprendizagem do Spark máquina com o Spark MLlib
Nesta aplicação, utiliza um Spark [pipeline de ML](https://spark.apache.org/docs/2.2.0/ml-pipeline.html) para fazer a classificação de um documento. Pipelines de ML fornecem um conjunto de APIs de alto nível desenvolvidas DataFrames ajudar os utilizadores a criar e otimizar prático de machine learning pipelines uniform. No pipeline, dividir o documento em palavras, converter as palavras um vetor de funcionalidade numéricos e, finalmente, criar um modelo de previsão a utilizar o vetores de funcionalidade e as etiquetas. Execute os seguintes passos para criar a aplicação.

1. Crie um bloco de notas do Jupyter com o kernel do PySpark. Para instruções, consulte [criar um bloco de notas do Jupyter](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).
2. Importe os tipos necessários para este cenário. Cole o seguinte fragmento numa célula vazia e, em seguida, prima **SHIFT + ENTER**. 

    ```PySpark
    from pyspark.ml import Pipeline
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml.feature import HashingTF, Tokenizer
    from pyspark.sql import Row

    import os
    import sys
    from pyspark.sql.types import *

    from pyspark.mllib.classification import LogisticRegressionWithSGD
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array
    ```
3. Carregar os dados (hvac.csv), analisá-lo e utilizá-la para preparar o modelo. 

    ```PySpark
    # Define a type called LabelDocument
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
    data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
    training = documents.toDF()
    ```

    No fragmento de código, é possível definir uma função que compara a temperatura real com a temperatura de destino. Se for superior a temperatura real, criar é frequente, a designação pelo valor **1.0**. Caso contrário a criação é amovíveis, a designação pelo valor **0,0**. 

4. Configurar o pipeline de aprendizagem do Spark máquina que consiste em três fases: atomizador, hashingTF e lr. 

    ```PySpark
    tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
    ```

    Para obter mais informações sobre o que é um pipeline e como funciona consulte <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">pipeline do Spark machine learning</a>.

5. Ajustar o pipeline o documento de formação.
   
    ```PySpark
    model = pipeline.fit(training)
    ```

6. Certifique-se o documento de formação para o ponto de verificação do progresso com a aplicação.
   
    ```PySpark
    training.show()
    ```
   
    Isto deve dar o resultado semelhante ao seguinte:

    ```
    +----------+----------+-----+
    |BuildingID|SystemInfo|label|
    +----------+----------+-----+
    |         4|     13 20|  0.0|
    |        17|      3 20|  0.0|
    |        18|     17 20|  1.0|
    |        15|      2 23|  0.0|
    |         3|      16 9|  1.0|
    |         4|     13 28|  0.0|
    |         2|     12 24|  0.0|
    |        16|     20 26|  1.0|
    |         9|      16 9|  1.0|
    |        12|       6 5|  0.0|
    |        15|     10 17|  1.0|
    |         7|      2 11|  0.0|
    |        15|      14 2|  1.0|
    |         6|       3 2|  0.0|
    |        20|     19 22|  0.0|
    |         8|     19 11|  0.0|
    |         6|      15 7|  0.0|
    |        13|      12 5|  0.0|
    |         4|      8 22|  0.0|
    |         7|      17 5|  0.0|
    +----------+----------+-----+
    ```

    Comparar o resultado contra o ficheiro raw de CSV. Por exemplo, a primeira linha do ficheiro CSV tem este tipo de dados:

    ![Saída de dados de instantâneos de exemplo do Spark machine learning](./media/apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "instantâneo dos dados de saída de exemplo do Spark machine learning")

    Repare como a temperatura real é menor do que a temperatura de destino, sugerindo que criar é amovíveis. Por conseguinte, na formação de saída, o valor para **etiqueta** na primeira linha é **0,0**, não é frequente que significa que criar.

7. Prepare um conjunto de dados para executar o modelo treinado contra. Para tal, passa num ID de sistema e a idade do sistema (como a designação **SystemInfo** na saída de formação), e o modelo prevê se a criação com esse ID de sistema e a idade do sistema vai ser hotter (designação por 1.0) ou cooler (em falta por que 0,0).
   
    ```PySpark   
    # SystemInfo here is a combination of system ID followed by system age
    Document = Row("id", "SystemInfo")
    test = sc.parallelize([(1L, "20 25"),
                    (2L, "4 15"),
                    (3L, "16 9"),
                    (4L, "9 22"),
                    (5L, "17 10"),
                    (6L, "7 22")]) \
        .map(lambda x: Document(*x)).toDF() 
    ```
8. Por fim, fazer predições nos dados de teste. 
   
    ```PySpark
    # Make predictions on test documents and print columns of interest
    prediction = model.transform(test)
    selected = prediction.select("SystemInfo", "prediction", "probability")
    for row in selected.collect():
        print row
    ```

    Deverá ver um resultado semelhante ao seguinte:

    ```   
    Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
    Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
    Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
    Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
    Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
    Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
    ```
   
   Da primeira linha na predição, pode ver que para um sistema de AVAC com ID 20 e duração de sistema de 25 anos, criar será acesso frequente (**predição = 1.0**). O primeiro valor para DenseVector (0.49999) corresponde da predição 0,0 e o segundo valor (0.5001) corresponde da predição 1.0. Na saída, apesar do segundo valor só é aumenta ligeiramente superior, o modelo mostra **predição = 1.0**.
10. Encerrar o bloco de notas para libertar os recursos. Para o fazer, no menu **Ficheiro** do bloco de notas, clique em **Fechar e Parar**. Deste modo, o bloco de notas é encerrado e fechado.

## <a name="anaconda"></a>Utilizar Anaconda scikit-saiba biblioteca para o machine learning do Spark
Clusters do Apache Spark no HDInsight incluem bibliotecas Anaconda. Isto também inclui o **scikit-Saiba** biblioteca para o machine learning. A biblioteca também inclui vários conjuntos de dados que pode utilizar para criar aplicações de exemplo diretamente a partir de um bloco de notas do Jupyter. Para obter exemplos sobre como utilizar o scikit-saber biblioteca, consulte [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Ver também
* [Descrição geral: Apache Spark no Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Cenários
* [Spark com BI: Efetuar uma análise de dados interativa com o Spark no HDInsight com ferramentas do BI](apache-spark-use-bi-tools.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](apache-spark-machine-learning-mllib-ipython.md)
* [Transmissão em Fluxo do Spark: Utilizar o Spark no HDInsight para criar aplicações de transmissão em fluxo em tempo real](apache-spark-eventhub-streaming.md)
* [Análise de registos de sites com o Spark no HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Criar e executar aplicações
* [Criar uma aplicação autónoma com o Scala](apache-spark-create-standalone-application.md)
* [Executar tarefas remotamente num cluster do Spark com o Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Ferramentas e extensões
* [Utilizar o Plug-in das Ferramentas do HDInsight para o IntelliJ IDEA para criar e submeter aplicações do Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Utilizar o Plug-in das Ferramentas do HDInsight para o IntelliJ IDEA para depurar aplicações do Spark remotamente](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Utilizar blocos de notas do Zeppelin com um cluster do Spark no HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o bloco de notas do Jupyter no cluster do Spark para o HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Utilizar pacotes externos com blocos de notas do Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter no computador e ligar a um cluster do Spark do HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gerir recursos
* [Gerir recursos para o cluster do Apache Spark no Azure HDInsight](apache-spark-resource-manager.md)
* [Controlar e depurar tarefas em execução num cluster do Apache Spark do HDInsight](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]:../hadoop/apache-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]:../hadoop/apache-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md
