---
title: Limites de recurso baseado em vcore da base de dados SQL do Azure - base de dados | Documentos da Microsoft
description: Esta página descreve alguns limites de recursos baseados em vCore comuns para uma base de dados na base de dados do Azure SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/24/2018
ms.author: carlrab
ms.openlocfilehash: 8e7af24133b7e034bb7bc1898609cebbb860b738
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/24/2018
ms.locfileid: "42919083"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Base de dados do SQL do Azure limites de modelo para uma base de dados de compra baseado em vCore

Este artigo fornece os limites de recursos detalhados para a base de dados do Azure SQL únicas bases de dados com o modelo de compra baseado em vCore.

Para baseado em DTU limites de modelo compra, veja [limites dos recursos baseados em DTU da base de dados de SQL](sql-database-dtu-resource-limits.md).

> [!IMPORTANT]
> Em algumas circunstâncias, poderá ter reduzir uma base de dados para recuperar espaço não utilizado. Para obter mais informações, consulte [gerir o espaço de ficheiro na base de dados do Azure SQL](sql-database-file-space-management.md).


## <a name="single-database-storage-sizes-and-performance-levels"></a>Base de dados: tamanhos de armazenamento e níveis de desempenho

Para bases de dados individuais, as tabelas seguintes mostram os recursos disponíveis para uma base de dados em cada nível de desempenho e a camada de serviço. Pode definir a camada de serviços, nível de desempenho e a quantidade de armazenamento para uma base de dados individual utilizando o [portal do Azure](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), o [Da CLI do azure](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases), ou o [REST API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="general-purpose-service-tier"></a>Escalão de serviço de fins geral

#### <a name="generation-4-compute-platform"></a>Plataforma de computação de geração 4
|Nível de desempenho|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Memória (GB)|7|14|28|56|112|168|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento em OLTP na memória (GB)|N/A|N/D|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|
|Tamanho máximo de dados (GB)|1024|1024|1536|3072|4096|4096|
|Tamanho máximo do registo (GB)|307|307|461|922|1229|1229|
|TempDB tamanho (GB)|32|64|128|256|384|384|
|Destino IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|Máximo de trabalhadores simultâneos (pedidos)|200|400|800|1600|3200|4800|
|Máximo permitido de sessões|30000|30000|30000|30000|30000|30000|
|Número de réplicas|1|1|1|1|1|1|
|Multi-AZ|N/A|N/D|N/D|N/D|N/D|N/A|000
|Leia o Escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de computação de geração 5
|Nível de desempenho|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|Geração de H/W|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Memória (GB)|11|22|44|88|132|176|220|440|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento em OLTP na memória (GB)|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Tipo de armazenamento|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|Armazenamento Premium (remoto)|
|Latência de e/s (aproximada)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|ms de 5 a 7 (escrita)<br>ms de 5 a 10 (ler)|
|Tamanho máximo de dados (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Tamanho máximo do registo (GB)|307|307|461|614|1229|1229|1229|1229|
|TempDB tamanho (GB)|64|128|256|384|384|384|384|384|
|Destino IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Máximo de trabalhadores simultâneos (pedidos)|200|400|800|1600|2400|3200|4000|8000|
|Máximo permitido de sessões|30000|30000|30000|30000|30000|30000|30000|30000|
|Número de réplicas|1|1|1|1|1|1|1|1|
|Multi-AZ|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Leia o Escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|
|||

### <a name="business-critical-service-tier"></a>Camada de serviços críticos de negócio

#### <a name="generation-4-compute-platform"></a>Plataforma de computação de geração 4
|Nível de desempenho|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|Geração de H/W|4|4|4|4|4|4|
|vCores|1|2|4|8|16|24|
|Memória (GB)|7|14|28|56|112|168|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento em OLTP na memória (GB)|1|2|4|8|20|36|
|Tipo de armazenamento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Tamanho máximo de dados (GB)|1024|1024|1024|1024|1024|1024|
|Tamanho máximo do registo (GB)|307|307|307|307|307|307|
|TempDB tamanho (GB)|32|64|128|256|384|384|
|Destino IOPS (64 KB)|5000|10000|20000|40000|80000|120000|
|Latência de e/s (aproximada)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|
|Máximo de trabalhadores simultâneos (pedidos)|200|400|800|1600|3200|4800|
|Máximo permitido de sessões|30000|30000|30000|30000|30000|30000|
|Número de réplicas|3|3|3|3|3|3|
|Multi-AZ|N/A|N/D|N/D|N/D|N/D|N/A|
|Leia o Escalamento horizontal|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento de cópia de segurança incluído|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|
|||

#### <a name="generation-5-compute-platform"></a>Plataforma de computação de geração 5
|Nível de desempenho|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|Geração de H/W|5|5|5|5|5|5|5|5|
|vCores|2|4|8|16|24|32|40|80|
|Memória (GB)|11|22|44|88|132|176|220|440|
|Suporte de Columnstore|Sim|Sim|Sim|Sim|Sim|Sim|Sim|Sim|
|Armazenamento em OLTP na memória (GB)|1.571|3.142|6.284|15.768|25.252|37.936|52.22|131.64|
|Tipo de armazenamento|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|SSD local|
|Latência de e/s (aproximada)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|1 a 2 ms (escrita)<br>ms de 1 a 2 (ler)|
|Tamanho máximo de dados (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Tamanho máximo do registo (GB)|307|307|307|307|614|1229|1229|1229|
|TempDB tamanho (GB)|64|128|256|384|384|384|384|384|
|Destino IOPS (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|Máximo de trabalhadores simultâneos (pedidos)|200|400|800|1600|2400|3200|4000|8000|
|Máximo permitido de sessões|30000|30000|30000|30000|30000|30000|30000|30000|
|Número de réplicas|3|3|3|3|3|3|3|3|
|Multi-AZ|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Leia o Escalamento horizontal|N/A|N/D|N/D|N/D|N/D|N/D|N/D|N/A|
|Armazenamento de cópia de segurança incluído|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|Tamanho de BD de X 1|
|||

## <a name="next-steps"></a>Passos Seguintes

- Ver [FAQ da base de dados de SQL](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites do serviço, quotas e restrições](../azure-subscription-service-limits.md).
