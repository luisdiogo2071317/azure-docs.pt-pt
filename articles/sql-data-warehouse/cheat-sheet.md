---
title: Cheat folha para o Azure SQL Data Warehouse | Microsoft Docs
description: "Localize hiperligações e melhores práticas para criar rapidamente as soluções do Azure SQL Data Warehouse."
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
ms.openlocfilehash: a16c2230c26865913285cb8cbd5b0f81426acdd1
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="cheat-sheet-for-azure-sql-data-warehouse"></a>Cheat folha para o Azure SQL Data Warehouse
Este cábula fornece sugestões úteis e melhores práticas para criar as suas soluções do Azure SQL Data Warehouse. Antes de começar, saiba mais sobre cada passo detalhadamente o lendo [padrões de carga de trabalho de armazém de dados SQL do Azure e padrões de anti](https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns), que explica o que é o SQL Data Warehouse e que não se encontra.

O gráfico seguinte mostra o processo de estruturação de um armazém de dados:

![Sketch]

## <a name="queries-and-operations-across-tables"></a>As consultas e operações em tabelas

Quando souber antecipadamente as operações primárias e as consultas para ser executado no seu armazém de dados, pode atribuir prioridades da arquitetura do armazém de dados para essas operações. Estas operações de consultas e poderão incluir:
* Associar uma ou duas tabelas de factos com as tabelas de dimensões, a tabela combinada de filtragem e, em seguida, acrescentando os resultados para um data mart.
* Efetuar atualizações de grandes ou pequenas na sua vendas de factos.
* Acrescentar dados apenas para as tabelas.

Saber antecipadamente os tipos de operações ajuda-o a otimizar a estrutura da sua tabelas.

## <a name="data-migration"></a>Migração de dados

Em primeiro lugar, carregar os dados em [Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-store) ou armazenamento de Blobs do Azure. Em seguida, utilize o PolyBase para carregar os dados para o SQL Data Warehouse numa tabela de teste. Utilize a seguinte configuração:

| Design | Recomendação |
|:--- |:--- |
| Distribuição | Round Robin |
| Indexação | Área dinâmica para dados |
| Criação de partições | Nenhum |
| Classe de Recursos | largerc ou xlargerc |

Saiba mais sobre [migração de dados], [carregamento de dados]e o [processo de extração, carregamento e transformação (ELT)](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-elt-data-loading). 

## <a name="distributed-or-replicated-tables"></a>Tabelas replicadas ou distribuídas

Utilize as seguintes estratégias, consoante as propriedades de tabela:

| Tipo | Excelente opção para...| Tenha atenção se...|
|:--- |:--- |:--- |
| Replicado | • As tabelas de pequena dimensão de um esquema em estrela com menos de 2 GB de armazenamento após compressão (compressão de x ~ 5) |• Muitas transações de escrita são na tabela (como a inserção upsert, eliminar, atualização)<br></br>• Alterar unidades de armazém de dados (DWU) com frequência de aprovisionamento<br></br>• Utilizar apenas 2-3 colunas, mas a tabela tem demasiadas colunas<br></br>• Índice uma tabela não replicada |
| O Round Robin (predefinição) | • Temporário/teste tabela<br></br> • Não óbvios a associar a coluna de chave ou um bom candidato |• Desempenho for lento devido ao movimento de dados |
| Hash | Tabelas de factos •<br></br>• Tabelas de grande dimensão |• Não é possível atualizar a chave de distribuição |

**Sugestões:**
* Começar a utilizar o Round Robin, mas aspire uma estratégia de distribuição de hash para tirar partido de uma arquitetura de paralelo em grande escala.
* Certifique-se de que as chaves de hash comuns têm o mesmo formato de dados.
* Não distribua no formato varchar.
* As tabelas de dimensões com uma chave de hash comuns a uma tabela de factos com operações de associação frequentes podem ser distribuída de hash.
* Utilize  *[sys.dm_pdw_nodes_db_partition_stats]*  para analisar quaisquer skewness nos dados.
* Utilize  *[sys.dm_pdw_request_steps]*  para analisar dados movimentos atrás de consultas, monitorizar o tempo de difusão e as operações de shuffle demorar. Esta ação é útil para rever a estratégia de distribuição.

