---
title: Monitorização do Azure SQL Database Using Dynamic Management Views | Documentos da Microsoft
description: Saiba como detetar e diagnosticar problemas de desempenho comuns ao utilizar vistas de gestão dinâmica para monitorizar a base de dados do Microsoft Azure SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: carlrab
ms.openlocfilehash: c4d1170bd2fe4acb135c88191b447f734e312723
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715962"
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Monitorização da Base de Dados SQL do Azure utilizando vistas de gestão dinâmica
Base de dados do Microsoft Azure SQL permite que um subconjunto das exibições de gerenciamento dinâmico para diagnosticar problemas de desempenho, que podem ser causados por consultas bloqueadas ou de execução longa, afunilamentos de recursos, planos de consulta fraco e assim por diante. Este tópico fornece informações sobre como detectar problemas de desempenho comuns ao utilizar vistas de gestão dinâmica.

Base de dados SQL parcialmente oferece suporte a três categorias de vistas de gestão dinâmica:

* Vistas de gestão dinâmica relacionadas com a base de dados.
* Vistas de gestão dinâmica relacionadas com a execução.
* Vistas de gestão dinâmica relacionadas com a transação.

Para obter informações detalhadas sobre vistas de gestão dinâmica, consulte [exibições de gerenciamento dinâmico e as funções (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) nos manuais Online do SQL Server.

## <a name="permissions"></a>Permissões
Na base de dados SQL, a consulta de uma vista de gestão dinâmica necessita **estado de banco de dados de EXIBIÇÃO** permissões. O **estado de banco de dados de EXIBIÇÃO** permissão devolve informações sobre todos os objetos na base de dados atual.
Para conceder a **estado de banco de dados de EXIBIÇÃO** permissão a um utilizador de base de dados específica, execute a seguinte consulta:

```GRANT VIEW DATABASE STATE TO database_user; ```

Numa instância do SQL Server no local, vistas de gestão dinâmica retornam informações de estado do servidor. Na base de dados SQL, o que elas retornam informações relativas à sua atual base de dados lógica apenas.

## <a name="calculating-database-size"></a>Calcular o tamanho de base de dados
A seguinte consulta devolve o tamanho da base de dados (em megabytes):

```
-- Calculates the size of the database.
SELECT SUM(CAST(FILEPROPERTY(name, 'SpaceUsed') AS bigint) * 8192.) / 1024 / 1024 AS DatabaseSizeInMB
FROM sys.database_files
WHERE type_desc = 'ROWS';
GO
```

A seguinte consulta devolve o tamanho dos objetos individuais (em megabytes) na base de dados:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Ligações de monitorização
Pode utilizar o [sys](https://msdn.microsoft.com/library/ms181509.aspx) para obter informações sobre as ligações estabelecidas para um servidor de base de dados do Azure SQL específico e os detalhes de cada ligação. Além disso, o [exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) vista é útil quando a recuperação das informações sobre todas as ligações de utilizador do Active Directory e tarefas internas.
A consulta seguinte obtém informações sobre a ligação atual:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> Ao executar o **sys.dm_exec_requests** e **vistas exec_sessions**, se tiver **estado da base de dados de EXIBIÇÃO** permissão na base de dados, verá a execução de todos os sessões na base de dados; caso contrário, verá apenas na sessão atual.
> 
> 

## <a name="monitoring-query-performance"></a>Monitorização do desempenho de consulta
Lento ou muito tempo a execução de consultas pode consumir recursos significativos do sistema. Esta secção demonstra como usar exibições de gerenciamento dinâmico para detectar alguns problemas de desempenho de consulta comuns. É uma referência mais antiga, mas ainda útil para resolução de problemas, o [resolver problemas de desempenho no SQL Server 2008](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) artigo no Microsoft TechNet.

### <a name="finding-top-n-queries"></a>Localizar consultas principais N
O exemplo seguinte devolve informações sobre as cinco principais consultas classificadas por tempo médio de CPU. Neste exemplo agrega as consultas de acordo com o hash de consulta, para que as consultas logicamente equivalentes são agrupadas pelo respetivo consumo cumulativo de recursos.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Monitorização de consultas bloqueadas
Consultas de longa ou lentas podem contribuir para o consumo excessivo de recursos e ser a conseqüência de consultas bloqueadas. A causa do bloqueio pode ser o design da aplicação insatisfatório, planos de consulta incorreta, a falta de índices úteis e assim por diante. Pode utilizar a vista de sys.dm_tran_locks para obter informações sobre a atividade atual de bloqueio na base de dados SQL do Azure. Por exemplo de código, consulte [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) nos manuais Online do SQL Server.

### <a name="monitoring-query-plans"></a>Monitorização de planos de consulta
Um plano de consulta ineficientes também pode aumentar o consumo da CPU. O exemplo seguinte utiliza a [DM exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) vista para determinar que consulta usa a CPU mais cumulativa.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Consulte também
[Introdução à base de dados SQL](sql-database-technical-overview.md)

