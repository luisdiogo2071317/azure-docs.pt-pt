---
title: Utilizar etiquetas para consultas de instrumento no SQL Data Warehouse | Documentos da Microsoft
description: Sugestões para utilizar etiquetas para consultas de instrumento no armazém de dados SQL do Azure para o desenvolvimento de soluções.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 5c60a7dc44471599834c4b1225b397c8e6eabbd5
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55456464"
---
# <a name="using-labels-to-instrument-queries-in-azure-sql-data-warehouse"></a>Utilizar etiquetas para consultas de instrumento no Azure SQL Data Warehouse
Sugestões para utilizar etiquetas para consultas de instrumento no armazém de dados SQL do Azure para o desenvolvimento de soluções.


## <a name="what-are-labels"></a>Quais são as etiquetas?
SQL Data Warehouse suporta um conceito chamado etiquetas de consulta. Antes de entrar em qualquer profundidade, vamos examinar um exemplo:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

A última linha as etiquetas a cadeia de caracteres 'Minha consulta de etiqueta' para a consulta. Esta etiqueta é particularmente útil, uma vez que a etiqueta é consulta ajustável através de DMVs. Consulta para procurar etiquetas de fornece um mecanismo para a localização de consultas de problemas e ajudar a identificar o progresso por meio de um ELT executar.

Uma boa Convenção de nomenclatura realmente ajuda. Por exemplo, a partir da etiqueta do PROJETO, comentário, instrução ou procedimento ajuda a identificar exclusivamente a consulta entre todo o código no controle de origem.

A consulta seguinte utiliza uma vista de gestão dinâmica para pesquisar por etiqueta.

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> É essencial para colocar os colchetes ou entre aspas duplas a etiqueta do word ao consultar. É uma palavra reservada e causa um erro quando não é delimitado por etiqueta. 
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Para obter mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).


