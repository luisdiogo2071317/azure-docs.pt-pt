---
title: FAQ do banco de dados do SQL do Azure | Documentos da Microsoft
description: Respostas para os clientes de perguntas comuns pedir mais informações sobre a base de dados do Azure SQL, bases de dados na cloud e da base de dados e sistema de gestão da Microsoft da base de dados relacional (RDBMS) como um serviço na cloud.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: reference
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: carlrab
ms.openlocfilehash: 6c43a52309a6f41d73ead19fc5f5b7df2f06d6f6
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308818"
---
# <a name="sql-database-faq"></a>FAQ da Base de Dados SQL

## <a name="what-is-the-current-version-of-sql-database"></a>O que é a versão atual da base de dados SQL?
A versão atual da base de dados SQL é V12. Versão V11 foi preterido.

## <a name="what-is-the-sla-for-sql-database"></a>O que é o SLA para a base de dados SQL?
Garantimos, pelo menos, 99,99% do tempo, ter conectividade entre a base de dados de SQL do Microsoft Azure e o nosso gateway de Internet, independentemente do escalão de serviço. Para obter mais informações, consulte [SLA](http://azure.microsoft.com/support/legal/sla/).

## <a name="what-is-the-new-vcore-based-purchasing-model-for-azure-sql-database"></a>O que é o novo vCore com base no modelo de compra para a base de dados do Azure SQL?

O novo modelo de compra é um acréscimo ao modelo existente baseado na DTU. O modelo baseado em vCore desenvolvido para fornecer flexibilidade aos clientes, controlo, transparência e uma forma direta de traduzir locais requisitos de carga de trabalho para a cloud. Ela também permite aos clientes dimensionar os seus recursos de computação e armazenamento com base nas suas necessidades de carga de trabalho. Base de dados e opções do conjunto elástico com o modelo de vCore também são elegíveis para a economia de 30 por cento com o [Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md). Ver [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) e [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md) para obter mais informações. 

## <a name="what-is-a-vcore"></a>O que é um vCore? 
Um núcleo virtual representa a CPU lógica oferecida com a opção de escolher entre gerações de hardware. Geração 4 CPUs lógicas baseiam nos v3 Intel E5-2673 processadores de 2,4 GHz (Haswell) e de CPUs lógicas de geração 5 baseiam-se no Intel E5-2673 v4 (Broadwell) 2.3 GHz processadores.

## <a name="is-moving-to-the-vcore-based-model-required"></a>É preciso mudar para o modelo baseado em vCore?
Não, a introdução do modelo baseado em vCore para o conjunto elástico e opções de implementação de base de dados individual reflete o nosso compromisso para com a opção de cliente e a flexibilidade. Se quiser que os clientes continuar a utilizar o modelo baseado em DTU, não precisam de fazer nada com este anúncio e sua experiência e faturação irão permanecer inalteradas. 

Em muitos casos, aplicativos podem se beneficiar da simplicidade de um pacote pré-configurado de recursos. Por conseguinte, continuamos a oferecer e dar suporte essas opções de acesso baseado em DTU aos nossos clientes. Se estiver a utilizá-los e cumpre os requisitos de negócio, deve continuar a fazê-lo.

Os modelos baseados em DTU e vCore continuarão a existir lado a lado. Estamos a lançar o modelo baseado em vCore em resposta aos pedidos dos clientes de uma maior transparência em torno de seus recursos de base de dados e a capacidade de dimensionar os seus recursos de computação e armazenamento em separado. O modelo baseado em vCore também permite mais poupanças para os clientes com Software Assurance ativo através do benefício híbrido do Azure para o SQL Server.

## <a name="how-should-i-choose-between-the-dtu-based-purchasing-model-vs-the-vcore-based-purchasing-model"></a>Como devo escolher entre os vs modelo de compra baseado em DTU o modelo de compra baseado em vCore? 
A Unidade de Transação de Base de Dados (DTU) é baseada numa medida combinada de CPU, memória, leituras e escritas. Os níveis de desempenho baseados em DTU representam pacotes de recursos pré-configurados para impulsionar diferentes níveis de desempenho de aplicações. Os clientes que não pretende se preocupar sobre os recursos subjacentes e preferir a simplicidade de um pacote pré-configurado enquanto paga uma quantia fixa por mês podem encontrar o modelo baseado em DTU mais adequado às suas necessidades. No entanto, para os clientes que precisam de mais informações sobre os recursos subjacentes ou precisam de aumentar de forma independente para obter um desempenho ideal, o modelo baseado em vCore será a melhor opção.  Além disso, se um cliente tiver um Active Directory Software Assurance (SA) para o SQL Server, podem tirar partido dos seus investimentos existentes em e poupe até 30% com [Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Opções de dentro de cada um dos modelos de compras oferecem os benefícios de um serviço totalmente gerido, como cópias de segurança automáticas, atualizações de software e patches. 

## <a name="what-is-the-azure-hybrid-benefit-for-sql-server"></a>O que é o Benefício Híbrido do Azure para o SQL Server? 
O [Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) ajuda-o a maximizar o valor dos seus investimentos de licença atuais e acelerar a sua migração para a cloud. Benefício híbrido do Azure para o SQL Server é um benefício baseado no Azure que permite-lhe utilizar as suas licenças do SQL Server com Software Assurance para pagar uma taxa reduzida ("taxa base") na base de dados SQL. O benefício híbrido do Azure para o SQL Server está disponível em pré-visualização pública do modelo de compra baseado em vCore para conjuntos elásticos e bases de dados individuais de base de dados SQL. Pode aplicar este benefício mesmo se a SKU estiver ativa, mas tenha em atenção de que a taxa base é aplicada a partir do momento em que a selecionar no portal do Azure. Não será emitido crédito retroativamente.

## <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>Existem direitos de uso de dupla com o benefício híbrido do Azure para o SQL Server?
Terá de 180 dias de direitos de uso dual da licença para garantir que as migrações executem de forma totalmente integrada. Depois desse período de 180 dias, a licença do SQL Server só pode ser utilizada na nuvem na base de dados SQL e não tem uso dual direitos no local e na cloud.


## <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>Azure Hybrid Benefit para o SQL Server difere da mobilidade de licenças?
Hoje em dia, podemos oferecer benefícios da mobilidade de licenças para clientes do SQL Server com Software Assurance, que permite a reatribuição de suas licenças para servidores compartilhados de terceiros. Este benefício pode ser utilizado no IaaS do Azure e AWS EC2.
O benefício híbrido do Azure para o SQL Server é diferente da mobilidade de licenças em duas áreas principais:
- Ele fornece benefícios econômicos para mover cargas de trabalho altamente virtualizadas para o Azure. Os clientes de SQL EE podem obter 4 núcleos no Azure, o SKU de fins gerais para cada núcleo tiver no local para aplicativos altamente virtualizados. Mobilidade de licenças não permite quaisquer benefícios de custo especial para mover cargas de trabalho virtualizadas para a cloud.
- Ele fornece para um destino de PaaS do Azure (instância de gerida de base de dados do SQL), que é altamente compatível com o SQL Server no local

## <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>Quais são os direitos específicos do benefício híbrido do Azure, para o SQL Server?
Os clientes da base de dados SQL terão os seguintes direitos associados com o benefício híbrido do Azure para o SQL Server:

|Requisitos de espaço de licença|O que faz o benefício híbrido do Azure para SQL Server obter?|
|---|---|
|Clientes de núcleo do SQL Server Enterprise Edition com SA|<li>Pode pagar a taxa Base no SKU de críticos de negócio ou para fins gerais</li><br><li>1 núcleo no local = 4 núcleos no SKU de fins gerais</li><br><li>1 núcleo no local = 1 núcleo na SKU crítica de negócios</li>|
|Clientes de núcleo do SQL Server Standard Edition com SA|<li>Paguem apenas taxa Base na SKU de fins gerais</li><br><li>1 núcleo no local = 1 núcleo no SKU de fins gerais</li>|
|||

## <a name="is-the-vcore-based-model-available-to-sql-database-managed-instance"></a>O modelo baseado em vCore está disponível para a instância de gerida de base de dados do SQL?
[Instância gerida](sql-database-managed-instance.md) está disponível apenas com o modelo baseado em vCore. Para obter mais informações, consulte também os [base de dados SQL página de preços](https://azure.microsoft.com/pricing/details/sql-database/managed/). 

## <a name="does-the-cost-of-compute-and-storage-depend-on-the-service-tier-that-i-choose"></a>O custo de computação e armazenamento depende do escalão de serviço devo escolher?
O custo de computação reflete a capacidade de total de computação é aprovisionada para a aplicação. No escalão de serviço crítico para a empresa, podemos atribuir automaticamente, pelo menos, 3 réplicas de Always ON. Para refletir esta adicional a alocação de recursos de computação, o preço de vCore é aproximadamente de 2,7 x de crítico para a empresa num nível superior. Pela mesma razão, o maior preço do armazenamento por GB no escalão crítico para a empresa reflete a e/s elevado e baixa latência de armazenamento SSD. Ao mesmo tempo, o custo de armazenamento de cópia de segurança não é diferente porque em ambos os casos, podemos usar uma classe de armazenamento standard.

## <a name="how-am-i-charged-for-storage---based-on-what-i-configure-upfront-or-on-what-the-database-uses"></a>Como sou cobrado por armazenamento - com base na qual posso configurar inicialmente ou sobre o que utiliza a base de dados?
Diferentes tipos de armazenamento são cobrados de forma diferente. Para o armazenamento de dados, é-lhe cobrada o com base no tamanho máximo de base de dados ou um conjunto de que selecionar o armazenamento aprovisionado. O custo não altera a menos que reduzir ou aumentar esse máximo. O armazenamento de cópias de segurança é associado às cópias de segurança automáticas da sua instância. Se aumentar o período de retenção de cópias de segurança, está a aumentar o armazenamento de cópias de segurança que a sua instância consome. O armazenamento de cópias de segurança até 100 por cento do total do armazenamento de servidor aprovisionado não implica custos adicionais. O consumo de armazenamento de cópia de segurança adicional é cobrado em GB por mês. Por exemplo, se tiver um tamanho de armazenamento de base de dados de 100 GBs, obterá 100 GBs de cópia de segurança sem custos adicionais. Mas se a cópia de segurança tiver 110 GBs, pague os 10 GB adicionais.

Armazenamento de cópias de segurança de uma base de dados, é cobrado numa base rateada para o armazenamento que foi alocado para as cópias de segurança da base de dados menos o tamanho da base de dados. Armazenamento de cópias de segurança de um conjunto elástico, é cobrado numa base rateada para o armazenamento que foi alocado para as cópias de segurança da base de dados de todas as bases de dados no agrupamento de menos o tamanho máximo de dados do conjunto elástico. Qualquer aumento no tamanho da base de dados ou conjunto elástico ou aumento de velocidade de transação, necessita de mais armazenamento e, portanto, aumenta a sua fatura de armazenamento de cópia de segurança.  Quando aumenta o tamanho máximo de dados, este é o novo valor é deduzido do tamanho de armazenamento de cópias de segurança faturadas.

## <a name="how-do-i-select-the-right-sku-when-converting-an-existing-database-to-the-new-service-tiers"></a>Como posso selecionar o SKU direito ao converter de uma base de dados existente para novos escalões de serviço? 
Para aplicativos existentes da base de dados SQL com o modelo baseado em DTU, a camada de serviços de fins gerais é comparável com o escalão Standard. O escalão de serviço crítico para a empresa é comparável com o escalão Premium. Em ambos os casos, deve alocar, pelo menos, 1 vCore para cada 100 DTU que utiliza o seu aplicativo no modelo baseado em DTU.

## <a name="do-the-new-vcore-based-service-tiers-offer-the-performance-levels-compatible-with-all-existing-service-level-objectives-slos"></a>Novos escalões de serviço baseado em vCore oferece os níveis de desempenho, compatíveis com o existente todos os objetivos de nível serviço (SLO)?
Os novos escalões de serviço baseado em vcore da oferecem de opções de desempenho comparável para todos os dados elásticas e bases de dados com 100 DTUs ou mais.  Vamos continuar a adicionar mais SLOs ao longo do tempo para acomodar cargas de trabalho sub 100 DTUS.

## <a name="are-there-any-database-feature-differences-between-the-existing-dtu-based-and-new-vcore-based-service-tiers"></a>Há qualquer base de dados diferenças de funcionalidade entre os escalões de serviço baseado em DTU e novos baseado em vCore existente? 
Os novos escalões de serviço suportam um superconjunto de funcionalidades disponíveis com as ofertas de baseado em DTU atuais. As funcionalidades adicionais incluem um conjunto de vistas adicionais de gestão dinâmica (DMVs) e opções de configuração de recursos adicionais. 

## <a name="if-my-database-is-cpu-bound-and-does-not-use-much-storage-can-i-increase-the-compute-without-paying-for-extra-storage"></a>Se meu banco de dados é vinculada à CPU e não utiliza o espaço de armazenamento, posso aumentar a computação sem ter de adquirir armazenamento adicional?
Sim, pode selecionar independentemente do nível de computação a sua aplicação precisa e manter o armazenamento inalterado. O armazenamento pode ser definido como tão baixo como 32GB. 

## <a name="will-the-new-vcore-based-tiers-support-point-in-time-restore-pitr-for-35-days-as-today"></a>Os novos escalões de acesso baseado em vCore suportará ponto no tempo (PITR) durante 35 dias como hoje? 
Pode configurar a retenção de cópia de segurança para PITR entre 7 e 35 dias. O armazenamento de cópias de segurança é cobrado separadamente com base no consumo de armazenamento real se exceder a quantidade de armazenamento equivalente ao tamanho máximo de dados. Em pré-visualização, por predefinição o período de retenção PITR está definido para 7 dias. Em muitos casos, o tamanho máximo dos dados é suficiente para armazenar a 7 dias de cópias de segurança.

## <a name="why-do-you-allow-selection-of-the-hardware-generation-for-compute"></a>Por que permitir a seleção da geração de hardware para computação?
Nosso objetivo é permitir flexibilidade máxima para que pode escolher uma configuração de desempenho que se aproxima as necessidades do aplicativo. Hardware de Gen4 oferece substancialmente mais memória por vCore. No entanto, o Gen5 hardware permite-lhe aumentar verticalmente os recursos de computação muito superiores. Para obter mais informações, consulte [modelo de compra de vCore](sql-database-service-tiers-vcore.md)

## <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-database-to-a-vcore-based-service-tier"></a>É necessário colocar a minha aplicação offline para converter de uma base de dados baseado em DTU para um escalão de serviço baseado em vCore? 
Os novos escalões de serviço oferecem um método de conversão online simples, semelhante ao processo existente de atualização de bases de dados do escalão de serviço Standard para Premium e vice-versa. Esta conversão pode ser iniciado através do portal do Azure, PowerShell, CLI do Azure, T-SQL ou a API REST. Ver [gerir bases de dados individuais](sql-database-single-database-scale.md) e [gerir conjuntos elásticos](sql-database-elastic-pool.md).

## <a name="can-i-convert-a-database-from-a-vcore-based-service-tier-to-a-dtu-based-one"></a>Posso converter a uma base de dados de um escalão de serviço baseado em vCore para um baseado em DTU? 
Sim, pode converter facilmente sua base de dados para qualquer objetivo de desempenho suportados com o portal do Azure, PowerShell, CLI do Azure, T-SQL ou a API REST. Ver [gerir bases de dados individuais](sql-database-single-database-scale.md) e [gerir conjuntos elásticos](sql-database-elastic-pool.md).

## <a name="can-i-upgrade-or-downgrade-between-the-general-purpose-and-business-critical-service-tiers"></a>É possível atualizar ou mudar entre os escalões de serviço para fins gerais e crítico para a empresa? 
Sim, com algumas restrições. O SKU de destino tem de cumprir a base de dados máximo ou o tamanho do conjunto elástico que configurou para a sua implementação existente. Se estiver a utilizar [Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md), o SKU de críticos de negócios só está disponível para os clientes com licenças do Enterprise Edition. Apenas os clientes que migradas do local para fins gerais com o benefício híbrido do Azure para o SQL Server com licenças do Enterprise Edition podem atualizar para crítico para a empresa. Para obter detalhes, consulte [quais são os direitos específicos do Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md)?

Essa conversão resulta num período de indisponibilidade e pode ser iniciada com o portal do Azure, PowerShell, CLI do Azure, T-SQL ou a API REST. Ver [gerir bases de dados individuais](sql-database-single-database-scale.md) e [gerir conjuntos elásticos](sql-database-elastic-pool.md).

## <a name="i-am-using-a-premium-rs-database-that-will-not-be-generally-available---can-i-upgrade-it-to-a-new-tier-and-achieve-a-similar-priceperformance-benefit"></a>Estou usando um banco de dados de Premium RS não estará disponível em geral - posso atualizá-lo para um novo escalão e obter um benefício de relação preço/desempenho semelhante?
Como o modelo de vCore permite independente controlo sobre a quantidade de armazenamento e computação aprovisionados, pode com mais eficiência gerir os custos resultantes, tornando um destino apelativo para bases de dados de Premium RS. Além disso, o [Azure Hybrid Use Benefit para o SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md) fornece um desconto substancial quando é utilizado o modelo baseado em vCore. 

## <a name="how-often-can-i-adjust-the-resources-per-pool"></a>Com que frequência posso ajustar os recursos por conjunto?
Sempre que quiser. Ver [gerir conjuntos elásticos](sql-database-elastic-pool.md).

## <a name="how-long-does-it-take-to-change-the-service-tier-or-performance-level-of-a-single-database-or-move-a-database-in-and-out-of-an-elastic-pool"></a>Quanto tempo demora alterar o nível de desempenho ou de camada de serviço de uma base de dados ou mover uma base de dados dentro e fora de um conjunto elástico?
Alterar o escalão de serviço de uma base de dados e saem de um agrupamento requerem a base de dados ser copiados na plataforma como uma operação em segundo plano. A alteração da camada de serviço pode demorar entre alguns minutos ou diversas horas, dependendo do tamanho dos bancos de dados. Em ambos os casos, as bases de dados permanecem online e disponíveis durante a mudança. Para obter detalhes sobre como alterar as bases de dados individuais, consulte [alterar o escalão de serviço das bases de dados](sql-database-service-tiers-dtu.md). 

## <a name="when-should-i-use-a-single-database-vs-elastic-databases"></a>Quando devo utilizar uma base de dados individual versus bases de dados elásticas?
Em geral, os conjuntos elásticos foram concebidos para típica [padrão da aplicação software-como-serviço (SaaS)](sql-database-design-patterns-multi-tenancy-saas-applications.md), onde existe uma base de dados por inquilino ou cliente. Na maioria dos casos, comprar bases de dados individuais e aprovisionar em excesso para satisfazer os picos de procura para cada base de dados não é rentável. Com os conjuntos, gerir o desempenho coletivo do conjunto e as bases de dados aumente e diminua automaticamente. Motor de inteligente do Azure recomenda um agrupamento para bases de dados ao garante um padrão de utilização. Para obter detalhes, consulte [documentação de orientação do conjunto elástico](sql-database-elastic-pool.md).

## <a name="how-does-the-usage-of-sql-database-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Como é a utilização da base de dados SQL com o modelo de compra baseado em DTU aparece na minha fatura?
Listas de base de dados SQL a uma tarifa por hora previsível com base na [modelo de compra](sql-database-service-tiers-dtu.md). A utilização real é calculada e contabilizada pro rata à hora, para que a sua fatura pode apresentar frações de uma hora. Por exemplo, se uma base de dados existir durante 12 horas num mês, a fatura mostra a utilização de 0,5 dias. 

## <a name="what-if-a-single-database-is-active-for-less-than-an-hour-or-uses-a-higher-service-tier-for-less-than-an-hour"></a>E se uma base de dados esteve ativa menos de uma hora ou utiliza um escalão de serviço mais elevado por menos de uma hora?
É cobrada por cada hora, uma base de dados existe ao utilizar o escalão de serviço mais elevado + o nível de desempenho aplicável durante essa hora, independentemente da utilização ou se a base de dados esteve ativo durante menos de uma hora. Por exemplo, se criar uma base de dados e a eliminar passados cinco minutos, a fatura reflete um custo de hora de uma base de dados. 

Exemplos:

* Se criar uma base de dados básico e, em seguida, atualizá-lo imediatamente para Standard S1, são cobradas à tarifa Standard S1 para a primeira hora.
* Se passar uma base de dados de Basic para Premium às 10 meio-dia e a conclusão da atualização à 1H: 35 no dia seguinte, é cobrado à tarifa Premium, começando às 1:00 
* Se mudar a versão bases de dados de Premium para Basic às 11:00 e ele for concluída à tarde 2:15, em seguida, a base de dados é cobrado à tarifa Premium até 3 meio-dia, após o qual é cobrada às tarifas básicas.

## <a name="how-does-elastic-pool-usage-using-the-dtu-based-purchasing-model-show-up-on-my-bill"></a>Como é a utilização de conjunto elástico com o modelo de compra baseado em DTU aparece na minha fatura?
Conjunto elástico cobra show cópia de segurança na sua fatura como DTUs elásticas (eDTUs) ou vCores e armazenamento em incrementos mostrado na [a página de preços](https://azure.microsoft.com/pricing/details/sql-database/). Não existe nenhum custo por base de dados para conjuntos elásticos. É faturado por cada hora, que existe um conjunto no eDTU mais alto ou vCores, independentemente da utilização ou se o conjunto esteve ativo durante menos de uma hora. 

Baseado em DTU exemplos de modelo compra:

* Se criar um conjunto elástico Standard com 200 eDTUs ao 18 às 11:00h, a adição de cinco bancos de dados ao conjunto, é-lhe cobrada 200 eDTUs para a hora completa, começando na 11 horas por meio do resto do dia.
* No dia 2, às 5:05 da manhã, a base de dados 1 começa a consumir 50 eDTUs e mantém constante por meio do dia. Bases de dados 2 e 5 variar entre 0 e 80 eDTUs. Durante o dia, adicionar cinco outras bases de dados que consomem eDTUs variados ao longo do dia. Dia 2 é um dia inteiro cobrada de acordo com 200 Edtus. 
* No dia 3, em 17 horas Adicionar outro 15 bases de dados. Aumenta a utilização de base de dados ao longo do dia para o ponto em que decidir o aumento de eDTUs para o conjunto de 200 a 400 em 8:05 tarde Custos no nível de 200 eDTU foram em vigor até 8 pm e aumenta para 400 eDTUs para as restantes quatro horas. 

## <a name="how-are-elastic-pool-billed-for-the-dtu-based-purchasing-model"></a>Como é cobrado a baseado em DTU de conjunto elástico compra modelo?
Conjuntos elásticos são cobrados pelas seguintes características:

* Um conjunto elástico é cobrado durante a criação, mesmo quando não existem não existem bases de dados no conjunto.
* Um conjunto elástico é cobrado à hora. Esta é a mesma frequência de medição que nos níveis de desempenho de bases de dados individuais.
* Se um conjunto elástico é redimensionado, em seguida, o conjunto não é faturado, de acordo com a nova quantidade de recursos até que a operação de redimensionamento for concluída. Isso segue o mesmo padrão como alterar o nível de desempenho de bases de dados individuais.
* O preço de um conjunto elástico baseia-se nos recursos do conjunto. O preço de um conjunto elástico é independente do número e a utilização das bases de dados elásticas dentro da mesma.

Para obter detalhes, consulte [preços de base de dados SQL](https://azure.microsoft.com/pricing/details/sql-database/), [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md), e [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).

## <a name="how-does-the-vcore-based-usage-show-up-in-my-bill"></a>Como é a utilização baseado em vCore aparece na minha fatura? 
No modelo baseado em vCore, o serviço é faturado a uma tarifa por hora previsível com base no escalão de serviço, computação aprovisionada em vCores, armazenamento em GB/mês fornecido e consumidos o armazenamento de cópia de segurança. Se o armazenamento de cópias de segurança exceder o tamanho total da base de dados (ou seja, 100% do tamanho da base de dados), existem custos adicionais. horas de vCore, o armazenamento de base de dados configuradas, o e/s consumida e o armazenamento de cópia de segurança estão claramente discriminados na fatura, tornando mais fácil para que possa ver os detalhes de recursos que utilizou. Cópias de segurança armazenamento até 100% do tamanho máximo da base de dados está incluída, para além dos que são faturados em GB/mês consumido num mês.

Por exemplo:
- Se a base de dados SQL existir durante 12 horas num mês, a fatura mostra a utilização de 12 horas do vCore. Se a base de dados do SQL aprovisionado mais 100 GB de armazenamento, a fatura mostra a utilização de armazenamento em unidades de GB/mês contabilizadas à hora e o número de s consumido num mês.
- Se a base de dados SQL estiver ativo durante menos de uma hora, é-lhe cobrada para cada hora de existência da base de dados com o escalão de serviço mais elevado selecionado e o armazenamento aprovisionado, e/s aplicado durante essa hora, independentemente da utilização ou se a base de dados esteve ativo para menos do que uma hora.
- Se criar uma Instância Gerida e eliminá-la cinco minutos depois, será cobrada uma hora de base de dados.
- Se criar um servidor de Instância Gerida no escalão Fins Gerais com 8 vCores e, em seguida, atualizá-lo imediatamente para 16 vCores, é-lhe cobrada a taxa dos 16 vCore durante a primeira hora.

> [!NOTE]
> Por um período limitado, até 30 de Junho de 2018, os custos de cópia de segurança e os encargos de e/s são gratuitos.

## <a name="how-does-the-use-of-active-geo-replication-in-an-elastic-pool-show-up-on-my-bill"></a>Como funciona o uso de georreplicação ativa num conjunto elástico aparecem na minha fatura?
Ao contrário das bases de dados individuais, utilizando [georreplicação ativa](sql-database-geo-replication-overview.md) com bancos de dados elásticos, não tem um impacto direto de faturação.  É cobrado apenas para os recursos aprovisionados para cada um dos agrupamentos (conjunto primário e secundário conjunto)

## <a name="how-does-the-use-of-the-auditing-feature-impact-my-bill"></a>De que forma é que a utilização da funcionalidade de auditoria se reflete na fatura?
A auditoria está incorporada no serviço base de dados SQL não adicionais de custos e está disponível em todos os escalões de serviço. No entanto, para armazenar os registos de auditoria, as utilizações de funcionalidade de auditoria de uma conta de armazenamento do Azure e taxas de tabelas e filas do armazenamento do Azure aplicam-se com base no tamanho do seu registo de auditoria.

## <a name="how-do-i-reset-the-password-for-the-server-admin"></a>Como repor a palavra-passe para o administrador do servidor?
Na [portal do Azure](https://portal.azure.com), clique em **SQL Servers**, selecione o servidor da lista e, em seguida, clique em **Repor palavra-passe**.

## <a name="how-do-i-manage-databases-and-logins"></a>Como posso gerir bases de dados e inícios de sessão?
Ver [gestão de bases de dados e inícios de sessão](sql-database-manage-logins.md).

## <a name="how-do-i-make-sure-only-authorized-ip-addresses-are-allowed-to-access-a-server"></a>Como me certifico de que apenas endereços IP autorizados têm permissão para aceder a um servidor?
Ver [como: configurar as definições da firewall na base de dados SQL](sql-database-configure-firewall-settings.md).

## <a name="what-is-an-expected-replication-lag-when-geo-replicating-a-database-between-two-regions-within-the-same-azure-geography"></a>O que é um atraso de replicação esperado quando georreplicação uma base de dados entre duas regiões dentro da mesma geografia do Azure?
Podemos suportam atualmente um RPO de cinco segundos e o atraso de replicação foi inferior a que, quando o geo-secundária está alojada no Azure recomendado região emparelhada e na mesma camada de serviço.

## <a name="what-is-an-expected-replication-lag-when-geo-secondary-is-created-in-the-same-region-as-the-primary-database"></a>O que é um atraso de replicação esperado quando geo-secundária é criado na mesma região que a base de dados primária?
Com base em dados empíricos, não há muita diferença entre regiões dentro e entre regiões desfasamento quando o Azure recomendado região emparelhada é utilizado. 

## <a name="if-there-is-a-network-failure-between-two-regions-how-does-the-retry-logic-work-when-geo-replication-is-set-up"></a>Se houver uma falha de rede entre duas regiões, como a lógica de repetição funciona quando é configurada a georreplicação?
Se houver uma desconexão, podemos Repetir cada 10 segundos para voltar a estabelecer ligações.

## <a name="what-can-i-do-to-guarantee-that-a-critical-change-on-the-primary-database-is-replicated"></a>O que posso fazer para garantir que uma alteração fundamental na base de dados primário é replicada?
A geo-secundária é uma réplica de async e não tentamos mantê-la numa sincronização completa com o principal. Mas fornecemos um método para forçar a sincronização para garantir que a replicação das alterações críticas (por exemplo, atualizações de palavra-passe). Sincronização forçada afeta o desempenho, porque ele bloqueia o thread de chamada até que todas as transações consolidadas são replicadas. Para obter detalhes, consulte [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx). 

## <a name="what-tools-are-available-to-monitor-the-replication-lag-between-the-primary-database-and-geo-secondary"></a>Quais ferramentas estão disponíveis para monitorizar o desfasamento de replicação entre a base de dados primária e geo-secundária?
Vamos expor o desfasamento de replicação em tempo real entre a base de dados primária e geo-secundária através de um DMV. Para obter detalhes, consulte [sys.dm_geo_replication_link_status](https://msdn.microsoft.com/library/mt575504.aspx).

## <a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Para mover uma base de dados para um servidor diferente na mesma subscrição
Na [portal do Azure](https://portal.azure.com), clique em **bases de dados SQL**, selecione uma base de dados a partir da lista e, em seguida, clique em **cópia**. Ver [copiar uma base de dados SQL do Azure](sql-database-copy.md) para obter mais detalhes.

## <a name="to-move-a-database-between-subscriptions"></a>Para mover uma base de dados entre subscrições
Na [portal do Azure](https://portal.azure.com), clique em **servidores SQL** e, em seguida, selecione o servidor que aloja a base de dados da lista. Clique em **mover**e, em seguida, escolha os recursos para mover e a subscrição de destino.

