---
title: Registos do servidor da base de dados do Azure para MariaDB
description: Descreve os registos disponíveis no banco de dados do Azure para MariaDB e os parâmetros disponíveis para ativar os níveis de registo diferente.
author: rachel-msft
ms.author: raagyema
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: d982af64517ed452d907f62a39e975d472951392
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992988"
---
# <a name="server-logs-in-azure-database-for-mariadb"></a>Registos do servidor na base de dados do Azure para MariaDB
Na base de dados do Azure para MariaDB, o log de consulta lenta está disponível para os utilizadores. Acesso ao registo de transação não é suportado. O log de consulta lenta pode ser utilizado para identificar afunilamentos de desempenho para resolução de problemas.

Para obter mais informações sobre o registo de consulta lenta, consulte a documentação de MariaDB para [registo de consulta lenta](https://mariadb.com/kb/en/library/slow-query-log-overview/).

## <a name="access-server-logs"></a>Aceder a registos do servidor
Pode listar e transferir a base de dados do Azure para MariaDB registos do servidor com o portal do Azure e a CLI do Azure.

No portal do Azure, selecione a base de dados do Azure para MariaDB server. Sob o **monitorização** cabeçalho, selecione a **os registos do servidor** página.

<!-- For more information on Azure CLI, see [Configure and access server logs using Azure CLI](howto-configure-server-logs-in-cli.md).-->

## <a name="log-retention"></a>Retenção do registo
Os registos estão disponíveis até sete dias desde a sua criação. Se o tamanho total dos registos disponíveis exceder 7 GB, em seguida, os mais antigos ficheiros são eliminados até que o espaço está disponível.

Os registos são revezados cada 24 horas ou 7 GB, o que ocorrer primeiro.

## <a name="configure-logging"></a>Configurar o registo
Por predefinição, o registo de consulta lenta está desativado. Para ativá-lo, defina slow_query_log on.

Outros parâmetros que pode ajustar incluem:

- **long_query_time**: se uma consulta demora mais tempo do que long_query_time (em segundos) dessa consulta é registada. A predefinição é 10 segundos.
- **log_slow_admin_statements**: se no inclui instruções administrativas como ALTER_TABLE e ANALYZE_TABLE nas instruções escritas para o slow_query_log.
- **log_queries_not_using_indexes**: determina se as consultas que não utilizam os índices são registadas a slow_query_log
- **log_throttle_queries_not_using_indexes**: este parâmetro limita o número de consultas não índice que podem ser gravados no log de consulta lenta. Este parâmetro entra em vigor quando log_queries_not_using_indexes está definido como ligado.

Consulte a MariaDB [lento de documentação do log de consulta](https://mariadb.com/kb/en/library/slow-query-log-overview/) para descrições completas dos parâmetros de registo de consulta lenta.

## <a name="next-steps"></a>Próximos Passos
- [Como configurar e aceder aos registos de servidor do portal do Azure](howto-configure-server-logs-portal.md).
