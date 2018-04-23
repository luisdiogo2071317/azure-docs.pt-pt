---
title: Como utilizar sys_schema para otimização de desempenho e manutenção de base de dados na base de dados do Azure para MySQL
description: Este artigo descreve como utilizar sys_schema para encontrar problemas de desempenho e manter a base de dados na base de dados do Azure para MySQL.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 2e5b6b859df06d686a97fc1b134da8d66df6783e
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="how-to-use-sysschema-for-performance-tuning-and-database-maintenance-in-azure-database-for-mysql"></a>Como utilizar sys_schema para manutenção de base de dados e otimização de desempenho na base de dados do Azure para MySQL

O MySQL performance_schema, o primeiro disponível no MySQL 5.5, fornece instrumentation para vários recursos de servidor vitais, como a atribuição de memória, programas armazenados, metadados de bloqueio, etc. No entanto, o performance_schema contém mais de 80 tabelas e obter as informações necessárias, muitas vezes, requer a associação a tabelas dentro de performance_schema, bem como tabelas do information_schema. Tendo performance_schema e information_schema, o sys_schema fornece uma coleção de elevado desempenho [vistas intuitivo](https://dev.mysql.com/doc/refman/5.7/en/sys-schema-views.html) numa base de dados só de leitura e totalmente está ativado na base de dados do Azure para MySQL versão 5.7.

![vistas de sys_schema](./media/howto-troubleshoot-sys-schema/sys-schema-views.png)

Existem 52 vistas na sys_schema e cada vista tem um dos seguintes prefixos:

- Host_summary ou e/s: latências relacionadas com a e/s.
- InnoDB: Estado da memória intermédia InnoDB e bloqueios.
- Memória: Utilização de memória o anfitrião e os utilizadores.
- : Relacionados com o esquema informações de esquema, Tal como de incremento automático, índices, etc.
- Declaração: Informações nas instruções SQL; pode ser instrução que resultaram numa análise da tabela completa ou desde a hora de consulta.
- Utilizador: Recursos consumidos e agrupados por utilizadores. Os exemplos são ficheiro e/s, ligações e memória.
- Espera: Eventos de espera agrupados pelo anfitrião ou o utilizador.

Agora vamos ver alguns padrões de utilização comuns do sys_schema. Lugar com, iremos irá agrupar os padrões de utilização em duas categorias: **otimização de desempenho** e **manutenção de base de dados**.

## <a name="performance-tuning"></a>Otimização do desempenho

### <a name="sysusersummarybyfileio"></a>*sys.user_summary_by_file_io*

E/s é a mais dispendiosa operação na base de dados. Iremos pode descobrir a latência média de e/s consultando o *sys.user_summary_by_file_io* vista. Com a predefinição que é de 125 GB de armazenamento com aprovisionamento, os meus latência de e/s cerca de 15 segundos.

![Latência de e/s: 125 GB](./media/howto-troubleshoot-sys-schema/io-latency-125GB.png)

Porque a base de dados do Azure para MySQL dimensiona e/s no que respeita à armazenamento, após aumentar o meu storage aprovisionado para 1 TB, reduz os meus latência de e/s para 571 ms, que representa um aumento de desempenho X 26!

![Latência de e/s: 1TB](./media/howto-troubleshoot-sys-schema/io-latency-1TB.png)

### <a name="sysschematableswithfulltablescans"></a>*sys.schema_tables_with_full_table_scans*

Apesar de um planeamento cuidadoso, demasiadas consultas ainda podem resultar num análises de tabela completa. Para obter informações adicionais sobre os tipos de índices e de como otimizá-los, pode fazer referência a este artigo: [como resolver problemas de desempenho de consulta](./howto-troubleshoot-query-performance.md). Análises de tabela completa são intensivas em recursos e degradar o desempenho de base de dados. A forma mais rápida para localizar a tabelas com a análise da tabela completa é consulta o *sys.schema_tables_with_full_table_scans* vista.

![análises de tabela completa](./media/howto-troubleshoot-sys-schema/full-table-scans.png)

### <a name="sysusersummarybystatementtype"></a>*sys.user_summary_by_statement_type*

Para resolver problemas de desempenho de base de dados, pode ser vantajoso identificar os eventos a acontecer no interior da base de dados e utilizar o *sys.user_summary_by_statement_type* vista só pode fazer o truque.

![Resumo pela instrução](./media/howto-troubleshoot-sys-schema/summary-by-statement.png)

Neste exemplo, base de dados do Azure para MySQL despendido minutos 53 limpar o registo de consultas slog 44579 vezes. É muito tempo e muitos IOs. Pode reduzir esta atividade, desative o registo de consultas lenta ou diminuir a frequência de início de sessão de consulta lenta portal do Azure.

## <a name="database-maintenance"></a>Manutenção da base de dados

### <a name="sysinnodbbufferstatsbytable"></a>*sys.innodb_buffer_stats_by_table*

O conjunto de memória intermédia InnoDB reside na memória e é o mecanismo de cache principal entre o DBMS e armazenamento. O tamanho do conjunto de memória intermédia InnoDB está associado para a camada de desempenho e não pode ser alterado a menos que outro produto SKU é escolhido. Tal como acontece com a memória do sistema operativo, as páginas antigas são trocadas enviados para disponibilizar espaço para dados fresher. Para determinar que tabelas consumam mais de memória de conjunto de memória intermédia InnoDB, pode consultar o *sys.innodb_buffer_stats_by_table* vista.

![Estado da memória intermédia de InnoDB](./media/howto-troubleshoot-sys-schema/innodb-buffer-status.png)

O gráfico acima, é aparente que diferente de tabelas de sistema e vistas, cada tabela na base de dados mysqldatabase033, que aloje um dos meus sites WordPress, ocupar 1 página, dos dados na memória ou 16 KB.

### <a name="sysschemaunusedindexes--sysschemaredundantindexes"></a>*Sys.schema_unused_indexes* & *sys.schema_redundant_indexes*

Os índices são excelente ferramentas para melhorar o desempenho de leitura, mas estes implicar custos adicionais de inserções e de armazenamento. *Sys.schema_unused_indexes* e *sys.schema_redundant_indexes* fornecem informações para índices utilizadas ou duplicados.

![os índices](./media/howto-troubleshoot-sys-schema/unused-indexes.png)

![índices redundantes](./media/howto-troubleshoot-sys-schema/redundant-indexes.png)

## <a name="conclusion"></a>Conclusão

Em resumo, a sys_schema é uma ótima ferramenta para ambos os manutenção de base de dados e a otimização do desempenho. Certifique-se tirar partido desta funcionalidade na sua base de dados do Azure para MySQL. 

## <a name="next-steps"></a>Passos Seguintes
- Para encontrar respostas de ponto a ponto para as perguntas mais pretendidas ou após uma nova pergunta/resposta, visite [fórum MSDN](https://social.msdn.microsoft.com/forums/security/en-US/home?forum=AzureDatabaseforMySQL) ou [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
