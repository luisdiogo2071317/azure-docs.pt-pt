---
title: Os limites de recursos baseados em vCore de base de dados SQL do Azure | Microsoft Docs
description: Esta página descreve alguns limites de recursos baseados em vCore comuns para a SQL Database do Azure.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: 204702eee1cf502ac873e0c1f5e3fd257ecce33c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-preview"></a>Base de dados do SQL do Azure baseados em vCore compra limites de modelo (pré-visualização)

> [!IMPORTANT]
> Para baseado em DTU compra modelo os limites, consulte [dos limites de DTU de base de dados do SQL Server com base em recursos](sql-database-dtu-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Base de dados individual: tamanhos de armazenamento e níveis de desempenho

Para bases de dados individuais, as tabelas seguintes mostram os recursos disponíveis para uma base de dados a cada nível de desempenho e o escalão de serviço. Pode definir a camada de serviço, o nível de desempenho e a quantidade de armazenamento para uma única base de dados utilizando o [portal do Azure](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), o [CLI do azure](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli), ou o [REST API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api).

### <a name="general-purpose-service-tier"></a>Camada de serviço de objetivo geral
|Nível de desempenho|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Memória (GB)|7|14|28|56|112|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|N/A|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|
|Tamanho máximo de dados (GB)|1024|1024|1536|3072|4096|
|Tamanho máximo de registo|307|307|461|922|1229|
|TempDB size(DB)|32|64|128|256|384|
|Destino IOPS|320|640|1280|2560|5120|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)
|Funcionários em simultâneo de máx. (pedidos)|200|400|800|1600|3200|
|Máx. permitido sessões|30000|30000|30000|30000|30000|
|Número de réplicas|1|1|1|1|1|
|Várias AZ|N/A|N/D|N/D|N/D|N/A|
|Leitura de escalamento horizontal|N/A|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

### <a name="business-critical-service-tier"></a>Camada de serviços críticos de negócio
|Nível de desempenho|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Memória (GB)|7|14|28|56|112|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|1|2|4|8|20|
|Tipo de armazenamento|SSD ligado|SSD ligado|SSD ligado|SSD ligado|SSD ligado|
|Tamanho máximo de dados (GB)|1024|1024|1024|1024|1024|
|Tamanho máximo de registo|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|
|Destino IOPS|5000|10000|20000|40000|80000|
|Latência de e/s (aproximada)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|
|Funcionários em simultâneo de máx. (pedidos)|200|400|800|1600|3200|
|Máx. permitido sessões|30000|30000|30000|30000|30000|
|Número de réplicas|3|3|3|3|3|
|Várias AZ|Sim|Sim|Sim|Sim|Sim|
|Leitura de escalamento horizontal|Sim|Sim|Sim|Sim|Sim|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

## <a name="single-database-change-storage-size"></a>Base de dados individual: Altere o tamanho de armazenamento

