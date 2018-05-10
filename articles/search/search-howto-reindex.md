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
ms.openlocfilehash: 006d04efb0a6bebc424cb005bf63af2b3cd7a42e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Como reconstruir um índice da Azure Search

Reconstruir um alterações de índice a estrutura de alterar a expressão física do índice no seu serviço da Azure Search. Por outro lado, a atualizar um índice é uma atualização só de conteúdo para recolher as alterações mais recentes a partir de uma origem de dados externas coadjuvantes. Este artigo fornece direção sobre como atualizar índices estruturalmente e substantively.

Permissões de leitura e escrita ao nível do serviço são necessárias para as atualizações de índice. Programaticamente, pode chamar REST ou .NET APIs para uma reconstrução completa ou incrementais a indexação de conteúdo, com parâmetros especificar opções de atualização. 

Geralmente, as atualizações para um índice estão a pedido. No entanto, para índices povoados utilizando específica de origem [indexadores](search-indexer-overview.md), pode utilizar um agendador incorporado. O Programador de suporta a atualização do documento, no máximo, 15 em 15 minutos, até qualquer intervalo e precisa de padrão. Uma taxa de atualização mais rápida requer enviar índice atualizações manualmente, talvez através de uma valor de duplo-escrita em transações, ao atualizar a origem de dados externo e o índice da Azure Search em simultâneo.

## <a name="full-rebuilds"></a>Recria completa

