---
title: Gerir os recursos de cluster do Apache Spark no Azure HDInsight | Microsoft Docs
description: Saiba como utilizar Gerir os recursos de clusters do Spark no Azure HDInsight para um melhor desempenho.
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2018
ms.author: jgao
ms.openlocfilehash: 639f8540be289c03abc8d352f4bd9150c945625e
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Gerir os recursos de cluster do Apache Spark no Azure HDInsight 

Saiba como aceder as interfaces, como Ambari IU, IU do YARN e o servidor de histórico de Spark associado um cluster do Spark e de como otimizar a configuração de cluster para um desempenho ideal.

**Pré-requisitos:**

* Um cluster do Apache Spark no HDInsight. Para obter instruções, consulte [clusters do Apache Spark criar no Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="open-the-ambari-web-ui"></a>Abra a IU da Web do Ambari

Apache Ambari é utilizado para monitorizar o cluster e efetuar alterações de configuração. Para obter mais informações, consulte [clusters do Hadoop gerir no HDInsight ao utilizar o portal do Azure](../hdinsight-administer-use-portal-linux.md#open-the-ambari-web-ui)

## <a name="open-the-spark-history-server"></a>Abra o servidor de histórico do Spark

O servidor de histórico do Spark é a IU da web para aplicações do Spark foi concluídas e em execução. É uma extensão da IU da Web do Sparkl.

**Para abrir a IU do Spark histórico de servidor Web**

1. Do [portal do Azure](https://portal.azure.com/), abra o cluster do Spark. Para obter mais informações, consulte [clusters lista e mostrar](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. De **ligações rápidas**, clique em **Cluster Dashboard**e, em seguida, clique em **Spark histórico de servidor**

    ![Servidor do histórico de spark](./media/apache-spark-resource-manager/launch-history-server.png "Spark histórico servidor")

    Quando lhe for pedido, introduza as credenciais de administrador para o cluster do Spark. Pode também abrir o servidor de histórico do Spark ao navegar para o seguinte URL:

    ```
    https://<ClusterName>.azurehdinsight.net/sparkhistory
    ```

    Substitua <ClusterName> com o nome de cluster do Spark.

Web Server de histórico do Spark IU aspeto:

![Servidor do HDInsight Spark histórico](./media/apache-spark-resource-manager/hdinsight-spark-history-server.png)

## <a name="open-the-yarn-ui"></a>Abra o IU do Yarn
Pode utilizar a IU do YARN para monitorizar as aplicações que estão atualmente em execução no cluster do Spark.

1. Do [portal do Azure](https://portal.azure.com/), abra o cluster do Spark. Para obter mais informações, consulte [clusters lista e mostrar](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. De **ligações rápidas**, clique em **Cluster Dashboard**e, em seguida, clique em **YARN**.

    ![Iniciar a IU do YARN](./media/apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > Em alternativa, pode também iniciar a IU do YARN da IU do Ambari. Para iniciar a IU do Ambari, clique em **Cluster Dashboard**e, em seguida, clique em **Dashboard de Cluster do HDInsight**. Na IU do Ambari, clique em **YARN**, clique em **ligações rápidas**, clique em Gestor de recursos do Active Directory e, em seguida, clique em **da IU do Gestor de recursos**.
   >
   >

## <a name="optimize-clusters-for-spark-applications"></a>Otimizar clusters para aplicações do Spark
Os três parâmetros de chaves que podem ser utilizados para a configuração de Spark consoante os requisitos da aplicação são `spark.executor.instances`, `spark.executor.cores`, e `spark.executor.memory`. Um Executor é um processo iniciado para uma aplicação de Spark. Este é executado no nó de trabalho e é responsável para realizar as tarefas para a aplicação. O número predefinido de executor e os tamanhos de executor de cada cluster é calculado com base no número de nós de trabalho e o tamanho de nó de trabalho. Esta informação é armazenada no `spark-defaults.conf` sobre os nós principais do cluster.

Os parâmetros de três configuração podem ser configurados ao nível do cluster (para todas as aplicações executadas no cluster) ou podem ser especificados para cada aplicação individuais bem.

### <a name="change-the-parameters-using-ambari-ui"></a>Altere os parâmetros através da IU do Ambari
1. De clique a IU do Ambari **Spark**, clique em **Contigs**e, em seguida, expanda **personalizada spark-predefinições**.

    ![Parâmetros de conjunto com o Ambari](./media/apache-spark-resource-manager/set-parameters-using-ambari.png)
2. Os valores predefinidos são boas ter quatro Spark as aplicações são executadas em simultâneo no cluster. Pode alterar estes valores de interface de utilizador, conforme mostrado na captura de ecrã seguinte:

    ![Parâmetros de conjunto com o Ambari](./media/apache-spark-resource-manager/set-executor-parameters.png)
3. Clique em **guardar** para guardar as alterações de configuração. Na parte superior da página, lhe for pedido para reiniciar os serviços afetados. Clique em **reiniciar**.

    ![Reinicie os serviços](./media/apache-spark-resource-manager/restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Altere os parâmetros para uma aplicação em execução no bloco de notas do Jupyter
Para aplicações em execução no bloco de notas do Jupyter, pode utilizar o `%%configure` magic que efetue as alterações de configuração. Idealmente, tem de se essas alterações no início da aplicação, antes de executar a sua primeira célula do código. Efetuar este procedimento assegura que a configuração é aplicada à sessão Livy, quando é criada. Se pretender alterar a configuração de uma fase posterior da aplicação, tem de utilizar o `-f` parâmetro. No entanto, ao fazê-progresso de todas as na aplicação é perdido.

O fragmento seguinte mostra como alterar a configuração para uma aplicação em execução no Jupyter.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Parâmetros de configuração tem de ser transmitidos como uma cadeia JSON e tem de estar na linha seguinte após a magia, conforme apresentado na coluna de exemplo.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>Altere os parâmetros para uma aplicação enviados através de spark-submeter
Os seguintes comandos é um exemplo de como pode alterar os parâmetros de configuração para uma aplicação de batch que é submetido utilizando `spark-submit`.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>Altere os parâmetros para uma aplicação submetido utilizando cURL
O comando seguinte é um exemplo de como pode alterar os parâmetros de configuração para uma aplicação de batch que é submetido utilizando cURL.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="change-these-parameters-on-a-spark-thrift-server"></a>Alterar estes parâmetros num servidor Thrift de Spark
Servidor Thrift de Spark fornece acesso JDBC/ODBC para um cluster do Spark e é utilizado para consultas de Spark SQL do serviço. Ferramentas como o Power BI, Tableau etc. Utilize o protocolo ODBC para comunicar com o servidor de Thrift de Spark para executar consultas do Spark SQL como uma aplicação de Spark. Quando é criado um cluster do Spark, duas instâncias do servidor Thrift de Spark são iniciadas, um em cada nó principal. Cada servidor Thrift de Spark é visível como uma aplicação de Spark na IU do YARN.

Servidor Thrift de Spark utiliza alocação do Spark executor dinâmica e, por conseguinte, o `spark.executor.instances` não é utilizado. Em vez disso, utiliza servidor Thrift de Spark `spark.dynamicAllocation.minExecutors` e `spark.dynamicAllocation.maxExecutors` para especificar a contagem de executor. Os parâmetros de configuração `spark.executor.cores` e `spark.executor.memory` é utilizado para modificar o tamanho de executor. Pode alterar estes parâmetros, conforme mostrado nos passos seguintes:

* Expanda o **avançadas sparkconf de thrift de spark** categoria para atualizar os parâmetros `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, e `spark.executor.memory`.

    ![Configurar o servidor thrift de Spark](./media/apache-spark-resource-manager/spark-thrift-server-1.png)    
* Expanda o **personalizada spark-thrift-sparkconf** categoria para atualizar o parâmetro `spark.executor.cores`.

    ![Configurar o servidor thrift de Spark](./media/apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="change-the-driver-memory-of-the-spark-thrift-server"></a>Altere a memória do controlador do servidor Thrift de Spark
Memória do servidor Thrift de Spark controlador é configurado para 25% do tamanho da RAM de nó principal, fornecido que o tamanho da RAM total do nó principal é superior ao 14 GB. Pode utilizar a IU do Ambari para alterar a configuração de memória do controlador, conforme mostrado na captura de ecrã seguinte:

* De clique a IU do Ambari **Spark**, clique em **folhas**, expanda **avançadas spark env**e, em seguida, forneça o valor para **spark_thrift_cmd_opts**.

    ![Configurar o servidor thrift de Spark RAM](./media/apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="reclaim-spark-cluster-resources"></a>Recuperar recursos de cluster do Spark
Devido a alocação dinâmica de Spark, os únicos recursos que são consumidos pelo servidor thrift são os recursos para modelos de estrutura mestres duas aplicações. Para recuperar estes recursos, tem de parar os serviços de servidor Thrift em execução no cluster.

1. Na IU do Ambari, no painel esquerdo, clique em **Spark**.
2. Na página seguinte, clique em **servidores de Thrift de Spark**.

    ![Reinicie o servidor thrift](./media/apache-spark-resource-manager/restart-thrift-server-1.png)
3. Deverá ver os duas headnodes no qual está a executar o servidor Thrift de Spark. Clique do headnodes.

    ![Reinicie o servidor thrift](./media/apache-spark-resource-manager/restart-thrift-server-2.png)
4. A página seguinte apresenta uma lista de todos os serviços em execução nesse headnode. Na lista, clique no botão de lista pendente junto do servidor Thrift de Spark e, em seguida, clique em **parar**.

    ![Reinicie o servidor thrift](./media/apache-spark-resource-manager/restart-thrift-server-3.png)
5. Repita estes passos em de outra headnode bem.

## <a name="restart-the-jupyter-service"></a>Reinicie o serviço do Jupyter
Inicie a IU de Web do Ambari, conforme mostrado no início do artigo. No painel de navegação esquerdo, clique em **Jupyter**, clique em **serviço ações**e, em seguida, clique em **todas reinicie**. Esta ação inicia o serviço de Jupyter em todos os headnodes.

![Reinicie o Jupyter](./media/apache-spark-resource-manager/restart-jupyter.png "reiniciar Jupyter")

## <a name="monitor-resources"></a>Monitorizar recursos
Inicie a IU do Yarn, conforme mostrado no início do artigo. Na tabela de métricas de Cluster por cima do ecrã, verifique os valores de **memória utilizada** e **memória Total** colunas. Se os dois valores forem fechar, poderão não existir recursos suficientes para iniciar a aplicação seguinte. O mesmo se aplica para o **VCores utilizado** e **VCores Total** colunas. Além disso, na vista de principal, se existir uma aplicação stayed no **ACEITES** estado e não vão transitar para **executar** nem **falha** Estado, isto também pode ser uma indicação de que Não é ao obter recursos suficientes para iniciar.

![Limite de recurso](./media/apache-spark-resource-manager/resource-limit.png "limite de recursos")

## <a name="kill-running-applications"></a>Eliminar aplicações em execução
1. Na IU do Yarn, a partir do painel esquerdo, clique em **executar**. Na lista de aplicações em execução, determinar a aplicação a ser cancelado e clique em de **ID**.

    ![Kill App1](./media/apache-spark-resource-manager/kill-app1.png "Kill App1")

2. Clique em **Kill aplicação** no canto superior direito, em seguida, clique em **OK**.

    ![Kill App2](./media/apache-spark-resource-manager/kill-app2.png "Kill App2")

## <a name="see-also"></a>Consulte também
* [Controlar e depurar tarefas em execução num cluster do Apache Spark do HDInsight](apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Para os analistas de dados

* [Spark com Machine Learning: Utilizar o Spark no HDInsight para analisar a temperatura do edifício com dados de AVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](apache-spark-machine-learning-mllib-ipython.md)
* [Análise de registos de sites com o Spark no HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Análise de dados telemétricos do Application Insight com o Spark no HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Utilizar Caffe no Azure HDInsight Spark para aprender profunda distribuída](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Para programadores do Spark

* [Criar uma aplicação autónoma com o Scala](apache-spark-create-standalone-application.md)
* [Executar tarefas remotamente num cluster do Spark com o Livy](apache-spark-livy-rest-interface.md)
* [Utilizar o Plug-in das Ferramentas do HDInsight para o IntelliJ IDEA para criar e submeter aplicações do Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Transmissão em Fluxo do Spark: Utilizar o Spark no HDInsight para criar aplicações de transmissão em fluxo em tempo real](apache-spark-eventhub-streaming.md)
* [Utilizar o Plug-in das Ferramentas do HDInsight para o IntelliJ IDEA para depurar aplicações do Spark remotamente](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Utilizar blocos de notas do Zeppelin com um cluster do Spark no HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o bloco de notas do Jupyter no cluster do Spark para o HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Utilizar pacotes externos com blocos de notas do Jupyter](apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter no computador e ligar a um cluster do Spark do HDInsight](apache-spark-jupyter-notebook-install-locally.md)
