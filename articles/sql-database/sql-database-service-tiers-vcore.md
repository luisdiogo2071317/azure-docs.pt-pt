---
title: Serviço de base de dados SQL do Azure - vCore | Documentos da Microsoft
description: O modelo de compra baseado em vCore permite-lhe dimensionar recursos de computação e armazenamento, combine o desempenho no local e otimizar o preço de forma independente.
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: sashan, moslake
manager: craigg
ms.date: 10/04/2018
ms.openlocfilehash: 9056eb6d93b0ead2a62dc5f58984fa6342af2d8e
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801664"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Escolher uma camada de serviços de vCore, computação, memória, armazenamento e recursos de e/s

O modelo de compra baseado em vCore permite-lhe dimensionar recursos de computação e armazenamento, combine o desempenho no local e otimizar o preço de forma independente. Ele também permite-lhe escolher a geração de hardware:
- Gen 4 - até 24 CPUs lógicas com base na Intel E5-2673 v3 (Haswell) 2,4 GHz, vCore = 1 PP (núcleos físicos), 7 GB por núcleo, anexado SSD
- Geração 5 - até 80 CPUs lógicas com base na Intel E5-2673 v4 (Broadwell) 2,3 GHz, vCore = 1 LP (hyper-thread), 5.5. GB por núcleo, o rápido eNVM SSD

modelo de vCore também permite que use [Azure Hybrid Use Benefit para o SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) para obter poupanças de custos.

## <a name="service-tier-characteristics"></a>Características de camada de serviço

O modelo de vCore oferece dois escalões de serviço para fins gerais e crítico para a empresa. Escalões de serviço são diferenciados por uma variedade de tamanhos de computação, design de elevada disponibilidade, isolamento de falhas, tipos de armazenamento e o intervalo de e/s. O cliente tem de configurar separadamente o período de retenção e armazenamento necessário para cópias de segurança.

A tabela seguinte ajuda-o a compreender as diferenças entre estes dois escalões:

||**Fins gerais**|**Crítico para a empresa**|**Hiperescala (pré-visualização)**|
|---|---|---|---|
|Melhor para|A maioria das cargas de trabalho da empresa. Ofertas de orçamento orientadas opções de equilibradas e dimensionáveis de computação e armazenamento.|Aplicações empresariais com requisitos elevados de E/S. Oferece maior resiliência a falhas com várias réplicas isoladas.|A maioria das cargas de trabalho empresariais com requisitos de escala de leitura e de armazenamento altamente escalável|
|Computação|Gen4: vCore de 1 a 24<br/>Gen5: vCore de 1 a 80|Gen4: vCore de 1 a 24<br/>Gen5: vCore de 1 a 80|Gen4: vCore de 1 a 24<br/>Gen5: vCore de 1 a 80|
|Memória|Gen4: 7 GB por núcleo<br>Gen5: 5,5 GB por núcleo | Gen4: 7 GB por núcleo<br>Gen5: 5,5 GB por núcleo |Gen4: 7 GB por núcleo<br>Gen5: 5,5 GB por núcleo|
|Armazenamento|[O armazenamento remoto Premium](../virtual-machines/windows/premium-storage.md),<br/>Base de dados: 5 GB – 4 TB<br/>Instância gerida: 32 GB-8 TB |Armazenamento SSD local,<br/>Base de dados: 5 GB – 4 TB<br/>Instância gerida: 32 GB-4 TB |Flexível, o aumento automático de armazenamento conforme necessário. Suporta até 100 TB de armazenamento e muito mais. Armazenamento SSD local para a cache de pool de local buffer e o armazenamento de dados local. Armazenamento remoto do Azure como arquivo de dados de longo prazo final. |
|Débito de e/s (aproximado)|Base de dados: 500 IOPS por vCore com IOPS máximos de 7000</br>Instância gerida: Depende [tamanho do ficheiro](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes)|5000 IOPS por núcleo com 200 000 IOPS máximos|TBD|
|Disponibilidade|1 réplica, sem uma escala de leitura|3 réplicas, 1 [réplica de uma escala de leitura](sql-database-read-scale-out.md),<br/>HA com redundância de zona|?|
|Cópias de segurança|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 dias (7 dias por predefinição)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 dias (7 dias por predefinição)|com base em instantâneo de cópia de segurança no armazenamento remoto do Azure e restaurações utilizar estes instantâneos para a recuperação rápida. As cópias de segurança são instantâneas e não afetam o desempenho de e/s de computação. Restauros são muito rápida e não do tamanho das operações de dados (em minutos não horas/dias).|
|Dentro da memória|Não suportado|Suportadas|Não suportado|
|||