Para muitos tipos de atualizações, é necessária uma reconstrução completa. Uma reconstrução completa refere-se a eliminação de um índice, dados e metadados, seguido de repopulating o índice de origens de dados externas. Através de programação, [eliminar](https://docs.microsoft.com/rest/api/searchservice/delete-index), [criar](https://docs.microsoft.com/rest/api/searchservice/create-index), e [recarregar](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) o índice de reconstrui-lo. 

Após reconstruir, lembre-se de que se tiver sido a testar padrões de consulta e perfis de classificação, que pode esperar variação nos resultados da consulta se o conteúdo subjacente foi alterada.

## <a name="when-to-rebuild"></a>Quando reconstruir

Planeia frequentes, completa Reconstrói durante o desenvolvimento de Active Directory, quando esquemas do índice estão num Estado de flux.

| Modificação | Estado de reconstrução|
|--------------|---------------|
| Alterar um nome de campo, o tipo de dados, ou o respetivo [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index) | Alterar uma definição de campo normalmente implica uma reconstrução penalidade, à exceção estes [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index): recuperável, SearchAnalyzer, SynonymMaps. Pode adicionar os atributos recuperável, SearchAnalyzer e SynonymMaps a um campo existente sem ter de reconstruir o índice.|
| Adicionar um campo | Sem requisito de strict no reconstrução. Documentos indexados existentes recebem um valor nulo para o novo campo. Num reindex futura, valores de dados de origem são adicionados a documentos. |
| Eliminar um campo | Sem requisito de strict no reconstrução. Não é utilizado um campo eliminado, mas fisicamente a definição de campo e os conteúdos permanecem no índice até que a reconstrução seguinte. |

> [!Note]
> Uma reconstrução também é necessária se mudar camadas. Se a determinada altura que opte por utilizar mais capacidade, não há nenhuma atualização no local. Tem de ser criado um novo serviço no ponto de nova capacidade e índices tem de ser criados a partir do zero no novo serviço. 

## <a name="partial-or-incremental-indexing"></a>A indexação parcial ou incrementais

Depois de um índice estiver em produção, concentre-se de que altera para incremental indexação, normalmente com sem interrupções ao serviço discernable. A indexação parcial ou incrementais carga de trabalho apenas para conteúdo que sincroniza o conteúdo de um índice de pesquisa para refletir o estado do conteúdo de uma origem de dados coadjuvantes é. Um documento de adicionado ou eliminado na origem é adicionado ou eliminado para o índice. No código, chame o [adicionar, atualizar ou eliminar documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) operação ou o equivalente ao .NET.

> [!Note]
> Quando utilizar indexadores que pesquisam origens de dados externas, mecanismos de controlo de alterações em sistemas de origem são maximizados para indexação incremental. Para [Blob storage do Azure](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), um `lastModified` campo é utilizado. No [Table storage do Azure](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` tiver a mesma finalidade. Da mesma forma, ambos [indexador SQL Database do Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) e [indexador de Azure Cosmos DB](search-howto-index-cosmosdb.md#indexing-changed-documents) ter campos para sinalizar atualizações de linha. Para obter mais informações sobre indexadores, consulte [descrição geral do indexador](search-indexer-overview.md).

## <a name="scale-out-indexing"></a>A indexação de escalamento horizontal

Como volumes de dados aumentam ou processamento tem de alteração, poderá achar que simples Reconstrói e tarefas reindexing não são suficientes. Como primeiro passo para a aumentos dos pedidos de reunião, recomendamos que aumente o [escala e a capacidade](search-capacity-planning.md) dentro dos limites do seu serviço existente. 

Se pode utilizar [indexadores](search-indexer-overview.md), mecanismos de escalamento horizontal adicionais fiquem disponíveis. Indexadores vêm com um agendador incorporado que lhe permite parcel saída indexação em intervalos regulares ou expandem o processamento fora da janela de 24 horas. Além disso, quando emparelhado com definições de origem de dados, indexadores permitem-lhe obter uma forma de paralelismo a criação de partições de dados e utilizando agendas para executar em paralelo.

### <a name="scheduled-indexing-for-large-data-sets"></a>Agendar a indexação para grandes conjuntos de dados

Agendamento é um mecanismo importante para o processamento de grandes conjuntos de dados e estatísticas de execução lenta, como a análise de imagem num pipeline pesquisa cognitivos. Processamento de indexador funciona numa janela de 24 horas. Se não for concluída dentro de 24 horas de processamento, os comportamentos de agendamento do indexador podem trabalhar em sua partido. 

Por predefinição, agendada indexação começa em intervalos específicos, com uma tarefa normalmente concluir antes da retomar a num intervalo agendado seguinte. No entanto, se o processamento não for concluída dentro do intervalo, o indexador deixa (porque foi esgotada tempo). No próximo intervalo, retoma do processamento onde pela última vez foi deixada, com a manutenção do sistema controlar de onde que ocorre. 

Em termos práticos, para cargas de índice expansão de vários dias, pode colocar o indexador com base numa agenda de 24 horas. Quando a indexação retoma para o seguinte stint de 24 horas, reinicia na última documento boa conhecido. Desta forma, um indexador pode trabalhar a forma através de um registo de segurança de documentos através de uma série de dias até que todos os documentos não processados são processados. Para obter mais informações sobre esta abordagem, consulte [indexação grandes conjuntos de dados](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)

<a name="parallel-indexing"></a>

### <a name="parallel-indexing"></a>A indexação paralela

Uma segunda opção consiste em definir um estratégia de indexação de paralelo. Para não-rotina, viáveis que consomem muita indexação requisitos, tais como OCR em documentos digitalizados num pipeline pesquisa cognitivos, um estratégia de indexação de paralelo poderá ser a abordagem de direita para esse objetivo específico. Num pipeline sem causa pesquisa cognitivos, análise de imagem e processamento de linguagem natural são execução longos. A indexação Parallel num serviço que não está em simultâneo a processar os pedidos de consulta pode ser uma opção viável para trabalhar com um grande corpo de conteúdo de processamento lenta. 

Uma estratégia para processamento paralelo tem estes elementos:

+ Divida a origem de dados entre vários contentores ou várias pastas virtuais no interior do contentor do mesmo. 
+ Mapear cada mini conjunto de dados para um [origem data](https://docs.microsoft.com/rest/api/searchservice/create-data-source), emparelhada para os seus próprios [indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer).
+ Para efetuar uma pesquisa cognitivos, referenciar a mesma [skillset](ref-create-skillset.md) em cada definição do indexador.
+ Escreva para o mesmo índice de pesquisa de destino. 
+ Agende a todos os indexadores para executar em simultâneo.

> [!Note]
> A pesquisa do Azure não suporta a dedicação de réplicas ou partições para cargas de trabalho específicas. O risco de indexação simultâneas pesada é overburdening sistema para o detrimento de desempenho de consulta. Se tiver um ambiente de teste, implemente paralelas indexação existe primeiro compreender fala.

### <a name="configure-parallel-indexing"></a>Configurar a indexação paralela

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