---
title: Utilizar etiquetas para consultas de instrumento no SQL Data Warehouse | Microsoft Docs
description: Sugestões para utilizar etiquetas para consultas de instrumento no Azure SQL Data Warehouse para desenvolver soluções.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 48fe625573639c0ec98e672f02a35e4a9ae268e8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Utilizar etiquetas para consultas de instrumento no Azure SQL Data Warehouse
Sugestões para utilizar etiquetas para consultas de instrumento no Azure SQL Data Warehouse para desenvolver soluções.


## <a name="what-are-labels"></a>Quais são as etiquetas?
Armazém de dados do SQL Server suporta um conceito chamado etiquetas de consulta. Antes de avançar para qualquer profundidade, vamos ver um exemplo:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

A última linha de etiquetas a cadeia 'Os meus de consulta de etiqueta' para a consulta. Esta etiqueta é particularmente útil, dado que a etiqueta é consulta possível através de DMVs. Consultar as etiquetas fornece um mecanismo para localizar consultas de problema e ajudar a identificar progresso através de um ELT executar.

Uma convenção de nomenclatura boa realmente ajuda. Por exemplo, a partir da etiqueta do PROJETO, comentário, instrução ou procedimento ajuda a identificar exclusivamente a consulta entre todos os o código de controlo de origem.

A seguinte consulta utiliza uma vista de gestão dinâmica para procurar por etiqueta.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> É essencial para colocar entre parênteses Retos ou aspas à volta a etiqueta do word, ao consultar. Etiqueta é uma palavra reservada e causa um erro quando não está a ser delimitado. 
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Para mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).


