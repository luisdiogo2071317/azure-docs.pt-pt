---
title: Com o SQL dinâmico no Azure SQL Data Warehouse | Microsoft Docs
description: Sugestões de utilização do SQL Server dinâmico no Azure SQL Data Warehouse para desenvolver soluções.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 604074e0a645918f7033360b79a1b7cad050c9e4
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dinâmica SQL no SQL Data Warehouse
Sugestões de utilização do SQL Server dinâmico no Azure SQL Data Warehouse para desenvolver soluções.

## <a name="dynamic-sql-example"></a>Exemplo SQL dinâmico

Ao desenvolver código da aplicação para o SQL Data Warehouse, poderá ter de utilizar o sql dinâmica para o ajudar a fornecer soluções flexíveis, genéricas e modulares. Armazém de dados do SQL Server não suporta tipos de dados de BLOBs neste momento. Não suporta tipos de dados de blob pode limitar o tamanho do seu cadeias, uma vez que os tipos de dados de blob incluem tipos varchar e nvarchar (Max). Se tiver utilizado estes tipos no código da aplicação para criar cadeias de grandes dimensões, terá de quebrar o código de segmentos e utilizar a instrução EXEC como alternativa.

Um exemplo simples:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Se a cadeia é pequeno, pode utilizar [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) como normal.

> [!NOTE]
> Continuarão a estar sujeitos a todas as regras de validação de TSQL instruções executadas como SQL dinâmico.
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Para mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).

