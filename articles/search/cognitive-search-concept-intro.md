---
title: Pesquisa cognitivos para extração de dados, linguagem natural processamento na Azure Search | Microsoft Docs
description: Extração de dados, processamento de linguagem natural (NLP) e processamento de imagem para criar conteúdo pesquisável na Azure Search utilizando as competências cognitivos de indexação.
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/04/2018
ms.author: heidist
ms.openlocfilehash: 853e8fbc0c7044dc97a29254bfd4130e7f81bbbd
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="what-is-cognitive-search"></a>O que é pesquisa cognitivos?

Pesquisa cognitivos é uma funcionalidade de pré-visualização do Azure Search, disponível em todas as camadas numa Sul Central-nos e Europa Ocidental, que adiciona AI a indexação de cargas de trabalho. Extração de dados, processamento de linguagem natural e processamento de imagem durante a indexação localizar as informações latentes o conteúdo não estruturado ou não pesquisáveis e torna pesquisável na Azure Search.

Integração de AI é efetuada através de *competências cognitivos* que enriquecer a documentos de origem através de processos sequenciais, na rota num índice de pesquisa. 

![Diagrama de pipeline de pesquisa cognitivos](./media/cognitive-search-intro/cogsearch-architecture.png "descrição geral do pipeline de pesquisa cognitivos")

Utilizado durante a indexação de competências podem ser personalizado ou predefinido:

+ [Prefined competências](cognitive-search-predefined-skills.md) baseiam os algoritmos de AI mesmos utilizados em APIs de serviços cognitivos: [reconhecimento de entidade com o nome](cognitive-search-skill-named-entity-recognition.md), [extração da expressão de chave](cognitive-search-skill-keyphrases.md), e [OCR](cognitive-search-skill-ocr.md) são apenas alguns. 

+ [Competências personalizadas](cognitive-search-create-custom-skill-example.md) podem ser desenvolvidas por si para qualquer processamento especializado que precisa. Exemplos de competências personalizados podem ser um classificador de módulo ou documento de entidade personalizado direcionada para um domínio específico, como o departamento financeiro, publicações científicos ou medicine.

> [!NOTE]
> A Pesquisa Cognitiva encontra-se em pré-visualização pública e a execução do conjunto de competências é, de momento, disponibilizada gratuitamente. Posteriormente, iremos anunciar os preços desta capacidade.

## <a name="components-of-cognitive-search"></a>Componentes de pesquisa cognitivos

O pipeline de pesquisa cognitivos baseia-se no [da Azure Search *indexadores* ](search-indexer-overview.md) que pesquisam origens de dados e fornece processamento de índice de ponto a ponto. Competências agora estão ligadas ao indexadores, intercetar e no enriquecimento documentos, de acordo com o skillset definir. Depois de indexada, pode aceder a conteúdo através de pedidos de pesquisa através de todos os [consultar tipos suportados pelo Azure Search](search-query-overview.md).  Se estiver familiarizado com indexadores, esta secção explica os passos.

### <a name="source-data-and-document-cracking-phase"></a>Dados de origem e o documento cracking fase

O início do pipeline, tem em texto não estruturado ou conteúdo não sejam de texto (como imagens e documentos digitalizados JPEG ficheiros). Dados tem de existir um serviço de armazenamento de dados do Azure que possa ser acedido por um indexador. Indexadores podem "violar" documentos de origem, a extrair texto de origem de dados.

