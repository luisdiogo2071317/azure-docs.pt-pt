---
title: Ação de script - pacotes de instalar o Python com o Jupyter no Azure HDInsight
description: Instruções passo a passo sobre como utilizar a ação de script para configurar disponíveis da blocos de notas do Jupyter com clusters do Spark do HDInsight para utilizar pacotes externos python.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: jasonh
ms.openlocfilehash: 36e727a59b91303c8c62c5525f72c328e2792ad6
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619180"
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Utilize a ação de Script para instalar pacotes de Python externos para blocos de notas do Jupyter nos clusters do Apache Spark no HDInsight
> [!div class="op_single_selector"]
> * [Magia de células](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Com a ação de Script](apache-spark-python-package-installation.md)
>
>

Saiba como utilizar as ações de Script para configurar um cluster do Apache Spark no HDInsight (Linux) para utilizar externo, fornecidas pela Comunidade **python** pacotes que não são incluídos out of box, no cluster.

> [!NOTE]
> Também pode configurar um bloco de notas do Jupyter com o `%%configure` mágica para utilizar pacotes externos. Para obter instruções, consulte [utilizar pacotes externos com blocos de notas do Jupyter nos clusters do Apache Spark no HDInsight](apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Pode pesquisar o [índice de pacote](https://pypi.python.org/pypi) para obter a lista completa de pacotes que estão disponíveis. Também pode obter uma lista dos pacotes disponíveis a partir de outras origens. Por exemplo, pode instalar disponibilizados por meio de pacotes [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) ou [uma falsificação conda](https://conda-forge.org/feedstocks/).

Neste artigo, irá aprender a instalar o [TensorFlow](https://www.tensorflow.org/) com a ação de Script no seu cluster do pacote e usá-lo por meio do bloco de notas do Jupyter.

## <a name="prerequisites"></a>Pré-requisitos
Tem de ter o seguinte:

* Uma subscrição do Azure. Consulte [Obter uma avaliação gratuita do Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Um cluster do Apache Spark no HDInsight. Para obter instruções, veja [Criar clusters do Apache Spark no Azure HDInsight](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Se ainda não tiver um cluster do Spark no HDInsight Linux, pode executar ações de script durante a criação do cluster. Visite a documentação sobre [como utilizar as ações de script personalizado](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a>Utilizar pacotes externos com blocos de notas do Jupyter

1. No [Portal do Azure](https://portal.azure.com/), no startboard, clique no mosaico relativo ao cluster do Spark (se o tiver afixado ao startboard). Também pode navegar até ao cluster em **Procurar Tudo** > **Clusters do HDInsight**.   

2. No painel do cluster do Spark, clique em **ações de Script** no painel à esquerda. Executar a ação personalizada que instalará TensorFlow os nós principais e os nós de trabalho. O script de bash pode ser referenciado em: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh visite a documentação sobre [como utilizar as ações de script personalizado](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Existem dois python instalações no cluster. Spark irá utilizar a instalação do python Anaconda localizada em `/usr/bin/anaconda/bin`. Essa instalação nas suas ações personalizadas através de referência `/usr/bin/anaconda/bin/pip` e `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Abra um bloco de notas do Jupyter do PySpark

    ![Criar um novo bloco de notas Jupyter](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Criar um novo bloco de notas Jupyter")

4. É criado e aberto um novo bloco de notas com o nome Untitled.pynb. Clique no nome do bloco de notas na parte superior e introduza um nome amigável.

    ![Forneça um nome para o bloco de notas](./media/apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Forneça um nome para o bloco de notas")

5. Irá agora `import tensorflow` e executar um exemplo do hello world. 

    Código para copiar:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    O resultado tem o seguinte aspeto:
    
    ![Execução de código do TensorFlow](./media/apache-spark-python-package-installation/execution.png "TensorFlow executar código")

## <a name="seealso"></a>Ver também
* [Descrição geral: Apache Spark no Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Cenários
* [Spark com BI: Efetuar uma análise de dados interativa com o Spark no HDInsight com ferramentas do BI](apache-spark-use-bi-tools.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para analisar a temperatura do edifício com dados de AVAC](apache-spark-ipython-notebook-machine-learning.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](apache-spark-machine-learning-mllib-ipython.md)
* [Análise de registos de sites com o Spark no HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Criar e executar aplicações
* [Criar uma aplicação autónoma com o Scala](apache-spark-create-standalone-application.md)
* [Executar tarefas remotamente num cluster do Spark com o Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Ferramentas e extensões
* [Utilizar pacotes externos com blocos de notas do Jupyter nos clusters do Apache Spark no HDInsight](apache-spark-jupyter-notebook-use-external-packages.md)
* [Utilizar o Plug-in das Ferramentas do HDInsight para o IntelliJ IDEA para criar e submeter aplicações do Spark Scala](apache-spark-intellij-tool-plugin.md)
* [Utilizar o Plug-in das Ferramentas do HDInsight para o IntelliJ IDEA para depurar aplicações do Spark remotamente](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Utilizar blocos de notas do Zeppelin com um cluster do Spark no HDInsight](apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o bloco de notas do Jupyter no cluster do Spark para o HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Instalar o Jupyter no computador e ligar a um cluster do Spark do HDInsight](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gerir recursos
* [Gerir recursos para o cluster do Apache Spark no Azure HDInsight](apache-spark-resource-manager.md)
* [Controlar e depurar tarefas em execução num cluster do Apache Spark do HDInsight](apache-spark-job-debugging.md)
