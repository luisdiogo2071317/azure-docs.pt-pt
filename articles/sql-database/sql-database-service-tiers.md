---
title: Base de dados do SQL Azure Compra Modelos | Microsoft Docs
description: Saiba mais sobre a aquisição de modelo para a SQL Database do Azure.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: dda37600dffaf1e8e1c85c9757696f2d8219febb
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751675"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Base de dados do SQL do Azure a compra de recursos e modelos 

Servidores lógicas [SQL Database do Azure](sql-database-technical-overview.md) oferece dois modelos de compras para computação, armazenamento e recursos de e/s: um modelo de compra baseado em DTU e um modelo de compra baseado em vCore (pré-visualização). 

> [!NOTE]
> [Gerido instâncias](sql-database-managed-instance.md) na apenas para o Azure SQL Database oferecem o modelo de compra baseado em vCore.

A tabela e o gráfico seguinte comparam e contraste estes dois modelos de compras.

> [!IMPORTANT]
> Para vCore com base no modelo de compra (pré-visualização), consulte [vCore com base no modelo de compra](sql-database-service-tiers-vcore.md)

|**Modelo de compra**|**Descrição**|**Melhor opção para**|
|---|---|---|
|modelo baseado em DTU|Este modelo é baseado numa medida agrupada de computação, armazenamento e recursos de e/s. Os níveis de desempenho são expressos em Unidades de Transação de Base de Dados (DTUs) para bases de dados únicas e Unidades de Transação de Bases de Dados elásticas (eDTUs) para conjuntos elásticos. Para mais informações sobre as DTUs e eDTUs, consulte [quais são as DTUs e eDTUs](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?|Melhor para os clientes que pretendem simples, pré-configurados opções de recursos.| 
|modelo baseado em vCore|Este modelo permite-lhe independentemente Dimensionar recursos de computação e armazenamento. Também permite-lhe utilizar o benefício de híbrida do Azure para o SQL Server para obter uma redução de custos.|Melhor opção para os clientes que valor flexibilidade, controlo e transparência.|
||||  

![modelo de preços](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model--preview"></a>com base em vCore modelo de compra (pré-visualização)

Um núcleo virtual representa a CPU lógica oferecida com a opção de escolha entre gerações de hardware. O modelo de compra baseado em vCore (pré-visualização) permite a flexibilidade, o controlo, a transparência de consumo de recursos individuais e uma forma simples de traduzir requisitos no local carga de trabalho para a nuvem. Este modelo permite-lhe dimensionar a computação, memória e armazenamento com base nas suas necessidades de carga de trabalho. No vCore com base no modelo de compra (pré-visualização), os clientes podem optar entre fins gerais e de camadas de serviços críticos de negócio (pré-visualização) para ambos [único bases de dados](sql-database-single-database-scale.md) e [conjuntos elásticos](sql-database-elastic-pool.md). 

putador vCore com base no modelo de compra (pré-visualização) permite-lhe dimensionar recursos de armazenamento e computação independentemente, corresponde ao desempenho no local e otimizar o preço. Se a base de dados ou o conjunto elástico consome mais do que 300 DTU conversão para vCore pode reduzir o custo. Pode converter a utilizar a API escolhidas ou no portal do Azure, sem período de indisponibilidade. No entanto, a conversão não é necessária. Se o modelo de compra de DTU cumprir os seus requisitos de negócio e desempenho, deve continuar a utilizá-la. Se optar por converter do modelo de DTU vCore modelo, deve selecionar o nível de desempenho utilizando a seguinte regra prática: pelo menos 1 vCore na camada fins gerais; requer que cada 100 DTU no escalão Standard cada DTU 125 no escalão Premium requer, pelo menos, 1 vCore na camada de negócio crítico.

No vCore com base no modelo de compra (pré-visualização), os clientes pagar para:
- Computação (camada de serviço + número vCores + geração de hardware) *
- Tipo e a quantidade de armazenamento de dados e de registo 
- Número de IOs * *
- Cópia de segurança de armazenamento (RA-GRS) * * 

\* Em pré-visualização pública inicial, as CPUs lógico de 4 Gen baseiam Intel E5-2673 v3 (Haswell) 2.4 GHz processadores.

\*\* Durante a pré-visualização, 7 dias de cópias de segurança e o IOs são gratuitos.

> [!IMPORTANT]
> IOs, dados, de computação e armazenamento de registo são cobrados por base de dados ou o conjunto elástico. Armazenamento de cópias de segurança é cobrado por cada base de dados. Para obter detalhes de encargos de instância geridos, consulte [da base de dados geridas por instância de SQL do Azure](sql-database-managed-instance.md).
> **Limitações de região:** o modelo de compra baseado em vCore (pré-visualização) ainda não está disponível nas seguintes regiões: Europa Ocidental, França Central, sul do RU, RU oeste e Sudeste da Austrália.

## <a name="dtu-based-purchasing-model"></a>Modelo de compra baseado em DTU

A unidade de débito de base de dados (DTU) representa uma medida combinada de CPU, memória, lê e escreve. O modelo de compra baseado em DTU oferece um conjunto de pacotes pré-configuradas de recursos de computação e incluídos armazenamento em diferentes níveis de unidade do desempenho da aplicação. Os clientes que preferem simplicidade de um pacote pré-configurada e pagamentos fixos cada mês, pode localizar o modelo de DTU mais adequado para as suas necessidades. No modelo de compra baseado em DTU, os clientes podem optar entre **básico**, **padrão**, e **Premium** escalões de serviço para ambos [único bases de dados](sql-database-single-database-scale.md) e [conjuntos elásticos](sql-database-elastic-pool.md). 

### <a name="what-are-database-transaction-units-dtus"></a>Quais são as unidades de transação de base de dados (DTUs)?
Para uma única base de dados do SQL do Azure a um nível de desempenho específicos dentro de um [camada de serviço](sql-database-single-database-scale.md), Microsoft garante que um determinado nível de recursos para a base de dados (independentemente de qualquer outra base de dados na nuvem do Azure), fornecendo um nível de desempenho previsível. A quantidade de recursos é calculada como um número de unidades de transação de base de dados ou DTUs e é uma medida agrupada de computação, armazenamento e recursos de e/s. O rácio entre estes recursos foi originalmente determinado por uma [carga de trabalho OLTP benchmark](sql-database-benchmark-overview.md), concebida para ser típico de cargas de trabalho do mundo real OLTP. Quando a carga de trabalho excede a quantidade de qualquer um destes recursos, o débito é otimizadas - resultante de desempenho mais lento e de tempos limite. Os recursos utilizados pela sua carga de trabalho não afetam os recursos disponíveis para outras bases de dados do SQL Server na nuvem do Azure e os recursos utilizados por outras cargas de trabalho não afetar os recursos disponíveis para a base de dados do SQL Server.

![caixa delimitadora](./media/sql-database-what-is-a-dtu/bounding-box.png)

As DTUs são mais úteis para compreender a quantidade de recursos entre bases de dados do Azure SQL níveis de desempenho diferentes e escalões de serviço relativa. Por exemplo, duplicando as DTUs ao aumentar o nível de desempenho de uma base de dados equivale a duplicando o conjunto de recursos disponíveis para a base de dados. Por exemplo, uma base de dados Premium P11 com 1750 DTUs fornece 350 x mais potência de computação de DTUs do que uma base de dados básica com cinco DTUs.  

Para obter informações mais aprofundada sobre o consumo de recursos (DTU) da sua carga de trabalho, utilize [Azure SQL da base de dados Query Performance Insight](sql-database-query-performance.md) para:

- Identificar os principais de consultas por contagem de CPU/duração/execução que potencialmente pode ser otimizada para um melhor desempenho. Por exemplo, uma consulta exigente em termos de e/s pode beneficiar da utilização de [técnicas de otimização de memória](sql-database-in-memory.md) para tornar o melhor utilização de memória disponível num determinado e o desempenho do escalão de nível de serviço.
- Desagregar os detalhes de uma consulta, ver o texto e o histórico de utilização de recursos.
- Desempenho de acesso de otimização de recomendações que mostram as ações executadas pelo [SQL Database Advisor](sql-database-advisor.md).

Pode alterar [escalões de serviço DTU](sql-database-service-tiers-dtu.md) em qualquer altura, sem período de indisponibilidade mínimo para a sua aplicação (em média, menos em quatro segundos). Para muitas empresas e aplicações, ser capaz de criar bases de dados e aumentar e reduzir o desempenho a pedido é suficiente, sobretudo se os padrões de utilização forem relativamente previsíveis. No entanto, se tiver padrões de utilização imprevisíveis, pode tornar-se difícil gerir os custos e o seu modelo de negócio. Para este cenário, utilize um conjunto elástico com um determinado número de eDTUs que são partilhadas entre várias bases de dados no conjunto.

![Introdução à Base de Dados SQL: DTUs de base de dados individual por camada e nível](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="what-are-elastic-database-transaction-units-edtus"></a>Quais são as unidades de transação de base de dados elásticas (eDTUs)?
Em vez do que fornecem um conjunto de recursos (DTUs) que pode não ser sempre necessária para uma base de dados do SQL Server que está sempre disponível dedicado, pode colocar as bases de dados para um [conjunto elástico](sql-database-elastic-pool.md) num servidor de base de dados SQL que partilha um conjunto de recursos entre essas bases de dados. Os recursos partilhados num conjunto elástico são avaliados por elástico unidades de transação de base de dados ou eDTUs. Conjuntos elásticos fornecem uma solução económica simple para gerir os objetivos de desempenho para ter vários amplamente várias bases de dados e padrões de utilização imprevisíveis. Um conjunto elástico garantias de recursos não podem ser consumidos por uma base de dados no agrupamento, enquanto garantir que cada base de dados no conjunto sempre tem uma quantidade mínima de recursos necessários disponíveis. 

![Introdução à Base de Dados SQL: eDTUs por camada e nível](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Um conjunto é atribuído um número definido de eDTUs para um preço do conjunto. Dentro do conjunto elástico, é dada às bases de dados individuais a flexibilidade para se dimensionarem automaticamente dentro dos limites configurados. Uma base de dados com mais pesada carga irá consumir mais eDTUs para satisfazer o pedido. Bases de dados mais ligeira cargas irão consumir menos eDTUs. Bases de dados com nenhuma carga irão consumir não eDTUs. Por recursos para o conjunto completo de aprovisionamento, vez por base de dados, tarefas de gestão são simplificadas, fornecendo um orçamento previsível para o conjunto.

Podem ser adicionais mais eDTUs a um conjunto existente sem qualquer período de indisponibilidade da base de dados e sem impacto nas bases de dados do conjunto. Do mesmo modo, se as eDTUs adicionais já não forem necessárias, podem ser removidas de um conjunto existente em qualquer momento. Pode adicionar ou subtrair bases de dados para o conjunto ou limite a quantidade de edtus que uma base de dados pode utilizar com muita carga reservar eDTUs para outras bases de dados. Se uma base de dados é previsibilidade em-a utilização de recursos, pode movê-la fora do conjunto e configurá-lo como uma base de dados com uma quantidade previsível de recursos necessários.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Como posso determinar o número de DTUs necessárias para a minha carga de trabalho?
Se pretender migrar a carga de trabalho de uma máquina virtual do SQL Server ou no local existente para a Base de Dados SQL do Azure, pode utilizar a [Calculadora de DTUs](http://dtucalculator.azurewebsites.net/) para se aproximar do número de DTUs necessárias. Para uma carga de trabalho existente do SQL Database do Azure, pode utilizar [SQL da base de dados Query Performance Insight](sql-database-query-performance.md) para compreender o consumo de recursos de base de dados (DTUs) para obter conhecimentos aprofundados mais aprofundado para otimizar a carga de trabalho. Também pode utilizar o [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV para ver o consumo de recursos de última hora. Em alternativa, a vista de catálogo [resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) mostra o consumo de recursos para os últimos 14 dias, mas numa fidelidade inferior de médias de cinco minutos.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Como posso saber se poderia beneficiar de um conjunto elástico de recursos?
Os conjuntos são adequados para um grande número de bases de dados com padrões de utilização específicos. Para uma determinada base de dados, este padrão é caracterizado por uma média de utilização baixa com os picos de utilização relativamente pouco frequente. A Base de Dados SQL avalia automaticamente o histórico de utilização de recursos de bases de dados num servidor de Base de Dados SQL existente e recomenda a configuração de conjunto adequada no portal do Azure. Para obter mais informações, consulte [When should an elastic pool be used? (Quando deve ser utilizado um conjunto elástico?)](sql-database-elastic-pool.md)

### <a name="what-happens-when-i-hit-my-maximum-dtus"></a>O que acontece quando atinjo o meu DTUs máximas?
Níveis de desempenho são calibrated e regidos para fornecer os recursos necessários para executar a carga de trabalho de base de dados até o máximo permitido para o nível / desempenho da camada de serviço selecionado. Se a carga de trabalho é atingir um os limites de e/s/registo de e/s da CPU/dados, irá continuar a receber o nível máximo permitido de recursos, mas provavelmente também irá ocorrer latências maiores de consulta. Estes limites não resultam em erros, mas num abrandamento da carga de trabalho, a menos que o abrandamento se torne tão acentuado que as consultas comecem a exceder o tempo limite. Se contactar os máximo permitido em simultâneo sessões/pedidos de utilizador (threads de trabalho), irá ver erros explícitos. Consulte [dos limites de recursos de base de dados do Azure SQL]( sql-database-resource-limits.md#what-happens-when-database-resource-limits-are-reached) para obter informações sobre limites de recursos não relacionadas com a CPU, memória, dados e/s ou e/s de registo de transações.

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Correlacionar os resultados do benchmark para desempenho de base de dados do mundo real
É importante compreender de que todos os testes de desempenho estão representativo e facto apenas. As taxas de transacção conseguidas com a aplicação do benchmark não poderá ser os mesmos que os que pode ser conseguido com outras aplicações. O benchmark é composta por uma coleção de transação de diferentes tipos de executam em relação a um esquema que contém uma variedade de tipos de dados e tabelas. Enquanto o benchmark exercises as mesmas operações básicas que são comuns a todas as cargas de trabalho OLTP, não representa qualquer classe específica da aplicação ou a base de dados. O objetivo do benchmark consiste em fornecer um guia razoável para o desempenho relativo de uma base de dados que pode esperar quando aumentar ou reduzir verticalmente entre níveis de desempenho. Na realidade, as bases de dados têm tamanhos diferentes e a complexidade, encontrarem mixes diferentes das cargas de trabalho em responderá formas diferentes. Por exemplo, uma aplicação de e/s intensivas pode atingir os limiares de e/s mais cedo, ou uma aplicação intensivas em CPU pode atingir os limites de CPU mais cedo. Não há nenhuma garantia que sejam dimensionadas de qualquer base de dados específica da mesma forma como o benchmark em aumento de carga.

O benchmark e respetivos metodologia são descritos mais detalhadamente abaixo.

### <a name="benchmark-summary"></a>Resumo do benchmark
ASDB mede o desempenho de uma combinação de operações de base de dados básica que ocorrem com maior frequência em cargas de trabalho (OLTP) de processamento de transações online. Embora o benchmark concebido com em mente, o esquema de base de dados, a população de dados de informática na cloud e transações foram concebidas para ser amplamente representativa dos elementos básicos normalmente utilizados em cargas de trabalho OLTP.

### <a name="schema"></a>Esquema
O esquema foi concebido para ter suficiente variedade e complexidade para suportar uma vasta gama de operações. O benchmark é executada relativamente a uma base de dados composto seis tabelas. As tabelas enquadram-se em três categorias: tamanho fixo, dimensionamento e a crescer. Existem duas tabelas de tamanho fixo; três tabelas dimensionamento; e uma tabela crescente. Tabelas de tamanho fixo de ter um número constante de linhas. Tabelas de dimensionamento tem uma cardinalidade que é proporcional ao desempenho de base de dados, mas não irá alterar durante o benchmark. A tabela crescente é um tamanho adequado, como uma tabela de dimensionamento do carregamento inicial, mas, em seguida, as alterações de cardinalidade durante a execução do benchmark como linhas estão inseridas e eliminadas.

O esquema inclui uma mistura de tipos de dados, incluindo o número inteiro, numérico, caráter e data/hora. O esquema inclui as chaves primária e secundária, mas não quaisquer chaves externas - ou seja, existem não existem restrições de integridade referencial entre as tabelas.

Um programa de geração de dados gera os dados para a base de dados inicial. Dados numéricos e de número inteiro são gerados com várias estratégias. Em alguns casos, os valores são distribuídos aleatoriamente ao longo de um intervalo. Noutros casos, um conjunto de valores é aleatoriamente permuted para se certificar de que é mantida uma distribuição específico. Campos de texto são gerados a partir de uma lista de palavras para produzir os dados de procura realistas ponderada.

A base de dados é dimensionada com base no "fator de dimensionamento". O factor de dimensionamento (abreviado como SF) determina a cardinalidade do dimensionamento e tabelas a crescer. Conforme descrito abaixo na secção utilizadores e Pacing, o tamanho de base de dados, número de utilizadores e o máximo desempenho todas Dimensionar in proportion to entre si.

### <a name="transactions"></a>Transações
A carga de trabalho consiste em tipos de transação nove, conforme mostrado na tabela abaixo. Cada transação foi concebida para realçar um conjunto específico de características de sistema na base de dados motor e sistema de hardware, com alto contraste a partir de outras transações. Esta abordagem torna mais fácil avaliar o impacto de diferentes componentes para o desempenho global. Por exemplo, a transação "Leitura pesada" produz um número significativo de operações de leitura do disco.

| Tipo de Transação | Descrição |
| --- | --- |
| Ler Lite |SELECIONAR; dentro da memória; só de leitura |
| Média de leitura |SELECIONAR; sobretudo em memória; só de leitura |
| Pesado de leitura |SELECIONAR; não sobretudo em memória; só de leitura |
| Atualização Lite |ATUALIZAR; dentro da memória; leitura-escrita |
| Atualizar pesado |ATUALIZAR; não sobretudo em memória; leitura-escrita |
| INSERT Lite |INSERIR; dentro da memória; leitura-escrita |
| Inserir pesado |INSERIR; não sobretudo em memória; leitura-escrita |
| Eliminar |ELIMINAR; combinação de dentro da memória e não em memória; leitura-escrita |
| CPU pesado |SELECIONAR; dentro da memória; relativamente CPU sobrecarga; só de leitura |

### <a name="workload-mix"></a>Combinação de carga de trabalho
Transações são selecionadas aleatoriamente entre uma distribuição ponderada com a seguinte combinação geral. A mistura geral tem um rácio de leitura/escrita de aproximadamente 2:1.

| Tipo de Transação | % de combinação |
| --- | --- |
| Ler Lite |35 |
| Média de leitura |20 |
| Pesado de leitura |5 |
| Atualização Lite |20 |
| Atualizar pesado |3 |
| INSERT Lite |3 |
| Inserir pesado |2 |
| Eliminar |2 |
| CPU pesado |10 |

### <a name="users-and-pacing"></a>Os utilizadores e o ritmo de processamento
A carga de trabalho do benchmark é suscitada pelo departamento de uma ferramenta que submete transações através de um conjunto de ligações para simular o comportamento de um número de utilizadores em simultâneo. Embora todas as ligações e transações máquina gerada, de simplicidade denominamos estas ligações "utilizadores". Apesar de cada utilizador funciona independentemente todos os outros utilizadores, todos os utilizadores executam o mesmo ciclo de passos abaixo:

1. Estabelece uma ligação de base de dados.
2. Repita até assinalado para sair:
   * Selecione uma transação aleatória, (a partir de uma distribuição ponderada).
   * Executar a transação selecionada e medir o tempo de resposta.
   * Aguarde um atraso de ritmo de processamento.
3. Feche a ligação de base de dados.
4. Saída.

O ritmo de processamento atraso (no passo 2c) é aleatória, selecionado, mas com uma distribuição que tenha uma média de segundo 1.0. Deste modo, cada utilizador pode, em média, gerar no máximo uma transação por segundo.

### <a name="scaling-rules"></a>Regras de dimensionamento
O número de utilizadores é determinado pelo tamanho da base de dados (em unidades de fator de dimensionamento). Não há um utilizador a cada cinco unidades de fator de dimensionamento. Devido a atraso ritmo de processamento, um utilizador pode gerar um máximo de uma transação por segundo, em média.

Por exemplo, um-fator de dimensionamento de 500 (SF = 500) base de dados terá 100 utilizadores e pode conseguir uma velocidade máxima de 100 TPS. A unidade um TPS superior taxa requer uma base de dados maior e mais utilizadores.

A tabela abaixo mostra o número de utilizadores que efetivamente constante para cada nível de desempenho e o escalão de serviço.

| Camada de serviço (nível de desempenho) | Utilizadores | Tamanho da Base de Dados |
| --- | --- | --- |
| Básica |5 |720 MB |
| Padrão (S0) |10 |1 GB |
| Padrão (S1) |20 |2.1 GB |
| Padrão (S2) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

### <a name="measurement-duration"></a>Duração de medida
Um benchmark válido executar requer uma duração de medição do Estado de repouso de, pelo menos, uma hora.

### <a name="metrics"></a>Métricas
As métricas chave no benchmark são débito e a resposta de tempo.

* Débito é a medida de desempenho essenciais no benchmark. Débito é reportado no transações por unidade-de-time, todos os tipos de transação de contagem.
* Tempo de resposta é uma medida de previsibilidade quanto de desempenho. A restrição de tempo de resposta varia de acordo com a classe de serviço, com classes superiores de serviço ter um requisito de tempo de resposta mais rigorosos, conforme mostrado abaixo.

| Classe de serviço | Medida de débito | Requisito de tempo de resposta |
| --- | --- | --- |
| Premium |Transações por segundo |percentil 95th em segundos 0.5 |
| Standard |Transações por minuto |percentil 90th em segundos 1.0 |
| Básica |Transações por hora |percentil 80th em segundos 2.0 |

## <a name="next-steps"></a>Passos Seguintes

- Para vCore com base no modelo de compra (pré-visualização), consulte [vCore com base no modelo de compra](sql-database-service-tiers-vcore.md)
- Para o DTU com base no modelo de compra, consulte [DTU com base no modelo de compra](sql-database-service-tiers-dtu.md).
