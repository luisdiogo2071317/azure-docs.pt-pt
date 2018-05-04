---
title: Gerir recursos de computação no Azure SQL Data Warehouse | Microsoft Docs
description: Saiba mais sobre o desempenho de ampliação capacidades no Azure SQL Data Warehouse. Aumentar horizontalmente ao ajustar as DWUs ou custos inferiores ao colocar em pausa o armazém de dados.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: ca6d34d3b670bfd05a9b65fe9e6b260120e3a5b8
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/18/2018
---
# <a name="manage-compute-in-azure-sql-data-warehouse"></a>Gerir computação no Azure SQL Data Warehouse
Saiba mais sobre a gestão de recursos de computação no Azure SQL Data Warehouse. Baixar os custos ao colocar em pausa o armazém de dados ou aumentar o armazém de dados para satisfazer os pedidos de desempenho. 

## <a name="what-is-compute-management"></a>O que é a gestão de computação?
A arquitetura do SQL Data Warehouse separa o armazenamento e computação independentes, permitindo cada dimensionar de forma independente. Como resultado, pode dimensionar a computação para satisfazer os pedidos de desempenho independentes do armazenamento de dados. Também pode colocar em pausa e retomar a recursos de computação. Um consequence natural colateral desta arquitetura é que [faturação](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) de computação e de armazenamento está separado. Se não precisa de utilizar o seu armazém de dados para o tempo, pode poupar custos de computação por colocar em pausa computação. 

## <a name="scaling-compute"></a>Dimensionamento de computação
Pode aumentar ou reduzir horizontalmente computação back ao ajustar o [unidades do armazém de dados](what-is-a-data-warehouse-unit-dwu-cdwu.md) definição para o seu armazém de dados. Desempenho de consulta e de carregamento pode aumentar de forma linear à medida que adiciona mais unidades de armazém de dados. 

Para obter passos de escalamento horizontal, consulte o [portal do Azure](quickstart-scale-compute-portal.md), [PowerShell](quickstart-scale-compute-powershell.md), ou [T-SQL](quickstart-scale-compute-tsql.md) inícios rápidos. Também pode efetuar operações de escalamento horizontal com um [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute).

Para efetuar uma operação de dimensionamento, o SQL Data Warehouse inutilizam primeiro todas as consultas de entrada e, em seguida, reverte as transações para Certifique-se num estado consistente. Dimensionamento só ocorre depois de concluída a reversão de transação. Para uma operação de dimensionamento, o sistema Desanexa a camada de armazenamento de nós de computação, adiciona nós de computação e, em seguida, reattaches a camada de armazenamento para a camada de computação. Cada armazém de dados é armazenado como 60 distribuições, que são distribuídas uniformemente em nós de computação. Adicionar mais nós de computação, adiciona que mais poder de computação. À medida que aumenta o número de nós de computação, reduz o número de distribuições por nó de computação, fornecer maior potência de computação para as suas consultas. Da mesma forma, a diminuir unidades do data warehouse reduz o número de nós de computação, o que reduz os recursos de computação para consultas.

A tabela seguinte mostra como o número de distribuições por alterações de nó de computação como as unidades de armazém de dados alterados.  DWU6000 fornece 60 nós de computação e distribui muito maior desempenho das consultas que DWU100. 

| Unidades do armazém de dados  | \# de nós de computação | \# de distribuições por nó |
| ---- | ------------------ | ---------------------------- |
| 100  | 1                  | 60                           |
| 200  | 2                  | 30                           |
| 300  | 3                  | 20                           |
| 400  | 4                  | 15                           |
| 500  | 5                  | 12                           |
| 600  | 6                  | 10                           |
| 1000 | 10                 | 6                            |
| 1200 | 12                 | 5                            |
| 1500 | 15                 | 4                            |
| 2000 | 20                 | 3                            |
| 3000 | 30                 | 2                            |
| 6000 | 60                 | 1                            |


## <a name="finding-the-right-size-of-data-warehouse-units"></a>Localizar o tamanho adequado de unidades do data warehouse

Para ver os benefícios de desempenho do dimensionamento, especialmente para unidades de armazém de dados maiores, que pretende utilizar, pelo menos, um conjunto de dados de 1 TB. Para determinar o melhor número de unidades de armazém de dados para o seu armazém de dados, experimente aumentar e reduzir verticalmente. Execute algumas consultas com diferentes números de unidades de armazém de dados depois de carregar os dados. Uma vez que o dimensionamento é rápido, pode tentar vários níveis de desempenho numa hora ou menos. 

Recomendações para localizar o número mais adequado de dados do armazém de unidades:

- Para um armazém de dados de desenvolvimento, comece por selecionar um menor número de unidades do data warehouse.  Um ponto de partida é DW400 ou DW200.
- Monitorize o desempenho de aplicações, observar o número de unidades de armazém de dados selecionada em comparação comparado o desempenho que observar.
- Partem do princípio de escala linear e determinar a quantidade tem de aumentar ou diminuir as unidades de armazém de dados. 
- Continue a efetuar ajustes até chegar um nível de desempenho ideais para os seus requisitos de negócio.

## <a name="when-to-scale-out"></a>Quando a ampliar
Aumentar horizontalmente unidades do data warehouse tem impacto sobre estes aspetos de desempenho:

- Forma linear melhora o desempenho do sistema para análises, agregações e CTAS instruções.
- Aumenta o número de leitores e escritores para carregar dados.
- Número máximo de consultas em simultâneo e de ranhuras de concorrência.

Recomendações para quando ampliar a dados do armazém de unidades:

- Antes de executar uma operação de transformação ou de carregamento de dados pesados, aumentar horizontalmente para disponibilizar os dados mais rapidamente.
- Durante o horário de pico, aumentar horizontalmente para acomodar grandes quantidades de consultas em simultâneo. 

## <a name="what-if-scaling-out-does-not-improve-performance"></a>E se ampliar não melhora o desempenho?

A adicionar a aumentar o paralelismo de unidades do data warehouse. Se o trabalho é dividido uniformemente entre os nós de computação, o paralelismo adicional melhora o desempenho de consulta. Se aumentar horizontalmente não está a alterar o seu desempenho, existem alguns motivos por que motivo Isto poderá acontecer. Os dados poderão ser desviados para as distribuições ou consultas poderão estar a introduzir uma grande quantidade de movimento de dados. Para investigar problemas de desempenho de consulta, consulte [resolução de problemas de desempenho](sql-data-warehouse-troubleshoot.md#performance). 

## <a name="pausing-and-resuming-compute"></a>Colocar em pausa e retomar a computação
Computação colocar em pausa faz com que a camada de armazenamento para anular a exposição de nós de computação. Os recursos de computação são lançados da sua conta. Não lhe serem cobrados da computação enquanto computação está em pausa. A retomar a computação reattaches armazenamento para os nós de computação e retoma encargos de computação. Quando colocar em pausa um armazém de dados:

* Recursos de computação e memória são devolvidos para o agrupamento de recursos disponíveis no Centro de dados
* Custos de unidade do armazém de dados são zero para a duração de pausa.
* Armazenamento de dados não é afetado e os dados permanecem intactos. 
* O SQL Data Warehouse cancela todas as operações em execução ou em fila.

Quando retomar um armazém de dados:

* O SQL Data Warehouse adquire recursos de computação e memória para as unidades de armazém de dados a definição.
* Os encargos para a retoma de unidades de armazém de dados de computação.
* Os dados ficam disponíveis.
* Depois do armazém de dados estiver online, terá de reiniciar as consultas de carga de trabalho.

Se pretender que a sempre o armazém de dados acessível, considere dimensionamento-a para baixo até o tamanho mais pequeno, em vez de colocar em pausa. 

Para colocar em pausa e retomar passos, consulte o [portal do Azure](pause-and-resume-compute-portal.md), ou [PowerShell](pause-and-resume-compute-powershell.md) inícios rápidos. Também pode utilizar o [colocar em pausa a REST API](sql-data-warehouse-manage-compute-rest-api.md#pause-compute) ou [retomar a REST API](sql-data-warehouse-manage-compute-rest-api.md#resume-compute).

## <a name="drain-transactions-before-pausing-or-scaling"></a>Drenar transações antes de colocar em pausa ou dimensionar
Recomendamos que transações existentes sejam concluídas antes de iniciar uma operação de colocação em pausa ou escala.

Ao colocar em pausa ou dimensionar o SQL Data Warehouse, em segundo plano, as consultas são canceladas quando iniciar o pedido de colocação em pausa ou dimensionamento.  Cancelar uma consulta SELECT simples é uma operação rápida e quase não tem impacto sobre o tempo que demora a colocar em pausa ou a dimensionar a sua instância.  No entanto, as consultas transacionais que modificam os seus dados ou a estrutura dos mesmos, podem não ser paradas rapidamente.  **As consultas transacionais, por definição, têm de ser concluídas na sua totalidade ou têm de reverter as alterações.**  A reversão do trabalho concluído por uma consulta transacional pode demorar tanto tempo, ou mais, como a alteração original que a consulta aplicou.  Por exemplo, se cancelar uma consulta que estava a eliminar linhas e que já estiva a ser executada há uma hora, o sistema pode demorar uma hora a inserir novamente as linhas que foram eliminadas.  Se colocar em pausa ou dimensionar enquanto as transações estiverem a decorrer, a colocação em pausa ou o dimensionamento podem demorar muito tempo, uma vez que a colocação em pausa e o dimensionamento têm de aguardar que a reversão esteja concluída para poderem continuar.

Consulte também [transações compreender](sql-data-warehouse-develop-transactions.md), e [otimizar transações](sql-data-warehouse-develop-best-practices-transactions.md).

## <a name="automating-compute-management"></a>Automatizar a gestão de computação
Para automatizar as operações de gestão de computação, consulte [gerir computação com as funções do Azure](manage-compute-with-azure-functions.md).

Cada um de escalamento horizontal, colocar em pausa e operações de retoma pode demorar vários minutos a concluir. Se o dimensionamento são, colocar em pausa, ou retomar automaticamente, recomendamos que implementar a lógica para garantir que determinadas operações concluiu antes de continuar com a ação de outra. A verificar o estado do armazém de dados através de vários pontos finais permite-lhe implementar corretamente a automatização de operações. 

Para verificar o estado do armazém de dados, consulte o [PowerShell](quickstart-scale-compute-powershell.md#check-data-warehouse-state) ou [T-SQL](quickstart-scale-compute-tsql.md#check-data-warehouse-state) início rápido. Também pode verificar o estado do armazém de dados com um [REST API](sql-data-warehouse-manage-compute-rest-api.md#check-database-state).


## <a name="permissions"></a>Permissões

Dimensionamento do armazém de dados requer as permissões descritas [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse).  Colocar em pausa e retomar requerem o [contribuinte da BD SQL](../role-based-access-control/built-in-roles.md#sql-db-contributor) permissão, especificamente Microsoft.Sql/servers/databases/action.


## <a name="next-steps"></a>Passos Seguintes
Outro aspeto de gerir recursos de computação é alocar recursos de computação diferentes para consultas individuais. Para obter mais informações, consulte [classes de recursos para a gestão de carga de trabalho](resource-classes-for-workload-management.md).