Para obter mais informações, consulte [limites de recursos de vCore na base de dados única](sql-database-vcore-resource-limits-single-databases.md) e [limites de recursos de vCore na instância gerida](sql-database-managed-instance.md#vcore-based-purchasing-model). 

> [!IMPORTANT]
> Se precisar de menos de um vCore de capacidade de computação, utilize o modelo de compra baseado em DTU.

Ver [FAQ da base de dados de SQL](sql-database-faq.md) para obter respostas a perguntas mais frequentes. 

## <a name="storage-considerations"></a>Considerações sobre armazenamento

### <a name="general-purpose-and-business-critical-service-tiers"></a>Escalões de serviço crítico para a empresa e de finalidade gerais
Considere o seguinte:
- O armazenamento atribuído é utilizado por ficheiros de dados (. MDF) e ficheiros de registo de ficheiros (LDF).
- Cada base de dados de computação tamanho suporta um tamanho máximo da base de dados, com o tamanho máximo predefinido de 32 GB.
- Quando configura o tamanho de base de dados necessária (tamanho do MDF), 30% de armazenamento adicional é adicionado automaticamente para suportar LDF
- Tamanho de armazenamento na instância gerida tem de ser especificado em múltiplos de 32 GB.
- Pode escolher qualquer tamanho de base de dados entre 10 GB e o máximo suportado
 - Para o armazenamento Standard, aumentar ou diminuir o tamanho em incrementos de 10 GB
 - Para armazenamento Premium, aumentar ou diminuir o tamanho em incrementos de 250 GB
- No escalão de serviço para fins gerais, `tempdb` utiliza SSD anexado e este custo de armazenamento está incluído no preço vCore.
- No escalão de serviço crítico para a empresa, `tempdb` partilhas o SSD anexado com os arquivos MDF e LDF e o custo de armazenamento de tempDB está incluído no preço vCore.

> [!IMPORTANT]
> É cobrado o armazenamento total alocado para MDF e LDF.

Para monitorizar o tamanho total atual do MDF e LDF, utilize [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Para monitorizar o tamanho atual dos arquivos MDF e LDF individuais, utilize [database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> Em algumas circunstâncias, poderá ter reduzir uma base de dados para recuperar espaço não utilizado. Para obter mais informações, consulte [gerir o espaço de ficheiro na base de dados do Azure SQL](sql-database-file-space-management.md).

### <a name="hyperscale-service-tier-preview"></a>Camada de serviços de Hiperescala (pré-visualização)

O armazenamento é gerida automaticamente da base de dados de grande escala. Armazenamento aumenta conforme necessário. Armazenamento de "Infinito Log" no armazenamento de rápida do Azure premium SSD com nenhuma truncagem de registos frequentes necessário.

## <a name="backups-and-storage"></a>Cópias de segurança e armazenamento

### <a name="general-purpose-and-business-critical-service-tiers"></a>Escalões de serviço crítico para a empresa e de finalidade gerais

Armazenamento para cópias de segurança da base de dados é alocado para suportar o ponto no tempo de restauro (PITR) e [longo período de retenção (LTR)](sql-database-long-term-retention.md) capacidades de base de dados SQL. Este armazenamento é alocado em separado para cada base de dados e faturado como duas Cobranças separado por base de dados. 

- **PITR**: cópias de segurança da base de dados individuais são copiadas para [armazenamento RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md) são automaticamente. O tamanho de armazenamento aumenta dinamicamente à medida que novas cópias de segurança são criadas.  O armazenamento é utilizado por cópias de segurança completas semanais, cópias de segurança diferenciais diárias e cópias de segurança de registo de transações copiadas a cada 5 minutos. O consumo de armazenamento depende da taxa de alteração da base de dados e o período de retenção. Pode configurar um período de retenção separado para cada base de dados entre 7 e 35 dias. Uma quantidade mínima de armazenamento igual a 1 x do tamanho dos dados é fornecida sem custos adicionais. A maioria das bases de dados, este é o valor é o suficiente para armazenar a 7 dias de cópias de segurança.
- **LTR**: base de dados SQL oferece a opção de configurar a retenção de longa duração das cópias de segurança completas para até 10 anos. Se a política LTR estiver ativada, as cópias de segurança theses são armazenadas no armazenamento RA-GRS automaticamente, mas pode controlar a frequência com que as cópias de segurança são copiadas. Para cumprir o requisito de conformidade diferentes, pode selecionar os períodos de retenção diferentes para cópias de segurança semanais, mensais e/ou anuais. Esta configuração irá definir a quantidade de armazenamento será utilizada para as cópias de segurança LTR. Pode utilizar a Calculadora de preços de LTR para estimar os custos de armazenamento de LTR. Para obter mais informações, veja [Retenção de longa duração](sql-database-long-term-retention.md).

### <a name="hyperscale-service-tier-preview"></a>Camada de serviços de Hiperescala (pré-visualização)

A camada de serviços de hiperescala, as cópias de segurança são baseadas em instantâneos e armazenados no armazenamento remoto do Azure. Restauros utilizam estes instantâneos para a recuperação rápida. As cópias de segurança são instantâneas e não afetam o desempenho de e/s de computação. Restauros são muito rápida e não do tamanho das operações de dados (em minutos não horas/dias).

## <a name="azure-hybrid-use-benefit"></a>Benefício Híbrido do Azure

No modelo de compra baseado em vCore, podem trocar suas licenças existentes para as tarifas com desconto na base de dados SQL com o [Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Este benefício do Azure permite-lhe utilizar as suas licenças do SQL Server no local para economizar até 30% na base de dados do Azure SQL com as suas licenças do SQL Server no local com Software Assurance.

![preços](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-from-dtu-model-to-vcore-model"></a>Migração do modelo DTU para o modelo de vCore

### <a name="migration-of-single-databases-with-geo-replication-links"></a>Migração de bases de dados individuais com ligações de georreplicação

Migrar para o modelo baseado em DTU modelo baseado em vCore é semelhante a atualizar ou fazer downgrade as relações de georreplicação entre bases de dados Standard e Premium. Não requer a terminar a georreplicação, mas o usuário deve observar as regras de sequenciamento. Ao atualizar, tem de atualizar a base de dados secundária primeiro e, em seguida, atualizar o principal. Ao fazer downgrade, inverter a ordem: tem de mudar para a versão da base de dados principal primeiro e, em seguida, mudar o secundário. 

Ao utilizar a georreplicação entre dois conjuntos elásticos, recomenda-se que designar um conjunto como principal e o outro – como o secundário. Nesse caso, migração de dados elásticas devem utilizar a mesma diretriz.  No entanto, é tecnicamente é possível que um conjunto elástico contém as bases de dados primárias e secundárias. Neste caso migrar corretamente deve tratar o conjunto com a utilização mais alta como "principal" e seguir as regras de sequenciamento em conformidade.  

A tabela seguinte fornece orientações para os cenários de migração específica: 

|Escalão de serviço atual|Escalão de serviço de destino|Tipo de migração|Ações de utilizador|
|---|---|---|---|
|Standard|Fins gerais|Lateral|Pode migrar em qualquer ordem, mas certifique-se de um dimensionamento apropriado vCore *|
|Premium|Crítico para a Empresa|Lateral|Pode migrar em qualquer ordem, mas certifique-se de dimensionamento apropriado vCore *|
|Standard|Crítico para a Empresa|Atualizar|Deve migrar primeiro secundário|
|Crítico para a Empresa|Standard|Mudar para uma versão anterior|Deve migrar primeiro primário|
|Premium|Fins gerais|Mudar para uma versão anterior|Deve migrar primeiro primário|
|Fins gerais|Premium|Atualizar|Deve migrar primeiro secundário|
|Crítico para a Empresa|Fins gerais|Mudar para uma versão anterior|Deve migrar primeiro primário|
|Fins gerais|Crítico para a Empresa|Atualizar|Deve migrar primeiro secundário|
||||

\* Cada 100 DTUS no escalão Standard requer, pelo menos, 1 vCore e cada 125 DTU no escalão Premium requer, pelo menos, 1 vCore

### <a name="migration-of-failover-groups"></a>Migração de grupos de ativação pós-falha 

Migração de grupos de ativação pós-falha com várias bases de dados requer individual migração de bases de dados primárias e secundárias. Durante esse processo, as mesmas considerações e regras de sequenciamento aplicam-se. Depois das bases de dados são convertidos para o modelo baseado em vCore, o grupo de ativação pós-falha permanecerão em vigor com as mesmas definições de política. 

### <a name="creation-of-a-geo-replication-secondary"></a>Criação de uma secundária de georreplicação

Só pode criar usando o mesmo escalão de serviço como principal geo-secundária. Para a base de dados com a taxa de geração de log elevada, recomenda-se que o elemento secundário é criado com o mesmo tamanho de computação como principal. Se estiver a criar geo-secundária do conjunto elástico para um único banco de dados principal, recomenda-se que o conjunto tem o `maxVCore` definição que coincide com o tamanho de computação de base de dados primária. Se estiver a criar geo-secundária do conjunto elástico para um site primário em outro conjunto elástico, recomenda-se que os agrupamentos têm a mesma `maxVCore` definições

### <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Utilizando a cópia de base de dados para converter um banco de dados baseado em DTU para uma base de dados baseado em vCore.

Pode copiar qualquer base de dados com um tamanho de computação baseado em DTU para uma base de dados com um tamanho de computação baseada em vCore sem restrições ou sequenciamento especial, desde que o tamanho de computação de destino suporta o tamanho máximo da base de dados da base de dados de origem. Isto acontece porque a cópia da base de dados cria um instantâneo dos dados desde a hora de início da operação de cópia e não efetua a sincronização de dados entre a origem e de destino. 

## <a name="next-steps"></a>Passos Seguintes

- Para obter detalhes sobre específicas de computação tamanhos e opções de tamanho de armazenamento disponíveis para a base de dados, consulte [limites de recursos baseados em vCore de base de dados SQL para bases de dados individuais](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)
- Para obter detalhes sobre específicos de tamanhos de computação e ver opções de tamanho de armazenamento disponíveis para conjuntos elásticos [limites de recursos baseados em vcore da base de dados SQL para conjuntos elásticos](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
