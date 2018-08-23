---
title: Reconhecimento de entidades biomédicas - processo de ciência de dados de equipa - Azure Machine Learning | Documentos da Microsoft
description: Um processo de ciência de dados de equipa projeto início rápido que utiliza a aprendizagem profunda para reconhecimento de entidades biomédicas no Azure Machine Learning Workbench.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.openlocfilehash: f6ce43c2d290bacee10e102cc6c382981db9917f
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/20/2018
ms.locfileid: "42056222"
---
# <a name="biomedical-entity-recognition-using-team-data-science-process-tdsp-template"></a>Reconhecimento de entidades biomédicas com o modelo de processo de ciência de dados de equipa (TDSP)

Extração de entidades é uma subtarefa de extração de informações (também conhecido como [reconhecimento de entidades nomeado (NER)](https://en.wikipedia.org/wiki/Named-entity_recognition), entidade segmentação e identificação de entidade). O objetivo deste cenário do mundo real é destacar como utilizar o Azure Machine Learning Workbench para resolver uma tarefa de processamento de linguagem Natural (NLP) complicado, como a extração de entidades de texto não estruturado:

1. Como para preparar uma palavra neural embeddings modelações num corpo de texto de cerca de 18 milhões PubMed resumos usando [implementação do Spark Word2Vec](https://spark.apache.org/docs/latest/mllib-feature-extraction.html#word2vec).
2. Como criar um modelo de rede neural recorrente de memória de curto prazo longo (LSTM) profunda para extração de entidades num ativadas para GPU dados ciência de Máquina Virtual do Azure (VM DS de GPU) no Azure.
2. Demonstre que esse modelo de embeddings palavra específica do domínio pode superar o desempenho de modelos de embeddings word genérico na tarefa de reconhecimento de entidades. 
3. Demonstre como dar formação e operacionalizar modelos de aprendizagem profunda com o Azure Machine Learning Workbench.

4. Demonstre os seguintes recursos no Azure Machine Learning Workbench:

    * Instanciação de [estrutura do processo de ciência de dados de equipa (TDSP) e modelos](how-to-use-tdsp-in-azure-ml.md)
    * Gerenciamento automatizado de suas dependências de projeto, incluindo o download e a instalação
    * Execução de scripts do Python nos ambientes de computação diferentes
    * Histórico de controlo para scripts de Python de execução
    * Com clusters do HDInsight Spark 2.1 de contextos de computação de execução de tarefas no Spark remoto
    * Execução de tarefas na remoto de GPU de VMs no Azure
    * Operacionalização fácil de modelos de aprendizagem profunda como serviços da web nos serviços de contentor do Azure (ACS)

## <a name="use-case-overview"></a>Descrição geral de caso de utilização
Com o nome de entidades biomédicas é uma etapa crítica para tarefas NLP biomédicas complexas, tais como: 
* Extrair as menções de entidades nomeadas tais doenças, drogas, compostos químicos e os sintomas de registos Eletrónicos de médico ou estado de funcionamento.
* Deteção de medicamentos
* Compreender as interações entre entidades de diferentes tipos, como interação de medicamentos de medicamentos, relação cerebral drug e relação de gene proteínas.

Nosso cenário de caso se concentra em como uma grande quantidade de corpo de dados não estruturados, como Medline PubMed resumos pode ser analisada para preparar uma palavra incorporar o modelo. Em seguida, os embeddings de saída são considerados como recursos gerados automaticamente para preparar um extrator de entidades neural.

Nossos resultados mostram que a preparação de modelos de extração de entidades biomédicas na palavra específica do domínio incorporar recursos supera o modelo com base em com o tipo de recurso genérico. O modelo de domínio específico pode detetar 7012 entidades corretamente (fora 9475) com F1-pontuação de 0.73 em comparação comparada 5274 entidades com F1-pontuação de 0.61 para o modelo genérico.

A figura a seguir mostra a arquitetura que foi utilizada para processar dados e formar modelos.

![Arquitetura](./media/scenario-tdsp-biomedical-recognition/architecture.png)

## <a name="data-description"></a>Descrição de dados

### <a name="1-word2vec-model-training-data"></a>1. Dados de treinamento de modelo Word2Vec
Baixamos o MEDLINE abstratos dados brutos do primeiro [MEDLINE](https://www.nlm.nih.gov/pubs/factsheets/medline.html). Os dados estão publicamente disponíveis na forma de arquivos XML em seus [servidor de FTP](https://ftp.ncbi.nlm.nih.gov/pubmed/baseline). Existem 892 arquivos XML no servidor e cada um dos arquivos XML tem as informações de 30 000 artigos. Obter mais detalhes sobre o passo de recolha de dados são fornecidos na secção de estrutura do projeto. Os campos presentes em cada arquivo são 
        
        abstract
        affiliation
        authors
        country 
        delete: boolean if False means paper got updated so you might have two XMLs for the same paper.
        file_name   
        issn_linking    
        journal 
        keywords    
        medline_ta: this is abbreviation of the journal nam 
        mesh_terms: list of MeSH terms  
        nlm_unique_id   
        other_id: Other IDs 
        pmc: Pubmed Central ID  
        pmid: Pubmed ID
        pubdate: Publication date
        title

### <a name="2-lstm-model-training-data"></a>2. Dados de treinamento de modelo LSTM

O modelo de extração de entidades neural foi treinado e avaliado em conjuntos de dados publicamente disponíveis. Para obter uma descrição detalhada sobre estes conjuntos de dados, pode fazer referência às seguintes origens:
 * [Tarefa de reconhecimento de entidade de biografia na BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [Corpus de tarefa BioCreative V CDR](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - tarefa 9.1 (reconhecimento de medicamentos)](https://www.cs.york.ac.uk/semeval-2013/task9/)

## <a name="link-to-the-azure-gallery-github-repository"></a>Ligar para o repositório do GitHub de galeria do Azure
Segue-se a ligação para o repositório do GitHub público do cenário do mundo real que contém o código e uma descrição mais detalhada: 

[https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction)


## <a name="prerequisites"></a>Pré-requisitos 

* Azure [subscrição](https://azure.microsoft.com/free/)
* Azure Machine Learning Workbench. Ver [guia de instalação](../service/quickstart-installation.md). Atualmente, o Azure Machine Learning Workbench pode ser instalado nos seguintes sistemas operativos apenas: 
    * Windows 10 ou Windows Server 2016
    * macOS Sierra (ou mais recente)


### <a name="azure-services"></a>Serviços do Azure
* [Cluster do HDInsight Spark](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql) versão 2.1 do Spark no Linux (HDI 3.6) para o cálculo de escalamento horizontal. Para processar a quantia total de resumos MEDLINE abordadas a seguir, terá da configuração mínima de: 
    * Nó principal: [D13_V2](https://azure.microsoft.com/pricing/details/hdinsight/) tamanho
    * Nós de trabalho:, pelo menos, 4 de [D12_V2](https://azure.microsoft.com/pricing/details/hdinsight/). No nosso trabalho, nós usamos 11 nós de trabalho de tamanho de D12_V2.
* [Máquina Virtual de ciência de dados (DSVM) do NC6](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-linux-dsvm-intro) para computação de aumento vertical.

### <a name="python-packages"></a>Pacotes de Python

Todas as dependências necessárias são definidas no ficheiro aml_config/conda_dependencies.yml sob a pasta de projeto do cenário. As dependências definidas nesse arquivo são automaticamente aprovisionadas para execuções em relação a docker, a VM e HDI cluster destinos. Para obter detalhes sobre o formato de ficheiro de ambiente de Conda, consulte [aqui](https://conda.io/docs/using/envs.html#create-environment-file-by-hand).

* [TensorFlow](https://www.tensorflow.org/install/)
* [CNTK 2.0](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras)
* [Keras](https://keras.io/#installation)
* NLTK
* Fastparquet

### <a name="basic-instructions-for-azure-machine-learning-aml-workbench"></a>Instruções básicas para a Bancada de trabalho do Azure Machine Learning (AML)
* [Descrição geral](../service/overview-what-is-azure-ml.md)
* [Instalação](../service/quickstart-installation.md)
* [Utilizar o TDSP](how-to-use-tdsp-in-azure-ml.md)
* [Como ler e escrever ficheiros](how-to-read-write-files.md)
* [Como utilizar blocos de notas do Jupyter](how-to-use-jupyter-notebooks.md)
* [Como utilizar o GPU](how-to-use-gpu.md)

## <a name="scenario-structure"></a>Estrutura do cenário
O cenário, vamos utilizar os TDSP estrutura e a documentação de modelos de projeto (figura 1), que segue a [ciclo de vida do TDSP](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). Projeto é criado com base nas instruções fornecidas [aqui](./how-to-use-tdsp-in-azure-ml.md).


![Preencha as informações do projeto](./media/scenario-tdsp-biomedical-recognition/instantiation-3.png) 

O fluxo de trabalho de ciência de dados passo a passo é o seguinte:

### <a name="1-data-acquisition-and-understanding"></a>1. Aquisição e compreensão de dados

Ver [dados aquisição e compreensão](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md).

O corpus MEDLINE brutos tem um total de resumos de milhões de 27 em cerca de 10 milhões de artigos tem um campo de abstrato vazio. O Azure HDInsight Spark é usado para processar grandes volumes de dados que não podem ser carregadas na memória de uma única máquina como uma [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). Em primeiro lugar, os dados são transferidos para o cluster do Spark. Em seguida, os seguintes passos são executados no [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html): 
* analisar os ficheiros XML a utilizar o analisador de XML Medline
* pré-processar o texto abstrato, incluindo a divisão de sentença, atomização e normalização maiúsculas.
* excluir artigos em que o campo abstrato está vazio ou tem texto curto 
* criar o vocabulário do word a partir de resumos de treinamento
* Prepare a palavra incorporar modelo neural. Para obter mais informações, consulte [ligação de código do GitHub](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md) para começar a utilizar.


Depois de analisar os arquivos XML, dados tem o seguinte formato: 

![Amostra de dados](./media/scenario-tdsp-biomedical-recognition/datasample.png)

O modelo de extração de entidades neural foi treinado e avaliado em conjuntos de dados publicamente disponíveis. Para obter uma descrição detalhada sobre estes conjuntos de dados, pode fazer referência às seguintes origens:
 * [Tarefa de reconhecimento de entidade de biografia na BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [Corpus de tarefa BioCreative V CDR](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - tarefa 9.1 (reconhecimento de medicamentos)](https://www.cs.york.ac.uk/semeval-2013/task9/)

### <a name="2-modeling"></a>2. Modelação

Ver [modelagem](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling).

A Modelagem é a fase em que vamos mostrar como pode utilizar os dados transferidos na secção anterior para seu próprio word incorporar o modelo de formação e utilizá-lo para outras tarefas a jusante. Apesar de que estão a utilizar os dados de PubMed, o pipeline para gerar o embeddings é genérico e pode ser reutilizado para treinar incorporações para qualquer outro domínio. Para embeddings ser uma representação precisa dos dados, é essencial que o word2vec é preparado numa grande quantidade de dados.
Assim que tivermos as incorporações prontas, pode preparar um modelo de rede neural profunda que usa o embeddings adquirido para inicializar a camada de incorporar. Podemos marcar a camada de incorporação como não trainable mas que não é obrigatório. O treinamento da palavra incorporar o modelo for não supervisionado e, por conseguinte, conseguimos tirar partido de textos sem etiqueta. No entanto, o treinamento de modelo de reconhecimento de entidades é uma tarefa de aprendizagem supervisionada e sua precisão depende da quantidade e a qualidade de um de dados anotados manualmente. 


#### <a name="21-feature-generation"></a>2.1. Geração de funcionalidade

Ver [geração de recursos](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering).

Word2Vec é a palavra incorporar o algoritmo de aprendizado não supervisionado, que prepara um modelo de rede neural de um corpo de treinamento sem etiqueta. Produz um vetor contínuo de cada palavra no corpus que representa as informações de semânticas. Esses modelos são as redes neurais simples com uma única camada oculta. As vetores/incorporações adquiridas por backpropagation e stochastic gradient descendente gradação. Existem dois tipos de modelos de word2vec, ou seja, ignorar-grama e contínua-matriz de-de-palavras (verifique [aqui](https://arxiv.org/pdf/1301.3781.pdf) para obter mais detalhes). Uma vez que estamos a utilizar implementação o MLlib word2vec, que suporta o modelo de ignorar-gram, podemos resumidamente descrevê-lo aqui (imagem obtida a partir da [link](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/)): 

![Modelo de ignorar grama](./media/scenario-tdsp-biomedical-recognition/skip-gram.png)

O modelo utiliza Softmax hierárquica e amostragem negativa para otimizar o desempenho. SoftMax hierárquica (H-SoftMax) é uma aproximação inspirado pelo árvores binárias. H-SoftMax basicamente substitui a camada de SoftMax simples com uma camada hierárquica que tem as palavras, conforme sai. Isto permite-nos para decompor a calcular a probabilidade de uma palavra numa seqüência de cálculos de probabilidade, que nos poupa de ter que calcular a normalização cara ao longo de todas as palavras. Uma vez que uma árvore binária balanceada tem uma profundidade de log2 (| V |) (V é o vocabulário), precisamos apenas avaliar, no máximo, log2 (| V |) nós para obter a probabilidade de final de uma palavra. A probabilidade de um w word tendo em conta o c de contexto, em seguida, é simplesmente o produto das probabilidades de tirar direita e esquerda respectivamente transforma que levam para o nó de folha. Podemos criar uma árvore Huffman com base na frequência de palavras no conjunto de dados para garantir que palavras mais frequentes obtém representações menores. Para obter mais informações, consulte [esta ligação](http://sebastianruder.com/word-embeddings-softmax/).
Imagem obtida a partir da [aqui](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/).

##### <a name="visualization"></a>Visualização

Assim que tivermos as incorporações, gostaríamos de visualize-os e ver a relação entre as palavras semanticamente similares. 

![W2V semelhança](./media/scenario-tdsp-biomedical-recognition/w2v-sim.png)

Nós temos mostrado duas formas diferentes de visualizar o embeddings. Primeiro usa um PCA para projetar o vetor dimensional elevado para um espaço de vetor 2D. Isso nos leva a uma perda significativa de informações e a visualização não é tão precisa. O segundo é para utilizar com o PCA [t SNE](https://distill.pub/2016/misread-tsne/). t-SNE é uma técnica de redução de dimensionalidade não-linear que é bem adequada para incorporar dados altamente dimensionais num espaço de duas ou três dimensões, que podem ser visualizados num gráfico de dispersão.  Modela a cada objeto altamente dimensionais por um ponto duas ou três dimensões de forma que objetos semelhantes são modelados por pontos próximos e objetos diferentes são modelados por pontos distantes. Ele funciona em duas partes. Em primeiro lugar, ele cria uma distribuição de probabilidade sobre os pares de contratos no espaço de dimensional superior de uma forma que objetos semelhantes têm uma elevada probabilidade de a ser recolhidos e pontos diferentes têm baixa probabilidade de introdução escolhido. Em segundo lugar, ele define uma distribuição de probabilidade semelhante ao longo de pontos num mapa dimensional baixo e minimiza o divergência de KL entre as duas distribuições em relação à localização dos pontos no mapa. A localização dos pontos na dimensão baixa é obtida ao minimizarem o divergência de KL com gradiente descendente. Mas t SNE poderá não ser sempre fiável. Pode encontrar detalhes de implementação [aqui](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering). 


Conforme mostrado na figura a seguir, a visualização de t-SNE fornece mais separações de vetores de word e padrões de clusters potenciais. 


* Visualização com o PCA

![PCA](./media/scenario-tdsp-biomedical-recognition/pca.png)

* Visualização com o t-SNE

![t-SNE](./media/scenario-tdsp-biomedical-recognition/tsne.png)

* Pontos mais próximos "Cancro" (estão todos os subtipos dos cancro)

![Pontos mais próximos para o cancro](./media/scenario-tdsp-biomedical-recognition/nearesttocancer.png)

#### <a name="22-train-the-neural-entity-extractor"></a>2.2. Preparar o extrator de entidades neural

Ver [treinar o extrator de entidades neural](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation/ReadMe.md).

A arquitetura de rede neural de feed-forward sofre um problema que trate cada entrada e saída, independentemente das entradas e saídas. Esta arquitetura não é possível modelar a sequência de sequência de tarefas de etiquetas como tradução automática e extração de entidades. Modelos de rede neural recorrente superar esse problema, como eles podem transmitir informações computadas até agora para o próximo nó. Esta propriedade é chamada de ter a memória na rede, uma vez que é capaz de usar as informações anteriormente calculadas, conforme mostrado na figura a seguir:

![RNN](./media/scenario-tdsp-biomedical-recognition/rnn-expanded.png)

RNNs baunilha, na verdade, sofrem os [problema de gradação de fuga](https://en.wikipedia.org/wiki/Vanishing_gradient_problem) devido a que eles não são possível utilizar todas as informações de que eles viram antes. O problema torna-se evidente apenas quando uma grande quantidade de contexto é necessário para efetuar uma predição. Mas modelos como LSTM não sofrem de um problema desse tipo, na verdade eles foram criados para não se esqueça de dependências de longo prazo. Ao contrário de baunilha RNNs que tenham uma única rede neural, as LSTMs têm as interações entre quatro redes neurais para cada célula. Para obter uma explicação detalhada de como funcionam LSTM, consulte [esta publicação](http://colah.github.io/posts/2015-08-Understanding-LSTMs/).

![Célula LSTM](./media/scenario-tdsp-biomedical-recognition/lstm-cell.png)

Vamos tentar juntar-se de nossa própria rede de neural recorrente baseada em LSTM e tente extrair os tipos de entidade como menções de medicamentos, doenças e sintoma de dados de PubMed. A primeira etapa é obter uma grande quantidade de dados etiquetados e como deve ter adivinhado, que não é fácil! A maioria dos dados médicos contém muitas informações confidenciais sobre a pessoa e, por conseguinte, não estão disponível publicamente. Podemos contar com uma combinação de dois conjuntos de dados diferentes que estão publicamente disponíveis. O primeiro conjunto de dados é de Semeval 2013 - tarefa 9.1 (reconhecimento de medicamentos) e o outro é da tarefa de BioCreative V CDR. Podemos está a combinar e automático a etiquetagem esses dois conjuntos de dados, para que possamos detetar fármacos e doenças de textos médicos e avaliar nossa incorporações. Para obter detalhes de implementação, consulte [ligação de código do GitHub](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation).

A arquitetura de modelo que usamos em todos os códigos de e para a comparação é apresentada abaixo. O parâmetro que é alterado para diferentes conjuntos de dados é o comprimento máximo da sequência (613 aqui).

![Modelo LSTM](./media/scenario-tdsp-biomedical-recognition/d-a-d-model.png)

#### <a name="23-model-evaluation"></a>2.3. Avaliação do modelo
Ver [modelar avaliação](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/03_model_evaluation/ReadMe.md).

Utilizamos o script de avaliação da tarefa partilhada [tarefas de reconhecimento de entidades de biografia na biografia NLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html) para avaliar a precisão, lembre-se e pontuação de F1 do modelo. 

#### <a name="in-domain-versus-generic-word-embedding-models"></a>No domínio em comparação com o word genérico incorporar modelos

Segue-se uma comparação entre a precisão dos dois tipos de recurso: (1) incorporações com base em com resumos de PubMed e (2) incorporações com base em com notícias do Google. Podemos ver claramente que o modelo no domínio supera o modelo genérico. Ter, pelo que uma palavra específica incorporar o modelo em vez de utilizar um genérica é muito mais útil. 

* Tarefa #1: Fármacos e de deteção de doenças

![Comparação de modelo 1](./media/scenario-tdsp-biomedical-recognition/mc1.png)

Vamos executar a avaliação das incorporações em outros conjuntos de dados do modo semelhante e ver que esse modelo no domínio é sempre melhor.

* Tarefa #2: Proteínas, célula linha, tipo de célula, DNA e deteção de RNA

![Comparação do modelo de 2](./media/scenario-tdsp-biomedical-recognition/mc2.png)

* Tarefa #3: Produtos químicos e de deteção de doenças

![Comparação do modelo de 3](./media/scenario-tdsp-biomedical-recognition/mc3.png)

* Tarefa #4: Deteção de drogas

![Comparação do modelo de 4](./media/scenario-tdsp-biomedical-recognition/mc4.png)

* Tarefa #5: Deteção de Genes

![Comparação do modelo de 5](./media/scenario-tdsp-biomedical-recognition/mc5.png)

#### <a name="tensorflow-versus-cntk"></a>TensorFlow em comparação com o CNTK
Todos os modelos que comunicados são treinados com Keras TensorFlow como back-end. Keras com back-end do CNTK não suporta o "reverter" no momento que este trabalho foi concluído. Por conseguinte, para fins de comparação, temos preparado um modelo LSTM unidirecional com o back-end do CNTK e comparado com um modelo LSTM unidirecional com o back-end do TensorFlow. Instalar CNTK 2.0 para Keras partir [aqui](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras). 

![Comparação de modelo 6](./media/scenario-tdsp-biomedical-recognition/mc6.png)

Podemos concluíram que o CNTK executa como boa como o Tensorflow, tanto em termos do tempo de treinamento direcionado por "Epoch" (60 segundos para CNTK e 75 segundos para Tensorflow) e o número de entidades de teste detetados. Estamos a utilizar as camadas Unidirecionais para avaliação.


### <a name="3-deployment"></a>3. Implementação

Ver [implementação](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/03_deployment).

Implementar um serviço da web num cluster no [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/). O ambiente de operacionalização Aprovisiona o Docker e Kubernetes no cluster para gerenciar a implantação de serviço web. Pode encontrar mais informações sobre o processo de operacionalização [aqui](model-management-service-deploy.md ).


## <a name="conclusion"></a>Conclusão

Fomos sobre os detalhes de como pode preparar um modelo de incorporação do word usando o algoritmo de Word2Vec no Spark e, em seguida, utilize o embeddings extraídos como recursos para preparar uma rede neural profunda para extração de entidades. Vamos tiver aplicado o pipeline de treinamento no domínio biomédicas. No entanto, o pipeline é suficientemente genérico a ser aplicado para detetar tipos de entidade personalizada de qualquer outro domínio. Precisa apenas dados suficientes e pode adaptar facilmente o fluxo de trabalho apresentado aqui para um domínio diferente.

## <a name="references"></a>Referências

* Tomas Mikolov, Kai Chen, Greg Corrado e Jeffrey Dean. 2013a. Estimativa eficiente de representações de word no espaço de vetor. No judiciais de ICLR.
* Tomas Mikolov, Ilya Sutskever, Kai Chen, Greg S Corrado e Jeff Dean. 2013b. Distribuída representações de palavras e frases e seus compositividade. No judiciais de NIPS, páginas 3111 – 3119.
* Billy Chiu, Gamal Crichton, Anna Korhonen e Sampo Pyysalo. 2016. [Como boa incorporações Train para NLP biomédicas](http://aclweb.org/anthology/W/W16/W16-2922.pdf), nos processos do décimo quinto Workshop sobre o processamento de linguagem Natural biomédicas, páginas 166 – 174.
* [Representações de vetor de palavras](https://www.tensorflow.org/tutorials/word2vec)
* [Redes Neurais recorrentes](https://www.tensorflow.org/tutorials/recurrent)
* [Problemas encontrados com o Spark ml Word2Vec](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/problems-encountered-with-spark-ml-word2vec/)
* [Spark Word2Vec: as lições aprendidas](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/spark-word2vec-lessons-learned/)

