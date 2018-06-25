---
title: Base de dados SQL do Azure gerida diferenças de T-SQL de instância | Microsoft Docs
description: Este artigo aborda as diferenças de T-SQL entre a base de dados geridas por instância de SQL do Azure e o SQL Server.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab, bonova
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: 95eca05d695e039f59b71caa4d730f4e1f84fc97
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337948"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Diferenças de SQL da base de dados geridos instância T-SQL do Azure do SQL Server 

Instância do Azure SQL da base de dados geridos (pré-visualização) fornece elevada compatibilidade com o motor de base de dados de servidor de SQL no local. A maioria das funcionalidades do motor de base de dados do SQL Server é suportada na instância geridos. Uma vez que ainda existem algumas diferenças na sintaxe e o comportamento, este artigo resume e explica estas diferenças.
 - [Diferenças de T-SQL e funcionalidades não suportadas](#Differences)
 - [Funcionalidades que têm um comportamento diferente na instância geridos](#Changes)
 - [Limitações temporárias e problemas conhecidos](#Issues)

## <a name="Differences"></a> Diferenças de T-SQL do SQL Server 

Esta secção resume as principais diferenças na sintaxe de T-SQL e o comportamento entre geridos instância e o motor de base de dados de servidor de SQL no local, bem como funcionalidades não suportadas.

### <a name="always-on-availability"></a>Disponibilidade Always On

[Elevada disponibilidade](sql-database-high-availability.md) está incorporado na instância geridos e não pode ser controlado pelos utilizadores. Não são suportadas as seguintes instruções:
 - [CRIE O PONTO FINAL... PARA DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
 - [CRIAR GRUPO DE DISPONIBILIDADE](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
 - [GRUPO DE DISPONIBILIDADE DE ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
 - [GRUPO DE DISPONIBILIDADE DE LARGAR](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
 - [Definir HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) cláusula o [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) declaração

### <a name="auditing"></a>Auditoria 
 
As principais diferenças entre a auditoria de SQL na instância geridos, SQL Database do Azure e SQL Server no local são:
- Na instância geridos, a auditoria de SQL funciona no nível do servidor e armazena `.xel` conta de armazenamento de BLOBs de ficheiros no Azure.  
- Base de dados SQL do Azure, a auditoria de SQL funciona ao nível da base de dados.
- No SQL Server no local / máquina virtual, a auditoria de SQL funciona ao nível do servidor, mas armazena eventos em registos de eventos do windows/sistema de ficheiros.  
  
Auditoria XEvent na instância geridos suporta destinos de armazenamento de Blobs do Azure. Os registos do ficheiro e o windows não são suportados.    
 
A chave de diferenças no `CREATE AUDIT` sintaxe de auditoria para o armazenamento de Blobs do Azure são:
- Uma nova sintaxe `TO URL` é fornecida e permite-lhe especificar o URL do contentor de armazenamento de Blobs do Azure onde `.xel` ficheiros irão ser colocados 
- A sintaxe `TO FILE` não é suportada porque a instância geridos não é possível aceder a partilhas de ficheiros do Windows. 
 
Para obter mais informações, consulte:  
- [CRIAR A AUDITORIA DO SERVIDOR](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql) 
- [Auditoria](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)     

### <a name="backup"></a>Cópia de segurança 

Instância gerida tem cópias de segurança automáticas e permite aos utilizadores criar base de dados completa `COPY_ONLY` cópias de segurança. Valor diferencial, registo e cópias de segurança de instantâneos de ficheiros não são suportadas.  
- Instância gerida pode fazer cópias de segurança uma base de dados apenas uma conta do Blob Storage do Azure: 
 - Apenas `BACKUP TO URL` é suportada 
 - `FILE`, `TAPE`, e os dispositivos de cópia de segurança não são suportados  
- Maioria dos geral `WITH` opções são suportadas 
 - `COPY_ONLY` é obrigatório
 - `FILE_SNAPSHOT` Não é suportado  
 - As opções da banda: `REWIND`, `NOREWIND`, `UNLOAD`, e `NOUNLOAD` não são suportados 
 - Opções de registo específicos: `NORECOVERY`, `STANDBY`, e `NO_TRUNCATE` não são suportados 
 
Limitações:  
- Instância gerida pode fazer cópias de segurança uma base de dados para uma cópia de segurança com até 32 reparte, que é suficiente para as bases de dados até 4 TB se for utilizada a compressão de cópias de segurança.
- Tamanho máximo de cópia de segurança stripe é 195 GB (tamanho do blob máximo). Aumente o número de reparte no comando de cópia de segurança para reduzir o tamanho do stripe individuais e permanecer neste limite. 

> [!TIP]
> Para contornar esta limitação no local, a cópia de segurança para `DISK` em vez de cópia de segurança `URL`, carregar o ficheiro de cópia de segurança para o blob, em seguida, restaurar. Restaure ficheiros maiores suporta porque é utilizado um tipo de blob diferente.  

Para obter informações sobre cópias de segurança utilizando o T-SQL, consulte [cópia de segurança](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

### <a name="buffer-pool-extension"></a>Extensão do conjunto de memória intermédia 
 
- [A memória intermédia de extensão do conjunto](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) não é suportada.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` Não é suportada. Consulte [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql). 
 
### <a name="bulk-insert--openrowset"></a>Inserção em massa / openrowset

Instância gerida não é possível aceder a partilhas de ficheiros e pastas do Windows, pelo que os ficheiros devem ser importados a partir do blob storage do Azure.
- `DATASOURCE` é necessário no `BULK INSERT` comando ao importar os ficheiros do armazenamento de Blobs do Azure. Consulte [inserção em massa](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` é necessário no `OPENROWSET` funcionar ao ler um conteúdo de um ficheiro de armazenamento de Blobs do Azure. Consulte [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
 
### <a name="certificates"></a>Certificados 

Instância gerida não é possível aceder a partilhas de ficheiros e pastas do Windows, pelo que são aplicáveis as seguintes restrições: 
- `CREATE FROM`/`BACKUP TO` Não é suportado o ficheiro de certificados
- `CREATE`/`BACKUP` certificado do `FILE` / `ASSEMBLY` não é suportada. Ficheiros de chave privados não podem ser utilizados.  
 
Consulte [Criar certificado](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) e [certificados de cópia de segurança](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
> [!TIP]
> Solução: chave privada do certificado do script, guarde como ficheiro. SQL e criar a partir do binário: 
> 
> ``` 
CREATE CERTIFICATE  
 FROM BINARY = asn_encoded_certificate    
WITH PRIVATE KEY ( <private_key_options> ) 
>```   
 
### <a name="clr"></a>CLR 

Instância gerida não é possível aceder a partilhas de ficheiros e pastas do Windows, pelo que são aplicáveis as seguintes restrições: 
- Apenas `CREATE ASSEMBLY FROM BINARY` é suportada. Consulte [criar ASSEMBLAGEM de binário](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` Não é suportada. Consulte [criar ASSEMBLAGEM do ficheiro](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` Não é possível referenciar os ficheiros. Consulte [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).
 
### <a name="compatibility-levels"></a>Níveis de compatibilidade 
 
- Níveis de compatibilidade suportados são: 100, 110, 120, 130, 140  
- Níveis de compatibilidade inferior a 100 não são suportadas. 
- O nível de compatibilidade de predefinição para novas bases de dados é 140. Para bases de dados restauradas, nível de compatibilidade não serão alterados se esta tiver 100 e acima.

Consulte [nível de compatibilidade de base de dados ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).
 
### <a name="credential"></a>Credencial 
 
Apenas o Cofre de chaves do Azure e `SHARED ACCESS SIGNATURE` identidades são suportadas. Utilizadores do Windows não são suportados.
 
Consulte [criar CREDENCIAL](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) e [ALTER CREDENCIAL](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql). 
 
### <a name="cryptographic-providers"></a>Fornecedores de criptografia

Instância gerida não é possível aceder a ficheiros, para que Fornecedores de criptografia não não possível criar:
- `CREATE CRYPTOGRAPHIC PROVIDER` Não é suportada. Consulte [criar o fornecedor de CRIPTOGRAFIA](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` Não é suportada. Consulte [fornecedor de CRIPTOGRAFIA ALTER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql). 

### <a name="collation"></a>Agrupamento 
 
Agrupamento de servidores for `SQL_Latin1_General_CP1_CI_AS` e não pode ser alterada. Consulte [agrupamentos](https://docs.microsoft.com/sql/t-sql/statements/collations).
 
### <a name="database-options"></a>Opções de base de dados 
 
- Não são suportados vários ficheiros de registo. 
- Objetos em memória não são suportados na camada de serviço de objetivo geral.  
- Não há um limite de 280 ficheiros por cada instância implying 280 ficheiros máximas por base de dados. Ficheiros de registo e dados são contados para este limite.  
- Base de dados não pode conter os grupos de ficheiros que contém dados filestream.  Restauro falhará se contiver. bak `FILESTREAM` dados.  
- Todos os ficheiros é colocados no armazenamento do Azure Premium. E/s e débito por ficheiro dependem do tamanho de cada ficheiro individual, da mesma forma tal como para os discos Premium Storage do Azure. Consulte [desempenho de disco do Azure Premium](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)  
 
#### <a name="create-database-statement"></a>Instrução CREATE DATABASE

Seguem-se `CREATE DATABASE` limitações: 
- Ficheiros e grupos de ficheiros não podem ser definidos.  
- `CONTAINMENT` opção não é suportada.  
- `WITH`as opções não são suportadas.  
   > [!TIP]
   > Como solução, utilize `ALTER DATABASE` depois `CREATE DATABASE` para definir as opções de base de dados para adicionar ficheiros de ou para definir a contenção.  

- `FOR ATTACH` opção não é suportada 
- `AS SNAPSHOT OF` opção não é suportada 

Para obter mais informações, consulte [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Instrução ALTER DATABASE

Algumas propriedades de ficheiro não não possível definir ou alterar:
- Não é possível especificar o caminho do ficheiro no `ALTER DATABASE ADD FILE (FILENAME='path')` instrução T-SQL. Remover `FILENAME` do script porque gerida automaticamente instância coloca os ficheiros.  
- Nome de ficheiro não pode ser alterado utilizando `ALTER DATABASE` instrução.

As seguintes opções são definidas por predefinição e não podem ser alteradas: 
- `MULTI_USER` 
- `ENABLE_BROKER ON` 
- `AUTO_CLOSE OFF` 

Não não possível modificar as seguintes opções: 
- `AUTO_CLOSE` 
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)` 
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)` 
- `DISABLE_BROKER` 
- `EMERGENCY` 
- `ENABLE_BROKER` 
- `FILESTREAM` 
- `HADR`   
- `NEW_BROKER` 
- `OFFLINE` 
- `PAGE_VERIFY` 
- `PARTNER` 
- `READ_ONLY` 
- `RECOVERY BULK_LOGGED` 
- `RECOVERY_SIMPLE` 
- `REMOTE_DATA_ARCHIVE`  
- `RESTRICTED_USER` 
- `SINGLE_USER` 
- `WITNESS`

Modificar nome não é suportado.

Para obter mais informações, consulte [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="database-mirroring"></a>Espelhamento da base de dados

Não é suportado para o espelhamento da base de dados.
 - `ALTER DATABASE SET PARTNER` e `SET WITNESS` opções não são suportadas.
 - `CREATE ENDPOINT … FOR DATABASE_MIRRORING` Não é suportada.

Para obter mais informações, consulte [ALTER DATABASE SET PARTNER e SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) e [CREATE ENDPOINT... PARA DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="dbcc"></a>DBCC 
 
As instruções de DBCC sinalizadores que estão ativadas no SQL Server não são suportadas na instância geridos.
- `Trace Flags` não são suportadas. Consulte [sinalizadores de rastreio](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` Não é suportada. Consulte [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` Não é suportada. Consulte [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Transações distribuídas

Nenhum dos MSDTC nem [transações elástico](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-transactions-overview) atualmente são suportadas na instância geridos.

### <a name="extended-events"></a>Eventos Expandidos 

Alguns destinos específico do Windows para XEvents não são suportados:
- `etw_classic_sync target` Não é suportada. Arquivo `.xel` armazenamento de BLOBs de ficheiros no Azure. Consulte [etw_classic_sync destino](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target). 
- `event_file target`Não é suportada. Arquivo `.xel` armazenamento de BLOBs de ficheiros no Azure. Consulte [event_file destino](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#eventfile-target).

### <a name="external-libraries"></a>Bibliotecas externas

Na base de dados R e Python bibliotecas externas ainda não são suportadas. Consulte [máquina do SQL Server dos serviços de aprendizagem](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FileStream e Filetable

- dados FileStream não são suportados. 
- Base de dados não pode conter os grupos de ficheiros com `FILESTREAM` dados
- `FILETABLE` Não é suportada
- As tabelas não podem ter `FILESTREAM` tipos
- Não são suportadas as seguintes funções:
 - `GetPathLocator()` 
 - `GET_FILESTREAM_TRANSACTION_CONTEXT()` 
 - `PathName()` 
 - `GetFileNamespacePath()` 
 - `FileTableRootPath()` 

Para obter mais informações, consulte [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) e [FileTables](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Pesquisa em texto completo semântica

[Pesquisa semântica](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) não é suportada.

### <a name="linked-servers"></a>Servidores ligados
 
Servidores ligados na instância gerida suportam um número limitado de destinos: 
- Suportado destinos: SQL Server e base de dados SQL
- Não suportado destinos: ficheiros, Analysis Services e outros RDBMS.

Operações

- Transações de escrita de instância entre não são suportadas.
- `sp_dropserver` é suportada para remover um servidor ligado. Consulte [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` a função pode ser utilizada para executar consultas apenas em instâncias do SQL Server (o geridos no local, ou em máquinas virtuais). Consulte [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` a função pode ser utilizada para executar consultas apenas em instâncias do SQL Server (o geridos no local, ou em máquinas virtuais). Apenas `SQLNCLI`, `SQLNCLI11`, e `SQLOLEDB` valores são suportados como o fornecedor. Por exemplo: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Consulte [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).
 
### <a name="logins--users"></a>Inícios de sessão / utilizadores 

- Inícios de sessão do SQL Server criados `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, e `FROM SID` são suportados. Consulte [criar início de sessão](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Inícios de sessão do Windows criados com `CREATE LOGIN ... FROM WINDOWS` sintaxe não são suportadas.
- Tem de utilizador do Active Directory (Azure AD) do Azure que criou a instância [sem restrições privilégios de administrador](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#unrestricted-administrative-accounts).
- Os utilizadores de nível de base de dados do Azure Active Directory (Azure AD) administrador não podem ser criados utilizando `CREATE USER ... FROM EXTERNAL PROVIDER` sintaxe. Consulte [criar utilizador... DO FORNECEDOR EXTERNO](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#non-administrator-users)
 
### <a name="polybase"></a>Polybase

Não são suportadas tabelas externas referenciar os ficheiros no blob storage do HDFS ou do Azure. Para obter informações sobre o Polybase, consulte [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replicação 
 
Replicação ainda não é suportada. Para obter informações sobre a replicação, consulte [replicação do SQL Server](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication).
 
### <a name="restore-statement"></a>RESTAURAR declaração 
 
- Sintaxe suportado  
   - `RESTORE DATABASE` 
   - `RESTORE FILELISTONLY ONLY` 
   - `RESTORE HEADER ONLY` 
   - `RESTORE LABELONLY ONLY` 
   - `RESTORE VERIFYONLY ONLY` 
- Sintaxe não suportada 
   - `RESTORE LOG ONLY` 
   - `RESTORE REWINDONLY ONLY`
- Origem  
 - `FROM URL` (Armazenamento de Blobs do azure) é apenas suportada opção.
 - `FROM DISK`/`TAPE`/ dispositivos de cópia de segurança não é suportado.
 - Conjuntos de cópia de segurança não são suportados. 
- `WITH` não são suportadas opções (não `DIFFERENTIAL`, `STATS`, etc.)     
- `ASYNC RESTORE` -Restauro continua, mesmo se as quebras de ligação de cliente. Se a ligação for removida, pode verificar `sys.dm_operation_status` vista para o estado de uma operação de restauro (bem como para criar e LARGAR base de dados). Consulte [operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  
 
As seguintes opções de base de dados são o conjunto/substituída e não não possível alterar mais tarde:  
- `NEW_BROKER` (se mediador não está ativado no ficheiro. bak)  
- `ENABLE_BROKER` (se mediador não está ativado no ficheiro. bak)  
- `AUTO_CLOSE=OFF` (se tiver uma base de dados no ficheiro. bak `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (se tiver uma base de dados no ficheiro. bak `SIMPLE` ou `BULK_LOGGED` modo de recuperação)
- Grupo de ficheiros com otimização de memória é adicionado e chamado XTP se não estava no ficheiro. bak de origem  
- Qualquer grupo de ficheiros com otimização de memória é mudado para XTP  
- `SINGLE_USER` e `RESTRICTED_USER` opções são convertidas em `MULTI_USER`   
Limitações:  
- `.BAK` não não possível restaurar os ficheiros que contêm vários conjuntos de cópias de segurança. 
- `.BAK` não não possível restaurar os ficheiros que contém vários ficheiros de registo. 
- Restauro falhará se contiver. bak `FILESTREAM` dados.
- Atualmente não não possível restaurar cópias de segurança que contêm bases de dados com objetos do Active Directory dentro da memória.  
- Atualmente não não possível restaurar cópias de segurança que contêm bases de dados onde, a determinada altura objetos em memória existiam.   
- Atualmente não não possível restaurar cópias de segurança que contêm bases de dados no modo só de leitura. Esta limitação será removida em breve.   
 
Para obter informações sobre o restauro instruções, consulte [restaurar instruções](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Mediador de serviço 
 
- Mediador de serviço da instância em vários locais não é suportada 
 - `sys.routes` -Pré-requisito: selecione endereços sys.routes. Endereço tem de ser LOCAL em cada rota. Consulte [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
 - `CREATE ROUTE` -Não é possível `CREATE ROUTE` com `ADDRESS` diferente de `LOCAL`. Consulte [criar rota](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
 - `ALTER ROUTE` Não é possível `ALTER ROUTE` com `ADDRESS` diferente de `LOCAL`. Consulte [ALTER rota](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  
 
### <a name="service-key-and-service-master-key"></a>Chave de mestre de chave e serviço do serviço 
 
- [Cópia de segurança da chave mestra](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) não é suportada (que é gerido pelo serviço de base de dados SQL) 
- [Restauro da chave mestra](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) não é suportada (que é gerido pelo serviço de base de dados SQL) 
- [Cópia de segurança de chave mestra de serviço](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) não é suportada (que é gerido pelo serviço de base de dados SQL) 
- [Restauro da chave mestra de serviço](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) não é suportada (que é gerido pelo serviço de base de dados SQL) 
 
### <a name="stored-procedures-functions-triggers"></a>Os procedimentos armazenados, funções, acionadores 
 
- `NATIVE_COMPILATION` Não é atualmente suportada. 
- O seguinte [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) opções não são suportadas: 
 - `allow polybase export` 
 - `allow updates` 
 - `filestream_access_level` 
 - `max text repl size` 
 - `remote data archive` 
 - `remote proc trans` 
- `sp_execute_external_scripts` Não é suportada. Consulte [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` Não é suportada. Consulte [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` não são suportadas, incluindo `sp_addextendedproc` e `sp_dropextendedproc`. Consulte [expandido procedimentos armazenados](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`, e `sp_detach_db` não são suportadas. Consulte [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), e [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` Não é suportada. Consulte [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

### <a name="sql-server-agent"></a>Agente do SQL Server 
 
- Definições do agente do SQL Server são só de leitura. Procedimento `sp_set_agent_properties` não é suportada na instância geridos.  
- As tarefas - apenas os passos da tarefa de T-SQL são atualmente suportados (serão adicionados mais passos durante a pré-visualização pública).
 - SSIS ainda não é suportado. 
 - Replicação ainda não é suportada  
  - Leitor do registo de transações ainda não é suportado.  
  - Instantâneos ainda não é suportado.  
  - Distribuidor ainda não é suportado.  
  - Não é suportada a intercalação.  
  - Leitor de filas não é suportada.  
 - Shell de comandos ainda não é suportada. 
  - Instância gerida não é possível aceder a recursos externos (por exemplo, as partilhas de rede através de robocopy).  
 - PowerShell ainda não é suportado.
 - Não é suportado do Analysis Services.  
- As notificações parcialmente são suportadas.
 - Notificação por correio eletrónico é suportado, é necessário configurar um perfil de correio de base de dados. Pode haver perfil de correio de base de dados apenas um e tem de ser chamado `AzureManagedInstance_dbmail_profile` em pré-visualização pública (limitação temporária).  
 - Não é suportada paginação.  
 - NetSend não é suportada. 
 - Alertas de não ainda não são suportados.
 - Proxies não são suportadas.  
- Registo de eventos não é suportado. 
 
As seguintes funcionalidades não são atualmente suportadas, mas serão ativadas no futuro:  
- Proxies
- Agendar tarefas na CPU inativa 
- Ativar/desativar o agente
- Alertas

Para obter informações sobre o agente do SQL Server, consulte [do SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).
 
### <a name="tables"></a>Tabelas 

Os seguintes não são suportados: 
- `FILESTREAM` 
- `FILETABLE` 
- `EXTERNAL TABLE` 
- `MEMORY_OPTIMIZED`  

Para obter informações sobre como criar e alterar as tabelas, consulte [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) e [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).
 
## <a name="Changes"></a> Alterações de comportamento 
 
As seguintes variáveis, funções e vistas devolvem resultados diferentes:  
- `SERVERPROPERTY('EngineEdition')` Devolve o valor 8. Esta propriedade identifica exclusivamente geridos instância. Consulte [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` Devolve um valor nulo, porque o conceito de instância conforme existe para o SQL Server não é aplicável a instância geridos. Consulte [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` Devolve nome DNS completo 'conectável', por exemplo, os meus instance.wcus17662feb9ce98.database.windows.net geridos. Consulte [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -Devolve completa nome DNS de 'conectável', tal como `myinstance.domain.database.windows.net` para propriedades 'name' e 'data_source'. Consulte [SYS. SERVIDORES](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql). 
- `@@SERVICENAME` Devolve um valor nulo, porque o conceito de serviço conforme existe para o SQL Server não é aplicável a instância geridos. Consulte [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).   
- `SUSER_ID` é suportada. Devolve Nulo se o início de sessão do AAD não está a ser sys.syslogins. Consulte [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` Não é suportada. Devolve (temporário problema conhecido) de dados incorreto. Consulte [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 
- `GETDATE()` e outras funções de data/hora incorporada devolve sempre tempo fuso horário UTC. Consulte [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Limitações e problemas conhecidos

### <a name="tempdb-size"></a>Tamanho TEMPDB

`tempdb` é dividido em 12 14 GB de tamanho de ficheiros com máx. por ficheiro. Não é possível alterar este tamanho máximo por ficheiro e não não possível adicionar novos ficheiros `tempdb`. Esta limitação será removida em breve. Algumas consultas podem devolver um erro se precisar de mais do que 168 GB `tempdb`.

### <a name="exceeding-storage-space-with-small-database-files"></a>Que excede o espaço de armazenamento com ficheiros de base de dados pequena

Cada instância geridos tem cópias de segurança para armazenamento de 35 TB reservado para o espaço em disco do Azure Premium, e cada ficheiro de base de dados está colocado num disco físico separado. Tamanhos de disco podem ser 128 GB, 256 GB, 512 GB, 1 TB ou 4 TB. Não é cobrado o espaço não utilizado no disco, mas a soma total dos tamanhos de disco do Azure Premium não pode exceder 35 TB. Em alguns casos, uma instância geridos que não precisa de 8 TB no total pode exceder o limite de TB Azure do tamanho de armazenamento, devido à fragmentação interno de 35. 

Por exemplo, uma instância geridos ter um ficheiro com tamanho 1.2 TB que utiliza um disco de 4 TB e 248 ficheiros com 1 GB cada que são colocados em 248 discos com o tamanho de 128 GB. Neste exemplo, o tamanho de armazenamento total do disco é 1 x 4 TB + GB de 248 x 128 = 35 TB. No entanto, o tamanho total de instância reservado para bases de dados é 1 x 1.2 TB + GB de 248 1 = 1,4 TB. Isto ilustra que em determinados circunstância, devido a uma distribuição muito específica dos ficheiros, uma instância geridos poderá atingir o limite de armazenamento do disco Premium do Azure onde poderá não esperada. 

Não haveria nenhum erro nas bases de dados existentes e pode crescer sem qualquer problema se não foram adicionados novos ficheiros, mas as novas bases de dados não podem ser criados ou restaurar porque não existe espaço suficiente de unidades de disco novas, mesmo que o tamanho total de todas as bases de dados não atingir t a instância de limite de tamanho. Nesse caso o erro devolvido não está desmarcado.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>Restaurar a configuração incorreta da chave SAS durante a base de dados

`RESTORE DATABASE` que lê o ficheiro. bak poderá ser constantemente repetir a tentativa de ler o ficheiro. bak e o erro devolvido após longo período de tempo se assinatura de acesso partilhado no `CREDENTIAL` está incorreto. Execute RESTORE HEADERONLY antes de restaurar uma base de dados não se esqueça de que a chave SAS está correta.
Certifique-se de que remova à esquerda `?` partir da chave SAS gerada utilizando o portal do Azure.

### <a name="tooling"></a>As ferramentas

SQL Server Management Studio e SQL Server Data Tools podem ter alguns problemas ao aceder a instância geridos. Todas as ferramentas problemas serão resolvidos antes de disponibilidade geral.

### <a name="incorrect-database-names"></a>Nomes de base de dados incorreto

Instância gerida poderá mostrar o valor guid em vez do nome de base de dados durante o restauro ou em algumas mensagens de erro. Estes problemas serão ser corrigidos antes de disponibilidade geral.

### <a name="database-mail-profile"></a>Perfil de correio de base de dados
Pode haver perfil de correio de base de dados apenas um e tem de ser chamado `AzureManagedInstance_dbmail_profile`. Esta limitação é temporária que será removida em breve.

## <a name="next-steps"></a>Passos Seguintes

- Para obter detalhes sobre a instância geridos, consulte [o que é uma instância gerido?](sql-database-managed-instance.md)
- Para funcionalidades e a lista de comparação, consulte [funcionalidades comuns do SQL Server](sql-database-features.md).
- Para um tutorial mostrar como criar uma nova instância geridos, consulte [criação de uma instância geridos](sql-database-managed-instance-create-tutorial-portal.md).
