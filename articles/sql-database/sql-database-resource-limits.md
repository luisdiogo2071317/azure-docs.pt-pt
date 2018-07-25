---
title: Descrição geral de limites de recursos de base de dados SQL do Azure | Documentos da Microsoft
description: Esta página descreve alguns limites comuns do recurso baseado em DTU para bases de dados individuais na base de dados do Azure SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/02/2018
ms.author: carlrab
ms.openlocfilehash: 62b0639f134a134739b09593a0b21b47d06699dc
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236929"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Limites de recursos de base de dados do Azure SQL de descrição geral 

Este artigo fornece uma descrição geral do recurso de base de dados do Azure SQL limita e fornece informações sobre o que acontece quando esses limites de recursos são atingidos ou excedidos.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>O que é o número máximo de servidores e bases de dados?

| Máximo | Valor |
| :--- | :--- |
| Bases de dados por servidor | 5000 |
| Número de servidores por subscrição em qualquer região predefinido | 20 |
| Número máx. de servidores por subscrição em qualquer região | 200 |
| DTU / quota de eDTU por servidor | 54,000 |
|||

> [!NOTE]
> Para obter mais quota de /eDTU DTU ou mais servidores que o valor predefinido, pode ser submetido um novo pedido de suporte no portal do Azure para a subscrição com o tipo de problema "Quota". As DTU / limite de quota e base de dados de eDTU por servidor restringe o número de conjuntos elásticos por servidor. 

> [!IMPORTANT]
> Como o número de bases de dados se aproxima do limite por servidor, pode ocorrer o seguinte:
> - A aumentar a latência em execução de consultas na base de dados mestra.  Isto inclui as vistas de estatísticas de utilização de recursos, tais como resource_stats.
> - Aumentar a latência nas operações de gestão e composição de pontos de vista portais que envolvem a enumerar as bases de dados no servidor.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>O que acontece quando atingir os limites de recursos da base de dados?

### <a name="compute-dtus-and-edtus--vcores"></a>Computação (DTUs e eDTUs / vCores)

Quando a utilização de computação de base de dados (medida por DTUs e eDTUs ou vCores) se tornar elevada, aumentos de latência de consulta e pode até mesmo limite de tempo. Nestas condições, consultas podem ser colocados em fila pelo serviço e são fornecidas a recursos para execução como recurso estar livres.
Quando se deparar com a utilização de computação de alto, as opções de atenuação incluem:

- Aumentar o nível de desempenho da base de dados ou conjunto elástico para fornecer a base de dados com mais recursos de computação. Ver [Dimensionar recursos de base de dados individual](sql-database-single-database-scale.md) e [dimensionar os recursos do conjunto elástico](sql-database-elastic-pool-scale.md).
- Otimizar as consultas para reduzir a utilização de recursos de cada consulta. Para obter mais informações, consulte [ajuste de consulta/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Armazenamento

Quando utilizado de espaço de base de dados atinge o limite de tamanho máximo, insere a base de dados e as atualizações que aumentam o tamanho de dados falharem e os clientes recebem um [mensagem de erro](sql-database-develop-error-messages.md). SELECIONA de base de dados e eliminações continuam a ter êxito.

Quando se deparar com utilização elevada de espaço, as opções de atenuação incluem:

- Aumentar o tamanho máximo da base de dados ou elástica do agrupamento ou adicionar mais armazenamento. Ver [Dimensionar recursos de base de dados individual](sql-database-single-database-scale.md) e [dimensionar os recursos do conjunto elástico](sql-database-elastic-pool-scale.md).
- Se a base de dados num conjunto elástico, em seguida, em alternativa a base de dados pode ser movida fora do conjunto, para que o seu espaço de armazenamento não é partilhado com outras bases de dados.

### <a name="sessions-and-workers-requests"></a>Sessões e funções de trabalho (pedidos) 

O número máximo de sessões e funções de trabalho é determinado pelo desempenho e a camada de nível de serviço (DTUs e eDTUs). Novos pedidos são rejeitados quando atingir os limites de sessão ou de trabalho e os clientes recebem uma mensagem de erro. Embora o número de ligações disponíveis pode ser controlado pelo aplicativo, o número de trabalhadores simultâneos, muitas vezes, é mais difícil de fazer uma estimativa e controle. Isso é especialmente verdadeiro durante períodos de carga de pico quando atingir os limites de recursos da base de dados e funções de trabalho acumuladas devido a consultas mais tempo de execução. 

Quando se deparar com alta utilização de sessão ou de trabalho, as opções de atenuação incluem:
- Aumentar o nível de desempenho ou camada de serviço do conjunto elástico ou base de dados. Ver [Dimensionar recursos de base de dados individual](sql-database-single-database-scale.md) e [dimensionar os recursos do conjunto elástico](sql-database-elastic-pool-scale.md).
- Otimização de consultas para reduzir a utilização de recursos de cada consulta, se a causa da utilização da função de trabalho maior é devido à contenção de recursos de computação. Para obter mais informações, consulte [ajuste de consulta/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Passos Seguintes

- Ver [FAQ da base de dados de SQL](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites do serviço, quotas e restrições](../azure-subscription-service-limits.md).
- Para obter informações sobre DTUs e eDTUs, veja [DTUs e eDTUs](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Para obter informações sobre limites de tamanho de tempdb, consulte https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
