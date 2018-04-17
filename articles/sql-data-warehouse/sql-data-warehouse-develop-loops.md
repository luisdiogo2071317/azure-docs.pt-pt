---
title: Utilizar ciclos de T-SQL no Azure SQL Data Warehouse | Microsoft Docs
description: Sugestões para utilizar ciclos de T-SQL e de substituir cursores no Azure SQL Data Warehouse para desenvolver soluções.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: ''
ms.assetid: f3384b81-b943-431b-bc73-90e47e4c195f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 2e49a0de0e4a6aba6639f7f3100f41c8db254220
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="using-t-sql-loops-in-sql-data-warehouse"></a>Utilizar ciclos de T-SQL no SQL Data Warehouse
Sugestões para utilizar ciclos de T-SQL e de substituir cursores no Azure SQL Data Warehouse para desenvolver soluções.

## <a name="purpose-of-while-loops"></a>Objetivo ao ciclos

Armazém de dados do SQL Server suporta o [enquanto](/sql/t-sql/language-elements/while-transact-sql) ciclo para executar repetidamente blocos de instrução. Este ciclo de tempo continua para, desde que as condições especificadas são true, ou até que o código especificamente termina o ciclo utilizando a quebra de palavra-chave. Ciclos são úteis para substituir os cursores definidos no código do SQL Server. Felizmente, quase todos os cursores que são escritos no código do SQL Server têm a variedade de reencaminhar rápido só de leitura. Por conseguinte, [enquanto] ciclos são uma excelente alternativa para substituir os cursores.

## <a name="replacing-cursors-in-sql-data-warehouse"></a>Substituir os cursores no SQL Data Warehouse
No entanto, antes de diving no cabeçalho pela primeira vez deve faça a mesmo à seguinte pergunta: "foi deste cursor ser rescrito para utilizar operações com base em conjunto?." Em muitos casos, a resposta é Sim e é, muitas vezes, a abordagem das melhores. Uma operação baseada em conjunto, muitas vezes, efetua mais rapidamente do que uma abordagem iterativa, linha por linha.

Cursores só de leitura rápida reencaminhar podem ser facilmente substituídos com uma construção de ciclo. Segue-se um exemplo simples. Este exemplo de código atualiza as estatísticas de cada tabela na base de dados. Por iterating através de tabelas do ciclo, cada comando executa na sequência.

Em primeiro lugar, crie uma tabela temporária contendo um número de linha exclusivo utilizado para identificar as declarações de individuais:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Segundo, inicializar as variáveis necessárias para executar o ciclo:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Agora cíclicas através de instruções executar um cada vez:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Por fim, remover a tabela temporária criada no primeiro passo

```
DROP TABLE #tbl;
```

## <a name="next-steps"></a>Passos Seguintes
Para mais sugestões de desenvolvimento, consulte [descrição geral do desenvolvimento](sql-data-warehouse-overview-develop.md).

