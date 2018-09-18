---
title: Limites de recursos baseados em vcore da base de dados SQL do Azure - conjuntos elásticos | Documentos da Microsoft
description: Esta página descreve alguns limites de recursos baseados em vCore comuns para conjuntos elásticos na base de dados do Azure SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: carlrab
ms.openlocfilehash: 19693e394b0bd3a589ead25e2ddfa2fa88e92bad
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730384"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools"></a>Base de dados do SQL do Azure limites de modelo para conjuntos elásticos de compra baseado em vCore

Este artigo fornece os limites de recursos detalhados para conjuntos elásticos da base de dados do Azure SQL e bases de dados agrupadas com o modelo de compra baseado em vCore.

Para baseado em DTU limites de modelo compra, veja [limites de recursos baseados em DTU de base de dados do SQL - conjuntos elásticos](sql-database-dtu-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> Em algumas circunstâncias, poderá ter reduzir uma base de dados para recuperar espaço não utilizado. Para obter mais informações, consulte [gerir o espaço de ficheiro na base de dados do Azure SQL](sql-database-file-space-management.md).

## <a name="elastic-pool-storage-sizes-and-compute-sizes"></a>Conjunto elástico: tamanhos de armazenamento e tamanhos de computação

Para conjuntos elásticos da base de dados SQL, as tabelas seguintes mostram os recursos disponíveis em cada escalão de serviço e tamanho de computação. Pode definir o escalão de serviço, o tamanho de computação e quantidade de armazenamento utilizando o [portal do Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), o [da CLI do Azure](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), ou o [API de REST](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Os limites de recursos de bases de dados individuais em conjuntos elásticos são geralmente as mesmas usadas para bases de dados individuais fora agrupamentos que tem o mesmo tamanho de computação. Por exemplo, os máximos de trabalhadores simultâneos para uma base de dados GP_Gen4_1 é de 200 funções de trabalho. Então, os máximos de trabalhadores simultâneos para uma base de dados num conjunto GP_Gen4_1 também é de 200 funções de trabalho. Tenha em atenção de que o número total de trabalhadores simultâneos no agrupamento de GP_Gen4_1 é 210.

### <a name="general-purpose-service-tier"></a>Escalão de serviço de fins geral

#### <a name="generation-4-compute-platform"></a>Plataforma de computação de geração 4
|Tamanho de computação|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Memória (GB)|7|14|28|56|112|168|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento em OLTP na memória (GB)|N/A|N/D|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|
|Tamanho máximo de dados (GB)|512|756|1536|2048|3584|4096|
|Tamanho máximo do registo|154|227|461|614|1075|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Destino IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|
|Máximo de trabalhadores simultâneos (pedidos)|210|420|840|1680|3360|5040|
|Máximo permitido de sessões|30000|30000|30000|30000|30000|30000|
|Número de DBs máximo por conjunto|100|200|500|500|500|500|
|Opções de vcore de conjuntos elásticos Mín/Máx por base de dados|0, 0,25, 0,5, 1|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|
|Número de réplicas|1|1|1|1|1|1|
|Multi-AZ|N/A|N/D|N/D|N/D|N/D|N/A|
|Leia o Escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de computação de geração 5
|Tamanho de computação|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Geração de H/W|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Memória (GB)|11|22|44|88|132|176|220|440|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento em OLTP na memória (GB)|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|
|Tamanho máximo de dados (GB)|512|756|1536|2048|3072|4096|4096|4096|
|Tamanho máximo do registo|154|227|461|614|922|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Destino IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|
|Máximo de trabalhadores simultâneos (pedidos)|210|420|840|1680|2520|3360|4200|8400
|Máximo permitido de sessões|30000|30000|30000|30000|30000|30000|30000|30000|
|Número de DBs máximo por conjunto|100|200|500|500|500|500|500|500|
|Opções de vcore de conjuntos elásticos Mín/Máx por base de dados|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Número de réplicas|1|1|1|1|1|1|1|1|
|Multi-AZ|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Leia o Escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|
|||

### <a name="business-critical-service-tier"></a>Camada de serviços críticos de negócio

#### <a name="generation-4-compute-platform"></a>Plataforma de computação de geração 4
|Tamanho de computação|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Memória (GB)|7|14|28|56|112|168|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento em OLTP na memória (GB)|1|2|4|8|20|36|
|Tipo de armazenamento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamanho máximo de dados (GB)|1024|1024|1024|1024|1024|1024|
|Tamanho máximo do registo|307|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|384|
|Destino IOPS (64 KB)|5000|10000|20000|40000|80000|120000|
|Latência de e/s (aproximada)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|
|Máximo de trabalhadores simultâneos (pedidos)|210|420|840|1680|3360|5040|
|Máximo permitido de sessões|30000|30000|30000|30000|30000|30000|
|Número de DBs máximo por conjunto|Apenas únicos bds são suportadas para este tamanho de computação|50|100|100|100|100|
|Opções de vcore de conjuntos elásticos Mín/Máx por base de dados|N/A|0, 0,25, 0,5, 1, 2|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|
|Número de réplicas|3|3|3|3|3|3|
|Multi-AZ|N/A|N/D|N/D|N/D|N/D|N/A|
|Leia o Escalamento horizontal|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento de cópia de segurança incluído|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de computação de geração 5
|Tamanho de computação|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|Geração de H/W|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Memória (GB)|11|22|44|88|132|176|220|440|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento em OLTP na memória (GB)|1.571|3.142|6.284|15.768|25.252|37.936|52.22|131.64|
|Tipo de armazenamento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latência de e/s (aproximada)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|
|Tamanho máximo de dados (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Tamanho máximo do registo|307|307|307|307|614|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Destino IOPS (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|Máximo de trabalhadores simultâneos (pedidos)|210|420|840|1680|2520|3360|5040|8400|
|Máximo permitido de sessões|30000|30000|30000|30000|30000|30000|30000|30000|
|Número de DBs máximo por conjunto|N/A|50|100|100|100|100|100|100|
|Opções de vcore de conjuntos elásticos Mín/Máx por base de dados|N/A|0, 0,25, 0,5, 1, 2, 4|0, 0,25, 0,5, 1, 2, 4, 8|0, 0,25, 0,5, 1, 2, 4, 8, 16|0, 0,25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Número de réplicas|3|3|3|3|3|3|3|3|
|Multi-AZ|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Leia o Escalamento horizontal|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento de cópia de segurança incluído|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|
|||

Se todos os vCores de um conjunto elástico está ocupados, cada base de dados no conjunto recebe uma quantidade igual de recursos de computação para processar consultas. O serviço Base de Dados SQL fornece equidade de partilha de recursos entre bases de dados, garantindo frações iguais de tempo de computação. Recurso de conjunto elástico equidade de partilha é qualquer quantidade de recursos garantido de outro modo para cada base de dados quando o mínimo de vCore por base de dados está definido como um valor diferente de zero.

### <a name="database-properties-for-pooled-databases"></a>Propriedades de base de dados para bases de dados agrupadas

A tabela seguinte descreve as propriedades de bases de dados agrupadas.

| Propriedade | Descrição |
|:--- |:--- |
| VCores máximo por base de dados |O número máximo de vCores que qualquer base de dados no conjunto pode utilizar, caso estejam disponíveis com base na utilização por outras bases de dados no conjunto. VCores máximo por base de dados não é uma garantia de recurso para uma base de dados. Esta definição é uma definição global que se aplica a todas as bases de dados no conjunto. Definir vCores máximo por base de dados alto o suficiente para lidar com picos na utilização da base de dados. É esperado algum grau de consolidação excessiva, uma vez que, geralmente, o conjunto assume padrões de utilização a frio e a quente para bases de dados em que todas as bases de dados não atingem o pico em simultâneo.|
| Min vCores por base de dados |O número mínimo de vCores que é a garantia de qualquer base de dados no conjunto. Esta definição é uma definição global que se aplica a todas as bases de dados no conjunto. Os vCores min por base de dados pode ser definidos como 0 e também é o valor predefinido. Esta propriedade é definida como qualquer valor entre 0 e a utilização de vCores média por base de dados. O produto do número de bases de dados no conjunto e os vCores min por base de dados não pode exceder os vCores por conjunto.|
| Armazenamento máximo por base de dados |O tamanho de base de dados máximo definido pelo utilizador para uma base de dados num conjunto. Bases de dados agrupadas partilham o armazenamento de agrupamento alocados, pelo que o tamanho pode chegar a uma base de dados é limitado ao agrupamento de armazenamento mais pequeno de restantes e o tamanho de base de dados. Tamanho da base de dados de máx. refere-se ao tamanho máximo dos ficheiros de dados e não inclui o espaço utilizado por ficheiros de registo. |
|||
 
## <a name="next-steps"></a>Passos Seguintes

- Ver [FAQ da base de dados de SQL](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Ver [limites de recursos de visão geral do Azure SQL Database](sql-database-resource-limits.md) para obter informações sobre os limites nos níveis de servidor e de subscrição.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites do serviço, quotas e restrições](../azure-subscription-service-limits.md).
