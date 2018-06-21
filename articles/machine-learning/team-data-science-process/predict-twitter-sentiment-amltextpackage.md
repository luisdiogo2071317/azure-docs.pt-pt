---
title: Classificação de dados de sentimento com o pacote do Azure Machine Learning (AML) para análise de texto (AMLPTA) e o processo de ciência de dados (TDSP) da equipa do twitter | Microsoft Docs
description: Descreve a utilização de TDSP (processo de ciência de dados de equipa) e AMLPTA para classificação de dados de sentimento
services: machine-learning, team-data-science-process
documentationcenter: ''
author: deguhath
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: deguhath
ms.openlocfilehash: 559af47bcf41cd6af59f8ba1b27ff8e64e849925
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296170"
---
# <a name="twitter-sentiment-classification-with-azure-machine-learning-aml-package-for-text-analytics-amlpta-and-team-data-science-process-tdsp"></a>Classificação de dados de sentimento do twitter com o pacote do Azure Machine Learning (AML) para análise de texto (AMLPTA) e o processo de ciência de dados de equipa (TDSP)

## <a name="introduction"></a>Introdução
A uniformização da estrutura e a documentação de ciência de dados projetos, que é ancorada um estabelecida [ciclo de vida de ciência de dados](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), é importante para facilitar o início efetiva colaboração no equipas de ciência de dados.