Saiba mais sobre [tabelas replicadas] e [distribuídas tabelas].

## <a name="index-your-table"></a>A tabela de índice

A indexação é útil para a leitura de tabelas rapidamente. Há um conjunto exclusivo de tecnologias que pode utilizar com base nas necessidades:

| Tipo | Excelente opção para... | Tenha atenção se...|
|:--- |:--- |:--- |
| Área dinâmica para dados | Tabela de teste/temporário •<br></br>• Tabelas pequeno com pesquisas pequenas |• Qualquer pesquisa analisa a tabela completa |
| Índice em cluster | • Tabelas com até 100 milhões de linhas<br></br>• Tabelas grandes (mais de 100 milhões de linhas) com apenas 1-2 colunas de descontos elevados utilizadas |• Utilizado em tabelas replicadas<br></br>• Tem consultas complexas, envolvendo vários Group By operações de associação e<br></br>• Efetuar atualizações nas colunas indexadas: demora memória |
| Índice columnstore em cluster (CCI) (predefinição) | • Tabelas grandes (mais de 100 milhões de linhas) | • Utilizado em tabelas replicadas<br></br>• Efetuar enormes atualizar operações na sua tabela<br></br>• Overpartition a tabela: grupos de linhas não span em nós de distribuição diferente e partições |

**Sugestões:**
* Por cima de um índice em cluster, pode querer adicionar um índice não em cluster para uma coluna muito utilizada para filtragem. 
* Seja cuidadoso como gerir a memória numa tabela com CCI. Carregar dados, quer o utilizador (ou a consulta) para tirar partido de uma classe de recursos de grandes dimensões. Certifique-se evitar corte e criar vários grupos de pequena linha comprimido.
* Otimizado para a camada de computação rocks com CCI.
* Para CCI, um desempenho lento pode acontecer devido a compressão fraco dos seus grupos de linhas. Se isto ocorrer, reconstruir ou reorganizar o CCI. Pretende que, pelo menos, 100 000 linhas por grupos de linhas comprimido. O ideal é 1 milhões de linhas de um grupo de linha.
* Com base na frequência de carregamento incremental e tamanho, que pretende automatizar quando reorganize ou reconstruir os índices. Limpeza de mola sempre é útil.
* Ser estratégicos quando pretende compactar um grupo de linhas. Como grandes são os grupos de linhas aberto? Quantidade de dados espera que a carga nos próximos dias?

Saiba mais sobre [índices].

## <a name="partitioning"></a>Criação de partições
Pode particionar a tabela quando tem uma tabela de factos grande (maior do que 1 milhões de linhas). 99 por cento dos casos, a chave de partição deve basear-se numa data. Tenha o cuidado de não overpartition, especialmente quando tem um índice columnstore em cluster.

Com as tabelas que requerem ELT de teste, pode beneficiar de criação de partições. Facilita a gestão de ciclo de vida de dados.
Tenha cuidado não de overpartition os seus dados, especialmente num índice columnstore em cluster.

Saiba mais sobre [partições].

## <a name="incremental-load"></a>Carga incremental

Se incrementalmente carregar os dados, primeiro certifique-se de que atribua maior recursos classes para carregar os dados. Recomendamos que utilize o PolyBase e ADF V2 para automatizar os seus pipelines ELT para o SQL Data Warehouse.

Para um lote grande de atualizações nos seus dados históricos, elimine primeiro os dados pretendidos. Em seguida, marca uma inserção em massa de novos dados. Esta abordagem de dois passos é mais eficiente.

## <a name="maintain-statistics"></a>Manter as estatísticas
 Até automática estatísticas são geralmente disponíveis, o SQL Data Warehouse requer manutenção manual das estatísticas. É importante atualizar as estatísticas como *significativas* acontecer alterações aos seus dados. Isto ajuda a otimizar os planos de consulta. Se achar que demora demasiado tempo a manter todas as estatísticas, ser mais seletiva sobre as colunas tenham estatísticas. 

