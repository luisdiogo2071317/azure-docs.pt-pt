---
title: Analisar a sua carga de trabalho - Azure SQL Data Warehouse | Documentos da Microsoft
description: Técnicas de análise de priorização de consulta para a sua carga de trabalho no Azure SQL Data Warehouse.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 9025eccabcbf7052131fee741a1e1f6a2139366b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/31/2019
ms.locfileid: "55476762"
---
# <a name="analyze-your-workload-in-azure-sql-data-warehouse"></a>Analisar a sua carga de trabalho no Azure SQL Data Warehouse
Técnicas de análise de priorização de consulta para a sua carga de trabalho no Azure SQL Data Warehouse.

## <a name="workload-groups"></a>Grupos de carga de trabalho 
O SQL Data Warehouse implementa classes de recursos utilizando grupos de carga de trabalho. Há um total de oito grupos de carga de trabalho que controlam o comportamento das classes de recursos entre vários tamanhos DWU. Para qualquer DWU, o SQL Data Warehouse utiliza apenas quatro dos grupos de carga de trabalho de oito. Essa abordagem faz sentido, porque cada grupo de carga de trabalho está atribuído a um dos quatro classes de recursos: smallrc mediumrc, largerc, ou xlargerc. A importância de compreender os grupos de carga de trabalho é que alguns destes grupos de carga de trabalho estão definidas para mais *importância*. Importância é utilizada para a CPU de agendamento. Três vezes mais ciclos de CPU que as consultas executados com importância Média de obtenção de consultas executadas com importância alta. Por conseguinte, os mapeamentos de ranhura de simultaneidade também determinam prioridade de CPU. Quando uma consulta consome os blocos de 16 ou mais, ele é executado como tendo importância elevada.

A tabela seguinte mostra os mapeamentos de importância para cada grupo de carga de trabalho.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Mapeamentos de grupo de carga de trabalho para blocos de simultaneidade e importância

| Grupos de carga de trabalho | Mapeamento de ranhura de simultaneidade | MB / distribuição (elasticidade) | MB / distribuição (computação) | Mapeamento de importância |
|:---------------:|:------------------------:|:------------------------------:|:---------------------------:|:------------------:|
| SloDWGroupC00   | 1                        |    100                         | 250                         | Médio             |
| SloDWGroupC01   | 2                        |    200                         | 500                         | Médio             |
| SloDWGroupC02   | 4                        |    400                         | 1000                        | Médio             |
| SloDWGroupC03   | 8                        |    800                         | 2000                        | Médio             |
| SloDWGroupC04   | 16                       |  1,600                         | 4000                        | Elevado               |
| SloDWGroupC05   | 32                       |  3,200                         | 8000                        | Elevado               |
| SloDWGroupC06   | 64                       |  6,400                         | 16,000                      | Elevado               |
| SloDWGroupC07   | 128                      | 12,800                         | 32,000                      | Elevado               |
| SloDWGroupC08   | 256                      | 25,600                         | 64,000                      | Elevado               |

<!-- where are the allocation and consumption of concurrency slots charts? --> O **alocação e o consumo de ranhuras de simultaneidade** gráfico mostra um DW500 usa 1, 4, 8 ou blocos de simultaneidade 16 para smallrc, mediumrc, largerc e xlargerc, respectivamente. Para localizar a importância de cada classe de recursos, pode consultar esses valores no gráfico anterior.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Mapeamento de DW500 das classes de recursos para importância
| Classe de recursos | Grupo de carga de trabalho | Ranhuras de simultaneidade utilizadas | MB / distribuição | Importância |
|:-------------- |:-------------- |:----------------------:|:-----------------:|:---------- |
| smallrc        | SloDWGroupC00  | 1                      | 100               | Médio     |
| mediumrc       | SloDWGroupC02  | 4                      | 400               | Médio     |
| largerc        | SloDWGroupC03  | 8                      | 800               | Médio     |
| xlargerc       | SloDWGroupC04  | 16                     | 1,600             | Elevado       |
| staticrc10     | SloDWGroupC00  | 1                      | 100               | Médio     |
| staticrc20     | SloDWGroupC01  | 2                      | 200               | Médio     |
| staticrc30     | SloDWGroupC02  | 4                      | 400               | Médio     |
| staticrc40     | SloDWGroupC03  | 8                      | 800               | Médio     |
| staticrc50     | SloDWGroupC03  | 16                     | 1,600             | Elevado       |
| staticrc60     | SloDWGroupC03  | 16                     | 1,600             | Elevado       |
| staticrc70     | SloDWGroupC03  | 16                     | 1,600             | Elevado       |
| staticrc80     | SloDWGroupC03  | 16                     | 1,600             | Elevado       |

