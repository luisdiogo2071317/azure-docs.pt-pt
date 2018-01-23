---
title: "Descrição geral Machine learning - Azure HDInsight | Microsoft Docs"
description: "Descreve a aprendizagem automática opções no HDInsight."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: nitinme
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
ms.author: nitinme
ms.openlocfilehash: ff99a7a60573cad5e6dd30d4ca48903423e9f87f
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/22/2018
---
# <a name="machine-learning-on-hdinsight"></a>Machine learning no HDInsight

HDInsight permite aprendizagem com macrodados que fornece a capacidade para obter informações importantes de grandes quantidades (petabytes ou até mesmo exabytes) de structured, não estruturados e mover de fast dados. Existem várias aprendizagem opções no HDInsight: SparkML e MLlib, R, Hive e o Toolkit de cognitivos.

## <a name="sparkml-and-mllib"></a>SparkML e o MLlib

[HDInsight Spark](spark/apache-spark-overview.md) é uma oferta alojado no Azure de [Spark](http://spark.apache.org/), uma origem unificada, abra, a arquitetura de processamento de dados paralelo que suporta o processamento dentro da memória para melhorar a análise de macrodados. O motor de processamento do Spark foi concebido para velocidade, facilidade de utilização e efetuar análises sofisticadas. Capacidades de computação distribuída na memória do Spark tornam uma boa opção para algoritmos iterativos utilizado no computações de machine learning e do graph. Existem duas bibliotecas de aprendizagem máquina escalável que colocar as capacidades de modelação usam este ambiente distribuído: MLlib e SparkML. MLlib contém a API original desenvolvida RDDs. SparkML é um pacote mais recente que fornece uma API de nível mais elevada desenvolvida DataFrames para construir os pipelines de ML. SparkML ainda não suporta todas as funcionalidades de MLlib, mas está a substituir o MLlib como padrão biblioteca de machine do Spark learning.

A biblioteca do Microsoft Machine Learning para Apache Spark está [MMLSpark](https://github.com/Azure/mmlspark). Esta biblioteca foi concebida para torná-cientistas de dados mais produtivos no Spark, aumentar a taxa de experimentação e tirar partido de técnicas de aprendizagem inovadores, incluindo a aprendizagem profunda, muito grandes conjuntos de dados. MMLSpark fornece uma camada sobreposta APIs de baixo nível do SparkML quando a criação de modelos de ML dimensionáveis, por exemplo, cadeias de indexação, coercing dados para um esquema esperado pela máquina algoritmos de aprendizagem e reuni vetores de funcionalidade. A biblioteca de MMLSpark simplifica a estas e outras tarefas comuns para a criação de modelos no PySpark.

## <a name="r"></a>R

[R](https://www.r-project.org/) está atualmente a linguagem de programação análises mais popular do mundo. É uma ferramenta de visualização de dados de código aberto com uma Comunidade de mais 2,5 milhões de utilizadores e a crescer. Com o respetivo utilizador thriving base e mais 8.000 pacotes contribuídos, R é uma opção provável para muitas empresas que necessitam de aprendizagem. Pode criar um cluster do HDInsight com o servidor R pronta para ser utilizada com modelos e conjuntos de dados gigantescos. Esta capacidade fornece cientistas de dados e statisticians com uma interface de R familiar, que pode dimensionar a pedido através do HDInsight, sem a sobrecarga de configuração do cluster e de manutenção.

![Formação da predição com o servidor R](./media/hdinsight-machine-learning-overview/r-training.png)

O nó de extremidade de um cluster fornece um local conveniente para ligar ao cluster e executar os scripts de R.  Também tem a opção para executar scripts de R em todos os nós do cluster através Hadoop mapa reduzir do ScaleR ou contextos de computação de Spark.

Com o servidor R no HDInsight com o Spark, pode parallelize formação em todos os nós de um cluster utilizando um contexto de computação de Spark. Pode executar scripts de R diretamente no nó de extremidade, utilizando todos os núcleos disponíveis em paralelo, conforme necessário. Em alternativa, pode executar código a partir do nó de extremidade para iniciar o processamento é distribuído por todos os nós do cluster. R Server no HDInsight com o Spark também lhe permite parallelizing funções a partir de pacotes de open source para R, se assim o desejar.

## <a name="azure-machine-learning-and-hive"></a>Ramo de registo e de aprendizagem do Azure

O Azure Machine Learning fornece ferramentas para Análise Preditiva modelo, bem como um serviço completamente gerido que pode utilizar para implementar os seus modelos preditivos como serviços web prontos a consumir. O Azure Machine Learning é uma solução de completas de Análise Preditiva na nuvem que pode utilizar para criar, testar, operacionalizar e gerir modelos preditivos. Selecione uma grande biblioteca de algoritmos, utilize um studio baseada na web para a criação de modelos e implementar facilmente o seu modelo como um serviço web.

![Efetuar análise acessível ao Hadoop com o Microsoft Azure Machine Learning avançada](./media/hdinsight-machine-learning-overview/hadoop-azure-ml.png)

Criar funcionalidades para o cluster de dados num Hadoop do HDInsight utilizando [ramo de registo de consultas](../machine-learning/team-data-science-process/create-features-hive.md). *Engenharia da funcionalidade* tenta aumentar a potência preditiva de algoritmos de aprendizagem através da criação de funcionalidades de dados não processados que facilitam o processo de aprendizagem. Pode executar consultas de HiveQL a partir do Azure ML e aceder a dados processados no ramo de registo e armazenados no blob storage, utilizando o [módulo importar dados](../machine-learning/studio/import-data.md).

## <a name="microsoft-cognitive-toolkit"></a>Microsoft Cognitive Toolkit

[Learning profunda](https://www.microsoft.com/en-us/research/group/dltc/) é um ramo de aprendizagem que utiliza as redes neurais, inspiradas pelos processos biological do "humana-brain". Os investigadores muitos Consulte learning profunda como uma abordagem promising otimização artificial intelligence. Exemplos de aprendizagem profunda são por tradutores automáticos de idioma ditas, sistemas de reconhecimento de imagem e raciocínio máquina.

Para ajudar a produzir o seu próprio trabalho no learning profunda, a Microsoft desenvolveu o livre, fácil de utilizar, open source [Toolkit de cognitivos](https://www.microsoft.com/en-us/cognitive-toolkit/). Este toolkit está a ser utilizado por uma vasta gama de produtos Microsoft, empresas em todo o mundo com a necessidade de implementar learning profunda em escala e alunos interessados em técnicas e algoritmos mais recente. 

## <a name="see-also"></a>Consulte também

### <a name="scenarios"></a>Cenários

* [Spark com Machine Learning: Utilizar o Spark no HDInsight para analisar a temperatura do edifício com dados de AVAC](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Gerar recomendações de filmes com o Mahout](hadoop/apache-hadoop-mahout-linux-mac.md)
* [Ramo de registo e de aprendizagem do Azure](../machine-learning/team-data-science-process/create-features-hive.md)
* [Ramo de registo e o Azure Machine Learning ponto-a-ponto](../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Machine learning com o Spark no HDInsight](../machine-learning/team-data-science-process/spark-overview.md)

### <a name="deep-learning-resources"></a>Recursos de aprendizagem profunda

* [Toolkit de aprendizagem avançada com o Spark](https://blogs.technet.microsoft.com/machinelearning/2017/04/25/using-microsofts-deep-learning-toolkit-with-spark-on-azure-hdinsight-clusters/)
* [Classificação de imagem constrangedoramente paralelas com o toolkit cognitivos + Tensorflow no Spark](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
