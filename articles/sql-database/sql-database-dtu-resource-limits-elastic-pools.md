---
title: Recursos baseados em DTU de base de dados SQL do Azure limita conjuntos elásticos | Microsoft Docs
description: Esta página descreve alguns limites de recursos baseados em DTU comuns para conjuntos elásticos na SQL Database do Azure.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 08dabf1ad66f69c5e0f55aedbc2a4d0bb265a0bd
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752236"
---
# <a name="resources-limits-for-elastic-pools-using-the-dtu-based-purchasing-model"></a>Limites de recursos para conjuntos elásticos utilizando o modelo de compra baseado em DTU 

Este artigo fornece os limites de recursos de detalhado para conjuntos elásticos SQL Database do Azure e agrupados bases de dados utilizando o modelo de compra baseado em DTU. 

Com base em DTU compra modelo recursos limites das bases de dados, consulte [limites de recursos baseados em DTU - bases de dados individuais](sql-database-vcore-resource-limits-elastic-pools.md). Para os limites de recursos baseados em vCore, consulte [limites de recursos baseados em vCore - bases de dados individuais](sql-database-vcore-resource-limits-single-databases.md) e [limites de recursos baseados em vCore - conjuntos elásticos](sql-database-vcore-resource-limits-elastic-pools.md).

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Agrupamento elástico: tamanhos de armazenamento e níveis de desempenho

