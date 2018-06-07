---
title: Reconstruir um conteúdo do pesquisável como índice ou a atualização da Azure Search | Microsoft Docs
description: Adicionar novos elementos, atualizar elementos existentes ou documentos ou eliminar documentos obsoletos uma reconstrução completa ou parcial indexação incremental para atualizar um índice da Azure Search.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.openlocfilehash: cb99096c1217fca1527b17946dc12390ddf3f62c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655711"
---
# <a name="how-to-scale-out-indexing-in-azure-seearch"></a>Como a indexação de escalamento horizontal no Azure Seearch

Como aumentam volumes de dados ou processamento tem de alteração, poderá achar que simples [Reconstrói e reindexing tarefas](search-howto-reindex.md) não são suficientes. 

Como primeiro passo para a aumentos dos pedidos de reunião, recomendamos que aumente o [escala e a capacidade](search-capacity-planning.md) dentro dos limites do seu serviço existente. 

Um segundo passo, se pode utilizar [indexadores](search-indexer-overview.md), adiciona mecanismos para indexação scaleable. Indexadores vêm com um agendador incorporado que lhe permite parcel saída indexação em intervalos regulares ou expandem o processamento fora da janela de 24 horas. Além disso, quando emparelhado com definições de origem de dados, indexadores permitem-lhe obter uma forma de paralelismo a criação de partições de dados e utilizando agendas para executar em paralelo.

### <a name="scheduled-indexing-for-large-data-sets"></a>Agendar a indexação para grandes conjuntos de dados

Agendamento é um mecanismo importante para o processamento de grandes conjuntos de dados e estatísticas de execução lenta, como a análise de imagem num pipeline pesquisa cognitivos. Processamento de indexador funciona numa janela de 24 horas. Se não for concluída dentro de 24 horas de processamento, os comportamentos de agendamento do indexador podem trabalhar em sua partido. 

Por predefinição, agendada indexação começa em intervalos específicos, com uma tarefa normalmente concluir antes da retomar a num intervalo agendado seguinte. No entanto, se o processamento não for concluída dentro do intervalo, o indexador deixa (porque foi esgotada tempo). No próximo intervalo, retoma do processamento onde pela última vez foi deixada, com a manutenção do sistema controlar de onde que ocorre. 

Em termos práticos, para cargas de índice expansão de vários dias, pode colocar o indexador com base numa agenda de 24 horas. Quando a indexação retoma para o seguinte stint de 24 horas, reinicia na última documento boa conhecido. Desta forma, um indexador pode trabalhar a forma através de um registo de segurança de documentos através de uma série de dias até que todos os documentos não processados são processados. Para obter mais informações sobre esta abordagem, consulte [indexação grandes conjuntos de dados](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>A indexação paralela

Uma segunda opção consiste em definir um estratégia de indexação de paralelo. Para não-rotina, viáveis que consomem muita indexação requisitos, tais como OCR em documentos digitalizados num pipeline pesquisa cognitivos, um estratégia de indexação de paralelo poderá ser a abordagem de direita para esse objetivo específico. Num pipeline sem causa pesquisa cognitivos, análise de imagem e processamento de linguagem natural são execução longos. A indexação Parallel num serviço que não está em simultâneo a processar os pedidos de consulta pode ser uma opção viável para trabalhar com um grande corpo de conteúdo de processamento lenta. 

Uma estratégia para processamento paralelo tem estes elementos:

+ Divida a origem de dados entre vários contentores ou várias pastas virtuais no interior do contentor do mesmo. 
+ Mapear cada mini conjunto de dados para um [origem data](https://docs.microsoft.com/rest/api/searchservice/create-data-source), emparelhada para os seus próprios [indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Para efetuar uma pesquisa cognitivos, referenciar a mesma [skillset](https://docs.microsoft.com/rest/api/searchservice/create-skillset) em cada definição do indexador.
+ Escreva para o mesmo índice de pesquisa de destino. 
+ Agende a todos os indexadores para executar em simultâneo.

> [!Note]
> A pesquisa do Azure não suporta a dedicação de réplicas ou partições para cargas de trabalho específicas. O risco de indexação simultâneas pesada é overburdening sistema para o detrimento de desempenho de consulta. Se tiver um ambiente de teste, implemente paralelas indexação existe primeiro compreender fala.

## <a name="configure-parallel-indexing"></a>Configurar a indexação paralela

Para indexadores, capacidade de processar aproximadamente ligado baseia um subsistema de indexador para cada unidade de serviço (SU) utilizada pelo seu serviço de pesquisa. Vários indexadores simultâneos são possíveis nos serviços de pesquisa do Azure aprovisionados na escalões básico ou padrão de ter, pelo menos, duas réplicas. 

1. No [portal do Azure](https://portal.azure.com), no seu dashboard do serviço de pesquisa **descrição geral** página, verifique o **escalão de preço** para confirmar pode acomodar a indexação paralela. Escalões básico e padrão oferecem várias réplicas.

2. No **definições** > **escala**, [aumentar réplicas](search-capacity-planning.md) para processamento paralelo: uma réplica adicional para cada carga de trabalho do indexador. Deixe um número suficiente para o volume de consulta existente. Sacrificar as cargas de trabalho de consulta para indexação não é um boa compromisso.

3. Distribua dados em vários contentores com um nível de indexadores de pesquisa do Azure podem aceder. Isto pode ser várias tabelas na base de dados do Azure SQL, vários contentores no armazenamento de Blobs do Azure ou várias coleções. Defina um objeto de origem de dados para cada tabela ou o contentor.

4. Crie e agendar vários indexadores para executar em paralelo:

   + Partem do princípio de um serviço com seis réplicas. Configure seis indexadores, cada um deles mapeado para uma origem de dados que contém uma-sexto do conjunto de dados para uma divisão de 6 forma do conjunto completo de dados. 

   + Ponto de cada indexador para o mesmo índice. Para cargas de trabalho de pesquisa cognitivos apontam cada indexador para o mesmo skillset.

   + Dentro de cada definição de indexador, agende o mesmo padrão de execução do tempo de execução. Por exemplo, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` cria uma agenda no 2018-05-15 em todos os indexadores, em execução em intervalos de oito horas.

Na hora agendada, todos os indexadores iniciar a execução, carregar dados, a aplicar enrichments (se tiver configurado um pipeline de pesquisa cognitivos) e a escrita para o índice. A pesquisa do Azure não bloquear o índice de atualizações. Escritas em simultâneo são geridas com a repetição se uma operação de escrita específica não funcionar na primeira tentativa.

> [!Note]
> Quando aumento as réplicas, considere aumentar a contagem da partição se o tamanho do índice é projetado para aumentar significativamente. Partições armazenam setores de conteúdo indexado; as mais partições que tem, o menor o setor cada um deles tem de armazenar.

## <a name="see-also"></a>Consulte também

+ [Descrição geral do Indexador](search-indexer-overview.md)
+ [A indexação no portal](search-import-data-portal.md)
+ [Indexador de base de dados SQL do Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Indexador da BD do Cosmos para o Azure](search-howto-index-cosmosdb.md)
+ [Indexador do Armazenamento de Blobs do Azure](search-howto-indexing-azure-blob-storage.md)
+ [Indexador do Armazenamento de Tabelas do Azure](search-howto-indexing-azure-tables.md)
+ [Segurança na pesquisa do Azure](search-security-overview.md)