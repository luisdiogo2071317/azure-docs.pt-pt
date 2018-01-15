---
title: Como utilizar MMLSpark Machine Learning | Microsoft Docs
description: Guia de como utilizar a biblioteca de MMLSpark com o Azure Machine Learning.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/12/2018
ms.openlocfilehash: f978805f800a35908629a6febb59d7db50d14023
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/13/2018
---
# <a name="how-to-use-microsoft-machine-learning-library-for-apache-spark"></a>Como utilizar o Microsoft biblioteca de Machine Learning para Apache Spark

## <a name="introduction"></a>Introdução

[Biblioteca Microsoft aprendizagem máquina para o Apache Spark](https://github.com/Azure/mmlspark) (MMLSpark) fornece ferramentas que permitem facilmente criar modelos de aprendizagem máquina dimensionável para grandes conjuntos de dados. Inclui integração de SparkML pipelines com a [Toolkit de cognitivos ](https://github.com/Microsoft/CNTK) e [OpenCV](http://www.opencv.org/), permitindo-lhe: 
 * Entrada e de dados de imagem previamente processos
 * Featurize imagens e texto utilizando modelos learning profunda previamente preparado
 * Dar formação e Pontuar os modelos de classificação e regressão utilizando featurization implícita.

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este guia de procedimentos, tem de:
- [Instalar do Azure Machine Learning Workbench](quickstart-installation.md)
- [Configurar o cluster Azure HDInsight Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql)

## <a name="run-your-experiment-in-docker-container"></a>Executar a sua experimentação no contentor de Docker

O Workbench do Azure Machine Learning é configurado para utilizar MMLSpark quando executar experimentações no contentor de Docker, localmente ou numa VM remoto. Esta capacidade permite-lhe facilmente depurar e experimentar seus modelos do PySpark, antes de executá-los numa escala num cluster. 

Para começar a utilizar um exemplo, crie um novo projeto e selecione exemplo da Galeria "MMLSpark para adultos Census". Selecione "Docker" como o contexto de computação do dashboard do projeto e clique em "Executar". Workbench de aprendizagem máquina do Azure cria o contentor de Docker para executar o programa de PySpark e, em seguida, executa o programa.

Após a execução foi concluída, pode ver os resultados na vista do histórico de execução do Workbench do Azure Machine Learning.

## <a name="install-mmlspark-on-azure-hdinsight-spark-cluster"></a>Instale MMLSpark no Cluster do Azure HDInsight Spark.

Para concluir esta e o passo seguinte, terá primeiro [criará um cluster do Azure HDInsight Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql).

Por predefinição, o Azure Machine Learning Workbench instala MMLSpark pacote no seu cluster quando executar a sua experimentação. Pode controlar este comportamento e instalar outros pacotes de Spark ao editar um ficheiro denominado _aml_config/spark_dependencies.yml_ na pasta do projeto.

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

Também pode instalar MMLSpark diretamente no seu cluster do Spark do HDInsight utilizando [ação de Script](https://github.com/Azure/mmlspark#hdinsight).

## <a name="set-up-azure-hdinsight-spark-cluster-as-compute-target"></a>Configurar o Cluster do Azure HDInsight Spark como destino de computação

Abra a janela CLI do Azure Machine Learning Workbench acedendo ao Menu "Ficheiro" e clique em "Abra a linha de comando"

Na janela do CLI, execute os seguintes comandos:

```
az ml computetarget attach cluster --name <myhdi> --address <myhdi-ssh.azurehdinsight.net> --username <sshusername> --password <sshpwd> 
```

```
az ml experiment prepare -c <myhdi>
```

O cluster está agora disponível como destino para o projeto de computação.

## <a name="run-experiment-on-azure-hdinsight-spark-cluster"></a>Execute experimentação no cluster Azure HDInsight Spark.

Volte ao dashboard do projeto de exemplo de "MMLSpark para adultos Census". Selecione o cluster como o destino de computação e clique em executar.

Azure Workbench de aprendizagem máquina submete a tarefa de spark para o cluster. Pode monitorizar o progresso e ver os resultados na vista do histórico de execução.

## <a name="next-steps"></a>Passos Seguintes
Para obter informações sobre a biblioteca de MMLSpark e exemplos, consulte [repositório do MMLSpark GitHub](https://github.com/Azure/mmlspark)

*Apache®, o Apache Spark e Spark® são marcas registadas ou marcas comerciais do Foundation de Software Apache nos Estados Unidos e/ou noutros países.*
