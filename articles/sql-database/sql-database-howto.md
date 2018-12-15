---
title: Como configurar a base de dados SQL do Azure | Documentos da Microsoft
description: Saiba como configurar e gerir a base de dados do Azure SQL.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: b4dd21324591075d7625a82fbbb661c4a8e84b1d
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/15/2018
ms.locfileid: "53440507"
---
# <a name="how-to-use-azure-sql-database"></a>Como utilizar a base de dados do Azure SQL

Nesta seção encontrará várias guias, scripts e explicações que podem ajudar a gerir e configurar a sua base de dados do SQL do Azure. Também encontrará guias de procedimentos específicos para [base de dados individual](sql-database-howto-single-database.md) e [instância gerida](sql-database-howto-managed-instance.md).

## <a name="load-data"></a>Carregar dados

- [Copiar uma base de dados no Azure](https://docs.microsoft.com/azure/sql-database/sql-database-copy)
- [Importar uma BD de um BACPAC](https://docs.microsoft.com/azure/sql-database/sql-database-import)
- [Exportar uma BD para BACPAC](https://docs.microsoft.com/azure/sql-database/sql-database-export)
- [Carregar dados com o BCP](https://docs.microsoft.com/azure/sql-database/sql-database-load-from-csv-with-bcp)
- [Carregar dados com o ADF](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database?toc=/azure/sql-database/toc.json)

### <a name="data-sync"></a>Sincronização de dados

- [Sincronização de dados SQL](https://docs.microsoft.com/azure/sql-database/sql-database-sync-data)
- [Agente de sincronização de dados](https://docs.microsoft.com/azure/sql-database/sql-database-data-sync-agent)
- [Replicar as alterações de esquema](https://docs.microsoft.com/azure/sql-database/sql-database-update-sync-schema)
- [Monitorizar com o OMS](https://docs.microsoft.com/azure/sql-database/sql-database-sync-monitor-oms)
- [Melhores práticas para a sincronização de dados](https://docs.microsoft.com/azure/sql-database/sql-database-best-practices-data-sync)
- [Resolver problemas da sincronização de dados](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-data-sync)

## <a name="monitoring-and-tuning"></a>Monitorização e otimização

-  [Ajuste manual](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
- [Utilizar DMVs para monitorizar o desempenho](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
- [Utilizar o arquivo de consultas para monitorizar o desempenho](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)
- [Resolver problemas de desempenho com informações inteligentes](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-troubleshoot-performance)
- [Utilizar o registo de diagnóstico de informações inteligentes](https://docs.microsoft.com/azure/sql-database/sql-database-intelligent-insights-use-diagnostics-log)
- [Monitorizar o espaço OLTP dentro da memória](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-monitoring)

### <a name="extended-events"></a>Eventos expandidos

- [Eventos expandidos](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-db-diff-from-svr)
- [Store eventos expandidos no arquivo de eventos](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-event-file)
- [Eventos expandidos de Store em memória intermédia em anel](https://docs.microsoft.com/azure/sql-database/sql-database-xevent-code-ring-buffer)

## <a name="configure-features"></a>Configurar funcionalidades

- [Configurar a autenticação do Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)
- [Configurar o acesso condicional](https://docs.microsoft.com/azure/sql-database/sql-database-conditional-access)
- [Multi-factor Authentication do AAD](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication)
- [Configurar a autenticação multifator](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication-configure)
- [Configurar a política de retenção temporal](https://docs.microsoft.com/azure/sql-database/sql-database-temporal-tables-retention-policy)
- [Configurar o TDE com o BYOK](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure)
- [Rodar chaves de TDE BYOK](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-key-rotation)
- [Remover o protetor de TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-byok-azure-sql-remove-tde-protector)
- [Configurar o OLTP dentro da memória](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory-oltp-migration)
- [Configurar a automatização do Azure](https://docs.microsoft.com/azure/sql-database/sql-database-manage-automation)

## <a name="develop-applications"></a>Programar aplicações

- [Conetividade](https://docs.microsoft.com/azure/sql-database/sql-database-libraries)
- [Utilizar o conector do Spark](https://docs.microsoft.com/azure/sql-database/sql-database-spark-connector)
- [Autenticar aplicação](https://docs.microsoft.com/azure/sql-database/sql-database-client-id-keys)
- [Mensagens de erro](https://docs.microsoft.com/azure/sql-database/sql-database-develop-error-messages)
- [Utilize a criação de batches para um melhor desempenho](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance)
- [Orientações para conectividade](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues)
- [Aliases de DNS](https://docs.microsoft.com/azure/sql-database/dns-alias-overview)
- [Configurar o alias PowerShell de DNS](https://docs.microsoft.com/azure/sql-database/dns-alias-powershell)
- [Portas - ADO.NET](https://docs.microsoft.com/azure/sql-database/sql-database-develop-direct-route-ports-adonet-v12)
- [C e C++](https://docs.microsoft.com/azure/sql-database/sql-database-develop-cplusplus-simple)
- [Excel](https://docs.microsoft.com/azure/sql-database/sql-database-connect-excel)

## <a name="design-applications"></a>Conceber aplicações

- [Design para recuperação após desastre](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery)
- [Design para conjuntos elásticos](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool)
- [Design para atualizações de aplicações](https://docs.microsoft.com/azure/sql-database/sql-database-manage-application-rolling-upgrade)

### <a name="design-multi-tenant-saas-applications"></a>Criar aplicações SaaS de multi-inquilino

- [Padrões de design de SaaS](https://docs.microsoft.com/azure/sql-database/saas-tenancy-app-design-patterns)
- [Indexador de vídeos de SaaS](https://docs.microsoft.com/azure/sql-database/saas-tenancy-video-index-wingtip-brk3120-20171011)
- [Segurança de aplicação SaaS](https://docs.microsoft.com/azure/sql-database/saas-tenancy-elastic-tools-multi-tenant-row-level-security)

## <a name="next-steps"></a>Passos Seguintes
- Saiba mais sobre [guias de procedimentos na instância gerida](sql-database-howto-managed-instance.md).
- Saiba mais sobre [guias de procedimentos na base de dados única](sql-database-howto-single-database.md).
