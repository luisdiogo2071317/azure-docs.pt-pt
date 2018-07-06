---
title: Documente a análise de coleção – Azure | Documentos da Microsoft
description: Como resumir e analisar uma grande coleção de documentos, incluindo técnicas, como a aprendizagem de frase, tópico de modelagem e análise de modelo de tópico com o Azure ML Workbench.
services: machine-learning
author: kehuan
ms.author: kehuan
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, MicrosoftDocs/mlreview, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 29f493449d48df26919a98452fa7f832d653d45e
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861925"
---
# <a name="document-collection-analysis"></a>Análise de coleção de documentos

Este cenário demonstra como resumir e analisar uma grande coleção de documentos, incluindo técnicas, como a aprendizagem de frase, tópico de modelagem e análise de modelo de tópico com o Azure ML Workbench. O Azure Machine Learning Workbench fornece para fácil Dimensionar para a coleção de documentos muito grandes e fornece mecanismos para treinar e otimizar modelos de dentro de uma variedade de contextos de cálculo, abrangendo desde cálculo local para dados de máquinas virtuais de ciência para Cluster do Spark. Fácil desenvolvimento é fornecido por meio de blocos de notas do Jupyter no Azure Machine Learning Workbench.

## <a name="link-to-the-gallery-github-repository"></a>Ligação para o repositório do GitHub de galeria

O repositório do GitHub público para este cenário do mundo real contém todos os materiais, incluindo exemplos de código, necessários para este exemplo:

[https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis](https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis)

## <a name="overview"></a>Descrição geral

Com uma grande quantidade de dados (dados de texto especialmente não estruturado) recolhida todos os dias, um desafio significativo é a organizar, pesquisa e compreender as vastas quantidades dessas publicações. Neste cenário de análise de coleção de documentos demonstra um automatizada e eficiente ponto-a-ponto fluxo de trabalho para a análise de coleção de documentos grandes e ativar tarefas NLP downstream.

Os elementos principais fornecidos por este cenário são:

1. Frase de várias palavras destaque de documentos de aprendizado.

1. Detetar subjacentes entre os tópicos apresentados na coleção de documentos.

1. Que representa documentos, a distribuição por tópicos.

1. Apresentação de métodos de organizar, pesquisa e que resume os documentos com base no conteúdo por tópicos.

Os métodos apresentados neste cenário foi possível ativar uma variedade do cargas de trabalho de industriais críticas, como deteção de anomalias de tendências de tópico, resumo de coleção de documentos e pesquisa de documento semelhante. Ele pode ser aplicado a muitos tipos diferentes de análise de documento, como a legislação de governo, histórias de notícias, análises de produtos, comentários de clientes e artigos de pesquisa científica.

Os técnicas/algoritmos de machine learning utilizados neste cenário incluem:

1. Processamento de texto e a limpeza

1. Aprendizagem de frase

1. Modelagem de tópico

1. Resumo de corpo

1. Tendências por tópicos e deteção de anomalias

## <a name="prerequisites"></a>Pré-requisitos

As pré-requisitos para executar este exemplo são os seguintes:

* Certifique-se de que instalou corretamente [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) ao seguir os [instalar e criar o guia de introdução](../service/quickstart-installation.md).

* Neste exemplo pode ser executado em qualquer contexto de cálculo. No entanto, recomenda-se para executá-lo numa máquina com vários núcleo com, pelo menos, de 16GB de memória e espaço em disco de 5GB.

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do Workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abrir o Azure Machine Learning Workbench
2.  Sobre o **projetos** página, clique no **+** iniciar sessão e selecionar **novo projeto**
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Análise de coleção de documentos" e selecione o modelo
5.  Clique em **Criar**.

## <a name="data-description"></a>Descrição de dados

