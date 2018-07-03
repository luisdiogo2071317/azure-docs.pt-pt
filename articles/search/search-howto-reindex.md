---
title: Reconstruir um conteúdo do pesquisável como índice ou a atualização da Azure Search | Documentos da Microsoft
description: Adicionar novos elementos, atualizar elementos existentes ou documentos ou eliminar documentos obsoletos numa recriação completa ou a indexação incremental parcial para atualizar o índice da Azure Search.
services: search
author: HeidiSteen
manager: cgronlun
ms.service: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: heidist
ms.openlocfilehash: 374e7601169647f0eb7d3a214cf15567b7b11090
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "34641429"
---
# <a name="how-to-rebuild-an-azure-search-index"></a>Como reconstruir um índice da Azure Search

Reconstruir um alterações de índice sua estrutura, alterar a expressão física do índice no seu serviço de Azure Search. Por outro lado, a atualizar um índice é uma atualização só de conteúdo para recolher as alterações mais recentes a partir de uma origem de dados externas colaborador. Este artigo fornece uma direção sobre como atualizar índices estruturalmente e substantively.

Permissões de leitura / escrita no nível de serviço são necessárias para atualizações de índices. Por meio de programação, pode chamar REST ou de APIs do .NET para uma reconstrução completa ou a indexação incremental de conteúdo, com parâmetros que especificar opções de atualização. 

Em geral, as atualizações para um índice são a pedido. No entanto, para índices utilizando específicos da fonte [indexadores](search-indexer-overview.md), pode usar um agendador interno. O agendador suporta a atualização de documentos com a frequência de 15 minutos, até qualquer intervalo e o padrão de que necessita. Uma taxa de atualização mais rápida requer o envio de índice atualizações manualmente, talvez por meio de uma gravação de double em transações, a atualizar a origem de dados externo e o índice da Azure Search em simultâneo.

## <a name="full-rebuilds"></a>Recompilações completas

Para muitos tipos de atualizações, uma reconstrução completa é necessária. Uma reconstrução completa refere-se até à eliminação de um índice, dados e metadados, seguido de preencher novamente o índice a partir de origens de dados externas. Programaticamente, [elimine](https://docs.microsoft.com/rest/api/searchservice/delete-index), [crie](https://docs.microsoft.com/rest/api/searchservice/create-index), e [recarregar](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) o índice reconstrui-lo. 

Pós-reconstrua, lembre-se de que se tem estado a testar padrões de consulta e perfis de classificação, pode esperar variação nos resultados da consulta se o conteúdo subjacente tiver sido alterado.

## <a name="when-to-rebuild"></a>Quando deve recriar

Tencione recompilações freqüentes e completas durante o desenvolvimento do Active Directory, quando os esquemas de índice estão num estado recém-implantados.

| Modificação | Recriação do Estado|
|--------------|---------------|
| Alterar um nome de campo, tipo de dados, ou seu [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index) | Alterar uma definição de campo, normalmente, implica uma penalidade de reconstrução, com exceção estes [atributos de índice](https://docs.microsoft.com/rest/api/searchservice/create-index): recuperável, SearchAnalyzer, SynonymMaps. Pode adicionar os atributos recuperável, SearchAnalyzer e SynonymMaps para um campo existente sem ter de recriar o seu índice.|
| Adicionar um campo | Sem requisito rígido na reconstrução. Documentos indexados existentes tem um valor nulo para o novo campo. Numa futura reindex, os valores dos dados de origem substituem nulos adicionados pelo Azure Search. |
| Eliminar um campo | Diretamente não é possível eliminar um campo de índice da Azure Search. Em vez disso, deve ter o seu aplicativo ignorar o campo "eliminado" para evitar usá-lo. Fisicamente, a definição de campo e o conteúdo mantêm o índice até da próxima vez que reconstrua o índice utilizando um esquema que omite o campo em questão.|

> [!Note]
> Uma reconstrução também é necessária se alternar escalões. Se em algum momento optar por mais capacidade, não existe nenhuma atualização no local. Tem de ser criado um novo serviço no ponto de nova capacidade e índices devem ser criados a partir do zero no novo serviço. 

## <a name="partial-or-incremental-indexing"></a>Parcial ou incrementais de indexação

Depois de um índice em produção, o foco muda para indexação incremental, normalmente com sem interrupções de serviço discernable. A indexação incremental parcial ou de carga de trabalho apenas o conteúdo que sincroniza o conteúdo de um índice de pesquisa para refletir o estado do conteúdo numa origem de dados colaborador é. Um documento adicionada ou excluída na origem é adicionado ou eliminado para o índice. No código, chamar o [adicionar, atualizar ou eliminar documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents) operação ou o equivalente do .NET.

> [!Note]
> Quando utilizar indexadores que pesquisam origens de dados externas, mecanismos de controle de alterações em sistemas de origem são aproveitados para indexação incremental. Para [armazenamento de Blobs do Azure](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection), um `lastModified` campo é utilizado. No [armazenamento de tabelas do Azure](search-howto-indexing-azure-tables.md#incremental-indexing-and-deletion-detection), `timestamp` tem a mesma finalidade. Da mesma forma, ambos [indexador do Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#capture-new-changed-and-deleted-rows) e [indexador do Azure Cosmos DB](search-howto-index-cosmosdb.md#indexing-changed-documents) ter campos para sinalizando atualizações de linha. Para obter mais informações sobre indexadores, consulte [descrição geral do indexador](search-indexer-overview.md).


## <a name="see-also"></a>Consulte também

+ [Descrição geral do Indexador](search-indexer-overview.md)
+ [Índice grandes conjuntos de dados em escala](search-howto-large-index.md)
+ [Indexação no portal](search-import-data-portal.md)
+ [Indexador da base de dados SQL do Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
+ [Indexador da BD do Cosmos para o Azure](search-howto-index-cosmosdb.md)
+ [Indexador do Armazenamento de Blobs do Azure](search-howto-indexing-azure-blob-storage.md)
+ [Indexador do Armazenamento de Tabelas do Azure](search-howto-indexing-azure-tables.md)
+ [Segurança no Azure Search](search-security-overview.md)