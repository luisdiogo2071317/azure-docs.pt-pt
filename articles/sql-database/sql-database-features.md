---
title: Comparação de funcionalidades de base de dados SQL do Azure | Microsoft Docs
description: Este artigo compara as funcionalidades do SQL Server que estão disponíveis em diferentes tipos de SQL Database do Azure.
services: sql-database
author: jovanpop-msft
ms.reviewer: bonova, carlrab
ms.service: sql-database
ms.topic: conceptual
ms.date: 06/08/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: de85a555def31d164f2cda5c6c6bc9fae9fb5a7e
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309287"
---
# <a name="feature-comparison-azure-sql-database-versus-sql-server"></a>Comparação de funcionalidades: base de dados de SQL do Azure versus do SQL Server 

Base de dados SQL do Azure tem uma base de código comuns com SQL Server. As funcionalidades do SQL Server suportada pelo SQL Database do Azure dependem do tipo de base de dados SQL do Azure que criar. Base de dados SQL do Azure, pode criar uma base de dados como parte de um [instância gerida](sql-database-managed-instance.md) (atualmente em pré-visualização pública) ou pode criar uma base de dados que fazem parte do servidor lógico e, opcionalmente, colocá-la num agrupamento elástico. 

Microsoft continua adicionar funcionalidades à SQL Database do Azure. Visite a página atualizações de serviço Web do Azure para as atualizações mais recentes utilizando estes filtros:

