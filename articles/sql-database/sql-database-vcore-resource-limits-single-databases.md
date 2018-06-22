---
title: Limites de recursos baseados em vCore de base de dados SQL do Azure - base de dados individual | Microsoft Docs
description: Esta página descreve alguns limites de recursos baseados em vCore comuns para uma base de dados na base de dados do Azure SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 1a14e1a7c50f458067491a8605a0518056ac0aa8
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309898"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database-preview"></a>Base de dados do SQL do Azure baseados em vCore compra limites de modelo para uma base de dados (pré-visualização)

Este artigo fornece os limites de recursos de detalhado SQL Database do Azure das bases de dados utilizando o modelo de compra baseado em vCore.

Para baseado em DTU compra modelo os limites, consulte [dos limites de DTU de base de dados do SQL Server com base em recursos](sql-database-dtu-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Base de dados individual: tamanhos de armazenamento e níveis de desempenho

Para bases de dados individuais, as tabelas seguintes mostram os recursos disponíveis para uma base de dados a cada nível de desempenho e o escalão de serviço. Pode definir a camada de serviço, o nível de desempenho e a quantidade de armazenamento para uma única base de dados utilizando o [portal do Azure](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-servers-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-servers-databases-manage.md#powershell-manage-logical-servers-and-databases), o [CLI do azure](sql-database-servers-databases-manage.md#azure-cli-manage-logical-servers-and-databases), ou o [REST API](sql-database-servers-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="general-purpose-service-tier"></a>Camada de serviço de objetivo geral

#### <a name="generation-4-compute-platform"></a>Plataforma de computação de geração 4
|Nível de desempenho|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Memória (GB)|7|14|28|56|112|168|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|N/A|N/D|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|
|Tamanho máximo de dados (GB)|1024|1024|1536|3072|4096|4096|
|Tamanho máximo de registo|307|307|461|922|1229|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Destino IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|Funcionários em simultâneo de máx. (pedidos)|200|400|800|1600|3200|4800|
|Máx. permitido sessões|30000|30000|30000|30000|30000|30000|
|Número de réplicas|1|1|1|1|1|1|
|Várias AZ|N/A|N/D|N/D|N/D|N/D|N/A|000
|Leitura de escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de computação de geração 5
|Nível de desempenho|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|Geração de H/W|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Memória (GB)|11|22|44|88|132|176|220|440|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento OLTP dentro da memória (GB)|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|Armazenamento (remota) Premium|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|ms de 5 a 7 (escrita)<br>ms 5-10 (leitura)|
|Tamanho máximo de dados (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Tamanho máximo de registo|307|307|461|614|1229|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Destino IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Funcionários em simultâneo de máx. (pedidos)|200|400|800|1600|2400|3200|4000|8000|
|Máx. permitido sessões|30000|30000|30000|30000|30000|30000|30000|30000|
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
|Funcionários em simultâneo de máx. (pedidos)|200|400|800|1600|3200|4800|
|Máx. permitido sessões|30000|30000|30000|30000|30000|30000|
|Número de réplicas|3|3|3|3|3|3|
|Várias AZ|N/A|N/D|N/D|N/D|N/D|N/A|
|Leitura de escalamento horizontal|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de computação de geração 5
|Nível de desempenho|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
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
|Funcionários em simultâneo de máx. (pedidos)|200|400|800|1600|2400|3200|4000|8000|
|Máx. permitido sessões|30000|30000|30000|30000|30000|30000|30000|30000|
|Número de réplicas|3|3|3|3|3|3|3|3|
|Várias AZ|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Leitura de escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|Tamanho de base de dados X 1|
|||

## <a name="next-steps"></a>Passos Seguintes

- Consulte [FAQ de base de dados do SQL Server](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites de serviço, quotas e restrições](../azure-subscription-service-limits.md).