O conjunto de dados utilizado neste cenário contém resumos de texto e os dados de metadados associados para cada ação de legislativas executada pelo Congresso-nos. Os dados são recolhidos a partir [GovTrack.us](https://www.govtrack.us/), que controla as atividades de Congresso dos Estados Unidos e ajuda a norte-americanos participar em seu processo de legislativas nacional. Os dados em massa podem ser baixados via [esta ligação](https://www.govtrack.us/data/congress/) usando um script manual, que não está incluído neste cenário. Os detalhes de como transferir os dados, foi possível encontrar na [documentação da API de GovTrack](https://www.govtrack.us/developers).

### <a name="data-source"></a>Origem de dados

Neste cenário, os dados brutos coletados são uma série de ações legislativas introduzida pela nos Congresso (listas de propostas e as resoluções) de 1973 para Junho de 2017 (93rd para os Congresses 115th). Os dados recolhidos estão no formato JSON e contém um conjunto avançado de informações sobre as ações legislativas. Consulte a [esta ligação do GitHub](https://github.com/unitedstates/congress/wiki/bills) para uma descrição detalhada dos campos de dados. Para o efeito de demonstração neste cenário, apenas um subconjunto dos campos de dados foram extraídos dos ficheiros JSON. Um ficheiro TSV compilado previamente `CongressionalDataAll_Jun_2017.tsv` com registos de uma dessas ações legislativas é fornecido neste cenário. O ficheiro TSV pode ser transferido automaticamente com os blocos de notas `1_Preprocess_Text.ipynb` sob a pasta do bloco de notas ou `preprocessText.py` no pacote do Python.

### <a name="data-structure"></a>Estrutura dos dados

Há nove campos de dados no ficheiro de dados. Os nomes de campo de dados e as descrições são apresentadas da seguinte forma.

| Nome do Campo | Tipo | Descrição | Contém o valor em falta |
|------------|------|-------------|---------------|
| `ID` | Cadeia | O ID da fatura/resolução. O formato deste campo é [bill_type] [número]-[Congresso]. Por exemplo, "hconres1 93" significa o tipo de fatura é "hconres" (significa para a resolução do simultâneas de casa, consulte [este documento](https://github.com/unitedstates/congress/wiki/bills#basic-information)), o número de fatura for ' 1 'e o número de Congresso é ' 93'. | Não |
| `Text` | Cadeia | O conteúdo da fatura/resolução. | Não |
| `Date` | Cadeia | A data em que a fatura/resolução proposta inicialmente. Num formato de "aaaa-mm-dd". | Não |
| `SponsorName` | Cadeia | O nome do patrocinador de principal proposto a fatura/resolução. | Sim |
| `Type` | Cadeia | O tipo de título dos principais patrocinador, "representante" (representante) ou "Enviar" (senador). | Sim |
| `State` | Cadeia | O estado do patrocinador primário. | Sim |
| `District` | Número inteiro | O número de distrito do patrocinador primário se o título do patrocinador for um representante. | Sim |
| `Party` | Cadeia | A parte do patrocinador primário. | Sim |
| `Subjects` | Cadeia | Os termos de assunto adicionados cumulativamente e pela biblioteca do Congresso à fatura. Os termos são concatenados por vírgulas. Esses termos são escritos por uma pessoa na biblioteca do Congresso e não estão normalmente presentes quando informações sobre a fatura são publicadas pela primeira vez. Eles podem ser adicionados a qualquer momento. Portanto, no final de vida de uma fatura, algum assunto pode não ser relevante mais. | Sim |

## <a name="scenario-structure"></a>Estrutura do cenário

O exemplo de análise de coleção de documento está organizado em dois tipos de resultados finais. O primeiro tipo é uma série de iPython blocos de notas que mostram as descrições passo a passo do fluxo de trabalho inteiro. O segundo tipo é um pacote de Python, bem como alguns exemplos de código de como utilizar esse pacote. O pacote do Python é suficientemente genérico para servir vários casos de utilização.

Os ficheiros neste exemplo são organizados da seguinte forma.

| Nome de Ficheiro | Tipo | Descrição |
|-----------|------|-------------|
| `aml_config` | Pasta | Pasta de configuração do Azure Machine Learning Workbench, consulte [esta documentação](./experimentation-service-configuration-reference.md) para configuração de execução da experimentação detalhadas |
| `Code` | Pasta | A pasta de código usada para salvar os scripts de Python e o pacote do Python |
| `Data` | Pasta | A pasta de dados utilizada para guardar ficheiros intermédios |
| `notebooks` | Pasta | A pasta de blocos de notas do Jupyter |
| `Code/documentAnalysis/__init__.py` | Ficheiro de Python | O ficheiro de inicialização de pacote do Python |
| `Code/documentAnalysis/configs.py` | Ficheiro de Python | O ficheiro de configuração utilizado pela análise de documento pacote do Python, incluindo constantes predefinidas |
| `Code/documentAnalysis/preprocessText.py` | Ficheiro de Python | O ficheiro de Python utilizado para processar previamente os dados para tarefas a jusante |
| `Code/documentAnalysis/phraseLearning.py` | Ficheiro de Python | O ficheiro de Python utilizado para saber mais expressões a partir dos dados e transformar os dados não processados |
| `Code/documentAnalysis/topicModeling.py` | Ficheiro de Python | O ficheiro de Python utilizado para formar um modelo de tópico de alocação de Dirichlet latente (LDA) |
| `Code/step1.py` | Ficheiro de Python | Passo 1 de análise de coleção de documentos: processar previamente os texto |
| `Code/step2.py` | Ficheiro de Python | Passo 2 de análise de coleção de documentos: frase learning |
| `Code/step3.py` | Ficheiro de Python | Passo 3 de análise de coleção de documentos: dar formação e avaliar o modelo de tópico LDA |
| `Code/runme.py` | Ficheiro de Python | Exemplo de executar todos os passos num arquivo |
| `notebooks/1_Preprocess_Text.ipynb` | iPython Notebook | Pré-processar texto e transformar os dados não processados |
| `notebooks/2_Phrase_Learning.ipynb` | iPython Notebook | Saiba as frases de dados de texto (após a transformação de dados) |
| `notebooks/3_Topic_Model_Training.ipynb` | iPython Notebook | Preparar modelo de tópico LDA |
| `notebooks/4_Topic_Model_Summarization.ipynb` | iPython Notebook | Resumir os conteúdos da coleção de documentos com base num modelo de tópico LDA preparado |
| `notebooks/5_Topic_Model_Analysis.ipynb` | iPython Notebook | Analisar o conteúdo por tópicos de uma coleção de documentos de texto e correlacionar as informações por tópicos em relação a outros metadados associados à coleção de documentos |
| `notebooks/6_Interactive_Visualization.ipynb` | iPython Notebook | Visualização interativa do modelo de tópico adquirido |
| `notebooks/winprocess.py` | Ficheiro de Python | O script de python para multiprocessamento utilizado por blocos de notas |
| `README.md` | Ficheiro de markdown | O ficheiro de markdown Leia-me |

### <a name="data-ingestion-and-transformation"></a>Ingestão de dados e transformação

O conjunto de dados compilado `CongressionalDataAll_Jun_2017.tsv` é guardado no armazenamento de BLOBs e está acessível a ambos de dentro de blocos de notas e os scripts de Python. Há duas etapas para ingestão de dados e transformação: os dados de texto o pré-processamento e frase learning.

#### <a name="preprocess-text-data"></a>Processar previamente os dados de texto

O passo de pré-processamento aplica-se técnicas de processamento de linguagem natural para limpar e preparar os dados de texto não processado. Funciona como uma precursora para o aprendizado não supervisionado frase e modelação de tópico latente. A descrição passo a passo detalhada pode ser encontrada no bloco de notas `1_Preprocess_Text.ipynb`. Há também um script de Python `step1.py` corresponde a este bloco de notas.

Neste passo, o ficheiro de dados TSV é transferido a partir do armazenamento de Blobs do Azure e importado como um Pandas DataFrame. Cada elemento de linha do pacote de dados é uma única coesa longa cadeia de caracteres de texto ou um documento. Cada documento, em seguida, é dividido em segmentos de texto que são provável que ser frases, expressões ou subphrases. Divisão de foi concebido para proibir a frase de processo do word entre frase ou frase em várias cadeias de caracteres a utilizar durante o aprendizado frases de aprendizado.

Existem várias funções definidas para o passo de pré-processamento no bloco de notas e o pacote do Python. A maioria do trabalho pode ser obtida ao chamar essas duas linhas de códigos.

```python
# Read raw data into a Pandas DataFrame
textDF = getData()

# Write data frame with preprocessed text out to TSV file
cleanedDataFrame = CleanAndSplitText(textDF, saveDF=True)
```

#### <a name="phrase-learning"></a>Aprendizagem de frase

A etapa de aprendizagem frase implementa uma estrutura básica para saber de expressões-chave entre uma grande coleção de documentos. Ele é descrito no artigo intitulado "[modelagem frases de Multiword com restrita a árvore de expressões para melhorada tópico de modelagem de conversação voz](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf)", que foi originalmente apresentado no Workshop de IEEE 2012 na linguagem falada Tecnologia. A implementação detalhada da etapa de aprendizagem da expressão é mostrada no iPython Notebook `2_Phrase_Learning.ipynb` e o script de Python `step2.py`.

Este passo demora o texto limpo como entrada e aprende as frases mais destaque presentes numa grande coleção de documentos. O aprendizado de frase é um processo iterativo que pode ser dividido em três tarefas: contagem de n-gramas, classificar as ponderada Pointwise mútua informações das suas palavras que constituem as sentenças potenciais e reescrever frase em texto. Essas três tarefas são executadas seqüencialmente em várias iterações até que aprendeu as frases especificadas.

Do pacote de Python de análise de documento, uma classe de Python `PhraseLearner` está definido no `phraseLearning.py` ficheiro. Segue-se o fragmento de código usado para se obter frases.

```python
# Instantiate a PhraseLearner and run a configuration
phraseLearner = PhraseLearner(cleanedDataFrame, "CleanedText", numPhrase,
                        maxPhrasePerIter, maxPhraseLength, minInstanceCount)

# The chunks of text in a list
textData = list(phraseLearner.textFrame['LowercaseText'])

# Learn most salient phrases present in a large collection of documents
phraseLearner.RunConfiguration(textData,
            phraseLearner.learnedPhrases,
            addSpace=True,
            writeFile=True,
            num_workers=cpu_count()-1)
```

> [!NOTE]
> A etapa de aprendizagem frase é implementada com multiprocessamento. No entanto, mais núcleos de CPU fazer **não** significa que um tempo de execução mais rápido. Em nossos testes, o desempenho não é melhorado com mais de oito núcleos devido à sobrecarga no multiprocessamento. Demorou cerca de duas e um meio horas para saber mais de 25.000 frases numa máquina com oito núcleos (3,6 GHz).
>

### <a name="topic-modeling"></a>Modelagem de tópico

Uma utilização do modelo de latente tópico de aprendizagem LDA é o terceiro passo neste cenário. O [gensim](https://radimrehurek.com/gensim/) pacote do Python é necessária neste passo para saber uma [modelo de tópico LDA](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation). O bloco de notas correspondente para este passo é `3_Topic_Model_Training.ipynb`. Também pode consultar `step3.py` para saber como utilizar o pacote de análise do documento.

Neste passo, a principal tarefa é aprender um modelo de tópico LDA e otimizar os parâmetros hyper. Existem vários parâmetros tem de ser ajustado quando preparar um modelo LDA, mas o parâmetro mais importante é o número de tópicos. Tópicos muito poucos podem não ter uma visão para a coleção de documentos; ao escolher demasiado muitos resultará no "excessivamente clustering" de um corpo para vários tópicos de pequenas, muito semelhantes. O objetivo deste cenário é mostrar como ajustar o número de tópicos. O Azure Machine Learning Workbench oferece a liberdade para executar experimentações com a configuração de parâmetro diferentes em contextos de computação diferentes.

No pacote de Python de análise do documento, algumas funções foram definidas para ajudar os utilizadores descobrir o melhor número de tópicos. A primeira abordagem é ao avaliar a coerência do modelo de tópico. Existem quatro matrizes de avaliação suportadas: `u_mass`, `c_v`, `c_uci`, e `c_npmi`. Os detalhes dessas quatro métricas são discutidos [este documento](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). A segunda abordagem é avaliar perplexity num corpo de mantidos-out.

Para a avaliação de perplexity, espera-se uma curva de forma 'U' para obter o melhor número de tópicos. E a posição de elbow é a melhor opção. Recomenda-se para avaliar várias vezes com diferente seed aleatório e obter a média. Avaliar a coerência é esperado que seja um "n" Formatar, que significa que os aumentos de coerência com o aumento do número de tópicos e, em seguida, diminuir. Um gráfico de exemplo de perplexity e `c_v` coerência está a aparecer da seguinte forma.

![Perplexity](./media/scenario-document-collection-analysis/Perplexity_Value.png)

![c_v coerência](./media/scenario-document-collection-analysis/c_v_Coherence.png)

Neste cenário, o perplexity aumenta significativamente após 200 tópicos, o valor de coerência diminui significativamente após 200 tópicos também. Base esses gráficos e o desejo de assuntos mais gerais em relação ao longo de tópicos em cluster, escolha a 200 tópicos devem ser uma boa opção.

Pode preparar um modelo de tópico LDA na experimentação de uma execução, ou dar formação e avaliar vários modelos de LDA com configurações de número de tópico diferente numa experimentação individual ser executado. Recomenda-se para ser executado várias vezes para uma configuração e, em seguida, obter das avaliações de coerência e/ou perplexity médias. Os detalhes de como utilizar o pacote de análise do documento podem ser encontrados no `step3.py` ficheiro. Segue-se um fragmento de código de exemplo.

```python
topicmodeler = TopicModeler(docs,
        stopWordFile=FUNCTION_WORDS_FILE,
        minWordCount=MIN_WORD_COUNT,
        minDocCount=MIN_DOC_COUNT,
        maxDocFreq=MAX_DOC_FREQ,
        workers=cpu_count()-1,
        numTopics=NUM_TOPICS,
        numIterations=NUM_ITERATIONS,
        passes=NUM_PASSES,
        chunksize=CHUNK_SIZE,
        random_state=RANDOM_STATE,
        test_ratio=test_ratio)

# Train an LDA topic model
lda = topicmodeler.TrainLDA(saveModel=saveModel)

# Evaluate coherence metrics
coherence = topicmodeler.EvaluateCoherence(lda, coherence_types)

# Evaluate perplexity on a held-out corpus
perplex = topicmodeler.EvaluatePerplexity(lda)
```

> [!NOTE]
> O tempo de execução para preparar um modelo de tópico LDA depende de vários fatores, como o tamanho do corpo, configuração de parâmetro hyper, bem como o número de núcleos na máquina. Usar vários núcleos de CPU prepara um modelo mais rápido. No entanto, com o mesmo parâmetro hyper definição mais núcleos significa menos atualizações durante o treinamento. É recomendado ter **, pelo menos, 100 atualizações para preparar um modelo LDA convergido**. É discutida com a relação entre o número de atualizações e os parâmetros hyper [esta publicação](https://groups.google.com/forum/#!topic/gensim/ojySenxQHi4) e [nesta mensagem](http://miningthedetails.com/blog/python/lda/GensimLDA/). Em nossos testes, demorou cerca de 3 horas para preparar um modelo LDA com 200 tópicos com a configuração do `workers=15`, `passes=10`, `chunksize=1000` numa máquina com 16 núcleos (de 2.0 GHz).
>

### <a name="topic-summarization-and-analysis"></a>Resumo de tópico e análises

O resumo de tópico e análises é composta por dois blocos de notas, apesar de existirem não existem funções correspondentes no pacote de análise do documento.

No `4_Topic_Model_Summarization.ipynb`, ele mostra como resumir o conteúdo do documento com base num modelo de tópico LDA preparado. O resumo é aplicado a um modelo de tópico LDA aprendido no passo 3. Ele mostra como medir a importância ou de qualidade de um tópico com o tópico a medida de pureza do documento. Esta medida pureza parte do princípio de tópicos latentes dominam os documentos em que aparecem são semanticamente mais importantes do que a tópicos latentes tem rigidez espalhadas em vários documentos. Esse conceito foi introduzido no documento "[latente modelagem de tópico de resumo de Corpus de áudio](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf)."

Bloco de notas `5_Topic_Model_Analysis.ipynb` mostra como analisar o conteúdo por tópicos de uma coleção de documentos e correlacionar as informações por tópicos em relação a outros metadados associados à coleção de documentos. Alguns gráficos são introduzidos neste bloco de notas para ajudar os utilizadores a compreender melhor o tópico de aprender e a coleção de documentos.

Bloco de notas `6_Interactive_Visualization.ipynb` mostra como visualizar interativamente o modelo de tópico adquirido. Ele inclui quatro de visualização interativa de tarefas.

## <a name="conclusion"></a>Conclusão

Neste cenário do mundo real que realça como usar técnicas de análise de texto conhecidos (neste caso, a aprendizagem de frase e modelagem de tópico LDA) para produzir um modelo robusto e como Azure Machine Learning Workbench pode ajudar a controlar o desempenho do modelo e execute de forma totalmente integrada aprendizes uma escala elevada. Mais detalhadamente:

* Utilize a aprendizagem de frase e modelagem de tópico para processar uma coleção de documentos e criar um modelo robusto. Se a coleção de documentos for grande, o Azure Machine Learning Workbench facilmente pode dimensioná-lo a cópia de segurança e de fora. Além disso, os utilizadores têm a liberdade para executar experimentações no contexto de cálculo diferentes facilmente de dentro do Azure Machine Learning Workbench.

* O Azure Machine Learning Workbench oferece ambas as opções para executar blocos de notas num modo passo a passo e o script de Python para executar uma experiência completa.

* Com o Azure Machine Learning Workbench para encontrar o melhor número de tópicos de otimização de Hyper-parâmetro necessário para saber mais. O Azure Machine Learning Workbench pode ajudar a controlar o desempenho do modelo e execute facilmente aprendizes diferentes uma escala elevada.

* O Azure Machine Learning Workbench pode gerir o histórico de execuções e modelos de aprender. Permite que os cientistas de dados para identificar rapidamente os modelos com melhor desempenho e para localizar os scripts e os dados utilizados para gerá-los.

## <a name="references"></a>Referências

* **Timothy J. Hazen, Fred Richardson**, [ _modelagem Multiword frases com a árvore de expressões restrita para melhorada a Modelagem de tópico de discurso Conversacional_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Um idioma falado tecnologia Workshop (SLT), IEEE de 2012. IEEE, 2012.

* **Timothy J. Hazen**, [ _tópico latente modelagem de resumo de áudio Corpus_](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf). 12 de conferência anual da associação de comunicação de voz internacionais. 2011.

* **Michael Roder, Andreas ambas, Alexander Hinneburg**, [ _exploração do espaço de medidas de coerência de tópico_](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). Processos da conferência internacional ACM oitavo na pesquisa na Web e extração de dados. ACM, 2015.
