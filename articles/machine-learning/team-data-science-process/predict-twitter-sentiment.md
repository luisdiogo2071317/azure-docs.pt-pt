---
title: "Prever sentimento do Twitter com embeddings word utilizando o processo de ciência de dados de equipa - Azure | Microsoft Docs"
description: "Os passos necessários para executar os seus projetos de ciência de dados"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: bradsev;
ms.openlocfilehash: fe1c87df40102a62e1e0c8873b25fa3df7d743bc
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/16/2017
---
# <a name="predict-twitter-sentiment-with-word-embeddings-using-the-team-data-science-process"></a>Prever sentimento do Twitter com embeddings word utilizando o processo de ciência de dados de equipa

Este artigo mostra como colaborar de forma eficaz ao utilizar o **Word2Vec** word incorporar algoritmo e o **algoritmo sentimento específico Word incorporar (SSWE)** para prever os dados de sentimento do Twitter com o [Do azure Machine Learning](../preview/index.yml). Para obter detalhes adicionais sobre a tarefa de prever polarity sentimento do twitter, consulte [repositório](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction). A chave para o facilitar o início colaboração em equipa Efetivo em projetos de ciência de dados é para uniformizar a estrutura e a documentação de projetos com um ciclo de vida de ciência de dados estabelecida. O [processo de ciência de dados de equipa (TDSP)](overview.md) fornece apenas esses um estruturados [ciclo de vida](lifecycle.md). 

