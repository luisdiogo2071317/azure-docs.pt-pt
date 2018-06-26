---
title: Bases de dados individuais de limites de recursos com base em DTU de base de dados SQL do Azure | Microsoft Docs
description: Esta página descreve alguns limites de recursos baseados em DTU comuns das bases de dados na base de dados do Azure SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 5a7abb7d67de59ea326b5180cf94e3594cd06576
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753392"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>Limites de recursos das bases de dados utilizando o modelo de compra baseado em DTU 

Este artigo fornece os limites de recursos de detalhado para conjuntos elásticos SQL Database do Azure utilizando o modelo de compra baseado em DTU.

Para baseado em DTU compra modelo dos limites de recursos para conjuntos elásticos, consulte [limites de recursos baseados em DTU - conjuntos elásticos](sql-database-vcore-resource-limits-elastic-pools.md). Para os limites de recursos baseados em vCore, consulte [limites de recursos baseados em vCore - bases de dados individuais](sql-database-vcore-resource-limits-single-databases.md) e [limites de recursos baseados em vCore - conjuntos elásticos](sql-database-vcore-resource-limits-elastic-pools.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Base de dados individual: tamanhos de armazenamento e níveis de desempenho

Para bases de dados individuais, as tabelas seguintes mostram os recursos disponíveis para uma base de dados a cada nível de desempenho e o escalão de serviço. Pode definir a camada de serviço, o nível de desempenho e a quantidade de armazenamento para uma única base de dados utilizando o [portal do Azure](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [PowerShell](sql-database-servers-databases-manage.md#powershell-manage-logical-servers-and-databases), a [CLI do Azure](sql-database-servers-databases-manage.md#azure-cli-manage-logical-servers-and-databases), ou o [REST API](sql-database-servers-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="basic-service-tier"></a>Camada de serviços básicos
| **Nível de desempenho** | **Básica** |
| :--- | --: |
| Máximo de DTUs | 5 |
| Incluído armazenamento (GB) | 2 |
| Opções de armazenamento máximo (GB) | 2 |
| Armazenamento em OLTP na memória máx. (GB) |N/A |
| Funcionários em simultâneo de máx. (pedidos) | 30 |
| Sessões simultâneas máx. | 300 |
|||

### <a name="standard-service-tier"></a>Camada de serviços padrão
| **Nível de desempenho** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| Máximo de DTUs | 10 | 20 | 50 | 100 |
| Incluído armazenamento (GB) | 250 | 250 | 250 | 250 |
| Opções de armazenamento máximo (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Armazenamento em OLTP na memória máx. (GB) | N/A | N/D | N/D | N/A |
| Funcionários em simultâneo de máx. (pedidos)| 60 | 90 | 120 | 200 |
| Sessões simultâneas máx. |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Camada de serviço Standard (continuação)
| **Nível de desempenho** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| Máximo de DTUs | 200 | 400 | 800 | 1600 | 3000 |
| Incluído armazenamento (GB) | 250 | 250 | 250 | 250 | 250 |
| Opções de armazenamento máximo (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Armazenamento em OLTP na memória máx. (GB) | N/A | N/D | N/D | N/D |N/A |
| Funcionários em simultâneo de máx. (pedidos)| 400 | 800 | 1600 | 3200 |6000 |
| Sessões simultâneas máx. |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Camada de serviços premium 
| **Nível de desempenho** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Máximo de DTUs | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Incluído armazenamento (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| Opções de armazenamento máximo (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Armazenamento em OLTP na memória máx. (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Funcionários em simultâneo de máx. (pedidos)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Sessões simultâneas máx. | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||


> [!IMPORTANT]
> Mais de 1 TB de armazenamento no escalão Premium está atualmente disponível em todas as regiões, exceto o seguinte: norte do RU Central EUA oeste, RU South2, leste da China, USDoDCentral, Datacenters Central, USDoDEast, GOV us Southwest, nos us Sul Central, Nordeste da Alemanha, China Norte, US de e.u. a leste Noutras regiões, o armazenamento máximo no escalão Premium está limitado a 1 TB. Ver [Limitações Atuais P11-P15](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="single-database-change-storage-size"></a>Base de dados individual: Altere o tamanho de armazenamento

- O preço DTU para uma base de dados inclui uma determinada quantidade de armazenamento sem custos adicionais. Armazenamento adicional para além da quantidade incluída pode ser aprovisionado um custo adicionais até ao limite de tamanho máximo em incrementos de 250 GB de cópia de segurança para 1 TB e, em seguida, em incrementos de 256 GB para além de 1 TB. Para quantidades incluídas do armazenamento e limites de tamanho máximo, consulte [única base de dados: tamanhos de armazenamento e níveis de desempenho](#single-database-storage-sizes-and-performance-levels).
- Armazenamento adicional para uma base de dados individual pode ser aprovisionado pelos aumentar o tamanho máximo utilizando o [portal do Azure](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), a [Azure CLI](/cli/azure/sql/db#az_sql_db_update), ou o [REST API](/rest/api/sql/databases/update).
- O preço de armazenamento adicional para uma base de dados é a quantidade de armazenamento adicional multiplicada pelo preço de unidade de armazenamento adicional da camada de serviços. Para obter detalhes sobre os preços de armazenamento adicional, consulte [preços da SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Base de dados individual: DTUs da alteração

Após inicialmente escolher uma camada de serviço, o nível de desempenho e a quantidade de armazenamento, pode dimensionar uma base de dados ou reduzir verticalmente dinamicamente com base na experiência real utilizando o [portal do Azure](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), a [CLI do Azure](/cli/azure/sql/db#az_sql_db_update), ou o [REST API](/rest/api/sql/databases/update). 

O vídeo seguinte mostra dinamicamente a alteração da camada de desempenho para aumentar as DTUs disponíveis para uma base de dados.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Alterar o escalão de serviço e/ou nível de desempenho de uma base de dados cria uma réplica da base de dados original ao nível do novo desempenho e, em seguida, troca ligações para a réplica. Não são perdidos dados durante este processo mas durante o breve momento em que trocamos para a réplica, as ligações à base de dados são desativadas, pelo que algumas transações em voo poderão ser revertidas. O período de tempo para o comutador a ativação pós-falha varia, mas é menos de 30 segundos 99% das vezes. Se existirem grande número de transações em trânsito, as ligações do momento em que está desativado, o período de tempo para o comutador a ativação pós-falha pode ser maior. 

A duração de todo o processo de aumento vertical depende do tamanho e do escalão de serviço da base de dados antes e após a alteração. Por exemplo, uma base de dados de 250 GB que está a mudar para ou dentro de uma camada de serviço Standard, deve ser concluído dentro de seis horas. Para uma base de dados do mesmo tamanho que está a alterar os níveis de desempenho na camada de serviço Premium, a escala cópia de segurança deve ser concluído dentro de três horas.

> [!TIP]
> Para monitorizar operações no progess, consulte: [gerir operações utilizando a API de REST do SQL Server](/rest/api/sql/Operations/List), [gerir operações com a CLI](/cli/azure/sql/db/op), [monitorizar as operações com T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) e estes dois Comandos do PowerShell: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) e [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Se estiver a atualizar para um nível de desempenho ou camada de serviço mais elevado, o tamanho máximo da base de dados não aumenta a menos que especifique explicitamente um tamanho maior (maxsize).
* Para mudar uma base de dados, o espaço de base de dados utilizada tem de ser menor que o tamanho máximo permitido do nível de desempenho e o escalão de serviço do destino. 
* Quando desatualização de **Premium** para o **padrão** camada, um custo de armazenamento adicional aplica-se se tanto (1) o tamanho máximo da base de dados é suportado no nível de desempenho de destino, e (2) o tamanho máximo excede o quantidade de armazenamento incluída com o nível de desempenho de destino. Por exemplo, se uma base de dados P1 com um tamanho máximo de 500 GB é downsized para S3, em seguida, um custo de armazenamento adicional aplica-se porque S3 suporta um tamanho máximo de 500 GB e a quantidade de armazenamento incluída é apenas de 250 GB. Por isso, a quantidade de armazenamento adicional é 500 GB – 250 GB = 250 GB. Para obter preços de armazenamento adicional, consulte [preços da SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Se a quantidade real de espaço utilizado é inferior à quantidade incluída de armazenamento, em seguida, este custo extra pode ser evitada ao reduzir o tamanho máximo da base de dados para a quantidade incluída. 
* Ao atualizar a base de dados com [georreplicação](sql-database-geo-replication-portal.md) ativada, atualizar suas bases de dados secundárias para a camada de desempenho pretendido antes de atualizar a base de dados primária (orientações gerais para um melhor desempenho). Quando atualizar para o outro, atualizar a base de dados secundária primeiro é necessário.
* Quando a desatualização de uma base de dados com [georreplicação](sql-database-geo-replication-portal.md) ativada, mudar o respetivos bases de dados primárias para a camada de desempenho pretendido antes desatualização de base de dados secundária (orientações gerais para um melhor desempenho). Quando a mudança para uma edição diferente, desatualização de base de dados primária primeiro é necessário.
* As ofertas de serviço de restauro diferem entre os vários escalões de serviço. Se estão a mudança para o **básico** camada, há um período de retenção de cópias de segurança inferior - consulte [cópias de segurança do Azure SQL da base de dados](sql-database-automated-backups.md).
* As novas propriedades para a base de dados não são aplicadas até que as alterações estejam concluídas.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Base de dados individual: limitações de P11 e P15 quando o número máximo tamanho superior a 1 TB

Um tamanho máximo superior a 1 TB para base de dados P11 e P15 é suportada nas seguintes regiões: Leste da Austrália, Sudeste da Austrália, sul do Brasil, Canadá Central, Canadá leste, EUA Central, França Central, Datacenters Central, leste do Japão, oeste do Japão, Coreia Central, EUA Centro-Norte, Europa do Norte, EUA Centro-Sul, Sudeste asiático, sul do RU, RU oeste, E.U.A. East2, EUA oeste, E.U.A. us Virginia e Europa Ocidental. As seguintes considerações e limitações aplicam-se às bases de dados P11 e P15 com um tamanho máximo superior a 1 TB:

- Se escolher um tamanho máximo superior a 1 TB ao criar uma base de dados (utilizando um valor de 4096 GB ou de 4 TB), o comando create falha com um erro se a base de dados é aprovisionado numa região não suportada.
- Para existentes P11 P15 bases de dados e localizados das regiões suportadas, pode aumentar o máximo de armazenamento para além de 1 TB em incrementos de 256 GB até 4 TB. Para ver se um tamanho maior é suportado na sua região, utilize o [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) funcionar ou Inspecione o tamanho de base de dados no portal do Azure. Atualizar um P11 existente ou P15 base de dados só pode ser efetuada por um início de sessão principal ao nível do servidor ou por membros da função de base de dados de dbmanager. 
- Se uma operação de atualização é executada numa região suportada a configuração está atualizada imediatamente. A base de dados permanece online durante o processo de atualização. No entanto, não é possível utilizar a quantidade total de armazenamento para além de 1 TB de armazenamento até que os ficheiros de base de dados real foram atualizados para o novo tamanho máximo. O período de tempo necessário depende após o tamanho da base de dados a ser atualizado. 
- Quando criar ou atualizar uma base de dados P11 ou P15, apenas pode escolher entre 1 TB e 4 TB o tamanho máximo em incrementos de 256 GB. Ao criar um P11/P15, a opção de armazenamento predefinida de 1 TB é pré-seleccionado. Para bases de dados localizados das regiões suportadas, pode aumentar o número máximo de armazenamento para até um máximo de 4 TB para uma base de dados nova ou existente. Para todas as outras regiões, não é possível aumentar o tamanho máximo superiores a 1 TB. O preço não se altera quando seleciona 4 TB de armazenamento incluído.
- Se o tamanho máximo de uma base de dados está definido como superior a 1 TB, em seguida, este não pode ser alterada para 1 TB, mesmo que o armazenamento real utilizado é inferior a 1 TB. Assim, a não é possível mudar um P11 ou P15 com um tamanho máximo superior a 1 TB para 1 TB P11 ou desempenho inferior a 1 TB P15 camada, tais como P1 P6). Esta restrição aplica-se também de restauro e cenários de cópia, incluindo o ponto no tempo, georrestauro, longo-prazo-cópia de segurança-retenção e a cópia da base de dados. Depois de uma base de dados está configurado com um tamanho máximo superior a 1 TB, todas as operações de restauro desta base de dados devem ser executadas para um P11/P15 com um tamanho máximo superior a 1 TB.
- Para cenários de replicação geográfica activa:
   - Configurar uma relação de replicação geográfica: se a base de dados primária estiver P11 ou P15, o secondary(ies) também tem de ser P11 ou P15; os escalões de desempenho inferiores são rejeitados como bases de dados secundárias, uma vez que não são capazes de oferecer suporte mais de 1 TB.
   - Atualizar a base de dados primária numa relação de replicação geográfica: alterar o tamanho máximo para mais de 1 TB numa base de dados primária aciona a mesma alteração na base de dados secundária. Ambas as atualizações tem de ser efetuada com êxito para a alteração principal tenha efeito. Aplicam limitações de região para a opção de mais de 1 TB. Se secundário está numa região que não suporta mais de 1 TB, o principal não está atualizado.
- Não é suportada a utilização do serviço de importação/exportação para carregar P11/P15 bases de dados com mais de 1 TB. Utilizar SqlPackage.exe para [importar](sql-database-import.md) e [exportar](sql-database-export.md) dados.

## <a name="next-steps"></a>Passos Seguintes

- Consulte [FAQ de base de dados do SQL Server](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites de serviço, quotas e restrições](../azure-subscription-service-limits.md).
- Para obter informações sobre as DTUs e eDTUs, consulte [DTUs e eDTUs](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Para obter informações sobre limites de tamanho de tempdb, consulte https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
