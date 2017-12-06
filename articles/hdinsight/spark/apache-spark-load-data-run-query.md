---
title: Executar consultas interativas num cluster do Azure HDInsight Spark | Microsoft Docs
description: "Início rápido do HDInsight Spark sobre como criar um cluster do Apache Spark no HDInsight."
keywords: spark quickstart,interactive spark,interactive query,hdinsight spark,azure spark
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 78ab44a7afa6523e1e9e4082b3f45b1a28affe77
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="run-interactive-queries-on-spark-clusters-in-hdinsight"></a>Executar consultas interativas nos clusters do Spark no HDInsight

Saiba como utilizar o bloco de notas do Jupyter para executar consultas de Spark SQL interativas contra um cluster do Spark. 

[Bloco de notas do Jupyter](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html) é uma aplicação baseada no browser que expande a experiência interativa consola baseada na Web. O Spark no HDInsight também inclui [bloco de notas do Zeppelin](apache-spark-zeppelin-notebook.md). Bloco de notas do Jupyter é utilizado neste tutorial.

Blocos de notas do Jupyter nos clusters do HDInsight suportam três kernels - **PySpark**, **PySpark3**, e **Spark**. O **PySpark** kernel é utilizado neste tutorial. Para obter mais informações sobre os kernels e as vantagens de utilizar **PySpark**, consulte [clusters kernels de bloco de notas do Jupyter de utilização com o Apache Spark no HDInsight](apache-spark-jupyter-notebook-kernels.md). Para utilizar o bloco de notas do Zeppelin, consulte [cluster notas do Zeppelin utilizar o Apache spark no Azure HDInsight](./apache-spark-zeppelin-notebook.md).

Neste tutorial, consulta os dados num ficheiro csv. Primeiro tem carregar dados para o Spark como um dataframe. Em seguida, pode executar consultas no dataframe utilizando o bloco de notas do Jupyter. 

## <a name="prerequisites"></a>Pré-requisitos

* **Um cluster do Azure HDInsight Spark**. Para instruções, consulte [criar um cluster do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* **Um bloco de notas do Jupyter com PySpark**. Para instruções, consulte [criar um bloco de notas do Jupyter](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

## <a name="create-a-dataframe-from-a-csv-file"></a>Criar um dataframe a partir de um ficheiro csv

Com um SQLContext aplicações podem criar dataframes partir um RDD existente, uma tabela do Hive ou das origens de dados. 

**Para criar um dataframe de um ficheiro csv**

1. Crie um bloco de notas do Jupyter com PySpark se não tiver uma. Para instruções, consulte [criar um bloco de notas do Jupyter](./apache-spark-jupyter-spark-sql.md#create-a-jupyter-notebook).

2. Cole o seguinte código numa célula vazia do bloco de notas e, em seguida, prima **SHIFT + ENTER** para executar o código. O código importa os tipos necessários para este cenário:

    ```PySpark
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```
    Ao utilizar o kernel do PySpark para criar um bloco de notas, Spark e o Hive contextos são criados automaticamente para si quando executa a primeira célula do código. Não é necessário criar quaisquer contextos explicitamente.

    Ao executar uma consulta interativa no Jupyter, a legenda de separador ou janela de browser web mostra um **(ocupado)** Estado juntamente com o título do bloco de notas. Também vê um círculo sólido junto ao texto do **PySpark** no canto superior direito. Após a conclusão da tarefa, este é alterado para um círculo vazio.

    ![Estado da consulta interativa do Spark SQL](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")

3. Execute o seguinte código para criar um dataframe e uma tabela temporária (**AVAC**) executando o seguinte código: O código não extrair todas as colunas disponíveis no ficheiro CSV. 

    ```PySpark
    # Create an RDD from sample data
    hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
    # Create a schema for our data
    Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')
    
    # Parse the data and create a schema
    hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
    hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
    
    # Infer the schema and create a table       
    hvacTable = sqlContext.createDataFrame(hvac)
    hvacTable.registerTempTable('hvactemptable')
    dfw = DataFrameWriter(hvacTable)
    dfw.saveAsTable('hvac')
    ```
    A seguinte captura de ecrã mostra um instantâneo do ficheiro HVAC.csv. O ficheiro csv é fornecido com todos os clusters do HDInsigt Spark. Os dados de captura as variações de temperatura de um edifício.

    ![Instantâneo dos dados para a consulta de Spark SQL interativa](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "instantâneo dos dados para a consulta de Spark SQL interativa")

## <a name="run-queries-on-the-dataframe"></a>Executar consultas no dataframe

Assim que a tabela for criada, pode executar uma consulta interativa nos dados.

**Para executar uma consulta**

1. Execute o seguinte código numa célula vazia do bloco de notas:

    ```PySpark
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Como o kernel do PySpark é utilizado no bloco de notas, pode agora executar diretamente uma consulta SQL interativa na tabela temporária **AVAC** que criou utilizando o `%%sql` magic. Para obter mais informações sobre a magia `%%sql`, bem como sobre outras magias disponíveis com o kernel do PySpark, veja [Kernels disponíveis nos blocos de notas do Jupyter com clusters do Spark HDInsight](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

   O resultado da tabela seguinte é apresentado por predefinição.

     ![Saída da tabela do resultado da consulta interativa do Spark](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")

3. Também pode ver os resultados noutras visualizações. Para ver um gráfico de área do resultado da mesmo, selecione **área** , em seguida, defina outros valores, conforme mostrado.

    ![Gráfico de área do resultado da consulta interativa do Spark](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")

10. Do **ficheiro** menu do bloco de notas, clique em **guardar e o ponto de verificação**. 

11. Se estiver a iniciar o [próximo tutorial](apache-spark-use-bi-tools.md) agora, deixe o bloco de notas aberta. Se não, encerre o bloco de notas para libertar os recursos de cluster: do **ficheiro** menu do bloco de notas, clique em **fechar e parar**.

## <a name="next-step"></a>Passo seguinte

Neste artigo, aprendeu como executar consultas interativas no Spark utilizando o bloco de notas do Jupyter. Avançar para o seguinte artigo para ver como os dados que registou no Spark ser solicitados para uma ferramenta de análise de BI como o Power BI e o Tableau. 

> [!div class="nextstepaction"]
>[BI Spark utilizando ferramentas de visualização de dados com o Azure HDInsight](apache-spark-use-bi-tools.md)




