---
title: Descrição geral de limites de recursos de base de dados SQL do Azure | Microsoft Docs
description: Esta página descreve alguns limites de recursos baseados em DTU comuns das bases de dados na base de dados do Azure SQL.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 6806b0c5b5e5ac5e1189f628786f0c8f9b223395
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750956"
---
# <a name="overview-azure-sql-database-resource-limits"></a>Limites de recursos de SQL Database do Azure de descrição geral 

Este artigo fornece uma descrição geral do recurso SQL Database do Azure limita e fornece informações sobre o que acontece quando esses limites de recursos são atingidos ou excedidos.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>O que é o número máximo de servidores e bases de dados?

| Máximo | Valor |
| :--- | :--- |
| Bases de dados por servidor | 5000 |
| Número predefinido de servidores por subscrição em qualquer região | 20 |
| Número máx. de servidores por subscrição em qualquer região | 200 |
|||

> [!NOTE]
> Para obter mais quota do servidor que o período predefinido, um novo pedido de suporte pode ser submetido no portal do Azure para a subscrição com o tipo de problema "A Quota de".

> [!IMPORTANT]
> Como o número de bases de dados se aproxima o valor limite por servidor, pode ocorrer o seguinte:
> - Aumentar a latência na execução de consultas na base de dados mestra.  Isto inclui a vistas de estatísticas de utilização de recursos, tais como resource_stats.
> - Aumentar a latência das operações de gestão e compor viewpoints portais que envolvam a enumerar as bases de dados no servidor.

## <a name="what-happens-when-database-resource-limits-are-reached"></a>O que acontece quando os limites de recursos de base de dados são atingidos?

### <a name="compute-dtus-and-edtus--vcores"></a>Computação (DTUs e eDTUs / vCores)

Quando a utilização de computação de base de dados (medida por DTUs e eDTUs ou vCores) se tornar elevada, consultar aumentos de latência e pode, mesmo que o limite de tempo. Nas seguintes condições, as consultas podem ser colocados em fila pelo serviço e são fornecidas os recursos para execução como recurso ficar livres.
Quando encontrar a utilização de computação de elevado, as opções de mitigação incluem:

- Aumentar o nível de desempenho da base de dados ou do conjunto elástico para fornecer a base de dados com mais recursos de computação. Consulte [Dimensionar recursos de base de dados individual](sql-database-single-database-scale.md) e [Dimensionar recursos de agrupamento elástico](sql-database-elastic-pool-scale.md).
- Otimizar as consultas para reduzir a utilização de recursos de cada consulta. Para obter mais informações, consulte [consulta Tuning/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Armazenamento

Quando utilizado de espaço de base de dados atinge o limite de tamanho máximo, insere a base de dados e as atualizações que aumentam o tamanho de dados falharem e os clientes recebem um [mensagem de erro](sql-database-develop-error-messages.md). SELECIONA a base de dados e eliminações continuam a ter êxito.

Quando encontrar a utilização do espaço elevada, opções de mitigação incluem:

- Aumentar o tamanho máximo da base de dados ou elástico agrupamento, ou adicione mais armazenamento. Consulte [Dimensionar recursos de base de dados individual](sql-database-single-database-scale.md) e [Dimensionar recursos de agrupamento elástico](sql-database-elastic-pool-scale.md).
- Se a base de dados está num conjunto elástico, em seguida, em alternativa a base de dados pode ser movido fora do conjunto para que o espaço de armazenamento não está partilhado com outras bases de dados.

### <a name="sessions-and-workers-requests"></a>Sessões e funcionários (pedidos) 

O número máximo de sessões e funcionários é determinado pelo desempenho e a camada de nível de serviço (DTUs e eDTUs). Novos pedidos são rejeitados quando são atingidos os limites de sessão ou de trabalho e os clientes recebem uma mensagem de erro. Enquanto o número de ligações disponíveis pode ser controlado através da aplicação, o número de funcionários em simultâneo, muitas vezes, é mais difícil estimar e controlar. Isto é particularmente verdadeiro durante períodos de carga máxima quando os limites de recursos de base de dados são atingidos e trabalhadores pile cópias de segurança devido a tempo consultas em execução. 

Quando encontrar elevada utilização de sessão ou de trabalho, opções de mitigação incluem:
- Aumentar o nível de desempenho ou camada de serviço do agrupamento de base de dados ou elástico. Consulte [Dimensionar recursos de base de dados individual](sql-database-single-database-scale.md) e [Dimensionar recursos de agrupamento elástico](sql-database-elastic-pool-scale.md).
- Otimizar as consultas para reduzir a utilização de recursos de cada consulta, se a causa da utilização de trabalho maior se dever a contenção de recursos de computação. Para obter mais informações, consulte [consulta Tuning/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

Quando encontrar elevada utilização de sessão ou de trabalho, opções de mitigação incluem:
- Aumentar o nível de desempenho ou camada de serviço da base de dados. Consulte [Dimensionar recursos de base de dados individual](sql-database-single-database-scale.md) e [Dimensionar recursos de agrupamento elástico](sql-database-elastic-pool-scale.md).
- Otimizar as consultas para reduzir a utilização de recursos de cada consulta, se a causa da utilização de trabalho maior se dever a contenção de recursos de computação. Para obter mais informações, consulte [consulta Tuning/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Passos Seguintes

- Consulte [FAQ de base de dados do SQL Server](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Para obter informações sobre os limites do Azure gerais, consulte [subscrição do Azure e limites de serviço, quotas e restrições](../azure-subscription-service-limits.md).
- Para obter informações sobre as DTUs e eDTUs, consulte [DTUs e eDTUs](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Para obter informações sobre limites de tamanho de tempdb, consulte https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
