---
title: "Prever sentimento do Twitter com o word embeddings utilizando o processo de ciência de dados de equipa no Azure | Microsoft Docs"
description: "Os passos necessários para executar os seus projetos de ciência de dados."
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
ms.openlocfilehash: df1124ddb436f0cfeec8f4ed9728fa15278b9325
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="predict-twitter-sentiment-with-word-embeddings-by-using-the-team-data-science-process"></a>Prever sentimento do Twitter com o word embeddings utilizando o processo de ciência de dados de equipa

Este artigo mostra-lhe como colaborar de forma eficaz, utilizando o _Word2Vec_ word incorporar algoritmo e o _específicos de dados de sentimento do Word incorporar (SSWE)_ algoritmo para prever os dados de sentimento do Twitter com o [Do azure Machine Learning](../preview/index.yml). Para obter mais informações sobre prever polarity de sentimento do Twitter, consulte o [MachineLearningSamples TwitterSentimentPrediction repositório](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction) no GitHub. A chave para o facilitar o início colaboração em equipa Efetivo em projetos de ciência de dados é para uniformizar a estrutura e a documentação de projetos com um ciclo de vida de ciência de dados estabelecida. O [processo de ciência de dados de equipa (TDSP)](overview.md) fornece este tipo de estruturados [ciclo de vida](lifecycle.md). 

