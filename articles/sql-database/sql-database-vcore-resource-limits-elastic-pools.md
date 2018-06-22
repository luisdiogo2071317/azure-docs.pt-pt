---
title: Limites de recursos baseados em vCore de base de dados SQL do Azure - conjuntos elásticos | Microsoft Docs
description: Esta página descreve alguns limites de recursos baseados em vCore comuns para conjuntos elásticos na SQL Database do Azure.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 01f213c7cf5f6be3ef84601a50bb4455422faf22
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309919"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools-preview"></a>Base de dados do SQL do Azure baseados em vCore compra limites de modelo para conjuntos elásticos (pré-visualização)

Este artigo fornece os limites de recursos de detalhado para conjuntos elásticos SQL Database do Azure e agrupados bases de dados utilizando o modelo de compra baseado em vCore.

Para baseado em DTU compra modelo os limites, consulte [limites de recursos baseados em SQL da base de dados DTU - conjuntos elásticos](sql-database-dtu-resource-limits-elastic-pools.md).


## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Agrupamento elástico: tamanhos de armazenamento e níveis de desempenho

Para conjuntos elásticos SQL Database, as tabelas seguintes mostram os recursos disponíveis em cada nível de desempenho e o escalão de serviço. Pode definir a camada de serviço, nível de desempenho e quantidade de armazenamento utilizando o [portal do Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), a [CLI do Azure](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), ou o [API de REST](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Os limites de recursos de bases de dados individuais em conjuntos elásticos, geralmente, são iguais às bases de dados individuais fora agrupamentos que tem o mesmo nível de desempenho. Por exemplo, os trabalhadores simultâneos máximas para uma base de dados GP_Gen4_1 é 200 workers. Por isso, os trabalhadores simultâneos máximas para uma base de dados num agrupamento GP_Gen4_1 também é 200 workers. Tenha em atenção de que o número total de trabalhadores simultâneas no agrupamento de GP_Gen4_1 é 210.

### <a name="general-purpose-service-tier"></a>Camada de serviço de objetivo geral

#### <a name="generation-4-compute-platform"></a>Plataforma de computação de geração 4
|Nível de desempenho|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Memória (GB)|7|14|28|56|112|168|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|N/A|N/D|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|
|Tamanho máximo de dados (GB)|512|756|1536|2048|3584|4096|
|Tamanho máximo de registo|154|227|461|614|1075|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Destino IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|
|Funcionários em simultâneo de máx. (pedidos)|210|420|840|1680|3360|5040|
|Máx. permitido sessões|30000|30000|30000|30000|30000|30000|
|Densidade de agrupamento máx.|100|200|500|500|500|500|
|Clique em-paragem do conjunto elástico mínimo/máximo|0.25, 0, 1 a 0,5|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|
|Número de réplicas|1|1|1|1|1|1|
|Várias AZ|N/A|N/D|N/D|N/D|N/D|N/A|
|Leitura de escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de computação de geração 5
|Nível de desempenho|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Geração de H/W|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Memória (GB)|11|22|44|88|132|176|220|440|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|
|Tamanho máximo de dados (GB)|512|756|1536|2048|3072|4096|4096|4096|
|Tamanho máximo de registo|154|227|461|614|922|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Destino IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|
|Funcionários em simultâneo de máx. (pedidos)|210|420|840|1680|2520|3360|4200|8400
|Máx. permitido sessões|30000|30000|30000|30000|30000|30000|30000|30000|
|Densidade de agrupamento máx.|100|200|500|500|500|500|500|500|
|Clique em-paragem do conjunto elástico mínimo/máximo|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Número de réplicas|1|1|1|1|1|1|1|1|
|Várias AZ|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Leitura de escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

### <a name="business-critical-service-tier"></a>Camada de serviços críticos de negócio

#### <a name="generation-4-compute-platform"></a>Plataforma de computação de geração 4
|Nível de desempenho|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Memória (GB)|7|14|28|56|112|168|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|1|2|4|8|20|36|
|Tipo de armazenamento|SSD Local|SSD Local|SSD Local|SSD Local|SSD Local|SSD Local|
|Tamanho máximo de dados (GB)|1024|1024|1024|1024|1024|1024|
|Tamanho máximo de registo|307|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|384|
|Destino IOPS (64 KB)|5000|10000|20000|40000|80000|120000|
|Latência de e/s (aproximada)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|
|Funcionários em simultâneo de máx. (pedidos)|210|420|840|1680|3360|5040|
|Máx. permitido sessões|30000|30000|30000|30000|30000|30000|
|Densidade de agrupamento máx.|N/A|50|100|100|100|100|
|Clique em-paragem do conjunto elástico mínimo/máximo|N/A|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|
|Número de réplicas|3|3|3|3|3|3|
|Várias AZ|N/A|N/D|N/D|N/D|N/D|N/A|
|Leitura de escalamento horizontal|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de computação de geração 5
|Nível de desempenho|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Geração de H/W|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Memória (GB)|11|22|44|88|132|176|220|440|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|1.571|3.142|6.284|15.768|25.252|37.936|52.22|131.64|
|Tipo de armazenamento|SSD Local|SSD Local|SSD Local|SSD Local|SSD Local|SSD Local|SSD Local|SSD Local|
|Latência de e/s (aproximada)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|1-2 ms (escrita)<br>ms 1-2 (leitura)|
|Tamanho máximo de dados (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Tamanho máximo de registo|307|307|307|307|614|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Destino IOPS (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|Funcionários em simultâneo de máx. (pedidos)|210|420|840|1680|2520|3360|5040|8400|
|Máx. permitido sessões|30000|30000|30000|30000|30000|30000|30000|30000|
|Densidade de agrupamento máx.|N/A|50|100|100|100|100|100|100|
|Clique em-paragem do conjunto elástico mínimo/máximo|N/A|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Número de réplicas|3|3|3|3|3|3|3|3|
|Várias AZ|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Leitura de escalamento horizontal|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
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
 
## <a name="next-steps"></a>Passos Seguintes

- Consulte [FAQ de base de dados do SQL Server](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites de serviço, quotas e restrições](../azure-subscription-service-limits.md).
