---
title: "Serviço de base de dados SQL do Azure | Microsoft Docs"
description: "Saiba mais sobre os escalões de serviço para único e bases de dados de conjunto para fornecer os níveis de desempenho e tamanhos de armazenamento."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 02/28/2018
ms.author: carlrab
ms.openlocfilehash: ebfd44098a81b4a9d8863623458460ceb25f0d5d
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/08/2018
---
# <a name="what-are-azure-sql-database-service-tiers"></a>Quais são os escalões de serviço do SQL Database do Azure?

[Base de dados SQL do Azure](sql-database-technical-overview.md) oferece **básico**, **padrão**, e **Premium** escalões de serviço para ambos [único bases de dados](sql-database-single-database-resources.md) e [conjuntos elásticos](sql-database-elastic-pool.md). Base de dados do SQL Server oferece uma camada de serviços para fins gerais [da base de dados geridas por instância de SQL do Azure](sql-database-managed-instance.md#managed-instance-service-tier). Escalões de serviço são principalmente diferenciadas por um intervalo de nível de desempenho e opções de tamanho de armazenamento e preços.  Todos os escalões de serviço fornecem flexibilidade na alteração do tamanho de armazenamento e de nível de desempenho.  Bases de dados individuais e conjuntos elásticos são faturados por hora com base na camada de serviço, o nível de desempenho e o tamanho de armazenamento.   

> [!IMPORTANT]
> Instância de geridos de base de dados de SQL, atualmente em pré-visualização pública, oferece uma camada de serviços para fins gerais único. Para obter mais informações, consulte [da base de dados geridas por instância de SQL do Azure](sql-database-managed-instance.md). O resto deste artigo não é aplicável a instância geridos.

## <a name="choosing-a-service-tier"></a>Escolher uma camada de serviços

Escolher uma camada de serviço depende principalmente continuidade do negócio, armazenamento e requisitos de desempenho.
| | **Básica** | **Standard** |**Premium**  |
| :-- | --: |--:| --:| --:| 
|Carga de trabalho de destino|Desenvolvimento e de produção|Desenvolvimento e de produção|Desenvolvimento e de produção||
|SLA de Tempo de Atividade|99,99%|99,99%|99,99%|N/d enquanto em pré-visualização|
|Retenção da cópia de segurança|7 dias|35 dias|35 dias|
|CPU|Baixa|Baixa, média, alta|Média, alta|
|Débito de e/s (aproximado) |2.5 IOPS por DTU  | 2.5 IOPS por DTU | 48 IOPS por DTU|
|Latência de e/s (aproximada)|ms 5 (leitura), 10 ms (escrita)|ms 5 (leitura), 10 ms (escrita)|2 ms (leitura/escrita)|
|A indexação Columnstore e OLTP na memória|N/A|N/A|Suportadas|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Limites de tamanho de armazenamento e de nível de desempenho

Os níveis de desempenho são expressos em Unidades de Transação de Base de Dados (DTUs) para bases de dados únicas e Unidades de Transação de Bases de Dados elásticas (eDTUs) para conjuntos elásticos. Para mais informações sobre as DTUs e eDTUs, consulte [quais são as DTUs e eDTUs?](sql-database-what-is-a-dtu.md)

### <a name="single-databases"></a>Bases de dados individuais

|  | **Básica** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Armazenamento máximo tamanho * | 2GB | 1 TB | 4 TB  | 
| DTUs máximas | 5 | 3000 | 4000 | |
||||||

### <a name="elastic-pools"></a>Conjuntos elásticos

| | **Básica** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Tamanho máximo de armazenamento por base de dados *  | 2GB | 1 TB | 1 TB | 
| Tamanho máximo de armazenamento por agrupamento * | 156 GB | 4 TB | 4 TB | 
| EDTUs máximo por base de dados | 5 | 3000 | 4000 | 
| Máximo eDTUs por conjunto | 1600 | 3000 | 4000 | 
| Número máximo de bases de dados por agrupamento | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> \*Tamanhos de armazenamento maiores do que a quantidade de armazenamento incluído estão em pré-visualização e são aplicados custos adicionais. Para detalhes, ver os preços da [Base de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> \* No escalão Premium, mais de 1 TB de armazenamento está atualmente disponível nas seguintes regiões: Leste da Austrália, Sudeste da Austrália, Sul do Brasil, Canadá Central, Leste do Canadá, E.U.A. Central, Centro de França, Alemanha Central, Leste do Japão, Oeste do Japão, Coreia Central, E.U.A. Centro-Norte, Europa do Norte, E.U.A. Centro-Sul, Sudeste Asiático, Sul do Reino Unido, Oeste do Reino Unido, E.U.A. Leste 2, E.U.A. Oeste, Gov (US) - Virginia e Europa Ocidental. Ver [Limitações Atuais P11-P15](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Para obter detalhes sobre as opções de tamanho de armazenamento disponíveis e níveis de desempenho específicos, consulte [dos limites de recursos de base de dados SQL](sql-database-resource-limits.md).


## <a name="next-steps"></a>Passos Seguintes

- Saiba mais sobre [único recursos de base de dados](sql-database-single-database-resources.md).
- Saiba mais sobre conjuntos elásticos, consulte [conjuntos elásticos](sql-database-elastic-pool.md).
- Saiba mais sobre [subscrição do Azure e limites de serviço, Quotas e restrições](../azure-subscription-service-limits.md)
* Saiba mais sobre [DTUs e eDTUs](sql-database-what-is-a-dtu.md).
* Saiba mais sobre a monitorização de utilização, consulte [monitorização e a otimização de desempenho](sql-database-troubleshoot-performance.md).