![Documento cracking fase](./media/cognitive-search-intro/document-cracking-phase-blowup.png "cracking do documento")

 Fontes suportadas incluem o armazenamento de Blobs do Azure, table storage do Azure, SQL Database do Azure e Azure Cosmos DB. Conteúdo baseado em texto pode ser extraído dos seguintes tipos de ficheiro: PDFs, Word, PowerPoint, ficheiros CSV. Para obter uma lista completa, consulte [formatos suportados](search-howto-indexing-azure-blob-storage.md#supported-document-formats).

### <a name="cognitive-skills-and-enrichment-phase"></a>Cognitivos competências e fase sem causa

Sem causa é efetuada através de *competências cognitivos* efetuar operações atómicas. Por exemplo, assim que tiver conteúdo de texto de um PDF, pode aplicar a deteção de idioma de reconhecimento de entidade ou extração da expressão de chave para produzir novos campos no seu índice que não estão disponíveis nativamente na origem. Conjunto, a coleção de competências utilizado no seu pipeline denomina-se um *skillset*.  

![Fase de sem causa](./media/cognitive-search-intro/enrichment-phase-blowup.png "fase sem causa")

Um skillset baseia-se no [predefinidas competências cognitivos](cognitive-search-predefined-skills.md) ou [competências personalizadas](cognitive-search-create-custom-skill-example.md) forneça e estabelecer ligação com o skillset. Um skillset pode ser muito complexos ou mínima e determina não só o tipo de processamento, mas também a ordem das operações. Um skillset plus os mapeamentos de campo definidos como parte de um indexador Especifica completamente o pipeline sem causa. Para obter mais informações sobre a extração todas estas duas em conjunto, consulte [definir um skillset](cognitive-search-defining-skillset.md).

Internamente, o pipeline gera uma coleção de documentos avançadas. Pode decidir que partes de documentos avançadas devem ser mapeadas para indexable campos no seu índice de pesquisa. Por exemplo, se aplicar a extração de expressões de chave e as competências de reconhecimento de entidade, em seguida, os novos campos iriam tornar-se parte do documento avançado e podem ser mapeadas para os campos no seu índice. Consulte [anotações](cognitive-search-concept-annotations-syntax.md) para saber mais sobre formação que resultam de entrada/saída.

### <a name="search-index-and-query-based-access"></a>Índice de pesquisa e de acesso baseadas em consultas

Quando o processamento é concluído, tem um corpus pesquisa consiste em documentos avançados, completamente texto-pesquisável na Azure Search. [Consultar o índice](search-query-overview.md) é a forma como os programadores e os utilizadores aceder aos conteúdos avançados gerado pelo pipeline. 

![Índice com o ícone de pesquisa](./media/cognitive-search-intro/search-phase-blowup.png "índice com o ícone de pesquisa")

O índice é como a quaisquer outros poderá criar para a pesquisa do Azure: pode complementar com analisadores personalizados, invocar consultas de pesquisa difusa, adicionar pesquisa filtrada ou testar com a classificação de perfis para reshape os resultados da pesquisa.

Os índices são gerados a partir de um esquema de índice que define os campos, atributos, e outras construções ligado a um índice específico, tais como perfis de classificação e sinónimo mapas. Depois de um índice é definido e preenchido, pode indexar incrementalmente para recolher os documentos de origem novos e atualizados. Determinados modificações requerem uma reconstrução completa. Deve utilizar um pequeno conjunto de dados até que a estrutura de esquema está estável. Para obter mais informações, consulte [como reconstruir um índice](search-howto-reindex.md).

<a name="feature-concepts"></a>

## <a name="key-features-and-concepts"></a>Funcionalidades e conceitos principais

| Conceito | Descrição| Ligações |
|---------|------------|-------|
| Skillset | Um nível superior com o nome de recurso que contém uma coleção de competências. Um skillset é o pipeline de sem causa. É invocado durante a indexação por um indexador. | [Definir um skillset](cognitive-search-defining-skillset.md) |
| Skill cognitivos | Uma transformação atómica um pipeline sem causa. Muitas vezes, é um componente que extrai ou infere estrutura e, por conseguinte, aumenta a compreensão dos dados de entrada. Quase sempre, o resultado é baseado em texto e o processamento é o processamento de linguagem natural ou processamento de imagem que extrai ou gera o texto de entradas de imagem. Saída de um skill pode ser mapeada para um campo num índice ou utilizada como uma entrada para uma sem causa a jusante. Um skill está predefinida e fornecida pela Microsoft ou personalizada: criada e implementada por si. | [Competências predefinidas](cognitive-search-predefined-skills.md) |
| Extração de dados | Abrange uma vasta gama de processamento, mas relativas à pesquisa cognitivos, o skill de reconhecimento de entidade com o nome é normalmente utilizada para extrair dados (entidade) a partir de uma origem que não fornece informações nativamente. | [Entidade nomeado reconhecimento Skill](cognitive-search-skill-named-entity-recognition.md)| 
| Processamento de imagem | Infere texto a partir de uma imagem, tal como a capacidade para reconhecer um landmark ou extrai o texto a partir de uma imagem. Exemplos comuns incluem OCR para lifting carateres a partir de um ficheiro de documentos digitalizados (JPEG) ou reconhecer um nome completo de uma fotografia que contém um início de sessão completo. | [Imagem Analysis Skill](cognitive-search-skill-image-analysis.md) ou [OCR Skill](cognitive-search-skill-ocr.md)
| Processamento de linguagem natural | Texto de processamento para insights e informações sobre as entradas de texto. Deteção de idioma, análise de dados de sentimento e extração da expressão de chave são competências abrangidos de processamento de linguagem natural.  | [Chave o frase extração Skill](cognitive-search-skill-keyphrases.md), [Skill de deteção de idioma](cognitive-search-skill-language-detection.md), [Skill de análise de dados de sentimento](cognitive-search-skill-sentiment.md) |
| Documento cracking | O processo de extrair ou a criação de conteúdos de texto de fontes não sejam de texto durante a indexação. Reconhecimento de caráter Optical (OCR) é um exemplo, mas geralmente refere-se a funcionalidade de indexador principal como o indexador extrai o conteúdo dos ficheiros de aplicação. A origem de dados, fornecendo a localização do ficheiro de origem e a definição de indexador fornecer mapeamentos campo, são ambos os principais fatores documento cracking. | Consulte [indexadores](search-indexer-overview.md) |
| Formação | Consolidar os fragmentos de texto para uma estrutura maior, ou por outro lado dividir maior segmentos de texto para um tamanho gerível para obter mais processamento a jusante. | [Shaper Skill](cognitive-search-skill-shaper.md), [texto fusão Skill](cognitive-search-skill-textmerger.md), [Skill de divisão de texto](cognitive-search-skill-textsplit.md) |
| Documentos avançados | Uma temporárias estrutura interna, não acessível diretamente no código. Documentos avançados são gerados durante o processamento, mas apenas finais saídas são mantidas no índice de pesquisa. Mapeamentos de campo determinam os elementos de dados são adicionados ao índice. | Consulte [aceder a documentos avançados](cognitive-search-tutorial-blob.md#access-enriched-document). |
| Indexador |  Um crawler que extrai dados pesquisáveis e metadados a partir de uma origem de dados externa e preenche um índice com base nos mapeamentos campo a campo entre o índice e a origem de dados para cracking do documento. Para enrichments cognitivos pesquisa, o indexador invoca um skillset e contém os mapeamentos de campo associar saída sem causa aos campos de destino no índice. A definição de indexador contém todas as instruções e referências para operações de pipeline e o pipeline é invocado quando executar o indexador. | [Indexadores](search-indexer-overview.md) |
| Origem de Dados  | Um objeto utilizado por um indexador para ligar a uma origem de dados externas dos tipos suportados no Azure. | Consulte [indexadores](search-indexer-overview.md) |
| Índice | Um corpus persistente de pesquisa na Azure Search, criada a partir de um esquema de índice que define a estrutura de campo e a utilização. | [Índices na pesquisa do Azure](search-what-is-an-index.md) | 


## <a name="where-do-i-start"></a>Por onde devo começar?

**Passo 1: Criar um serviço de pesquisa numa região fornecer as APIs** 

+ EUA Centro-Sul
+ Europa Ocidental

**Passo 2: Experiência prática a mestre o fluxo de trabalho**

+ [Início rápido (portal)](cognitive-search-quickstart-blob.md)
+ [Tutorial (pedidos de HTTP)](cognitive-search-tutorial-blob.md)
+ [Competências personalizadas de exemplo (c#)](cognitive-search-create-custom-skill-example.md)

**Passo 3: Rever a API (REST apenas)**

Atualmente, são fornecidas apenas as APIs REST. Utilize `api-version=2017-11-11-Preview` em todos os pedidos. Utilize as APIs seguintes para criar uma solução de pesquisa cognitivos. APIs apenas dois são adicionadas ou expandidas para pesquisa cognitivos. Outras APIs tem a mesma sintaxe que as versões geralmente disponíveis.

| API REST | Descrição |
|-----|-------------|
| [Criar origem de dados](https://docs.microsoft.com/rest/api/searchservice/create-data-source)  | Um recurso de identificar uma origem de dados externos a fornecer os dados de origem utilizados para criar documentos avançados.  |
| [Criar Skillset (api-version = 2017-11-11-pré-visualização)](ref-create-skillset.md)  | Um recurso em coordenação a utilização de [predefinidas competências](cognitive-search-predefined-skills.md) e [personalizadas competências cognitivos](cognitive-search-custom-skill-interface.md) utilizado um pipeline sem causa durante a indexação. |
| [Criar o índice](https://docs.microsoft.com/rest/api/searchservice/create-index)  | Um esquema expressa um índice da Azure Search. Os campos no índice mapeiam os campos na origem de dados ou aos campos fabricados durante a fase de sem causa (por exemplo, um campo para nomes de organização criado pelo reconhecimento de entidade). |
| [Criar indexador (api-version = 2017-11-11-pré-visualização)](ref-create-skillset.md)  | Um recurso que define os componentes utilizados durante a indexação: incluindo uma origem de dados, uma skillset, associações de campo de origem e de estruturas de dados intermédios ao índice de destino e o índice de si próprio. Executar o indexador é o acionador para ingestão de dados e sem causa. O resultado é um corpus de pesquisa com base no esquema de índice, preenchido com dados de origem, enriquecidos através de skillsets.  |

**Lista de verificação: Um fluxo de trabalho normal**

1. Subconjunto os dados de origem do Azure para uma amostra representativa. Tempo demora a indexação para começar a utilizar um conjunto de dados pequeno e representativo e, em seguida, crie-cópia de segurança incremental porque a sua solução evoluiu.

1. Criar um [objeto de origem de dados](https://docs.microsoft.com/rest/api/searchservice/create-data-source) na pesquisa do Azure para fornecer uma cadeia de ligação para obtenção de dados.

1. Criar um [skillset](ref-create-skillset.md) com passos de sem causa.

1. Definir o [esquema de índice](https://docs.microsoft.com/rest/api/searchservice/create-index). O *campos* coleção inclui campos de dados de origem. Também deve stub os campos adicionais para conter valores gerados para o conteúdo criado durante sem causa.

1. Definir o [indexador](ref-create-skillset.md) referenciar a origem de dados, skillset e índice.

1. Dentro do indexador, adicionar *outputFieldMappings*. Esta secção mapeia o resultado da skillset (no passo 3) para os campos de entradas do esquema de índice (no passo 4).

1. Enviar *criar indexador* pedido que acabou de criar (um pedido POST com uma definição de indexador no corpo do pedido) para expressar o indexador na Azure Search. Este passo é como executar o indexador invocar o pipeline.

1. Execute consultas para avaliar os resultados e modificar o código para atualização skillsets, de esquema ou de configuração do indexador.

1. [Repor o indexador](search-howto-reindex.md) antes de recriar o pipeline.

Para obter mais informações sobre específicos questões ou problemas, consulte [sugestões de resolução de problemas](cognitive-search-concept-troubleshooting.md).

## <a name="next-steps"></a>Passos Seguintes

+ [Documentação de pesquisa cognitivos](cognitive-search-resources-documentation.md)
+ [Início rápido: Tente cognitivos pesquisa um portal explicação passo a passo](cognitive-search-quickstart-blob.md)
+ [Tutorial: Saber a APIs de pesquisa cognitivos](cognitive-search-tutorial-blob.md)