Iremos tinha anteriormente lançadas um [repositório do GitHub para a estrutura de projeto TDSP e modelos](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Iremos agora tiver ativado a criação de projetos do Azure Machine Learning que são instanciado com [modelos de estrutura e a documentação de TDSP do Azure Machine Learning](https://github.com/amlsamples/tdsp). São fornecidas instruções sobre como utilizar a estrutura TDSP e modelos no Azure Machine Learning [aqui](https://docs.microsoft.com/en-us/azure/machine-learning/preview/how-to-use-tdsp-in-azure-ml). 

Neste exemplo, vamos demonstrar a utilização do Azure Machine Learning pacote de análise de texto e TDSP para desenvolver e implementar modelos preditivos a classificação de dados de sentimento do Twitter.


## <a name="use-case"></a>Caso de utilização
### <a name="twitter-sentiment-polarity-sample"></a>Exemplo de polarity de sentimento do twitter
Este artigo utiliza uma amostra para lhe mostrar como instanciar e executar um projeto de Machine Learning. Este exemplo utiliza a estrutura TDSP e modelos no Azure Machine Learning Workbench. O exemplo completo é fornecido nestas instruções. A tarefa de modelação prevê polarity sentimento (positivo ou negativo) utilizando o texto de tweets. Este artigo descreve as tarefas de modelação de dados que são descritas as instruções. As instruções abrangem as seguintes tarefas:

1. Exploração de dados, formação e implementação de um modelo de machine learning que resolva o problema de predição descrita na descrição geral caso utilize. Dados de sentimento do twitter são utilizados para estas tarefas.
2. Execução do projeto, utilizando o modelo TDSP do Azure Machine Learning para este projeto. Para execução do projeto e relatórios, o ciclo de vida TDSP é utilizado.
3. Operationalization da solução diretamente a partir do Azure Machine Learning no serviço de contentor do Azure.

O projeto realça a utilização do pacote de análise de texto para o Azure Machine Learning.


## <a name="link-to-github-repository"></a>Associar ao repositório do GitHub
Ligação para o repositório do GitHub é [aqui](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction). 

### <a name="purpose"></a>Objetivo
O objetivo principal deste exemplo é mostrar como instanciar e executar uma projeto com a estrutura de processo de ciência de dados de equipa (TDSP) e modelos no Azure Machine Learning trabalho Bench de aprendizagem. Para esta finalidade, utilizamos [dados de sentimento do Twitter](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip). A tarefa de modelação é prever polarity sentimento (positivo ou negativo) utilizando o texto de tweets.

### <a name="scope"></a>Âmbito
- Exploração de dados, formação e implementação de um modelo de machine learning que abordar o problema de predição descrito na descrição geral de cenário de utilização.
- Execução do projeto no Azure Machine Learning utilizando o modelo de processo de ciência de dados de equipa (TDSP) do Azure Machine Learning para este projeto. Para execução do projeto e relatórios, vamos utilizar o ciclo de vida TDSP.
- Operationalization da solução diretamente a partir do Azure Machine Learning nos serviços de contentor do Azure.

O projeto destaca várias funcionalidades do Azure Machine Learning, como TDSP estrutura Instanciação do objeto e a utilização, a execução de código no Bench de trabalho do Azure Machine Learning e fácil operationalization nos serviços de contentor Azure utilizando o Docker e Kubernetes.

## <a name="team-data-science-process-tds"></a>Processo de ciência de dados de equipa (recebido)
Utilizamos os modelos de estrutura e a documentação de projeto do TDSP para executar este exemplo. Segue a [TDSP ciclo de vida](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/lifecycle). O projeto ser criado com base nas instruções fornecidas [aqui](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).


<img src="./media/predict-twitter-sentiment-amltextpackage/tdsp-lifecycle2.png" alt="tdsp-lifecycle" width="800" height="600">


## <a name="use-case-overview"></a>Descrição geral de cenário de utilização
A tarefa é prever polarity de binários de cada twitter sentimento utilizar funcionalidades de embeddings word extraídas a partir do texto do twitter. Para obter uma descrição detalhada, consulte este [repositório](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction).

### <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Aquisição de dados e a compreensão](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Neste exemplo, o primeiro passo é transferir o conjunto de dados sentiment140 e dividi-la em formação e testar conjuntos de dados. Sentiment140 conjunto de dados contém o conteúdo real o tweet (com emoticons removidos) juntamente com o polarity de cada uma do tweet (negativo = 0, positivo = 4), com tweets independentes removidos. Os dados de formação resultante tem 1.3 milhões de linhas e dados de teste tem 320 linhas de k.

### <a name="modelinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode02modeling"></a>[Modelação](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/02_modeling)
Esta parte do exemplo mais está dividido em três subparts: 
- **Engenharia da funcionalidade** corresponde da geração de funcionalidades através do word diferentes ao incorporar o algoritmo, nomeadamente Word2Vec. 
- **Criação de modelo** oportunidades com a preparação de modelos diferentes, como _regressão logística da_ e _os aumentos gradação_ para prever sentimento texto de entrada. 
- **Modelo de avaliação** aplica-se o modelo treinado sobre dados de teste.

#### <a name="feature-engineering"></a>Com engenharia
Utilizamos <b>Word2Vec</b> para gerar word embeddings. Primeiro vamos utilizar o algoritmo de Word2Vec no modo de Skipgram conforme explicado no documento [Mikolov, Tomas, definir usados. Representações distribuídas palavras e expressões e as respetivas compositionality. Avanços nas informações neuronal sistemas de processamento de mensagens em fila. 2013.](https://arxiv.org/abs/1310.4546) para gerar word embeddings.

Ignorar grama é uma rede neuronal shallow colocar o word de destino codificados como um vetor de acesso frequente um como entrada e utilizá-la para prever próximas palavras. Se V é o tamanho de vocabulário, em seguida, o tamanho da camada de saída seria __C * V__ onde C é o tamanho da janela de contexto. A arquitetura de ignorar-grama com base é mostrada na figura seguinte.
 
<table class="image" align="center">
<caption align="bottom">Ignorar grama modelo</caption>
<tr><td><img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/06/05000515/Capture2-276x300.png" alt="Skip-gram model"/></td></tr>
</table>

Os detalhes do modelo de algoritmo e ignorar grama de Word2Vec ultrapassam o âmbito deste exemplo e leitores interessados são pedidos para percorrer as ligações seguintes para obter mais detalhes. O 02_A_Word2Vec.py código referenciado [tensorflow exemplos](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)

* [Representação de vetor de palavras](https://www.tensorflow.org/tutorials/word2vec)
* [Exatamente como funciona o word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
* [Notas sobre o ruído de estimativa Contrastive e amostragem negativa](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)

Depois de terminar o processo de preparação, dois ficheiros incorporar no formato de TSV são gerados para a fase de modelação.

#### <a name="model-training"></a>modelo de formação
Depois de tem sido gerados vetores de word com o algoritmo SSWE ou Word2vec, o passo seguinte é preparar os modelos de classificação para prever polarity sentimento real. Iremos aplicam-se os dois tipos de funcionalidades: Word2Vec e SSWE em dois modelos: regressão logística da e redes neuronal Convolutional (CNN). 

#### <a name="model-evaluation"></a>Modelo de avaliação
Fornecemos código sobre como carregar e avaliar vários modelos de formação num conjunto de dados de teste.


### <a name="deploymenthttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode03deployment"></a>[Implementação](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/03_deployment)
Esta parte fornecemos ponteiros para obter instruções sobre como operacionalizar um modelo de previsão sentimento previamente preparado para um serviço web num cluster no serviço de contentor do Azure (AKS). O ambiente de operationalization Aprovisiona Docker e Kubernetes do cluster para gerir a implementação de serviço web. Pode encontrar mais informações sobre o processo de operationalization [aqui](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Conclusão
Iremos Ocorreu através dos detalhes sobre como preparar um modelo de embedding word Word2Vec a utilizar e, em seguida, utilizar o embeddings extraída como as funcionalidades para preparar os dois modelos diferentes para prever a classificação de dados de sentimento do Twitter dados de texto. Um destes modelos é implementado nos serviços de contentor do Azure (AKS). 

## <a name="next-steps"></a>Passos Seguintes
Ler mais documentação no [Azure Machine Learning pacote de análise de texto (AMLPTA)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/textanalytics?view=azure-ml-py-latest) e [processo de ciência de dados de equipa (TDSP)](https://aka.ms/tdsp) para começar a utilizar.

## <a name="references"></a>Referências
* [Processo de Ciência de Dados de Equipa](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Como utilizar o processo de ciência de dados de equipa (TDSP) no Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [Modelo de projeto TDSP do Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Bench de trabalho do Azure ML](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [Mikolov, Tomas, definir usados. Representações distribuídas palavras e expressões e as respetivas compositionality. Avanços nas informações neuronal sistemas de processamento de mensagens em fila. 2013.](https://arxiv.org/abs/1310.4546)
