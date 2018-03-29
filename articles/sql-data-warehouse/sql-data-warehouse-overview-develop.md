---
title: Recursos para desenvolver um armazém de dados no Azure | Microsoft Docs
description: Conceitos de desenvolvimento, as decisões de conceção, recomendações e técnicas de programação para o SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: ''
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: develop
ms.date: 03/22/2018
ms.author: jrj;barbkess
ms.openlocfilehash: c81a6f9af556a86f10d2482333fdc98f9cfb1454
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>As decisões de conceção e técnicas de programação para o SQL Data Warehouse
Veja através destes artigos de desenvolvimento para compreender melhor as decisões de conceção importantes, recomendações e técnicas de programação para o SQL Data Warehouse.

## <a name="key-design-decisions"></a>Decisões de conceção de chave
Os artigos seguintes realce conceitos e as decisões de conceção para desenvolver um armazém de dados distribuídos através do armazém de dados do SQL Server:

* [connections][connections]
* [concurrency][concurrency]
* [transactions][transactions]
* [Esquemas definido pelo utilizador][user-defined schemas]
* [distribuição de tabela][table distribution]
* [índices da tabela][table indexes]
* [partições da tabela][table partitions]
* [CTAS][CTAS]
* [statistics][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Recomendações de desenvolvimento e técnicas de programação
Estes artigos realce técnicas de programação específicas, sugestões e as recomendações para desenvolver o seu armazém de dados do SQL Server:

* [Procedimentos armazenados][stored procedures]
* [labels][labels]
* [views][views]
* [tabelas temporárias][temporary tables]
* [dynamic SQL][dynamic SQL]
* [looping][looping]
* [Agrupar por opções][group by options]
* [Atribuição de variáveis][variable assignment]

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações de referência, consulte [instruções T-SQL do armazém de dados SQL](sql-data-warehouse-reference-tsql-statements.md).

<!--Image references-->

<!--Article references-->
[concurrency]: ./resource-classes-for-workload-management.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md


<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