- É possível aprovisionar armazenamento até ao limite de tamanho máximo com incrementos de 1 GB. O armazenamento de dados configurável mínimo é de 5GB 
- Armazenamento para uma base de dados individual pode ser aprovisionado pelos aumentar ou diminuir o tamanho máximo utilizando o [portal do Azure](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), o [CLI do azure](/cli/azure/sql/db#az_sql_db_update), ou o [REST API](/rest/api/sql/databases/update).
- Base de dados SQL atribui automaticamente 30% do armazenamento adicional para os ficheiros de registo e 32GB por vCore para TempDB, mas não para exceder 384GB. TempDB está localizado num SSD em todos os escalões de serviço ligado.
- O preço do armazenamento para uma base de dados é a soma de dados de armazenamento e registo armazenamento quantidades multiplicado pelo preço de unidade de armazenamento da camada de serviços. O custo de TempDB está incluído no preço vCore. Para obter detalhes sobre os preços de armazenamento adicional, consulte [preços da SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-vcores"></a>Base de dados individual: alterar vCores

Após escolher inicialmente o número de vCores, pode dimensionar uma base de dados ou reduzir verticalmente dinamicamente com base na experiência real utilizando o [portal do Azure](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), a [CLI do Azure](/cli/azure/sql/db#az_sql_db_update), ou o [REST API](/rest/api/sql/databases/update). 

Alterar o escalão de serviço e/ou nível de desempenho de uma base de dados cria uma réplica da base de dados original ao nível do novo desempenho e, em seguida, troca ligações para a réplica. Não são perdidos dados durante este processo mas durante o breve momento em que trocamos para a réplica, as ligações à base de dados são desativadas, pelo que algumas transações em voo poderão ser revertidas. O período de tempo para o comutador a ativação pós-falha varia, mas é, geralmente, é de 4 segundos menos de 30 segundos 99% das vezes. Se existirem grande número de transações em trânsito, as ligações do momento em que está desativado, o período de tempo para o comutador a ativação pós-falha pode ser maior. 

A duração de todo o processo de aumento vertical depende do tamanho e do escalão de serviço da base de dados antes e após a alteração. Por exemplo, uma base de dados de 250 GB que está a mudar para ou dentro de uma camada de serviços de objetivo geral, deve ser concluído dentro de seis horas. Para uma base de dados do mesmo tamanho que está a alterar os níveis de desempenho na camada de serviço críticos de negócio, deve efetuar o vertical no prazo de três horas.

> [!TIP]
> Para monitorizar operações no progess, consulte: [gerir operações utilizando a API de REST do SQL Server](/rest/api/sql/Operations/List), [gerir operações com a CLI](/cli/azure/sql/db/op), [monitorizar as operações com T-SQL](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) e estes dois Comandos do PowerShell: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) e [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Se estiver a atualizar para um nível de desempenho ou camada de serviço mais elevado, o tamanho máximo da base de dados não aumenta a menos que especifique explicitamente um tamanho maior (maxsize).
* Para mudar uma base de dados, o espaço de base de dados utilizada tem de ser menor que o tamanho máximo permitido do nível de desempenho e o escalão de serviço do destino. 
* Ao atualizar a base de dados com [georreplicação](sql-database-geo-replication-portal.md) ativada, atualizar suas bases de dados secundárias para a camada de desempenho pretendido antes de atualizar a base de dados primária (orientações gerais para um melhor desempenho). Quando atualizar para o outro, atualizar a base de dados secundária primeiro é necessário.
* Quando a desatualização de uma base de dados com [georreplicação](sql-database-geo-replication-portal.md) ativada, mudar o respetivos bases de dados primárias para a camada de desempenho pretendido antes desatualização de base de dados secundária (orientações gerais para um melhor desempenho). Quando a mudança para uma edição diferente, desatualização de base de dados primária primeiro é necessário.
* As novas propriedades para a base de dados não são aplicadas até que as alterações estejam concluídas.

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Agrupamento elástico: tamanhos de armazenamento e níveis de desempenho

Para conjuntos elásticos SQL Database, as tabelas seguintes mostram os recursos disponíveis em cada nível de desempenho e o escalão de serviço. Pode definir a camada de serviço, nível de desempenho e quantidade de armazenamento utilizando o [portal do Azure](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-powershell), a [CLI do Azure](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-cli), ou o [API de REST](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-rest-api).

> [!NOTE]
> Os limites de recursos de bases de dados individuais em conjuntos elásticos, geralmente, são iguais às bases de dados individuais fora agrupamentos que tem o mesmo nível de desempenho. Por exemplo, os trabalhadores simultâneos máximas para uma base de dados GP_Gen4_1 é 200 workers. Por isso, os trabalhadores simultâneos máximas para uma base de dados num agrupamento GP_Gen4_1 também é 200 workers. Tenha em atenção de que o número total de trabalhadores simultâneas no agrupamento de GP_Gen4_1 é 210.

### <a name="general-purpose-service-tier"></a>Camada de serviço de objetivo geral
|Nível de desempenho|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Memória (GB)|7|14|28|56|112|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|N/A|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|
|Tamanho máximo de dados (GB)|512|756|1536|2048|3584|
|Tamanho máximo de registo|154|227|461|614|1075|
|TempDB size(DB)|32|64|128|256|384|
|Destino IOPS|320|640|1280|2560|5120|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|
|Funcionários em simultâneo de máx. (pedidos)|210|420|840|1680|3360|
|Máx. permitido sessões|30000|30000|30000|30000|30000|
|Densidade de agrupamento máx.|100|200|500|500|500|
|Clique em-paragem do conjunto elástico mínimo/máximo|0.25, 0, 1 a 0,5|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|
|Número de réplicas|1|1|1|1|1|
|Várias AZ|N/A|N/D|N/D|N/D|N/A|
|Leitura de escalamento horizontal|N/A|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

### <a name="business-critical-service-tier"></a>Camada de serviços críticos de negócio
|Nível de desempenho|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Memória (GB)|7|14|28|56|112|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|1|2|4|8|20|
|Tipo de armazenamento|SSD ligado|SSD ligado|SSD ligado|SSD ligado|SSD ligado|
|Tamanho máximo de dados (GB)|1024|1024|1024|1024|1024|
|Tamanho máximo de registo|307|307|307|461|614|
|TempDB size(DB)|32|64|128|256|384|
|Destino IOPS|320|640|1280|2560|5120|
|Latência de e/s (aproximada)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|
|Funcionários em simultâneo de máx. (pedidos)|210|420|840|1680|3360|
|Máx. permitido sessões|30000|30000|30000|30000|30000|
|Densidade de agrupamento máx.|N/A|50|100|100|100|
|Clique em-paragem do conjunto elástico mínimo/máximo|0.25, 0, 1 a 0,5|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|
|Várias AZ|Sim|Sim|Sim|Sim|Sim|
|Leitura de escalamento horizontal|Sim|Sim|Sim|Sim|Sim|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||
Se todos os vCores de um conjunto elástico estiver ocupados, cada base de dados no conjunto recebe uma quantidade de recursos de computação para processar consultas igual. O serviço Base de Dados SQL fornece equidade de partilha de recursos entre bases de dados, garantindo frações iguais de tempo de computação. Partilha Justiça de recursos de agrupamento elástico está além de qualquer número de recursos garantido caso contrário, cada base de dados quando o mínimo de vCore por base de dados está definido como um valor diferente de zero.

### <a name="database-properties-for-pooled-databases"></a>Propriedades de base de dados para bases de dados agrupados

A tabela seguinte descreve as propriedades para bases de dados agrupados.

| Propriedade | Descrição |
|:--- |:--- |
| VCores máx. por base de dados |O número máximo de vCores que poderá utilizar qualquer base de dados no agrupamento, se disponíveis com base na utilização por outras bases de dados no conjunto. VCores máx. por base de dados não é uma garantia de recursos para uma base de dados. Esta definição é uma definição global que se aplica a todas as bases de dados no conjunto. Definir vCores máximo por suficientemente alto para processar picos na utilização de base de dados da base de dados. É esperado algum grau de consolidação excessiva, uma vez que, geralmente, o conjunto assume padrões de utilização a frio e a quente para bases de dados em que todas as bases de dados não atingem o pico em simultâneo.|
| VCores Mín por base de dados |O número mínimo de vCores qualquer base de dados no conjunto é garantida. Esta definição é uma definição global que se aplica a todas as bases de dados no conjunto. O vCores Mín por base de dados pode ser definido como 0 e também é o valor predefinido. Esta propriedade está definida como em qualquer lugar, entre 0 e a utilização média vCores por base de dados. O produto do número de bases de dados no agrupamento e o vCores Mín por base de dados não pode exceder vCores por agrupamento.|
| Armazenamento máximo por base de dados |O tamanho máximo da base de dados definido pelo utilizador para uma base de dados num agrupamento. Bases de dados agrupados partilham do armazenamento de agrupamento alocados, pelo que o tamanho pode aceder uma base de dados é limitado a base de dados de tamanho e agrupamento de armazenamento o menor dos restantes. Tamanho máximo de base de dados refere-se ao tamanho máximo dos ficheiros de dados e não incluem o espaço utilizado por ficheiros de registo. |
|||
 
## <a name="elastic-pool-change-storage-size"></a>Agrupamento elástico: Altere o tamanho de armazenamento

- É possível aprovisionar armazenamento até ao limite de tamanho máximo: 
 - Para armazenamento Standard, aumentar ou diminuir o tamanho em incrementos de 10 GB
 - Para o Premium storage, aumentar ou diminuir o tamanho em incrementos de 250 GB
- Armazenamento de um conjunto elástico pode ser aprovisionado pelos aumentar ou diminuir o tamanho máximo utilizando o [portal do Azure](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), a [CLI do Azure](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), ou o [ REST API](/rest/api/sql/elasticpools/update).
- O preço do armazenamento de um conjunto elástico é a quantidade de armazenamento multiplicada pelo preço de unidade de armazenamento da camada de serviços. Para obter detalhes sobre os preços de armazenamento adicional, consulte [preços da SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="elastic-pool-change-vcores"></a>Agrupamento elástico: alterar vCores

Pode aumentar ou diminuir o nível de desempenho para um conjunto elástico com base no recurso necessidades utilizando o [portal do Azure](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), a [CLI do Azure](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), ou o [REST API](/rest/api/sql/elasticpools/update).

- Quando rescaling vCores de agrupamento, ligações de base de dados que são ignoradas. Este é o mesmo comportamento como ocorre quando rescaling DTUs para uma base de dados (não num agrupamento). Para obter detalhes sobre a duração e o impacto de ignorados ligações para uma base de dados durante as operações de rescaling, consulte [Rescaling DTUs para uma base de dados único](#single-database-change-storage-size). 
- A duração como rescale vCores agrupamento pode depender a quantidade total de espaço de armazenamento utilizada por todas as bases de dados no conjunto. Em geral, a latência rescaling averages 90 minutos ou menos por 100 GB. Por exemplo, se o espaço total utilizado por todas as bases de dados no conjunto é 200 GB, então, a latência esperada para rescaling conjunto é 3 horas ou menos. Em alguns casos dentro o escalão Standard ou Basic, a latência rescaling pode estar em cinco minutos, independentemente da quantidade de espaço utilizado.
- Em geral, a duração para alterar o vCores Mín por base de dados ou max vCores por base de dados é de cinco minutos ou menos.
- Quando downsizing vCores de agrupamento, o espaço do agrupamento utilizado tem de ser menor que o tamanho máximo permitido da camada e agrupamento vCores serviço de destino.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>O que é o número máximo de servidores e bases de dados?

| Máximo | Valor |
| :--- | :--- |
| Bases de dados por servidor | 5000 |
| Número de servidores por subscrição por região | 20 |
|||

> [!IMPORTANT]
> Como o número de bases de dados se aproxima o valor limite por servidor, pode ocorrer o seguinte:
> <br> • Latência aumento na execução de consultas na base de dados mestra.  Isto inclui a vistas de estatísticas de utilização de recursos, tais como resource_stats.
> <br> • Aumentando a latência das operações de gestão e compor viewpoints portais que envolvam a enumerar as bases de dados no servidor.

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>O que acontece quando são atingidos e limites de recursos de conjunto elástico da base de dados?

### <a name="compute-vcores"></a>(VCores) de computação

Quando a utilização de computação de base de dados (medida pela utilização de vCore) se tornar elevada, consultar aumentos de latência e pode, mesmo que o limite de tempo. Nas seguintes condições, as consultas podem ser colocados em fila pelo serviço e são fornecidas os recursos para execução como recurso ficar livres.
Quando encontrar a utilização de computação de elevado, as opções de mitigação incluem:

- Aumentar o nível de desempenho da base de dados ou do conjunto elástico para fornecer a base de dados com mais vCores. Consulte [única base de dados: alterar cVcores](#single-database-change-vcores) e [conjunto elástico: alterar vCores](#elastic-pool-change-vcores).
- Otimizar as consultas para reduzir a utilização de recursos de cada consulta. Para obter mais informações, consulte [consulta Tuning/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Armazenamento

Quando utilizado de espaço de base de dados atinge o limite de tamanho máximo, insere a base de dados e as atualizações que aumentam o tamanho de dados falharem e os clientes recebem um [mensagem de erro](sql-database-develop-error-messages.md). SELECIONA a base de dados e eliminações continuam a ter êxito.

Quando encontrar a utilização do espaço elevada, opções de mitigação incluem:

- Aumentar o tamanho máximo da base de dados ou agrupamento elástico ou altere o nível de desempenho para aumentar o máximo de armazenamento. Consulte [limites de recursos baseados em vCore de base de dados SQL](sql-database-vcore-resource-limits.md).
- Se a base de dados está num conjunto elástico, em seguida, em alternativa a base de dados pode ser movido fora do conjunto para que o espaço de armazenamento não está partilhado com outras bases de dados.

### <a name="sessions-and-workers-requests"></a>Sessões e funcionários (pedidos) 

O número máximo de sessões e funcionários é determinado pelo nível de desempenho e o escalão de serviço. Novos pedidos são rejeitados quando são atingidos os limites de sessão ou de trabalho e os clientes recebem uma mensagem de erro. Enquanto o número de ligações disponíveis pode ser controlado através da aplicação, o número de funcionários em simultâneo, muitas vezes, é mais difícil estimar e controlar. Isto é particularmente verdadeiro durante períodos de carga máxima quando os limites de recursos de base de dados são atingidos e trabalhadores pile cópias de segurança devido a tempo consultas em execução. 

Quando encontrar elevada utilização de sessão ou de trabalho, opções de mitigação incluem:
- Aumentar o nível de desempenho ou camada de serviço do agrupamento de base de dados ou elástico. Consulte [única base de dados: Altere o tamanho de armazenamento](#single-database-change-storage-size), [única base de dados: alterar vCores](#single-database-change-vcores), [conjunto elástico: Altere o tamanho de armazenamento](#elastic-pool-change-storage-size), e [conjunto elástico: alterar vCores ](#elastic-pool-change-vcores).
- Otimizar as consultas para reduzir a utilização de recursos de cada consulta, se a causa da utilização de trabalho maior se dever a contenção de recursos de computação. Para obter mais informações, consulte [consulta Tuning/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Passos Seguintes

- Consulte [FAQ de base de dados do SQL Server](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites de serviço, quotas e restrições](../azure-subscription-service-limits.md).