Criação de projetos de ciência de dados com o **modelo TDSP** fornece esta estrutura padronizada para projetos do Azure Machine Learning. Anteriormente, a equipa TDSP tinha lançou um [repositório do GitHub para a estrutura de projeto TDSP e modelos](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Agora a criação de projetos do Azure Machine Learning que são instanciado com [modelos de estrutura e a documentação de TDSP do Azure Machine Learning](https://github.com/amlsamples/tdsp) foi ativado. Para obter instruções sobre como utilizar a estrutura TDSP e modelos no Azure Machine Learning, consulte [estrutura projetos com o modelo de processo de ciência de dados de equipa](../preview/how-to-use-tdsp-in-azure-ml.md). 


## <a name="the-sentiment-polarity-sample"></a>O exemplo de polarity sentiment

Um exemplo que mostra como instanciar e executar um machine learning utilizando a estrutura do processo de ciência de dados de equipa do projeto e modelos no Azure Machine Learning trabalho Bench tiver sido fornecido nisto [instruções](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md). A tarefa de modelação é prever polarity sentimento (positivo ou negativo) utilizando o texto de tweets. Este artigo descrevem os dados de modelação tarefas abrangidas as instruções. As instruções abrangem as seguintes tarefas:

- Exploração de dados, formação e implementação de um modelo de machine learning que abordar o problema de predição descrito na descrição geral de cenário de utilização. Para esta finalidade, [dados de sentimento do Twitter](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) é utilizado.
- Execução do projeto no Azure Machine Learning utilizando o modelo de processo de ciência de dados de equipa (TDSP) do Azure Machine Learning para este projeto. Para execução do projeto e relatórios, o ciclo de vida TDSP é utilizado.
- Operationalization da solução diretamente a partir do Azure Machine Learning nos serviços de contentor do Azure.

O projeto destaca várias funcionalidades do Azure Machine Learning, como TDSP estrutura Instanciação do objeto e a utilização, a execução de código no Bench de trabalho do Azure Machine Learning e fácil operationalization nos serviços de contentor Azure utilizando o Docker e Kubernetes.

## <a name="team-data-science-process"></a>Processo de Ciência de Dados de Equipa
Pode utilizar os modelos de estrutura e a documentação de projeto do TDSP para executar este exemplo. Segue a [TDSP ciclo de vida]((https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)). O projeto ser criado com base nas instruções fornecidas [aqui](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).

![Ciclo de vida TDSP](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

![Instanciar TDSP](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Aquisição de dados e a compreensão](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
O primeiro passo neste exemplo é para transferir o conjunto de dados sentiment140 e dividi-la em formação e testar conjuntos de dados. O conjunto de dados sentiment140 contém o conteúdo real o tweet (com emoticons removidos) juntamente com o polarity de cada uma do tweet (negativo = 0, positivo = 4), com tweets independentes removidos. Quando dividido, os dados de formação resultante tem 1.3 milhões de linhas e dados de teste tem 320 linhas de k.


## <a name="modelinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Modelação](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

Esta parte do exemplo é ainda mais dividido em três partes:
 
- **Engenharia da funcionalidade** corresponde a geração das funcionalidades diferentes word incorporar algoritmos a utilizar. 
- **Criação de modelo** oportunidades com a preparação de modelos diferentes, como _regressão logística da_ e _os aumentos gradação_ para prever sentimento texto de entrada. 
- **Modelo de avaliação** aplica-se o modelo treinado sobre dados de teste.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Engenharia da funcionalidade](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

Word2Vec SSWE, sendo o word incorporar algoritmos utilizados aqui para gerar word embeddings. 


#### <a name="word2vec"></a>Word2Vec

O algoritmo de Word2Vec é utilizado no modo de Skipgram. Desta forma de gerar palavra embeddings é explicada no documento através do Tomas Mikolov ter al.: [distribuídas representações de palavras e expressões e as respetivas Compositionality. Avanços nas informações neuronal sistemas de processamento de mensagens em fila. 2013.](https://arxiv.org/abs/1310.4546).

Ignorar grama é uma rede neuronal shallow. A entrada é a palavra de destino codificada como um vetor de acesso frequente um, que utiliza para prever próximas palavras. Se **V** é o tamanho de vocabulário, em seguida, o tamanho da camada de saída seria __C * V__ onde C é o tamanho da janela de contexto. A arquitetura de ignorar-grama com base é apresentada na figura seguinte:

![Ignorar grama modelo](./media/predict-twitter-sentiment/skip-gram-model.PNG)

***Ignorar grama modelo***

Os mechanics detalhadas do modelo de algoritmo e ignorar grama de word2vec estão fora do âmbito deste exemplo. Os leitores estão interessados em mais detalhes sobre os trabalhos podem consulte as seguintes referências:

- [O código 02_A_Word2Vec.py referenciado TensorFlow exemplos](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)
- [Representação de vetor de palavras](https://www.tensorflow.org/tutorials/word2vec)
- [Exatamente como funciona o word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
- [Notas sobre o ruído de estimativa Contrastive e amostragem negativa](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)


#### <a name="sswe"></a>SSWE
O **sentimento específico Word incorporar (SSWE) algoritmo** tenta ultrapassar uma vulnerabilidade do algoritmo Word2vec que as palavras com contextos semelhantes e opposite polarity podem ter vetores de word semelhantes. Este semelhança significa que não pode efetuar Word2vec com precisão para tarefas como a análise de dados de sentimento. O algoritmo SSWE tenta processar de acordo com esta vulnerabilidade incorporando polarity o frase e contexto da palavra para a respetiva função de perda.

Este exemplo utiliza uma variante de SSWE. O SSWE utiliza ambos o ngram original e ngram danificada como entrada e de utiliza um estilo de classificação hinge a função de perda de diferenças sintáticas perda e a perda de semântica. Função de perda Ultimate é a combinação ponderada da perda de diferenças sintáticas e perda de semântica. Para efeitos de simplicidade, apenas a semântica cruzada entropia é utilizada como a função de perda. Como ver mais tarde, mesmo com esta função de perda mais simples do desempenho de incorporar o SSWE é melhor do que o Word2Vec incorporar.

O modelo de rede neuronal inspirada SSWE que utilizar neste exemplo é mostrado na figura seguinte:

![Comparação do modelo de ROC](./media/predict-twitter-sentiment/embedding-model-2.PNG)

***Convolutional modelo para gerar incorporar específicos de dados de sentimento do word.***


Depois de terminar o processo de preparação, dois ficheiros incorporar no formato TSV são gerados para a fase de modelação.

Para obter mais informações sobre os algoritmos SSWE, consulte o documento através do Duyu Tang ter al.: [Learning específicos de dados de sentimento do Word ao incorporar para classificação de dados de sentimento do Twitter. ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146) 


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Criação de modelo](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Depois de tem sido gerados vetores de word utilizando um dos algoritmos de SSWE ou Word2vec, o passo seguinte é preparar os modelos de classificação para prever polarity sentimento real. Dois tipos de funcionalidades, Word2Vec e SSWE, estão a ser aplicados para dois modelos, o modelo GBM e o modelo de regressão logística da. Modelos diferentes, por isso, quatro estão a ser preparados.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Modelo de avaliação](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Agora utilizar os modelos de quatro preparados no passo anterior nos dados de testes para avaliar o desempenho do modelo. 

1. Gradação Boosting através de SSWE incorporar
2. Regressão logística da ativação pós-falha SSWE incorporar
3. Gradação Boosting através de Word2Vec incorporar
4. Regressão logística da ativação pós-falha Word2Vec incorporar

![Comparação do modelo de ROC](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

O modelo GBM com funcionalidades SSWE é o melhor com a métrica AUC.


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Implementação](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

Esta parte implementa o modelo de previsão sentimento treinado (SSWE incorporar + modelo GBM) para um serviço web num cluster no serviço de contentor do Azure (ACS). O ambiente de operationalization Aprovisiona Docker e Kubernetes do cluster para gerir a implementação de serviço web. Pode encontrar mais informações sobre o processo de operationalization [aqui](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

![kubenetes_dashboard](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)


## <a name="conclusion"></a>Conclusão

Os detalhes sobre como preparar um modelo ao incorporar o word utilizando os algoritmos Word2Vec e SSWE foram explicados e os embeddings extraídos foram utilizados como as funcionalidades para preparar vários modelos para prever pontuações de sentimento para dados de texto do Twitter. A funcionalidade de sentimento específicos a utilização de expressões Embeddings(SSWE) com o modelo de gradação árvore elevada deu o melhor desempenho. Este modelo, em seguida, foi implementado como um serviço web em tempo real nos serviços de contentor Azure utilizando o benchmark de trabalho do Azure Machine Learning.


## <a name="references"></a>Referências

* [Processo de ciência de dados de equipa](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Como utilizar o processo de ciência de dados de equipa (TDSP) no Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [Modelo de projeto TDSP do Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Bench de trabalho do Azure ML](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [E.u. a receitas de conjunto de dados de repositório UCI ML](https://archive.ics.uci.edu/ml/datasets/adult)
* [Biomedical reconhecimento de entidade com o modelo de TDSP](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov, Tomas, definir usados. Representações distribuídas palavras e expressões e as respetivas compositionality. Avanços nas informações neuronal sistemas de processamento de mensagens em fila. 2013.](https://arxiv.org/abs/1310.4546)
* [Tang, Duyu, definir usados. "Learning incorporar específicos de dados de sentimento do Word para classificação de dados de sentimento do Twitter." ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146)