---
title: "Monitorização na base de dados do Azure para PostgreSQL"
description: "Este artigo descreve as métricas de monitorização e alertas da base de dados do Azure para PostgreSQL, incluindo as estatísticas de CPU, de armazenamento e de ligação."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: d0a57fe6d7b1040c32f6d67e2bf0259176c72099
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/28/2018
---
# <a name="monitoring-in-azure-database-for-postgresql"></a>Monitorização na base de dados do Azure para PostgreSQL
Dados sobre os servidores de monitorização ajuda-o a resolver problemas e otimizar a sua carga de trabalho. Base de dados do Azure para PostgreSQL fornece várias métricas que permitem determinar o comportamento de recursos que suportam o servidor de PostgreSQL. 

## <a name="metrics"></a>Métricas
Todas as métricas do Azure tem uma frequência de um minuto e cada métrica fornece 30 dias do histórico. Pode configurar alertas nas métricas. Para orientações passo a passo, consulte [como configurar alertas](howto-alert-on-metric.md). Outras tarefas incluem como configurar as ações automatizadas, efetuar análises avançadas e arquivar histórico. Para obter mais informações, consulte o [descrição geral do Azure métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lista de métricas
Estas métricas estão disponíveis para a base de dados do Azure para PostgreSQL:

|Métrica|Nome a apresentar métrica|Unidade|Descrição|
|---|---|---|---|---|
|cpu_percent|Percentagem de CPU|Percentagem|A percentagem de CPU em utilização.|
|memory_percent|Percentagem de memória|Percentagem|A percentagem de memória em utilização.|
|io_consumption_percent|Percentagem de e/s|Percentagem|A percentagem de e/s em utilização.|
|storage_percent|Percentagem de armazenamento|Percentagem|A percentagem de armazenamento utilizado fora do servidor 's máxima.|
|storage_used|Armazenamento utilizado|Bytes|A quantidade de armazenamento em utilização. O armazenamento utilizado pelo serviço inclui os ficheiros de base de dados, os registos de transações e os registos do servidor.|
|storage_limit|Limite de armazenamento|Bytes|Armazenamento máximo para este servidor.|
|active_connections|Totais ligações ativas|Contagem|O número de ligações ativas para o servidor.|
|connections_failed|Totais de ligações com falhas|Contagem|O número de ligações com falhas ao servidor.|


## <a name="next-steps"></a>Passos Seguintes
- Consulte [como configurar alertas](howto-alert-on-metric.md) para obter orientações sobre a criação de um alerta numa métrica.
- Para obter mais informações sobre como aceder e exportar com o portal do Azure, a REST API ou a CLI de métricas, consulte o [descrição geral do Azure métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
