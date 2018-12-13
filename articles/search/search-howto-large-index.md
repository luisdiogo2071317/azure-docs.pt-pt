---
title: Aumentar horizontalmente a indexação com indexadores incorporados - Azure Search
description: Adicionar novos elementos, atualizar elementos existentes ou documentos ou eliminar documentos obsoletos numa recriação completa ou a indexação incremental parcial para atualizar o índice da Azure Search.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 5f268de43f4f860458c062cb80e5bea0134b4407
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316692"
---
# <a name="how-to-scale-out-indexing-in-azure-search"></a>Como a indexação de escalamento horizontal no Azure Search

À medida que os volumes de dados aumentam ou necessidades de processamento, pode achar que simples [recompilações e reindexação tarefas](search-howto-reindex.md) não são o suficiente. 

Como um primeiro passo na direção de atendendo à demanda aumentada, recomendamos que aumente a [dimensionamento e a capacidade](search-capacity-planning.md) dentro dos limites do seu serviço existente. 

Uma segunda etapa, se puder usar [indexadores](search-indexer-overview.md), adiciona mecanismos para indexação dimensionável. Indexadores vir com um agendador interno que permite que dividir indexação em intervalos regulares ou expandir o processamento além do período de 24 horas. Além disso, quando combinado com definições de origem de dados, indexadores ajudá-lo a alcançar um formulário de paralelismo através da criação de partições de dados e utilização de agendas para executar em paralelo.

### <a name="scheduled-indexing-for-large-data-sets"></a>Agendado a indexação de grandes conjuntos de dados

Agendar é um importante mecanismo para processar grandes conjuntos de dados e análises de execução lenta, como a análise de imagem num pipeline de pesquisa cognitiva. Processamento de indexador opera durante um período de 24 horas. Se não for concluída dentro de 24 horas de processamento, os comportamentos de agendamento do indexador podem trabalhar em seu favor. 

Por design, agendada indexação começa em intervalos específicos, com uma tarefa normalmente concluir antes de continuar no próximo intervalo agendado. No entanto, se o processamento não for concluída dentro do intervalo, o indexador será interrompido (porque ele ficou sem tempo). No intervalo seguinte, onde pela última vez parou, com a manutenção do sistema de retoma de processamento de controle de onde o que ocorre. 

Em termos práticos, para cargas de índice que abrangem vários dias, pode colocar o indexador com base numa agenda de 24 horas. Durante a indexação CVS para o próximo Incursão de 24 horas, ele será reiniciada no último documento bom conhecido. Dessa forma, um indexador pode trabalhar sua maneira por meio de uma lista de pendências do documento em relação a uma série de dias até que todos os documentos não processados são processados. Para obter mais informações sobre esta abordagem, consulte [indexação de grandes conjuntos de dados](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

<a name="parallel-indexing"></a>

## <a name="parallel-indexing"></a>Indexação paralela

Uma segunda opção consiste em configurar um paralelo estratégia de indexação. Para não-rotina, computacionalmente intensivas, a indexação de requisitos, tais como OCR em documentos digitalizados num pipeline de pesquisa cognitiva, um estratégia de indexação de paralelo poderá ser a abordagem correta para esse objetivo específico. Num pipeline de enriquecimento da pesquisa cognitiva, análise de imagem e processamento de linguagem natural são de longa. Indexação paralela num serviço que não está em simultâneo a processar pedidos de consulta pode ser uma opção viável para trabalhar com uma grande quantidade de conteúdo de processamento lento. 

Uma estratégia para processamento paralelo tem esses elementos:

+ Divida a origem de dados entre vários contentores ou várias pastas virtuais dentro do mesmo contêiner. 
+ Mapear cada mini conjunto de dados para um [origem data](https://docs.microsoft.com/rest/api/searchservice/create-data-source), juntamente a sua própria [indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Para a pesquisa cognitiva, referenciar o mesmo [conjunto de capacidades](https://docs.microsoft.com/rest/api/searchservice/create-skillset) em cada definição de indexador.
+ Escreva para o mesmo índice de pesquisa de destino. 
+ Agende todos os indexadores para executar ao mesmo tempo.

> [!Note]
> O Azure Search não suporta dedicar réplicas ou partições para cargas de trabalho específicas. O risco de indexação simultâneas pesado é overburdening seu sistema para o dano de desempenho de consulta. Se tiver um ambiente de teste, implemente paralela indexação lá em primeiro lugar para entender as compensações.

## <a name="configure-parallel-indexing"></a>Configurar a indexação paralela

Para indexadores, capacidade de processamento é menos baseada num subsistema de indexador para cada unidade de serviço (SU) utilizada pelo seu serviço de pesquisa. Vários indexadores simultâneos são possíveis em serviços Azure Search aprovisionados nos escalões básico ou Standard de ter, pelo menos, duas réplicas. 

1. Na [portal do Azure](https://portal.azure.com), no seu dashboard de serviço de pesquisa **descrição geral** página, verifique o **escalão de preço** para confirmar que pode acomodar a indexação paralela. Escalões básico e Standard oferecem várias réplicas.

2. Na **configurações** > **dimensionamento**, [aumentar réplicas](search-capacity-planning.md) para processamento paralelo: uma réplica adicional para cada carga de trabalho do indexador. Deixe um número suficiente de volume de consulta existente. Sacrificar cargas de trabalho de consulta para indexação não é uma boa troca.

3. Distribua os dados em vários contentores num nível que podem atingir indexadores do Azure Search. Isto pode ser várias tabelas na base de dados do Azure SQL, vários contentores no armazenamento de Blobs do Azure ou várias coleções. Defina um objeto de origem de dados para cada tabela ou o contentor.

4. Criar e agendar vários indexadores para executar em paralelo:

   + Suponha que um serviço com seis réplicas. Configure seis indexadores, cada um deles mapeado para uma origem de dados que contém um sexto do conjunto de dados para uma divisão de 6-vias de todo o conjunto de dados. 

   + Aponte cada indexador para o mesmo índice. Para cargas de trabalho de pesquisa cognitiva, aponte cada indexador para o mesmo conjunto de capacidades.

   + Dentro de cada definição de indexador, agende o mesmo padrão de tempo de execução. Por exemplo, `"schedule" : { "interval" : "PT8H", "startTime" : "2018-05-15T00:00:00Z" }` cria uma agenda em 2018-05-15 em todos os indexadores, em execução em intervalos de oito horas.

Na hora agendada, todos os indexadores inicia a execução, carregamento de dados, aplicar possível (se tiver configurado um pipeline de pesquisa cognitiva) e escrever para o índice. O Azure Search não bloquear o índice para atualizações. Escritas simultâneas são geridas, com a repetição se uma gravação particular não tiver êxito na primeira tentativa.

> [!Note]
> Ao aumentar as réplicas, considere aumentar o número de partições, se o tamanho do índice, está projetado para aumentar significativamente. Partições armazenam setores de conteúdo indexado; as partições mais tiver, menor será o setor de cada um deles tem de armazenar.

## <a name="see-also"></a>Consulte também

+ [Descrição geral do Indexador](search-indexer-overview.md)
+ [Indexação no portal](search-import-data-portal.md)
+ [Indexador da base de dados SQL do Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Indexador da BD do Cosmos para o Azure](search-howto-index-cosmosdb.md)
+ [Indexador do Armazenamento de Blobs do Azure](search-howto-indexing-azure-blob-storage.md)
+ [Indexador do Armazenamento de Tabelas do Azure](search-howto-indexing-azure-tables.md)
+ [Segurança no Azure Search](search-security-overview.md)