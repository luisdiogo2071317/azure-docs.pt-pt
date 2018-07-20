---
title: Base de dados do SQL do Azure adquirir modelos | Documentos da Microsoft
description: Saiba mais sobre a compra de modelo para a base de dados do Azure SQL.
services: sql-database
author: CarlRabeler
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 07/19/2018
manager: craigg
ms.author: carlrab
ms.openlocfilehash: 33d23de2cd0ddae95a34c2c9f7acabdc7315cd36
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/19/2018
ms.locfileid: "39160047"
---
# <a name="azure-sql-database-purchasing-models-and-resources"></a>Base de dados do SQL do Azure compra de modelos e recursos 

Servidores lógicos no [base de dados do Azure SQL](sql-database-technical-overview.md) oferece dois modelos de compras para computação, armazenamento e recursos de e/s: um modelo de compra baseado em DTU e um modelo de compra baseado em vCore. 

> [!NOTE]
> [Instâncias geridas](sql-database-managed-instance.md) no Azure SQL Database só oferecem o modelo de compra baseado em vCore.

O gráfico e tabela seguintes comparam e contrastar esses dois modelos de compras.

> [!IMPORTANT]
> Para o modelo de compra baseado em vCore, consulte [o modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md)

|**Modelo de compra**|**Descrição**|**Melhor para**|
|---|---|---|
|modelo baseado em DTU|Este modelo baseia-se numa medida integrados em recursos de computação, armazenamento e e/s. Os níveis de desempenho são expressos em Unidades de Transação de Base de Dados (DTUs) para bases de dados únicas e Unidades de Transação de Bases de Dados elásticas (eDTUs) para conjuntos elásticos. Para obter mais informações sobre DTUs e eDTUs, veja [quais são DTUs e eDTUs](sql-database-service-tiers.md#what-are-database-transaction-units-dtus)?|Ideal para clientes que desejam simples, pré-configurado opções de recursos.| 
|modelo baseado em vCore|Este modelo permite-lhe dimensionar recursos de computação e armazenamento de forma independente. Ele também permite-lhe utilizar o Azure Hybrid Benefit para o SQL Server para obter poupanças de custos.|Melhor para os clientes que o valor de flexibilidade, controlo e transparência.|
||||  

![modelo de preços](./media/sql-database-service-tiers/pricing-model.png)

## <a name="vcore-based-purchasing-model"></a>modelo de compra baseado em vCore 

Um núcleo virtual representa a CPU lógica oferecida com a opção de escolher entre gerações de hardware. O modelo de compra baseado em vCore oferece a flexibilidade, o controle, a transparência de consumo de recursos individuais e uma forma direta de traduzir locais requisitos de carga de trabalho para a cloud. Este modelo permite-lhe dimensionar a computação, memória e armazenamento com base nas suas necessidades de carga de trabalho. No modelo de compra baseado em vCore, os clientes podem escolher entre as camadas de críticos de serviço de negócios e fins gerais para ambos [bases de dados únicas](sql-database-single-database-scale.md) e [conjuntos elásticos](sql-database-elastic-pool.md). 

O modelo de compra baseado em vCore permite-lhe dimensionar recursos de computação e armazenamento, combine o desempenho no local e otimizar o preço de forma independente. Se a sua base de dados ou conjunto elástico consome mais de 300 conversão de DTU para vCore pode reduzir o custo. Pode converter a utilizar a API de escolha ou no portal do Azure, sem períodos de indisponibilidade. No entanto, a conversão não é necessária. Se o modelo de compra DTU cumpre os requisitos de negócios e desempenho, deve continuar a utilizá-lo. Se optar por converter o modelo de DTU para o modelo de vCore, selecione o nível de desempenho com a seguinte regra geral: cada 100 DTUS no escalão Standard requer, pelo menos, 1 vCore na camada de fins gerais; cada 125 DTU no escalão Premium requer, pelo menos, 1 vCore na camada de negócio críticos.

No modelo de compra baseado em vCore, os clientes pagam pelo:
- Computação (camada de serviços + número de vCores + geração de hardware) *
- Tipo e a quantidade de armazenamento de dados e de registo 
- Número de IOs * *
- Cópia de segurança de armazenamento (RA-GRS) * * 

\* Na pré-visualização pública inicial, as CPUs lógicas do Gen 4 baseiam-se no Intel E5-2673 v3 processadores de 2,4 GHz (Haswell).

\*\* Durante a pré-visualização, 7 dias de cópias de segurança e o IOs são gratuitos.

> [!IMPORTANT]
> Computação, IOs, dados e armazenamento de registo é cobrada por base de dados ou conjunto elástico. Armazenamento de cópias de segurança é cobrado por cada base de dados. Para obter detalhes de encargos de instância gerida, consulte [instância gerida da base de dados SQL do Azure](sql-database-managed-instance.md).
> **Limitações de região:** o modelo de compra baseado em vCore ainda não está disponível nas seguintes regiões: Europa Ocidental, Centro de França, sul do Reino Unido, oeste do Reino Unido e Sudeste da Austrália.

## <a name="dtu-based-purchasing-model"></a>Modelo de compra baseado em DTU

As unidades de transação de base de dados (DTU) representa uma medida combinada de CPU, memória, lê e escreve. O modelo de compra baseado em DTU oferece um conjunto de pacotes de recursos de computação pré-configurados e armazenamento para impulsionar diferentes níveis de desempenho do aplicativo incluído. Os clientes que preferem a simplicidade de um pacote pré-configurado e pagamentos fixos a cada mês, pode encontrar o modelo baseado em DTU mais adequado às suas necessidades. No modelo de compra baseado em DTU, os clientes podem escolher entre **básica**, **padrão**, e **Premium** escalões de serviço para ambos [bases de dadosúnicas](sql-database-single-database-scale.md) e [conjuntos elásticos](sql-database-elastic-pool.md). 

### <a name="what-are-database-transaction-units-dtus"></a>O que são unidades de transação de base de dados (DTUs)?
Para uma base de dados SQL do Azure num nível de desempenho específico dentro de um [escalão de serviço](sql-database-single-database-scale.md), Microsoft garante um determinado nível de recursos para essa base de dados (independentemente de qualquer outro banco de dados na cloud do Azure), fornecendo um previsível nível de desempenho. A quantidade de recursos é calculada como um número de unidades de transação de base de dados ou DTUs e é uma medida integrados em recursos de computação, armazenamento e e/s. A relação entre estes recursos foi originalmente determinada por uma [carga de trabalho do OLTP benchmark](sql-database-benchmark-overview.md), projetado para ser típica das cargas de trabalho do mundo real OLTP. Quando a carga de trabalho excede a quantidade de qualquer um desses recursos, o débito é limitados - resultante no tempos limite e desempenho mais lento. Os recursos utilizados pela sua carga de trabalho não afetam os recursos disponíveis para outros bancos de dados do SQL na cloud do Azure e os recursos utilizados por outras cargas de trabalho não afetam os recursos disponíveis para a base de dados SQL.

![caixa delimitadora](./media/sql-database-what-is-a-dtu/bounding-box.png)

DTUs são mais úteis para compreender o nível relativo de recursos entre bases de dados do Azure SQL em diferentes níveis de desempenho e escalões de serviço. Por exemplo, duplicar as DTUs aumentando o nível de desempenho de uma base de dados equivale a duplicar o conjunto de recursos disponíveis para essa base de dados. Por exemplo, uma base de dados Premium P11 com 1750 DTUs fornece 350 x mais potência de computação de DTUs do que uma base de dados básica com cinco DTUs.  

Para obter informações mais aprofundadas sobre o consumo de recursos (DTUS) da sua carga de trabalho, utilize [do Azure SQL Database Query Performance Insight](sql-database-query-performance.md) para:

- Identificar as consultas principais por contagem de CPU/duração/execução que potencialmente pode ser otimizada para um melhor desempenho. Por exemplo, uma consulta de intensiva de e/s pode beneficiar do uso da opção [técnicas de otimização de memória](sql-database-in-memory.md) para garantir uma melhor utilização da memória disponível num determinado escalão e o desempenho nível de serviço.
- Desagregar os detalhes de uma consulta, ver o texto e o histórico de utilização de recursos.
- Recomendações que mostram as ações realizadas por de otimização de desempenho de acesso [Assistente de base de dados SQL](sql-database-advisor.md).

Pode alterar [escalões de serviço DTU](sql-database-service-tiers-dtu.md) em qualquer altura com o período de indisponibilidade mínimo para a sua aplicação (em média, menos de quatro segundos). Para muitas empresas e aplicações, ser capaz de criar bases de dados e aumentar e reduzir o desempenho a pedido é suficiente, sobretudo se os padrões de utilização forem relativamente previsíveis. No entanto, se tiver padrões de utilização imprevisíveis, pode tornar-se difícil gerir os custos e o seu modelo de negócio. Para este cenário, utilize um conjunto elástico com um determinado número de eDTUs que são partilhados entre várias bases de dados no conjunto.

![Introdução à Base de Dados SQL: DTUs de base de dados individual por camada e nível](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="what-are-elastic-database-transaction-units-edtus"></a>O que são unidades de transação de base de dados elástica (eDTUs)?
Em vez disso, apenas fornecer um conjunto dedicado de recursos (DTUs) que pode não ser sempre necessária para uma base de dados do SQL que estará sempre disponível, pode colocar as bases de dados num [conjunto elástico](sql-database-elastic-pool.md) num servidor de base de dados SQL que partilham um conjunto de recursos entre essas bases de dados. Os recursos partilhados num conjunto elástico são avaliados por unidades de transação de base de dados de elásticas ou eDTUs. Os conjuntos elásticos fornecem uma solução rentável simples para gerir os objetivos de desempenho de várias bases de dados ter variáveis e padrões de utilização imprevisíveis. Um conjunto elástico garante recursos não podem ser consumidos por um banco de dados no agrupamento, enquanto garantir que cada base de dados no conjunto sempre tem uma quantidade mínima de recursos necessários disponíveis. 

![Introdução à Base de Dados SQL: eDTUs por camada e nível](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Um conjunto é atribuído um número definido de eDTUs por um preço definido. Dentro do conjunto elástico, é dada às bases de dados individuais a flexibilidade para se dimensionarem automaticamente dentro dos limites configurados. Uma base de dados numa carga mais pesada irá consumir mais eDTUs para responder à procura. Bases de dados sob cargas mais leves consumirá menos eDTUs. Bases de dados com qualquer carga não serão não consomem eDTUs. Aprovisionamento de recursos para o conjunto completo, vez por base de dados, tarefas de gestão são simplificadas, fornecendo um orçamento previsível para o conjunto.

Podem ser adicionais mais eDTUs a um conjunto existente sem qualquer período de indisponibilidade da base de dados e sem impacto nas bases de dados do conjunto. Do mesmo modo, se as eDTUs adicionais já não forem necessárias, podem ser removidas de um conjunto existente em qualquer momento. Pode adicionar ou subtrair bases de dados para o pool ou limitar a quantidade de eDTUs que uma base de dados pode utilizar com muita carga para reservar eDTUs para outras bases de dados. Se uma base de dados estiver a subutilizar recursos, pode movê-lo para fora do conjunto e configurá-la como uma base de dados com uma quantidade previsível dos recursos necessários.

### <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Como posso determinar o número de DTUs necessárias para a minha carga de trabalho?
Se pretender migrar a carga de trabalho de uma máquina virtual do SQL Server ou no local existente para a Base de Dados SQL do Azure, pode utilizar a [Calculadora de DTUs](http://dtucalculator.azurewebsites.net/) para se aproximar do número de DTUs necessárias. Para uma carga de trabalho de base de dados do Azure SQL existente, pode usar [SQL Database Query Performance Insight](sql-database-query-performance.md) para compreender o consumo de recursos de base de dados (DTUs) para obter informações mais aprofundadas sobre para otimizar a sua carga de trabalho. Também pode utilizar o [sys resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV para ver o consumo de recursos para a última hora. Em alternativa, a exibição de catálogo [resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) mostra o consumo de recursos para os últimos 14 dias, mas a uma fidelidade inferior das médias de cinco minutos.

### <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Como posso saber se poderia beneficiar de um conjunto elástico de recursos?
Os conjuntos são adequados para um grande número de bases de dados com padrões de utilização específicos. Para um determinado banco de dados, este padrão é caracterizado por uma média de utilização baixa com picos de utilização relativamente raros. A Base de Dados SQL avalia automaticamente o histórico de utilização de recursos de bases de dados num servidor de Base de Dados SQL existente e recomenda a configuração de conjunto adequada no portal do Azure. Para obter mais informações, consulte [When should an elastic pool be used? (Quando deve ser utilizado um conjunto elástico?)](sql-database-elastic-pool.md)

### <a name="what-happens-when-i-hit-my-maximum-dtus"></a>O que acontece quando atinjo o limite máximo de DTUs?
Níveis de desempenho são calibrados e regem-se para fornecer os recursos necessários para executar a sua carga de trabalho de base de dados até o máximo permitido para o seu nível de desempenho/da camada de serviço selecionado. Se a sua carga de trabalho está a ocorrer um os limites de e/s de e/s/do registo de CPU/dados, continuará a receber o nível máximo de recursos permitidos, mas provavelmente também irá ocorrer latências de aumento de consulta. Estes limites não resultam em erros, mas num abrandamento da carga de trabalho, a menos que o abrandamento se torne tão acentuado que as consultas comecem a exceder o tempo limite. Se atingir os máximo utilizador em simultâneo permitidos sessões/pedidos (threads de trabalho), serão apresentados erros específicos. Ver [limites de recursos do Azure SQL Database]( sql-database-resource-limits.md#what-happens-when-database-resource-limits-are-reached) para obter informações sobre limites de recursos não relacionados com a CPU, memória, e/s de dados ou e/s de registo de transação.

### <a name="correlating-benchmark-results-to-real-world-database-performance"></a>Correlacionando os resultados do benchmark do desempenho de base de dados do mundo real
É importante compreender que todos os benchmarks são representativas e indicativos apenas. As taxas de transação obtidas com a aplicação de parâmetro de comparação não serão iguais aos que pode ser alcançado com outros aplicativos. O parâmetro de comparação é composto por uma coleção de transação diferentes tipos de executam num esquema que contém uma série de tabelas e os tipos de dados. Embora o benchmark exercita as mesmas operações básicas que são comuns a todas as cargas de trabalho OLTP, não representam qualquer classe específica de banco de dados ou aplicação. O objetivo do parâmetro de comparação é fornecer um guia razoável para o desempenho relativo de uma base de dados que pode ser esperado quando aumentando ou reduzindo entre níveis de desempenho. Na realidade, bases de dados são de tamanhos diferentes e a complexidade, encontram mixes diferentes de cargas de trabalho e irão responder de formas diferentes. Por exemplo, um aplicativo de e/s intensiva pode atingir os limites de e/s mais cedo ou um aplicativo de uso intenso da CPU pode atingir os limites de CPU mais cedo. Não é garantido que qualquer banco de dados específico dimensionará da mesma forma como o parâmetro de comparação em aumento de carga.

O parâmetro de comparação e a sua metodologia são descritas em mais detalhes abaixo.

### <a name="benchmark-summary"></a>Resumo de benchmark
ASDB mede o desempenho de uma combinação de operações de banco de dados básicos que ocorrem mais frequentemente em cargas de trabalho (OLTP) de processamento de transações online. Embora o benchmark destina-se com computação de nuvem em mente, o esquema de base de dados, a população de dados e transações foram concebidas para ser amplamente representa os elementos básicos, geralmente utilizados em cargas de trabalho OLTP.

### <a name="schema"></a>Esquema
O esquema foi concebido para ter suficiente variedade e complexidade para oferecer suporte a uma ampla gama de operações. O parâmetro de comparação é executado numa base de dados composto por seis tabelas. As tabelas caem em três categorias: tamanho fixo, dimensionar e crescer. Existem duas tabelas de tamanho fixo; três tabelas dimensionamento; e uma tabela de cada vez maior. Tabelas de tamanho fixo têm um número constante de linhas. Dimensionamento tabelas têm uma cardinalidade que é proporcional ao desempenho da base de dados, mas não muda durante o benchmark. A tabela de cada vez maior é o tamanho como uma tabela de dimensionamento no carregamento inicial, mas, em seguida, as alterações de cardinalidade no decorrer de executar o benchmark como linhas são inseridas e eliminadas.

O esquema inclui uma combinação de tipos de dados, incluindo o número inteiro, numéricos, de caracteres e de data/hora. O esquema inclui chaves primárias e secundárias, mas não quaisquer chaves externas - ou seja, existem sem restrições de integridade referencial entre tabelas.

Um programa de geração de dados gera os dados da base de dados inicial. Dados de número inteiro e numéricos são gerados com várias estratégias. Em alguns casos, os valores são distribuídos aleatoriamente ao longo de um intervalo. Em outros casos, um conjunto de valores aleatoriamente é permuted para garantir que uma distribuição específica é mantida. Campos de texto são gerados a partir de uma lista ponderada de palavras para produzir os dados de aparência realistas.

A base de dados tem o tamanho com base no "fator de escala". O fator de dimensionamento (abreviado como SF) determina a cardinalidade da dimensionar e crescer tabelas. Conforme descrito abaixo na secção utilizadores e Pacing, o tamanho da base de dados, número de utilizadores e o desempenho máximo todos Dimensionar em proporção entre si.

### <a name="transactions"></a>Transações
A carga de trabalho consiste em nove tipos de transação, conforme mostrado na tabela abaixo. Cada transação foi concebida para destacar um determinado conjunto de características de sistema na base de dados sistema e de motor de hardware, com alto contraste das outras transações. Essa abordagem torna mais fácil avaliar o impacto de diferentes componentes para o desempenho geral. Por exemplo, a transação "Leitura pesado" produz um número significativo de operações de leitura do disco.

| Tipo de Transação | Descrição |
| --- | --- |
| Ler Lite |SELECIONAR; dentro da memória; só de leitura |
| Média de leitura |SELECIONAR; principalmente na memória; só de leitura |
| Pesado de leitura |SELECIONAR; não principalmente na memória; só de leitura |
| Atualização Lite |ATUALIZAR; dentro da memória; leitura-escrita |
| Atualizar pesado |ATUALIZAR; não principalmente na memória; leitura-escrita |
| INSERT Lite |INSERIR; dentro da memória; leitura-escrita |
| Inserir pesado |INSERIR; não principalmente na memória; leitura-escrita |
| Eliminar |ELIMINAR; combinação de dentro da memória e não na memória; leitura-escrita |
| Pesado de CPU |SELECIONAR; dentro da memória; carga de CPU relativamente pesada; só de leitura |

### <a name="workload-mix"></a>Combinação de carga de trabalho
Transações são selecionadas aleatoriamente entre uma distribuição ponderada com a mistura geral seguinte. A mistura geral tem um rácio de leitura/escrita de aproximadamente 2:1.

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
| Pesado de CPU |10 |

### <a name="users-and-pacing"></a>Os utilizadores e o ritmo de processamento
A carga de trabalho de parâmetro de comparação é orientada a partir de uma ferramenta que submete transações num conjunto de ligações para simular o comportamento de um número de utilizadores em simultâneo. Embora todas as ligações e transações sejam máquina gerada, para manter a simplicidade nos Referimos a estas ligações como "utilizadores". Embora cada usuário funciona independentemente todos os outros utilizadores, todos os utilizadores executam o mesmo ciclo de passos abaixo:

1. Estabelece uma ligação de base de dados.
2. Repita até que o indicado para sair:
   * Selecione uma transação aleatoriamente (a partir de uma distribuição ponderada).
   * Executar a transação selecionada e medir o tempo de resposta.
   * Aguarde um atraso de ritmo de processamento.
3. Feche a ligação de base de dados.
4. Saída.

O atraso ritmo de processamento (no passo c de 2) está selecionado aleatoriamente, mas com uma distribuição que tenha uma média de segundo 1.0. Assim, a cada utilizador pode, a gerar em média, no máximo uma transação por segundo.

### <a name="scaling-rules"></a>Regras de dimensionamento
O número de utilizadores é determinado pelo tamanho da base de dados (em unidades de fator de escala). Existe um utilizador para todos os cinco unidades de fator de escala. Devido a atraso o ritmo de processamento, um usuário pode gerar um máximo de uma transação por segundo, em média.

Por exemplo, um-fator de escala de 500 (SF = 500) base de dados terão de 100 utilizadores e pode chegar um ritmo máximo de 100 TPS. Para orientar um TPS maior taxa requer mais usuários e uma base de dados maior.

A tabela abaixo mostra o número de utilizadores, na verdade, a constante para cada nível de desempenho e a camada de serviço.

| Escalão de serviço (nível de desempenho) | Utilizadores | Tamanho da Base de Dados |
| --- | --- | --- |
| Básica |5 |720 MB |
| Standard (S0) |10 |1 GB |
| Standard (S1) |20 |2.1 GB |
| Standard (S2) |50 |7.1 GB |
| Premium (P1) |100 |14 GB |
| Premium (P2) |200 |28 GB |
| Premium (P6) |800 |114 GB |

### <a name="measurement-duration"></a>Duração de medida
Um parâmetro de comparação válido executar requer uma duração de medição do estado estável de, pelo menos, uma hora.

### <a name="metrics"></a>Métricas
As métricas chave no benchmark são débito e tempo de resposta.

* O débito é a medida de desempenho essenciais no benchmark. Taxa de transferência é reportada no transações por unidade-de-time, contagem de todos os tipos de transação.
* Tempo de resposta é uma medida de previsibilidade do desempenho. A restrição de tempo de resposta varia de acordo com a classe de serviço, com mais classes de serviço com um requisito de tempo de resposta mais rigoroso, conforme mostrado abaixo.

| Classe de serviço | Medida de débito | Requisito de tempo de resposta |
| --- | --- | --- |
| Premium |Transações por segundo |é o percentil 95 0,5 segundos |
| Standard |Transações por minuto |percentil 90th em segundos 1.0 |
| Básica |Transações por hora |percentil 80th em segundos 2.0 |

## <a name="next-steps"></a>Passos Seguintes

- Para o modelo de compra baseado em vCore, consulte [o modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md)
- Para o modelo de compra baseado em DTU, consulte [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md).