Criação de projetos de ciência de dados com o _modelo TDSP_ fornece o framework padronizado para projetos do Azure Machine Learning. Anteriormente, a equipa TDSP lançou um [repositório do GitHub para a estrutura de projeto TDSP e modelos](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Agora os projetos de Machine Learning que são instanciados com [modelos TDSP do Azure Machine Learning](https://github.com/amlsamples/tdsp) estão ativadas. Para obter instruções, consulte como utilizar [projetos de estrutura TDSP com o modelo TDSP](../preview/how-to-use-tdsp-in-azure-ml.md) no Azure Machine Learning. 


## <a name="twitter-sentiment-polarity-sample"></a>Exemplo de polarity de sentimento do twitter

Este artigo utiliza uma amostra para lhe mostrar como instanciar e executar um projeto de Machine Learning. Este exemplo utiliza a estrutura TDSP e modelos no Azure Machine Learning Workbench. O exemplo completo é fornecido na [estas instruções](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md). A tarefa de modelação prevê polarity sentimento (positivo ou negativo) utilizando o texto de tweets. Este artigo descreve as tarefas de modelação de dados que são descritas as instruções. As instruções abrangem as seguintes tarefas:

- Exploração de dados, formação e implementação de um modelo de machine learning que resolva o problema de predição descrita na descrição geral caso utilize. [Dados de sentimento do twitter](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) é utilizado para estas tarefas.
- Execução do projeto, utilizando o modelo TDSP do Azure Machine Learning para este projeto. Para execução do projeto e relatórios, o ciclo de vida TDSP é utilizado.
- Operationalization da solução diretamente a partir do Azure Machine Learning no serviço de contentor do Azure.

O projeto realça as seguintes funcionalidades do Azure Machine Learning:

- Instanciação e a utilização da estrutura de TDSP.
- Execução de código no Azure Machine Learning Workbench.
- Fácil operationalization no serviço de contentor utilizando o Docker e Kubernetes.

## <a name="team-data-science-process"></a>Processo de Ciência de Dados de Equipa
Para executar este exemplo, pode utilizar os modelos de estrutura e a documentação de projeto do TDSP no Azure Machine Learning Workbench. O exemplo implementa o [TDSP ciclo de vida](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), conforme mostrado na figura seguinte:

![Ciclo de vida TDSP](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

O projeto TDSP é criado no Azure Machine Learning Workbench com base no [estas instruções](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md), conforme mostrado na figura seguinte:

![Criar TDSP no Azure Machine Learning Workbench](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-preparationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Aquisição de dados e preparação](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
O primeiro passo neste exemplo é para transferir o conjunto de dados sentiment140 e dividir os dados em formação e testar conjuntos de dados. O conjunto de dados sentiment140 contém o conteúdo real do tweet (com emoticons removidas). O conjunto de dados também contém polarity de cada tweet (negativo = 0, positivo = 4) com os tweets independentes removido. Depois dos dados são divididos, os dados de preparação tem 1.3 milhões de linhas e dados de teste tem 320,000 linhas.


## <a name="model-developmenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Desenvolvimento de modelo](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

O passo seguinte na amostra é para desenvolver um modelo de dados. A tarefa de modelação está dividida em três partes:

- Engenharia da funcionalidade: gerar funcionalidades para o modelo através do word diferentes algoritmos de incorporar. 
- Criação de modelo: Preparar modelos diferentes para prever o sentimento texto de entrada. Exemplos destes modelos incluem _regressão logística da_ e _os aumentos gradação_.
- Modelo de avaliação: avaliar os modelos de formação sobre os dados de teste.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Engenharia da funcionalidade](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

Os algoritmos Word2Vec e SSWE são utilizados para gerar word embeddings. 


#### <a name="word2vec-algorithm"></a>Algoritmo de Word2Vec

O algoritmo de Word2Vec é utilizado no modelo de grama ignorar. Este modelo é explicado no documento por Tomas Mikolov, definir usados. "[Distribuídas representações palavras e expressões e as respetivas Compositionality. Avanços nas informações neuronal sistemas de processamento de mensagens em fila. ](https://arxiv.org/abs/1310.4546)" 2013.

O modelo de ignorar grama é uma rede neuronal shallow. A entrada é a palavra de destino que está codificada como um vetor de acesso frequente um, que é utilizado para prever próximas palavras. Se **V** é o tamanho de vocabulário, em seguida, o tamanho da camada de saída é __C * V__ onde C é o tamanho da janela de contexto. A figura seguinte mostra uma arquitetura que é baseada no modelo de ignorar grama:

![Ignorar grama modelo](./media/predict-twitter-sentiment/skip-gram-model.PNG)

Os mechanics detalhadas do algoritmo Word2Vec e ignorar grama modelo estão fora do âmbito deste exemplo. Para obter mais informações, consulte as seguintes referências:

- [Código de 02_A_Word2Vec.PY com exemplos de TensorFlow referenciados](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py) 
- [Representações vetor palavras](https://www.tensorflow.org/tutorials/word2vec)
- [Exatamente como funciona o word2vec?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
- [Notas sobre o ruído de estimativa Contrastive e amostragem negativa](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)




#### <a name="sentiment-specific-word-embedding-algorithm"></a>Algoritmo específicos de dados de sentimento do Word incorporar
O algoritmo SSWE tenta ultrapassar uma vulnerabilidade do algoritmo Word2Vec onde palavras com contextos semelhantes e opposite polarity podem ter vetores de word semelhantes. As semelhanças podem fazer com que o algoritmo de Word2Vec para não efetuar com precisão para tarefas como a análise de dados de sentimento. O algoritmo SSWE tenta processar de acordo com esta vulnerabilidade incorporando polarity o frase e contexto da palavra para a respetiva função de perda.

Este exemplo utiliza uma variante do algoritmo SSWE da seguinte forma:

- Ambos os original _ngram_ e o danificada _ngram_ são utilizados como entradas.
- A função de perda de hinge de estilo de classificação é utilizada para a perda de diferenças sintáticas e a perda de semântica.
- A função de perda ultimate é a combinação ponderada de diferenças sintáticas perda e a perda de semântica.
- Para uma simplicidade apenas a semântica cruzada entropia é utilizada como a função de perda.

O exemplo mostra que, mesmo com a função de perda mais simples, é melhor do que o Word2Vec ao incorporar o desempenho de incorporar o SSWE. A figura seguinte mostra o modelo convolutional que é utilizado para gerar incorporar específicos de dados de sentimento do word:

![Modelo de rede neuronal inspirado pelo SSWE](./media/predict-twitter-sentiment/embedding-model-2.PNG)

Depois de terminar o processo de preparação, dois ficheiros incorporar no formato separador com valores separados por (TSV) são gerados para a fase de modelação.

Para mais informações sobre os algoritmos SSWE, consulte o documento por Duyu Tang, definir usados. "[Sentimento específicos de aprendizagem do Word incorporar para classificação de dados de sentimento do Twitter](http://www.aclweb.org/anthology/P14-1146)." Associação para Linguistics computacional (1). 2014.


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Criação de modelo](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Depois de vetores de word são gerados ao utilizar o algoritmo SSWE ou Word2Vec, os modelos de classificação são preparados para prever a polarity sentimento real. Dois tipos de funcionalidades: Word2Vec e SSWE, são aplicados aos dois modelos: modelo os aumentos gradação e o modelo de regressão logística da. Como resultado, são preparados quatro modelos diferentes.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Modelo de avaliação](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Depois dos modelos são preparados, os modelos são utilizados para testar os dados de texto do Twitter e avaliar o desempenho de cada modelo. O exemplo avalia os modelos de quatro seguintes:

- Gradação Boosting através de SSWE incorporar.
- Regressão logística da ativação pós-falha SSWE incorporar.
- Gradação Boosting através de Word2Vec incorporar.
- Regressão logística da ativação pós-falha Word2Vec incorporar.

Uma comparação do desempenho dos modelos de quatro é apresentada na figura seguinte:

![Recetor operativo características comparação do modelo (ROC)](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

O modelo os aumentos gradação com a funcionalidade SSWE fornece o melhor desempenho quando a comparação com os modelos utilizando a área de métrica de curva (AUC).


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Implementação](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

O passo final é a implementação do modelo de previsão sentimento preparado para um serviço web num cluster no serviço de contentor do Azure. Este exemplo utiliza o modelo os aumentos gradação com o algoritmo embedding SSWE como o modelo treinado. O ambiente de operationalization Aprovisiona Docker e Kubernetes do cluster para gerir a implementação de serviço web, conforme mostrado na figura seguinte: 

![Dashboard de Kubernetes](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)

Para obter mais informações sobre o processo de operationalization, consulte [implementar um modelo do Azure Machine Learning como um serviço web](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Conclusão

Neste artigo, aprendeu a formação de um modelo ao incorporar o word, utilizando os algoritmos Word2Vec e específicos de dados de sentimento do Word incorporar. Os embeddings extraídos foram utilizados como as funcionalidades para preparar vários modelos para prever pontuações de sentimento para dados de texto do Twitter. A funcionalidade SSWE utilizada com o modelo os aumentos gradação deu o melhor desempenho. O modelo, em seguida, foi implementado como um serviço web em tempo real no serviço de contentor utilizando o Azure Machine Learning Workbench.


## <a name="references"></a>Referências

* [Processo de ciência de dados de equipa](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) 
* [Como utilizar o processo de ciência de dados de equipa (TDSP) no Azure Machine Learning](https://aka.ms/how-to-use-tdsp-in-aml)
* [Modelos de projeto TDSP do Azure Machine Learning](https://aka.ms/tdspamlgithubrepo)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/preview/)
* [Receitas de E.U.A. conjunto de dados do repositório de UCI ML](https://archive.ics.uci.edu/ml/datasets/adult)
* [Reconhecimento de entidade biomedical utilizando os modelos TDSP](https://docs.microsoft.com/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov, Tomas, definir usados. "Representações palavras e expressões e as respetivas Compositionality distribuído. Avança neuronal informações de sistemas de processamento." 2013.](https://arxiv.org/abs/1310.4546)
* [Tang, Duyu, definir usados. "Learning incorporar específicos de dados de sentimento do Word para classificação de dados de sentimento do Twitter." ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146)
