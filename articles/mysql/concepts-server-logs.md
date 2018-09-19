---
title: Registos do servidor da base de dados do Azure para MySQL
description: Descreve os registos disponíveis no banco de dados do Azure para MySQL e os parâmetros disponíveis para ativar os níveis de registo diferente.
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 09/17/2018
ms.openlocfilehash: ac5be20815b552c08e5cd1054bf24d7a10b56498
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46124274"
---
# <a name="server-logs-in-azure-database-for-mysql"></a>Registos do servidor na base de dados do Azure para MySQL
Na base de dados do Azure para MySQL, o log de consulta lenta está disponível para os utilizadores. Acesso ao registo de transação não é suportado. O log de consulta lenta pode ser utilizado para identificar afunilamentos de desempenho para resolução de problemas. 

Para obter mais informações sobre o registo de consulta lenta MySQL, consulte o manual de referência de MySQL [lento secção de registo de consulta](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html).

## <a name="access-server-logs"></a>Aceder a registos do servidor
Pode listar e transferir a base de dados do Azure para os registos do servidor MySQL com o portal do Azure e a CLI do Azure.

No portal do Azure, selecione a base de dados do Azure para o servidor MySQL. Sob o **monitorização** cabeçalho, selecione a **os registos do servidor** página.

Para obter mais informações sobre a CLI do Azure, consulte [acesso e configurar os registos do servidor com a CLI do Azure](howto-configure-server-logs-in-cli.md).

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

Consulte o MySQL [lento de documentação do log de consulta](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) para descrições completas dos parâmetros de registo de consulta lenta.

## <a name="next-steps"></a>Próximos Passos
- [Como configurar e aceder a registos do servidor a partir da CLI do Azure](howto-configure-server-logs-in-cli.md).
