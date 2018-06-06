---
title: Notas de versão de armazém de dados SQL do Azure, Abril de 2018 | Microsoft Docs
description: Notas de versão para o Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/28/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 49ffc3ddfd586964ae8a9688aeb48fffdd327b45
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738938"
---
# <a name="whats-new-in-azure-sql-data-warehouse-april-2018"></a>Novidades no Azure SQL Data Warehouse (Abril de 2018)?
O Azure SQL Data Warehouse recebe melhoramentos continuamente. Este artigo descreve as novas funcionalidades e as alterações que foram introduzidas no Abril de 2018.

## <a name="features"></a>Funcionalidades

### <a name="ability-to-truncate-a-partition-before-a-switch"></a>Capacidade para truncar uma partição antes de um comutador
Os clientes utilizam, frequentemente, como um padrão da mudança de partições para carregar dados de uma tabela para outro, alterando os metadados da tabela através de `ALTER TABLE SourceTable SWITCH PARTITION X TO TargetTable PARTITION X` sintaxe. Armazém de dados do SQL Server não suporta a mudança de partições quando a partição de destino contém dados. Se a partição de destino já contiver dados, o cliente iria precisar truncar a partição de destino e, em seguida, executar o comutador.

Armazém de dados do SQL Server suporta agora a esta operação numa única instrução T-SQL.

```sql
ALTER TABLE SourceTable 
    SWITCH PARTITION X TO TargetTable PARTITION X
    WITH (TRUNCATE_TARGET_PARTITION = ON)
```
Para obter mais informações, consulte o [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) artigo.

### <a name="improved-query-compilation-performance"></a>Desempenho de compilação da consulta melhorada
O SQL Data Warehouse introduziu um conjunto de alterações para melhorar o passo de compilação da consulta de consultas distribuídas. Estas alterações melhorar os tempos de compilação da consulta até **10x** globais reduzindo consultar tempos de execução de execução. Estas alterações são mais evidentes no armazéns de dados com um grande número de objetos (tabelas, funções, vistas, procedimentos).

## <a name="behavior-changes"></a>Alterações de comportamento

### <a name="dbcc-commands-do-not-consume-concurrency-slots"></a>Comandos DBCC não consumir ranhuras de concorrência
Armazém de dados do SQL Server suporta um subconjunto de T-SQL [comandos DBCC](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) como [DBCC DROPCLEANBUFFERS](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql). Anteriormente, seriam consumir estes comandos um [ranhura de concorrência](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#concurrency-slots) reduzir o número de carregamentos/consultas do utilizador que pode ser executado. O `DBCC` comandos agora são executados numa fila local que não a consumir uma ranhura de recurso melhorar o desempenho de execução de consulta global.

### <a name="updated-error-message-for-excessive-literals"></a>Mensagem de erro atualizado para literais excessivos
Anteriormente, o SQL Data Warehouse inclui um *aproximado* contagem quando uma consulta continha demasiados literais.
```
Msg 100086
Cannot have more than 20,000 literals in the query. The query contains [n] literals.
```

A mensagem de erro foi atualizada para indicar apenas atingiu o limite literal.
```
Msg 100086
The number of literals in the query is beyond the limit. Please rewrite your query.
```

Para obter mais informações, consulte o [consultas](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#queries) secção o [os limites de capacidade](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits) artigo para obter detalhes adicionais sobre os limites máximos.

### <a name="removed-the-syspdwdatabasemappings-view"></a>Remover o SYS. Vista PDW_DATABASE_MAPPINGS
Isto `sys.pdw_database_mappings` vista é utilizada no armazém de dados do SQL Server. Anteriormente, um SELECIONE desta vista seria não devolver resultados. A vista foi removida. 