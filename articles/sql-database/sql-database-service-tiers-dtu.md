---
title: Escalões de serviço de base de dados SQL do Azure - DTU | Microsoft Docs
description: Saiba mais sobre os escalões de serviço para único e bases de dados de conjunto para fornecer os níveis de desempenho e tamanhos de armazenamento.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: cf17ec616819da94678f2ae4f0f0ca283f99f629
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750429"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>Escolher uma camada de serviço com base em DTU, nível de desempenho e recursos de armazenamento 

Escalões de serviço são diferenciadas por um intervalo de níveis de desempenho com uma quantidade fixa de armazenamento incluída, fixo período de retenção de cópias de segurança e preço fixo. Todos os escalões de serviço fornecem flexibilidade de alterar os níveis de desempenho sem períodos de indisponibilidade. Bases de dados individuais e conjuntos elásticos são faturados por hora com base no nível de desempenho e o escalão de serviço.

> [!IMPORTANT]
> Instância de geridos de base de dados de SQL, atualmente em pré-visualização pública não suporta um modelo de compra baseado em DTU. Para obter mais informações, consulte [da base de dados geridas por instância de SQL do Azure](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>Escolher uma camada de serviço com base em DTU

Escolher uma camada de serviço depende principalmente continuidade do negócio, armazenamento e requisitos de desempenho.
||Básica|Standard|Premium|
| :-- | --: |--:| --:| --:| 
|Carga de trabalho de destino|Desenvolvimento e de produção|Desenvolvimento e de produção|Desenvolvimento e de produção||
|SLA de Tempo de Atividade|99,99%|99,99%|99,99%|N/d enquanto em pré-visualização|
|Retenção da cópia de segurança|7 dias|35 dias|35 dias|
|CPU|Baixa|Baixa, média, alta|Média, alta|
|Débito de e/s (aproximado) |2.5 IOPS por DTU| 2.5 IOPS por DTU | 48 IOPS por DTU|
|Latência de e/s (aproximada)|ms 5 (leitura), 10 ms (escrita)|ms 5 (leitura), 10 ms (escrita)|2 ms (leitura/escrita)|
|A indexação Columnstore |N/A|S3 e posterior|Suportadas|
|OLTP na memória|N/A|N/A|Suportadas|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>Limites DTU e armazenamento de base de dados individual

Os níveis de desempenho são expressos em Unidades de Transação de Base de Dados (DTUs) para bases de dados únicas e Unidades de Transação de Bases de Dados elásticas (eDTUs) para conjuntos elásticos. Para mais informações sobre as DTUs e eDTUs, consulte [quais são as DTUs e eDTUs](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?

||Básica|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Tamanho máximo de armazenamento | 2GB | 1 TB | 4 TB  | 
| DTUs máximas | 5 | 3000 | 4000 | |
||||||

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>Limites de base de dados agrupados, de armazenamento e de eDTU do conjunto elástico

| | **Básica** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Tamanho máximo de armazenamento por base de dados  | 2GB | 1 TB | 1 TB | 
| Tamanho máximo de armazenamento por agrupamento | 156 GB | 4 TB | 4 TB | 
| EDTUs máximo por base de dados | 5 | 3000 | 4000 | 
| Máximo eDTUs por conjunto | 1600 | 3000 | 4000 | 
| Número máximo de bases de dados por agrupamento | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> Mais de 1 TB de armazenamento no escalão Premium está atualmente disponível em todas as regiões, exceto o seguinte: norte do RU Central EUA oeste, RU South2, leste da China, USDoDCentral, Datacenters Central, USDoDEast, GOV us Southwest, nos us Sul Central, Nordeste da Alemanha, China Norte, US de e.u. a leste Noutras regiões, o armazenamento máximo no escalão Premium está limitado a 1 TB. Ver [Limitações Atuais P11-P15](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="next-steps"></a>Passos Seguintes

- Para obter detalhes sobre as opções de tamanho de armazenamento disponíveis para bases de dados individuais e níveis de desempenho específicos, consulte [limites de recursos baseados em SQL da base de dados DTU das bases de dados](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels).
- Para obter detalhes sobre as opções de tamanho de armazenamento disponíveis para conjuntos elásticos e níveis de desempenho específicos, consulte [dos limites de DTU de base de dados do SQL Server com base em recursos](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).