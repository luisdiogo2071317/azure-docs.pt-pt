---
title: Dimensionar recursos de agrupamento elástico - SQL Database do Azure | Microsoft Docs
description: Esta página descreve recursos dimensionamento para conjuntos elásticos na SQL Database do Azure.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 0d15382f413485ccc287bac945b3c88eb748a9f6
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313319"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Dimensionar recursos de agrupamento elástico de SQL Database do Azure

Este artigo descreve como dimensionar os recursos de armazenamento e computação disponíveis para conjuntos elásticos e bases de dados agrupados na SQL Database do Azure. 

## <a name="vcore-based-purchasing-model-change-elastic-pool-storage-size"></a>modelo de compra baseado em vCore: Altere o tamanho de armazenamento do conjunto elástico

- É possível aprovisionar armazenamento até ao limite de tamanho máximo: 
 - Para armazenamento Standard, aumentar ou diminuir o tamanho em incrementos de 10 GB
 - Para o Premium storage, aumentar ou diminuir o tamanho em incrementos de 250 GB
- Armazenamento de um conjunto elástico pode ser aprovisionado pelos aumentar ou diminuir o tamanho máximo.
- O preço do armazenamento de um conjunto elástico é a quantidade de armazenamento multiplicada pelo preço de unidade de armazenamento da camada de serviços. Para obter detalhes sobre os preços de armazenamento adicional, consulte [preços da SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="vcore-based-purchasing-model-change-elastic-pool-compute-resources-vcores"></a>modelo de compra baseado em vCore: (vCores) de recursos de computação de alteração do agrupamento elástica

Pode aumentar ou diminuir o nível de desempenho para um conjunto elástico com base no recurso necessidades utilizando o [portal do Azure](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), a [CLI do Azure](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), ou o [REST API](/rest/api/sql/elasticpools/update).

- Quando rescaling vCores de agrupamento, ligações de base de dados que são ignoradas. Este é o mesmo comportamento como ocorre quando rescaling DTUs para uma base de dados (não num agrupamento). Para obter detalhes sobre a duração e o impacto de ignorados ligações para uma base de dados durante as operações de rescaling, consulte [Rescaling DTUs para uma base de dados único](#single-database-change-storage-size). 
- A duração como rescale vCores agrupamento pode depender a quantidade total de espaço de armazenamento utilizada por todas as bases de dados no conjunto. Em geral, a latência rescaling averages 90 minutos ou menos por 100 GB. Por exemplo, se o espaço total utilizado por todas as bases de dados no conjunto é 200 GB, então, a latência esperada para rescaling conjunto é 3 horas ou menos. Em alguns casos dentro o escalão Standard ou Basic, a latência rescaling pode estar em cinco minutos, independentemente da quantidade de espaço utilizado.
- Em geral, a duração para alterar o vCores Mín por base de dados ou max vCores por base de dados é de cinco minutos ou menos.
- Quando downsizing vCores de agrupamento, o espaço do agrupamento utilizado tem de ser menor que o tamanho máximo permitido da camada e agrupamento vCores serviço de destino.

## <a name="dtu-based-purchasing-model-change-elastic-pool-storage-size"></a>Modelo de compra baseado em DTU: Altere o tamanho de armazenamento do conjunto elástico

- O preço de eDTU de um conjunto elástico inclui uma determinada quantidade de armazenamento sem custos adicionais. Armazenamento adicional para além da quantidade incluída pode ser aprovisionado um custo adicionais até ao limite de tamanho máximo em incrementos de 250 GB de cópia de segurança para 1 TB e, em seguida, em incrementos de 256 GB para além de 1 TB. Para quantidades incluídas do armazenamento e limites de tamanho máximo, consulte [conjunto elástico: tamanhos de armazenamento e níveis de desempenho](#elastic-pool-storage-sizes-and-performance-levels).
- Armazenamento adicional de um conjunto elástico pode ser aprovisionado pelos aumentar o tamanho máximo utilizando o [portal do Azure](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), a [CLI do Azure](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), ou o [REST API ](/rest/api/sql/elasticpools/update).
- O preço de armazenamento adicional de um conjunto elástico é a quantidade de armazenamento adicional multiplicada pelo preço de unidade de armazenamento adicional da camada de serviços. Para obter detalhes sobre os preços de armazenamento adicional, consulte [preços da SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="dtu-based-purchasing-model-change-elastic-pool-compute-resources-edtus"></a>Modelo de compra baseado em DTU: (eDTUs) de recursos de computação de alteração do agrupamento elástica

Pode aumentar ou diminuir os recursos disponíveis para um conjunto elástico com base no recurso necessidades utilizando o [portal do Azure](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), a [CLI do Azure](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), ou o [ REST API](/rest/api/sql/elasticpools/update).

- Quando rescaling eDTUs de conjunto, as ligações de base de dados que são ignoradas. Este é o mesmo comportamento como ocorre quando rescaling DTUs para uma base de dados (não num agrupamento). Para obter detalhes sobre a duração e o impacto de ignorados ligações para uma base de dados durante as operações de rescaling, consulte [Rescaling DTUs para uma base de dados único](#single-database-change-storage-size). 
- A duração como rescale eDTUs de conjunto pode depender a quantidade total de espaço de armazenamento utilizada por todas as bases de dados no conjunto. Em geral, a latência rescaling averages 90 minutos ou menos por 100 GB. Por exemplo, se o espaço total utilizado por todas as bases de dados no conjunto é 200 GB, então, a latência esperada para rescaling conjunto é 3 horas ou menos. Em alguns casos dentro o escalão Standard ou Basic, a latência rescaling pode estar em cinco minutos, independentemente da quantidade de espaço utilizado.
- Em geral, a duração para alterar as eDTUs Mín por base de dados ou o número máximo de eDTUs por base de dados é de cinco minutos ou menos.
- Quando downsizing eDTUs de conjunto, o espaço do agrupamento utilizado tem de ser menor que o tamanho máximo permitido das eDTUs de camada e um conjunto de serviço do destino.
- Quando rescaling eDTUs de conjunto, um custo de armazenamento adicional se aplica se (1) o tamanho máximo de armazenamento do conjunto é suportado pelo agrupamento de destino e o tamanho máximo (2) o armazenamento excede a quantidade de armazenamento incluídas de agrupamento de destino. Por exemplo, se um conjunto padrão com um tamanho máximo de 100 GB de eDTU 100 é downsized para um conjunto padrão de eDTU 50, em seguida, um custo de armazenamento adicional aplica-se, uma vez que o agrupamento de destino suporta um tamanho máximo de 100 GB e a quantidade de armazenamento incluída é apenas de 50 GB. Por isso, a quantidade de armazenamento adicional é 100 GB – 50 GB = 50 GB. Para obter preços de armazenamento adicional, consulte [preços da SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Se a quantidade real de espaço utilizado é inferior à quantidade incluída de armazenamento, em seguida, este custo extra pode ser evitada ao reduzir o tamanho máximo da base de dados para a quantidade incluída. 

## <a name="next-steps"></a>Passos Seguintes

Para geral dos limites de recursos, consulte [limites de recursos baseados em vCore de base de dados SQL - conjuntos elásticos](sql-database-vcore-resource-limits-elastic-pools.md) e [limites de recursos baseados em SQL da base de dados DTU - conjuntos elásticos](sql-database-dtu-resource-limits-elastic-pools.md).
