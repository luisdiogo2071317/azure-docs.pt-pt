---
title: Consultar um índice (portal - Azure Search) | Microsoft Docs
description: Emita uma consulta de pesquisa no Explorador de Pesquisa do Portal do Azure.
manager: cgronlun
author: HeidiSteen
ms.service: search
ms.topic: quickstart
ms.date: 07/10/2017
ms.author: heidist
ms.openlocfilehash: f952587150fa2d49709bdce6cbc5fe1a7aea16b1
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="query-an-azure-search-index-using-search-explorer-in-the-azure-portal"></a>Consultar um índice do Azure Search com o Explorador de Procura no Portal do Azure
> [!div class="op_single_selector"]
> * [Descrição geral](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Este artigo mostra-lhe como consultar um índice do Azure Search com o **Explorador de Procura** no portal do Azure. Pode utilizar o Explorador de Procura para submeter cadeias de consulta simples ou de Lucene completas a um índice existente no seu serviço.

## <a name="open-the-service-dashboard"></a>Abrir o dashboard de serviço
1. Clique em **Todos os recursos** na barra de atalhos no lado esquerdo do [portal do Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Selecione o seu serviço do Azure Search.

## <a name="select-an-index"></a>Selecionar um índice

Selecione o índice que pretende pesquisar a partir do mosaico **Índices**.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Abrir o Explorador de Procura

Clique no mosaico Explorador de Procura para abrir gradualmente a barra de pesquisa e o painel de resultados.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Inicie a pesquisa

Ao utilizar o Explorador de Procura, pode especificar [parâmetros de consulta](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) para formular a consulta.

1. Em **Cadeia de consulta**, escreva uma consulta e, em seguida, prima **Pesquisar**. 

   A cadeia de consulta é analisada automaticamente para o URL de pedido adequado, para submeter um pedido HTTP na API REST da Azure Search.   
   
   Pode utilizar qualquer sintaxe de consulta simples ou de Lucene completa para criar o pedido. O caráter `*` é equivalente a uma pesquisa em branco ou não especificada, que devolve todos os documentos por nenhuma ordem específica.

2. Em **Resultados**, os resultados da consulta são apresentados no JSON não processado, idênticos ao payload devolvido num Corpo de Resposta HTTP ao emitir pedidos através de programação.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Passos seguintes

Os seguintes recursos fornecem informações de sintaxe de consulta adicionais e exemplos.

 + [Sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Sintaxe de consulta Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Exemplos de sintaxe de consulta Lucene](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [Sintaxe de expressão de Filtro OData](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 