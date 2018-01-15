---
title: Cheat folha Azure SQL Data Warehouse | Microsoft Docs
description: "Localize hiperligações, melhores práticas para criar rapidamente as soluções do Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: acomet
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 12/14/2017
ms.author: acomet
ms.openlocfilehash: 2d17385ff255ddf7b85baa81600a2af60d015540
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/12/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Cheat folha para o Azure SQL Data Warehouse
Esta página deve ajudar a conceber para casos de utilização da principal a solução de armazém de dados. Esta folha de referência deve ser um suporte excelente da sua viagem para compilar um armazém de dados de trabalho, mas recomendamos vivamente a obter mais informações sobre cada passo nos detalhes. Em primeiro lugar, é recomendado ler este artigo excelente sobre o SQL Data Warehouse  **[e não é]**.

Segue-se um sketch do processo que deve seguir quando começar a estrutura do armazém de dados do SQL Server.

![Sketch]

## <a name="queries-and-operations-across-tables"></a>As consultas e operações em tabelas

É verdadeiramente importante saber antecipadamente as operações mais importantes e consultas executadas no seu armazém de dados. A arquitetura de armazém de dados deve ser definida para essas operações. Exemplos comuns de operações pode ser:
* Associar uma ou duas tabelas de factos com dimensão tabelas, filtragem esta tabela durante um período de tempo e de acréscimo os resultados para um data mart
* Efetuar atualizações de grandes ou pequenas na sua vendas facto
* Acrescentar dados apenas para as tabelas

Saber o tipo de operações ajuda-o a otimizar a estrutura da sua tabelas.

## <a name="data-migration"></a>Migração de Dados

Recomendamos que primeiro carregar os dados  **[para ADLS]**  ou armazenamento de Blobs do Azure. Em seguida, deve utilizar o Polybase para carregar os dados para o SQL Data Warehouse numa tabela de teste. Recomendamos a seguinte configuração:

| Design | Recomendação |
|:--- |:--- |
| Distribuição | Round Robin |
| Indexação | Área dinâmica para dados |
| Criação de partições | Nenhum |
| Classe de Recursos | largerc ou xlargerc |

Saiba mais sobre  **[migração de dados], [carregamento de dados]**  com  **[orientações mais profunda] no carregamento**. 

## <a name="distributed-or-replicated-tables"></a>Tabelas replicadas ou distribuídas

Recomendamos as seguintes estratégias consoante as propriedades de tabela:

| Tipo | Excelente opção para | Tenha atenção se...|
|:--- |:--- |:--- |
| Replicado | • As tabelas de pequena dimensão de um esquema em estrela com menos de 2 GB de armazenamento após compressão (compressão de x ~ 5) |• Muitas transações de escrita na tabela (por ex.: Inserir, upsert, eliminar, atualização)<br></br>• Alterar unidades de armazém de dados (DWU) com frequência de aprovisionamento<br></br>• Utilizar apenas 2-3 colunas e a sua tabela tem demasiadas colunas<br></br>• Índice uma tabela não replicada |
| O Round Robin (predefinição) | Tabela de temporário/testes •<br></br> • Não óbvios a associar a coluna de chave ou um bom candidato |• Um desempenho lento devido ao movimento de dados |
| Hash | Tabelas de factos •<br></br>• Tabelas de grande dimensão |• Não é possível atualizar a chave de distribuição |

**Sugestões:**
* Começar a utilizar o Round Robin, mas aspire para uma estratégia de distribuição de hash para tirar partido de uma arquitetura de paralelas em grande escala
* Certifique-se de que as chaves de hash comuns têm o mesmo formato de dados
* Não distribuir no formato varchar
* Dimensão tabelas com a chave de hash comuns a uma tabela de factos com operações de associação frequentes pode ser distribuído hash
* Utilize  *[sys.dm_pdw_nodes_db_partition_stats]*  para analisar quaisquer skewness dos dados
* Utilize  *[sys.dm_pdw_request_steps]*  para analisar movimentos de dados por trás de consultas, monitorizar a difusão de tempo e as operações de shuffle demorar. É útil para rever a estratégia de distribuição.

Saiba mais sobre  **[tabelas replicadas] e [distribuídas tabelas]**.

## <a name="indexing-your-table"></a>A tabela de indexação

A indexação é **excelente** para ler rapidamente tabelas. Há um conjunto exclusivo de tecnologias que pode utilizar com base nas necessidades:

| Tipo | Excelente opção para | Tenha atenção se...|
|:--- |:--- |:--- |
| Área dinâmica para dados | Tabela de teste/temporário •<br></br>• Tabelas pequeno com pesquisas pequenas |• Qualquer pesquisa analisa a tabela completa |
| Índice em cluster | • Até a tabela de linhas de 100-m<br></br>• Tabelas grandes (linhas de mais de 100-m) com apenas 1-2 colunas descontos elevados são utilizadas |• Utilizado em tabelas replicadas<br></br>• Consultas complexas, envolvendo vários associação, Group By operações<br></br>• Disponibilizar atualizações nas colunas indexadas, demora memória |
| Índice Columnstore em cluster (CCI) (predefinição) | • Tabelas grandes (mais de 100-m linhas) | • Utilizado em tabelas replicadas<br></br>• Efetuar enormes atualizar operações na sua tabela<br></br>• Excessiva particionar a tabela: grupos de linhas não span em nós de distribuição diferente e partições |

**Sugestões:**
* Por cima de um índice em cluster, pode querer adicionar índice não em cluster para uma coluna muito utilizada para o filtro. 
* Seja cuidadoso como gerir a memória numa tabela com CCI. Carregar dados, quer o utilizador (ou a consulta) para tirar partido de uma classe de recursos de grandes dimensões. Certificar-se evitar corte e criar vários grupos de linhas de comprimido pequeno
* Otimizado para a camada de computação rocks com CCI
* Para CCI, um desempenho lento pode acontecer devido a compressão fraco dos seus grupos de linha, pode querer reconstruir ou reorganizar o CCI. Pretende que, pelo menos, 100 linhas k por grupos de linhas comprimido. O ideal é 1-m linhas no grupo de linhas.
* Com base na frequência de carregamento incremental e tamanho, que pretende automatizar quando reorganize ou reconstruir os índices. Limpeza de mola sempre é útil.
* Ser estratégicos quando pretende compactar um grupo de linhas: como grandes são os grupos de linhas aberto? Quantidade de dados espera que a carga nos próximos dias?

Saiba mais sobre  **[índices]**.

## <a name="partitioning"></a>Criação de partições
Pode particionar a tabela quando tiver um tabelas de factos grande (> 1B tabela de linha). % de 99 dos casos, a chave de partição deve ser baseada no data. Tenha o cuidado de não superior partição, sobretudo quando tem um índice de Columnstore em cluster.
Com as tabelas que necessitam de ETL de teste, pode beneficiar de criação de partições. Facilita a gestão de ciclo de vida de dados.
Tenha cuidado não de overpartition os seus dados, especialmente num índice Columnstore em cluster.

Saiba mais sobre  **[partições]**.

## <a name="incremental-load"></a>Carga incremental

Em primeiro lugar, deve certificar-se que atribua maior recursos classes para carregar os dados. Recomendamos que utilize o Polybase e ADF V2 para automatizar os seus pipelines ETL no armazém de dados do SQL Server.

Para um lote grande de atualizações nos seus dados históricos, recomendamos que primeiro eliminar os dados pretendidos. Em seguida, pode efetuar um bulk insert de novos dados. Esta abordagem de passo 2 é mais eficiente.

## <a name="maintain-statistics"></a>Manter as estatísticas
Estatísticas de automática que vão ser normalmente disponível em breve. Até lá, o SQL Data Warehouse requer manutenção manual de estatísticas. É importante atualizar as estatísticas como **significativas** acontecer alterações aos seus dados. Isto ajuda a otimizar os planos de consulta. Se achar que demora demasiado tempo a manter todas as estatísticas, poderá ser mais seletiva sobre as colunas tenham estatísticas. Também pode definir a frequência das atualizações. Por exemplo, deve atualizar as colunas de data, onde podem ser adicionados diariamente novos valores. Obtém o maior parte dos benefício, fazendo com que as estatísticas em colunas envolvidas em associações, colunas utilizadas na cláusula WHERE e colunas encontradas no GROUP BY.

Saiba mais sobre  **[estatísticas]**.

## <a name="resource-class"></a>Classe de recursos
O SQL Data Warehouse utiliza grupos de recursos como uma forma de alocar memória para consultas. Se precisar de mais memória para melhorar a consulta ou carregar velocidade, deve alocar superiores classes de recursos. No lado flip, utilizar maior classes de recurso afeta concorrência. Pretende levá-los em consideração antes de mover todos os utilizadores a uma classe de recursos de grandes dimensões.

Se reparar em que as consultas demorar demasiado tempo, certifique-se de que os utilizadores não são executados em classes de recursos de grandes dimensões. Classes de recurso grande consumam muitos ranhuras de concorrência. Estes podem fazer com que outras consultas para a fila.

Por fim, se utilizar a camada de computação otimizada, cada classe de recursos obtém 2,5 x mais memória do que na camada elástico otimizada.

Saiba mais como trabalhar com  **[classes de recursos e simultaneidade]**.

## <a name="lower-your-cost"></a>O custo
Uma funcionalidade fundamental do SQL Data Warehouse é a capacidade de colocação em pausa quando não está a utilizá-lo, o que para a faturação de recursos de computação. Outra funcionalidade importante é a capacidade de dimensionar os recursos. A Colocação em Pausa e o Dimensionamento podem ser feitos através do Portal do Azure ou de comandos do PowerShell.

Escala automática agora no momento em que pretende com as funções do Azure:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-you-architecture-for-performance"></a>Otimizar a arquitetura de desempenho

É recomendável considerar a base de dados SQL e do Azure Analysis Services numa arquitetura de concentrador hub- and spokes a realizar. Essa solução pode proporcionar o isolamento de carga de trabalho entre grupos de utilizadores diferentes, enquanto também tirar partido de algumas funcionalidades de segurança da base de dados do SLQ avançadas e do Azure Analysis Services. Também se trata de uma forma de fornecer simultaneidade ilimitada aos seus utilizadores.

Saiba mais sobre  **[arquiteturas típicas tirar partido de armazém de dados do SQL Server]**.

Implemente um clique a spokes realizar nas bases de dados de base de dados SQL do armazém de dados do SQL Server:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Sketch]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[carregamento de dados]:./design-elt-data-loading.md
[orientações mais profunda]: ./guidance-for-loading-data.md
[índices]:./sql-data-warehouse-tables-index.md
[partições]:./sql-data-warehouse-tables-partition.md
[estatísticas]:./sql-data-warehouse-tables-statistics.md
[classes de recursos e simultaneidade]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[arquiteturas típicas tirar partido de armazém de dados do SQL Server]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[e não é]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[migração de dados]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[tabelas replicadas]:https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[distribuídas tabelas]:https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[para ADLS]: https://docs.microsoft.com/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
