---
title: Otimizar a recolha de estatísticas de consulta na base de dados do Azure para a recolha de estatísticas de consulta de servidor PostgreSQL
description: Este artigo descreve como pode otimizar a recolha de estatísticas de consulta numa base de dados do Azure para o servidor PostgreSQL.
author: dianaputnam
ms.author: dianas
editor: jasonwhowell
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/25/2018
ms.openlocfilehash: 076442d85d7f628504cca95c36f3e99f4d0c5117
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2018
ms.locfileid: "52966692"
---
# <a name="optimize-query-statistics-collection-in-azure-database-for-postgresql-server"></a>Otimizar a recolha de estatísticas de consulta na base de dados do Azure para o servidor PostgreSQL 
Este artigo descreve para otimizar a recolha de estatísticas de consulta numa base de dados do Azure para o servidor PostgreSQL.

## <a name="using-pgstatsstatements"></a>Usando pg_stats_statements
**Pg_stat_statements** é uma extensão de PostgreSQL que está ativada por predefinição na base de dados do Azure para PostgreSQL. A extensão fornece um meio para controlar as estatísticas de execução de todas as instruções SQL executadas por um servidor. No entanto, esse módulo conecta-se em cada execução de consulta e vem com um custo de desempenho não trivial. Habilitando **pg_stat_statements** forças consultar texto escritas arquivos no disco.

Para os clientes que tenham consultas exclusivas com texto de consulta longo ou não estão a monitorizar ativamente **pg_stat_statements**, recomendamos desativar **pg_stat_statements** para um melhor desempenho, definindo `pg_stat_statements.track = NONE`.

Em algumas cargas de trabalho do cliente que vimos até uma melhoria de desempenho de 50 por cento, desativando **pg_stat_statements**. No entanto, a compensação um torna desabilitando pg_stat_statements é a incapacidade de resolver problemas de desempenho.

Para definir `pg_stat_statements.track = NONE`:

- No portal do Azure, navegue para o [gestão de recursos do PostgreSQL página e selecione o painel de parâmetros de servidor](howto-configure-server-parameters-using-portal.md).

![Painel de parâmetro de servidor PostgreSQL](./media/howto-optimize-query-stats-collection/pg_stats_statements_portal.png)

- Usando [CLI do Azure](howto-configure-server-parameters-using-cli.md), az postgres server configuration set `--name pg_stat_statements.track --resource-group myresourcegroup --server mydemoserver --value NONE`.

## <a name="using-query-store"></a>Usando o Query Store 
O [Query Store](concepts-query-store.md) funcionalidade na base de dados do Azure para PostgreSQL fornece um método de alto desempenho mais para controlar as estatísticas de consulta e é recomendado como uma alternativa à utilização *pg_stats_statements*. 

## <a name="next-steps"></a>Passos Seguintes
Considere definir `pg_stat_statements.track = NONE` no [Portal do Azure](howto-configure-server-parameters-using-portal.md) ou ao utilizar [CLI do Azure](howto-configure-server-parameters-using-cli.md).

Consulte a [cenários de utilização de consulta Store](concepts-query-store-scenarios.md) e [Query Store melhores práticas](concepts-query-store-best-practices.md) para obter mais informações. 
