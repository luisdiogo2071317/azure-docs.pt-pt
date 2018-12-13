---
title: Explorador de pesquisa no portal do Azure para consultar índices - Azure Search
description: Utilize ferramentas do portal do Azure, como o Explorador de pesquisa para índices de consulta no Azure Search. Introduza os termos de pesquisa ou cadeias de procura completamente qualificado com a sintaxe avançada.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 11f102fcb2a24f9062313f9a3234c29e70a3dfe0
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315674"
---
# <a name="how-to-use-search-explorer-to-query-indexes-in-azure-search"></a>Como utilizar o Explorador de pesquisa para índices de consulta no Azure Search 

Este artigo mostra-lhe como consultar um através de índice de pesquisa do Azure existentes **Explorador de pesquisa** no portal do Azure. Pode utilizar o Explorador de procura para submeter simples ou completas cadeias de consulta de Lucene a um índice existente no seu serviço.

## <a name="open-the-service-dashboard"></a>Abrir o dashboard de serviço
1. Clique em **Todos os recursos** na barra de atalhos no lado esquerdo do [portal do Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Selecione o seu serviço do Azure Search.

## <a name="select-an-index"></a>Selecionar um índice

Selecione o índice que pretende pesquisar a partir do mosaico **Índices**.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Abra o Explorador de pesquisa

Clique no mosaico do Explorador de pesquisa para abrir gradualmente a barra de pesquisa e o painel de resultados.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Inicie a pesquisa

Ao utilizar o Explorador de pesquisa, pode especificar [parâmetros de consulta](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) para formular a consulta.

1. Em **Cadeia de consulta**, escreva uma consulta e, em seguida, prima **Pesquisar**. 

   A cadeia de consulta é analisada automaticamente para o URL de pedido adequado, para submeter um pedido HTTP na API REST da Azure Search.   
   
   Pode utilizar qualquer sintaxe de consulta simples ou de Lucene completa para criar o pedido. O caráter `*` é equivalente a uma pesquisa em branco ou não especificada, que devolve todos os documentos por nenhuma ordem específica.

2. Em **Resultados**, os resultados da consulta são apresentados no JSON não processado, idênticos ao payload devolvido num Corpo de Resposta HTTP ao emitir pedidos através de programação.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Passos Seguintes

Os seguintes recursos fornecem informações de sintaxe de consulta adicionais e exemplos.

 + [Sintaxe de consulta simples](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Sintaxe de consulta Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Exemplos de sintaxe de consulta Lucene](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [Sintaxe de expressão de Filtro OData](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) 