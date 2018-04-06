---
title: Base de dados SQL do Azure FAQ | Microsoft Docs
description: Colocar as respostas aos clientes de perguntas comuns sobre o bases de dados de nuvem e SQL Database do Azure, sistema de gestão da Microsoft base de dados relacional (RDBMS) e da base de dados como um serviço na nuvem.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: article
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: f98337044bdad788d2a4c9eac0c67a2031810430
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="sql-database-faq"></a>FAQ da Base de Dados SQL

## <a name="what-is-the-current-version-of-sql-database"></a>O que é a versão atual da base de dados SQL?
A versão atual da base de dados do SQL Server é V12. Versão V11 foi extinguido.

## <a name="what-is-the-sla-for-sql-database"></a>O que é o SLA para a base de dados SQL?
Podemos garantir que, pelo menos, de 99,99% de tempo, terá de conectividade entre a base de dados de SQL do Microsoft Azure e o nosso gateway para a Internet, independentemente do escalão de serviço. Para obter mais informações, consulte [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="whatis-the-new-vcore-based-purchasing-model-for-azure-sql-database"></a>Whatis o novo vCore com base no modelo de compra para a SQL Database do Azure?

O novo modelo de compra é além o modelo existente com base em DTU. O modelo baseado em vCore foi concebido para dar a clientes flexibilidade, controlo, transparência e uma forma simples para traduzir os requisitos de carga de trabalho no local para a nuvem. Também permite que os clientes para dimensionar a computação e armazenamento com base nas suas necessidades de carga de trabalho. Base de dados individual e opções de agrupamento elástico com o modelo de vCore também são elegíveis para cópia de segurança para as poupanças de 30 por cento com o [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Consulte [escalões de serviço](sql-database-service-tiers.md) para obter detalhes sobre o modelo de compra baseado em DTU e o modelo de compra baseado em vCore.

## <a name="what-is-a-vcore"></a>O que é um vCore? 
Um núcleo virtual representa a CPU lógica oferecida com a opção de escolha entre gerações de hardware. Gen 4 CPUs lógicas baseiam-se no Intel E5-2673 v3 (Haswell) processadores 2.4 GHz e Gen 5 lógica CPUs baseiam-se no Intel E5-2673 v4 (Broadwell) 2.3 GHz processadores.

## <a name="is-moving-to-the-vcore-based-model-required"></a>Está a mover para o modelo baseado em vCore necessário?
Não, a introdução de modelo baseado em vCore o conjunto elástico e opções de implementação de base de dados individual reflete o nosso compromisso com a opção de cliente e a flexibilidade. Se pretendem que os clientes continuar a utilizar o modelo de DTU, não precisa de efetuar qualquer ação com este anúncio e a sua experiência e faturação não serão alterados. 

Em muitos casos, as aplicações podem beneficiar do simplicidade de um bundle pré-configuradas de recursos. Por conseguinte, iremos continuar a oferta e suportar estas escolhas com base em DTU para os nossos clientes. Se estiver a utilizá-los e cumpre os requisitos de negócio, deve continuar a fazê-lo.

A DTU e modelos com base em vCore continuará a existir lado. Estamos a iniciar o modelo baseado em vCore em resposta aos pedidos de cliente para obter mais transparência em torno dos seus recursos de base de dados e a capacidade de dimensionar a computação e armazenamento em separado. O modelo baseado em vCore também poupança adicionais para clientes com o Active Directory Software Assurance através o benefício de híbrida do Azure para o SQL Server.

## <a name="how-should-i-choose-between-the-dtu-based-purchasing-model-vs-the-vcore-based-purchasing-model"></a>Como devo escolher entre os vs modelo de compra DTU com base no modelo de compra baseado em vCore? 
A unidade de transação de base de dados (DTU) é baseada numa medida combinada de CPU, memória, lê e escreve. Os níveis de desempenho com base em DTU representam pré-configurada pacotes de recursos para diferentes níveis de unidade do desempenho da aplicação. Clientes que não pretendem preocupar com os recursos subjacentes e preferir simplicidade de um bundle pré-configurada ao pagar uma quantidade fixa de cada mês podem localizar o modelo de DTU mais adequado para as suas necessidades. No entanto, para que os clientes que precisam de mais aprofundadas dos recursos subjacentes ou precisam de aumentá-los de forma independente para alcançar um desempenho ideal, o modelo baseado em vCore será a melhor opção.  Além disso, se um cliente tiver um Active Directory Software Assurance (SA) para o SQL Server, podem tirar partido do seu investimento existente e poupar até 30% com [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Opções de dentro de cada um dos modelos de compras fornecem as vantagens de um serviço completamente gerido, como as cópias de segurança automatizadas, atualizações de software e correções de erros. 

## <a name="what-is-the-azure-hybrid-benefit-for-sql-server"></a>O que é o benefício de híbrida do Azure para o SQL Server? 
O [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) ajuda a maximizar o valor do seu atuais investimentos de licenciamento e a acelerar a respetiva migração para a nuvem. Beneficiar híbrida do Azure para o SQL Server é uma vantagem baseado no Azure que permite-lhe utilizar as suas licenças do SQL Server com o Software Assurance aos quais deve prestar uma taxa reduzida ("taxa de base") na base de dados do SQL Server. Benefício de híbrida do Azure para o SQL Server está disponível em pré-visualização pública do modelo de compra baseado em vCore para conjuntos elásticos e bases de dados de únicas de base de dados SQL. Pode aplicar desta vantagem, mesmo que o SKU não está ativa mas tenha em atenção de que a taxa de base é aplicada a partir do momento que selecioná-lo no portal do Azure. Sem crédito será emitido retroactively.

## <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>Existem rights dual-utilização com vantagem de híbrida do Azure para o SQL Server?
Tem de 180 dias de direitos de utilização dupla da licença para garantir que as migrações em execução sem problemas. Depois desse período de 180 dias, a licença do SQL Server só pode ser utilizada na nuvem na base de dados do SQL Server e não tem a dupla utilizar direitos no local e na nuvem.


## <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>Como o benefício de híbrida do Azure para o SQL Server diferem de licença mobilidade?
Hoje em dia, iremos oferecer aos clientes de SQL Server com a licença do Software Assurance vantagens de mobilidade, permitindo reatribuição das respetivas licenças para servidores de terceiros partilhado. Pode ser utilizada desta vantagem em IaaS do Azure e AWS EC2.
Benefício de híbrida do Azure para o SQL Server é diferente da mobilidade de licenças em duas áreas principais:
- Fornece benefícios económico para mover as cargas de trabalho altamente virtualizadas para o Azure. Os clientes de SQL EE podem obter 4 núcleos no Azure, o SKU de objetivo geral para todos os núcleos possuem no local para aplicações altamente virtualizados. Mobilidade de licenças não permite quaisquer benefícios de custos especiais para mover as cargas de trabalho virtualizadas para a nuvem.
- Fornece para um destino de PaaS no Azure altamente compatível com SQL Server no local – a instância de gerido da base de dados do SQL Server.

## <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>Quais são os direitos específicos a vantagem de híbrida do Azure para o SQL Server?
Os clientes de base de dados do SQL Server terá os seguintes direitos associados a vantagem de híbrida do Azure para o SQL Server:

|Requisitos de espaço de licença|O que faz o benefício de híbrida do Azure para SQL Server obter?|
|---|---|
|Clientes de núcleo do SQL Server Enterprise Edition com SA|<li>Pode pagar a taxa de Base de finalidade geral ou SKU críticos de negócio</li><br><li>1 núcleo no local = 4 núcleos no SKU de objetivo geral</li><br><li>1 núcleo no local = 1 núcleo SKU críticos de negócio</li>|
|Clientes de núcleo do SQL Server Standard Edition com SA|<li>Pode pagar taxa Base no SKU de objetivo geral apenas</li><br><li>1 núcleo no local = 1 núcleo no SKU de objetivo geral</li>|
|||

## <a name="is-the-vcore-based-model-available-to-sql-database-managed-instance"></a>O modelo baseado em vCore está disponível para a instância de gerido da base de dados do SQL Server?
[Gerido instância](sql-database-managed-instance.md) está disponível apenas com o modelo baseado em vCore. Para obter mais informações, consulte também o [base de dados SQL página de preços](https://azure.microsoft.com/pricing/details/sql-database/managed/). 

## <a name="does-the-cost-of-compute-and-storage-depend-on-the-service-tier-that-i-choose"></a>O custo de armazenamento e computação dependem da camada de serviço que devo escolher?
O custo de computação reflete a capacidade de cálculo total que é aprovisionada para a aplicação. Na camada de serviços críticos de negócio, iremos atribuir automaticamente, pelo menos, 3 réplicas de Always ON. Para refletir esta adicional a alocação de recursos de computação, o preço vCore é aproximadamente 2.7 x de negócio críticos num nível superior. Para a mesma razão, o preço de armazenamento superior por GB na camada de negócio críticos reflete a e/s elevada e baixa latência de armazenamento SSD. Ao mesmo tempo, o custo de armazenamento de cópia de segurança não é diferente porque em ambos os casos utilizamos uma classe de armazenamento standard.

## <a name="how-am-i-charged-for-storage---based-on-what-i-configure-upfront-or-on-what-the-database-uses"></a>Como estou cobrado para armazenamento - com base no que posso configurar compromisso ou que utiliza a base de dados?
Diferentes tipos de armazenamento são faturados de forma diferente. Para armazenamento de dados, são-lhe ser cobrados para o armazenamento de aprovisionamento com base no máximo da base de dados ou selecionar o tamanho do conjunto. O custo não altera a menos que reduza ou aumentar esse máximo. O armazenamento de cópias de segurança é associado às cópias de segurança automáticas da sua instância. Se aumentar o período de retenção de cópias de segurança, está a aumentar o armazenamento de cópias de segurança que a sua instância consome. O armazenamento de cópias de segurança até 100 por cento do total do armazenamento de servidor aprovisionado não implica custos adicionais. Adicional consumo de armazenamento de cópia de segurança é cobrado GB por mês. Por exemplo, se tiver um tamanho de armazenamento de base de dados de 100 GBs, obterá 100 GBs de cópia de segurança sem custos adicionais. Mas se a cópia de segurança for 110 GBs, paga as 10 GBs adicionais.

Para armazenamento de cópia de segurança de uma base de dados, são-lhe cobrados numa base proporcional para o armazenamento que foi alocada para as cópias de segurança da base de dados menos o tamanho da base de dados. Para armazenamento de cópia de segurança de um conjunto elástico, são-lhe cobrados numa base proporcional para o armazenamento que foi alocada para as cópias de segurança da base de dados de todas as bases de dados no agrupamento de menos o tamanho máximo de dados do conjunto elástico. Qualquer aumento do tamanho de base de dados ou o conjunto elástico ou aumento da taxa de transacção, necessita de mais armazenamento e, por conseguinte, aumenta a fatura do armazenamento de cópia de segurança.  Quando aumenta o tamanho máximo de dados, este novo valor é deducted do tamanho de armazenamento de cópia de segurança cobrados.

## <a name="how-do-i-select-the-right-sku-when-converting-an-existing-database-to-the-new-service-tiers"></a>Como selecionar o SKU direito ao converter a base de dados existente para os novos escalões de serviço? 
Para aplicações de base de dados SQL existentes com o modelo de DTU, a camada de serviço de objetivo geral é comparável com o escalão Standard. A camada de serviços críticos de negócio é comparável com o escalão Premium. Em ambos os casos, deve alocar, pelo menos, 1 vCore para cada DTU 100 que utiliza a sua aplicação no modelo de DTU.

## <a name="do-the-new-vcore-based-service-tiers-offer-the-performance-levels-compatible-with-all-existing-service-level-objectives-slos"></a>Os novos escalões de serviço com base em vCore oferecem níveis de desempenho compatíveis com todas as existentes objetivos de nível serviço (SLO)?
Os novos escalões de serviço com base em vCore oferecem opções comparável em termos de desempenho para conjuntos elásticos todas as e bases de dados utilizando 100 DTUs ou mais.  Vamos continuar a adicionar os SLOs mais ao longo do tempo para acomodar sub 100 DTU as cargas de trabalho.

## <a name="are-there-any-database-feature-differences-between-the-existing-dtu-based-and-new-vcore-based-service-tiers"></a>Existem qualquer base de dados diferenças da funcionalidade entre as camadas de DTU e baseados no novo serviço baseado em vCore existente? 
Os novos escalões de serviço suportam um superconjunto das funcionalidades disponíveis com as ofertas de com base em DTU atuais. As funcionalidades adicionais incluem um conjunto de vistas de gestão dinâmica adicionais (DMVs) e opções de configuração de recursos adicionais. 

## <a name="if-my-database-is-cpu-bound-and-does-not-use-much-storage-can-i-increase-the-compute-without-paying-for-extra-storage"></a>Se a minha base de dados é vinculado à CPU e não utilizar o armazenamento, posso aumentar a capacidade de cálculo sem pagar armazenamento adicional?
Sim, pode selecionar independentemente o nível de computação tem da sua aplicação e manter o armazenamento inalterado. O armazenamento pode ser definido como tão baixo como 32GB. 

## <a name="will-the-new-vcore-based-tiers-support-point-in-time-restore-pitr-for-35-days-as-today"></a>As novas camadas de acesso baseado em vCore suportarão ponto no restauro de tempo (PITR) 35 dias como hoje? 
Pode configurar a retenção de cópias de segurança para PITR entre 7 e 35 dias. O armazenamento de cópias de segurança será cobrado em separado com base no consumo de armazenamento real se exceder a quantidade de armazenamento equivalente ao tamanho máximo de dados. Pré-visualização, por predefinição o período de retenção PITR está definido para 7 dias. Em muitos casos, o tamanho máximo de dados é suficiente para armazenar 7 dias de cópias de segurança.

## <a name="why-do-you-allow-selection-of-the-hardware-generation-for-compute"></a>Por que razão a permitir a seleção da geração de hardware da computação?
O nosso objetivo é permitir flexibilidade máxima para que possa escolher uma configuração de desempenho que coincida com as necessidades da aplicação. A tabela acima mostra as diferenças entre Gen4 e Gen5. Em particular, Gen4 hardware oferece substancialmente mais memória por vCore. No entanto, o Gen5 hardware permite-lhe aumentar verticalmente computação muito superior. Queremos estas diferenças transparente, para que pode alcançar o rácio de preço/desempenho ideal para a sua aplicação.

## <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>É necessário colocar offline a minha aplicação converter de uma base de dados baseada em DTU para uma camada de serviço com base em vCore? 
Os novos escalões de serviço disponibilizam um método simples de conversão online, que é semelhante ao processo existente de atualizar as bases de dados do padrão para o escalão de serviço Premium e vice-versa. Esta conversão pode ser iniciado através do Portal, ARM, do PowerShell, da CLI do Azure ou T-SQL. Consulte [gerir bases de dados único](sql-database-single-database-resources.md) e [gerir conjuntos elásticos](sql-database-elastic-pool.md).

## <a name="can-i-convert-a-database-from-a-vcore-based-service-tier-to-a-dtu-based-one"></a>Posso converter a uma base de dados de uma camada de serviço com base em vCore para um com base em DTU? 
Sim, pode converter facilmente a sua base de dados para qualquer objetivo de desempenho suportados através do Portal ou através de programação utilizando o Portal, ARM, do PowerShell, da CLI do Azure ou T-SQL. Consulte [gerir bases de dados único](sql-database-single-database-resources.md) e [gerir conjuntos elásticos](sql-database-elastic-pool.md).

## <a name="can-i-upgrade-or-downgrade-between-the-general-purpose-and-business-critical-service-tiers"></a>Pode atualizar ou mudar entre os escalões de serviço fins gerais e crítico de negócio? 
Sim, com algumas restrições. O SKU de destino têm de cumprir a base de dados máximo ou o tamanho do conjunto elástico configurado para a implementação existente. Se estiver a utilizar [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md), o SKU críticos de negócio apenas está disponível para clientes com licenças Enterprise Edition. Apenas os clientes que migrado no local para fins gerais que utilizar o benefício de híbrida do Azure para o SQL Server com licenças Enterprise Edition, podem atualizar para crítico de negócio. Para mais informações, consulte [quais são os direitos específicos a vantagem de utilizar híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md)?

Esta conversão não resulta num período de indisponibilidade e pode ser iniciado através do Portal, ARM, do PowerShell, da CLI do Azure ou T-SQL. Consulte [gerir bases de dados único](sql-database-single-database-resources.md) e [gerir conjuntos elásticos](sql-database-elastic-pool.md).

## <a name="i-am-using-a-premium-rs-database-that-will-not-be-generally-available---can-i-upgrade-it-to-a-new-tier-and-achieve-a-similar-priceperformance-benefit"></a>Estou a utilizar uma base de dados Premium RS não estará disponível em geral, - posso atualizá-lo para uma nova camada e alcançar uma vantagem de preço/desempenho semelhante?
Porque o modelo de vCore permite independente controlo sobre a quantidade de armazenamento e computação aprovisionada, é mais gerir de forma eficaz os custos resultantes, tornando um destino apelativo para bases de dados Premium RS. Além disso, o [benefício de utilização de híbrida do Azure para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) oferece um desconto significativo quando o modelo baseado em vCore é utilizado. 

## <a name="how-often-can-i-adjust-the-resources-per-pool"></a>Como muitas vezes, pode ajustar os recursos por agrupamento?
As vezes que pretender. Consulte [gerir conjuntos elásticos](sql-database-elastic-pool.md).

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Quanto tempo demora para alterar o nível de desempenho ou camada de serviço de uma base de dados ou mover uma base de dados dentro ou fora do conjunto elástico?
Alterar o escalão de serviço de uma base de dados e mover dentro e fora de um agrupamento requerem a base de dados para ser copiado na plataforma como operação em segundo plano. A alteração da camada de serviço pode demorar de alguns minutos ou várias horas, consoante o tamanho das bases de dados. Em ambos os casos, as bases de dados permanecerem online e disponíveis durante a mudança. Para obter detalhes sobre a alteração de bases de dados individuais, consulte [alterar o escalão de serviço de uma base de dados](sql-database-service-tiers.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Quando devo utilizar uma base de dados vs bases de dados elásticas?
Em geral, os conjuntos elásticos foram concebidos para típica [padrão de aplicação do software-como-um-serviço (SaaS)](sql-database-design-patterns-multi-tenancy-saas-applications.md), onde existe uma base de dados por inquilino ou cliente. Na maioria dos casos, comprar bases de dados individuais e aprovisionar em excesso para satisfazer os picos de procura para cada base de dados não é rentável. Com agrupamentos, gerir o desempenho coletivo do conjunto e as bases de dados e reduzir verticalmente Dimensionar automaticamente. Motor inteligente do Azure recomenda um conjunto de bases de dados quando um padrão de utilização warrants-lo. Para obter mais informações, consulte [orientações do conjunto elástico](sql-database-elastic-pool.md).

## <a name="how-does-the-usage-of-sql-database-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Como a utilização da base de dados do SQL Server utilizando o modelo de compra baseado em DTU apareçam na minha fatura?
Faturas de base de dados SQL numa taxa por hora previsível, com base no [compra modelo](sql-database-service-tiers.md). Utilização real é calculada e calculada hora a hora, pelo que a fatura poderá mostrar frações de uma hora. Por exemplo, se existir uma base de dados para 12 horas num mês, a fatura mostra a utilização de 0.5 dias. 

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>E se uma base de dados está ativo para menos de uma hora ou utiliza uma camada de serviço mais elevada para menos de uma hora?
É-lhe faturado para cada hora, existe uma base de dados utilizando a camada de serviço mais elevada + o nível de desempenho que aplicadas durante essa hora, independentemente da utilização ou a base de dados estava ativo para menos de uma hora. Por exemplo, se criar uma base de dados e eliminá-lo mais tarde cinco minutos a fatura reflete cobrada uma taxa por hora de uma base de dados. 

Exemplos:

* Se criar uma base de dados básica e, em seguida, atualizá-lo imediatamente Standard S1, são cobrado à taxa Standard S1 para a primeira hora.
* Se a atualizar uma base de dados do básica para o Premium às 10:00 e a conclusão da atualização à 1:35:00. no dia seguinte, são-lhe cobrados a taxa de Premium começando em 1:00:00. 
* Se mudar uma base de dados de Premium básico às 11:00 e terminar em 2: 00h 15, em seguida, a base de dados é cobrado à taxa Premium até 3: 00h, após o qual é cobrada às taxas de básicas.

## <a name="how-does-elastic-pool-usage-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Como utilização do conjunto elástico com o modelo de compra baseado em DTU apareçam na minha fatura?
Agrupamento elástico cobra Mostrar cópias de segurança na sua factura como DTUs elásticas (eDTUs) ou vCores plus armazenamento incrementos apresentada na [a página de preços](https://azure.microsoft.com/pricing/details/sql-database/). Não há sem qualquer encargo por base de dados para conjuntos elásticos. É-lhe faturado para cada hora, que existe um conjunto no eDTU máxima ou vCores, independentemente da utilização ou o conjunto estava ativo para menos de uma hora. 

Com base em DTU exemplos de modelo compra:

* Se criar um conjunto elástico Standard com 200 eDTUs às 11:18 a.m., bases de dados de cinco a adicionar ao agrupamento, são-lhe cobrados para 200 eDTUs para a hora de toda, começando na 11 a.m. através do resto do dia.
* No dia 2, em 5:05 a.m., base de dados 1 começa a consumir 50 eDTUs e de que detém gradual através do dia. Bases de dados 2 a 5 variam entre 0 e eDTUs de 80. Durante o dia, adicione cinco outras bases de dados que consomem eDTUs variando ao longo do dia. Dia 2 é um dia completo cobrado às 200 eDTU. 
* No dia 3, no 5 a.m. Adicione outro 15 bases de dados. Utilização de base de dados aumenta ao longo do dia para o ponto em que optar por aumentar os eDTUs de conjunto de 200 400 às 20:05 Os encargos ao nível da 200 eDTU foram em vigor até 8 pm e aumenta a 400 eDTUs para as restantes quatro horas. 

## <a name="how-are-elastic-pool-billed-for-the-dtu-based-purchasing-model"></a>Como é cobrado com base DTU de conjunto elástico compra do modelo?
Conjuntos elásticos são faturados pelas seguintes características:

* Um conjunto elástico é faturado após a respetiva criação, mesmo quando existem bases de dados no conjunto.
* Um conjunto elástico é faturado de hora a hora. Esta é a mesma frequência de medição para níveis de desempenho de bases de dados individuais.
* Se um conjunto elástico é redimensionado, em seguida, o conjunto não é faturado, de acordo com a quantidade de recursos novo até concluir a operação de redimensionamento. Este acompanha o mesmo padrão como alterar o nível de desempenho de bases de dados individuais.
* O preço do conjunto elástico baseia-se nos recursos do conjunto. O preço do conjunto elástico é independente do número e a utilização das bases de dados elásticas dentro da mesma.

Para obter mais informações, consulte [preços da SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) e [escalões de serviço](sql-database-service-tiers.md).

## <a name="how-does-the-vcore-based-usage-show-up-in-my-bill"></a>Como a utilização de vCore apresentada na minha fatura? 
No modelo baseado em vCore, a taxa diária previsível com base na camada de serviço, o serviço é faturado computação aprovisionada no vCores, aprovisionado armazenamento em GB/mês e consumidas armazenamento de cópia de segurança. Se o armazenamento de cópias de segurança exceder o tamanho de base de dados total (ou seja, 100% do tamanho da base de dados), existem encargos adicionais. horas de vCore, armazenamento de base de dados configuradas, foram consumidos e/s e armazenamento de cópia de segurança estão claramente descritos fatura, tornando mais fácil para ver os detalhes de recursos que utilizou. Cópia de segurança armazenamento até 100% do tamanho máximo da base de dados está incluído, para além de que é-lhe faturado em GB/mês consumido num mês.

Por exemplo:
- Se a base de dados do SQL Server para 12 horas num mês, a fatura mostra utilização para 12 horas de vCore. Se a base de dados do SQL Server aprovisionado 100 GB de armazenamento adicional, a fatura mostra a utilização de armazenamento em unidades de GB/mês proporcional hora a hora e número de IOs consumidos num mês.
- Se a base de dados do SQL Server está ativa para menos de uma hora, é-lhe faturado para cada hora na base de dados existe utilizando a camada de serviço mais elevada selecionada, aprovisionados de armazenamento e de e/s que aplicadas durante essa hora, independentemente da utilização ou se foi Active Directory para a menos que a base de dados uma hora.
- Se criar uma instância geridos e eliminá-lo mais tarde cinco minutos, será cobrada por hora de uma base de dados.
- Se criar um servidor de Instância Gerida no escalão Fins Gerais com 8 vCores e, em seguida, atualizá-lo imediatamente para 16 vCores, é-lhe cobrada a taxa dos 16 vCore durante a primeira hora.

> [!NOTE]
> Durante um período limitado através de de 2018 30th de Junho, os custos de cópia de segurança e encargos de e/s são gratuitos.

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Como funciona a utilização de georreplicação ativa num conjunto elástico são apresentados na minha fatura?
Ao contrário das bases de dados individuais, utilizando [georreplicação ativa](sql-database-geo-replication-overview.md) com bases de dados elásticas não tem um impacto direto de faturação.  Apenas são cobrados os recursos aprovisionados para cada um dos agrupamentos (conjunto principal e secundário agrupamento)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>De que forma é que a utilização da funcionalidade de auditoria se reflete na fatura?
Auditoria baseia-se para o serviço de base de dados SQL em nenhum extra de custos e está disponível em todos os escalões de serviço. No entanto, para armazenar os registos de auditoria, as utilizações de funcionalidade de auditoria uma conta de armazenamento do Azure e as taxas de tabelas e filas no armazenamento do Azure aplicam-se com base no tamanho do seu registo de auditoria.

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Como a reposição de palavra-passe do administrador do servidor?
No [portal do Azure](https://portal.azure.com), clique em **SQL Servers**, selecione o servidor da lista e, em seguida, clique em **Repor palavra-passe**.

## <a name="how-do-i-manage-databases-and-logins"></a>Como gerir bases de dados e inícios de sessão?
Consulte [gerir bases de dados e inícios de sessão](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Como torno se apenas endereços IP autorizados estão autorizados a aceder a um servidor?
Consulte [como: configurar as definições da firewall da SQL Database](sql-database-configure-firewall-settings.md).

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>O que é um atraso de replicação esperado ao replicar de georreplicação uma base de dados entre duas regiões dentro da mesma geografia do Azure?
Estamos atualmente são suportar um RPO de cinco segundos e o desfasamento foi inferior ao que quando o secundário georreplicação está alojado no Azure recomendado emparelhado região e na mesma camada de serviço.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>O que é um atraso de replicação esperado quando georreplicação secundário é criado na mesma região que a base de dados primária?
Com base nos dados empirical, não há demasiado diferença entre intra região e o desfasamento entre região quando o Azure recomendado região emparelhado é utilizado. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Se existir uma falha de rede entre duas regiões, como a lógica de repetição funciona quando configurar a georreplicação?
Se houver um desligar, iremos repetir a cada 10 segundos para voltar a estabelecer ligações.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>O que posso fazer para garantir que é replicada uma alteração fundamental na base de dados primária?
Georreplicação-secundário é uma réplica async e estamos a tentar guardá-lo em sincronização completa com o site primário. Mas fornecemos um método para forçar a sincronização para garantir que a replicação das alterações crítico (por exemplo, atualizações de palavra-passe). Sincronização forçada tem impacto no desempenho porque bloqueia o thread que efectua até que todas as transações consolidadas são replicadas. Para obter mais informações, consulte [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>As ferramentas estão disponíveis para monitorizar o desfasamento entre a base de dados primária e secundária georreplicação?
Expomos o desfasamento em tempo real entre a base de dados primária e a georreplicação-secundárias através de um DMV. Para obter mais informações, consulte [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Para mover uma base de dados para um servidor diferente na mesma subscrição
No [portal do Azure](https://portal.azure.com), clique em **bases de dados SQL**, selecione uma base de dados da lista e, em seguida, clique em **cópia**. Consulte [copiar uma base de dados SQL do Azure](sql-database-copy.md) para obter mais detalhes.

## <a name="to-move-a-database-between-subscriptions"></a>Para mover uma base de dados entre subscrições
No [portal do Azure](https://portal.azure.com), clique em **servidores SQL** e, em seguida, selecione o servidor que aloja a base de dados da lista. Clique em **mover**e, em seguida, escolha os recursos para mover e mover para a subscrição.