Para conjuntos elásticos SQL Database, as tabelas seguintes mostram os recursos disponíveis em cada nível de desempenho e o escalão de serviço. Pode definir a camada de serviço, nível de desempenho e quantidade de armazenamento utilizando o [portal do Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), a [CLI do Azure](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), ou o [API de REST](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Os limites de recursos de bases de dados individuais em conjuntos elásticos, geralmente, são iguais às bases de dados individuais fora conjuntos com base no DTUs e a camada de serviço. Por exemplo, os trabalhadores simultâneos máximas para uma base de dados de S2 é 120 workers. Por isso, os trabalhadores simultâneos máximas para uma base de dados num conjunto Standard também é 120 trabalhadores se o máximo de DTUS por base de dados no conjunto é 50 DTUs (que é equivalente ao S2).

### <a name="basic-elastic-pool-limits"></a>Limites do conjunto elástico básico

| eDTUs por conjunto | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Armazenamento incluído por conjunto (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Opções de armazenamento máximo por conjunto (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Armazenamento máximo de memória OLTP por conjunto (GB) | N/A | N/D | N/D | N/D | N/D | N/D | N/D | N/A |
| Número de DBs máximo por conjunto | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Máximo de trabalhadores simultâneos (pedidos) por conjunto | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Máximo de sessões simultâneas por conjunto | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Opções de eDTUs Mín por base de dados | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Opções de eDTUs máx. por base de dados | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Armazenamento máximo por base de dados (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Limites do conjunto elástico padrão

| eDTUs por conjunto | **50** | **100** | **200** | **300** | **400** | **800**| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Armazenamento incluído por conjunto (GB) | 50 | 100 | 200 | 300 | 400 | 800 | 
| Opções de armazenamento máximo por conjunto (GB) | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 | 
| Armazenamento máximo de memória OLTP por conjunto (GB) | N/A | N/D | N/D | N/D | N/D | N/A | 
| Número de DBs máximo por conjunto | 100 | 200 | 500 | 500 | 500 | 500 | 
| Máximo de trabalhadores simultâneos (pedidos) por conjunto | 100 | 200 | 400 | 600 | 800 | 1600 |
| Máximo de sessões simultâneas por conjunto | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Opções de eDTUs Mín por base de dados | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Opções de eDTUs máx. por base de dados | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Armazenamento máximo por base de dados (GB) | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Limites do conjunto elástico standard (continuação) 

| eDTUs por conjunto | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Armazenamento incluído por conjunto (GB) | 1200 | 1600 | 2000 | 2500 | 3000 | 
| Opções de armazenamento máximo por conjunto (GB) | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| Armazenamento máximo de memória OLTP por conjunto (GB) | N/A | N/D | N/D | N/D | N/A | 
| Número de DBs máximo por conjunto | 500 | 500 | 500 | 500 | 500 | 
| Máximo de trabalhadores simultâneos (pedidos) por conjunto | 2400 | 3200 | 4000 | 5000 | 6000 |
| Máximo de sessões simultâneas por conjunto | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Opções de eDTUs Mín por base de dados | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Opções de eDTUs máx. por base de dados | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Opções de armazenamento máximo por base de dados (GB) | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Limites do conjunto elástico premium

| eDTUs por conjunto | **125** | **250** | **500** | **1000** | **1500**| 
|:---|---:|---:|---:| ---: | ---: | 
| Armazenamento incluído por conjunto (GB) | 250 | 500 | 750 | 1024 | 1536 | 
| Opções de armazenamento máximo por conjunto (GB) | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| Armazenamento máximo de memória OLTP por conjunto (GB) | 1 | 2 | 4 | 10 | 12 | 
| Número de DBs máximo por conjunto | 50 | 100 | 100 | 100 | 100 | 
| Máximo de trabalhadores simultâneos por conjunto (pedidos) | 200 | 400 | 800 | 1600 | 2400 | 
| Máximo de sessões simultâneas por conjunto | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Mínimo de eDTUs por base de dados | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000, 1500 | 
| Máximo de eDTUs por base de dados | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Armazenamento máximo por base de dados (GB) | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Limites do conjunto elástico premium (continuação) 

| eDTUs por conjunto | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: | 
| Armazenamento incluído por conjunto (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Opções de armazenamento máximo por conjunto (GB) | 2048 | 2560 | 3072 | 3548 | 4096|
| Armazenamento máximo de memória OLTP por conjunto (GB) | 16 | 20 | 24 | 28 | 32 |
| Número de DBs máximo por conjunto | 100 | 100 | 100 | 100 | 100 | 
| Máximo de trabalhadores simultâneos (pedidos) por conjunto | 3200 | 4000 | 4800 | 5600 | 6400 |
| Máximo de sessões simultâneas por conjunto | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Opções de eDTUs Mín por base de dados | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Opções de eDTUs máx. por base de dados | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Armazenamento máximo por base de dados (GB) | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

> [!IMPORTANT]
> Mais de 1 TB de armazenamento no escalão Premium está atualmente disponível em todas as regiões, exceto o seguinte: norte do RU Central EUA oeste, RU South2, leste da China, USDoDCentral, Datacenters Central, USDoDEast, GOV us Southwest, nos us Sul Central, Nordeste da Alemanha, China Norte, US de e.u. a leste Noutras regiões, o armazenamento máximo no escalão Premium está limitado a 1 TB. Ver [Limitações Atuais P11-P15](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

Se todas as DTUs de um conjunto elástico forem utilizadas, cada base de dados no conjunto recebe uma quantidade igual de recursos para processar consultas. O serviço Base de Dados SQL fornece equidade de partilha de recursos entre bases de dados, garantindo frações iguais de tempo de computação. A equidade de partilha de recursos de um conjunto elástico é adicional a qualquer quantidade de recursos garantido de outro modo a cada base de dados quando o mínimo de DTUs por base de dados está definido como um valor diferente de zero.

### <a name="database-properties-for-pooled-databases"></a>Propriedades de base de dados para bases de dados agrupados

A tabela seguinte descreve as propriedades para bases de dados agrupados.

| Propriedade | Descrição |
|:--- |:--- |
| Máximo de eDTUs por base de dados |O número máximo de eDTUs que qualquer base de dados no conjunto pode utilizar, caso estejam disponíveis com base na utilização por outras bases de dados no conjunto. O número máximo de eDTUs por base de dados não é uma garantia de recurso para uma base de dados. Esta definição é uma definição global que se aplica a todas as bases de dados no conjunto. Defina um número máximo de eDTUs suficientemente elevado para processar picos na utilização de base de dados. É esperado algum grau de consolidação excessiva, uma vez que, geralmente, o conjunto assume padrões de utilização a frio e a quente para bases de dados em que todas as bases de dados não atingem o pico em simultâneo. Por exemplo, suponha que o pico de utilização por base de dados é 20 eDTUs e apenas 20% das 100 bases de dados no conjunto atingem o pico ao mesmo tempo. Se o número máximo de eDTUs por base de dados estiver definido como 20 eDTUs, é razoável sobreconsolidar o conjunto em 5 vezes e definir o número de eDTUs por conjunto como 400. |
| Mínimo de eDTUs por base de dados |O número mínimo de eDTUs que é garantido a qualquer base de dados no conjunto. Esta definição é uma definição global que se aplica a todas as bases de dados no conjunto. O mínimo de eDTUs por base de dados pode ser definido como 0, que é também o valor predefinido. Esta propriedade é definida como qualquer valor entre 0 e a utilização média de eDTUs por base de dados. O produto do número de bases de dados no conjunto e o número mínimo de eDTUs por base de dados não pode exceder as eDTUs por conjunto. Por exemplo, se um conjunto tiver 20 bases de dados e o número mínimo de eDTUs por base de dados definido como 10 eDTUs, as eDTUs por conjunto têm de ser, pelo menos, 200 eDTUs. |
| Armazenamento máximo por base de dados |O tamanho máximo da base de dados definido pelo utilizador para uma base de dados num agrupamento. No entanto, as bases de dados agrupados partilham do armazenamento de agrupamento alocado. Mesmo que o armazenamento total máximo *por base de dados* está configurado para ser maior que o armazenamento disponível total *espaço do agrupamento de*, total de espaço, na verdade, utilizada por todas as bases de dados não poderá exceder a limite de agrupamento disponível. Tamanho máximo de base de dados refere-se ao tamanho máximo dos ficheiros de dados e não incluem o espaço utilizado por ficheiros de registo. |
|||
 


## <a name="next-steps"></a>Passos Seguintes

- Consulte [FAQ de base de dados do SQL Server](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites de serviço, quotas e restrições](../azure-subscription-service-limits.md).
- Para obter informações sobre as DTUs e eDTUs, consulte [DTUs e eDTUs](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Para obter informações sobre limites de tamanho de tempdb, consulte https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
