---
title: Criar um cluster do Apache Spark no Azure HDInsight | Microsoft Docs
description: "Início rápido do HDInsight Spark sobre como criar um cluster do Apache Spark no HDInsight."
keywords: spark quickstart,interactive spark,interactive query,hdinsight spark,azure spark
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/29/2017
ms.author: jgao
ms.openlocfilehash: 516c48424ef5d1256296240541fb544c1e5d9205
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/05/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Criar um cluster do Apache Spark no Azure HDInsight

Saiba como criar um cluster do Apache Spark no Azure HDInsight e como executar consultas no Spark SQL relativamente a tabelas do Hive. Para obter informações sobre o Spark no HDInsight, consulte [descrição geral: Apache Spark no Azure HDInsight](apache-spark-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

* **Uma subscrição do Azure**. Antes de começar este tutorial, tem de ter uma subscrição do Azure. Veja o artigo [Crie a sua conta do Azure gratuita](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>Criar o cluster do HDInsight Spark

Crie um cluster do HDInsight Spark com um [modelo do Azure Resource Manager](../hdinsight-hadoop-create-linux-clusters-arm-templates.md). O modelo pode ser encontrado no [github](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Para obter outros métodos de criação de clusters, consulte [Criar clusters do HDInsight](../hdinsight-hadoop-provision-linux-clusters.md).

1. Clique na imagem seguinte para abrir o modelo no portal do Azure.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Introduza os seguintes valores:

    ![Criar um cluster do HDInsight Spark com um modelo do Azure Resource Manager](./media/apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Criar um cluster do Spark no HDInsight com um modelo do Azure Resource Manager")

    * **Subscrição**: selecione a subscrição do Azure utilizada para criar este cluster.
    * **Grupo de recursos**: crie um grupo de recursos ou selecione um grupo existente. O grupo de recursos é utilizado para gerir recursos do Azure para os seus projetos.
    * **Localização**: Selecione uma localização para o grupo de recursos. O modelo utiliza esta localização para criar o cluster, bem como para o armazenamento predefinido do mesmo.
    * **ClusterName**: introduza um nome para o cluster do HDInsight que pretende criar.
    * **Nome e palavra-passe de início de sessão do cluster**: o nome de início de sessão predefinido é admin.
    * **Nome de utilizador e palavra-passe do SSH**.

3. Selecione **Concordo com os termos e condições indicados acima**, selecione **Afixar ao dashboard** e, em seguida, clique em **Comprar**. Verá um novo mosaico intitulado Submeter a implementação para a implementação do Modelo. A criação do cluster demora cerca de 20 minutos.

Caso se depare com um problema ao criar clusters do HDInsight, estes poderão dever-se ao facto de não ter as permissões certas para o fazer. Para obter mais informações, veja [Access control requirements](../hdinsight-administer-use-portal-linux.md#create-clusters) (Requisitos do controlo de acesso).

> [!NOTE]
> Este artigo cria um cluster do Spark que utiliza [Blobs de Armazenamento do Azure como armazenamento de cluster](../hdinsight-hadoop-use-blob-storage.md). Também pode criar um cluster do Spark que utiliza o [Azure Data Lake Store](../hdinsight-hadoop-use-data-lake-store.md) como o armazenamento predefinido. Para obter instruções, consulte [Criar um cluster de HDInsight com o Arquivo Data Lake](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).
>
>

## <a name="create-a-jupyter-notebook"></a>Criar um bloco de notas do Jupyter

[O bloco de notas do Jupyter](http://jupyter.org) é um ambiente de bloco de notas interativo que suporta várias linguagens de programação que lhe permitem interagir com os seus dados, combinar código com texto markdown e efetuar visualizações simples. O Spark no HDInsight também inclui [bloco de notas do Zeppelin](apache-spark-zeppelin-notebook.md). Neste tutorial, é utilizado o bloco de notas do Jupyter.

**Para criar um bloco de notas do Jupyter**

1. Abra o [Portal do Azure](https://portal.azure.com/).

2. Abra o cluster do Spark que criou. Para instruções, consulte [Listar e mostrar clusters](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

3. A partir de **Ligações rápidas**, clique em **Dashboards de clusters** e, em seguida, clique em **Bloco de Notas do Jupyter**. Se lhe for solicitado, introduza as credenciais de administrador do cluster.

   ![Abra o Bloco de Notas do Jupyter para executar a consulta interativa do Spark SQL](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Open Jupyter Notebook to run interactive Spark SQL query")

   > [!NOTE]
   > Também pode abrir o URL seguinte no seu browser para aceder ao Bloco de Notas do Jupyter do cluster. Substitua **CLUSTERNAME** pelo nome do cluster:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Clique em **Novo** e, em seguida, clique em **PySpark** para criar um bloco de notas. Os blocos de notas do Jupyter nos clusters do HDInsight suportam três kernels – **PySpark**, **PySpark3** e **Spark**. Neste tutorial, utiliza-se o kernel **PySpark**. Para obter mais informações sobre os kernels e sobre os beneficios de utilizar o **PySpark**, veja [Utilizar kernels de blocos de notas do Jupyter com clusters do Apache Spark no HDInsight](apache-spark-jupyter-notebook-kernels.md).

   ![Crie um Bloco de Notas Jupyter para executar a consulta interativa do Spark SQL](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Create a Jupyter Notebook to run interactive Spark SQL query")

   É criado e aberto um novo bloco de notas com o nome Untitled (Untitled.pynb).

4. Clique no nome do bloco de notas na parte superior e introduza um nome amigável se assim o desejar.

    ![Indique um nome para o Bloco de Notas do Jupyter para executar uma consulta interativa do Spark](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Provide a name for the Jupter Notebook to run interactive Spark query from")

## <a name="run-spark-sql-statements-on-a-hive-table"></a>Executar instruções do Spark SQL numa tabela do Hive

SQL (Structured Query Language) é a linguagem mais comum e mais utilizada para consultar e definir dados. O Spark SQL funciona como uma extensão do Apache Spark para o processamento de dados estruturados e utiliza a sintaxe familiar do SQL Server.

O Spark SQL suporta o SQL e o HiveQL como idiomas de consulta. As capacidades da oferta incluem o enlace em Python, Scala e Java. Com o Spark SQL, pode consultar dados armazenados em muitas localizações, como bases de dados externas, ficheiros de dados estruturados (por exemplo, JSON) e tabelas do Hive.

Para obter um exemplo de dados de leitura de um ficheiro csv em vez de uma tabela do Hive, veja [Executar consultas interativas nos clusters do Spark no HDInsight](apache-spark-load-data-run-query.md).

**Para executar o Spark SQL**

1.  A partir do bloco de notas, cole o seguinte código numa célula vazia e, em seguida, prima **SHIFT + ENTER** para o executar. 

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```

    ![Consulta do Hive no HDInsight Spark](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Consulta do Hive no HDInsight Spark")

    Quando utiliza um Bloco de Notas do Jupyter com o seu cluster do HDInsight Spark, obtém um `sqlContext` predefinido que pode utilizar para executar consultas do Hive com o Spark SQL. `%%sql` indica ao Bloco de Notas do Jupyter que utilize o `sqlContext` predefinido para executar a consulta do Hive. A consulta devolve as primeiras dez linhas de uma tabela do Hive (**hivesampletable**) que vem em todos os clusters do HDInsight por predefinição. Para obter mais informações sobre o `%%sql` magic e os contextos predefinidos, veja [Jupyter kernels available for an HDInsight cluster](apache-spark-jupyter-notebook-kernels.md) (Kernels do Jupyter disponíveis para clusters do HDInsight).

    Sempre que executar uma consulta no Jupyter, o título da janela do browser apresenta o estado **(Ocupado)** juntamente com o título do bloco de notas. Também vê um círculo sólido junto ao texto do **PySpark** no canto superior direito. Após a conclusão da tarefa, este é alterado para um círculo vazio.
    
    O ecrã deve atualizar-se e mostrar o resultado da consulta.

    ![Hive query output in HDInsight Spark](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "(Saída de consulta do Hive no HDInsight Spark)")

2. A partir do menu **Ficheiro** do bloco de notas, clique em **Fechar e Parar**. Encerrar o bloco de notas liberta os recursos do cluster.

3. Se quiser concluir os passos seguintes mais tarde, não se esqueça de eliminar o cluster do HDInsight que criou neste artigo. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Passo seguinte 

Neste artigo, aprendeu a criar um cluster do HDInsight Spark e a executar uma consulta do Spark SQL básica. Avance para o artigo seguinte para saber como utilizar um cluster do HDInsight Spark para executar consultas interativas nos dados de exemplo.

> [!div class="nextstepaction"]
>[Executar consultas interativas num cluster do Spark do HDInsight](apache-spark-load-data-run-query.md)



