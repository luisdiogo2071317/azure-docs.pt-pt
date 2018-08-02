---
title: Escalões de serviço de base de dados SQL do Azure - DTU | Documentos da Microsoft
description: Saiba mais sobre escalões de serviço para único e de bases de dados do conjunto para fornecer níveis de desempenho e tamanhos de armazenamento.
services: sql-database
author: sachinpMSFT
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 5d16763fc8f3331082b98216d25190b945d95b60
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/02/2018
ms.locfileid: "39411825"
---
# <a name="choosing-a-dtu-based-service-tier-performance-level-and-storage-resources"></a>Escolher uma camada de serviço baseado em DTU, nível de desempenho e recursos de armazenamento 

Escalões de serviço são diferenciados por uma variedade de níveis de desempenho com uma quantidade fixa de armazenamento incluído, foi corrigido o período de retenção para cópias de segurança e o preço fixo. Os escalões de serviço oferecem a flexibilidade de alterar os níveis de desempenho sem tempo de inatividade. Conjuntos elásticos e bases de dados individuais são faturados por hora com base no nível de desempenho e a camada de serviço.

> [!IMPORTANT]
> Instância de gerida de base de dados de SQL, atualmente em pré-visualização pública não suporta um modelo de compra baseado em DTU. Para obter mais informações, consulte [instância gerida da base de dados SQL do Azure](sql-database-managed-instance.md). 

## <a name="choosing-a-dtu-based-service-tier"></a>Escolher uma camada de serviço baseado em DTU

Escolher uma camada de serviço depende principalmente continuidade do negócio, armazenamento e requisitos de desempenho.
||Básica|Standard|Premium|
| :-- | --: |--:| --:| --:| 
|Carga de trabalho de destino|Desenvolvimento e produção|Desenvolvimento e produção|Desenvolvimento e produção||
|SLA de Tempo de Atividade|99,99%|99,99%|99,99%|N/d enquanto está em pré-visualização|
|Retenção da cópia de segurança|7 dias|35 dias|35 dias|
|CPU|Baixa|Baixa, média, alta|Médio, alto|
|Débito de e/s (aproximado) |2,5 IOPS de por DTU| 2,5 IOPS de por DTU | 48 IOPS por DTU|
|Latência de e/s (aproximada)|5 ms (ler), 10 ms (escrita)|5 ms (ler), 10 ms (escrita)|2 ms (leitura/escrita)|
|Indexação Columnstore |N/A|S3 e posterior|Suportadas|
|OLTP dentro da memória|N/A|N/A|Suportadas|
|||||

## <a name="single-database-dtu-and-storage-limits"></a>Base de dados DTUS e limites de armazenamento

Os níveis de desempenho são expressos em Unidades de Transação de Base de Dados (DTUs) para bases de dados únicas e Unidades de Transação de Bases de Dados elásticas (eDTUs) para conjuntos elásticos. Para obter mais informações sobre DTUs e eDTUs, veja [quais são DTUs e eDTUs](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?

||Básica|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Tamanho máximo de armazenamento | 2 GB | 1 TB | 4 TB  | 
| Limite máximo de DTUs | 5 | 3000 | 4000 | |
||||||

> [!IMPORTANT]
> Em algumas circunstâncias, poderá ter reduzir uma base de dados para recuperar espaço não utilizado. Para obter mais informações, consulte [gerir o espaço de ficheiro na base de dados do Azure SQL](sql-database-file-space-management.md).

## <a name="elastic-pool-edtu-storage-and-pooled-database-limits"></a>EDTU do conjunto elástico, armazenamento e limites de bases de dados agrupadas

| | **Básica** | **Standard** | **Premium** | 
| :-- | --: | --: | --: | --: |
| Tamanho de armazenamento máximo por base de dados  | 2 GB | 1 TB | 1 TB | 
| Tamanho de armazenamento máximo por conjunto | 156 GB | 4 TB | 4 TB | 
| Máximo eDTUs por base de dados | 5 | 3000 | 4000 | 
| Máximo eDTUs por conjunto | 1600 | 3000 | 4000 | 
| Número máximo de bases de dados por conjunto | 500  | 500 | 100 | 
||||||

> [!IMPORTANT]
> Mais de 1 TB de armazenamento no escalão Premium está atualmente disponível em todas as regiões, exceto as seguintes: e.u.a. centro-oeste, leste da China, USDoDCentral, Alemanha Central, Sudoeste do USDoDEast, US Gov, USGov Iowa, Nordeste da Alemanha, Norte da China. Noutras regiões, o armazenamento máximo no escalão Premium está limitado a 1 TB. Ver [Limitações Atuais P11-P15](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

> [!IMPORTANT]
> Em algumas circunstâncias, poderá ter reduzir uma base de dados para recuperar espaço não utilizado. Para obter mais informações, consulte [gerir o espaço de ficheiro na base de dados do Azure SQL](sql-database-file-space-management.md).

## <a name="next-steps"></a>Passos Seguintes

- Para obter detalhes sobre os níveis de desempenho específicos e opções de tamanho de armazenamento disponíveis para bases de dados individuais, consulte [limites de recursos baseados em DTU de base de dados SQL para bases de dados individuais](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels).
- Para obter detalhes sobre os níveis de desempenho específicos e opções de tamanho de armazenamento disponíveis para conjuntos elásticos, veja [limites dos recursos baseados em DTU da base de dados de SQL](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).
