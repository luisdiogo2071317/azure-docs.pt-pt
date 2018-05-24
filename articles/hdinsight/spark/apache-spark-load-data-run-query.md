---
title: 'Tutorial: Carregar dados e executar consultas num cluster do Apache Spark no Azure HDInsight | Microsoft Docs'
description: Saiba como carregar dados e executar consultas interativas em clusters do Spark no Azure HDInsight.
services: azure-hdinsight
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.devlang: na
ms.topic: tutorial
ms.author: jgao
ms.date: 05/17/2018
ms.openlocfilehash: eeb0f8134d21d42c8401f58828160d613e8ef92b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/18/2018
ms.locfileid: "34302054"
---
# <a name="tutorial-load-data-and-run-queries-on-an-apache-spark-cluster-in-azure-hdinsight"></a>Tutorial: Carregar dados e executar consultas num cluster do Apache Spark no Azure HDInsight

Neste tutorial, irá aprender a criar um pacote de dados a partir de um ficheiro CSV e a executar consultas SQL interativas de Spark num cluster do Apache Spark no Azure HDInsight. No Spark, um pacote de dados é uma coleção distribuída de dados organizados em colunas com nomes. Do ponto de vista conceptual, o pacote de dados equivale a uma tabela numa base de dados relacional ou a um pacote de dados em R/Python.
 
Neste tutorial, ficará a saber como:
> [!div class="checklist"]
> * Criar um pacote de dados a partir de um ficheiro CSV
> * Executar consultas no pacote de dados

Se não tiver uma subscrição do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* Conclua [Criar um cluster do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-dataframe-from-a-csv-file"></a>Criar um pacote de dados a partir de um ficheiro CSV

As aplicações podem criar pacotes de dados a partir de um Conjunto de Dados Distribuído Resiliente (RDD) existente, de uma tabela do Hive ou de origens de dados que utilizam o objeto SQLContext. A seguinte captura de ecrã mostra um instantâneo do ficheiro HVAC.csv utilizado neste tutorial. O ficheiro CSV é fornecido com todos os clusters do Spark no HDInsight. Os dados ilustram as variações de temperatura de alguns edifícios.
    
![Instantâneo de dados para a consulta SQL interativa de Spark](./media/apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Instantâneo de dados para a consulta SQL interativa de Spark")


1. Abra o bloco de notas Jupyter Notebook que criou na secção de pré-requisitos.
2. Cole o seguinte código numa célula vazia do bloco de notas e, em seguida, prima **SHIFT + ENTER** para o executar. O código importa os tipos necessários para este cenário:

    ```PySpark
    from pyspark.sql import *
    from pyspark.sql.types import *
    ```

    Quando executa uma consulta interativa no Jupyter, a janela do browser ou a legenda do separador apresenta o estado **(Ocupado)**, juntamente com o título do bloco de notas. Também vê um círculo sólido junto ao texto do **PySpark** no canto superior direito. Após a conclusão da tarefa, este é alterado para um círculo vazio.

    ![Estado da consulta interativa do Spark SQL](./media/apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")

3. Execute o seguinte código para criar um pacote de dados e uma tabela temporária (**hvac**) ao utilizar o seguinte código. 

    ```PySpark
    # Create an RDD from sample data
    csvFile = spark.read.csv('wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv', header=True, inferSchema=True)
    csvFile.write.saveAsTable("hvac")
    ```

    > [!NOTE]
    > Ao utilizar o kernel do PySpark para criar um bloco de notas, os contextos de SQL são criados automaticamente quando executa a primeira célula de código. Não é necessário criar explicitamente os contextos.


## <a name="run-queries-on-the-dataframe"></a>Executar consultas no pacote de dados

Após a criação da tabela, pode executar uma consulta interativa nos dados.

1. Execute o seguinte código numa célula vazia do bloco de notas:

    ```PySpark
    %%sql
    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```

   Tendo em conta a utilização do kernel do PySpark no bloco de notas, agora pode executar diretamente uma consulta SQL interativa na tabela temporária **hvac**.

   É apresentado o seguinte resultado em forma de tabela.

     ![Saída da tabela do resultado da consulta interativa do Spark](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")

3. Também pode ver os resultados noutras visualizações. Para ver um gráfico de área do mesmo resultado, selecione **Area (Área)** e, em seguida, defina outros valores, conforme mostrado.

    ![Gráfico de área do resultado da consulta interativa do Spark](./media/apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")

10. No menu **File (Ficheiro)** do bloco de notas, selecione **Save and Checkpoint (Guardar e Ponto de Verificação)**. 

11. Se tenciona iniciar o [próximo tutorial](apache-spark-use-bi-tools.md) agora, deixe o bloco de notas aberto. Caso contrário, encerre o bloco de notas para libertar os recursos do cluster: no menu **File (Ficheiro)** do bloco de notas, selecione **Close and Halt (Fechar e Parar)**.

## <a name="clean-up-resources"></a>Limpar recursos

Com o HDInsight, os seus dados são armazenados no Armazenamento do Azure ou no Azure Data Lake Store, pelo que pode eliminar um cluster em segurança quando este não estiver a ser utilizado. Também lhe é cobrado o valor de um cluster do HDInsight mesmo quando não o está a utilizar. Uma vez que os custos do cluster são muito superiores aos custos do armazenamento, faz sentido do ponto de vista económico eliminar os clusters quando não estiverem a ser utilizados. Se tenciona começar já a trabalhar no próximo tutorial, convém manter o cluster.

Abra o cluster no portal do Azure e, em seguida, selecione **Eliminar**.

![Eliminar o cluster do HDInsight](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "Eliminar o cluster do HDInsight")

Também pode selecionar o nome do grupo de recursos para abrir a página do grupo de recursos e, em seguida, selecionar **Eliminar grupo de recursos**. Ao eliminar o grupo de recursos, está a eliminar o cluster do Spark no HDInsight e a conta de armazenamento predefinida.

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, ficou a saber como:

* Crie um pacote de dados do Spark.
* Execute a consulta SQL de Spark no pacote de dados.

Avance para o próximo artigo para ver como os dados que registou no Spark podem ser extraídos para uma ferramenta de análise de BI como o Power BI. 
> [!div class="nextstepaction"]
> [Analisar dados com ferramentas do BI](apache-spark-use-bi-tools.md)

