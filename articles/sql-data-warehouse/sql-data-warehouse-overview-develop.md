---
title: Recursos para desenvolver um armazém de dados no Azure | Microsoft Docs
description: Conceitos de desenvolvimento, as decisões de conceção, recomendações e técnicas de programação para o SQL Data Warehouse.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: d9a272b2f43e080cd44b7179fe6f9dc55507142b
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/19/2018
ms.locfileid: "31601809"
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>As decisões de conceção e técnicas de programação para o SQL Data Warehouse
Veja através destes artigos de desenvolvimento para compreender melhor as decisões de conceção importantes, recomendações e técnicas de programação para o SQL Data Warehouse.

## <a name="key-design-decisions"></a>Decisões de conceção de chave
Os artigos seguintes realce conceitos e as decisões de conceção para desenvolver um armazém de dados distribuídos através do armazém de dados do SQL Server:

* [Ligações][connections]
* [Simultaneidade][concurrency]
* [Transações][transactions]
* [Esquemas definido pelo utilizador][user-defined schemas]
* [distribuição de tabela][table distribution]
* [índices da tabela][table indexes]
* [partições da tabela][table partitions]
* [CTAS][CTAS]
* [Estatísticas][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Recomendações de desenvolvimento e técnicas de programação
Estes artigos realce técnicas de programação específicas, sugestões e as recomendações para desenvolver o seu armazém de dados do SQL Server:

* [Procedimentos armazenados][stored procedures]
* [labels][labels]
* [vistas][views]
* [tabelas temporárias][temporary tables]
* [SQL dinâmico][dynamic SQL]
* [ciclo][looping]
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
