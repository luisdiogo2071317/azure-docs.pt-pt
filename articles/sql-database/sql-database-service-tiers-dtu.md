---
title: Serviço de base de dados SQL do Azure - DTU | Microsoft Docs
description: Saiba mais sobre os escalões de serviço para único e bases de dados de conjunto para fornecer os níveis de desempenho e tamanhos de armazenamento.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/09/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 6e282291a6e6e219bb275dd4da91f3815590adc1
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="dtu-based-purchasing-model-for-azure-sql-database"></a>Modelo de compra baseado em DTU para a SQL Database do Azure 


[Base de dados SQL do Azure](sql-database-technical-overview.md) oferece dois modelos de compras para computação, armazenamento e recursos de e/s: um modelo de compra baseado em DTU e um modelo de compra baseado em vCore (pré-visualização). A tabela e o gráfico seguinte comparam e contraste estes dois modelos de compras.

> [!IMPORTANT]
> Para vCore com base no modelo de compra (pré-visualização), consulte [vCore com base no modelo de compra](sql-database-service-tiers-vcore.md)


|**Modelo de compra**|**Descrição**|**Melhor opção para**|
|---|---|---|
|Modelo de DTU|Este modelo é baseado numa medida agrupada de computação, armazenamento e recursos de e/s. Os níveis de desempenho são expressos em Unidades de Transação de Base de Dados (DTUs) para bases de dados únicas e Unidades de Transação de Bases de Dados elásticas (eDTUs) para conjuntos elásticos. Para mais informações sobre as DTUs e eDTUs, consulte [quais são as DTUs e eDTUs](sql-database-what-is-a-dtu.md)?|Melhor para os clientes que pretendem simples, pré-configurados opções de recursos.| 
|com base em vCore modelo|Este modelo permite-lhe independentemente Dimensionar recursos de computação e armazenamento. Também permite-lhe utilizar o benefício de híbrida do Azure para o SQL Server para obter uma redução de custos.|Melhor opção para os clientes que valor flexibilidade, controlo e transparência.|
||||  

![modelo de preços](./media/sql-database-service-tiers/pricing-model.png)

## <a name="dtu-based-purchasing-model"></a>Modelo de compra baseado em DTU

A unidade de débito de base de dados (DTU) representa uma medida combinada de CPU, memória, lê e escreve. O modelo de compra baseado em DTU oferece um conjunto de pacotes pré-configuradas de recursos de computação e incluídos armazenamento em diferentes níveis de unidade do desempenho da aplicação. Os clientes que preferem simplicidade de um pacote pré-configurada e pagamentos fixos cada mês, pode localizar o modelo de DTU mais adequado para as suas necessidades. No modelo de compra baseado em DTU, os clientes podem optar entre **básico**, **padrão**, e **Premium** escalões de serviço para ambos [único bases de dados](sql-database-single-database-resources.md) e [conjuntos elásticos](sql-database-elastic-pool.md). Escalões de serviço são diferenciadas por um intervalo de níveis de desempenho com uma quantidade fixa de armazenamento incluída, fixo período de retenção de cópias de segurança e preço fixo. Todos os escalões de serviço fornecem flexibilidade de alterar os níveis de desempenho sem períodos de indisponibilidade. Bases de dados individuais e conjuntos elásticos são faturados por hora com base no nível de desempenho e o escalão de serviço.

> [!IMPORTANT]
> Instância de geridos de base de dados de SQL, atualmente em pré-visualização pública não suporta um modelo de compra baseado em DTU. Para obter mais informações, consulte [da base de dados geridas por instância de SQL do Azure](sql-database-managed-instance.md). 

## <a name="choosing-a-service-tier-in-the-dtu-based-purchasing-model"></a>Escolher uma camada de serviço no modelo de compra baseado em DTU

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

## <a name="performance-level-and-storage-size-limits-in-the-dtu-based-purchasing-model"></a>Limites de tamanho de armazenamento e de nível de desempenho no modelo de compra baseado em DTU

Os níveis de desempenho são expressos em Unidades de Transação de Base de Dados (DTUs) para bases de dados únicas e Unidades de Transação de Bases de Dados elásticas (eDTUs) para conjuntos elásticos. Para mais informações sobre as DTUs e eDTUs, consulte [quais são as DTUs e eDTUs](sql-database-what-is-a-dtu.md)?

### <a name="single-databases"></a>Bases de dados individuais

||Básica|Standard|Premium|
| :-- | --: | --: | --: | --: |
| Armazenamento máximo tamanho * | 2GB | 1 TB | 4 TB  | 
| DTUs máximas | 5 | 3000 | 4000 | |
||||||

Para obter detalhes sobre as opções de tamanho de armazenamento disponíveis para bases de dados individuais e níveis de desempenho específicos, consulte [limites de recursos baseados em SQL da base de dados DTU das bases de dados](sql-database-dtu-resource-limits.md#single-database-storage-sizes-and-performance-levels).

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
> -  Tamanhos de armazenamento maiores do que a quantidade de armazenamento incluído estão em pré-visualização e aplicam custos adicionais. Para detalhes, ver os preços da [Base de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/). 
> -  No escalão Premium, mais de 1 TB de armazenamento está atualmente disponível nas seguintes regiões: Leste da Austrália, Sudeste da Austrália, sul do Brasil, Canadá Central, Canadá leste, EUA Central, França Central, Datacenters Central, leste do Japão, oeste do Japão, Coreia Central, EUA Centro-Norte, Europa do Norte, EUA Centro-Sul, Sudeste asiático, sul do RU, RU oeste, E.U.A. East2, EUA oeste, E.U.A. us Virginia e Europa Ocidental. Ver [Limitações Atuais P11-P15](sql-database-dtu-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

Para obter detalhes sobre as opções de tamanho de armazenamento disponíveis para conjuntos elásticos e níveis de desempenho específicos, consulte [dos limites de DTU de base de dados do SQL Server com base em recursos](sql-database-dtu-resource-limits.md#elastic-pool-storage-sizes-and-performance-levels).



## <a name="next-steps"></a>Passos Seguintes

- Para obter detalhes sobre as opções de tamanho de armazenamento disponíveis e níveis de desempenho específicos, consulte [dos limites de DTU de base de dados do SQL Server com base em recursos](sql-database-dtu-resource-limits.md) e [limites de recursos baseados em vCore de base de dados SQL](sql-database-vcore-resource-limits.md).
- Consulte [FAQ de base de dados do SQL Server](sql-database-faq.md) para obter respostas a perguntas mais frequentes.
- Saiba mais sobre [subscrição do Azure e limites de serviço, Quotas e restrições](../azure-subscription-service-limits.md)
