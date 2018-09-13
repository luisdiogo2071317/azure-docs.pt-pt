---
title: Como utilizar o MMLSpark Machine Learning | Documentos da Microsoft
description: Guia de como utilizar a biblioteca de MMLSpark com o Azure Machine Learning.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: 654b2559518cd52978153310fbb1e89a91838a8a
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/12/2018
ms.locfileid: "35649420"
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Como utilizar o Microsoft Machine Learning biblioteca para o Apache Spark

## <a name="introduction"></a>Introdução

[Microsoft Machine Learning biblioteca para o Apache Spark](https://github.com/Azure/mmlspark) (MMLSpark) fornece ferramentas que permitem-lhe facilmente criam modelos de aprendizagem de máquina dimensionável para grandes conjuntos de dados. Ela inclui a integração de SparkML pipelines com o [Microsoft Cognitive Toolkit ](https://github.com/Microsoft/CNTK) e [OpenCV](http://www.opencv.org/), permitindo-lhe: 
 * Entrada e de imagem pré-processar dados
 * Caracterização de imagens e texto usando modelos de aprendizagem de profunda com formação prévia
 * Dar formação e Pontuar os modelos de classificação e regressão com featurization implícita.

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este guia de procedimentos, terá de:
- [Instalar o Azure Machine Learning Workbench](../service/quickstart-installation.md)
- [Configurar o cluster do Azure HDInsight Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Executar a sua experimentação num contentor do Docker

O Azure Machine Learning Workbench está configurado para utilizar o MMLSpark quando executar experimentações no contentor do Docker, localmente ou numa VM remota. Esta capacidade permite-lhe facilmente depurar e testar seus modelos do PySpark, antes de executá-los em escala num cluster. 

Para começar a utilizar um exemplo, crie um novo projeto e selecione o exemplo da Galeria "MMLSpark para adultos recenseamento". Selecione "Docker" como o contexto de computação a partir do dashboard do projeto e clique em "Executar". O Azure Machine Learning Workbench cria o contentor do Docker para executar o programa de PySpark e, em seguida, executa o programa.

Depois de concluída a execução, pode exibir os resultados na vista de histórico de execuções do Azure Machine Learning Workbench.

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Instale o MMLSpark no Cluster do Azure HDInsight Spark.

Para concluir esta e o passo seguinte, precisa primeiro [criar um cluster do Azure HDInsight Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql).

Por predefinição, o Azure Machine Learning Workbench instala o pacote de MMLSpark no seu cluster, quando executa a sua experimentação. Pode controlar esse comportamento e instalar outros pacotes do Spark ao editar um arquivo chamado _aml_config/spark_dependencies.yml_ na pasta do projeto.

```
# Spark configuration properties.
configuration:
  "spark.app.name": "Azure ML Experiment"
  "spark.yarn.maxAppAttempts": 1

repositories:
  - "https://mmlspark.azureedge.net/maven"
packages:
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.9.9"
```

Também pode instalar o MMLSpark diretamente no seu cluster do HDInsight Spark com [ação de Script](https://github.com/Azure/mmlspark#hdinsight).

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>Configurar o Cluster do Spark HDInsight do Azure como destino de computação

Abra a janela da CLI do Azure Machine Learning Workbench ao aceder ao Menu de "Arquivo" e clique em "linha de comandos aberta"

Na janela da CLI, execute comandos a seguir:

```
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> 
```

```
az ml experiment prepare -c <myhdi>
```

Agora o cluster está disponível como destino para o projeto de computação.

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Experiência de execução no cluster do Azure HDInsight Spark.

Volte ao dashboard do projeto de exemplo "MMLSpark para adultos recenseamento". Selecione o cluster como o destino de computação e clique em executar.

O Azure Machine Learning Workbench submete a tarefa do spark para o cluster. Pode monitorizar o progresso e ver os resultados na vista de histórico de execuções.

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre a biblioteca de MMLSpark e exemplos, consulte [repositório do GitHub de MMLSpark](https://github.com/Azure/mmlspark)

*Apache®, Apache Spark e Spark® são marcas registradas ou marcas comerciais da Apache Software Foundation nos Estados Unidos e/ou em outros países.*
