---
title: Serviço de base de dados SQL do Azure - vCore | Microsoft Docs
description: O modelo de compra baseado em vCore (pré-visualização) permite-lhe dimensionar recursos de armazenamento e computação independentemente, corresponde ao desempenho no local e otimizar o preço.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: bfa32796b40033a13d1ced9f8431bd19492e6498
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309584"
---
# <a name="choosing-a-vcore-service-tier-compute-memory-storage-and-io-resources"></a>Escolher uma camada de serviço vCore, de computação, memória, armazenamento e recursos de e/s

Escalões de serviço são diferenciadas por um intervalo de design de elevada disponibilidade, níveis de desempenho, tipos de armazenamento e isolamento de falhas e o intervalo de e/s. O cliente separadamente tem de configurar o período de retenção de armazenamento e necessário para cópias de segurança. Com o modelo de vCore único bases de dados e conjuntos elásticos são elegíveis para cópia de segurança para as poupanças de 30 por cento com o [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

A tabela seguinte ajuda-o a compreender as diferenças entre estas duas camadas:

||**Fins gerais**|**Crítico de negócio**|
|---|---|---|
|Melhor para|A maioria das cargas de trabalho de negócio. Ofertas budget orientado para opções de armazenamento e computação equilibradas e dimensionáveis.|Aplicações empresariais com requisitos elevados de E/S. Oferece maior resiliência a falhas com várias réplicas isoladas.|
|Computação|1 e 80 vCore, geração 4 e 5 de geração |1 e 80 vCore, geração 4 e 5 de geração|
|Memória|7 GB por núcleo |7 GB por núcleo |
|Armazenamento|Armazenamento Premium remoto, 5 GB – 4 TB|Armazenamento SSD local, 5 GB – 4 TB|
|Débito de e/s (aproximado)|500 IOPS por vCore com 7000 IOPS máximo|5000 IOPS por núcleos com 200000 IOPS máximo|
|Disponibilidade|1 réplica, sem dimensionamento de leitura|3 réplicas, 1 [leitura escala](sql-database-read-scale-out.md), HA redundante de zona|
|Cópias de segurança|RA-GRS, 7-35 dias (7 dias por predefinição)|RA-GRS, 7-35 dias (7 dias por predefinição) *|
|Dentro da memória|N/A|Suportadas|
|||

\* Durante a pré-visualização, o período de retenção de cópias de segurança não é configurável e é resolvido para 7 dias.

> [!IMPORTANT]
> Se precisar de menos de um vCore da capacidade de cálculo, utilize o modelo de compra baseado em DTU.

Consulte [FAQ de base de dados do SQL Server](sql-database-faq.md) para obter respostas a perguntas mais frequentes. 

## <a name="storage-considerations"></a>Considerações sobre armazenamento

Considere o seguinte:
- O armazenamento atribuído é utilizado por ficheiros de dados (MDF) e ficheiros de ficheiros (LDF) do registo.
- Cada nível de desempenho suporta um tamanho máximo da base de dados, com um tamanho máximo predefinido de 32 GB.
- Quando configura o tamanho de base de dados necessária (tamanho do MDF), 30% do armazenamento adicional for adicionado automaticamente para suportar LDF
- Pode selecionar qualquer dimensão de base de dados entre 10 GB e o máximo suportado
 - Para armazenamento Standard, aumentar ou diminuir o tamanho em incrementos de 10 GB
 - Para o Premium storage, aumentar ou diminuir o tamanho em incrementos de 250 GB
- Na camada de serviço de objetivo geral, `tempdb` utiliza um SSD anexado e este custo de armazenamento está incluído no preço vCore.
- Na camada de serviços críticos de negócio, `tempdb` partilhas SSD anexado com o armazenamento de tempDB custo e os ficheiros. MDF e LDF está incluído no preço vCore.

> [!IMPORTANT]
> São-lhe cobrados para o armazenamento total atribuído para MDF e LDF.

Para monitorizar o tamanho total atual do MDF e LDF, utilize [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Para monitorizar o tamanho atual de ficheiros MDF e LDF individuais, utilize [database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

## <a name="backups-and-storage"></a>As cópias de segurança e de armazenamento

Armazenamento de cópias de segurança da base de dados está alocado para suportar o ponto no tempo restaurar (PITR) e de longo prazo retenção imediatamente disponíveis capacidades da base de dados do SQL Server. Este tipo de armazenamento é alocado em separado para cada base de dados e cobrado como dois encargos de separado por base de dados. 

- **PITR**: base de dados individuais de cópias de segurança são copiadas para o armazenamento RA-GRS são automaticamente. O tamanho de armazenamento dinamicamente aumenta à medida que novas cópias de segurança são criadas.  O armazenamento é utilizado por cópias de segurança completas semanais, cópias de segurança diferenciais diárias e cópias de segurança de registo de transações copiadas a cada 5 minutos. O consumo de armazenamento depende a taxa de alteração da base de dados e o período de retenção. Pode configurar um período de retenção separado para cada base de dados entre 7 e 35 dias. É fornecida uma quantidade de armazenamento mínima igual a 1 x do tamanho dos dados, sem encargos adicionais. Para a maioria das bases de dados, esta quantidade é suficiente para armazenar 7 dias de cópias de segurança.
- **Imediatamente disponíveis**: base de dados do SQL Server oferece a opção de configurar a retenção de longa duração das cópias de segurança completas para até 10 anos. Se estiver ativada a política de imediatamente disponíveis, as cópias de segurança presumem são armazenadas no armazenamento RA-GRS automaticamente, mas pode controlar a frequência de cópias de segurança são copiadas. Para cumprir o requisito de conformidade diferente, pode selecionar períodos de retenção diferentes para cópias de segurança semanais, mensais e/ou anuais. Esta configuração irá definir a quantidade de armazenamento será utilizado para as cópias de segurança imediatamente disponíveis. Pode utilizar a Calculadora de preços imediatamente disponíveis para estimar os custos de armazenamento imediatamente disponíveis. Para obter mais informações, veja [Retenção de longa duração](sql-database-long-term-retention.md).

## <a name="azure-hybrid-use-benefit"></a>Benefício Híbrido do Azure

No vCore com base no modelo de compra (pré-visualização), pode trocar as suas licenças existentes para taxas com desconto na base de dados do SQL Server utilizando o [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Este benefício do Azure permite-lhe utilizar as licenças de SQL Server no local para poupar até 30% na SQL Database do Azure utilizando as licenças de SQL Server no local com o Software Assurance.

![preços](./media/sql-database-service-tiers/pricing.png)

## <a name="migration-of-single-databases-with-geo-replication-links"></a>Migração de bases de dados individuais com ligações de georreplicação

Migrar para o modelo baseado em DTU ao modelo baseado em vCore é semelhante a atualizar ou desatualização as relações de replicação geográfica entre bases de dados Standard e Premium. Não requer a terminar georreplicação, mas o utilizador tem de observar as regras de sequenciação. Ao atualizar, tem primeiro de atualizar a base de dados secundária e, em seguida, atualizar o site primário. Quando desatualização, inverter a ordem: tem de mudar primeiro a base de dados primária e, em seguida, mudar secundário. 

Quando utilizar a georreplicação entre dois conjuntos elásticos, recomenda-se que designar um conjunto como principal e o outro – como secundário. Nesse caso, migrar conjuntos elásticos devem utilizar a mesma documentação de orientação.  No entanto, é tecnicamente é possível que um conjunto elástico contém bases de dados primários e secundários. Neste caso, migrar corretamente, deve o conjunto com a maior utilização como "primário" e siga as regras de sequenciação em conformidade.  

A tabela seguinte fornece orientações para cenários de migração específica: 

|Camada de serviço atual|Camada de serviço de destino|Tipo de migração|Ações de utilizador|
|---|---|---|---|
|Standard|Fins gerais|Lateral|Pode migrar por qualquer ordem, mas certifique-se de um dimensionamento vCore adequado *|
|Premium|Crítico para a Empresa|Lateral|Pode migrar por qualquer ordem, mas tem de garantir vCore adequado dimensionamento *|
|Standard|Crítico para a Empresa|Atualizar|Deve migrar primeiro secundário|
|Crítico para a Empresa|Standard|Mudar para uma versão anterior|Deve migrar primeiro primário|
|Premium|Fins gerais|Mudar para uma versão anterior|Deve migrar primeiro primário|
|Fins gerais|Premium|Atualizar|Deve migrar primeiro secundário|
|Crítico para a Empresa|Fins gerais|Mudar para uma versão anterior|Deve migrar primeiro primário|
|Fins gerais|Crítico para a Empresa|Atualizar|Deve migrar primeiro secundário|
||||

\* Cada 100 DTU no escalão Standard requer, pelo menos, 1 vCore e cada DTU 125 no escalão Premium requer, pelo menos, 1 vCore

## <a name="migration-of-failover-groups"></a>Migração de grupos de ativação pós-falha 

Migração de grupos de ativação pós-falha com várias bases de dados necessita de migração individuais das bases de dados primários e secundários. Durante esse processo, as regras de sequenciação e as mesmas considerações aplicam-se. Depois das bases de dados são convertidos para o modelo baseado em vCore, o grupo de ativação pós-falha permanecem em vigor com as mesmas definições de política. 

## <a name="creation-of-a-geo-replication-secondary"></a>Criação de uma secundária georreplicação

Só pode criar uma com a mesma camada de serviço como principal de georreplicação secundária. Para a base de dados com a taxa de geração de registo elevada, recomenda-se que secundário é criado com o mesmo nível de desempenho como principal. Se estiver a criar uma secundária georreplicação no agrupamento elástico de uma única base de dados principal, recomenda-se o conjunto tem o `maxVCore` definição que corresponde ao nível de desempenho de base de dados primária. Se estiver a criar uma secundária georreplicação no agrupamento elástico para um site primário noutro conjunto elástico, é recomendado que os conjuntos de ter o mesmo `maxVCore` definições

## <a name="using-database-copy-to-convert-a-dtu-based-database-to-a-vcore-based-database"></a>Para converter uma base de dados baseada em DTU para uma base de dados baseada em vCore, utilizando a cópia de base de dados.

Pode copiar uma base de dados com um nível de desempenho com base em DTU para uma base de dados com um desempenho baseado em vCore nível sem restrições ou especial de sequenciação, desde que o nível de desempenho de destino suporta o tamanho máximo da base de dados da base de dados de origem. Isto acontece porque a cópia da base de dados cria um instantâneo dos dados a partir da hora iniciar a operação de cópia e não efetuar a sincronização de dados entre a origem e de destino. 

## <a name="next-steps"></a>Passos Seguintes

- Para obter detalhes sobre as opções de tamanho de armazenamento disponíveis para a base de dados individual e níveis de desempenho específicos, consulte [limites de recursos baseados em vCore de base de dados SQL das bases de dados](sql-database-vcore-resource-limits-single-databases.md#single-database-storage-sizes-and-performance-levels)
- Para obter detalhes sobre o armazenamento e níveis de desempenho específicos Consulte opções de tamanho disponíveis para conjuntos elásticos [limites de recursos baseados em vCore de base de dados SQL para conjuntos elásticos](sql-database-vcore-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-performance-levels).