---
title: Criar e gerir o Azure SQL da base de dados as tarefas elásticas com o Transact-SQL (T-SQL) | Microsoft Docs
description: Execute scripts entre muitas bases de dados com o agente de tarefa de base de dados elásticas com o Transact-SQL (T-SQL).
services: sql-database
author: jaredmoo
manager: craigg
ms.service: sql-database
ms.topic: article
ms.date: 06/14/2018
ms.author: jaredmoo
ms.openlocfilehash: fb6e4ebd635d8afa8e679ee5bb0f5646f28f887b
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313340"
---
# <a name="use-transact-sql-t-sql-to-create-and-manage-elastic-database-jobs"></a>Utilize o Transact-SQL (T-SQL) para criar e gerir tarefas de base de dados elásticas

Este artigo fornece muitos cenários de exemplo para começar a trabalhar com as tarefas elásticas com o T-SQL.

Os exemplos utilizam o [procedimentos armazenados](#job-stored-procedures) e [vistas](#job-views) disponíveis no [ *base de dados de tarefa*](elastic-jobs-overview.md#job-database).

Transact-SQL (T-SQL) é utilizado para criar, configurar, executar e gerir tarefas. Criar o agente de tarefa elástico não é suportado em T-SQL, pelo que deve criar primeiro um *agente tarefa elástico* a utilizar o portal ou [PowerShell](elastic-jobs-powershell.md#create-the-elastic-job-agent).


## <a name="create-a-credential-for-job-execution"></a>Criar uma credencial para a execução da tarefa

A credencial é utilizada para ligar ao seu bases de dados de destino para a execução do script. A credencial tem as permissões adequadas, nas bases de dados especificadas por grupo de destino, com êxito a executar o script. Quando utilizar um servidor e/ou membro do grupo de destino de agrupamento, é altamente sugerida para criar uma credencial principal para ser utilizado atualizar a credencial antes da expansão do servidor e/ou do conjunto no tempo de execução da tarefa. A credencial com âmbito de base de dados é criada na base de dados de agente da tarefa. As mesma credenciais tem de ser utilizada para *criar um início de sessão* e *criar um utilizador de início de sessão para conceder as permissões de base de dados de início de sessão* nas bases de dados de destino.


```sql
--Connect to the job database specified when creating the job agent

-- Create a db master key if one does not already exist, using your own password.  
CREATE MASTER KEY ENCRYPTION BY PASSWORD='<EnterStrongPasswordHere>';  
  
-- Create a database scoped credential.  
CREATE DATABASE SCOPED CREDENTIAL myjobcred WITH IDENTITY = 'jobcred',
    SECRET = '<EnterStrongPasswordHere>'; 
GO

-- Create a database scoped credential for the master database of server1.
CREATE DATABASE SCOPED CREDENTIAL mymastercred WITH IDENTITY = 'mastercred',
    SECRET = '<EnterStrongPasswordHere>'; 
GO
```

## <a name="create-a-target-group-servers"></a>Criar um grupo de destino (servidores)

O exemplo seguinte mostra como executar uma tarefa em todas as bases de dados num servidor.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:


```sql
-- Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC jobs.sp_add_target_group 'ServerGroup1'

-- Add a server target member
EXEC jobs.sp_add_target_group_member
'ServerGroup1',
@target_type = 'SqlServer',
@refresh_credential_name='mymastercred', --credential required to refresh the databases in server
@server_name='server1.database.windows.net'

--View the recently created target group and target group members
SELECT * FROM jobs.target_groups WHERE target_group_name='ServerGroup1';
SELECT * FROM jobs.target_group_members WHERE target_group_name='ServerGroup1';
```


## <a name="exclude-a-single-database"></a>Excluir uma base de dados

O exemplo seguinte mostra como executar uma tarefa em todas as bases de dados num servidor, exceto para a base de dados com o nome *MappingDB*.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing server(s)
EXEC [jobs].sp_add_target_group = N'ServerGroup'
GO

-- Add a server target member
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'London.database.windows.net'
GO

-- Add a server target member
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name='server2.database.windows.net'
GO

--Excude a database target member from the server target group
EXEC [jobs].sp_add_target_group_member
@target_group_name = N'ServerGroup',
@membership_type = N'Exclude',
@target_type = N'SqlDatabase',
@server_name = N'server1.database.windows.net',
@database_name =N'MappingDB'
GO

--View the recently created target group and target group members
SELECT * FROM [jobs].target_groups WHERE target_group_name = N'ServerGroup';
SELECT * FROM [jobs].target_group_members WHERE target_group_name = N'ServerGroup';
```


## <a name="create-a-target-group-pools"></a>Criar um grupo de destino (agrupamentos)

O exemplo seguinte mostra como todas as bases de dados num ou mais conjuntos elásticos de destino.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

-- Add a target group containing pool(s)
EXEC jobs.sp_add_target_group 'PoolGroup'

-- Add an elastic pool(s) target member
EXEC jobs.sp_add_target_group_member
'PoolGroup',
@target_type = 'SqlElasticPool',
@refresh_credential_name='mymastercred', --credential required to refresh the databases in server
@server_name='server1.database.windows.net',
@elastic_pool_name='ElasticPool-1'

-- View the recently created target group and target group members
SELECT * FROM jobs.target_groups WHERE target_group_name = N'PoolGroup';
SELECT * FROM jobs.target_group_members WHERE target_group_name = N'PoolGroup';
```


## <a name="deploy-new-schema-to-many-databases"></a>Implementar o novo esquema em muitas bases de dados

O exemplo seguinte mostra como implementar o novo esquema em todas as bases de dados.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:


```sql
--Connect to the job database specified when creating the job agent

--Add job for create table
EXEC jobs.sp_add_job @job_name='CreateTableTest', @description='Create Table Test'

-- Add job step for create table
EXEC jobs.sp_add_jobstep @job_name='CreateTableTest',
@command=N'IF NOT EXISTS (SELECT * FROM sys.tables 
            WHERE object_id = object_id(''Test''))
CREATE TABLE [dbo].[Test]([TestId] [int] NOT NULL);',
@credential_name='myjobcred',
@target_group_name='PoolGroup'
```


## <a name="data-collection-using-built-in-parameters"></a>Recolha de dados utilizando parâmetros incorporados

Em vários cenários de recolha de dados, pode ser útil para incluir algumas destas variáveis de script para o ajudar a pós-cópia processar os resultados da tarefa.

- $(job_name)
- $(job_id)
- $(job_version)
- $(step_id)
- $(step_name)
- $(job_execution_id)
- $(job_execution_create_time)
- $(target_group_name)

Por exemplo, para o grupo de todos os resultados da execução da tarefa mesmo em conjunto, utilize o *$(job_execution_id)* conforme mostrado no comando seguinte:


```sql
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());'
```


## <a name="monitor-database-performance"></a>Monitorizar o desempenho da base de dados

O exemplo seguinte cria uma nova tarefa para recolher dados de desempenho a partir de várias bases de dados.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute os seguintes comandos:

```sql
--Connect to the job database specified when creating the job agent

-- Add a job to collect perf results
EXEC jobs.sp_add_job @job_name ='ResultsJob', @description='Collection Performance data from all customers'

-- Add a job step w/ schedule to collect results
EXEC jobs.sp_add_jobstep
@job_name='ResultsJob',
@command= N' SELECT DB_NAME() DatabaseName, $(job_execution_id) AS job_execution_id, * FROM sys.dm_db_resource_stats WHERE end_time > DATEADD(mi, -20, GETDATE());',
@credential_name='myjobcred',
@target_group_name='PoolGroup',
@output_type='SqlDatabase',
@output_credential_name=’myjobcred’,
@output_server_name=’server1.database.windows.net',
@output_database_name=’<resultsdb>',
@output_table_name='<resutlstable>'
Create a job to monitor pool performance
--Connect to the job database specified when creating the job agent

-- Add a target group containing master database
EXEC jobs.sp_add_target_group 'MasterGroup'

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name='MasterGroup',
@target_type='SqlDatabase',
@server_name='server1.database.windows.net',
@database_name='master'

-- Add a job to collect perf results
EXEC jobs.sp_add_job
@job_name='ResultsPoolsJob',
@description='Demo: Collection Performance data from all pools',
@schedule_interval_type='Minutes',
@schedule_interval_count=15

-- Add a job step w/ schedule to collect results
EXEC jobs.sp_add_jobstep
@job_name='ResultsPoolsJob',
@command=N'declare @now datetime
DECLARE @startTime datetime
DECLARE @endTime datetime
DECLARE @poolLagMinutes datetime
DECLARE @poolStartTime datetime
DECLARE @poolEndTime datetime
SELECT @now = getutcdate ()
SELECT @startTime = dateadd(minute, -15, @now)
SELECT @endTime = @now
SELECT @poolStartTime = dateadd(minute, -30, @startTime)
SELECT @poolEndTime = dateadd(minute, -30, @endTime)

SELECT elastic_pool_name , end_time, elastic_pool_dtu_limit, avg_cpu_percent, avg_data_io_percent, avg_log_write_percent, max_worker_percent, max_session_percent,
        avg_storage_percent, elastic_pool_storage_limit_mb FROM sys.elastic_pool_resource_stats
        WHERE end_time > @poolStartTime and end_time <= @poolEndTime;
'),
@credential_name='myjobcred',
@target_group_name='MasterGroup',
@output_type='SqlDatabase',
@output_credential_name='myjobcred',
@output_server_name=’server1.database.windows.net',
@output_database_name=’resultsdb',
@output_table_name='resutlstable'
```


## <a name="view-job-definitions"></a>Definições de tarefas da vista

O exemplo seguinte mostra como ver definições de tarefas atual.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

-- View all jobs
SELECT * FROM jobs.jobs

-- View the steps of the current version of all jobs
SELECT js.* FROM jobs.jobsteps js
JOIN jobs.jobs j 
  ON j.job_id = js.job_id AND j.job_version = js.job_version

-- View the steps of all versions of all jobs
select * from jobs.jobsteps
```


## <a name="begin-ad-hoc-execution-of-a-job"></a>Iniciar a execução do ad-hoc de uma tarefa

O exemplo seguinte mostra como iniciar uma tarefa de imediato.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

-- Execute the latest version of a job
EXEC jobs.sp_start_job 'CreateTableTest'

-- Execute the latest version of a job and receive the execution id
declare @je uniqueidentifier
exec jobs.sp_start_job 'CreateTableTest', @job_execution_id = @je output
select @je

select * from jobs.job_executions where job_execution_id = @je

-- Execute a specific version of a job (e.g. version 1)
exec jobs.sp_start_job 'CreateTableTest', 1
```


## <a name="schedule-execution-of-a-job"></a>Agendar a execução de uma tarefa

O exemplo seguinte mostra como agendar uma tarefa para execução futura.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_update_job
@job_name='ResultsJob',
@enabled=1,
@schedule_interval_type='Minutes',
@schedule_interval_count=15
```

## <a name="monitor-job-execution-status"></a>Monitorizar o estado de execução da tarefa

O exemplo seguinte mostra como ver os detalhes do Estado de execução para todas as tarefas.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

--View top-level execution status for the job named ‘ResultsPoolJob’
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' and step_id IS NULL
ORDER BY start_time DESC

--View all top-level execution status for all jobs
SELECT * FROM jobs.job_executions WHERE step_id IS NULL
ORDER BY start_time DESC

--View all execution statuses for job named ‘ResultsPoolsJob’
SELECT * FROM jobs.job_executions 
WHERE job_name = 'ResultsPoolsJob' 
ORDER BY start_time DESC

-- View all active executions
SELECT * FROM jobs.job_executions 
WHERE is_active = 1
ORDER BY start_time DESC
```


## <a name="cancel-a-job"></a>Cancelar uma tarefa

O exemplo seguinte mostra como cancelar uma tarefa.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

-- View all active executions to determine job execution id
SELECT * FROM jobs.job_executions 
WHERE is_active = 1 AND job_name = 'ResultPoolsJob'
ORDER BY start_time DESC
GO

-- Cancel job execution with the specified job execution id
EXEC jobs.sp_stop_job '01234567-89ab-cdef-0123-456789abcdef'
```


## <a name="delete-old-job-history"></a>Eliminar histórico antigo de tarefas

O exemplo seguinte mostra como eliminar histórico de tarefas antes de uma data específica.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

-- Delete history of a specific job’s executions older than the specified date
EXEC jobs.sp_purge_jobhistory @job_name='ResultPoolsJob', @oldest_date='2016-07-01 00:00:00'

--Note: job history is automatically deleted if it is >45 days old
```

## <a name="delete-a-job-and-all-its-job-history"></a>Eliminar uma tarefa e todos os respetivo histórico de tarefas

O exemplo seguinte mostra como eliminar uma tarefa e todos os relacionados de histórico de tarefas.  
Ligar para o [ *base de dados de tarefa* ](elastic-jobs-overview.md#job-database) e execute o seguinte comando:

```sql
--Connect to the job database specified when creating the job agent

EXEC jobs.sp_delete_job @job_name='ResultsPoolsJob'

--Note: job history is automatically deleted if it is >45 days old
```




## <a name="job-stored-procedures"></a>Tarefa de procedimentos armazenados

Os seguintes procedimentos armazenados são no [base de dados de tarefas](elastic-jobs-overview.md#job-database).



|Procedimento armazenado  |Descrição  |
|---------|---------|
|[sp_add_job](#spaddjob)     |     Adiciona uma nova tarefa.    |
|[sp_update_job ](#spupdatejob)    |      Atualiza uma tarefa existente.   |
|[sp_delete_job](#spdeletejob)     |      Elimina uma tarefa existente.   |
|[sp_add_jobstep](#spaddjobstep)    |    Adiciona um passo para uma tarefa.     |
|[sp_update_jobstep](#spupdatejobstep)     |     Atualiza um passo de tarefa.    |
|[sp_delete_jobstep](#spdeletejobstep)     |     Elimina um passo de tarefa.    |
|[sp_start_job](#spstartjob)    |  Começar a ser executada uma tarefa.       |
|[sp_stop_job](#spstopjob)     |     Interrompe a execução da tarefa.   |
|[sp_add_target_group](#spaddtargetgroup)    |     Adiciona um grupo de destino.    |
|[sp_delete_target_group](#spdeletetargetgroup)     |    Elimina um grupo de destino.     |
|[sp_add_target_group_member](#spaddtargetgroupmember)     |    Adiciona uma base de dados ou grupo de bases de dados a um grupo de destino.     |
|[sp_delete_target_group_member](#spdeletetargetgroupmember)     |     Remove um membro do grupo de destino um grupo de destino.    |
|[sp_purge_jobhistory ](#sppurgejobhistory)    |    Remove os registos do histórico para uma tarefa.     |





### <a name="spaddjob"></a>sp_add_job

Adiciona uma nova tarefa. 
  
#### <a name="syntax"></a>Sintaxe  
  

```sql
[jobs].sp_add_job [ @job_name = ] 'job_name'  
    [ , [ @description = ] 'description' ]   
    [ , [ @enabled = ] enabled ]
    [ , [ @schedule_interval_type = ] schedule_interval_type ]  
    [ , [ @schedule_interval_count = ] schedule_interval_count ]   
    [ , [ @schedule_start_time = ] schedule_start_time ]   
    [ , [ @schedule_end_time = ] schedule_end_time ]   
    [ , [ @job_id = ] job_id OUTPUT ]
```

  
#### <a name="arguments"></a>Argumentos  

[  **@job_name =** ] 'job_name'  
O nome da tarefa. O nome tem de ser exclusivo e não pode conter o caráter de percentagem (%). job_name é nvarchar(128), com sem predefinição.

[  **@description =** ] 'description'  
A descrição da tarefa. a descrição é nvarchar(512), com um predefinido nulo. Se for omitida descrição, é utilizada uma cadeia vazia.

[  **@enabled =** ] ativada  
Se a agenda da tarefa está ativada. Ativar bits, com uma predefinição de 0 (desativado). Se for 0, a tarefa não está ativada e não é executado de acordo com a sua agenda; No entanto, pode ser executado manualmente. Se 1, a tarefa será executada de acordo com a sua agenda e também pode ser executada manualmente.

[  **@schedule_interval_type =**] schedule_interval_type  
O valor indica quando a tarefa está a ser executado. schedule_interval_type é nvarchar(50), com uma predefinição de uma vez e pode ser um dos seguintes valores:
- 'Uma vez',
- 'Minutos',
- 'Hours'
- 'Dias',
- 'Semanas',
- 'Meses'

[  **@schedule_interval_count =** ] schedule_interval_count  
Número de períodos de schedule_interval_count ocorra entre cada execução da tarefa. schedule_interval_count é int, com um predefinido de 1. O valor tem de ser maior que ou igual a 1.

[  **@schedule_start_time =** ] schedule_start_time  
Data na qual a tarefa pode iniciar a execução. schedule_start_time é DATETIME2, com a predefinição de 00:00:00.0000000 0001 e-01-01.

[  **@schedule_end_time =** ] schedule_end_time  
Data na qual a tarefa pode parar a execução. schedule_end_time é DATETIME2, com a predefinição de 9999-12-31 11:59:59.0000000. 

[  **@job_id =** ] job_id saída  
O número de identificação de tarefa atribuído à tarefa se criada com êxito. job_id é uma variável de saída do tipo uniqueidentifier.

#### <a name="return-code-values"></a>Valores de código de retorno

0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
sp_add_job tem de ser executada a partir da base de dados de agente do trabalho especificado ao criar o agente de tarefa.
Depois de sp_add_job foi executado para adicionar uma tarefa, sp_add_jobstep pode ser utilizado para adicionar os passos que executam as atividades para a tarefa. Número de versão inicial da tarefa for 0, o qual será incrementada para 1 quando é adicionado o primeiro passo.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:

- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.

### <a name="spupdatejob"></a>sp_update_job

Atualiza uma tarefa existente.

#### <a name="syntax"></a>Sintaxe

```sql
[jobs].sp_update_job [ @job_name = ] 'job_name'  
    [ , [ @new_name = ] 'new_name' ]
    [ , [ @description = ] 'description' ]   
    [ , [ @enabled = ] enabled ]
    [ , [ @schedule_interval_type = ] schedule_interval_type ]  
    [ , [ @schedule_interval_count = ] schedule_interval_count ]   
    [ , [ @schedule_start_time = ] schedule_start_time ]   
    [ , [ @schedule_end_time = ] schedule_end_time ]   
```

#### <a name="arguments"></a>Argumentos
[  **@job_name =** ] 'job_name'  
O nome da tarefa a ser atualizado. job_name é nvarchar(128).

[  **@new_name =** ] 'new_name'  
O novo nome da tarefa. new_name é nvarchar(128).

[  **@description =** ] 'description'  
A descrição da tarefa. a descrição é nvarchar(512).

[  **@enabled =** ] ativada  
Especifica se o agendamento do trabalho é ativado (1) ou não ativada (0). Ativada é bit.

[  **@schedule_interval_type=** ] schedule_interval_type  
O valor indica quando a tarefa está a ser executado. schedule_interval_type é nvarchar(50) e pode ser um dos seguintes valores:

- 'Uma vez',
- 'Minutos',
- 'Hours'
- 'Dias',
- 'Semanas',
- 'Meses'

[  **@schedule_interval_count=** ] schedule_interval_count  
Número de períodos de schedule_interval_count ocorra entre cada execução da tarefa. schedule_interval_count é int, com um predefinido de 1. O valor tem de ser maior que ou igual a 1.

[  **@schedule_start_time=** ] schedule_start_time  
Data na qual a tarefa pode iniciar a execução. schedule_start_time é DATETIME2, com a predefinição de 00:00:00.0000000 0001 e-01-01.

[  **@schedule_end_time=** ] schedule_end_time  
Data na qual a tarefa pode parar a execução. schedule_end_time é DATETIME2, com a predefinição de 9999-12-31 11:59:59.0000000. 

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Depois de sp_add_job foi executado para adicionar uma tarefa, sp_add_jobstep pode ser utilizado para adicionar os passos que executam as atividades para a tarefa. Número de versão inicial da tarefa for 0, o qual será incrementada para 1 quando é adicionado o primeiro passo.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.



### <a name="spdeletejob"></a>sp_delete_job

Elimina uma tarefa existente.

#### <a name="syntax"></a>Sintaxe

```sql
[jobs].sp_delete_job [ @job_name = ] 'job_name'
    [ , [ @force = ] force ]
```

#### <a name="arguments"></a>Argumentos
[  **@job_name =** ] 'job_name'  
O nome da tarefa para ser eliminado. job_name é nvarchar(128).

[  **@force =** ] forçar  
Especifica se pretende eliminar se a tarefa tiver qualquer execuções em curso e cancele todas as execuções de em curso (1) ou falhar se forem as execuções de tarefa em curso (0). bit de imposição.

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Histórico de tarefas é eliminado automaticamente quando uma tarefa é eliminada.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.



### <a name="spaddjobstep"></a>sp_add_jobstep

Adiciona um passo para uma tarefa.

#### <a name="syntax"></a>Sintaxe


```sql
[jobs].sp_add_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] step_name ]   
     [ , [ @command_type = ] 'command_type' ]   
     [ , [ @command_source = ] 'command_source' ]  
     , [ @command = ] 'command'
     , [ @credential_name = ] 'credential_name'
     , [ @target_group_name = ] 'target_group_name'
     [ , [ @initial_retry_interval_seconds = ] initial_retry_interval_seconds ]   
     [ , [ @maximum_retry_interval_seconds = ] maximum_retry_interval_seconds ]   
     [ , [ @retry_interval_backoff_multiplier = ] retry_interval_backoff_multiplier ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @step_timeout_seconds = ] step_timeout_seconds ]   
     [ , [ @output_type = ] 'output_type' ]   
     [ , [ @output_credential_name = ] 'output_credential_name' ]   
     [ , [ @output_subscription_id = ] 'output_subscription_id' ]   
     [ , [ @output_resource_group_name = ] 'output_resource_group_name' ]   
     [ , [ @output_server_name = ] 'output_server_name' ]   
     [ , [ @output_database_name = ] 'output_database_name' ]   
     [ , [ @output_schema_name = ] 'output_schema_name' ]   
     [ , [ @output_table_name = ] 'output_table_name' ]
     [ , [ @job_version = ] job_version OUTPUT ]
     [ , [ @max_parallelism = ] max_parallelism ]
```

#### <a name="arguments"></a>Argumentos

[  **@job_name =** ] 'job_name'  
O nome da tarefa para o qual pretende adicionar o passo. job_name é nvarchar(128).

[  **@step_id =** ] step_id  
O número de identificação de sequência para o passo de tarefa. Os números de identificação de passo começam em 1 e incrementar sem lacunas. Se um passo existente já tiver este id, em seguida, que o passo e todos os passos seguintes terão as respetivas id do incrementado para que este novo passo pode ser inserido na sequência. Se não for especificado, o step_id será automaticamente atribuído para o último na sequência de passos. step_id é um inteiro.

[  **@step_name =** ] step_name  
O nome do passo. Tem de ser especificado, exceto o primeiro passo de uma tarefa que (para sua comodidade) tem um nome predefinido de 'Passo'. step_name é nvarchar(128).

[  **@command_type =** ] 'command_type'  
O tipo de comando que é executado por este passo de tarefa. command_type é nvarchar(50), com um valor predefinido de TSql, que significa que o valor da @command_type parâmetro é um script T-SQL.

Se for especificado, o valor tem de ser TSql.

[  **@command_source =** ] 'command_source'  
O tipo de localização onde está armazenado o comando. command_source é nvarchar(50), com um valor predefinido Inline, o que significa que o valor da @command_source parâmetro é o texto literal do comando.

Se for especificado, o valor tem de ser Inline.

[  **@command =** ] '. o comando'  
O comando tem de ser válido script T-SQL e, em seguida, é executado por este passo de tarefa. comando é nvarchar (Max), com um predefinido nulo.

[  **@credential_name =** ] 'credential_name'  
O nome da base de dados no âmbito credenciais armazenadas nesta base de dados de controlo de tarefa que é utilizada para ligar a cada uma das bases de dados de destino dentro do grupo de destino, quando este passo é executado. credential_name é nvarchar(128).

[  **@target_group_name =** ] 'group_name de destino'  
O nome do grupo de destino que contém as bases de dados de destino que o passo de tarefa será executado no. target_group_name é nvarchar(128).

[  **@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
O atraso antes de repetir a primeira tentativa, se o passo de tarefa falhar durante a tentativa de execução inicial. initial_retry_interval_seconds é int, com o valor predefinido de 1.

[  **@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
O máximo de atraso entre tentativas de repetição. Se o atraso entre tentativas seria aumentar maior do que este valor,-lo em vez disso, é limitado a este valor. maximum_retry_interval_seconds é int, com o valor predefinido de 120.

[  **@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
O multiplicador para aplicar o atraso de repetição, se a tarefa de vários passo execução tenta falhar. Por exemplo, se a primeira novamente tinha um atraso de 5 segundos e o multiplicador término 2.0, em seguida, o segundo novamente terão um atraso de 10 segundos e o terceiro novamente terá um atraso de 20 segundos. retry_interval_backoff_multiplier é reais, com o valor predefinido de 2.0.

[  **@retry_attempts =** ] retry_attempts  
O número de vezes para repetir a execução se a tentativa inicial falhar. Por exemplo, se o valor de retry_attempts for 10, existirão 1 inicial tentativa e 10 tentativas de repetição, dar um total de tentativas de 11. Se a tentativa de repetição final falhar, irá terminar a execução da tarefa com um ciclo de vida de falha. retry_attempts é int, com o valor predefinido de 10.

[  **@step_timeout_seconds =** ] step_timeout_seconds  
A quantidade máxima de tempo permitido para o passo para executar. Se for excedida desta vez, irá terminar a execução da tarefa com um ciclo de vida do ServiceHost. step_timeout_seconds é int, com o valor predefinido de 43.200 segundos (12 horas).

[  **@output_type =** ] 'output_type'  
Se não for nulo, o tipo de destino que definir resultado primeiro o comando é escrito. output_type é nvarchar(50), com um predefinido nulo.

Se for especificado, o valor tem de ser SqlDatabase.

[  **@output_credential_name =** ] 'output_credential_name'  
Se não nula, o nome da base de dados credencial com âmbito que é utilizada para ligar à base de dados de destino de saída. Tem de ser especificado se output_type é igual a SqlDatabase. output_credential_name é nvarchar(128), com um valor predefinido nulo.

[  **@output_subscription_id =** ] 'output_subscription_id'  
Tem de descrição.

[  **@output_resource_group_name =** ] 'output_resource_group_name'  
Tem de descrição.

[  **@output_server_name =** ] 'output_server_name'  
Se não for nulo, o nome DNS completamente qualificado do servidor que contém a base de dados de destino de saída. Tem de ser especificado se output_type é igual a SqlDatabase. output_server_name é nvarchar(256), com um predefinido nulo.

[  **@output_database_name =** ] 'output_database_name'  
Se não for nulo, o nome da base de dados que contém a tabela de destino de saída. Tem de ser especificado se output_type é igual a SqlDatabase. output_database_name é nvarchar(128), com um predefinido nulo.

[  **@output_schema_name =** ] 'output_schema_name'  
Se não for nulo, o nome do esquema SQL que contém a tabela de destino de saída. Se output_type for igual a SqlDatabase, o valor predefinido é dbo. output_schema_name é nvarchar(128).

[  **@output_table_name =** ] 'output_table_name'  
Se não for nulo, o nome da tabela que o resultado do comando de primeiro definido será escrito. Se a tabela já não existir, será criado com base no esquema do conjunto de resultados de devolver. Tem de ser especificado se output_type é igual a SqlDatabase. output_table_name é nvarchar(128), com um valor predefinido nulo.

[  **@job_version =** ] job_version saída  
Parâmetro de saída que será atribuído o novo número de versão da tarefa. job_version é int.

[  **@max_parallelism =** ] max_parallelism saída  
O nível máximo de paralelismo por agrupamento elástico. Se o conjunto, em seguida, o passo de tarefa serão restritas apenas com um máximo de que muitas bases de dados por conjunto elástico. Isto aplica-se cada agrupamento elástico que diretamente está incluído no grupo de destino ou está dentro de um servidor que está incluído no grupo de destino. max_parallelism é int.


#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Quando sp_add_jobstep for bem sucedida, o número da versão atual da tarefa é incrementado. Da próxima vez que a tarefa for executada, será utilizada a nova versão. Se a tarefa está atualmente em execução, essa execução não irá conter o passo de novo.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:  

- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.



### <a name="spupdatejobstep"></a>sp_update_jobstep

Atualiza um passo de tarefa.

#### <a name="syntax"></a>Sintaxe

```sql
[jobs].sp_update_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]   
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @new_id = ] new_id ]   
     [ , [ @new_name = ] 'new_name' ]   
     [ , [ @command_type = ] 'command_type' ]   
     [ , [ @command_source = ] 'command_source' ]  
     , [ @command = ] 'command'
     , [ @credential_name = ] 'credential_name'
     , [ @target_group_name = ] 'target_group_name'
     [ , [ @initial_retry_interval_seconds = ] initial_retry_interval_seconds ]   
     [ , [ @maximum_retry_interval_seconds = ] maximum_retry_interval_seconds ]   
     [ , [ @retry_interval_backoff_multiplier = ] retry_interval_backoff_multiplier ]   
     [ , [ @retry_attempts = ] retry_attempts ]   
     [ , [ @step_timeout_seconds = ] step_timeout_seconds ]   
     [ , [ @output_type = ] 'output_type' ]   
     [ , [ @output_credential_name = ] 'output_credential_name' ]   
     [ , [ @output_server_name = ] 'output_server_name' ]   
     [ , [ @output_database_name = ] 'output_database_name' ]   
     [ , [ @output_schema_name = ] 'output_schema_name' ]   
     [ , [ @output_table_name = ] 'output_table_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
     [ , [ @max_parallelism = ] max_parallelism ]
```

#### <a name="arguments"></a>Argumentos
[  **@job_name =** ] 'job_name'  
O nome da tarefa ao qual pertence o passo. job_name é nvarchar(128).

[  **@step_id =** ] step_id  
O número de identificação para o passo de tarefa a ser modificado. Step_id ou step_name tem de ser especificado. step_id é um inteiro.

[  **@step_name =** ] 'step_name'  
O nome do passo de ser modificados. Step_id ou step_name tem de ser especificado. step_name é nvarchar(128).

[  **@new_id =** ] new_id  
O novo número de identificação sequência para o passo de tarefa. Os números de identificação de passo começam em 1 e incrementar sem lacunas. Se um passo é reordenado, em seguida, obter outros passos irão ser automaticamente renumbered.

[  **@new_name =** ] 'new_name'  
O novo nome do passo. new_name é nvarchar(128).

[  **@command_type =** ] 'command_type'  
O tipo de comando que é executado por este passo de tarefa. command_type é nvarchar(50), com um valor predefinido de TSql, que significa que o valor da @command_type parâmetro é um script T-SQL.

Se for especificado, o valor tem de ser TSql.

[  **@command_source =** ] 'command_source'  
O tipo de localização onde está armazenado o comando. command_source é nvarchar(50), com um valor predefinido Inline, o que significa que o valor da @command_source parâmetro é o texto literal do comando.

Se for especificado, o valor tem de ser Inline.

[  **@command =** ] '. o comando'  
O aparece tem de ser válido script T-SQL e, em seguida, é executado por este passo de tarefa. comando é nvarchar (Max), com um predefinido nulo.

[  **@credential_name =** ] 'credential_name'  
O nome da base de dados no âmbito credenciais armazenadas nesta base de dados de controlo de tarefa que é utilizada para ligar a cada uma das bases de dados de destino dentro do grupo de destino, quando este passo é executado. credential_name é nvarchar(128).

[  **@target_group_name =** ] 'group_name de destino'  
O nome do grupo de destino que contém as bases de dados de destino que o passo de tarefa será executado no. target_group_name é nvarchar(128).

[  **@initial_retry_interval_seconds =** ] initial_retry_interval_seconds  
O atraso antes de repetir a primeira tentativa, se o passo de tarefa falhar durante a tentativa de execução inicial. initial_retry_interval_seconds é int, com o valor predefinido de 1.

[  **@maximum_retry_interval_seconds =** ] maximum_retry_interval_seconds  
O máximo de atraso entre tentativas de repetição. Se o atraso entre tentativas seria aumentar maior do que este valor,-lo em vez disso, é limitado a este valor. maximum_retry_interval_seconds é int, com o valor predefinido de 120.

[  **@retry_interval_backoff_multiplier =** ] retry_interval_backoff_multiplier  
O multiplicador para aplicar o atraso de repetição, se a tarefa de vários passo execução tenta falhar. Por exemplo, se a primeira novamente tinha um atraso de 5 segundos e o multiplicador término 2.0, em seguida, o segundo novamente terão um atraso de 10 segundos e o terceiro novamente terá um atraso de 20 segundos. retry_interval_backoff_multiplier é reais, com o valor predefinido de 2.0.

[  **@retry_attempts =** ] retry_attempts  
O número de vezes para repetir a execução se a tentativa inicial falhar. Por exemplo, se o valor de retry_attempts for 10, existirão 1 inicial tentativa e 10 tentativas de repetição, dar um total de tentativas de 11. Se a tentativa de repetição final falhar, irá terminar a execução da tarefa com um ciclo de vida de falha. retry_attempts é int, com o valor predefinido de 10.

[  **@step_timeout_seconds =** ] step_timeout_seconds  
A quantidade máxima de tempo permitido para o passo para executar. Se for excedida desta vez, irá terminar a execução da tarefa com um ciclo de vida do ServiceHost. step_timeout_seconds é int, com o valor predefinido de 43.200 segundos (12 horas).

[  **@output_type =** ] 'output_type'  
Se não for nulo, o tipo de destino que definir resultado primeiro o comando é escrito. Repor o valor de output_type nulo, defina o valor deste parâmetro como ' (uma cadeia vazia). output_type é nvarchar(50), com um predefinido nulo.

Se for especificado, o valor tem de ser SqlDatabase.

[  **@output_credential_name =** ] 'output_credential_name'  
Se não nula, o nome da base de dados credencial com âmbito que é utilizada para ligar à base de dados de destino de saída. Tem de ser especificado se output_type é igual a SqlDatabase. Repor o valor de output_credential_name nulo, defina o valor deste parâmetro como ' (uma cadeia vazia). output_credential_name é nvarchar(128), com um valor predefinido nulo.

[  **@output_server_name =** ] 'output_server_name'  
Se não for nulo, o nome DNS completamente qualificado do servidor que contém a base de dados de destino de saída. Tem de ser especificado se output_type é igual a SqlDatabase. Repor o valor de output_server_name nulo, defina o valor deste parâmetro como ' (uma cadeia vazia). output_server_name é nvarchar(256), com um predefinido nulo.

[  **@output_database_name =** ] 'output_database_name'  
Se não for nulo, o nome da base de dados que contém a tabela de destino de saída. Tem de ser especificado se output_type é igual a SqlDatabase. Repor o valor de output_database_name nulo, defina o valor deste parâmetro como ' (uma cadeia vazia). output_database_name é nvarchar(128), com um predefinido nulo.

[  **@output_schema_name =** ] 'output_schema_name'  
Se não for nulo, o nome do esquema SQL que contém a tabela de destino de saída. Se output_type for igual a SqlDatabase, o valor predefinido é dbo. Repor o valor de output_schema_name nulo, defina o valor deste parâmetro como ' (uma cadeia vazia). output_schema_name é nvarchar(128).

[  **@output_table_name =** ] 'output_table_name'  
Se não for nulo, o nome da tabela que o resultado do comando de primeiro definido será escrito. Se a tabela já não existir, será criado com base no esquema do conjunto de resultados de devolver. Tem de ser especificado se output_type é igual a SqlDatabase. Repor o valor de output_server_name nulo, defina o valor deste parâmetro como ' (uma cadeia vazia). output_table_name é nvarchar(128), com um valor predefinido nulo.

[  **@job_version =** ] job_version saída  
Parâmetro de saída que será atribuído o novo número de versão da tarefa. job_version é int.

[  **@max_parallelism =** ] max_parallelism saída  
O nível máximo de paralelismo por agrupamento elástico. Se o conjunto, em seguida, o passo de tarefa serão restritas apenas com um máximo de que muitas bases de dados por conjunto elástico. Isto aplica-se cada agrupamento elástico que diretamente está incluído no grupo de destino ou está dentro de um servidor que está incluído no grupo de destino. Para repor o valor de max_parallelism nulo, defina o valor deste parâmetro como -1. max_parallelism é int.


#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
As execuções de em curso da tarefa não serão afetadas. Quando sp_update_jobstep for bem sucedida, o número de versão da tarefa é incrementado. Da próxima vez que a tarefa for executada, será utilizada a nova versão.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:

- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores




### <a name="spdeletejobstep"></a>sp_delete_jobstep

Remove um passo de tarefa uma tarefa.

#### <a name="syntax"></a>Sintaxe


```sql
[jobs].sp_delete_jobstep [ @job_name = ] 'job_name'   
     [ , [ @step_id = ] step_id ]
     [ , [ @step_name = ] 'step_name' ]   
     [ , [ @job_version = ] job_version OUTPUT ]
```

#### <a name="arguments"></a>Argumentos
[  **@job_name =** ] 'job_name'  
O nome da tarefa de que o passo será removido. job_name é nvarchar(128), com sem predefinição.

[  **@step_id =** ] step_id  
O número de identificação para o passo de tarefa a ser eliminada. Step_id ou step_name tem de ser especificado. step_id é um inteiro.

[  **@step_name =** ] 'step_name'  
O nome do passo será eliminada. Step_id ou step_name tem de ser especificado. step_name é nvarchar(128).

[  **@job_version =** ] job_version saída  
Parâmetro de saída que será atribuído o novo número de versão da tarefa. job_version é int.

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
As execuções de em curso da tarefa não serão afetadas. Quando sp_update_jobstep for bem sucedida, o número de versão da tarefa é incrementado. Da próxima vez que a tarefa for executada, será utilizada a nova versão.

Os outros passos de tarefa irão ser automaticamente renumbered para preencher o intervalo restante, o passo de tarefa foi eliminado.
 
#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.






### <a name="spstartjob"></a>sp_start_job

Começar a ser executada uma tarefa.

#### <a name="syntax"></a>Sintaxe


```sql
[jobs].sp_start_job [ @job_name = ] 'job_name'   
     [ , [ @job_execution_id = ] job_execution_id OUTPUT ]   
```

#### <a name="arguments"></a>Argumentos
[  **@job_name =** ] 'job_name'  
O nome da tarefa de que o passo será removido. job_name é nvarchar(128), com sem predefinição.

[  **@job_execution_id =** ] job_execution_id saída  
O parâmetro que será atribuído o id de execução da tarefa de saída. job_version é uniqueidentifier.

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Nenhum.
 
#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.

### <a name="spstopjob"></a>sp_stop_job

Interrompe a execução da tarefa.

#### <a name="syntax"></a>Sintaxe


```sql
[jobs].sp_stop_job [ @job_execution_id = ] ' job_execution_id '
```


#### <a name="arguments"></a>Argumentos
[  **@job_execution_id =** ] job_execution_id  
O número de identificação de execução da tarefa para parar. job_execution_id é uniqueidentifier, com predefinido nulo.

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Nenhum.
 
#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.


### <a name="spaddtargetgroup"></a>sp_add_target_group

Adiciona um grupo de destino.

#### <a name="syntax"></a>Sintaxe


```sql
[jobs].sp_add_target_group [ @target_group_name = ] 'target_group_name'   
     [ , [ @target_group_id = ] target_group_id OUTPUT ]
```


#### <a name="arguments"></a>Argumentos
[  **@target_group_name =** ] 'target_group_name'  
O nome do grupo de destino para criar. target_group_name é nvarchar(128), com sem predefinição.

[  **@target_group_id =** ] target_group_id saída do destino grupo número de identificação atribuído à tarefa se criada com êxito. target_group_id é uma variável de saída do tipo uniqueidentifier, com um predefinido nulo.

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Os grupos de destino fornecem uma forma fácil de uma tarefa de uma coleção de bases de dados de destino.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.

### <a name="spdeletetargetgroup"></a>sp_delete_target_group

Elimina um grupo de destino.

#### <a name="syntax"></a>Sintaxe


```sql
[jobs].sp_delete_target_group [ @target_group_name = ] 'target_group_name'
```


#### <a name="arguments"></a>Argumentos
[  **@target_group_name =** ] 'target_group_name'  
O nome do grupo de destino para eliminar. target_group_name é nvarchar(128), com sem predefinição.

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Nenhum.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.

### <a name="spaddtargetgroupmember"></a>sp_add_target_group_member

Adiciona uma base de dados ou grupo de bases de dados a um grupo de destino.

#### <a name="syntax"></a>Sintaxe

```sql
[jobs].sp_add_target_group_member [ @target_group_name = ] 'target_group_name'
         [ @membership_type = ] ‘membership_type’ ]   
        [ , [ @target_type = ] ‘target_type’ ]   
        [ , [ @refresh_credential_name = ] ‘refresh_credential_name’ ]   
        [ , [ @server_name = ] ‘server_name’ ]   
        [ , [ @database_name = ] ‘database_name’ ]   
        [ , [ @elastic_pool_name = ] ‘elastic_pool_name’ ]   
        [ , [ @shard_map_name = ] ‘shard_map_name’ ]   
        [ , [ @target_id = ] ‘target_id’ OUTPUT ]
```

#### <a name="arguments"></a>Argumentos
[  **@target_group_name =** ] 'target_group_name'  
O nome do grupo de destino para os quais o membro será adicionado. target_group_name é nvarchar(128), com sem predefinição.

[  **@membership_type =** ] 'membership_type'  
Especifica se o membro do grupo de destino será incluído ou excluído. target_group_name é nvarchar(128), com a predefinição de 'Include'. Os valores válidos para target_group_name são 'Include' ou 'Excluir'.

[  **@target_type =** ] 'target_type'  
O tipo de base de dados de destino ou coleção de bases de dados, incluindo todas as bases de dados num servidor, todas as bases de dados num conjunto elástico, todas as bases de dados no mapa de partições horizontais ou uma base de dados individuais. target_type é nvarchar(128), com sem predefinição. Os valores válidos para target_type são 'SqlServer', 'SqlElasticPool', 'SqlDatabase' ou 'SqlShardMap'. 

[  **@refresh_credential_name =** ] 'refresh_credential_name'  
O nome do servidor lógico. refresh_credential_name é nvarchar(128), com sem predefinição.

[  **@server_name =** ] 'server_name'  
O nome do servidor lógico que deve ser adicionado ao grupo de destino especificado. SERVER_NAME deve ser especificado quando target_type 'SqlServer'. SERVER_NAME é nvarchar(128), com sem predefinição.

[  **@database_name =** ] 'database_name'  
O nome da base de dados que deve ser adicionado ao grupo de destino especificado. database_name deve ser especificado quando target_type 'SqlDatabase'. database_name é nvarchar(128), com sem predefinição.

[  **@elastic_pool_name =** ] 'elastic_pool_name'  
O nome do conjunto elástico que deve ser adicionado ao grupo de destino especificado. elastic_pool_name deve ser especificado quando target_type 'SqlElasticPool'. elastic_pool_name é nvarchar(128), com sem predefinição.

[  **@shard_map_name =** ] 'shard_map_name'  
O nome do agrupamento de mapa de partições horizontais que deve ser adicionado ao grupo de destino especificado. elastic_pool_name deve ser especificado quando target_type 'SqlSqlShardMap'. shard_map_name é nvarchar(128), com sem predefinição.

[  **@target_id =** ] target_group_id saída  
O número de identificação de destino atribuído para o membro do grupo de destino, se criou adicionada ao grupo de destino. target_id é uma variável de saída do tipo uniqueidentifier, com um predefinido nulo.
Devolver valores de código 0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Executa uma tarefa em todas as bases de dados dentro de um servidor ou o conjunto elástico em tempo de execução, quando um servidor lógico ou o conjunto elástico está incluído no grupo de destino.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.

#### <a name="examples"></a>Exemplos
O exemplo seguinte adiciona todas as bases de dados em servidores de Londres e NewYork ao grupo de servidores manter informações de cliente. Tem de ligar à base de dados tarefas especificado ao criar o agente de tarefa, neste caso ElasticJobs.

```sql
--Connect to the jobs database specified when creating the job agent
USE ElasticJobs ; 
GO

-- Add a target group containing server(s)
EXEC jobs.sp_add_target_group @target_group_name =  N'Servers Maintaining Customer Information'
GO

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'London.database.windows.net' ;
GO

-- Add a server target member
EXEC jobs.sp_add_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_type = N'SqlServer',
@refresh_credential_name=N'mymastercred', --credential required to refresh the databases in server
@server_name=N'NewYork.database.windows.net' ;
GO

--View the recently added members to the target group
SELECT * FROM [jobs].target_group_members WHERE target_group_name= N'Servers Maintaining Customer Information';
GO
```

### <a name="spdeletetargetgroupmember"></a>sp_delete_target_group_member

Remove um membro do grupo de destino um grupo de destino.

#### <a name="syntax"></a>Sintaxe


```sql
[jobs].sp_delete_target_group_member [ @target_group_name = ] 'target_group_name'
        [ , [ @target_id = ] ‘target_id’]
```



Argumentos [ @target_group_name =] 'target_group_name'  
O nome do grupo de destino a partir dos quais pretende remover o membro do grupo de destino. target_group_name é nvarchar(128), com sem predefinição.

[ @target_id =] target_id  
 O número de identificação de destino atribuído para o membro do grupo de destino a ser removido. target_id é um uniqueidentifier, com um predefinido nulo.

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou 1 (falha)

#### <a name="remarks"></a>Observações
Os grupos de destino fornecem uma forma fácil de uma tarefa de uma coleção de bases de dados de destino.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.

#### <a name="examples"></a>Exemplos
O exemplo a seguir remove o servidor de Londres do grupo de servidores manter informações de cliente. Tem de ligar à base de dados tarefas especificado ao criar o agente de tarefa, neste caso ElasticJobs.

```sql
--Connect to the jobs database specified when creating the job agent
USE ElasticJobs ; 
GO

-- Retrieve the target_id for a target_group_members
declare @tid uniqueidentifier
SELECT @tid = target_id FROM [jobs].target_group_members WHERE target_group_name = 'Servers Maintaining Customer Information' and server_name = 'London.database.windows.net'

-- Remove a target group member of type server
EXEC jobs.sp_delete_target_group_member
@target_group_name = N'Servers Maintaining Customer Information',
@target_id = @tid
GO
```

### <a name="sppurgejobhistory"></a>sp_purge_jobhistory

Remove os registos do histórico para uma tarefa.

#### <a name="syntax"></a>Sintaxe


```sql
[jobs].sp_purge_jobhistory [ @job_name = ] 'job_name'   
      [ , [ @job_id = ] job_id ]
      [ , [ @oldest_date = ] oldest_date []
```

#### <a name="arguments"></a>Argumentos
[  **@job_name =** ] 'job_name'  
O nome da tarefa para o qual pretende eliminar os registos do histórico. job_name é nvarchar(128), com um predefinido nulo. Job_id ou job_name tem de ser especificado, mas não não possível especificar ambos.

[  **@job_id =** ] job_id  
 O número de identificação de trabalho da tarefa para os registos a eliminar. job_id é uniqueidentifier, com um predefinido nulo. Job_id ou job_name tem de ser especificado, mas não não possível especificar ambos.

[  **@oldest_date =** ] oldest_date  
 O registo mais antigo para manter no histórico. oldest_date é DATETIME2, com um predefinido nulo. Quando é especificado oldest_date, sp_purge_jobhistory remove apenas registos que são mais antigos do que o valor especificado.

#### <a name="return-code-values"></a>Valores de código de retorno
0 (êxito) ou grupos de destino de observações 1 (falha) fornecem uma forma fácil de uma tarefa de uma coleção de bases de dados de destino.

#### <a name="permissions"></a>Permissões
Por predefinição, os membros da função de servidor fixa sysadmin podem executar este procedimento armazenado. A restringir um utilizador apenas poderá monitorizar tarefas, pode conceder ao utilizador como parte da função de base de dados seguintes na base de dados de agente tarefa, especificado ao criar o agente de tarefa:
- jobs_reader

Para obter detalhes sobre as permissões de uma destas funções, consulte a secção de permissão neste documento. Apenas os membros da sysadmin podem utilizar este procedimento armazenado para editar os atributos de tarefas que são proprietário por outros utilizadores.

#### <a name="examples"></a>Exemplos
O exemplo seguinte adiciona todas as bases de dados em servidores de Londres e NewYork ao grupo de servidores manter informações de cliente. Tem de ligar à base de dados tarefas especificado ao criar o agente de tarefa, neste caso ElasticJobs.

```sql
--Connect to the jobs database specified when creating the job agent

EXEC sp_delete_target_group_member   
    @target_group_name = N'Servers Maintaining Customer Information',  
    @server_name = N'London.database.windows.net';  
GO
```


## <a name="job-views"></a>Vistas de tarefa

As vistas seguintes estão disponíveis no [base de dados de tarefas](elastic-jobs-overview.md#job-database).


|Vista  |Descrição  |
|---------|---------|
|[jobs_executions](#jobsexecutions-view)     |  Mostra o histórico de execução da tarefa.      |
|[Tarefas](#jobs-view)     |   Mostra todas as tarefas.      |
|[job_versions](#jobversions-view)     |   Mostra todas as versões de tarefa.      |
|[passos](#jobsteps-view)     |     Mostra todos os passos na versão atual de cada tarefa.    |
|[jobstep_versions](#jobstepversions-view)     |     Mostra todos os passos em todas as versões de cada tarefa.    |
|[target_groups](#targetgroups-view)     |      Mostra todos os grupos de destino.   |
|[target_group_members](#targetgroups-view)     |   Mostra todos os membros de todos os grupos de destino.      |


### <a name="jobsexecutions-view"></a>Vista de jobs_executions

[tarefas]. [jobs_executions]

Mostra o histórico de execução da tarefa.


|Nome da coluna|   Tipo de dados   |Descrição|
|---------|---------|---------|
|**job_execution_id**   |uniqueidentifier|  ID exclusivo de uma instância de execução da tarefa.
|**job_name**   |nvarchar(128)  |Nome da tarefa.
|**job_id** |uniqueidentifier|  ID exclusivo da tarefa.
|**job_version**    |Int    |Versão da tarefa (automaticamente atualizada sempre que a tarefa é modificada).
|**step_id**    |Int|   Identificador exclusivo (para esta tarefa) para o passo. NULO indica que esta é a execução da tarefa principal.
|**is_active**| bits |Indica se as informações estão ativos ou inativos. 1 indica tarefas ativas e 0 indica inativa.
|**Ciclo de vida**| nvarchar(50)|Valor que indica o estado da tarefa: 'Criado', "Em curso", "Falha", 'Foi concluída com êxito', 'Ignorado', 'SucceededWithSkipped'|
|**create_time**|   datetime2(7)|   Data e hora em que a tarefa foi criada.
|**start_time** |datetime2(7)|  Data e hora da execução da tarefa foi iniciada. NULO se ainda não foi executada a tarefa.
|**end_time**|  datetime2(7)    |Data e hora em que a tarefa Terminar execução. Um valor nulo se a tarefa ainda não foi executada ou não foi ainda concluiu a execução.
|**current_attempts**   |Int    |Número de vezes que o passo foi tentada novamente. Tarefa principal será 0, execuções de tarefa subordinada vai ser 1 ou superior com base na política de execução.
|**current_attempt_start_time** |datetime2(7)|  Data e hora da execução da tarefa foi iniciada. NULO indica que esta é a execução da tarefa principal.
|**last_message**   |nvarchar (Max)| Mensagem de histórico do trabalho ou passo. 
|**target_type**|   nvarchar(128)   |Tipo de base de dados de destino ou coleção de bases de dados, incluindo todas as bases de dados no servidor, todas as bases de dados num conjunto elástico ou uma base de dados. Os valores válidos para target_type são 'SqlServer', 'SqlElasticPool' ou 'SqlDatabase'. NULO indica que esta é a execução da tarefa principal.
|**target_id**  |uniqueidentifier|  ID exclusivo do membro do grupo de destino.  NULO indica que esta é a execução da tarefa principal.
|**target_group_name**  |nvarchar(128)  |Nome do grupo de destino. NULO indica que esta é a execução da tarefa principal.
|**target_server_name**|    nvarchar(256)|  Nome do servidor lógico contido no grupo de destino. Especificado apenas se target_type é 'SqlServer'. NULO indica que esta é a execução da tarefa principal.
|**target_database_name**   |nvarchar(128)| Nome da base de dados contido no grupo de destino. Especificar o apenas quando target_type é 'SqlDatabase'. NULO indica que esta é a execução da tarefa principal.


### <a name="jobs-view"></a>Vista de tarefas

[tarefas]. [tarefas]

Mostra todas as tarefas.

|Nome da coluna|   Tipo de dados|  Descrição|
|------|------|-------|
|**job_name**|  nvarchar(128)   |Nome da tarefa.|
|**job_id**|    uniqueidentifier    |ID exclusivo da tarefa.|
|**job_version**    |Int    |Versão da tarefa (automaticamente atualizada sempre que a tarefa é modificada).|
|**Descrição**    |nvarchar(512)| Descrição da tarefa. bits ativado indica se a tarefa está ativada ou desativada. 1 indica tarefas ativadas e 0 indica tarefas desativadas.|
|**schedule_interval_type** |nvarchar(50)   |Valor que indica quando a tarefa é para ser executada: 'Uma vez', 'Minutos', 'Horas', 'Dias', 'Semanas', 'Meses'
|**schedule_interval_count**|   Int|    Número de períodos de schedule_interval_type ocorra entre cada execução da tarefa.|
|**schedule_start_time**    |datetime2(7)|  Data e hora em que a tarefa foi a última execução foi iniciada.|
|**schedule_end_time**| datetime2(7)|   Data e hora em que a tarefa foi a última execução foi concluída.|


### <a name="jobversions-view"></a>Vista de job_versions

[tarefas]. [job_verions]

Mostra todas as versões de tarefa.

|Nome da coluna|   Tipo de dados|  Descrição|
|------|------|-------|
|**job_name**|  nvarchar(128)   |Nome da tarefa.|
|**job_id**|    uniqueidentifier    |ID exclusivo da tarefa.|
|**job_version**    |Int    |Versão da tarefa (automaticamente atualizada sempre que a tarefa é modificada).|


### <a name="jobsteps-view"></a>Vista de passos

[tarefas]. [passos]

Mostra todos os passos na versão atual de cada tarefa.

|Nome da coluna    |Tipo de dados| Descrição|
|------|------|-------|
|**job_name**   |nvarchar(128)| Nome da tarefa.|
|**job_id** |uniqueidentifier   |ID exclusivo da tarefa.|
|**job_version**|   Int|    Versão da tarefa (automaticamente atualizada sempre que a tarefa é modificada).|
|**step_id**    |Int    |Identificador exclusivo (para esta tarefa) para o passo.|
|**step_name**  |nvarchar(128)  |(Para esta tarefa) exclusivo para o passo.|
|**command_type**   |nvarchar(50)   |Tipo de comando a executar o passo de tarefa. Para v1, valor tem de ser igual ao e por predefinição 'TSql'.|
|**command_source** |nvarchar(50)|  Localização do comando. Para v1, 'Inline' é a predefinição e apenas aceite o valor.|
|**Comando**|   nvarchar (Max)|  Os comandos para ser executado por tarefas elásticas através de command_type.|
|**credential_name**|   nvarchar(128)   |Nome da credencial com âmbito de base de dados utilizado para a execução da tarefa.|
|**target_group_name**| nvarchar(128)   |Nome do grupo de destino.|
|**target_group_id**|   uniqueidentifier|   ID exclusivo do grupo de destino.|
|**initial_retry_interval_seconds**|    Int |O atraso antes da primeira tentativa de repetição. Valor predefinido é 1.|
|**maximum_retry_interval_seconds** |Int|   O máximo de atraso entre tentativas de repetição. Se o atraso entre tentativas seria aumentar maior do que este valor,-lo em vez disso, é limitado a este valor. Valor predefinido é de 120.|
|**retry_interval_backoff_multiplier**  |real|  O multiplicador para aplicar o atraso de repetição, se a tarefa de vários passo execução tenta falhar. Valor predefinido é a 2.0.|
|**retry_attempts** |Int|   O número de repetições tenta utilizar caso este passo falhe. Predefinição de 10, que indica sem as tentativas de repetição.|
|**step_timeout_seconds**   |Int|   A quantidade de tempo em minutos entre tentativas de repetição. A predefinição é 0, o que indica um intervalo de 0 minutos.|
|**output_type**    |nvarchar(11)|  Localização do comando. Pré-visualização atual, 'Inline' é a predefinição e apenas aceite o valor.|
|**output_credential_name**|    nvarchar(128)   |Definir o nome das credenciais para ser utilizado para ligar ao servidor de destino para armazenar os resultados.|
|**output_subscription_id**|    uniqueidentifier|   ID exclusivo da subscrição de server\database o destino para os resultados definida a partir da execução da consulta.|
|**output_resource_group_name** |nvarchar(128)| Nome do grupo de recursos onde reside o servidor de destino.|
|**output_server_name**|    nvarchar(256)   |Nome do servidor de destino para o conjunto de resultados.|
|**output_database_name**   |nvarchar(128)| Nome da base de dados de destino para o conjunto de resultados.|
|**output_schema_name** |nvarchar (Max)| Nome do esquema de destino. Por predefinição, dbo, se não for especificado.|
|**output_table_name**| nvarchar (Max)|  Nome da tabela para armazenar os resultados de conjunto de resultados da consulta. Tabela será criada automaticamente com base no esquema de resultados definido se ainda não existir. Esquema têm de corresponder o esquema do conjunto de resultados.|
|**max_parallelism**|   Int|    O número máximo de bases de dados por conjunto elástico que o passo de tarefa será executado cada vez. A predefinição é nulo, o que significa que nenhum limite. |


### <a name="jobstepversions-view"></a>Vista de jobstep_versions

[tarefas]. [jobstep_versions]

Mostra todos os passos em todas as versões de cada tarefa. O esquema é idêntico ao [passos](#jobsteps-view).

### <a name="targetgroups-view"></a>Vista de target_groups

[tarefas]. [target_groups]

Apresenta uma lista de todos os grupos de destino.

|Nome da coluna|Tipo de dados| Descrição|
|-----|-----|-----|
|**target_group_name**| nvarchar(128)   |O nome do grupo de destino, numa coleção de bases de dados. 
|**target_group_id**    |uniqueidentifier   |ID exclusivo do grupo de destino.

### <a name="targetgroupsmembers-view"></a>Vista de target_groups_members

[tarefas]. [target_groups_members]

Mostra todos os membros de todos os grupos de destino.

|Nome da coluna|Tipo de dados| Descrição|
|-----|-----|-----|
|**target_group_name**  |nvarchar (128|O nome do grupo de destino, numa coleção de bases de dados. |
|**target_group_id**    |uniqueidentifier   |ID exclusivo do grupo de destino.|
|**membership_type**    |Int|   Especifica se o membro do grupo de destino é incluído ou excluído do grupo de destino. Os valores válidos para target_group_name são 'Include' ou 'Excluir'.|
|**target_type**    |nvarchar(128)| Tipo de base de dados de destino ou coleção de bases de dados, incluindo todas as bases de dados no servidor, todas as bases de dados num conjunto elástico ou uma base de dados. Os valores válidos para target_type são 'SqlServer', 'SqlElasticPool', 'SqlDatabase' ou 'SqlShardMap'.|
|**target_id**  |uniqueidentifier|  ID exclusivo do membro do grupo de destino.|
|**refresh_credential_name**    |nvarchar(128)  |Nome da base de dados confinadas a credencial utilizada para estabelecer ligação com o membro do grupo de destino.|
|**subscription_id**    |uniqueidentifier|  ID exclusivo da subscrição.|
|**resource_group_name**    |nvarchar(128)| Nome do grupo de recursos em que reside o membro do grupo de destino.|
|**SERVER_NAME**    |nvarchar(128)  |Nome do servidor lógico contido no grupo de destino. Especificado apenas se target_type é 'SqlServer'. |
|**database_name**  |nvarchar(128)  |Nome da base de dados contido no grupo de destino. Especificar o apenas quando target_type é 'SqlDatabase'.|
|**elastic_pool_name**  |nvarchar(128)| Nome do conjunto elástico contido no grupo de destino. Especificar o apenas quando target_type é 'SqlElasticPool'.|
|**shard_map_name** |nvarchar(128)| Nome do mapa de partições horizontais contido no grupo de destino. Especificar o apenas quando target_type é 'SqlShardMap'.|


## <a name="resources"></a>Recursos

 - ![Ícone de ligação de tópico](https://docs.microsoft.com/sql/database-engine/configure-windows/media/topic-link.gif "ícone de ligação de tópico") [convenções de sintaxe Transact-SQL](/sql/t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  


## <a name="next-steps"></a>Passos Seguintes

- [Criar e gerir as tarefas elásticas com o PowerShell](elastic-jobs-powershell.md)
- [Autorização e permissões do SQL Server](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/authorization-and-permissions-in-sql-server)
  