* Filtrado para o [serviço da Base de Dados SQL](https://azure.microsoft.com/updates/?service=sql-database).
* Filtrado para [anúncios](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) de Disponibilidade Geral (DG) para funcionalidades de Base de Dados SQL.

## <a name="sql-server-feature-support-in-azure-sql-database"></a>Suporte de funcionalidades do SQL Server na SQL Database do Azure

A tabela seguinte lista as principais funcionalidades do SQL Server e fornece informações sobre se a funcionalidade é total ou parcialmente suportada e uma hiperligação para obter mais informações sobre a funcionalidade. 

| **Funcionalidade do SQL Server** | **Suportado no servidor de base de dados lógico de SQL do Azure** | **Suportadas na instância de base de dados/gerida do SQL do Azure (pré-visualização)** |
| --- | --- | --- |
| [Sempre encriptado](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) | Sim - consulte [arquivo de certificados](sql-database-always-encrypted.md) e [Cofre de chaves](sql-database-always-encrypted-azure-key-vault.md) | Sim - consulte [arquivo de certificados](sql-database-always-encrypted.md) e [Cofre de chaves](sql-database-always-encrypted-azure-key-vault.md) |
| [Always On nos grupos de disponibilidade](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) | [Elevada disponibilidade](sql-database-high-availability.md) está incluído com cada base de dados. Recuperação após desastre é abordada em [descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md) | [Elevada disponibilidade](sql-database-high-availability.md) está incluído com cada base de dados. Recuperação após desastre é abordada em [descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md) |
| [Anexar uma base de dados](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database) | Não | Não |
| [Funções de aplicação](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/application-roles) | Sim | Sim |
|[Auditoria](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | [Sim](sql-database-auditing.md)| [Sim](sql-database-managed-instance-auditing.md) |
| [Cópias de segurança automáticas](sql-database-automated-backups.md) | Sim | Sim |
| [Automático otimização (forçar o plano de)](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Sim](sql-database-automatic-tuning.md)| [Sim](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) |
| [Automático (índices) de otimização](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning)| [Sim](sql-database-automatic-tuning.md)| Não |
| [Ficheiro BACPAC (exportação)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/export-a-data-tier-application) | Sim - consulte [exportação de base de dados SQL](sql-database-export.md) | Não |
| [Ficheiro BACPAC (importar)](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/import-a-bacpac-file-to-create-a-new-user-database) | Sim - consulte [importar da base de dados SQL](sql-database-import.md) | Não |
| [Comando de cópia de segurança](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql) | Não, apenas iniciadas por sistema cópias de segurança automáticas - Consulte [cópias de segurança automatizadas](sql-database-automated-backups.md) | Cópias de segurança automatizadas iniciadas pelo sistema e o utilizador iniciou as cópias de segurança apenas de cópia - consulte [diferenças de cópia de segurança](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Funções incorporadas](https://docs.microsoft.com/sql/t-sql/functions/functions) | Consulte mais - funções individuais | Sim - consulte [os procedimentos armazenados, funções, aciona diferenças](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Captura de dados de alteração](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server) | Não | Sim |
| [Registo de alterações](https://docs.microsoft.com/sql/relational-databases/track-changes/about-change-tracking-sql-server) | Sim |Sim |
| [Instruções de agrupamento](https://docs.microsoft.com/sql/t-sql/statements/collations) | Sim | Sim |
| [Índices Columnstore](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) | Sim - [escalão Premium, o escalão Standard - S3 e superior, a camada de objetivo geral e camadas críticos de negócio](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) |Sim |
| [Common language runtime (CLR)](https://docs.microsoft.com/sql/relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts) | Não | Sim - consulte [diferenças CLR](sql-database-managed-instance-transact-sql-information.md#clr) |
| [Bases de dados contidas](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) | Sim | Sim |
| [Utilizadores contidos](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) | Sim | Sim |
| [Controlo de palavras-chave de idioma de fluxo](https://docs.microsoft.com/sql/t-sql/language-elements/control-of-flow) | Sim | Sim |
| [Consultas de base de dados em vários locais](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Não - consulte [consultas elásticas](sql-database-elastic-query-overview.md) | Sim, plus [consultas elásticas](sql-database-elastic-query-overview.md) |
| [Transações entre bases de dados](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Não | Sim - consulte [ligado diferenças de servidor](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#linked-servers) |
| [Cursores](https://docs.microsoft.com/sql/t-sql/language-elements/cursors-transact-sql) | Sim |Sim | 
| [Compressão de dados](https://docs.microsoft.com/sql/relational-databases/data-compression/data-compression) | Sim |Sim |
| [Correio de base de dados](https://docs.microsoft.com/sql/relational-databases/database-mail/database-mail) | Não | Sim |
| [Serviço de migração de dados (DMS)](https://docs.microsoft.com/sql/dma/dma-overview) | Sim | Sim |
| [O espelhamento da base de dados](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server) | Não | Não |
| [Definições de configuração de base de dados](https://docs.microsoft.com/sql/t-sql/statements/alter-database-scoped-configuration-transact-sql) | Sim | Sim |
| [Data Quality Services (DQS)](https://docs.microsoft.com/sql/data-quality-services/data-quality-services) | Não | Não |
| [Instantâneos da base de dados](https://docs.microsoft.com/sql/relational-databases/databases/database-snapshots-sql-server) | Não | Não |
| [Tipos de dados](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql) | Sim |Sim |
| [Instruções de DBCC](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) | Consulte mais - instruções individuais | Sim - consulte [diferenças DBCC](sql-database-managed-instance-transact-sql-information.md#dbcc) |
| [Instruções DDL](https://docs.microsoft.com/sql/t-sql/statements/statements) | Consulte mais - instruções individuais | Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Acionadores DDL](https://docs.microsoft.com/sql/relational-databases/triggers/ddl-triggers) | Apenas a base de dados |  Sim |
| [Vistas distribuídas de partição](https://docs.microsoft.com/sql/t-sql/statements/create-view-transact-sql#partitioned-views) | Não | Sim |
| [Transações distribuídas - MS DTC](https://docs.microsoft.com/sql/relational-databases/native-client-ole-db-transactions/supporting-distributed-transactions) | Não - consulte [transações elásticas](sql-database-elastic-transactions-overview.md) |  Não - consulte [transações elásticas](sql-database-elastic-transactions-overview.md) |
| [Instruções de DML](https://docs.microsoft.com/sql/t-sql/queries/queries) | Sim | Sim |
| [Acionadores DML](https://docs.microsoft.com/sql/relational-databases/triggers/create-dml-triggers) | Consulte mais - instruções individuais |  Sim |
| [DMVs](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views) | Consulte mais - DMVs individuais |  Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md) |
|[Máscara de dados dinâmica](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)|[Sim](sql-database-dynamic-data-masking-get-started.md)| [Sim](sql-database-dynamic-data-masking-get-started.md) |
| [Conjuntos elásticos](sql-database-elastic-pool.md) | Sim | Incorporado-uma única instância geridos pode ter várias bases de dados que partilham o mesmo agrupamento de recursos |
| [Notificações de eventos](https://docs.microsoft.com/sql/relational-databases/service-broker/event-notifications) | Não - consulte [alertas](sql-database-insights-alerts-portal.md) | Sim |
| [Expressões](https://docs.microsoft.com/sql/t-sql/language-elements/expressions-transact-sql) |Sim | Sim |
| [Eventos expandidos](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events) | Algumas - consulte [expandido eventos na base de dados SQL](sql-database-xevent-db-diff-from-svr.md) | Sim - consulte [expandido diferenças de eventos ](sql-database-managed-instance-transact-sql-information.md#extended-events) |
| [procedimentos armazenados expandidos](https://docs.microsoft.com/sql/relational-databases/extended-stored-procedures-programming/creating-extended-stored-procedures) | Não | Não |
[Ficheiros e grupos de ficheiros](https://docs.microsoft.com/sql/relational-databases/databases/database-files-and-filegroups) | Grupo de ficheiros primário apenas | Sim |
| [FileStream](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) | Não | Não |
| [Pesquisa em texto completo](https://docs.microsoft.com/sql/relational-databases/search/full-text-search) |  Breakers palavras de terceiros não são suportados |Breakers palavras de terceiros não são suportados |
| [Funções](https://docs.microsoft.com/sql/t-sql/functions/functions) | Consulte mais - funções individuais | Sim - consulte [os procedimentos armazenados, funções, aciona diferenças](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [georrestauro](sql-database-recovery-using-backups.md#geo-restore) | Sim | Não – COPY_ONLY pode restaurar cópias de segurança completas que tomar periodicamente - consulte [diferenças de cópia de segurança](sql-database-managed-instance-transact-sql-information.md#backup) e [restaurar diferenças](sql-database-managed-instance-transact-sql-information.md#restore-statement). |
| [Georreplicação](sql-database-geo-replication-overview.md) | Sim | Não |
| [Processamento de gráfico](https://docs.microsoft.com/sql/relational-databases/graphs/sql-graph-overview) | Sim | Sim |
| [Otimização dentro da memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization) | Sim - [camadas Premium e crítico de negócio](sql-database-in-memory.md) | Não |
| [Suporte de dados JSON](https://docs.microsoft.com/sql/relational-databases/json/json-data-sql-server) | [Sim](https://docs.microsoft.com/azure/sql-database/sql-database-json-features) | [Sim](https://docs.microsoft.com/azure/sql-database/sql-database-json-features) |
| [Elementos de linguagem](https://docs.microsoft.com/sql/t-sql/language-elements/language-elements-transact-sql) | Consulte mais - elementos individuais |  Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Servidores ligados](https://docs.microsoft.com/sql/relational-databases/linked-servers/linked-servers-database-engine) | Não - consulte [consulta elástica](sql-database-elastic-query-horizontal-partitioning.md) | Apenas para o SQL Server e base de dados SQL |
| [Envio de registo](https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) | [Elevada disponibilidade](sql-database-high-availability.md) está incluído com cada base de dados. Recuperação após desastre é abordada em [descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md) |[Elevada disponibilidade](sql-database-high-availability.md) está incluído com cada base de dados. Recuperação após desastre é abordada em [descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md) |
| [Dados do Master Data Services (MDS)](https://docs.microsoft.com/sql/master-data-services/master-data-services-overview-mds) | Não | Não |
| [Registo mínimo na importação em volume](https://docs.microsoft.com/sql/relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import) | Não | Não |
| [Modificar os dados de sistema](https://docs.microsoft.com/sql/relational-databases/databases/system-databases) | Não | Sim |
| [Operações de índice online](https://docs.microsoft.com/sql/relational-databases/indexes/perform-index-operations-online) | Sim | Sim |
| [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql)|Não|Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md)|
| [OPENJSON](https://docs.microsoft.com/sql/t-sql/functions/openjson-transact-sql)|Sim|Sim|
| [OPENQUERY](https://docs.microsoft.com/sql/t-sql/functions/openquery-transact-sql)|Não|Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md)|
| [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql)|Não|Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md)|
| [OPENXML](https://docs.microsoft.com/sql/t-sql/functions/openxml-transact-sql)|Sim|Sim|
| [Operadores](https://docs.microsoft.com/sql/t-sql/language-elements/operators-transact-sql) | Consulte mais - operadores individuais |Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Criação de partições](https://docs.microsoft.com/sql/relational-databases/partitions/partitioned-tables-and-indexes) | Sim | Sim |
| [Ponto de restauro de base de dados do tempo](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model) | Sim - consulte [recuperação de base de dados SQL](sql-database-recovery-using-backups.md#point-in-time-restore) | Sim - consulte [recuperação de base de dados SQL](sql-database-recovery-using-backups.md#point-in-time-restore) |
| [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) | Não | Não |
| [Gestão baseada em políticas](https://docs.microsoft.com/sql/relational-databases/policy-based-management/administer-servers-by-using-policy-based-management) | Não | Não |
| [Predicados de](https://docs.microsoft.com/sql/t-sql/queries/predicates) | Sim | Sim |
| [Serviços R](https://docs.microsoft.com/sql/advanced-analytics/r-services/sql-server-r-services) | Versão de pré-visualização consulte [Novidades no machine learning](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services)  | Não |
| [Governador de recursos](https://docs.microsoft.com/sql/relational-databases/resource-governor/resource-governor) | Não | Sim |
| [RESTAURAR instruções](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-for-restoring-recovering-and-managing-backups-transact-sql) | Não | Sim - consulte [restaurar diferenças](sql-database-managed-instance-transact-sql-information.md#restore-statement) |
| [Restaurar a base de dados da cópia de segurança](https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases#restore-data-backups) | De automatizada cópias de segurança só - consulte [recuperação de base de dados SQL](sql-database-recovery-using-backups.md) | De cópias de segurança automatizadas - consulte [recuperação de base de dados SQL](sql-database-recovery-using-backups.md) e de cópias de segurança completas - consulte [diferenças de cópia de segurança](sql-database-managed-instance-transact-sql-information.md#backup) |
| [Segurança ao nível da linha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) | Sim | Sim |
| [Pesquisa semântica](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) | Não | Não |
| [Números de sequência](https://docs.microsoft.com/sql/relational-databases/sequence-numbers/sequence-numbers) | Sim | Sim |
| [Mediador de serviço](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-service-broker) | Não | Sim - consulte [diferenças de Mediador de serviço](sql-database-managed-instance-transact-sql-information.md#service-broker) |
| [Definições de configuração do servidor](https://docs.microsoft.com/sql/database-engine/configure-windows/server-configuration-options-sql-server) | Não | Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Conjunto de instruções](https://docs.microsoft.com/sql/t-sql/statements/set-statements-transact-sql) | Consulte mais - instruções individuais | Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md)|
| [SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | Sim | Sim |
| [Spatial](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server) | Sim | Sim |
| [Sincronização de dados do SQL Server](sql-database-get-started-sql-data-sync.md) | Sim | Não |
| [SQL Operations Studio](https://docs.microsoft.com/sql/sql-operations-studio/what-is) | Sim | Sim |
| [Agente do SQL Server](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) | Não - consulte [as tarefas elásticas](sql-database-elastic-jobs-getting-started.md) | Sim - consulte [diferenças de agente do SQL Server](sql-database-managed-instance-transact-sql-information.md#sql-server-agent) |
| [SQL Server Analysis Services (SSAS)](https://docs.microsoft.com/sql/analysis-services/analysis-services) | Não - consulte [do Analysis Services do Azure](https://azure.microsoft.com/services/analysis-services/) | Não - consulte [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) |
| [Auditoria do SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine) | Não - consulte [auditoria de base de dados SQL](sql-database-auditing.md) | Sim - consulte [diferenças de auditoria](sql-database-managed-instance-transact-sql-information.md#auditing) |
| [SQL Server Data Tools (SSDT)] (https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt) | Sim | Sim |
| [SQL Server Integration Services (SSIS)](https://docs.microsoft.com/sql/integration-services/sql-server-integration-services) | Sim, com um SSIS gerido num ambiente do Azure Data Factory (ADF), onde estão armazenados os pacotes SSISDB alojadas pelo SQL Database do Azure e executado num [Runtime de integração do Azure SSIS](../data-factory/tutorial-deploy-ssis-packages-azure-powershell.md). | Sim, com um SSIS gerido no ambiente do Azure Data Factory (ADF), onde estão armazenados os pacotes SSISDB alojada pela instância geridos e executado num [Runtime de integração do Azure SSIS](../data-factory/tutorial-deploy-ssis-packages-azure-powershell.md). |
| [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) | Sim | Sim |
| [SQL Server do PowerShell](https://docs.microsoft.com/sql/relational-databases/scripting/sql-server-powershell) | Sim | Sim |
| [O gerador de perfis do SQL Server](https://docs.microsoft.com/sql/tools/sql-server-profiler/sql-server-profiler) | Não - consulte [alargadas de eventos](sql-database-xevent-db-diff-from-svr.md) | Sim |
| [Replicação do SQL Server](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication) | [Apenas subscritor de replicação de instantâneo e transacional](sql-database-cloud-migrate.md) | Não |
| [SQL Server Reporting Services (SSRS)](https://docs.microsoft.com/sql/reporting-services/create-deploy-and-manage-mobile-and-paginated-reports) | Não - [Consulte Power BI](https://docs.microsoft.com/power-bi/) | Não - [Consulte Power BI](https://docs.microsoft.com/power-bi/) |
| [Procedimentos armazenados](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) | Sim | Sim |
| [Funções de sistema armazenados](https://docs.microsoft.com/sql/relational-databases/system-functions/system-functions-for-transact-sql) | Consulte mais - funções individuais | Sim - consulte [os procedimentos armazenados, funções, aciona diferenças](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Procedimentos armazenados de sistema](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/system-stored-procedures-transact-sql) | Algumas - Consulte individuais procedimentos armazenados | Sim - consulte [os procedimentos armazenados, funções, aciona diferenças](sql-database-managed-instance-transact-sql-information.md#stored-procedures-functions-triggers) |
| [Tabelas de sistema](https://docs.microsoft.com/sql/relational-databases/system-tables/system-tables-transact-sql) | Algumas - Consulte tabelas individuais | Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Vistas de catálogo de sistema](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/catalog-views-transact-sql) | Algumas - Consulte vistas individuais | Sim - consulte [diferenças de T-SQL](sql-database-managed-instance-transact-sql-information.md) |
| [Tabelas temporárias](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql#database-scoped-global-temporary-tables-azure-sql-database) | Tabelas temporárias globais locais e âmbito de base de dados | Tabelas temporárias globais locais e âmbito de instância |
| [Tabelas temporais](https://docs.microsoft.com/sql/relational-databases/tables/temporal-tables) | [Sim](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables) | [Sim](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables) |
|Deteção de ameaças|  [Sim](sql-database-threat-detection.md)|[Sim](sql-database-managed-instance-threat-detection.md)|
| [Sinalizadores de rastreio](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql) | Não | Não |
| [Variáveis](https://docs.microsoft.com/sql/t-sql/language-elements/variables-transact-sql) | Sim | Sim |
| [Encriptação de dados transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-tde) | Sim | Parcial, apenas com encriptação serviço gerido |
[VNet](../virtual-network/virtual-networks-overview.md) | Parcial - consulte [pontos finais de VNET](sql-database-vnet-service-endpoint-rule-overview.md) | Sim, apenas modelo do Resource Manager |
| [Windows Server Clustering de ativação pós-falha](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server) | [Elevada disponibilidade](sql-database-high-availability.md) está incluído com cada base de dados. Recuperação após desastre é abordada em [descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md) | [Elevada disponibilidade](sql-database-high-availability.md) está incluído com cada base de dados. Recuperação após desastre é abordada em [descrição geral da continuidade do negócio com a SQL Database do Azure](sql-database-business-continuity.md) |
| [Índices XML](https://docs.microsoft.com/sql/t-sql/statements/create-xml-index-transact-sql) | Sim | Sim |

## <a name="next-steps"></a>Passos Seguintes

- Para obter informações sobre o serviço Base de Dados SQL do Azure, veja [O que é a Base de Dados SQL?](sql-database-technical-overview.md)
- Para obter informações sobre uma instância geridos, consulte [o que é uma instância gerido?](sql-database-managed-instance.md).
