---
title: O que é o Azure Machine Learning? | Microsoft Docs
description: Explica os conceitos básicos de machine learning na nuvem, descreve como pode utilizá-lo e define os termos de machine learning. Descrição geral do Azure Machine Learning, uma solução de ciência de dados completa e integrada para cientistas de dados profissionais, que lhes permite desenvolver, experimentar e implementar aplicações de análise avançada à escala da cloud.
services: machine-learning
author: mwinkle
ms.author: mwinkle
manager: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.topic: get-started-article
ms.date: 09/21/2017
ms.openlocfilehash: 8a92f42ecee926042e9e0662f6b0bd9438024248
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/29/2018
---
# <a name="what-is-machine-learning"></a>O que é o Machine Learning?

A aprendizagem automática é uma técnica da ciência de dados que permite aos computadores utilizar dados existentes para prever futuros comportamentos, resultados e tendências. Ao utilizarem a aprendizagem automática, os computadores aprendem sem serem explicitamente programados.

As previsões ou predições da aprendizagem automática podem tornar as aplicações e os dispositivos mais inteligentes. Quando faz compras online, o Machine Learning recomenda outros produtos que poderá gostar com base nos que já comprou. Quando o seu cartão de crédito é usado, o machine learning compara a transação com uma base de dados de transações e ajuda a detetar fraudes. Quando o robô aspirador limpa uma sala, o machine learning ajuda-o a decidir se a tarefa está concluída.

## <a name="what-is-azure-machine-learning"></a>O que é o Azure Machine Learning?
O Azure Machine Learning é uma solução de análise avançada e de ciência de dados integrada e completa. Permite que os cientistas de dados prepararem dados, desenvolvam experimentações e implementem modelos à escala da cloud.

Os componentes principais do Azure Machine Learning são:
- Azure Machine Learning Workbench
- Serviço de Experimentação do Azure Machine Learning
- Serviço de Gestão de Modelos do Azure Machine Learning
- Bibliotecas do Microsoft Machine Learning para Apache Spark (biblioteca MMLSpark)
- Visual Studio Code Tools para IA

Em conjunto, estes serviços e aplicações ajudam a acelerar significativamente o desenvolvimento e a implementação do seu projeto de ciência de dados. 

![Conceitos do Azure Machine Learning](media/overview-what-is-azure-ml/aml-concepts.png)


## <a name="open-source-compatible"></a>Compatível com código aberto

O Azure Machine Learning suporta totalmente as tecnologias de código aberto. Pode utilizar dezenas de milhares de pacotes Python de código aberto, como as arquiteturas de machine learning seguintes:

- [scikit-learn](http://scikit-learn.org/)
- [TensorFlow](https://www.tensorflow.org/)
- [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/)
- [Spark ML](https://spark.apache.org/docs/2.1.1/ml-pipeline.html)

Pode executar as experimentações em ambientes geridos, como contentores do Docker e clusters do Spark. Também pode utilizar hardware avançado, como [máquinas virtuais ativadas para GPU no Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu), para acelerar a execução.

O Azure Machine Learning foi concebido sobre as tecnologias de código aberto seguintes:

- [Bloco de Notas do Jupyter](http://jupyter.org/)
- [Apache Spark](https://spark.apache.org/)
- [Docker](https://www.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Python](https://www.python.org/)
- [Conda](https://conda.io/docs/)

Também inclui as tecnologias de código aberto da própria Microsoft, tais como a [Biblioteca do Microsoft Machine Learning para Apache Spark](https://github.com/Azure/mmlspark) e o Cognitive Toolkit.

Além disso, também pode tirar partido de algumas das mais avançadas e comprovadas tecnologias de machine learning desenvolvidas pela Microsoft para resolver problemas em grande escala. Estas tecnologias são testadas em muitos produtos Microsoft, entre os quais:

- Windows
- Bing
- Xbox
- Office
- SQL Server

As tecnologias de machine learning da Microsoft abaixo são algumas das que estão incluídas no Azure Machine Learning:

- [PROSE](https://microsoft.github.io/prose/) (PrOgram Synthesis using Examples)
- [microsoftml](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package)
- [revoscalepy](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package)

## <a name="azure-machine-learning-workbench"></a>Azure Machine Learning Workbench
O Azure Machine Learning Workbench é uma aplicação de computador, que inclui ferramentas de linha de comandos, suportada em Windows e em macOS. Permite-lhe gerir soluções de machine learning ao longo de todo o ciclo de vida da ciência de dados:

- Ingestão e preparação de dados
- Desenvolvimento de modelos e gestão de experimentações
- Implementação de modelos em vários ambientes de destino

Seguem-se as principais funcionalidades que o Azure Machine Learning Workbench oferece:

- Ferramenta de preparação de dados , que pode aprender a lógica da transformação de dados através de exemplos.
- Abstração de origens de dados, acessível através de UX e código Python.
- SDK Python para invocar pacotes de preparação de dados construídos visualmente.
- Serviço Jupyter Notebook incorporado e UX cliente.
- Monitorização e gestão de experimentações mediante vistas de histórico de execuções.
- Controlo de acesso baseado em funções que permite a partilha e a colaboração através do Azure Active Directory.
- Instantâneos de projetos automáticos para cada execução e controlo de versões explícito, permitido pela integração de Git nativa. 
- Integração com IDEs de Python populares.

Para obter mais informações, veja os artigos seguintes:
- [Data Preparation User Guide](data-prep-user-guide.md) (Guia de Utilizador da Preparação de Dados)
- [Using Git with Azure Machine Learning](using-git-ml-project.md) (Utilizar o Git com o Azure Machine Learning)
- [Using Jupyter Notebook in Azure Machine Learning](how-to-use-jupyter-notebooks.md) (Utilizar o Jupyter Notebook no Azure Machine Learning)
- Roaming and Sharing (Roaming e Partilha)
- [Run History Guide](how-to-use-run-history-model-metrics.md) (Guia do Histórico de Execuções)
- [IDE Integration](how-to-configure-your-IDE.md) (Integração de IDEs)

## <a name="azure-machine-learning-experimentation-service"></a>Serviço de Experimentação do Azure Machine Learning
O Serviço de Experimentação processa a execução de experimentações de machine learning. Também suporta o Workbench, ao proporcionar gestão de projetos, integração do Git, controlo de acesso, roaming e partilha. 

Através de uma configuração fácil, pode executar as suas experiências numa gama de opções de ambientes de computação:

- Nativo local
- Contentor do Docker local
- Contentor do Docker numa VM remota
- Cluster do Spark no Azure de aumento horizontal

O Serviço de Experimentação constrói ambientes virtuais para garantir que o seu script pode ser executado em isolamento com resultados reproduzíveis. Regista as informações do histórico de execuções e apresenta-o visualmente. Pode facilmente selecionar o melhor modelo de entre as execuções da experimentação. 

Para obter mais informações, veja [Configuração do Serviço de Experimentação](experimentation-service-configuration.md).

## <a name="azure-machine-learning-model-management-service"></a>Serviço de Gestão de Modelos do Azure Machine Learning

O Serviço de Gestão de Modelos permite aos cientistas de dados e às equipas de dev-ops implementar modelos preditivos numa grande variedade de ambientes. As versões e a linhagem dos modelos são monitorizados das execuções de preparação às implementações. Os modelos são armazenados, registados e geridos na cloud. 

Ao utilizar comandos da CLI simples, pode contentorizar o seu modelo, os scripts de classificação e as dependências em imagens do Docker. Estas imagens são registadas no seu próprio registo do Docker alojado no Azure (Azure Container Registry). Podem ser implementadas de forma fiável nos destinos seguintes:

- Máquinas locais
- Servidores no local
- Cloud
- Dispositivos IoT de periferia

É utilizado o Kubernetes em execução no Azure Container Service (ACS) para a implementação de aumento horizontal na cloud. A telemetria da execução do modelo é capturada no AppInsights, para análise visual. 

Para obter mais informações sobre o Serviço de Gestão de Modelos, veja [Model Management Overview](model-management-overview.md) (Descrição Geral do Modelo de Gestão).


## <a name="microsoft-machine-learning-library-for-apache-spark"></a>Biblioteca do Microsoft Machine Learning para Apache Spark

A [MMLSpark](https://github.com/Azure/mmlspark)(Biblioteca do Microsoft Machine Learning para Apache Spark) é um pacote Spark de código aberto que disponibiliza aprendizagem profunda e ferramentas de ciência de dados para Apache Spark. Integra o [Machine Learning Pipelines do Spark](https://spark.apache.org/docs/2.1.1/ml-pipeline.html) no [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) e na biblioteca [OpenCV](http://opencv.org/). Permite-lhe criar rapidamente modelos preditivos e analíticos poderosos e altamente dimensionáveis para conjuntos de imagens e textos de grandes dimensões. Alguns destaques incluem:

- Ingestão de imagens fácil do HDFS no Spark DataFrame
- Pré-processamento de dados de imagens através de transformações a partir de OpenCV
- Caracterização de imagens mediante a utilização de redes neurais profundas pré-preparadas com o Microsoft Cognitive Toolkit 
- Utilização de LSTMs bidirecionais pré-preparadas a partir da Keras para extração de entidades médicas
- Preparação de modelos de classificação de imagens baseadas em DNN em VMs GPU da Série N no Azure
- Caracterização de dados de textos livres mediante a utilização de APIs práticas com base em primitivos no SparkML através de um único transformador
- Preparação fácil de modelos de classificação e regressão mediante a caracterização implícita de dados
- Calcular um conjunto rico de métricas de avaliação, incluindo métricas por instância individual

Para obter mais informações, veja [Using MMLSpark in Azure Machine Learning](how-to-use-mmlspark.md) (Utilizar o MMLSspark no Azure Machine Learning)

## <a name="visual-studio-code-tools-for-ai"></a>Visual Studio Code Tools para IA
As Ferramentas do Visual Studio Code para AI são uma extensão do Visual Studio Code para criar, testar e implementar soluções de Aprendizagem Profunda e AI. Conta com muitos pontos de integração no Azure Machine Learning, incluindo:
- Uma vista do histórico de execuções que apresenta o desempenho das execuções de preparação e as métricas registadas.
- Uma vista de galeria, que permite aos utilizadores procurar e iniciar projetos novos com o Microsoft Cognitive Toolkit, o TensorFlow e muitas outras arquiteturas de aprendizagem profunda. 
- Uma vista de explorador para selecionar destinos de computação para os seus scripts executarem.
 

## <a name="what-are-the-machine-learning-options-from-microsoft"></a>Quais são as opções de machine learning da Microsoft?
Para além do Azure Machine Learning, existe uma ampla gama de opções no Azure para criar, implementar e gerir modelos de machine learning. [Saiba mais sobre as mesmas aqui.](overview-more-machine-learning.md)

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="next-steps"></a>Passos seguintes
* [Install and create Azure Machine Learning](quickstart-installation.md) (Instalar e criar o Azure Machine Learning)
