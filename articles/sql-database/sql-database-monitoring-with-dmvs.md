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
ms.openlocfilehash: 8750670f2acc41cd712254ba11b4d2ec20aa58aa
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981849"
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

## <a name="monitor-resource-use"></a>Monitorizar a utilização de recursos

Pode monitorizar a utilização de recursos usando [SQL Database Query Performance Insight](sql-database-query-performance.md) e [Query Store](https://msdn.microsoft.com/library/dn817826.aspx).

Também pode monitorizar a utilização com as duas exibições:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
Pode utilizar o [DM db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) vista em cada base de dados SQL. O **DM db_resource_stats** vista mostra os dados de utilização de recursos recentes em relação a camada de serviços. Percentagens de média de CPU, de e/s de dados, de escritas de registo e de memória são gravadas a cada 15 segundos e são mantidas durante uma hora.

Uma vez que esta vista fornece uma visão mais granular no uso de recursos, utilize **DM db_resource_stats** primeiro para qualquer análise do estado atual ou resolução de problemas. Por exemplo, esta consulta mostra a utilização de recursos de média e máxima para a base de dados atual durante a última hora:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data IO in percent',
        MAX(avg_data_io_percent) AS 'Maximum data IO in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Para outras consultas, consulte os exemplos [DM db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats
O [resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) ver no **mestre** base de dados tem informações adicionais que podem ajudar a monitorizar o desempenho de base de dados SQL na sua camada de serviço específico e tamanho de computação. Os dados são recolhidos a cada 5 minutos e são mantidos para cerca de 14 dias. Esta vista é útil para uma análise de histórica de longo prazo de como a base de dados SQL utiliza recursos.

O gráfico seguinte mostra a utilização de recursos da CPU para um banco de dados Premium com o tamanho de computação P2 por cada hora numa semana. Este gráfico começa na segunda-feira, mostra as 5 dias de trabalho e, em seguida, mostra um fim de semana, quando muito menos acontece no aplicativo.

![Utilização de recursos de base de dados SQL](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Dos dados, esta base de dados tem atualmente um pico de carga de CPU de pouco mais de 50% de utilização de CPU relativo ao P2 computação tamanho (midday na Terça-feira). Se a CPU é o fator dominante no perfil de recurso do aplicativo, em seguida, poderá decidir que P2 tem um tamanho de computação certas para garantir que a carga de trabalho sempre se encaixa. Se necessitar de uma aplicação a crescer ao longo do tempo, é uma boa idéia ter um buffer de recurso adicional para que a aplicação já não atinge o limite de nível de desempenho. Se aumentar o tamanho de computação, pode ajudar a evitar erros visíveis para o cliente que podem ocorrer quando uma base de dados não tem energia suficiente para processar pedidos de forma eficaz, especialmente em ambientes sensíveis à latência. Um exemplo é uma base de dados que oferece suporte a um aplicativo que pinta páginas da Web com base nos resultados das chamadas de base de dados.

Outros tipos de aplicação podem interpretar o mesmo gráfico de forma diferente. Por exemplo, se um aplicativo tenta processar dados de folha de pagamento por dia e tem o mesmo gráfico, esse tipo de modelo de "tarefa do batch" pode fazer bem num tamanho de computação P1. O tamanho de computação P1 tem 100 DTUs em comparação com 200 DTUs em P2 tamanho de computação. O tamanho de computação P1 fornece o que tamanho de computação de metade o desempenho da P2. Então, 50 por cento de utilização de CPU em P2 é igual a utilização de CPU de 100 por cento do P1. Se o aplicativo não tiver tempos limite, poderá não importa se uma tarefa demora duas horas ou 2,5 horas a concluir, se isso é feito hoje mesmo. Provavelmente, uma aplicação nesta categoria pode utilizar um tamanho de computação P1. Pode aproveitar o fato de haver períodos de tempo durante o dia quando uso de recursos é mais baixo, para que qualquer "pico grande" poderá vazam ao longo para um dos vales mais tarde no dia. O tamanho de computação P1 pode ser bom para esse tipo de aplicativo (e poupar dinheiro), enquanto as tarefas podem concluído dentro do prazo por dia.

Expõe de base de dados SQL do Azure consumidos informações de recursos para cada base de dados no Active Directory a **resource_stats** ver da **mestre** em cada servidor da base de dados. Os dados na tabela são agregados para intervalos de 5 minutos. Com os escalões de serviço básico, Standard e Premium, os dados podem demorar mais de 5 minutos a aparecer na tabela, para que estes dados são mais úteis para análise histórica, em vez de análise quase em tempo real. Consulta a **resource_stats** visualizados para ver o histórico recente de uma base de dados e para validar a reserva decidir entregar o desempenho, mesmo quando necessário.

> [!NOTE]
> Tem de estar ligado para o **mestre** base de dados do seu servidor lógico para a base de dados SQL para consultar **resource_stats** nos exemplos a seguir.
> 
> 

Este exemplo mostra como os dados nesta vista são expostos:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![A vista de catálogo resource_stats](./media/sql-database-performance-guidance/sys_resource_stats.png)

O exemplo seguinte mostra-lhe diferentes formas que pode utilizar o **resource_stats** vista para obter informações sobre como a base de dados SQL utiliza recursos do catálogo:

1. Para examinar o recurso da semana passada utilizar para o userdb1 de base de dados, pode executar esta consulta:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. Para avaliar a eficiência com que sua carga de trabalho se ajusta o tamanho de computação, terá de fazer uma busca detalhada em cada aspecto das métricas de recursos: CPU, leituras, gravações, o número de workers e número de sessões. Aqui está um revisado consultar com o **resource_stats** para comunicar os valores de médios e máximo destas métricas de recurso:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data IO use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data IO use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Com essas informações sobre os valores de médios e máxima de cada métrica de recurso, pode avaliar a eficiência com que sua carga de trabalho se ajusta o tamanho de computação que escolheu. Normalmente, médio de valores da **resource_stats** dão-lhe uma linha de base boa para utilizar com o tamanho de destino. Deve ser o pen Drive primários medida. Por exemplo, poderá estar a utilizar o escalão de serviço Standard com o tamanho de computação de S2. A média utilizar percentagens para leituras de e/s e CPU e gravações são abaixo 40 por cento, o número médio de funções de trabalho está abaixo de 50 e o número médio de sessões é inferior a 200. A carga de trabalho pode se encaixa o tamanho de computação de S1. É fácil ver se a base de dados se encaixa os limites de trabalho e de sessão. Para ver se uma base de dados se encaixa num tamanho de computação mais baixo com respeito a divisão de CPU, leituras e escritas, o número de DTUS na parte inferior tamanho pelo número DTU de seu tamanho atual de computação de computação e, em seguida, multiplique o resultado em 100:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    O resultado é a diferença de desempenho relativo entre os dois tamanhos de percentagem de computação. Se a utilização de recursos não exceder este montante, sua carga de trabalho pode se encaixa o tamanho de computação mais baixo. No entanto, precisa examinar todos os intervalos de valores de utilização de recursos e determinar, por porcentagem, a frequência com que sua carga de trabalho de base de dados caberia no tamanho de computação mais baixo. A seguinte consulta devolve a percentagem de ajuste por dimensão de recursos, com base num limiar de 40% que Calculamos neste exemplo:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Com base no escalão de serviço de base de dados, pode decidir se a carga de trabalho se ajusta o tamanho de computação mais baixo. Se o objetivo de carga de trabalho de base de dados é de 99,9% e a consulta anterior devolve valores superiores a 99,9% para todas as dimensões de três recursos, a carga de trabalho provavelmente adapta-se o tamanho de computação mais baixo.
   
    Observando a percentagem de ajuste também fornece uma idéia se deve avançar para o próximo tamanho maior de computação para cumprir o objetivo. Por exemplo, userdb1 mostra a utilização de CPU seguinte para a última semana:
   
   | Percentagem de CPU média | Percentagem de CPU máxima |
   | --- | --- |
   | 24.5 |100.00 |
   
    A CPU média trata de um trimestre do limite do tamanho da computação, o que caberia bem no tamanho de computação da base de dados. No entanto, o valor máximo mostra que a base de dados atinge o limite do tamanho da computação. Precisa mover para o próximo tamanho maior de computação? Veja como muitas vezes sua carga de trabalho de atingir 100 por cento e depois o compare com o objetivo de carga de trabalho de base de dados.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Se essa consulta retorna um valor inferior a 99,9% para qualquer uma das dimensões três recursos, considere a mover para o próximo tamanho de computação superior ou usar técnicas de otimização de aplicações para reduzir a carga na base de dados SQL.
4. Neste exercício também considera o aumento de carga de trabalho previsto no futuro.

Para conjuntos elásticos, pode monitorizar bases de dados individuais no conjunto, com as técnicas descritas nesta secção. Mas também é possível monitorizar o conjunto como um todo. Para informações, consulte [Monitorizar e gerir um conjunto elástico](sql-database-elastic-pool-manage-portal.md).


### <a name="maximum-concurrent-requests"></a>Pedidos em simultâneo máximos
Para ver o número de pedidos simultâneos, execute esta consulta Transact-SQL na base de dados SQL:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Para analisar a carga de trabalho de uma base de dados do SQL Server no local, modificar esta consulta para filtrar na base de dados específico que pretende analisar. Por exemplo, se tiver uma base de dados no local com o nome MyDatabase, esta consulta de Transact-SQL retorna a contagem de pedidos simultâneos nessa base de dados:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Este é apenas um instantâneo num único ponto no tempo. Para obter uma melhor compreensão da sua carga de trabalho e os requisitos de pedido simultâneo, terá de recolher muitos exemplos ao longo do tempo.

### <a name="maximum-concurrent-logins"></a>Inícios de sessão simultâneos máximos
Pode analisar os padrões de utilizador e da aplicação para ter uma idéia da frequência de inícios de sessão. Também pode executar cargas reais num ambiente de teste para se certificar de que não estiver a obter isso ou outros limites que discutimos neste artigo. Não há uma única consulta ou a vista de gestão dinâmica (DMV) que pode apresentar-lhe em simultâneo a conta de início de sessão ou o histórico.

Se vários clientes utilizarem a mesma cadeia de ligação, o serviço autentica cada início de sessão. Se 10 utilizadores em simultâneo ligar a uma base de dados com o mesmo nome de utilizador e palavra-passe, haveria 10 inícios de sessão simultâneos. Este limite aplica-se apenas para a duração do início de sessão e a autenticação. Se os mesmos 10 utilizadores se ligarem à base de dados sequencialmente, o número de inícios de sessão simultâneos nunca seria maior que 1.

> [!NOTE]
> Atualmente, este limite não é aplicável às bases de dados nos conjuntos elásticos.
> 
> 

### <a name="maximum-sessions"></a>Máximo de sessões
Para ver o número de sessões ativas atuais, execute esta consulta Transact-SQL na base de dados SQL:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Se está a analisar uma carga de trabalho do SQL Server no local, modifique a consulta para se concentrar num banco de dados específico. Esta consulta ajuda a determinar as necessidades de sessão possíveis para a base de dados se estiver a considerar movê-lo para a base de dados do Azure SQL.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Novamente, estas consultas devolvem uma contagem de ponto no tempo. Se recolher vários exemplos ao longo do tempo, terá a melhor compreensão da sua sessão de utilizar.

Para análise de base de dados SQL, pode obter estatísticas históricas sessões consultando o [resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) vista e ao rever o **active_session_count** coluna. 

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

