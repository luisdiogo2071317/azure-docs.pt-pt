---
title: Os limites de capacidade - Azure SQL Data Warehouse | Microsoft Docs
description: Valores máximos permitidos para vários componentes do Azure SQL Data Warehouse.
services: sql-data-warehouse
author: antvgski
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: anvang
ms.reviewer: igorstan
ms.openlocfilehash: 233cba790aff60121bd1fdf43667ac48a5ef668d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34644941"
---
# <a name="sql-data-warehouse-capacity-limits"></a>Limites de capacidade do SQL Data Warehouse
Valores máximos permitidos para vários componentes do Azure SQL Data Warehouse.

## <a name="workload-management"></a>Gestão de cargas de trabalhos
| Categoria | Descrição | Máximo |
|:--- |:--- |:--- |
| [Unidades do Data Warehouse (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |DWU máx. para um SQL Data Warehouse único | Gen1: DW6000<br></br>Gen2: DW30000c |
| [Unidades do Data Warehouse (DWU)](what-is-a-data-warehouse-unit-dwu-cdwu.md) |Predefinição DTU por servidor |54,000<br></br>Por predefinição, cada SQL server (por exemplo, myserver.database.windows.net) tem uma Quota de DTU de 54,000, que permite até DW6000c. Esta quota é apenas um limite de segurança. Pode aumentar a quota por [criar um pedido de suporte](sql-data-warehouse-get-started-create-support-ticket.md) e selecionando *Quota* como o tipo de pedido.  Para calcular a DTU necessita de, multiplique a 7.5 pelo total que DWU necessários, ou multiplicar 9.0 pelo cDWU total necessário. Por exemplo:<br></br>DW6000 x 7.5 = 45.000 DTUs<br></br>DW600c x 9.0 = 54,000 DTUs.<br></br>Pode ver o consumo de DTU atual a partir da opção de servidor do SQL Server no portal. Tanto as bases de dados em pausa como as que não estão em pausa contam para a quota de DTU. |
| Ligação à base de dados |Sessões abertas em simultâneo |1024<br/><br/>Cada um das sessões ativas 1024 pode submeter pedidos para uma base de dados do armazém de dados do SQL Server ao mesmo tempo. Tenha em atenção de que existem limites sobre o número de consultas que podem ser executados em simultâneo. Quando o limite de concorrência for excedido, o pedido entra uma fila interna onde deve aguardar para ser processado. |
| Ligação à base de dados |Memória máxima instruções preparado |20 MB |
| [Gestão de cargas de trabalho](resource-classes-for-workload-management.md) |Consultas em simultâneo máximas |32<br/><br/> Por predefinição, o SQL Data Warehouse pode ser executado um máximo de 32 consultas em simultâneo e filas restantes consultas.<br/><br/>O número de consultas em simultâneo pode diminuir quando os utilizadores são atribuídos aos superiores classes de recursos ou quando o SQL Data Warehouse tem um inferior [unidade do armazém de dados](memory-and-concurrency-limits.md) definição. Algumas consultas, como consultas DMV, sempre estão autorizadas a executar. |
| [tempdb](sql-data-warehouse-tables-temporary.md) |Máximo GB |399 GB por DW100. Por conseguinte, DWU1000, tempdb é dimensionados de forma a 3.99 TB. |

## <a name="database-objects"></a>Objetos de base de dados
| Categoria | Descrição | Máximo |
|:--- |:--- |:--- |
| Base de Dados |Tamanho máximo | Gen1: 240 TB comprimido no disco. Este espaço é independente de espaço em tempdb ou de registo e, por conseguinte, este espaço dedicado para tabelas permanentes.  Compressão columnstore em cluster estima-se em 5 X.  Esta compressão permite que a base de dados aumente aproximadamente 1 PB quando todas as tabelas columnstore em cluster (o tipo de tabela predefinido). <br/><br/> Gen2: 240TB para rowstore e armazenamento ilimitado para tabelas columnstore |
| Tabela |Tamanho máximo |60 TB comprimido no disco |
| Tabela |Tabelas por base de dados |10,000 |
| Tabela |Colunas por tabela |1024 colunas |
| Tabela |Bytes por coluna |Depende de coluna [tipo de dados](sql-data-warehouse-tables-data-types.md). Limite é 8000 para tipos de dados char, 4000 para nvarchar, ou 2 GB para os tipos de dados máxima. |
| Tabela |Bytes por linha, tamanho definido |8060 bytes<br/><br/>O número de bytes por linha é calculado da mesma forma como está para o SQL Server com compressão de página. Como o SQL Server, do armazém de dados do SQL Server suporta o armazenamento de capacidade excedida de linha, que permite **colunas de comprimento variável** para fazer o Push consecutivos. Quando as linhas de comprimento variável são enviadas por push consecutivos, apenas 24 bytes raiz é armazenado no registo principal. Para obter mais informações, consulte [exceder de dados de capacidade excedida de linha 8-KB](https://msdn.microsoft.com/library/ms186981.aspx). |
| Tabela |Partições por tabela |15,000<br/><br/>Para elevado desempenho, recomendamos a minimizar o número de partições necessário enquanto ainda suportar os requisitos de negócio. À medida que cresce o número de partições, a sobrecarga para operações de linguagem de definição de dados (DDL) e de idioma de manipulação de dados (DML) que vai crescendo e faz com que um desempenho mais lento. |
| Tabela |Carateres por valor de limite de partição. |4000 |
| Índice |Índices não em cluster por tabela. |50<br/><br/>Aplica-se apenas a tabelas rowstore. |
| Índice |Índices em cluster por tabela. |1<br><br/>Aplica-se a tabelas rowstore e columnstore. |
| Índice |Tamanho de chave do índice. |900 bytes.<br/><br/>Aplica-se apenas a índices rowstore.<br/><br/>Os índices em colunas de varchar com um tamanho máximo de 900 bytes, mais do que podem ser criados se os dados existentes nas colunas não pode exceder 900 bytes quando o índice é criado. No entanto, inserir mais tarde ou ações de ATUALIZAÇÃO em colunas que fazer com que o tamanho total exceder os 900 bytes irão falhar. |
| Índice |Colunas de chave por índice. |16<br/><br/>Aplica-se apenas a índices rowstore. Índices columnstore em cluster incluem todas as colunas. |
| Estatísticas |Tamanho dos valores da coluna combinada. |900 bytes. |
| Estatísticas |Colunas por objeto de estatísticas. |32 |
| Estatísticas |Estatísticas criadas em colunas por tabela. |30,000 |
| Procedimentos Armazenados |Níveis de máximos de aninhamento. |8 |
| Vista |Colunas por vista |1,024 |

## <a name="loads"></a>Carrega
| Categoria | Descrição | Máximo |
|:--- |:--- |:--- |
| O Polybase cargas |MB por linha |1<br/><br/>O Polybase carrega apenas para linhas que são mais pequeno do que 1 MB e não é possível carregar varchar (Max), nvarchar (Max) ou varbinary (Max).<br/><br/> |

## <a name="queries"></a>Consultas
| Categoria | Descrição | Máximo |
|:--- |:--- |:--- |
| Consulta |Consultas em fila em tabelas de utilizador. |1000 |
| Consulta |Consultas em simultâneo em vistas de sistema. |100 |
| Consulta |Consultas em fila em vistas de sistema |1000 |
| Consulta |Parâmetros máximos |2098 |
| Batch |Tamanho máximo |65,536*4096 |
| SELECIONADOS resultados |Colunas por linha |4096<br/><br/>Nunca pode ter mais de 4096 colunas por linha no resultado de SELECT. Não há nenhuma garantia que pode ter sempre 4096. Se o plano de consulta necessita de uma tabela temporária, podem ser aplicadas as colunas de 1024 por tabela máxima. |
| SELECIONAR |Subconsultas aninhadas |32<br/><br/>Nunca pode ter mais do que 32 subconsultas aninhadas numa instrução SELECT. Não há nenhuma garantia que possam ter sempre a 32. Por exemplo, uma associação pode introduzir uma subconsulta no plano de consulta. O número de subconsultas também pode ser limitado pela memória disponível. |
| SELECIONAR |Colunas por associação |1024 colunas<br/><br/>Nunca pode ter mais de 1024 colunas na União. Não há nenhuma garantia que possam ter sempre a 1024. Se o plano de associação necessita de uma tabela temporária com mais colunas do que o resultado da JUNÇÃO, o limite de 1024 aplica-se a tabela temporária. |
| SELECIONAR |Total de bytes por grupo por colunas. |8060<br/><br/>As colunas na cláusula GROUP BY podem ter um máximo de 8060 bytes. |
| SELECIONAR |Bytes por colunas ORDER BY |8060 bytes<br/><br/>As colunas na cláusula ORDER BY podem ter um máximo de 8060 bytes |
| Identificadores por declaração |Número de identificadores referenciados |65,535<br/><br/>O SQL Data Warehouse limita o número de identificadores que podem ser incluídos sob uma única expressão de uma consulta. A exceder este número resulta num erro do SQL Server 8632. Para obter mais informações, consulte [Erro interno: foi atingido um limite de serviços de expressão] [Erro interno: foi atingido um limite de serviços de expressão]. |
| Literais de cadeia | Número de cadeias literais numa instrução | 20,000 <br/><br/>O SQL Data Warehouse limita o número de constantes de cadeia numa expressão única de uma consulta. A exceder este número resulta num erro do SQL Server 8632.|

## <a name="metadata"></a>Metadados
| Vista de sistema | Linhas máximas |
|:--- |:--- |
| sys.dm_pdw_component_health_alerts |10,000 |
| sys.dm_pdw_dms_cores |100 |
| sys.dm_pdw_dms_workers |Número total de trabalhadores DMS para as mais recentes 1000 pedidos de SQL. |
| sys.dm_pdw_errors |10,000 |
| sys.dm_pdw_exec_requests |10,000 |
| sys.dm_pdw_exec_sessions |10,000 |
| sys.dm_pdw_request_steps |Número total de passos para os pedidos SQL mais recentes de 1000 que estão armazenados no sys.dm_pdw_exec_requests. |
| sys.dm_pdw_os_event_logs |10,000 |
| sys.dm_pdw_sql_requests |O SQL mais recente de 1000 pedidos que são armazenados no sys.dm_pdw_exec_requests. |

## <a name="next-steps"></a>Passos Seguintes
Para ver as recomendações sobre como utilizar o SQL Data Warehouse, consulte o [Cheat folha](cheat-sheet.md).