Também pode definir a frequência das atualizações. Por exemplo, pode querer atualizar colunas de data, onde novos valores podem ser adicionados, numa base diária. Obtém o maior parte dos benefício, fazendo com que as estatísticas em colunas envolvidas em associações, colunas utilizadas na cláusula WHERE e colunas encontradas no GROUP BY.

Saiba mais sobre [estatísticas].

## <a name="resource-class"></a>Classe de recursos
O SQL Data Warehouse utiliza grupos de recursos como uma forma de alocar memória para consultas. Se precisar de mais memória para melhorar a consulta ou carregar velocidade, deve alocar superiores classes de recursos. No lado flip, utilizar maior classes de recurso afeta concorrência. Pretende tirar que em consideração antes de mover todos os utilizadores a uma classe de recursos de grandes dimensões.

Se reparar em que as consultas demorar demasiado tempo, certifique-se de que os utilizadores não são executados em classes de recursos de grandes dimensões. Classes de recurso grande consumam muitos ranhuras de concorrência. Estes podem fazer com que outras consultas para a fila.

Por fim, utilizando a camada de computação otimizada, cada classe de recursos obtém 2,5 vezes mais memória que na camada elástico otimizada.

Saiba mais como trabalhar com [classes de recursos e simultaneidade].

## <a name="lower-your-cost"></a>O custo
Uma funcionalidade chave do armazém de dados do SQL Server é a capacidade de colocar em pausa quando não estiver a utilizar, o que deixa de faturação de recursos de computação. Outra funcionalidade importante é a capacidade de dimensionar os recursos. Colocar em pausa e dimensionar podem ser feitos através do portal do Azure ou através de comandos do PowerShell.

Dimensionamento automático agora no momento em que pretende com as funções do Azure:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

## <a name="optimize-your-architecture-for-performance"></a>Otimizar a arquitetura de desempenho

É recomendável considerar a base de dados SQL e do Azure Analysis Services numa arquitetura de concentrador hub-and-spoke. Esta solução pode proporcionar o isolamento de carga de trabalho entre grupos de utilizadores diferentes ao também utilizar funcionalidades avançadas de segurança da base de dados SQL e do Azure Analysis Services. Também se trata de uma forma de fornecer simultaneidade ilimitada aos seus utilizadores.

Saiba mais sobre [arquiteturas típicas que tirar partido do SQL Data Warehouse](https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/).

Implemente um clique a spokes realizar nas bases de dados do SQL Server do armazém de dados do SQL Server:

<a href="https://ms.portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwSpokeDbTemplate%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>


<!--Image references-->
[Sketch]:media/sql-data-warehouse-cheat-sheet/picture-flow.png

<!--Article references-->
[carregamento de dados]:./design-elt-data-loading.md
[deeper guidance]: ./guidance-for-loading-data.md
[índices]:./sql-data-warehouse-tables-index.md
[partições]:./sql-data-warehouse-tables-partition.md
[estatísticas]:./sql-data-warehouse-tables-statistics.md
[classes de recursos e simultaneidade]:./sql-data-warehouse-develop-concurrency.md

<!--MSDN references-->


<!--Other Web references-->
[typical architectures that take advantage of SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/common-isv-application-patterns-using-azure-sql-data-warehouse/
[is and is not]:https://blogs.msdn.microsoft.com/sqlcat/2017/09/05/azure-sql-data-warehouse-workload-patterns-and-anti-patterns/
[migração de dados]:https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
[tabelas replicadas]:https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-guidance-for-replicated-tables
[distribuídas tabelas]:https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute
[Azure Data Lake Store]: https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-store
[sys.dm_pdw_nodes_db_partition_stats]: https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql
[sys.dm_pdw_request_steps]:https://docs.microsoft.com/en-us/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql
