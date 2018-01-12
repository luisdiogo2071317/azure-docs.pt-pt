---
title: "Descrição detalhada da-avançada de análise - Azure HDInsight | Microsoft Docs"
description: "Saiba como avançada de análise utiliza algoritmos para processar macrodados."
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 61a00c902be5cd3e37dabba09c15f9226e5e88b4
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="deep-dive---advanced-analytics"></a>Descrição detalhada da-análise avançada

## <a name="what-is-advanced-analytics-for-hdinsight"></a>O que é avançado de análise para o HDInsight?

O HDInsight fornece a capacidade para obter informações importante sobre grandes quantidades de structured, não estruturados, dados e de mover o fast. Análise avançada é a utilização das arquiteturas altamente dimensionáveis, análises e modelos de machine learning e dashboards inteligentes para lhe fornecer informações importantes. Machine learning, ou *Análise Preditiva*, utiliza algoritmos que identificarem e obter informações de relações nos seus dados para fazer predições e as suas decisões de guia.

## <a name="advanced-analytics-process"></a>Processo de análise avançada

![Processo](./media/apache-hadoop-deep-dive-advanced-analytics/process.png)

Depois de ter identificado o problema empresarial e de iniciar a recolha e processar os dados, tem de criar um modelo que representa a pergunta que pretende prever. O seu modelo irá utilizar um ou mais algoritmos do machine learning para tornar o tipo de predição que melhor se adequa às suas necessidades de negócio.  A maioria dos seus dados deve ser utilizada para preparar o seu modelo com o resto utilizado para testar ou avaliá-lo. 

Depois de criar, carregar, teste e avaliar o seu modelo, o passo seguinte é implementar o seu modelo, de modo a que começa fornecer respostas às suas perguntas. O último passo consiste em monitorizar o desempenho do seu modelo e otimizá-la conforme for necessário. 

## <a name="common-types-of-algorithms"></a>Tipos de algoritmos comuns

Soluções de análise avançadas fornecem um conjunto de algoritmos do machine learning. Eis um resumo das categorias de algoritmos e associados casos de utilização empresarial comuns.

![Casos de utilização do Machine Learning](./media/apache-hadoop-deep-dive-advanced-analytics/ml-use-cases.png)

Juntamente com a selecionar o algorithm(s) best-fitting, tem de ponderar se pretende ou não tem de fornecer dados para formação. Algoritmos de Machine learning são classificados da seguinte forma:

* Supervisionado - algoritmo tem de ser preparado num conjunto de dados identificados antes de poder fornecer resultados
* Por supervisionado - algoritmo pode ser aumentado mediante destinos Extras através de uma consulta interativo por um trainer, que não estavam disponíveis durante a fase inicial de formação
* Não supervisionado - algoritmo não necessita de dados de preparação 
* Reinforcement - o algoritmo utiliza os agentes de software para determinar o comportamento ideal dentro de um contexto específico (frequentemente utilizado no robotics)


| Categoria do algoritmo| Utilizar | Tipo de aprendizagem | Algoritmos |
| --- | --- | --- | -- |
| Classificação | Classificar pessoas ou ações em grupos | Supervisionado | Árvores de decisões, regressão logística da, as redes neurais |
| Clustering | Divisão de um conjunto de exemplos em grupos homogéneo | Não supervisionado | Significa-K clustering |
| Deteção de padrão | Identificar as associações frequentes dos dados | Não supervisionado | Regras de associação |
| Regressão | Prever resultados numéricos | Supervisionado | Regressão linear, as redes neurais |
| Reinforcement | Determinar comportamento ideal para robots | Reinforcement | Simulações Monte Carlo, DeepMind |

## <a name="machine-learning-on-hdinsight"></a>Machine learning no HDInsight

HDInsight tem vários do machine learning opções para um fluxo de trabalho de análise avançada:

* [Machine Learning e Spark](#machine-learning-and-spark)
* [Servidor r e R](#r-and-r-server)
* [Ramo de registo e de aprendizagem do Azure](#azure-machine-learning-and-hive)
* [O Spark e aprendizagem profunda](#spark-and-deep-learning)

### <a name="machine-learning-and-spark"></a>Machine Learning e Spark

[HDInsight Spark](../spark/apache-spark-overview.md) é uma oferta alojado no Azure de [Spark](http://spark.apache.org/), uma origem unificada, abrir, a arquitetura de processamento paralelo de dados que utiliza o processamento dentro da memória para melhorar a análise de macrodados. O motor de processamento do Spark foi concebido para velocidade, facilidade de utilização e efetuar análises sofisticadas. Capacidades de computação distribuída na memória do Spark tornam uma boa opção para algoritmos iterativos utilizado no computações de machine learning e do graph. 

Existem três bibliotecas de aprendizagem máquina escalável que colocar as capacidades de modelação usam este ambiente distribuído:

* [**MLlib** ](https://spark.apache.org/docs/latest/ml-guide.html) -MLlib contém a API original desenvolvida RDDs de Spark.
* [**SparkML** ](https://spark.apache.org/docs/1.2.2/ml-guide.html) -SparkML é um pacote mais recente que fornece uma API de nível mais elevada desenvolvida Spark DataFrames para construir os pipelines de ML.
* [**MMLSpark** ](https://github.com/Azure/mmlspark) - a Microsoft biblioteca de Machine Learning para Apache Spark (MMLSpark) foi concebido para tornar os cientistas de dados mais produtivos no Spark, para aumentar a taxa de experimentação e para tirar partido de aprendizagem inovadores técnicas, incluindo a aprendizagem profunda, muito grandes conjuntos de dados. A biblioteca de MMLSpark simplifica tarefas comuns de modelação para criação de modelos na PySpark. 

### <a name="r-and-r-server"></a>Servidor r e R

Como parte do HDInsight, pode criar um cluster do HDInsight com [R Server](../r-server/r-server-overview.md) pronta para ser utilizada com modelos e conjuntos de dados gigantescos. Esta nova funcionalidade fornece cientistas de dados e statisticians com uma interface de R familiar, que pode dimensionar a pedido através do HDInsight, sem a sobrecarga de configuração do cluster e de manutenção.

### <a name="azure-machine-learning-and-hive"></a>Ramo de registo e de aprendizagem do Azure

[Azure Machine Learning Studio](https://studio.azureml.net/) fornece ferramentas para Análise Preditiva modelo, bem como um serviço completamente gerido que pode utilizar para implementar os seus modelos preditivos como serviços web prontos a consumir. O Azure Machine Learning fornece ferramentas para criar soluções completas de Análise Preditiva na nuvem para rapidamente criar, testar, operacionalizar e gerir modelos preditivos. Selecione uma grande biblioteca de algoritmos, utilize um studio baseada na web para a criação de modelos e implementar facilmente o seu modelo como um serviço web.

### <a name="spark-and-deep-learning"></a>O Spark e aprendizagem profunda

[Learning profunda](https://www.microsoft.com/research/group/dltc/) é um ramo de aprendizagem que utiliza *as redes neurais profundo* (DNNs), inspired pelos processos biological do "humana-brain". Os investigadores muitos Consulte learning profunda como uma abordagem promising para artificial intelligence. Alguns exemplos de aprendizagem profunda são por tradutores automáticos de idioma ditas, sistemas de reconhecimento de imagem e raciocínio máquina. Para ajudar a produzir o seu próprio trabalho no learning profunda, a Microsoft desenvolveu o livre, fácil de utilizar, open source [Toolkit de cognitivos](https://www.microsoft.com/cognitive-toolkit/). O toolkit é amplamente utilizado por uma vasta gama de produtos Microsoft, empresas em todo o mundo com a necessidade de implementar learning profunda em escala e alunos interessados em técnicas e algoritmos mais recente. 

## <a name="scenario---score-images-to-identify-patterns-in-urban-development"></a>Cenário – imagens de classificação para identificar padrões de desenvolvimento urbano

Vamos rever um exemplo de um análise avançada de machine learning pipeline com o HDInsight.

Neste cenário, verá como DNNs produziu uma estrutura de aprendizagem profunda, Microsoft do Toolkit (CNTK cognitivos), pode ser operationalized para coleções de imagem grande armazenadas numa conta do Blob Storage do Azure utilizando PySpark num cluster do HDInsight Spark de classificação. Esta abordagem é aplicada a um caso de utilização DNN comuns, classificação de imagem com vista aérea e pode ser utilizada para identificar padrões recentes no desenvolvimento urbano.  Irá utilizar um modelo de classificação de imagem previamente preparado. O modelo é previamente treinado no [conjunto de dados de CIFAR 10](https://www.cs.toronto.edu/~kriz/cifar.html) e tenha sido aplicado a 10.000 imagens retiradas.

Existem três tarefas principais neste cenário de análise avançada:

1. Crie um cluster de Hadoop do Azure HDInsight com uma distribuição 2.1.0 do Apache Spark. 
2. Execute um script personalizado para instalar o Toolkit de cognitivos em todos os nós de um cluster do Azure HDInsight Spark. 
3. Carregar um bloco de notas do Jupyter pré-criadas ao cluster do HDInsight Spark para aplicar um treinado Microsoft cognitivos Toolkit profunda o modelo para ficheiros de uma conta de armazenamento de Blobs do Azure utilizando a API de Python do Spark (PySpark). 

Este exemplo utiliza a imagem de CIFAR 10 definir compilado e distribuídas por Alex Krizhevsky, Vinod Nair e Geoffrey Hinton. O conjunto de dados de CIFAR 10 contém 32 × 32 60 000 cor imagens que pertencem a 10 classes mutuamente exclusivas:

![Imagens](./media/apache-hadoop-deep-dive-advanced-analytics/ml-images.png)

Para obter mais detalhes sobre o conjunto de dados, consulte o artigo de Alex Krizhevsky [Learning várias camadas de funcionalidades a partir de imagens muito pequena](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf).

O conjunto de dados foi particionado para um conjunto de preparação de 50 000 imagens e um conjunto de teste de 10 000 imagens. O primeiro conjunto foi utilizado para preparar um modelo de rede residuais avançada de camada de vinte e convolutional (ResNet) utilizando o Toolkit de cognitivos seguindo [neste tutorial](https://github.com/Microsoft/CNTK/tree/master/Examples/Image/Classification/ResNet) do repositório GitHub do Toolkit cognitivos. As imagens de 10 000 restantes foram utilizadas para testar a precisão do modelo. Trata-se em que a informática distribuída se entra play: a tarefa de processamento previamente e as imagens de classificação é altamente paralelizável. Com o modelo treinado guardado na mão, utilizámos:

* PySpark para distribuir as imagens e o modelo treinado para nós de trabalho do cluster.
* Python para processar previamente as imagens em cada nó do cluster do HDInsight Spark.
* Toolkit cognitivos ao carregar o modelo e Pontuar as imagens de pré-processadas em cada nó.
* Blocos de notas do Jupyter para executar o script do PySpark, agregar os resultados e utilizar [Matplotlib](https://matplotlib.org/) para visualizar o desempenho do modelo.

O todo pré-processamentos/classificação das 10 000 imagens demora menos de um minuto num cluster com 4 nós de trabalho. O modelo prevê com precisão as etiquetas de imagens do ~ 9,100 (91%). Uma matriz de confusão ilustra os erros mais comuns de classificação. Por exemplo, a matriz mostra que mislabeling dogs como cats e vice versa ocorre mais frequentemente de para outros pares de etiqueta.

![Resultados](./media/apache-hadoop-deep-dive-advanced-analytics/ml-results.png)

### <a name="try-it-out"></a>Experimente!

Siga [neste tutorial](../spark/apache-spark-microsoft-cognitive-toolkit.md) para implementar esta solução ponto-a-ponto: configurar um cluster do HDInsight Spark, instalar o Toolkit cognitivos e executar o bloco de notas do Jupyter pontuações 10 000 imagens CIFAR.

## <a name="next-steps"></a>Passos Seguintes

Ramo de registo e de aprendizagem do Azure

* [Ramo de registo e o Azure Machine Learning ponto-a-ponto](../../machine-learning/team-data-science-process/hive-walkthrough.md)
* [Utilizar um Cluster de Hadoop do Azure HDInsight um conjunto de dados de 1 TB](../../machine-learning/team-data-science-process/hive-criteo-walkthrough.md)

O Spark e o MLLib

* [Machine learning com o Spark no HDInsight](../../machine-learning/team-data-science-process/spark-overview.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para analisar a temperatura do edifício com dados de AVAC](../spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark com Machine Learning: Utilizar o Spark no HDInsight para prever resultados de inspeções alimentares](../spark/apache-spark-machine-learning-mllib-ipython.md)

Learning profunda, Toolkit cognitivos e outros

* [Classificação de imagem constrangedoramente paralelas, utilizando Toolkit cognitivos e TensorFlow no Azure HDInsight Spark](https://blogs.technet.microsoft.com/machinelearning/2017/04/12/embarrassingly-parallel-image-classification-using-cognitive-toolkit-tensorflow-on-azure-hdinsight-spark/)
* [Ciência de dados Máquina Virtual do Azure](../../machine-learning/data-science-virtual-machine/overview.md)
* [Introdução ao H2O.ai no Azure HDInsight](https://azure.microsoft.com/blog/introducing-h2o-ai-with-on-azure-hdinsight-to-bring-the-most-robust-ai-platform-for-enterprises/)