## <a name="view-workload-groups"></a>Ver grupos de carga de trabalho
A seguinte consulta mostra detalhes sobre a alocação de recursos de memória da perspectiva do Governador de recursos. Isso é útil para análise da utilização de Active Directory e o histórico dos grupos de carga de trabalho quando a resolução de problemas.

```sql
WITH rg
AS
(   SELECT  
     pn.name                                AS node_name
    ,pn.[type]                              AS node_type
    ,pn.pdw_node_id                         AS node_id
    ,rp.name                                AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                                AS group_name
    ,wg.importance                          AS group_importance
    ,wg.request_max_memory_grant_percent    AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                             AS group_max_dop
    ,wg.effective_max_dop                   AS group_effective_max_dop
    ,wg.total_request_count                 AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
    AND     rp.name    = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
        node_name
,       group_request_max_memory_grant_pcnt
,       group_importance
;
```

## <a name="queued-query-detection-and-other-dmvs"></a>Deteção de consulta em fila e outros DMVs
Pode utilizar o `sys.dm_pdw_exec_requests` DMV para identificar consultas que estão a aguardar na fila de simultaneidade. Consultas a aguardar que um bloco de simultaneidade têm o estado **suspenso**.

```sql
SELECT  r.[request_id]                           AS Request_ID
,       r.[status]                               AS Request_Status
,       r.[submit_time]                          AS Request_SubmitTime
,       r.[start_time]                           AS Request_StartTime
,       DATEDIFF(ms,[submit_time],[start_time])  AS Request_InitiateDuration_ms
,       r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r
;
```

Funções de gestão da carga de trabalho podem ser vistas com `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0
;
```

A seguinte consulta mostra qual é atribuída a cada usuário para a função.

```sql
SELECT  r.name AS role_principal_name
,       m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id      = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc')
;
```

SQL Data Warehouse tem os seguintes tipos de espera:

* **LocalQueriesConcurrencyResourceType**: Consultas que ficam fora do framework da ranhura de simultaneidade. Consultas DMV e o sistema funciona como `SELECT @@VERSION` são exemplos de consultas locais.
* **UserConcurrencyResourceType**: Consultas que ficam dentro do framework da ranhura de simultaneidade. Consultas em tabelas de utilizador final representam exemplos que usaria este tipo de recurso.
* **DmsConcurrencyResourceType**: Esperas resultantes de operações de movimento de dados.
* **BackupConcurrencyResourceType**: Este wait indica que uma base de dados é a cópia de segurança. O valor máximo para este tipo de recurso é 1. Se várias cópias de segurança foram solicitadas ao mesmo tempo, os outros fila.

O `sys.dm_pdw_waits` DMV pode ser utilizado para ver quais os recursos que um pedido está a aguardar.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]                                           AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,       SESSION_ID()                                       AS Current_session
,       s.[status]                                         AS Session_status
,       s.[login_name]
,       s.[query_count]
,       s.[client_id]
,       s.[sql_spid]
,       r.[command]                                        AS Request_command
,       r.[label]
,       r.[status]                                         AS Request_status
,       r.[submit_time]
,       r.[start_time]
,       r.[end_compile_time]
,       r.[end_time]
,       DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,       DATEDIFF(ms,r.[start_time],r.[end_compile_time])   AS Request_compile_time_ms
,       DATEDIFF(ms,r.[end_compile_time],r.[end_time])     AS Request_execution_time_ms
,       r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID()
;
```

O `sys.dm_pdw_resource_waits` DMV mostra apenas as esperas de recursos consumidas por uma determinada consulta. Tempo de espera de recursos apenas mede o tempo aguardar por recursos para ser fornecido, em vez de tempo de espera de sinal, que é o tempo que demora para os servidores SQL subjacentes agendar a consulta para a CPU.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID()
;
```
Também pode utilizar o `sys.dm_pdw_resource_waits` DMV calcular quantos blocos de simultaneidade e foi concedidos.

```sql
SELECT  SUM([concurrency_slots_used]) as total_granted_slots 
FROM    sys.[dm_pdw_resource_waits] 
WHERE   [state]           = 'Granted' 
AND     [resource_class] is not null
AND     [session_id]     <> session_id()
;
```

O `sys.dm_pdw_wait_stats` DMV pode ser utilizado para análise de tendências históricas de esperas.

```sql
SELECT   w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w
;
```

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações sobre a gestão de utilizadores de base de dados e segurança, consulte [proteger uma base de dados no SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md). Para obter mais informações sobre classes de recursos maiores como podem melhorar a qualidade do índice columnstore em cluster, consulte [reconstruir índices para melhorar a qualidade do segmento](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality).


