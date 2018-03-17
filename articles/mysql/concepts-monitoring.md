---
title: "Monitorização na base de dados do Azure para MySQL"
description: "Este artigo descreve as métricas de monitorização e alertas da base de dados do Azure para MySQL, incluindo as estatísticas de CPU, de armazenamento e de ligação."
services: mysql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 03/15/2018
ms.openlocfilehash: c3cba00077fd65239382d6fdd98e73a55f926b3b
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="monitoring-in-azure-database-for-mysql"></a>Monitorização na base de dados do Azure para MySQL
Dados sobre os servidores de monitorização ajuda-o a resolver problemas e otimizar a sua carga de trabalho. Base de dados do Azure para MySQL fornece várias métricas que permitem determinar o comportamento de recursos que suportam o servidor de MySQL. 

## <a name="metrics"></a>Métricas
Todas as métricas do Azure tem uma frequência de um minuto e cada métrica fornece 30 dias do histórico. Pode configurar alertas nas métricas. Para orientações passo a passo, consulte [como configurar alertas](howto-alert-on-metric.md). Outras tarefas incluem como configurar as ações automatizadas, efetuar análises avançadas e arquivar histórico. Para obter mais informações, consulte o [descrição geral do Azure métricas](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lista de métricas
Estas métricas estão disponíveis para a base de dados do Azure para o MySQL:

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
