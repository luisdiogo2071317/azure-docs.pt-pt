---
title: Com o pacote do Azure Machine Learning (AML) para análise de texto (AMLPTA) e o processo de ciência de dados de equipa (TDSP), a classificação de sentimentos do twitter | Documentos da Microsoft
description: Descreve a utilização do TDSP (Team Data Science Process) e AMLPTA para a classificação de sentimentos
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
ms.openlocfilehash: 9e5018bc4c7b90897f7f8c91169410284217b172
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45577013"
---
# <a name="twitter-sentiment-classification-with-azure-machine-learning-aml-package-for-text-analytics-amlpta-and-team-data-science-process-tdsp"></a>Classificação de sentimento do twitter com o pacote do Azure Machine Learning (AML) para análise de texto (AMLPTA) e o processo de ciência de dados de equipa (TDSP)

## <a name="introduction"></a>Introdução
Padronização da estrutura e a documentação da ciência de dados de projetos, ou seja ancorada a uma estabelecido [ciclo de vida de ciência de dados](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), é fundamental para promovendo a colaboração eficaz em equipes de ciência de dados.

Tinha anteriormente lançámos um [repositório do GitHub para a estrutura do projeto TDSP e os modelos](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Agora ativámos a criação de projetos de Azure Machine Learning, que são instanciadas com [modelos de estrutura e a documentação do TDSP para o Azure Machine Learning](https://github.com/amlsamples/tdsp). São fornecidas instruções sobre como utilizar a estrutura do TDSP e modelos no Azure Machine Learning [aqui](https://docs.microsoft.com/azure/machine-learning/preview/how-to-use-tdsp-in-azure-ml). 

Neste exemplo, vamos demonstrar a utilização do pacote do Azure Machine Learning para análise de texto e TDSP para desenvolver e implementar modelos preditivos para classificação de sentimento do Twitter.


## <a name="use-case"></a>Caso de utilização
### <a name="twitter-sentiment-polarity-sample"></a>Exemplo de polarity de sentimento do twitter
Este artigo utiliza um exemplo para mostrar como criar uma instância e executar um projeto de Machine Learning. Este exemplo utiliza a estrutura TDSP e modelos no Azure Machine Learning Workbench. O exemplo completo é fornecido nestas instruções. A tarefa de modelagem prevê polarity de sentimento (positivo ou negativo) usando o texto de tweets. Este artigo descreve as tarefas de modelação de dados que são descritas no passo a passo. Passo a passo abrange as seguintes tarefas:

1. Exploração de dados, treinamento e implantação de um modelo de aprendizagem automática que resolva o problema de predição que está descrito com a descrição de geral de casos de utilização. Dados de sentimento do twitter são utilizados para estas tarefas.
2. Execução do projeto com o modelo TDSP do Azure Machine Learning para este projeto. Para a execução do projeto e relatórios, é utilizado o ciclo de vida do TDSP.
3. Operacionalização da solução diretamente a partir do Azure Machine Learning no Azure Container Service.

O projeto realça a utilização do pacote de análise de texto para o Azure Machine Learning.


## <a name="link-to-github-repository"></a>Ligar para o repositório do GitHub
Ligação para o repositório do GitHub está [aqui](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction). 

### <a name="purpose"></a>Objetivo
A principal finalidade deste exemplo é mostrar como criar uma instância e executar um projeto usando a estrutura de processo de ciência de dados de equipa (TDSP) e modelos no Azure Machine Learning trabalho banco de aprendizagem automática. Para essa finalidade, podemos usar [dados de sentimento do Twitter](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip). A tarefa de modelagem consiste em prever polarity de sentimento (positivo ou negativo) usando o texto de tweets.

### <a name="scope"></a>Âmbito
- Exploração de dados, formação e implementação de um modelo de aprendizagem automática que resolva o problema de predição descrito na descrição geral de caso de utilização.
- Execução do projeto no Azure Machine Learning com o modelo de processo de ciência de dados de equipa (TDSP) do Azure Machine Learning para este projeto. Para a execução do projeto e relatórios, vamos utilizar o ciclo de vida do TDSP.
- Operacionalização da solução diretamente a partir do Azure Machine Learning no Azure Container Service.

O projeto destaca vários recursos do Azure Machine Learning, tais Instanciação de estrutura do TDSP e a utilização, a execução de código no banco de trabalho do Azure Machine Learning e fácil operacionalização nos serviços de contentor do Azure utilizando o Docker e Kubernetes.

## <a name="team-data-science-process-tds"></a>Processo de ciência de dados de equipa (TDS)
Usamos os modelos de estrutura e a documentação de projeto do TDSP para executar este exemplo. Ela segue o [ciclo de vida do TDSP](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/lifecycle). O projeto é criado com base nas instruções fornecidas [aqui](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).


<img src="./media/predict-twitter-sentiment-amltextpackage/tdsp-lifecycle2.png" alt="tdsp-lifecycle" width="800" height="600">


## <a name="use-case-overview"></a>Descrição geral de caso de utilização
A tarefa é prever polarity de binários de sentimento de cada twitter usando recursos do word embeddings extraídos do texto do twitter. Para obter uma descrição detalhada, consulte este [repositório](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction).

### <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Aquisição de dados e compreensão](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Neste exemplo, a primeira etapa é baixar o conjunto de dados sentiment140 e dividi-la em train e conjuntos de dados de teste. Sentiment140 conjunto de dados contém o conteúdo real do tweet (com emoticons removidos), juntamente com o polarity de cada tweet (negativo = 0, positivo = 4), com tweets neutros removidos. Os dados de treinamento resultante tem 1,3 milhão de linhas e dados de teste tem 320 mil linhas.

### <a name="modelinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode02modeling"></a>[Modelagem](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/02_modeling)
Esta parte do exemplo é dividido em três subparts: 
- **"Feature Engineering"** corresponde à geração de recursos usando o algoritmo, ou seja Word2Vec de incorporação de palavras diferentes. 
- **Criação de modelos** gostam de negociações com a formação de modelos diferentes _regressão logística_ e _aumentam a gradação_ para prever os sentimentos do texto de entrada. 
- **Avaliação de modelo** aplica-se o modelo preparado através dos dados de testes.

#### <a name="feature-engineering"></a>Com engenharia
Usamos <b>Word2Vec</b> para gerar incorporações. Primeiro, usamos o algoritmo de Word2Vec no modo de Skipgram conforme explicado no documento [Mikolov, Tomas, e outros. Distribuída representações de palavras e frases e seus compositividade. Avanços nas informações neurais sistemas de processamento. 2013.](https://arxiv.org/abs/1310.4546) para gerar incorporações.

Ignorar-grama é uma rede neural rasa, levando a palavra de destino codificado como um vetor de acesso frequente um como entrada e usá-lo a prever nas proximidades de palavras. Se V é o tamanho de vocabulário, em seguida, o tamanho da camada de saída seria __C * V__ onde o C é o tamanho da janela do contexto. A arquitetura de grama-ignorar com base é mostrada na figura a seguir.
 
<table class="image" align="center">
<caption align="bottom">Modelo de grama-ignorar</caption>
<tr><td><img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/06/05000515/Capture2-276x300.png" alt="Skip-gram model"/></td></tr>
</table>

Os detalhes do modelo de algoritmo e ignorar-grama Word2Vec estão além do escopo deste exemplo e os leitores interessados são solicitados a percorrer as seguintes ligações para obter mais detalhes. O 02_A_Word2Vec.py código referenciado [exemplos do tensorflow](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)

* [Representação de vetor de palavras](https://www.tensorflow.org/tutorials/word2vec)
* [Exatamente como funciona a word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
* [Notas sobre a estimativa de Contrastive de ruído e amostragem negativa](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)

Depois de terminar o processo de treinamento, dois arquivos de incorporação no formato de TSV são gerados para o estágio de modelagem.

#### <a name="model-training"></a>Preparação de modelos
Depois dos vetores de word tenham sido gerados através do algoritmo SSWE ou Word2vec, a próxima etapa é preparar os modelos de classificação para prever polarity sentimentos real. Vamos aplicar os dois tipos de recursos: Word2Vec e SSWE em dois modelos: regressão logística e redes Neural Convolucional (CNN). 

#### <a name="model-evaluation"></a>Avaliação do modelo
Fornecemos código sobre como carregar e avaliar os vários modelos de formação no conjunto de dados de teste.


### <a name="deploymenthttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode03deployment"></a>[Implementação](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/03_deployment)
Nesta parte, fornecemos ponteiros para obter instruções sobre como operacionalizar um modelo de predição de sentimentos com formação prévia para um serviço da web num cluster no Azure Container Service (AKS). O ambiente de operacionalização Aprovisiona o Docker e Kubernetes no cluster para gerenciar a implantação de serviço web. Pode encontrar mais informações sobre o processo de operacionalização [aqui](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Conclusão
E passamos pelos detalhes sobre como preparar um modelo de incorporação do word usando Word2Vec e, em seguida, utilize o embeddings extraídos como recursos a criar dois modelos diferentes para prever a classificação de sentimento do Twitter dados de texto. Um desses modelos é implementado no Azure Container Services (AKS). 

## <a name="next-steps"></a>Passos Seguintes
Leia mais documentação sobre [pacote do Azure Machine Learning para análise de texto (AMLPTA)](https://docs.microsoft.com/python/api/overview/azure-machine-learning/textanalytics?view=azure-ml-py-latest) e [Team Data Science Process (TDSP)](https://aka.ms/tdsp) para começar a utilizar.

## <a name="references"></a>Referências
* [Processo de Ciência de Dados de Equipa](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) 
* [Como utilizar o processo de ciência de dados de equipa (TDSP) no Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [Modelo de projeto TDSP para o Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Banco de trabalho do Azure ML](https://docs.microsoft.com/azure/machine-learning/preview/)
* [Mikolov, Tomas, e outros. Distribuída representações de palavras e frases e seus compositividade. Avanços nas informações neurais sistemas de processamento. 2013.](https://arxiv.org/abs/1310.4546)
