---
title: Perguntas e respostas uma correspondência com o Azure Machine Learning Workbench | Documentos da Microsoft
description: Como utilizar diversos métodos de aprendizagem eficaz para corresponder ao abrir terminou consultas para pré-existente FAQ/respostas a perguntas de pares.
services: machine-learning
documentationcenter: ''
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ROBOTS: NOINDEX
ms.openlocfilehash: e0f6148e1fb28838bf99c63fbfbfbfe8cd127c8c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973212"
---
#  <a name="q--a-matching-using-azure-machine-learning-workbench"></a>Perguntas e respostas uma correspondência com o Azure Machine Learning workbench

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Responder a perguntas ended abertas, é difícil e muitas vezes requer um esforço manual de especialistas no assunto (SMEs). Para ajudar a reduzir as demandas na SMEs internos, as empresas, muitas vezes, criam listas de perguntas mais frequentes (FAQ) como um meio de prestar assistência aos utilizadores. Este exemplo apresenta vários métodos de aprendizado de máquina em vigor para corresponder ao abrir consultas ended para pré-existente pares de pergunta/respostas de perguntas frequentes. Este exemplo demonstra um processo de desenvolvimento fácil para a criação de uma solução com o Azure Machine Learning Workbench. 

## <a name="link-to-the-gallery-github-repository"></a>Ligar para o repositório do GitHub de galeria
[https://github.com/Azure/MachineLearningSamples-QnAMatching](https://github.com/Azure/MachineLearningSamples-QnAMatching)

## <a name="overview"></a>Descrição geral

Este exemplo resolve o problema de mapeamento de perguntas de utilizador para pré-existente pares (perguntas e respostas) pergunta e resposta porque, normalmente, é fornecido numa lista de perguntas mais frequentes (ou seja, um FAQ) ou em de perguntas e & um pares presentes nos Web sites, como [pilha Overflow](https://stackoverflow.com/). Há muitas abordagens para corresponder a uma pergunta para a sua resposta correta, como encontrar a resposta que é o mais semelhante à pergunta. No entanto, neste exemplo perguntas ended abertas correspondem a anteriormente frequentes, supondo que cada resposta nas perguntas frequentes pode responder a várias perguntas semanticamente equivalentes.

As principais etapas necessárias para fornecer essa solução são os seguintes:

1. Limpar e processar dados de texto.
2. Saiba mais informativos frases, que são seqüências de várias palavras que fornecem mais informações quando visualizado na sequência que quando tratada de maneira independente.
3. Extrair funcionalidades a partir de dados de texto.
4. Preparar modelos de classificação de texto e avaliar o desempenho do modelo.


## <a name="prerequisites"></a>Pré-requisitos

As pré-requisitos para executar este exemplo são os seguintes:

1. Uma [conta do Azure](https://azure.microsoft.com/free/) (avaliações gratuitas estão disponíveis).
2. Uma cópia instalada do [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) seguintes a [guia de início rápido de instalação](quickstart-installation.md) para instalar o programa e criar uma área de trabalho.
3. Neste exemplo pode ser executado em qualquer contexto de cálculo. No entanto, recomenda-se para executá-lo numa máquina com vários núcleo com, pelo menos, de 16GB de memória e espaço em disco de 5GB.

## <a name="create-a-new-workbench-project"></a>Criar um novo projeto do workbench

Crie um novo projeto com este exemplo como um modelo:
1.  Abrir o Azure Machine Learning Workbench
2.  Sobre o **projetos** página, clique no **+** iniciar sessão e selecionar **novo projeto**
3.  Na **criar novo projeto** painel, preencha as informações para o novo projeto
4.  Na **modelos de projeto de pesquisa** caixa de pesquisa, escreva "Q & A correspondência" e selecione o modelo
5.  Clique em **Criar**.

## <a name="data-description"></a>Descrição de dados

O conjunto de dados utilizado neste exemplo é uma pilha Exchange dados Despejar armazenado em [archive.org](https://archive.org/details/stackexchange). Estes dados são um despejo anónimo de todo o conteúdo usuários sobre o [rede Stack Exchange](https://stackexchange.com/). Cada site na rede esteja formatada como consistindo em arquivos XML num arquivo separado comprimido por meio de 7-zip usando bzip2 compressão. Cada arquivo de site inclui mensagens, os utilizadores, os votos, comentários, PostHistory e PostLinks. 

### <a name="data-source"></a>Origem de dados

Este exemplo utiliza o [(10 GB) de dados de Postagens](https://archive.org/download/stackexchange/stackoverflow.com-Posts.7z) e [PostLinks dados (515 MB)](https://archive.org/download/stackexchange/stackoverflow.com-PostLinks.7z) do site Stack Overflow. Para informações de esquema completo, consulte a [Readme. txt](https://ia800500.us.archive.org/22/items/stackexchange/readme.txt). 

O `PostTypeId` campo nos dados de Postagens indica se uma postagem é uma `Question` ou um `Answer`. O `PostLinkTypeId` campo nos dados PostLinks indica se duas mensagens ligadas ou duplicam. Postagens de pergunta incluem, geralmente, algumas etiquetas, o que são palavras-chave que categorizar uma pergunta com outras perguntas semelhantes/duplicado. Existem algumas marcas com alta frequência, tais como `javascript`, `java`, `c#`, `php` etc., consistem num grande número de postagens de pergunta. Neste exemplo, um subconjunto de p & um pares com o `javascript` marca é extraída.

Additionionally, uma postagem de perguntas pode estar relacionada a várias mensagens de resposta ou mensagens duplicadas pergunta. Para criar uma lista de perguntas frequentes sobre a partir desses dois conjuntos de dados, são considerados alguns critérios de recolha de dados. Três conjuntos de dados compilados são selecionados com um script SQL, que não está incluído neste exemplo. A descrição de dados resultante é o seguinte:

- `Original Questions (Q)`: Pergunta postagens são feitas e respondidas no site Stack Overflow.
- `Duplications (D)`: Pergunta postagens duplicadas outras perguntas já existentes (`PostLinkTypeId = 3`), que são as perguntas originais. Duplicações são consideradas semanticamente equivalentes às perguntas originais no sentido de que a resposta fornecida para a pergunta original também responde à nova pergunta duplicada.
- `Answers (A)`: Cada pergunta original e sua duplicações podem ligar a mais do que uma resposta postagens. Neste exemplo seleciona apenas a mensagem de resposta também é aceite pelo autor original (filtradas pela `AcceptedAnswerId`) ou tem a pontuação mais alta (classificados pelo `Score`) nas perguntas originais. 

A combinação desses três conjuntos de dados cria pares de perguntas e respostas em que uma resposta (A) é mapeada para uma pergunta original (P) e a várias perguntas duplicadas (D) como ilustraram o seguinte diagrama de dados:

<table><tr><td><img id ="data_diagram" src="media/scenario-qna-matching/data_diagram.png"></td></tr></table>

### <a name="data-structure"></a>Estrutura dos dados

O esquema de dados e os links de transferência direta dos três conjuntos de dados pode ser encontrado na tabela a seguir:

| Conjunto de dados | Campo | Tipo | Descrição
| ----------|------------|------------|--------
| [Perguntas](https://bostondata.blob.core.windows.net/stackoverflow/orig-q.tsv.gz) | Id | Cadeia | O ID exclusivo da pergunta (chave primária)
|  | AnswerId | Cadeia | O ID exclusivo da resposta por pergunta
|  | Text0 | Cadeia | Os dados de texto não processado, incluindo o título e o corpo a pergunta
|  | CreationDate | Carimbo de data/hora | O carimbo de hora de quando a pergunta foi pedida
| [dupes](https://bostondata.blob.core.windows.net/stackoverflow/dup-q.tsv.gz) | Id | Cadeia | O ID exclusivo de duplicação (chave primária)
|  | AnswerId | Cadeia | O ID de resposta associado a duplicação
|  | Text0 | Cadeia | Os dados de texto não processado, incluindo o título e o corpo a duplicação
|  | CreationDate | Carimbo de data/hora | O carimbo de hora de quando a duplicação foi pedida
| [respostas](https://bostondata.blob.core.windows.net/stackoverflow/ans.tsv.gz)  | Id | Cadeia | O ID exclusivo da resposta (chave primária)
|  | text0 | Cadeia | Os dados de texto não processado da resposta


## <a name="scenario-structure"></a>Estrutura do cenário

O exemplo de correspondência de perguntas e respostas é apresentado por meio de três tipos de ficheiros. O primeiro tipo é uma série de blocos de notas do Jupyter que mostram as descrições passo a passo do fluxo de trabalho inteiro. O segundo tipo é um conjunto de arquivos de Python contêm módulos personalizados do Python para extração de aprendizagem e a funcionalidade de expressões. Estes módulos de Python são suficientemente genéricos para servir não apenas neste exemplo, mas também outros casos de utilização. O terceiro tipo é um conjunto de arquivos de Python para ajustar o hiper parâmetros e acompanhar o desempenho do modelo com o Azure Machine Learning Workbench.

Os ficheiros neste exemplo são organizados da seguinte forma.

| Nome de Ficheiro | Tipo | Descrição
| ----------|------------|--------
| `Image` | Pasta | A pasta utilizada para guardar imagens para o ficheiro Leia-me
| `notebooks` | Pasta | A pasta de blocos de notas do Jupyter
| `modules` | Pasta | A pasta de módulos de Python
| `scripts` | Pasta | A pasta de ficheiros do Python
| `notebooks/Part_1_Data_Preparation.ipynb` | Bloco de Notas do Jupyter | Acessar os dados de exemplo, processar previamente o texto e preparar o treinamento e conjuntos de dados de teste
| `notebooks/Part_2_Phrase_Learning.ipynb` | Bloco de Notas do Jupyter | Saiba mais informativos frases e indexar o texto em representações de matriz de palavras (BOWs)
| `notebooks/Part_3_Model_Training_and_Evaluation.ipynb` | Bloco de Notas do Jupyter | Extrair funcionalidades, preparar os modelos de classificação de texto e de desempenho do modelo de avaliação
| `modules/__init__.py` | Ficheiro de Python | O ficheiro de inicialização de pacote do Python
| `modules/phrase_learning.py` | Ficheiro de Python | Os módulos de Python utilizados para transformar os dados não processados e saiba frases informativos
| `modules/feature_extractor.py` | Ficheiro de Python | Os módulos de Python extrair funcionalidades para a preparação de modelos
| `scripts/naive_bayes.py` | Ficheiro de Python | O Python para otimizar o hiper parâmetros no modelo de classificação bayesiana Ingênua
| `scripts/random_forest.py` | Ficheiro de Python | O Python para otimizar o hiper parâmetros no modelo de floresta de Random
| `README.md` | Ficheiro de markdown | O ficheiro de markdown Leia-me

> [!NOTE]
> A série de blocos de notas baseia-se em Python 3.5.
> 

### <a name="data-ingestion-and-transformation"></a>Ingestão de dados e transformação

Os três conjuntos de dados compilados são armazenados no armazenamento de BLOBs e são obtidos numa `Part_1_Data_Preparation.ipynb` bloco de notas.  

Antes dos modelos de classificação de texto de treinamento, o texto nas perguntas é limpo e pré-processado para excluir os trechos de código. Um aprendizado não supervisionado frase é aplicado sobre o material de treinamento para obter sequências de várias palavras informativas. Estas expressões são representadas como unidades de única composta palavra a jusante featurization de matriz de palavras (BOWs) utilizado pelos modelos de classificação de texto.

As descrições detalhadas passo a passo de processamento prévio de texto e de aprendizagem de expressão podem ser encontradas nos blocos de notas `Part_1_Data_Preparation.ipynb` e `Part_2_Phrase_Learning.ipynb`, respectivamente.

### <a name="modeling"></a>Modelação

Este exemplo foi concebido para pontuar novas questões em relação a já existente e respostas um pares pelo texto de treinamento, modelos de classificação em que cada pré-existente perguntas e respostas um par é uma classe exclusiva e um subconjunto das perguntas duplicadas para cada par de perguntas e respostas estão disponíveis como material de treinamento. No exemplo, as perguntas e 75% das perguntas duplicadas original são mantidos durante o treinamento e a 25% mais recentemente lançadas perguntas duplicadas são mantidos horizontal como dados de avaliação.

O modelo de classificação utiliza um método de ensemble para agregar três classificadores de bases, incluindo **Bayesiana Ingênua**, **máquina de Vetor com suporte**, e **floresta de Random**. Em cada classificador de base, o `AnswerId` é utilizado como a etiqueta de classe e as representações de BOWs for utilizado como os recursos.

O processo de treinamento de modelo é ilustrado na `Part_3_Model_Training_and_Evaluation.ipynb`.

### <a name="evaluation"></a>Avaliação

Duas métricas de avaliação diferentes são utilizadas para avaliar o desempenho. 
1. `Average Rank (AR)`: indica a posição de média em que a resposta correta for encontrada na lista de pares de perguntas e respostas obtidas (fora do conjunto completo de 103 classes de resposta). 
2. `Top 3 Percentage`: indica a percentagem de teste de perguntas que a resposta correta pode ser obtida em Opções de três principais no retornado com a classificação de lista. 

A avaliação é demonstrada `Part_3_Model_Training_and_Evaluation.ipynb`.


## <a name="conclusion"></a>Conclusão

Este exemplo realça como utilizar técnicas de análise de texto bem conhecidas, como a classificação de aprendizagem e de texto expressão, para produzir um modelo robusto. Também demonstra como o Azure Machine Learning Workbench pode ajudar com o desempenho do modelo de desenvolvimento e controle interativo da solução. 

Alguns destaques principais deste exemplo são:

- A pergunta e o problema de correspondência de resposta podem ser resolvidos com eficiência com modelos de classificação de texto e de aprendizagem do frase.
- Demonstração de desenvolvimento interativo do modelo com o Azure Machine Learning Workbench e o bloco de notas do Jupyter.
- O Azure Machine Learning Workbench gerencia o histórico de execuções e modelos de aprender com as métricas de avaliação para fins de comparação de registo. Esses recursos permite a rápida hiper-parâmetros e ajuda a identificar os modelos com melhor desempenho.


## <a name="references"></a>Referências

Timothy J. Hazen, Fred Richardson, [ _modelagem Multiword frases com a árvore de expressões restrita para melhorada a Modelagem de tópico de discurso Conversacional_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Um idioma falado tecnologia Workshop (SLT), IEEE de 2012. IEEE, 2012.

Timothy J. Hazen, [ _MCE técnicas de treinamento para a identificação de tópico de documentos de áudio faladas_ ](http://ieeexplore.ieee.org/abstract/document/5742980/) em transações de IEEE em áudio, voz e de processamento de linguagem, vol. 19, não. 8, pp. 2451-2460, Novembro de 2011.
