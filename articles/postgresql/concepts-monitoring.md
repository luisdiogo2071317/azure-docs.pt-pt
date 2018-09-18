---
title: Monitorização na base de dados do Azure para PostgreSQL
description: Este artigo descreve as métricas de monitorização e alertas para base de dados do Azure para PostgreSQL, incluindo as estatísticas de CPU, armazenamento e conexão.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 09/17/2018
ms.openlocfilehash: 17b12514e32ad8d1548d834d72e0f7564fe78143
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985784"
---
# <a name="monitoring-in-azure-database-for-postgresql"></a>Monitorização na base de dados do Azure para PostgreSQL
Dados sobre os servidores de monitorização ajuda-o a resolver problemas e otimizar a sua carga de trabalho. Base de dados do Azure para PostgreSQL fornece várias métricas que lhe dar informações sobre o comportamento de recursos que suportam o servidor PostgreSQL. 

## <a name="metrics"></a>Métricas
Todas as métricas do Azure têm uma frequência de um minuto e cada métrica oferece 30 dias do histórico. Pode configurar alertas sobre as métricas. Para obter orientações passo a passo, consulte [como posso configurar alertas](howto-alert-on-metric.md). Outras tarefas incluem definir ações automatizadas, a realização de análises avançadas e o arquivamento de histórico. Para obter mais informações, consulte a [descrição geral das métricas do Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lista de métricas
Estas métricas estão disponíveis para a base de dados do Azure para PostgreSQL:

|Métrica|Nome a apresentar de métrica|Unidade|Descrição|
|---|---|---|---|---|
|cpu_percent|Percentagem de CPU|Percentagem|A percentagem de CPU em utilização.|
|memory_percent|Percentagem de memória|Percentagem|A percentagem de memória em utilização.|
|io_consumption_percent|Percentagem de e/s|Percentagem|A percentagem de e/s em utilização.|
|storage_percent|Percentagem de armazenamento|Percentagem|A percentagem de armazenamento utilizado fora do servidor 's máxima.|
|storage_used|Armazenamento utilizado|Bytes|A quantidade de armazenamento em utilização. O armazenamento utilizado pelo serviço pode incluir os ficheiros de base de dados, registos de transações e os registos do servidor.|
|storage_limit|Limite de armazenamento|Bytes|O armazenamento máximo para este servidor.|
|serverlog_storage_percent|Percentagem de armazenamento de registo do servidor|Percentagem|A percentagem de armazenamento de registo de servidor utilizado fora do armazenamento de registos de servidor máxima do servidor.|
|serverlog_storage_usage|Armazenamento de registo de servidor utilizado|Bytes|A quantidade de armazenamento de registo do servidor em utilização.|
|serverlog_storage_limit|Limite de armazenamento de registo do servidor|Bytes|O armazenamento de registo de servidor máxima para este servidor.|
|active_connections|Ligações ativas|Contagem|O número de ligações ativas para o servidor.|
|connections_failed|Ligações Falhadas|Contagem|O número de ligações falhadas para o servidor.|
|network_bytes_egress|Saída de Rede|Bytes|Rede horizontalmente através de ligações de Active Directory.|
|network_bytes_ingress|Entrada de Rede|Bytes|Rede no através de ligações de Active Directory.|


## <a name="next-steps"></a>Passos Seguintes
- Ver [como posso configurar alertas](howto-alert-on-metric.md) para obter orientações sobre como criar um alerta na métrica.
- Para obter mais informações sobre como aceder e exportar métricas com o portal do Azure, a REST API ou a CLI, consulte a [descrição geral das métricas do Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
