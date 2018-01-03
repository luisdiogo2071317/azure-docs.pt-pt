---
title: "Métricas de Monitor do Azure - métricas suportadas por tipo de recurso | Microsoft Docs"
description: "Lista de haver métricas disponíveis para cada tipo de recurso com a monitorização do Azure."
author: anirudhcavale
manager: ashwink
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: ancav
ms.openlocfilehash: 673f5a5cd6832adb031ef72ce25f8a1622717cfd
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/20/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Métricas suportadas com a monitorização do Azure
Monitor do Azure fornece várias formas para interagir com métricas, incluindo charting-las no portal, aceder às mesmas através da API REST ou consultá-los utilizando o PowerShell ou a CLI. Segue-se uma lista completa de todas as métricas atualmente disponíveis no pipeline de métrico do Monitor do Azure.

> [!NOTE]
> Outras métricas poderão estar disponíveis no portal ou com APIs de legado. Esta lista inclui apenas as métricas disponíveis através do pipeline de métrico de Monitor do Azure consolidado. Para consultar e aceder métricas com dimensões utilize o [api-version de 2017-05-01-preview](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|qpu_metric|QPU|Contagem|Média|QPU. Intervalo de 0-100 para S1, 0-200 para S2 e 0-400 para S4|ServerResourceType|
|memory_metric|Memória|Bytes|Média|Memória. No intervalo 0-25 GB para S1, 0 a 50 GB para S2 e 0-100 GB para S4|ServerResourceType|
|TotalConnectionRequests|Pedidos de ligação total|Contagem|Média|Pedidos de ligação total. Estes são arrivals.|ServerResourceType|
|SuccessfullConnectionsPerSec|Ligações com êxito por seg|CountPerSecond|Média|Taxa de conclusões de ligação com êxito.|ServerResourceType|
|TotalConnectionFailures|Falhas de ligação total|Contagem|Média|Total de tentativas de ligação falhadas.|ServerResourceType|
|CurrentUserSessions|Sessões de utilizador atual|Contagem|Média|Número atual de sessões de utilizador estabelecida.|ServerResourceType|
|QueryPoolBusyThreads|Threads de ocupado de conjunto de consulta|Contagem|Média|Número de threads ocupados no conjunto de threads de consulta.|ServerResourceType|
|CommandPoolJobQueueLength|Comprimento de fila de tarefa de conjunto de comandos|Contagem|Média|Número de tarefas na fila do conjunto de threads de comando.|ServerResourceType|
|ProcessingPoolJobQueueLength|Comprimento de fila de tarefa de conjunto de processamento|Contagem|Média|Número de tarefas não-I/O na fila do conjunto de threads de processamento.|ServerResourceType|
|CurrentConnections|Ligação: Ligações atuais|Contagem|Média|Número atual de ligações de cliente estabelecido.|ServerResourceType|
|CleanerCurrentPrice|Memória: Preço atual de limpeza|Contagem|Média|Preços atual de memória, $/ byte/hora, normalizado a 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Memória: Memória de limpeza que possam ser encolhida|Bytes|Média|Quantidade de memória, em bytes, sujeita a remoção de limpeza por segundo plano.|ServerResourceType|
|CleanerMemoryNonshrinkable|Memória: Memória nonshrinkable de limpeza|Bytes|Média|Quantidade de memória, em bytes, não sujeitos a remoção de limpeza por segundo plano.|ServerResourceType|
|MemoryUsage|Memória: Utilização de memória|Bytes|Média|Utilização de memória do processo de servidor, conforme utilizado para calcular preço de memória de limpeza. Igual a Process\PrivateBytes mais o tamanho dos dados de mapeamento de memória, ignorando quaisquer memória que foi mapeada ou atribuída pelo motor de análise em memória xVelocity (VertiPaq) que excedam o limite de memória do motor de xVelocity do contador.|ServerResourceType|
|MemoryLimitHard|Memória: Disco rígido de limite de memória|Bytes|Média|Limite de memória de disco rígido, ficheiro de configuração.|ServerResourceType|
|MemoryLimitHigh|Memória: Limite de memória elevada|Bytes|Média|Limite de memória elevada, ficheiro de configuração.|ServerResourceType|
|MemoryLimitLow|Memória: Baixa de limite de memória|Bytes|Média|Limite de memória insuficiente, ficheiro de configuração.|ServerResourceType|
|MemoryLimitVertiPaq|Memória: VertiPaq de limite de memória|Bytes|Média|Limite de memória, ficheiro de configuração.|ServerResourceType|
|Quota|Memória: Quota|Bytes|Média|Quota atual de memória, em bytes. Quota de memória também é conhecido como uma reserva de memória ou de concessão de memória.|ServerResourceType|
|QuotaBlocked|Memória: Quota bloqueado|Contagem|Média|Número atual de pedidos de quota são bloqueada até que outros quotas de memória são libertadas.|ServerResourceType|
|VertiPaqNonpaged|Memória: VertiPaq na memória não paginável|Bytes|Média|Bytes de memória bloqueadas no conjunto de trabalho para utilização pelo motor dentro da memória.|ServerResourceType|
|VertiPaqPaged|Memória: VertiPaq bloco paginado|Bytes|Média|Bytes de memória paginada em utilização para dados em memória.|ServerResourceType|
|RowsReadPerSec|Processamento: Linhas lidas por seg|CountPerSecond|Média|Taxa de linhas lida todas as bases de dados relacionais.|ServerResourceType|
|RowsConvertedPerSec|Processamento: Converter o linhas por seg|CountPerSecond|Média|Taxa de linhas converter durante o processamento.|ServerResourceType|
|RowsWrittenPerSec|Processamento: Linhas escritas por seg|CountPerSecond|Média|Taxa de linhas escritos durante o processamento.|ServerResourceType|
|CommandPoolBusyThreads|Threads: Threads ocupado do conjunto de comandos|Contagem|Média|Número de threads ocupados no conjunto de threads de comando.|ServerResourceType|
|CommandPoolIdleThreads|Threads: Threads de inatividade do conjunto de comandos|Contagem|Média|Número de threads Inativos no conjunto de threads de comando.|ServerResourceType|
|LongParsingBusyThreads|Threads: Análise de threads ocupados de longa|Contagem|Média|Número de threads ocupados no conjunto de threads longa análise.|ServerResourceType|
|LongParsingIdleThreads|Threads: Longa análise threads Inativos|Contagem|Média|Número de threads Inativos no conjunto de threads longa análise.|ServerResourceType|
|LongParsingJobQueueLength|Threads: Análise longa comprimento da fila de tarefas|Contagem|Média|Número de tarefas na fila do conjunto de threads longa análise.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Threads: Threads de trabalho de e/s ocupados do conjunto de processamento|Contagem|Média|Número de threads em execução tarefas de e/s no conjunto de threads de processamento.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Threads: Threads de não-I/O ocupado do conjunto de processamento|Contagem|Média|Número de threads em execução de tarefas não-I/O no conjunto de threads de processamento.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Threads: Conjunto de comprimento de fila de trabalho de e/s de processamento|Contagem|Média|Número de tarefas de e/s na fila do conjunto de threads de processamento.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Threads: Conjunto Inativos threads de trabalho de e/s de processamento|Contagem|Média|Número de threads de inatividade para as tarefas de e/s no conjunto de threads de processamento.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Threads: Threads de não-I/O inativo do conjunto de processamento|Contagem|Média|Número de threads Inativos no conjunto de threads de processamento dedicado a tarefas não-I/O.|ServerResourceType|
|QueryPoolIdleThreads|Threads: Threads de inatividade do conjunto de consulta|Contagem|Média|Número de threads de inatividade para as tarefas de e/s no conjunto de threads de processamento.|ServerResourceType|
|QueryPoolJobQueueLength|Threads: Lengt de fila de tarefa de conjunto de consulta|Contagem|Média|Número de tarefas na fila do conjunto de threads de consulta.|ServerResourceType|
|ShortParsingBusyThreads|Threads: Curto período análise threads ocupados|Contagem|Média|Número de threads ocupados no conjunto de threads de análise curto.|ServerResourceType|
|ShortParsingIdleThreads|Threads: Curto período análise threads Inativos|Contagem|Média|Número de threads Inativos no conjunto de threads de análise curto.|ServerResourceType|
|ShortParsingJobQueueLength|Threads: Curto período comprimento de fila de tarefa de análise|Contagem|Média|Número de tarefas na fila do conjunto de threads de análise curto.|ServerResourceType|
|memory_thrashing_metric|Memória Thrashing|Percentagem|Média|Thrashing média da memória.|ServerResourceType|
|mashup_engine_qpu_metric|Motor de M QPU|Contagem|Média|Utilização QPU por processos de motor da aplicação híbrida|ServerResourceType|
|mashup_engine_memory_metric|M motor memória|Bytes|Média|Utilização de memória por processos de motor da aplicação híbrida|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|TotalRequests|Gateway de total de pedidos|Contagem|Total|Número de pedidos de gateway|Localização, nome de anfitrião|
|SuccessfulRequests|Pedidos de Gateway com êxito|Contagem|Total|Número de pedidos de gateway com êxito|Localização, nome de anfitrião|
|UnauthorizedRequests|Pedidos de Gateway não autorizado|Contagem|Total|Número de pedidos de gateway não autorizado|Localização, nome de anfitrião|
|FailedRequests|Pedidos de Gateway com falhas|Contagem|Total|Número de falhas nos pedidos de gateway|Localização, nome de anfitrião|
|OtherRequests|Outros pedidos de Gateway|Contagem|Total|Número de outros pedidos de gateway|Localização, nome de anfitrião|
|Duração|Duração global de pedidos de Gateway|Milissegundos|Média|Geral duração de pedidos Gateway em milissegundos|Localização, nome de anfitrião|
|Capacidade|Capacidade (pré-visualização)|Percentagem|Máximo|Métrica de utilização para o serviço de ApiManagement|Localização|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|TotalJob|Total de tarefas|Contagem|Total|O número total de tarefas|RunbookName, estado|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|CoreCount|Contagem de núcleos dedicado|Contagem|Total|Número total de núcleos dedicados da conta do batch|Não foram dimensões|
|TotalNodeCount|Número de nós dedicado|Contagem|Total|Número total de nós dedicado da conta do batch|Não foram dimensões|
|LowPriorityCoreCount|Contagem de núcleos LowPriority|Contagem|Total|Número total de núcleos de prioridade baixa na conta do batch|Não foram dimensões|
|TotalLowPriorityNodeCount|Número de nós de baixa prioridade|Contagem|Total|Número total de nós de prioridade baixa na conta do batch|Não foram dimensões|
|CreatingNodeCount|Criar o número de nós|Contagem|Total|Número de nós ser criada|Não foram dimensões|
|StartingNodeCount|Contagem inicial de nós|Contagem|Total|Número de nós partir de|Não foram dimensões|
|WaitingForStartTaskNodeCount|A aguardar para contagem de nós de tarefa de início|Contagem|Total|Número de nós a aguardar a conclusão da tarefa iniciar|Não foram dimensões|
|StartTaskFailedNodeCount|Número de nós de falha de tarefa de início|Contagem|Total|Número de nós em que a tarefa iniciar falhou|Não foram dimensões|
|IdleNodeCount|Contagem de nó inativo|Contagem|Total|Número de nós Inativos|Não foram dimensões|
|OfflineNodeCount|Número de nós offline|Contagem|Total|Número de nós offline|Não foram dimensões|
|RebootingNodeCount|Contagem de nós de reinício|Contagem|Total|Número de nós a ser reiniciado|Não foram dimensões|
|ReimagingNodeCount|Número de nós de reprocessamento de imagem|Contagem|Total|Número de nós de reprocessamento de imagem|Não foram dimensões|
|RunningNodeCount|Número de nós em execução|Contagem|Total|Número de nós a executar|Não foram dimensões|
|LeavingPoolNodeCount|Abandonar o fileparser número de nós de conjunto|Contagem|Total|Número de nós abandonar o fileparser o conjunto|Não foram dimensões|
|UnusableNodeCount|Número de nós não utilizável|Contagem|Total|Número de nós não utilizáveis|Não foram dimensões|
|PreemptedNodeCount|Número de nós antecipada|Contagem|Total|Número de nós antecipada|Não foram dimensões|
|TaskStartEvent|Eventos de início da tarefa|Contagem|Total|Número total de tarefas que foram iniciadas|Não foram dimensões|
|TaskCompleteEvent|Eventos de conclusão de tarefas|Contagem|Total|Número total de tarefas que foram concluídas|Não foram dimensões|
|TaskFailEvent|Eventos de falha de tarefa|Contagem|Total|Número total de tarefas concluiu em estado de falha|Não foram dimensões|
|PoolCreateEvent|Conjunto criar eventos|Contagem|Total|Número total de agrupamentos que foram criados|Não foram dimensões|
|PoolResizeStartEvent|Eventos de início de redimensionamento de conjunto|Contagem|Total|Número total de redimensiona conjunto que foram iniciadas|Não foram dimensões|
|PoolResizeCompleteEvent|Eventos de conclusão de redimensionamento do agrupamento|Contagem|Total|Número total de redimensiona conjunto que foram concluídas|Não foram dimensões|
|PoolDeleteStartEvent|Eventos de início de eliminação do conjunto|Contagem|Total|Número total de eliminações de conjunto que foram iniciadas|Não foram dimensões|
|PoolDeleteCompleteEvent|Conjunto eliminar eventos concluída|Contagem|Total|Número total de eliminações de conjunto que foram concluídas|Não foram dimensões|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|connectedclients|Clientes Ligados|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed|Total de Operações|Contagem|Total||Não foram dimensões|
|cachehits|Acertos na Cache|Contagem|Total||Não foram dimensões|
|cachemisses|Falhas na Cache|Contagem|Total||Não foram dimensões|
|getcommands|Obtém|Contagem|Total||Não foram dimensões|
|setcommands|Conjuntos|Contagem|Total||Não foram dimensões|
|evictedkeys|Chaves Excluídas|Contagem|Total||Não foram dimensões|
|totalkeys|Chaves Totais|Contagem|Máximo||Não foram dimensões|
|expiredkeys|Chaves Expiradas|Contagem|Total||Não foram dimensões|
|usedmemory|Memória Utilizada|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss|Memória utilizada RSS|Bytes|Máximo||Não foram dimensões|
|serverLoad|Carga do Servidor|Percentagem|Máximo||Não foram dimensões|
|cacheWrite|Escrita na Cache|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead|Leitura da Cache|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime|CPU|Percentagem|Máximo||Não foram dimensões|
|connectedclients0|Clientes ligados (ID de partição horizontal 0)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed0|Operações totais (ID de partição horizontal 0)|Contagem|Total||Não foram dimensões|
|cachehits0|Acertos na cache (ID de partição horizontal 0)|Contagem|Total||Não foram dimensões|
|cachemisses0|Pedidos sem êxito de cache (ID de partição horizontal 0)|Contagem|Total||Não foram dimensões|
|getcommands0|Obtém (ID de partição horizontal 0)|Contagem|Total||Não foram dimensões|
|setcommands0|Define (ID de partição horizontal 0)|Contagem|Total||Não foram dimensões|
|evictedkeys0|Expulsos as chaves (ID de partição horizontal 0)|Contagem|Total||Não foram dimensões|
|totalkeys0|Chaves totais (ID de partição horizontal 0)|Contagem|Máximo||Não foram dimensões|
|expiredkeys0|Chaves expiradas (ID de partição horizontal 0)|Contagem|Total||Não foram dimensões|
|usedmemory0|Memória utilizada (ID de partição horizontal 0)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss0|Memória utilizada RSS (ID de partição horizontal 0)|Bytes|Máximo||Não foram dimensões|
|serverLoad0|Carga de servidor (ID de partição horizontal 0)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite0|Cache de escrita (ID de partição horizontal 0)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead0|Cache de leitura (ID de partição horizontal 0)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime0|CPU (ID de partição horizontal 0)|Percentagem|Máximo||Não foram dimensões|
|connectedclients1|Clientes ligados (ID de partição horizontal 1)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed1|Operações (ID de partição horizontal 1) do totais|Contagem|Total||Não foram dimensões|
|cachehits1|Acertos na cache (ID de partição horizontal 1)|Contagem|Total||Não foram dimensões|
|cachemisses1|Pedidos sem êxito de cache (ID de partição horizontal 1)|Contagem|Total||Não foram dimensões|
|getcommands1|Obtém (ID de partição horizontal 1)|Contagem|Total||Não foram dimensões|
|setcommands1|Define (ID de partição horizontal 1)|Contagem|Total||Não foram dimensões|
|evictedkeys1|Expulsos as chaves (ID de partição horizontal 1)|Contagem|Total||Não foram dimensões|
|totalkeys1|Chaves totais (ID de partição horizontal 1)|Contagem|Máximo||Não foram dimensões|
|expiredkeys1|Chaves expiradas (ID de partição horizontal 1)|Contagem|Total||Não foram dimensões|
|usedmemory1|Memória utilizada (ID de partição horizontal 1)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss1|Memória utilizada RSS (ID de partição horizontal 1)|Bytes|Máximo||Não foram dimensões|
|serverLoad1|Carga de servidor (ID de partição horizontal 1)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite1|Cache de escrita (ID de partição horizontal 1)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead1|Cache de leitura (ID de partição horizontal 1)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime1|CPU (ID de partição horizontal 1)|Percentagem|Máximo||Não foram dimensões|
|connectedclients2|Clientes ligados (ID de partição horizontal 2)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed2|Operações (ID de partição horizontal 2) do totais|Contagem|Total||Não foram dimensões|
|cachehits2|Acertos na cache (ID de partição horizontal 2)|Contagem|Total||Não foram dimensões|
|cachemisses2|Pedidos sem êxito de cache (ID de partição horizontal 2)|Contagem|Total||Não foram dimensões|
|getcommands2|Obtém (ID de partição horizontal 2)|Contagem|Total||Não foram dimensões|
|setcommands2|Define (ID de partição horizontal 2)|Contagem|Total||Não foram dimensões|
|evictedkeys2|Expulsos as chaves (ID de partição horizontal 2)|Contagem|Total||Não foram dimensões|
|totalkeys2|Chaves totais (ID de partição horizontal 2)|Contagem|Máximo||Não foram dimensões|
|expiredkeys2|Chaves expiradas (ID de partição horizontal 2)|Contagem|Total||Não foram dimensões|
|usedmemory2|Memória utilizada (ID de partição horizontal 2)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss2|Memória utilizada RSS (ID de partição horizontal 2)|Bytes|Máximo||Não foram dimensões|
|serverLoad2|Carga de servidor (ID de partição horizontal 2)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite2|Cache de escrita (ID de partição horizontal 2)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead2|Cache de leitura (ID de partição horizontal 2)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime2|CPU (ID de partição horizontal 2)|Percentagem|Máximo||Não foram dimensões|
|connectedclients3|Clientes ligados (ID de partição horizontal 3)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed3|Operações (ID de partição horizontal 3) do totais|Contagem|Total||Não foram dimensões|
|cachehits3|Acertos na cache (ID de partição horizontal 3)|Contagem|Total||Não foram dimensões|
|cachemisses3|Pedidos sem êxito de cache (ID de partição horizontal 3)|Contagem|Total||Não foram dimensões|
|getcommands3|Obtém (ID de partição horizontal 3)|Contagem|Total||Não foram dimensões|
|setcommands3|Define (ID de partição horizontal 3)|Contagem|Total||Não foram dimensões|
|evictedkeys3|Expulsos as chaves (ID de partição horizontal 3)|Contagem|Total||Não foram dimensões|
|totalkeys3|Chaves totais (ID de partição horizontal 3)|Contagem|Máximo||Não foram dimensões|
|expiredkeys3|Chaves expiradas (ID de partição horizontal 3)|Contagem|Total||Não foram dimensões|
|usedmemory3|Memória utilizada (ID de partição horizontal 3)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss3|Memória utilizada RSS (ID de partição horizontal 3)|Bytes|Máximo||Não foram dimensões|
|serverLoad3|Carga de servidor (ID de partição horizontal 3)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite3|Cache de escrita (ID de partição horizontal 3)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead3|Cache de leitura (ID de partição horizontal 3)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime3|CPU (ID de partição horizontal 3)|Percentagem|Máximo||Não foram dimensões|
|connectedclients4|Clientes ligados (ID de partição horizontal 4)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed4|Operações totais (ID de partição horizontal 4)|Contagem|Total||Não foram dimensões|
|cachehits4|Acertos na cache (ID de partição horizontal 4)|Contagem|Total||Não foram dimensões|
|cachemisses4|Pedidos sem êxito de cache (ID de partição horizontal 4)|Contagem|Total||Não foram dimensões|
|getcommands4|Obtém (ID de partição horizontal 4)|Contagem|Total||Não foram dimensões|
|setcommands4|Define (ID de partição horizontal 4)|Contagem|Total||Não foram dimensões|
|evictedkeys4|Expulsos as chaves (ID de partição horizontal 4)|Contagem|Total||Não foram dimensões|
|totalkeys4|Chaves totais (ID de partição horizontal 4)|Contagem|Máximo||Não foram dimensões|
|expiredkeys4|Chaves expiradas (ID de partição horizontal 4)|Contagem|Total||Não foram dimensões|
|usedmemory4|Memória utilizada (ID de partição horizontal 4)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss4|Memória utilizada RSS (ID de partição horizontal 4)|Bytes|Máximo||Não foram dimensões|
|serverLoad4|Carga de servidor (ID de partição horizontal 4)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite4|Cache de escrita (ID de partição horizontal 4)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead4|Cache de leitura (ID de partição horizontal 4)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime4|CPU (ID de partição horizontal 4)|Percentagem|Máximo||Não foram dimensões|
|connectedclients5|Clientes ligados (ID de partição horizontal 5)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed5|Operações totais (ID de partição horizontal 5)|Contagem|Total||Não foram dimensões|
|cachehits5|Acertos na cache (ID de partição horizontal 5)|Contagem|Total||Não foram dimensões|
|cachemisses5|Pedidos sem êxito de cache (ID de partição horizontal 5)|Contagem|Total||Não foram dimensões|
|getcommands5|Obtém (ID de partição horizontal 5)|Contagem|Total||Não foram dimensões|
|setcommands5|Define (ID de partição horizontal 5)|Contagem|Total||Não foram dimensões|
|evictedkeys5|Expulsos as chaves (ID de partição horizontal 5)|Contagem|Total||Não foram dimensões|
|totalkeys5|Chaves totais (ID de partição horizontal 5)|Contagem|Máximo||Não foram dimensões|
|expiredkeys5|Chaves expiradas (ID de partição horizontal 5)|Contagem|Total||Não foram dimensões|
|usedmemory5|Memória utilizada (ID de partição horizontal 5)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss5|Memória utilizada RSS (ID de partição horizontal 5)|Bytes|Máximo||Não foram dimensões|
|serverLoad5|Carga de servidor (ID de partição horizontal 5)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite5|Cache de escrita (ID de partição horizontal 5)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead5|Cache de leitura (ID de partição horizontal 5)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime5|CPU (ID de partição horizontal 5)|Percentagem|Máximo||Não foram dimensões|
|connectedclients6|Clientes ligados (ID de partição horizontal 6)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed6|Operações totais (ID de partição horizontal 6)|Contagem|Total||Não foram dimensões|
|cachehits6|Acertos na cache (ID de partição horizontal 6)|Contagem|Total||Não foram dimensões|
|cachemisses6|Pedidos sem êxito de cache (ID de partição horizontal 6)|Contagem|Total||Não foram dimensões|
|getcommands6|Obtém (ID de partição horizontal 6)|Contagem|Total||Não foram dimensões|
|setcommands6|Define (ID de partição horizontal 6)|Contagem|Total||Não foram dimensões|
|evictedkeys6|Expulsos as chaves (ID de partição horizontal 6)|Contagem|Total||Não foram dimensões|
|totalkeys6|Chaves totais (ID de partição horizontal 6)|Contagem|Máximo||Não foram dimensões|
|expiredkeys6|Chaves expiradas (ID de partição horizontal 6)|Contagem|Total||Não foram dimensões|
|usedmemory6|Memória utilizada (ID de partição horizontal 6)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss6|Memória utilizada RSS (ID de partição horizontal 6)|Bytes|Máximo||Não foram dimensões|
|serverLoad6|Carga de servidor (ID de partição horizontal 6)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite6|Cache de escrita (ID de partição horizontal 6)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead6|Cache de leitura (ID de partição horizontal 6)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime6|CPU (ID de partição horizontal 6)|Percentagem|Máximo||Não foram dimensões|
|connectedclients7|Clientes ligados (ID de partição horizontal 7)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed7|Operações totais (ID de partição horizontal 7)|Contagem|Total||Não foram dimensões|
|cachehits7|Acertos na cache (ID de partição horizontal 7)|Contagem|Total||Não foram dimensões|
|cachemisses7|Pedidos sem êxito de cache (ID de partição horizontal 7)|Contagem|Total||Não foram dimensões|
|getcommands7|Obtém (ID de partição horizontal 7)|Contagem|Total||Não foram dimensões|
|setcommands7|Define (ID de partição horizontal 7)|Contagem|Total||Não foram dimensões|
|evictedkeys7|Expulsos as chaves (ID de partição horizontal 7)|Contagem|Total||Não foram dimensões|
|totalkeys7|Chaves totais (ID de partição horizontal 7)|Contagem|Máximo||Não foram dimensões|
|expiredkeys7|Chaves expiradas (ID de partição horizontal 7)|Contagem|Total||Não foram dimensões|
|usedmemory7|Memória utilizada (ID de partição horizontal 7)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss7|Memória utilizada RSS (ID de partição horizontal 7)|Bytes|Máximo||Não foram dimensões|
|serverLoad7|Carga de servidor (ID de partição horizontal 7)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite7|Cache de escrita (ID de partição horizontal 7)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead7|Cache de leitura (ID de partição horizontal 7)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime7|CPU (ID de partição horizontal 7)|Percentagem|Máximo||Não foram dimensões|
|connectedclients8|Clientes ligados (ID de partição horizontal 8)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed8|Operações totais (ID de partição horizontal 8)|Contagem|Total||Não foram dimensões|
|cachehits8|Acertos na cache (ID de partição horizontal 8)|Contagem|Total||Não foram dimensões|
|cachemisses8|Pedidos sem êxito de cache (ID de partição horizontal 8)|Contagem|Total||Não foram dimensões|
|getcommands8|Obtém (ID de partição horizontal 8)|Contagem|Total||Não foram dimensões|
|setcommands8|Define (ID de partição horizontal 8)|Contagem|Total||Não foram dimensões|
|evictedkeys8|Expulsos as chaves (ID de partição horizontal 8)|Contagem|Total||Não foram dimensões|
|totalkeys8|Chaves totais (ID de partição horizontal 8)|Contagem|Máximo||Não foram dimensões|
|expiredkeys8|Chaves expiradas (ID de partição horizontal 8)|Contagem|Total||Não foram dimensões|
|usedmemory8|Memória utilizada (ID de partição horizontal 8)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss8|Memória utilizada RSS (ID de partição horizontal 8)|Bytes|Máximo||Não foram dimensões|
|serverLoad8|Carga de servidor (ID de partição horizontal 8)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite8|Cache de escrita (ID de partição horizontal 8)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead8|Cache de leitura (ID de partição horizontal 8)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime8|CPU (ID de partição horizontal 8)|Percentagem|Máximo||Não foram dimensões|
|connectedclients9|Clientes ligados (ID de partição horizontal 9)|Contagem|Máximo||Não foram dimensões|
|totalcommandsprocessed9|Operações totais (ID de partição horizontal 9)|Contagem|Total||Não foram dimensões|
|cachehits9|Acertos na cache (ID de partição horizontal 9)|Contagem|Total||Não foram dimensões|
|cachemisses9|Pedidos sem êxito de cache (ID de partição horizontal 9)|Contagem|Total||Não foram dimensões|
|getcommands9|Obtém (ID de partição horizontal 9)|Contagem|Total||Não foram dimensões|
|setcommands9|Define (ID de partição horizontal 9)|Contagem|Total||Não foram dimensões|
|evictedkeys9|Expulsos as chaves (ID de partição horizontal 9)|Contagem|Total||Não foram dimensões|
|totalkeys9|Chaves totais (ID de partição horizontal 9)|Contagem|Máximo||Não foram dimensões|
|expiredkeys9|Chaves expiradas (ID de partição horizontal 9)|Contagem|Total||Não foram dimensões|
|usedmemory9|Memória utilizada (ID de partição horizontal 9)|Bytes|Máximo||Não foram dimensões|
|usedmemoryRss9|Memória utilizada RSS (ID de partição horizontal 9)|Bytes|Máximo||Não foram dimensões|
|serverLoad9|Carga de servidor (ID de partição horizontal 9)|Percentagem|Máximo||Não foram dimensões|
|cacheWrite9|Cache de escrita (ID de partição horizontal 9)|BytesPerSecond|Máximo||Não foram dimensões|
|cacheRead9|Cache de leitura (ID de partição horizontal 9)|BytesPerSecond|Máximo||Não foram dimensões|
|percentProcessorTime9|CPU (ID de partição horizontal 9)|Percentagem|Máximo||Não foram dimensões|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|Percentagem da CPU|Percentagem da CPU|Percentagem|Média|A percentagem de unidades do computador alocadas que estão atualmente em utilização pela Máquina Virtual ou Máquinas Virtuais.|Não foram dimensões|
|Entrada de Rede|Entrada de Rede|Bytes|Total|O número de bytes recebidos em todas as interfaces de rede pela Máquina Virtual ou Máquinas Virtuais (Tráfego de Entrada).|Não foram dimensões|
|Saída de Rede|Saída de Rede|Bytes|Total|O número de bytes saídos de todas as interfaces de rede pela(s) Máquina(s) Virtual(is) (Tráfego de Saída).|Não foram dimensões|
|Bytes Lidos de Disco/Seg|Leitura de Disco|BytesPerSecond|Média|Média de bytes lidos do disco durante o período de monitorização.|Não foram dimensões|
|Bytes Escritos em Disco/Seg|Escrita de Disco|BytesPerSecond|Média|Média de bytes escritos no disco durante o período de monitorização.|Não foram dimensões|
|Operações/Seg de Leitura do Disco|Operações/Seg de Leitura do Disco|CountPerSecond|Média|IOPS de Leitura do Disco.|Não foram dimensões|
|Operações/Seg de Escrita de Disco|Operações/Seg de Escrita de Disco|CountPerSecond|Média|IOPS de Escrita de Disco.|Não foram dimensões|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|TotalCalls|Total de Chamadas|Contagem|Total|Número total de chamadas.|Não foram dimensões|
|SuccessfulCalls|Chamadas com êxito|Contagem|Total|Número de chamadas com êxito.|Não foram dimensões|
|TotalErrors|Total de Erros|Contagem|Total|Número total de chamadas com resposta de erro (código de resposta HTTP 4xx ou 5xx).|Não foram dimensões|
|BlockedCalls|Chamadas Bloqueadas|Contagem|Total|Número de chamadas que excederam o limite da tarifa ou quota.|Não foram dimensões|
|ServerErrors|Erros no Servidor|Contagem|Total|Número de chamadas com erro interno do serviço (código de resposta HTTP 5xx).|Não foram dimensões|
|ClientErrors|Erros do Cliente|Contagem|Total|Número de chamadas com erro do lado do cliente (código de resposta HTTP 4xx).|Não foram dimensões|
|DataIn|Entrada de Dados|Bytes|Total|Tamanho dos dados recebidos em bytes.|Não foram dimensões|
|DataOut|Saída de Dados|Bytes|Total|Tamanho dos dados enviados em bytes.|Não foram dimensões|
|Latência|Latência|Milissegundos|Média|Latência em milisegundos.|Não foram dimensões|
|CharactersTranslated|Carateres Traduzidos|Contagem|Total|Número total de carateres na receção do pedido de texto.|Não foram dimensões|
|SpeechSessionDuration|Duração da Sessão de Voz|Segundos|Total|Duração total da sessão de voz em segundos.|Não foram dimensões|
|TotalTransactions|Total de transações|Contagem|Total|Número total de transações|Não foram dimensões|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|Percentagem da CPU|Percentagem da CPU|Percentagem|Média|A percentagem de unidades do computador alocadas que estão atualmente em utilização pela Máquina Virtual ou Máquinas Virtuais|Não foram dimensões|
|Entrada de Rede|Entrada de Rede|Bytes|Total|O número de bytes recebidos em todas as interfaces de rede pela Máquina Virtual ou Máquinas Virtuais (Tráfego de Entrada)|Não foram dimensões|
|Saída de Rede|Saída de Rede|Bytes|Total|O número de bytes saídos de todas as interfaces de rede pela(s) Máquina(s) Virtual(is) (Tráfego de Saída)|Não foram dimensões|
|Bytes de Leitura do Disco|Bytes de Leitura do Disco|Bytes|Total|Total de bytes lidos do disco durante o período de monitorização|Não foram dimensões|
|Bytes de Escrita de Disco|Bytes de Escrita de Disco|Bytes|Total|Total de bytes escritos no disco durante o período de monitorização|Não foram dimensões|
|Operações/Seg de Leitura do Disco|Operações/Seg de Leitura do Disco|CountPerSecond|Média|IOPS de Leitura do Disco|Não foram dimensões|
|Operações/Seg de Escrita de Disco|Operações/Seg de Escrita de Disco|CountPerSecond|Média|IOPS de Escrita de Disco|Não foram dimensões|
|Créditos CPU Restantes|Créditos CPU Restantes|Contagem|Média|Número total de créditos disponíveis para rajada|Não foram dimensões|
|Créditos CPU Consumidos|Créditos CPU Consumidos|Contagem|Média|Número total de créditos consumidos pela Máquina Virtual|Não foram dimensões|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|Percentagem da CPU|Percentagem da CPU|Percentagem|Média|A percentagem de unidades do computador alocadas que estão atualmente em utilização pela Máquina Virtual ou Máquinas Virtuais|Não foram dimensões|
|Entrada de Rede|Entrada de Rede|Bytes|Total|O número de bytes recebidos em todas as interfaces de rede pela Máquina Virtual ou Máquinas Virtuais (Tráfego de Entrada)|Não foram dimensões|
|Saída de Rede|Saída de Rede|Bytes|Total|O número de bytes saídos de todas as interfaces de rede pela(s) Máquina(s) Virtual(is) (Tráfego de Saída)|Não foram dimensões|
|Bytes de Leitura do Disco|Bytes de Leitura do Disco|Bytes|Total|Total de bytes lidos do disco durante o período de monitorização|Não foram dimensões|
|Bytes de Escrita de Disco|Bytes de Escrita de Disco|Bytes|Total|Total de bytes escritos no disco durante o período de monitorização|Não foram dimensões|
|Operações/Seg de Leitura do Disco|Operações/Seg de Leitura do Disco|CountPerSecond|Média|IOPS de Leitura do Disco|Não foram dimensões|
|Operações/Seg de Escrita de Disco|Operações/Seg de Escrita de Disco|CountPerSecond|Média|IOPS de Escrita de Disco|Não foram dimensões|
|Créditos CPU Restantes|Créditos CPU Restantes|Contagem|Média|Número total de créditos disponíveis para rajada|Não foram dimensões|
|Créditos CPU Consumidos|Créditos CPU Consumidos|Contagem|Média|Número total de créditos consumidos pela Máquina Virtual|Não foram dimensões|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|Percentagem da CPU|Percentagem da CPU|Percentagem|Média|A percentagem de unidades do computador alocadas que estão atualmente em utilização pela Máquina Virtual ou Máquinas Virtuais|Não foram dimensões|
|Entrada de Rede|Entrada de Rede|Bytes|Total|O número de bytes recebidos em todas as interfaces de rede pela Máquina Virtual ou Máquinas Virtuais (Tráfego de Entrada)|Não foram dimensões|
|Saída de Rede|Saída de Rede|Bytes|Total|O número de bytes saídos de todas as interfaces de rede pela(s) Máquina(s) Virtual(is) (Tráfego de Saída)|Não foram dimensões|
|Bytes de Leitura do Disco|Bytes de Leitura do Disco|Bytes|Total|Total de bytes lidos do disco durante o período de monitorização|Não foram dimensões|
|Bytes de Escrita de Disco|Bytes de Escrita de Disco|Bytes|Total|Total de bytes escritos no disco durante o período de monitorização|Não foram dimensões|
|Operações/Seg de Leitura do Disco|Operações/Seg de Leitura do Disco|CountPerSecond|Média|IOPS de Leitura do Disco|Não foram dimensões|
|Operações/Seg de Escrita de Disco|Operações/Seg de Escrita de Disco|CountPerSecond|Média|IOPS de Escrita de Disco|Não foram dimensões|
|Créditos CPU Restantes|Créditos CPU Restantes|Contagem|Média|Número total de créditos disponíveis para rajada|Não foram dimensões|
|Créditos CPU Consumidos|Créditos CPU Consumidos|Contagem|Média|Número total de créditos consumidos pela Máquina Virtual|Não foram dimensões|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|DCIApiCalls|Chamadas de API de informações de cliente|Contagem|Total||Não foram dimensões|
|DCIMappingImportOperationSuccessfulLines|Mapeamento de operação de importação linhas com êxito|Contagem|Total||Não foram dimensões|
|DCIMappingImportOperationFailedLines|Mapeamento de operação de importação falha linhas|Contagem|Total||Não foram dimensões|
|DCIMappingImportOperationTotalLines|Mapeamento de operação de importação linhas Total|Contagem|Total||Não foram dimensões|
|DCIMappingImportOperationRuntimeInSeconds|Runtime de operação de importação de mapeamento em segundos|Segundos|Total||Não foram dimensões|
|DCIOutboundProfileExportSucceeded|Exportação de perfil de saída foi concluída com êxito|Contagem|Total||Não foram dimensões|
|DCIOutboundProfileExportFailed|Falhada de exportação de perfil de saída|Contagem|Total||Não foram dimensões|
|DCIOutboundProfileExportDuration|Duração de exportação de perfil de saída|Segundos|Total||Não foram dimensões|
|DCIOutboundKpiExportSucceeded|Saída de Kpi de exportação com êxito|Contagem|Total||Não foram dimensões|
|DCIOutboundKpiExportFailed|Falha de saída de Kpi de exportação|Contagem|Total||Não foram dimensões|
|DCIOutboundKpiExportDuration|Duração de saída de Kpi de exportação|Segundos|Total||Não foram dimensões|
|DCIOutboundKpiExportStarted|Exportação de Kpi de saída foi iniciada|Segundos|Total||Não foram dimensões|
|DCIOutboundKpiRecordCount|Contagem de registo de Kpi de saída|Segundos|Total||Não foram dimensões|
|DCIOutboundProfileExportCount|Contagem de exportação de perfil de saída|Segundos|Total||Não foram dimensões|
|DCIOutboundInitialProfileExportFailed|Falhada de exportação de saída perfil inicial|Segundos|Total||Não foram dimensões|
|DCIOutboundInitialProfileExportSucceeded|Exportação de saída perfil inicial foi concluída com êxito|Segundos|Total||Não foram dimensões|
|DCIOutboundInitialKpiExportFailed|Exportação de Kpi inicial saída falhou|Segundos|Total||Não foram dimensões|
|DCIOutboundInitialKpiExportSucceeded|Exportação de Kpi inicial saída com êxito|Segundos|Total||Não foram dimensões|
|DCIOutboundInitialProfileExportDurationInSeconds|Duração de exportação de saída perfil inicial em segundos|Segundos|Total||Não foram dimensões|
|AdlaJobForStandardKpiFailed|Tarefa de Adla para Standard Kpi falhada em segundos|Segundos|Total||Não foram dimensões|
|AdlaJobForStandardKpiTimeOut|Tarefa de Adla para padrão de Kpi de tempo limite em segundos|Segundos|Total||Não foram dimensões|
|AdlaJobForStandardKpiCompleted|Tarefa de Adla para Standard Kpi foi concluída em segundos|Segundos|Total||Não foram dimensões|
|ImportASAValuesFailed|Contagem de falha na importação ASA valores|Contagem|Total||Não foram dimensões|
|ImportASAValuesSucceeded|Importar ASA valores êxito contagem|Contagem|Total||Não foram dimensões|
|DCIProfilesCount|Contagem de instâncias de perfil|Contagem|Último||Não foram dimensões|
|DCIInteractionsPerMonthCount|Interações por contagem de mês|Contagem|Último||Não foram dimensões|
|DCIKpisCount|Contagem de KPI|Contagem|Último||Não foram dimensões|
|DCISegmentsCount|Contagem de Segmentos|Contagem|Último||Não foram dimensões|
|DCIPredictiveMatchPoliciesCount|Contagem de correspondências preditiva|Contagem|Último||Não foram dimensões|
|DCIPredictionsCount|Contagem de predição|Contagem|Último||Não foram dimensões|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|SuccessfulRuns|Execuções Com Êxito|Contagem|Total|Número de execuções com êxito.|Não foram dimensões|
|FailedRuns|Execuções Falhadas|Contagem|Total|Número de execuções de falha.|Não foram dimensões|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|PipelineFailedRuns|Falha de métricas de execução de pipeline|Contagem|Total||Não foram dimensões|
|PipelineSucceededRuns|Foi concluída com êxito as métricas de execução de pipeline|Contagem|Total||Não foram dimensões|
|ActivityFailedRuns|Falha de métricas de execuções de atividade|Contagem|Total||Não foram dimensões|
|ActivitySucceededRuns|Foi concluída com êxito as métricas de execuções de atividade|Contagem|Total||Não foram dimensões|
|TriggerFailedRuns|Falha de métricas de execução do acionador|Contagem|Total||Não foram dimensões|
|TriggerSucceededRuns|Foi concluída com êxito as métricas de execuções do acionador|Contagem|Total||Não foram dimensões|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|JobEndedSuccess|Tarefas com êxito|Contagem|Total|Contagem de tarefas com êxito.|Não foram dimensões|
|JobEndedFailure|Tarefas falhadas|Contagem|Total|Contagem de tarefas com falha.|Não foram dimensões|
|JobEndedCancelled|Tarefas canceladas|Contagem|Total|Contagem de tarefas canceladas.|Não foram dimensões|
|JobAUEndedSuccess|Tempo de AU com êxito|Segundos|Total|Tempo de AU total para as tarefas com êxito.|Não foram dimensões|
|JobAUEndedFailure|Falha de AU de tempo|Segundos|Total|Tempo de AU total para as tarefas falhadas.|Não foram dimensões|
|JobAUEndedCancelled|Tempo de AU cancelada|Segundos|Total|Tempo total de AU para tarefas canceladas.|Não foram dimensões|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|TotalStorage|Armazenamento total|Bytes|Máximo|Quantidade total de dados armazenados na conta.|Não foram dimensões|
|DataWritten|Dados escritos|Bytes|Total|Quantidade total de dados escritos para a conta.|Não foram dimensões|
|DataRead|Leitura de dados|Bytes|Total|Quantidade total de dados de leitura da conta.|Não foram dimensões|
|WriteRequests|Pedidos de Escrita|Contagem|Total|Contagem de dados de pedidos de escrita para a conta.|Não foram dimensões|
|ReadRequests|Pedidos de Leitura|Contagem|Total|Contagem de dados de pedidos para a conta de leitura.|Não foram dimensões|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Percentagem de CPU|Percentagem|Média|Percentagem de CPU|Não foram dimensões|
|compute_limit|Limite de unidade de computação|Contagem|Média|Limite de unidade de computação|Não foram dimensões|
|compute_consumption_percent|Percentagem de unidade de computação|Percentagem|Média|Percentagem de unidade de computação|Não foram dimensões|
|memory_percent|Percentagem de memória|Percentagem|Média|Percentagem de memória|Não foram dimensões|
|io_consumption_percent|Percentagem de e/s|Percentagem|Média|Percentagem de e/s|Não foram dimensões|
|storage_percent|Percentagem de armazenamento|Percentagem|Média|Percentagem de armazenamento|Não foram dimensões|
|storage_used|Armazenamento utilizado|Bytes|Média|Armazenamento utilizado|Não foram dimensões|
|storage_limit|Limite de armazenamento|Bytes|Média|Limite de armazenamento|Não foram dimensões|
|active_connections|Totais ligações ativas|Contagem|Média|Totais ligações ativas|Não foram dimensões|
|connections_failed|Totais de ligações com falhas|Contagem|Média|Totais de ligações com falhas|Não foram dimensões|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Percentagem de CPU|Percentagem|Média|Percentagem de CPU|Não foram dimensões|
|compute_limit|Limite de unidade de computação|Contagem|Média|Limite de unidade de computação|Não foram dimensões|
|compute_consumption_percent|Percentagem de unidade de computação|Percentagem|Média|Percentagem de unidade de computação|Não foram dimensões|
|memory_percent|Percentagem de memória|Percentagem|Média|Percentagem de memória|Não foram dimensões|
|io_consumption_percent|Percentagem de e/s|Percentagem|Média|Percentagem de e/s|Não foram dimensões|
|storage_percent|Percentagem de armazenamento|Percentagem|Média|Percentagem de armazenamento|Não foram dimensões|
|storage_used|Armazenamento utilizado|Bytes|Média|Armazenamento utilizado|Não foram dimensões|
|storage_limit|Limite de armazenamento|Bytes|Média|Limite de armazenamento|Não foram dimensões|
|active_connections|Totais ligações ativas|Contagem|Média|Totais ligações ativas|Não foram dimensões|
|connections_failed|Totais de ligações com falhas|Contagem|Média|Totais de ligações com falhas|Não foram dimensões|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|d2c.telemetry.Ingress.allProtocol|Tentativas de envio de mensagem de telemetria|Contagem|Total|Número de mensagens de telemetria do dispositivo para nuvem tentada a enviar ao seu IoT hub|Não foram dimensões|
|d2c.telemetry.Ingress.Success|Mensagens de telemetria enviadas|Contagem|Total|Número de mensagens de telemetria do dispositivo para nuvem enviadas com êxito ao seu IoT hub|Não foram dimensões|
|c2d.Commands.egress.Complete.Success|Comandos concluídos|Contagem|Total|Número de comandos da nuvem para o dispositivo foi concluída com êxito pelo dispositivo|Não foram dimensões|
|c2d.Commands.egress.Abandon.Success|Comandos abandonados|Contagem|Total|Número de comandos da nuvem para o dispositivo abandonadas pelo dispositivo|Não foram dimensões|
|c2d.Commands.egress.Reject.Success|Comandos rejeitados|Contagem|Total|Número de comandos da nuvem para o dispositivo rejeitado pelo dispositivo|Não foram dimensões|
|devices.totalDevices|Total de dispositivos|Contagem|Total|Número de dispositivos registados ao seu IoT hub|Não foram dimensões|
|devices.connectedDevices.allProtocol|Dispositivos ligados|Contagem|Total|Número de dispositivos ligados ao seu IoT hub|Não foram dimensões|
|d2c.telemetry.egress.Success|Mensagens de telemetria entregues|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito para os pontos finais (total)|Não foram dimensões|
|d2c.telemetry.egress.dropped|Mensagens de ignorados|Contagem|Total|Número de mensagens removida porque o ponto final de entrega foi Inativos|Não foram dimensões|
|d2c.telemetry.egress.orphaned|Mensagens órfãos|Contagem|Total|O número de mensagens não corresponde a qualquer rotas, incluindo a rota de contingência|Não foram dimensões|
|d2c.telemetry.egress.Invalid|Mensagens inválidas|Contagem|Total|A contagem de mensagens não entregues devido a incompatibilidade com o ponto final|Não foram dimensões|
|d2c.telemetry.egress.fallback|Mensagens de correspondência de condição de contingência|Contagem|Total|Número de mensagens escritas para o ponto final de contingência|Não foram dimensões|
|d2c.Endpoints.egress.eventHubs|Mensagens entregues nos pontos finais de Hub de eventos|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de Hub de eventos|Não foram dimensões|
|d2c.Endpoints.latency.eventHubs|Latência de mensagem para pontos finais de Hub de eventos|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para um ponto final de Hub de eventos, em milissegundos|Não foram dimensões|
|d2c.Endpoints.egress.serviceBusQueues|Mensagens entregues nos pontos finais de fila do Service Bus|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de fila do Service Bus|Não foram dimensões|
|d2c.Endpoints.latency.serviceBusQueues|Latência de mensagem para pontos finais de fila do Service Bus|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para um ponto final de fila do Service Bus, em milissegundos|Não foram dimensões|
|d2c.Endpoints.egress.serviceBusTopics|Mensagens entregues nos pontos finais de tópico de barramento de serviço|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de tópico de barramento de serviço|Não foram dimensões|
|d2c.Endpoints.latency.serviceBusTopics|Latência de mensagem para pontos finais de tópico de barramento de serviço|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para um ponto final de tópico de barramento de serviço, em milissegundos|Não foram dimensões|
|d2c.Endpoints.egress.builtIn.Events|Mensagens entregues para o ponto final incorporado (mensagens/eventos)|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito para o ponto final incorporado (mensagens/eventos)|Não foram dimensões|
|d2c.Endpoints.latency.builtIn.Events|Latência de mensagem para o ponto final incorporado (mensagens/eventos)|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para o ponto final incorporado (mensagens/eventos), em milissegundos |Não foram dimensões|
|d2c.Endpoints.egress.Storage|Mensagens entregues nos pontos finais de armazenamento|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de armazenamento|Não foram dimensões|
|d2c.Endpoints.latency.Storage|Latência de mensagem para pontos finais de armazenamento|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para um ponto final de armazenamento, em milissegundos|Não foram dimensões|
|d2c.Endpoints.egress.Storage.bytes|Dados escritos para armazenamento|Bytes|Total|Quantidade de dados, em bytes, escritos para pontos finais de armazenamento|Não foram dimensões|
|d2c.Endpoints.egress.Storage.BLOBs|Escrito para o armazenamento de BLOBs|Contagem|Total|Número de escritas para pontos finais de armazenamento de blobs|Não foram dimensões|
|d2c.Twin.Read.Success|Leituras de duplo bem-sucedida dos dispositivos|Contagem|Total|A contagem de leituras de iniciadas por dispositivo duplo todos os com êxito.|Não foram dimensões|
|d2c.Twin.Read.Failure|Não foi possível duplo leituras dos dispositivos|Contagem|Total|A contagem de todas as falha leituras iniciadas por dispositivo duplo.|Não foram dimensões|
|d2c.Twin.Read.size|Tamanho da resposta de duplo lê a partir de dispositivos|Bytes|Média|A média, mínimo e máximo de todos os com êxito iniciadas por dispositivo duplo leituras.|Não foram dimensões|
|d2c.Twin.Update.Success|Atualizações de duplo com êxito a partir de dispositivos|Contagem|Total|A contagem das atualizações de iniciadas por dispositivo duplo todos os com êxito.|Não foram dimensões|
|d2c.Twin.Update.Failure|Falha de atualizações de duplo a partir de dispositivos|Contagem|Total|A contagem de todas as falha atualizações iniciadas por dispositivo duplo.|Não foram dimensões|
|d2c.Twin.Update.size|Tamanho das atualizações de duplo a partir de dispositivos|Bytes|Média|A média, min e o tamanho máximo de todos os com êxito iniciadas por dispositivo duplo atualizações.|Não foram dimensões|
|c2d.methods.Success|Invocações de método direto com êxito|Contagem|Total|A contagem de chamadas de método direto todos os com êxito.|Não foram dimensões|
|c2d.methods.Failure|Não foi possível invocações de método direto|Contagem|Total|A contagem de todas as falha chamadas de método direto.|Não foram dimensões|
|c2d.methods.requestSize|Tamanho do pedido de invocações de método direto|Bytes|Média|A média, mínimo e máximo de todos os com êxito direcionam os pedidos de método.|Não foram dimensões|
|c2d.methods.responseSize|Tamanho da resposta de invocações de método direto|Bytes|Média|A média, mínimo e máximo de respostas de método direto todos os com êxito.|Não foram dimensões|
|c2d.Twin.Read.Success|Leituras de duplo com êxito de back-end|Contagem|Total|A contagem de leituras de back-end-iniciada duplo todos os com êxito.|Não foram dimensões|
|c2d.Twin.Read.Failure|Leituras de duplo falhada de back-end|Contagem|Total|A contagem de todas as falha leituras duplo iniciada em back-end.|Não foram dimensões|
|c2d.Twin.Read.size|Tamanho da resposta de duplo lê a partir do back-end|Bytes|Média|O média, mínimo e máximo de todos os com êxito iniciada em back-end duplo leituras.|Não foram dimensões|
|c2d.Twin.Update.Success|Atualizações de duplo com êxito a partir de back-end|Contagem|Total|A contagem das atualizações de back-end-iniciada duplo todos os com êxito.|Não foram dimensões|
|c2d.Twin.Update.Failure|Atualizações de duplo falhada a partir de back-end|Contagem|Total|A contagem de todas as falha atualizações duplo iniciada em back-end.|Não foram dimensões|
|c2d.Twin.Update.size|Tamanho de atualizações de duplo do back-end|Bytes|Média|A média, min e o tamanho máximo de todos os com êxito iniciada em back-end duplo atualizações.|Não foram dimensões|
|twinQueries.success|Consultas de duplo com êxito|Contagem|Total|A contagem de todas as consultas de duplo com êxito.|Não foram dimensões|
|twinQueries.failure|Consultas de duplo falhada|Contagem|Total|A contagem de todas as consultas de duplo falhada.|Não foram dimensões|
|twinQueries.resultSize|Tamanho dos resultados consultas duplo|Bytes|Média|A média, mínimo e máximo, o tamanho dos resultados de todas as consultas de duplo com êxito.|Não foram dimensões|
|jobs.createTwinUpdateJob.success|Criações com êxito das tarefas de atualização de duplo|Contagem|Total|A contagem de todos os criação com êxito das tarefas de atualização de duplo.|Não foram dimensões|
|jobs.createTwinUpdateJob.failure|Falha de criações de tarefas de atualização de duplo|Contagem|Total|A contagem de todos os falha na criação de tarefas de atualização de duplo.|Não foram dimensões|
|jobs.createDirectMethodJob.success|Criações com êxito das tarefas de invocação do método|Contagem|Total|A contagem de todos os criação com êxito das tarefas de invocação do método direto.|Não foram dimensões|
|jobs.createDirectMethodJob.failure|Falha de criações de tarefas de invocação do método|Contagem|Total|A contagem de todos os falha na criação de tarefas de invocação do método direto.|Não foram dimensões|
|jobs.listJobs.success|Chamadas bem-sucedidas para tarefas de lista|Contagem|Total|A contagem de todas as chamadas bem-sucedidas para tarefas de lista.|Não foram dimensões|
|jobs.listJobs.failure|Falha de chamadas para tarefas de lista|Contagem|Total|A contagem de todas as chamadas de falha para tarefas de lista.|Não foram dimensões|
|jobs.cancelJob.success|Cancelamentos de tarefas com êxito|Contagem|Total|A contagem de todas as chamadas bem-sucedidas para cancelar uma tarefa.|Não foram dimensões|
|jobs.cancelJob.failure|Cancelamentos de tarefas com falhas|Contagem|Total|A contagem de todas as chamadas de falha para cancelar uma tarefa.|Não foram dimensões|
|jobs.queryJobs.success|Consultas de tarefa com êxito|Contagem|Total|A contagem de todas as chamadas bem-sucedidas para tarefas de consulta.|Não foram dimensões|
|jobs.queryJobs.failure|Falha na tarefa consulta|Contagem|Total|A contagem de todas as chamadas de falha para tarefas de consulta.|Não foram dimensões|
|Jobs.completed|Trabalhos concluídos|Contagem|Total|A contagem de todas as tarefas de conclusão.|Não foram dimensões|
|Jobs.Failed|Tarefas falhadas|Contagem|Total|A contagem de todas as tarefas falhadas.|Não foram dimensões|
|d2c.telemetry.Ingress.sendThrottle|Número de erros de limitação|Contagem|Total|Acelera o número de erros limitação devido ao débito do dispositivo|Não foram dimensões|
|dailyMessageQuotaUsed|Número total de mensagens utilizado|Contagem|Média|Número total de mensagens utilizada hoje em dia. Este é um valor de cumulativo é reposto para zero no 00:00 UTC todos os dias.|Não foram dimensões|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|RegistrationAttempts|Tentativas de registo|Contagem|Total|Número de registos de dispositivo tentada|Estado de ProvisioningServiceName, IotHubName,|
|DeviceAssignments|Dispositivos atribuídos|Contagem|Total|Número de dispositivos atribuídos a um IoT hub|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Tentativas de atestado|Contagem|Total|Número de tentativa de attestations de dispositivo|Protocolo ProvisioningServiceName, estado,|

## <a name="microsoftdeviceselasticpools"></a>Microsoft.Devices/ElasticPools

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|elasticPool.requestedUsageRate|taxa de utilização de pedido|Percentagem|Média|taxa de utilização de pedido|Não foram dimensões|

## <a name="microsoftdeviceselasticpoolsiothubtenants"></a>Microsoft.Devices/ElasticPools/IotHubTenants

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|tenantHub.requestedUsageRate|taxa de utilização de pedido|Percentagem|Média|taxa de utilização de pedido|Não foram dimensões|
|deviceDataUsage|Utilização de total devicedata|Contagem|Total|Bytes transferidos para e de quaisquer dispositivos ligados ao IotHub|Não foram dimensões|
|d2c.telemetry.Ingress.allProtocol|Tentativas de envio de mensagem de telemetria|Contagem|Total|Número de mensagens de telemetria do dispositivo para nuvem tentada a enviar ao seu IoT hub|Não foram dimensões|
|d2c.telemetry.Ingress.Success|Mensagens de telemetria enviadas|Contagem|Total|Número de mensagens de telemetria do dispositivo para nuvem enviadas com êxito ao seu IoT hub|Não foram dimensões|
|c2d.Commands.egress.Complete.Success|Comandos concluídos|Contagem|Total|Número de comandos da nuvem para o dispositivo foi concluída com êxito pelo dispositivo|Não foram dimensões|
|c2d.Commands.egress.Abandon.Success|Comandos abandonados|Contagem|Total|Número de comandos da nuvem para o dispositivo abandonadas pelo dispositivo|Não foram dimensões|
|c2d.Commands.egress.Reject.Success|Comandos rejeitados|Contagem|Total|Número de comandos da nuvem para o dispositivo rejeitado pelo dispositivo|Não foram dimensões|
|devices.totalDevices|Total de dispositivos|Contagem|Total|Número de dispositivos registados ao seu IoT hub|Não foram dimensões|
|devices.connectedDevices.allProtocol|Dispositivos ligados|Contagem|Total|Número de dispositivos ligados ao seu IoT hub|Não foram dimensões|
|d2c.telemetry.egress.Success|Mensagens de telemetria entregues|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito para os pontos finais (total)|Não foram dimensões|
|d2c.telemetry.egress.dropped|Mensagens de ignorados|Contagem|Total|Número de mensagens removida porque o ponto final de entrega foi Inativos|Não foram dimensões|
|d2c.telemetry.egress.orphaned|Mensagens órfãos|Contagem|Total|O número de mensagens não corresponde a qualquer rotas, incluindo a rota de contingência|Não foram dimensões|
|d2c.telemetry.egress.Invalid|Mensagens inválidas|Contagem|Total|A contagem de mensagens não entregues devido a incompatibilidade com o ponto final|Não foram dimensões|
|d2c.telemetry.egress.fallback|Mensagens de correspondência de condição de contingência|Contagem|Total|Número de mensagens escritas para o ponto final de contingência|Não foram dimensões|
|d2c.Endpoints.egress.eventHubs|Mensagens entregues nos pontos finais de Hub de eventos|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de Hub de eventos|Não foram dimensões|
|d2c.Endpoints.latency.eventHubs|Latência de mensagem para pontos finais de Hub de eventos|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para um ponto final de Hub de eventos, em milissegundos|Não foram dimensões|
|d2c.Endpoints.egress.serviceBusQueues|Mensagens entregues nos pontos finais de fila do Service Bus|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de fila do Service Bus|Não foram dimensões|
|d2c.Endpoints.latency.serviceBusQueues|Latência de mensagem para pontos finais de fila do Service Bus|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para um ponto final de fila do Service Bus, em milissegundos|Não foram dimensões|
|d2c.Endpoints.egress.serviceBusTopics|Mensagens entregues nos pontos finais de tópico de barramento de serviço|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de tópico de barramento de serviço|Não foram dimensões|
|d2c.Endpoints.latency.serviceBusTopics|Latência de mensagem para pontos finais de tópico de barramento de serviço|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para um ponto final de tópico de barramento de serviço, em milissegundos|Não foram dimensões|
|d2c.Endpoints.egress.builtIn.Events|Mensagens entregues para o ponto final incorporado (mensagens/eventos)|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito para o ponto final incorporado (mensagens/eventos)|Não foram dimensões|
|d2c.Endpoints.latency.builtIn.Events|Latência de mensagem para o ponto final incorporado (mensagens/eventos)|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para o ponto final incorporado (mensagens/eventos), em milissegundos |Não foram dimensões|
|d2c.Endpoints.egress.Storage|Mensagens entregues nos pontos finais de armazenamento|Contagem|Total|Número de vezes que as mensagens foram escritas com êxito aos pontos finais de armazenamento|Não foram dimensões|
|d2c.Endpoints.latency.Storage|Latência de mensagem para pontos finais de armazenamento|Milissegundos|Média|A latência média entre a entrada de mensagem para o IoT hub e a entrada de mensagem para um ponto final de armazenamento, em milissegundos|Não foram dimensões|
|d2c.Endpoints.egress.Storage.bytes|Dados escritos para armazenamento|Bytes|Total|Quantidade de dados, em bytes, escritos para pontos finais de armazenamento|Não foram dimensões|
|d2c.Endpoints.egress.Storage.BLOBs|Escrito para o armazenamento de BLOBs|Contagem|Total|Número de escritas para pontos finais de armazenamento de blobs|Não foram dimensões|
|d2c.Twin.Read.Success|Leituras de duplo bem-sucedida dos dispositivos|Contagem|Total|A contagem de leituras de iniciadas por dispositivo duplo todos os com êxito.|Não foram dimensões|
|d2c.Twin.Read.Failure|Não foi possível duplo leituras dos dispositivos|Contagem|Total|A contagem de todas as falha leituras iniciadas por dispositivo duplo.|Não foram dimensões|
|d2c.Twin.Read.size|Tamanho da resposta de duplo lê a partir de dispositivos|Bytes|Média|A média, mínimo e máximo de todos os com êxito iniciadas por dispositivo duplo leituras.|Não foram dimensões|
|d2c.Twin.Update.Success|Atualizações de duplo com êxito a partir de dispositivos|Contagem|Total|A contagem das atualizações de iniciadas por dispositivo duplo todos os com êxito.|Não foram dimensões|
|d2c.Twin.Update.Failure|Falha de atualizações de duplo a partir de dispositivos|Contagem|Total|A contagem de todas as falha atualizações iniciadas por dispositivo duplo.|Não foram dimensões|
|d2c.Twin.Update.size|Tamanho das atualizações de duplo a partir de dispositivos|Bytes|Média|A média, min e o tamanho máximo de todos os com êxito iniciadas por dispositivo duplo atualizações.|Não foram dimensões|
|c2d.methods.Success|Invocações de método direto com êxito|Contagem|Total|A contagem de chamadas de método direto todos os com êxito.|Não foram dimensões|
|c2d.methods.Failure|Não foi possível invocações de método direto|Contagem|Total|A contagem de todas as falha chamadas de método direto.|Não foram dimensões|
|c2d.methods.requestSize|Tamanho do pedido de invocações de método direto|Bytes|Média|A média, mínimo e máximo de todos os com êxito direcionam os pedidos de método.|Não foram dimensões|
|c2d.methods.responseSize|Tamanho da resposta de invocações de método direto|Bytes|Média|A média, mínimo e máximo de respostas de método direto todos os com êxito.|Não foram dimensões|
|c2d.Twin.Read.Success|Leituras de duplo com êxito de back-end|Contagem|Total|A contagem de leituras de back-end-iniciada duplo todos os com êxito.|Não foram dimensões|
|c2d.Twin.Read.Failure|Leituras de duplo falhada de back-end|Contagem|Total|A contagem de todas as falha leituras duplo iniciada em back-end.|Não foram dimensões|
|c2d.Twin.Read.size|Tamanho da resposta de duplo lê a partir do back-end|Bytes|Média|O média, mínimo e máximo de todos os com êxito iniciada em back-end duplo leituras.|Não foram dimensões|
|c2d.Twin.Update.Success|Atualizações de duplo com êxito a partir de back-end|Contagem|Total|A contagem das atualizações de back-end-iniciada duplo todos os com êxito.|Não foram dimensões|
|c2d.Twin.Update.Failure|Atualizações de duplo falhada a partir de back-end|Contagem|Total|A contagem de todas as falha atualizações duplo iniciada em back-end.|Não foram dimensões|
|c2d.Twin.Update.size|Tamanho de atualizações de duplo do back-end|Bytes|Média|A média, min e o tamanho máximo de todos os com êxito iniciada em back-end duplo atualizações.|Não foram dimensões|
|twinQueries.success|Consultas de duplo com êxito|Contagem|Total|A contagem de todas as consultas de duplo com êxito.|Não foram dimensões|
|twinQueries.failure|Consultas de duplo falhada|Contagem|Total|A contagem de todas as consultas de duplo falhada.|Não foram dimensões|
|twinQueries.resultSize|Tamanho dos resultados consultas duplo|Bytes|Média|A média, mínimo e máximo, o tamanho dos resultados de todas as consultas de duplo com êxito.|Não foram dimensões|
|jobs.createTwinUpdateJob.success|Criações com êxito das tarefas de atualização de duplo|Contagem|Total|A contagem de todos os criação com êxito das tarefas de atualização de duplo.|Não foram dimensões|
|jobs.createTwinUpdateJob.failure|Falha de criações de tarefas de atualização de duplo|Contagem|Total|A contagem de todos os falha na criação de tarefas de atualização de duplo.|Não foram dimensões|
|jobs.createDirectMethodJob.success|Criações com êxito das tarefas de invocação do método|Contagem|Total|A contagem de todos os criação com êxito das tarefas de invocação do método direto.|Não foram dimensões|
|jobs.createDirectMethodJob.failure|Falha de criações de tarefas de invocação do método|Contagem|Total|A contagem de todos os falha na criação de tarefas de invocação do método direto.|Não foram dimensões|
|jobs.listJobs.success|Chamadas bem-sucedidas para tarefas de lista|Contagem|Total|A contagem de todas as chamadas bem-sucedidas para tarefas de lista.|Não foram dimensões|
|jobs.listJobs.failure|Falha de chamadas para tarefas de lista|Contagem|Total|A contagem de todas as chamadas de falha para tarefas de lista.|Não foram dimensões|
|jobs.cancelJob.success|Cancelamentos de tarefas com êxito|Contagem|Total|A contagem de todas as chamadas bem-sucedidas para cancelar uma tarefa.|Não foram dimensões|
|jobs.cancelJob.failure|Cancelamentos de tarefas com falhas|Contagem|Total|A contagem de todas as chamadas de falha para cancelar uma tarefa.|Não foram dimensões|
|jobs.queryJobs.success|Consultas de tarefa com êxito|Contagem|Total|A contagem de todas as chamadas bem-sucedidas para tarefas de consulta.|Não foram dimensões|
|jobs.queryJobs.failure|Falha na tarefa consulta|Contagem|Total|A contagem de todas as chamadas de falha para tarefas de consulta.|Não foram dimensões|
|Jobs.completed|Trabalhos concluídos|Contagem|Total|A contagem de todas as tarefas de conclusão.|Não foram dimensões|
|Jobs.Failed|Tarefas falhadas|Contagem|Total|A contagem de todas as tarefas falhadas.|Não foram dimensões|
|d2c.telemetry.Ingress.sendThrottle|Número de erros de limitação|Contagem|Total|Acelera o número de erros limitação devido ao débito do dispositivo|Não foram dimensões|
|dailyMessageQuotaUsed|Número total de mensagens utilizado|Contagem|Média|Número total de mensagens utilizada hoje em dia. Este é um valor de cumulativo é reposto para zero no 00:00 UTC todos os dias.|Não foram dimensões|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|MetadataRequests|Pedidos de metadados|Contagem|Contagem|Contagem de pedidos de metadados. BD do cosmos mantém a coleção de metadados do sistema para cada conta, que permite-lhe enumerar coleções, as bases de dados, etc., e as respetivas configurações, gratuitamente.|GlobalDatabaseAccountName, DatabaseName, CollectionName, região, StatusCode|
|MongoRequestCharge|Encargos de pedido do mongo|Contagem|Total|Unidades de pedido do mongo consumidas|GlobalDatabaseAccountName, DatabaseName, CollectionName, região, CommandName, código de erro|
|MongoRequests|Pedidos de mongo|Contagem|Contagem|Número de pedidos do Mongo efetuados|GlobalDatabaseAccountName, DatabaseName, CollectionName, região, CommandName, código de erro|
|TotalRequestUnits|Unidades de pedido total|Contagem|Total|Consumido de unidades de pedido|GlobalDatabaseAccountName, DatabaseName, CollectionName, região, StatusCode|
|TotalRequests|Total de Pedidos|Contagem|Contagem|Número de pedidos efetuados|GlobalDatabaseAccountName, DatabaseName, CollectionName, região, StatusCode|


## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|SuccessfulRequests|Pedidos com êxito (pré-visualização)|Contagem|Total|Pedidos Com Êxito do Microsoft.EventHub. (Pré-visualização)|EntityName|
|ServerErrors|Erros de Servidor. (Pré-visualização)|Contagem|Total|Erros de Servidor do Microsoft.EventHub. (Pré-visualização)|EntityName|
|UserErrors|Erros do Utilizador. (Pré-visualização)|Contagem|Total|Erros de Utilizador do Microsoft.EventHub. (Pré-visualização)|EntityName|
|QuotaExceededErrors|Erros de Exceção de Quota. (Pré-visualização)|Contagem|Total|Erros de Quota Excedida do Microsoft.EventHub. (Pré-visualização)|EntityName|
|ThrottledRequests|Pedidos Limitados. (Pré-visualização)|Contagem|Total|Pedidos Limitados do Microsoft.EventHub. (Pré-visualização)|EntityName|
|IncomingRequests|Pedidos de entrada (pré-visualização)|Contagem|Total|Pedidos Recebidos do Microsoft.EventHub. (Pré-visualização)|EntityName|
|IncomingMessages|Mensagens a receber (pré-visualização)|Contagem|Total|Mensagens Recebidas do Microsoft.EventHub. (Pré-visualização)|EntityName|
|OutgoingMessages|Mensagens de saída (pré-visualização)|Contagem|Total|Mensagens Enviadas do Microsoft.EventHub. (Pré-visualização)|EntityName|
|IncomingBytes|Bytes Recebidos. (Pré-visualização)|Bytes|Total|Bytes Recebidos do Microsoft.EventHub. (Pré-visualização)|EntityName|
|OutgoingBytes|Bytes Enviados. (Pré-visualização)|Bytes|Total|Bytes Enviados do Microsoft.EventHub. (Pré-visualização)|EntityName|
|ActiveConnections|ActiveConnections (pré-visualização)|Contagem|Total|Total de Ligações Ativas do Microsoft.EventHub. (Pré-visualização)|EntityName|
|ConnectionsOpened|Ligações Abertas. (Pré-visualização)|Contagem|Total|Ligações Abertas do Microsoft.EventHub. (Pré-visualização)|EntityName|
|ConnectionsClosed|Ligações Fechadas. (Pré-visualização)|Contagem|Total|Ligações Fechadas do Microsoft.EventHub. (Pré-visualização)|EntityName|
|CaptureBacklog|Registo de tarefas pendentes de Captura. (Pré-visualização)|Contagem|Total|Registo de tarefas pendentes de captura do Microsoft.EventHub. (Pré-visualização)|EntityName|
|CapturedMessages|Mensagens Capturadas. (Pré-visualização)|Contagem|Total|Mensagens Capturadas do Microsoft.EventHub. (Pré-visualização)|EntityName|
|CapturedBytes|Bytes Capturados. (Pré-visualização)|Bytes|Total|Bytes Capturadas do Microsoft.EventHub. (Pré-visualização)|EntityName|
|INREQS|Pedidos Recebidos|Contagem|Total|Total de pedidos de entrada enviados para um espaço de nomes|Não foram dimensões|
|SUCCREQ|Pedidos com Êxito|Contagem|Total|Total de pedidos com êxito para um espaço de nomes|Não foram dimensões|
|FAILREQ|Pedidos Falhados|Contagem|Total|Total de pedidos falhados para um espaço de nomes|Não foram dimensões|
|SVRBSY|Erros de Servidor Ocupado|Contagem|Total|Total de erros de servidor ocupado para um espaço de nomes|Não foram dimensões|
|INTERR|Erros Internos de Servidor|Contagem|Total|Total de erros de servidor internos para um espaço de nomes|Não foram dimensões|
|MISCERR|Outros Erros|Contagem|Total|Total de pedidos falhados para um espaço de nomes|Não foram dimensões|
|INMSGS|Mensagens Recebidas (preteridas)|Contagem|Total|Total de mensagens de entrada para um espaço de nomes. Esta métrica foi preterida. Em vez disso, utilize a métrica de mensagens a receber|Não foram dimensões|
|EHINMSGS|Mensagens Recebidas|Contagem|Total|Total de mensagens a receber para um espaço de nomes|Não foram dimensões|
|OUTMSGS|Mensagens Enviadas (preteridas)|Contagem|Total|Total de envio de mensagens para um espaço de nomes. Esta métrica foi preterida. Em vez disso, utilize métrica de mensagens a enviar|Não foram dimensões|
|EHOUTMSGS|Mensagens Enviadas|Contagem|Total|Total de mensagens a enviar para um espaço de nomes|Não foram dimensões|
|EHINMBS|Bytes recebidos (preteridos)|Bytes|Total|Evento Hub receber mensagens débito para um espaço de nomes. Esta métrica foi preterida. Em vez disso, utilize métrica de bytes de entrada|Não foram dimensões|
|EHINBYTES|Bytes recebidos|Bytes|Total|Débito de mensagem a receber do Hub de Eventos para um espaço de nomes|Não foram dimensões|
|EHOUTMBS|Bytes Enviados (preteridos)|Bytes|Total|Evento Hub enviar mensagem débito para um espaço de nomes. Esta métrica foi preterida. Em vez disso, utilize métrica de bytes de enviar|Não foram dimensões|
|EHOUTBYTES|Bytes enviados|Bytes|Total|Débito de mensagens a enviar do Hub de Eventos para um espaço de nomes|Não foram dimensões|
|EHABL|Arquivar mensagens de registo de tarefas pendentes|Contagem|Total|Mensagens de arquivo do Hub de Eventos em registo de tarefas pendentes para um espaço de nomes|Não foram dimensões|
|EHAMSGS|Arquivar mensagens|Contagem|Total|Mensagens de arquivo do Hub de Eventos num espaço de nomes|Não foram dimensões|
|EHAMBS|Arquivar débito de mensagens|Bytes|Total|Débito de mensagem de arquivo do Hub de Eventos num espaço de nomes|Não foram dimensões|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|ObservedMetricValue|Valor de Métrica observado|Contagem|Média|O valor calculador pelo dimensionamento automático quando este foi executado|MetricTriggerSource|
|MetricThreshold|Limiar de Métrica|Contagem|Média|O limiar de dimensionamento automático configurado quando este foi executado.|MetricTriggerRule|
|ObservedCapacity|Capacidade observada|Contagem|Média|A capacidade reportada para dimensionamento automático quando este foi executado.|Não foram dimensões|
|ScaleActionsInitiated|Ações de Dimensionamento Iniciadas|Contagem|Total|A direção da operação de dimensionamento.|ScaleDirection|

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|Latência|Latência|Milissegundos|Média|Duração das chamadas de API|OperationName, OperationResult|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|RunsStarted|Execuções Iniciadas|Contagem|Total|Número de execuções de fluxo de trabalho iniciadas.|Não foram dimensões|
|RunsCompleted|Execuções Concluídas|Contagem|Total|Número de execuções de fluxo de trabalho concluídas.|Não foram dimensões|
|RunsSucceeded|Execuções com Êxito|Contagem|Total|Número de execuções de fluxo de trabalho com êxito.|Não foram dimensões|
|RunsFailed|Execuções Falhadas|Contagem|Total|Número de execuções de fluxo de trabalho falhadas.|Não foram dimensões|
|RunsCancelled|Execuções Canceladas|Contagem|Total|Número de execuções de fluxo de trabalho canceladas.|Não foram dimensões|
|RunLatency|Latência de Execução|Segundos|Média|Latência de execuções de fluxo de trabalho concluídas.|Não foram dimensões|
|RunSuccessLatency|Latência de Execução com Êxito|Segundos|Média|Latência de execuções de fluxo de trabalho com êxito.|Não foram dimensões|
|RunThrottledEvents|Eventos Limitados em relação à Execução|Contagem|Total|Número de eventos limitados em relação à ação ou acionador de fluxo de trabalho.|Não foram dimensões|
|RunFailurePercentage|Percentagem da Falha de Execução|Percentagem|Total|Percentagem das execuções de fluxo de trabalho falhadas.|Não foram dimensões|
|ActionsStarted|Ações Iniciadas |Contagem|Total|Número de ações de fluxo de trabalho iniciadas.|Não foram dimensões|
|ActionsCompleted|Ações Concluídas |Contagem|Total|Número de ações de fluxo de trabalho concluídas.|Não foram dimensões|
|ActionsSucceeded|Ações com Êxito |Contagem|Total|Número de ações de fluxo de trabalho com êxito.|Não foram dimensões|
|ActionsFailed|Ações Falhadas|Contagem|Total|Número de ações de fluxo de trabalho falhadas.|Não foram dimensões|
|ActionsSkipped|Ações Ignoradas |Contagem|Total|Número de ações de fluxo de trabalho ignoradas.|Não foram dimensões|
|ActionLatency|Latência da Ação |Segundos|Média|Latência das ações de fluxo de trabalho concluídas.|Não foram dimensões|
|ActionSuccessLatency|Latência de Ações com Êxito |Segundos|Média|Latência de ações de fluxo de trabalho com êxito.|Não foram dimensões|
|ActionThrottledEvents|Eventos Limitados em relação a Ação|Contagem|Total|Número de eventos limitados em relação à ação de fluxo de trabalho.|Não foram dimensões|
|TriggersStarted|Acionadores Iniciados |Contagem|Total|Número de acionadores de fluxo de trabalho iniciados.|Não foram dimensões|
|TriggersCompleted|Acionadores Concluídos |Contagem|Total|Número de acionadores de fluxo de trabalho concluídos.|Não foram dimensões|
|TriggersSucceeded|Acionadores Com Êxito |Contagem|Total|Número de acionadores de fluxo de trabalho com êxito.|Não foram dimensões|
|TriggersFailed|Acionadores Com Falha |Contagem|Total|Número de acionadores de fluxo de trabalho falhados.|Não foram dimensões|
|TriggersSkipped|Acionadores Ignorados|Contagem|Total|Número de acionadores de fluxo de trabalho ignorados.|Não foram dimensões|
|TriggersFired|Acionadores Disparados |Contagem|Total|Número de acionadores de fluxo de trabalho desencadeados.|Não foram dimensões|
|TriggerLatency|Latência de Acionador |Segundos|Média|Latência de acionadores de fluxo de trabalho concluídos.|Não foram dimensões|
|TriggerFireLatency|Latência de Disparo de Acionador |Segundos|Média|Latência de acionadores de fluxo de trabalho desencadeados.|Não foram dimensões|
|TriggerSuccessLatency|Latência de Êxito de Acionador |Segundos|Média|Latência de acionadores de fluxo de trabalho com êxito.|Não foram dimensões|
|TriggerThrottledEvents|Eventos Limitados de Acionador|Contagem|Total|Número de eventos limitados em relação ao acionador de fluxo de trabalho.|Não foram dimensões|
|BillableActionExecutions|Execuções de Ação a Cobrar|Contagem|Total|Número de execuções de ação de fluxo de trabalho a serem faturadas.|Não foram dimensões|
|BillableTriggerExecutions|Execuções de Acionador a Cobrar|Contagem|Total|Número de execuções do acionador de fluxo de trabalho a serem faturadas.|Não foram dimensões|
|TotalBillableExecutions|Total de Execuções a Cobrar|Contagem|Total|Número de execuções de fluxo de trabalho a serem faturadas.|Não foram dimensões|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|BytesSentRate|Bytes enviados|Contagem|Total|Número de bytes enviados a Interface de rede|Não foram dimensões|
|BytesReceivedRate|Bytes recebidos|Contagem|Total|Número de bytes a Interface de rede recebido|Não foram dimensões|
|PacketsSentRate|Pacotes enviados|Contagem|Total|Número de pacotes enviados a Interface de rede|Não foram dimensões|
|PacketsReceivedRate|Pacotes recebidos|Contagem|Total|Número de pacotes da Interface de rede recebido|Não foram dimensões|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|VipAvailability|Disponibilidade de VIP|Contagem|Média|Disponibilidade de pontos finais de VIP, com base nos resultados de pesquisa|VipAddress, VipPort|
|DipAvailability|Disponibilidade do DIP|Contagem|Média|Disponibilidade de pontos finais DIP, com base nos resultados de pesquisa|ProtocolType DipPort, VipAddress, VipPort, DipAddress|
|ByteCount|Contagem de bytes|Contagem|Total|Número total de Bytes transmitidos num período de tempo|Direção VipAddress, VipPort,|
|PacketCount|Contagem de pacotes|Contagem|Total|Número total de pacotes transmitidos num período de tempo|Direção VipAddress, VipPort,|
|SYNCount|Contagem de SIN|Contagem|Total|Número total de pacotes de SIN transmitidos num período de tempo|Direção VipAddress, VipPort,|
|SnatConnectionCount|Total de ligações de realizar o SNAT|Contagem|Total|Número total de novas ligações de realizar o SNAT criados num período de tempo|ConnectionState VipAddress, DipAddress,|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|PacketsInDDoS|Pacotes de entrada DDoS|CountPerSecond|Máximo|Pacotes de entrada DDoS|Não foram dimensões|
|PacketsDroppedDDoS|Pacotes de entrada ignorados DDoS|CountPerSecond|Máximo|Pacotes de entrada ignorados DDoS|Não foram dimensões|
|PacketsForwardedDDoS|Pacotes de entrada reencaminhados DDoS|CountPerSecond|Máximo|Pacotes de entrada reencaminhados DDoS|Não foram dimensões|
|TCPPacketsInDDoS|Pacotes de entrada de TCP DDoS|CountPerSecond|Máximo|Pacotes de entrada de TCP DDoS|Não foram dimensões|
|TCPPacketsDroppedDDoS|Entrada de TCP de pacotes ignorados DDoS|CountPerSecond|Máximo|Entrada de TCP de pacotes ignorados DDoS|Não foram dimensões|
|TCPPacketsForwardedDDoS|Entrada TCP os pacotes reencaminhados DDoS|CountPerSecond|Máximo|Entrada TCP os pacotes reencaminhados DDoS|Não foram dimensões|
|UDPPacketsInDDoS|Pacotes de entrada UDP DDoS|CountPerSecond|Máximo|Pacotes de entrada UDP DDoS|Não foram dimensões|
|UDPPacketsDroppedDDoS|Entrada de UDP de pacotes ignorados DDoS|CountPerSecond|Máximo|Entrada de UDP de pacotes ignorados DDoS|Não foram dimensões|
|UDPPacketsForwardedDDoS|Entrada UDP os pacotes reencaminhados DDoS|CountPerSecond|Máximo|Entrada UDP os pacotes reencaminhados DDoS|Não foram dimensões|
|BytesInDDoS|Bytes de entrada DDoS|BytesPerSecond|Máximo|Bytes de entrada DDoS|Não foram dimensões|
|BytesDroppedDDoS|Bytes de entrada ignorados DDoS|BytesPerSecond|Máximo|Bytes de entrada ignorados DDoS|Não foram dimensões|
|BytesForwardedDDoS|Bytes de entrada reencaminhados DDoS|BytesPerSecond|Máximo|Bytes de entrada reencaminhados DDoS|Não foram dimensões|
|TCPBytesInDDoS|Bytes de entrada TCP DDoS|BytesPerSecond|Máximo|Bytes de entrada TCP DDoS|Não foram dimensões|
|TCPBytesDroppedDDoS|Bytes TCP entradas ignorados DDoS|BytesPerSecond|Máximo|Bytes TCP entradas ignorados DDoS|Não foram dimensões|
|TCPBytesForwardedDDoS|Bytes TCP entradas reencaminhados DDoS|BytesPerSecond|Máximo|Bytes TCP entradas reencaminhados DDoS|Não foram dimensões|
|UDPBytesInDDoS|Bytes de entrada UDP DDoS|BytesPerSecond|Máximo|Bytes de entrada UDP DDoS|Não foram dimensões|
|UDPBytesDroppedDDoS|Bytes UDP entradas ignorados DDoS|BytesPerSecond|Máximo|Bytes UDP entradas ignorados DDoS|Não foram dimensões|
|UDPBytesForwardedDDoS|Bytes UDP entradas reencaminhados DDoS|BytesPerSecond|Máximo|Bytes UDP entradas reencaminhados DDoS|Não foram dimensões|
|IfUnderDDoSAttack|Em DDoS ataques ou não|Contagem|Máximo|Em DDoS ataques ou não|Não foram dimensões|
|DDoSTriggerTCPPackets|Pacotes TCP para acionar DDoS mitigação de entrada|CountPerSecond|Máximo|Pacotes TCP para acionar DDoS mitigação de entrada|Não foram dimensões|
|DDoSTriggerUDPPackets|Pacotes UDP para acionar DDoS mitigação de entrada|CountPerSecond|Máximo|Pacotes UDP para acionar DDoS mitigação de entrada|Não foram dimensões|
|DDoSTriggerSYNPackets|SIN pacotes para acionar DDoS mitigação de entrada|CountPerSecond|Máximo|SIN pacotes para acionar DDoS mitigação de entrada|Não foram dimensões|
|VipAvailability|Disponibilidade|Contagem|Média|Disponibilidade média de IPAddress num período de tempo|Porta|
|ByteCount|Contagem de bytes|Contagem|Total|Número total de Bytes transmitidos num período de tempo|Porta de direção|
|PacketCount|Contagem de pacotes|Contagem|Total|Número total de pacotes transmitidos num período de tempo|Porta de direção|
|SynCount|Contagem de SIN|Contagem|Total|Número total de pacotes de SIN transmitidos num período de tempo|Porta de direção|

## <a name="microsoftnetworkvirtualnetworks"></a>Network/virtualnetworks

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|PacketsInDroppedVMProtection|Pacotes de entrada ignorados para proteção de VM|CountPerSecond|Média|Pacotes de entrada ignorados para proteção de VM|Não foram dimensões|
|PacketsOutDroppedVMProtection|Pacotes de saída ignorados para proteção de VM|CountPerSecond|Média|Pacotes de saída ignorados para proteção de VM|Não foram dimensões|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|Débito|Débito|BytesPerSecond|Total|Número de bytes por segundo que foi servido o Gateway de aplicação|Não foram dimensões|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Largura de banda de túnel|BytesPerSecond|Média|Largura de banda média de um túnel de bytes por segundo|ConnectionName, RemoteIP|
|TunnelEgressBytes|Bytes de saída de túnel|Bytes|Total|Bytes de saída de um túnel|ConnectionName, RemoteIP|
|TunnelIngressBytes|Bytes de entrada de túnel|Bytes|Total|Bytes recebidos de um túnel|ConnectionName, RemoteIP|
|TunnelEgressPackets|Pacotes de saída de túnel|Contagem|Total|Contagem de pacotes de saída de um túnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|Pacotes de entrada de túnel|Contagem|Total|Contagem de pacotes de entrada de um túnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Túnel saída TS erro de correspondência de pacotes ignorados|Contagem|Total|Contagem de entrega de pacotes de saída de erro de correspondência de Seletor de tráfego de um túnel|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Túnel entrada TS erro de correspondência de pacotes ignorados|Contagem|Total|Contagem de lista de pacotes de entrada de erro de correspondência de Seletor de tráfego de um túnel|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Média|Bits ingressing do Azure por segundo|Não foram dimensões|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Média|Bits egressing do Azure por segundo|Não foram dimensões|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|QpsByEndpoint|Consultas pelo ponto final devolvido|Contagem|Total|Número de vezes que um ponto final do Gestor de tráfego foi devolvido num determinado período de tempo|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Ponto final estado pelo ponto final|Contagem|Máximo|1 se o estado de sonda de um ponto final é "ativado", 0, caso contrário.|EndpointName|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|Registration.all|Operações de Registo|Contagem|Total|Contagem de todas as operações de registo com êxito (consultas e eliminações de atualizações de criações). |Não foram dimensões|
|Registration.Create|Operações de Criação de Registo|Contagem|Total|Contagem de todas as criações de registo com êxito.|Não foram dimensões|
|Registration.Update|Operações de Atualização de Registo|Contagem|Total|Contagem de todas as atualizações de registo com êxito.|Não foram dimensões|
|Registration.Get|Operações de Leitura de Registo|Contagem|Total|Contagem de todas as consultas de registo com êxito.|Não foram dimensões|
|Registration.delete|Operações de Eliminação de Registo|Contagem|Total|Contagem de todas as eliminações de registo com êxito.|Não foram dimensões|
|entrada|Mensagens Recebidas|Contagem|Total|Contagem de todas as chamadas à API enviadas com êxito. |Não foram dimensões|
|incoming.Scheduled|Notificações Push Agendadas Enviadas|Contagem|Total|Notificações Push Agendadas Canceladas|Não foram dimensões|
|incoming.Scheduled.Cancel|Notificações Push Agendadas Canceladas|Contagem|Total|Notificações Push Agendadas Canceladas|Não foram dimensões|
|Scheduled.Pending|Notificações Agendadas Pendentes|Contagem|Total|Notificações Agendadas Pendentes|Não foram dimensões|
|Installation.all|Operações de Gestão de Instalação|Contagem|Total|Operações de Gestão de Instalação|Não foram dimensões|
|Installation.Get|Obter Operações de Instalação|Contagem|Total|Obter Operações de Instalação|Não foram dimensões|
|Installation.upsert|Criar ou Atualizar Operações de Instalação|Contagem|Total|Criar ou Atualizar Operações de Instalação|Não foram dimensões|
|Installation.patch|Corrigir Operações de Instalação|Contagem|Total|Corrigir Operações de Instalação|Não foram dimensões|
|Installation.delete|Eliminar Operações de Instalação|Contagem|Total|Eliminar Operações de Instalação|Não foram dimensões|
|Outgoing.allpns.Success|Notificações com êxito|Contagem|Total|Contagem de todas as notificações com êxito.|Não foram dimensões|
|Outgoing.allpns.invalidpayload|Erros de Payload|Contagem|Total|Contagem de pushes falhados porque o PNS devolveu um erro de payload inválido.|Não foram dimensões|
|Outgoing.allpns.pnserror|Erros de Sistema de Notificação Externos|Contagem|Total|Contagem de pushes falhados porque ocorreu um problema ao comunicar com o PNS (excluindo problemas de autenticação).|Não foram dimensões|
|Outgoing.allpns.channelerror|Erros de Canal|Contagem|Total|Contagem de pushes falhados porque o canal era inválido, não estava associado à aplicação correta, sofreu limitações ou expirou.|Não foram dimensões|
|Outgoing.allpns.badorexpiredchannel|Erros de Canal Incorreto ou Expirado|Contagem|Total|Contagem de pushes falhados porque o canal/token/registrationId no registo expirou ou era inválido.|Não foram dimensões|
|Outgoing.WNS.Success|Notificações com Êxito do WNS|Contagem|Total|Contagem de todas as notificações com êxito.|Não foram dimensões|
|Outgoing.WNS.invalidcredentials|Erros de Autorização do WNS (Credenciais Inválidas)|Contagem|Total|Contagem de pushes falhados porque o PNS não aceitou as credenciais fornecidas ou porque as credenciais estão bloqueadas. (Windows Live não reconhece as credenciais).|Não foram dimensões|
|Outgoing.WNS.badchannel|Erro de Canal Incorreto do WNS|Contagem|Total|Contagem de pushes falhados porque o ChannelURI no registo não foi reconhecido (estado do WNS: 404 não encontrado).|Não foram dimensões|
|Outgoing.WNS.expiredchannel|Erro de Canal Expirado do WNS|Contagem|Total|Contagem de pushes falhados porque o ChannelURI expirou (estado do WNS: 410 Já não existe).|Não foram dimensões|
|Outgoing.WNS.throttled|Notificações Limitadas do WNS|Contagem|Total|Contagem de pushes falhados porque o WNS está a limitar esta aplicação (estado do WNS: 406 Não Aceitável).|Não foram dimensões|
|Outgoing.WNS.tokenproviderunreachable|Erros de Autorização do WNS (Inacessível)|Contagem|Total|O Windows Live não está acessível.|Não foram dimensões|
|Outgoing.WNS.invalidtoken|Erros de Autorização do WNS (Token Inválido)|Contagem|Total|O token fornecido para o WNS não é válido (estado do WNS: 401 Não Autorizado).|Não foram dimensões|
|Outgoing.WNS.wrongtoken|Erros de Autorização do WNS (Token Incorreto)|Contagem|Total|O token fornecido para WNS é válido, mas para outra aplicação (estado WNS: 403 Proibido). Isto pode acontecer se o ChannelURI no registo está associada a outra aplicação. Certifique-se de que a aplicação de cliente está associada à mesma aplicação cujas credenciais são no notification hub.|Não foram dimensões|
|Outgoing.WNS.invalidnotificationformat|Formato de Notificação Inválido do WNS|Contagem|Total|O formato da notificação é inválido (estado WNS: 400). Tenha em atenção que o WNS não rejeitar todos os payloads inválidos.|Não foram dimensões|
|Outgoing.WNS.invalidnotificationsize|Erro de Tamanho de Notificação Inválido do WNS|Contagem|Total|O payload de notificação é demasiado grande (estado do WNS: 413).|Não foram dimensões|
|Outgoing.WNS.channelthrottled|Canal do WNS Limitado|Contagem|Total|A notificação foi removida porque o ChannelURI do registo está limitado (cabeçalho de resposta do WNS: X-WNS-NotificationStatus:channelThrottled).|Não foram dimensões|
|Outgoing.WNS.channeldisconnected|Canal do WNS Desligado|Contagem|Total|A notificação foi removida porque o ChannelURI do registo está limitado (cabeçalho de resposta do WNS: X-WNS-DeviceConnectionStatus: desligado).|Não foram dimensões|
|Outgoing.WNS.dropped|Notificações Removidas do WNS|Contagem|Total|A notificação foi removida porque o ChannelURI do registo está limitado (X-WNS-NotificationStatus: removido mas não X-WNS-DeviceConnectionStatus: desligado).|Não foram dimensões|
|Outgoing.WNS.pnserror|Erros do WNS|Contagem|Total|As notificações não foram entregues devido a erros de comunicação com o WNS.|Não foram dimensões|
|Outgoing.WNS.authenticationerror|Erros de Autenticação do WNS|Contagem|Total|Notificação não entregue devido a erros de comunicação com o Windows Live, com credenciais inválidas ou token errado.|Não foram dimensões|
|Outgoing.APNs.Success|Notificações com Êxito do APNS|Contagem|Total|Contagem de todas as notificações com êxito.|Não foram dimensões|
|Outgoing.APNs.invalidcredentials|Erros de Autorização do APNS|Contagem|Total|Contagem de pushes falhados porque o PNS não aceitou as credenciais fornecidas ou porque as credenciais estão bloqueadas.|Não foram dimensões|
|Outgoing.APNs.badchannel|Erro de Canal Incorreto do APNS|Contagem|Total|Contagem de pushes falhados porque o token é inválido (código de estado do APNS: 8).|Não foram dimensões|
|Outgoing.APNs.expiredchannel|Erro de Canal Expirado do APNS|Contagem|Total|Contagem de tokens invalidados pelo canal de relatórios do APNS.|Não foram dimensões|
|Outgoing.APNs.invalidnotificationsize|Erro de Tamanho de Notificação Inválido do APNS|Contagem|Total|Contagem de pushes falhados porque o payload era demasiado grande (código de estado do APNS: 7).|Não foram dimensões|
|Outgoing.APNs.pnserror|Erros do APNS|Contagem|Total|Contagem de pushes falhados devido a erros de comunicação com o APNS.|Não foram dimensões|
|Outgoing.GCM.Success|Notificações com Êxito do GCM|Contagem|Total|Contagem de todas as notificações com êxito.|Não foram dimensões|
|Outgoing.GCM.invalidcredentials|Erros de Autorização do GCM (Credenciais Inválidas)|Contagem|Total|Contagem de pushes falhados porque o PNS não aceitou as credenciais fornecidas ou porque as credenciais estão bloqueadas.|Não foram dimensões|
|Outgoing.GCM.badchannel|Erro de Canal Incorreto do GCM|Contagem|Total|Contagem de pushes falhados porque o registrationId no registo não foi reconhecido (resultado do GCM: Registo Inválido).|Não foram dimensões|
|Outgoing.GCM.expiredchannel|Erro de Canal Expirado do GCM|Contagem|Total|Contagem de pushes falhados porque o registrationId no registo expirou (resultado do GCM: NotRegistered).|Não foram dimensões|
|Outgoing.GCM.throttled|Notificações Limitadas do GCM|Contagem|Total|Contagem de pushes falhados porque o GCM limitou esta aplicação (código de estado do GCM: 501-599 ou resultado: Indisponível).|Não foram dimensões|
|Outgoing.GCM.invalidnotificationformat|Formato de Notificação Inválido do GCM|Contagem|Total|Contagem de pushes falhados porque o payload não estava formatado corretamente (resultado do GCM: InvalidDataKey ou InvalidTtl).|Não foram dimensões|
|Outgoing.GCM.invalidnotificationsize|Erro de Tamanho de Notificação Inválido do GCM|Contagem|Total|Contagem de pushes falhados porque o payload era demasiado grande (resultado do GCM: MessageTooBig).|Não foram dimensões|
|Outgoing.GCM.wrongchannel|Erro de Canal Errado do GCM|Contagem|Total|Contagem de pushes falhados porque o registrationId no registo não está associado à aplicação atual (resultado do GCM: InvalidPackageName).|Não foram dimensões|
|Outgoing.GCM.pnserror|Erros do GCM|Contagem|Total|Contagem de pushes falhados devido a erros de comunicação com o GCM.|Não foram dimensões|
|Outgoing.GCM.authenticationerror|Erros de Autenticação do GCM|Contagem|Total|Contagem de pushes falhados porque o PNS não aceitou as credenciais fornecidas ou porque as credenciais estão bloqueadas ou o SenderId não está configurado corretamente na aplicação (resultado do GCM: MismatchedSenderId).|Não foram dimensões|
|Outgoing.mpns.Success|Notificações com Êxito do MPNS|Contagem|Total|Contagem de todas as notificações com êxito.|Não foram dimensões|
|Outgoing.mpns.invalidcredentials|Credenciais Inválidas do MPNS|Contagem|Total|Contagem de pushes falhados porque o PNS não aceitou as credenciais fornecidas ou porque as credenciais estão bloqueadas.|Não foram dimensões|
|Outgoing.mpns.badchannel|Erro de Canal Incorreto do MPNS|Contagem|Total|Contagem de pushes falhados porque o ChannelURI no registo não foi reconhecido (estado do MPNS: 404 não encontrado).|Não foram dimensões|
|Outgoing.mpns.throttled|Notificações Limitadas do MPNS|Contagem|Total|Contagem de pushes falhados porque o MPNS está a limitar esta aplicação (WNS MPNS: 406 Não Aceitável).|Não foram dimensões|
|Outgoing.mpns.invalidnotificationformat|Formato de Notificação Inválido do MPNS|Contagem|Total|Contagem de pushes falhados porque o payload da notificação era demasiado grande.|Não foram dimensões|
|Outgoing.mpns.channeldisconnected|Canal do MPNS Desligado|Contagem|Total|Contagem de pushes falhados porque o ChannelURI no registo foi desligado (estado do MPNS: 412 não encontrado).|Não foram dimensões|
|Outgoing.mpns.dropped|Notificações Removidas do MPNS|Contagem|Total|Contagem de pushes removidos pelo MPNS (cabeçalho de resposta do MPNS: X-NotificationStatus: QueueFull ou Suprimido).|Não foram dimensões|
|Outgoing.mpns.pnserror|Erros do MPNS|Contagem|Total|Contagem de pushes falhados devido a erros de comunicação com o MPNS.|Não foram dimensões|
|Outgoing.mpns.authenticationerror|Erros de Autenticação do MPNS|Contagem|Total|Contagem de pushes falhados porque o PNS não aceitou as credenciais fornecidas ou porque as credenciais estão bloqueadas.|Não foram dimensões|
|notificationhub.pushes|Todas as Notificações Enviadas|Contagem|Total|Todas as notificações enviadas do hub de notificações|Não foram dimensões|
|incoming.all.Requests|Todos os Pedidos Recebidos|Contagem|Total|Total de pedidos recebidos de um hub de notificações|Não foram dimensões|
|incoming.all.failedrequests|Todos os Pedidos Falhados Recebidos|Contagem|Total|Total de pedidos falhados recebidos de um hub de notificações|Não foram dimensões|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Contagem|Total|ListenerConnections com êxito do Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Contagem|Total|ClientError em ListenerConnections do Microsoft.Relay.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Contagem|Total|ServerError em ListenerConnections do Microsoft.Relay.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Contagem|Total|SenderConnections com êxito do Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Contagem|Total|ClientError em SenderConnections do Microsoft.Relay.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Contagem|Total|ServerError em SenderConnections do Microsoft.Relay.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Contagem|Total|ListenerConnections totais do Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Contagem|Total|Pedidos de SenderConnections totais do Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Contagem|Total|ActiveConnections totais do Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Contagem|Total|ActiveListeners totais do Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Contagem|Total|BytesTransferred totais do Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Contagem|Total|ListenerDisconnects totais do Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Contagem|Total|SenderDisconnects totais do Microsoft.Relay.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|SearchLatency|Latência de pesquisa|Segundos|Média|Latência média de pesquisa para o serviço de pesquisa|Não foram dimensões|
|SearchQueriesPerSecond|Pesquisar consultas por segundo|CountPerSecond|Média|Consultas de pesquisa por segundo para o serviço de pesquisa|Não foram dimensões|
|ThrottledSearchQueriesPercentage|Percentagem de pesquisas de consultas otimizadas|Percentagem|Média|Percentagem de consultas de pesquisa que foram limitadas para o serviço de pesquisa|Não foram dimensões|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|SuccessfulRequests|Pedidos com êxito (pré-visualização)|Contagem|Total|Total de pedidos com êxito para um espaço de nomes (pré-visualização)|EntityName|
|ServerErrors|Erros de Servidor. (Pré-visualização)|Contagem|Total|Erros de Servidor do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|UserErrors|Erros do Utilizador. (Pré-visualização)|Contagem|Total|Erros de Utilizador do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|ThrottledRequests|Pedidos Limitados. (Pré-visualização)|Contagem|Total|Pedidos Limitados do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|IncomingRequests|Pedidos de entrada (pré-visualização)|Contagem|Total|Pedidos Recebidos do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|IncomingMessages|Mensagens a receber (pré-visualização)|Contagem|Total|Mensagens Recebidas do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|OutgoingMessages|Mensagens de saída (pré-visualização)|Contagem|Total|Mensagens Enviadas do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|ActiveConnections|ActiveConnections (pré-visualização)|Contagem|Total|Total de Ligações Ativas do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|ConnectionsOpened|Ligações Abertas. (Pré-visualização)|Contagem|Total|Ligações Abertas do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|ConnectionsClosed|Ligações Fechadas. (Pré-visualização)|Contagem|Total|Ligações Fechadas do Microsoft.ServiceBus. (Pré-visualização)|EntityName|
|CPUXNS|Utilização da CPU por espaço de nomes|Percentagem|Máximo|Métrica de utilização do espaço de nomes da CPU do service bus premium|Não foram dimensões|
|WSXNS|Utilização do tamanho da memória por espaço de nomes|Percentagem|Máximo|Métrica de utilização de memória do espaço de nomes do service bus premium|Não foram dimensões|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Percentagem de CPU|Percentagem|Média|Percentagem de CPU|Não foram dimensões|
|physical_data_read_percent|Percentagem de ES de Dados|Percentagem|Média|Percentagem de ES de Dados|Não foram dimensões|
|log_write_percent|Percentagem de es do registo|Percentagem|Média|Percentagem de es do registo|Não foram dimensões|
|dtu_consumption_percent|Percentagem de DTU|Percentagem|Média|Percentagem de DTU|Não foram dimensões|
|armazenamento|Tamanho total da base de dados|Bytes|Máximo|Tamanho total da base de dados|Não foram dimensões|
|connection_successful|Ligações com êxito|Contagem|Total|Ligações com êxito|Não foram dimensões|
|connection_failed|Falha de ligações|Contagem|Total|Falha de ligações|Não foram dimensões|
|blocked_by_firewall|Bloqueado pela Firewall|Contagem|Total|Bloqueado pela Firewall|Não foram dimensões|
|impasse|Impasses|Contagem|Total|Impasses|Não foram dimensões|
|storage_percent|Percentagem de tamanho da Base de Dados|Percentagem|Máximo|Percentagem de tamanho da Base de Dados|Não foram dimensões|
|xtp_storage_percent|Percentagem de armazenamento do OLTP dentro da memória|Percentagem|Média|Percentagem de armazenamento do OLTP dentro da memória|Não foram dimensões|
|workers_percent|Percentagem de trabalhadores|Percentagem|Média|Percentagem de trabalhadores|Não foram dimensões|
|sessions_percent|Percentagem de sessões|Percentagem|Média|Percentagem de sessões|Não foram dimensões|
|dtu_limit|Limite DTU|Contagem|Média|Limite DTU|Não foram dimensões|
|dtu_used|DTU utilizado|Contagem|Média|DTU utilizado|Não foram dimensões|
|dwu_limit|Limite DWU|Contagem|Máximo|Limite DWU|Não foram dimensões|
|dwu_consumption_percent|Percentagem DWU|Percentagem|Máximo|Percentagem DWU|Não foram dimensões|
|dwu_used|DWU utilizado|Contagem|Máximo|DWU utilizado|Não foram dimensões|
|dw_cpu_percent|Nível de nó de armazém de dados percentagem de CPU|Percentagem|Média|Nível de nó de armazém de dados percentagem de CPU|dw_logical_node_id|
|dw_physical_data_read_percent|Percentagem de es de dados de nível de nó do armazém de dados|Percentagem|Média|Percentagem de es de dados de nível de nó do armazém de dados|dw_logical_node_id|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Percentagem de CPU|Percentagem|Média|Percentagem de CPU|Não foram dimensões|
|database_cpu_percent|Percentagem de CPU|Percentagem|Média|Percentagem de CPU|DatabaseResourceId|
|physical_data_read_percent|Percentagem de ES de Dados|Percentagem|Média|Percentagem de ES de Dados|Não foram dimensões|
|database_physical_data_read_percent|Percentagem de ES de Dados|Percentagem|Média|Percentagem de ES de Dados|DatabaseResourceId|
|log_write_percent|Percentagem de es do registo|Percentagem|Média|Percentagem de es do registo|Não foram dimensões|
|database_log_write_percent|Percentagem de es do registo|Percentagem|Média|Percentagem de es do registo|DatabaseResourceId|
|dtu_consumption_percent|Percentagem de DTU|Percentagem|Média|Percentagem de DTU|Não foram dimensões|
|database_dtu_consumption_percent|Percentagem de DTU|Percentagem|Média|Percentagem de DTU|DatabaseResourceId|
|storage_percent|Percentagem de armazenamento|Percentagem|Média|Percentagem de armazenamento|Não foram dimensões|
|workers_percent|Percentagem de trabalhadores|Percentagem|Média|Percentagem de trabalhadores|Não foram dimensões|
|database_workers_percent|Percentagem de trabalhadores|Percentagem|Média|Percentagem de trabalhadores|DatabaseResourceId|
|sessions_percent|Percentagem de sessões|Percentagem|Média|Percentagem de sessões|Não foram dimensões|
|database_sessions_percent|Percentagem de sessões|Percentagem|Média|Percentagem de sessões|DatabaseResourceId|
|eDTU_limit|limite de eDTU|Contagem|Média|limite de eDTU|Não foram dimensões|
|storage_limit|Limite de armazenamento|Bytes|Média|Limite de armazenamento|Não foram dimensões|
|eDTU_used|eDTU utilizado|Contagem|Média|eDTU utilizado|Não foram dimensões|
|storage_used|Armazenamento utilizado|Bytes|Média|Armazenamento utilizado|Não foram dimensões|
|database_storage_used|Armazenamento utilizado|Bytes|Média|Armazenamento utilizado|DatabaseResourceId|
|xtp_storage_percent|Percentagem de armazenamento do OLTP dentro da memória|Percentagem|Média|Percentagem de armazenamento do OLTP dentro da memória|Não foram dimensões|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|dtu_consumption_percent|Percentagem de DTU|Percentagem|Média|Percentagem de DTU|ElasticPoolResourceId|
|database_dtu_consumption_percent|Percentagem de DTU|Percentagem|Média|Percentagem de DTU|DatabaseResourceId, ElasticPoolResourceId|
|storage_used|Armazenamento utilizado|Bytes|Média|Armazenamento utilizado|ElasticPoolResourceId|
|database_storage_used|Armazenamento utilizado|Bytes|Média|Armazenamento utilizado|DatabaseResourceId, ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Storage/storageaccounts

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|UsedCapacity|Capacidade utilizada|Bytes|Média|Capacidade da conta utilizada|Não foram dimensões|
|Transações|Transações|Contagem|Total|O número de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada. Este número inclui pedidos com êxito ou falhados, bem como os pedidos que produziu erros. Utilize ResponseType dimensão para o número de tipo diferente de resposta.|ApiName ResponseType, GeoType,|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Este número inclui a entrada de um cliente externo em armazenamento do Azure, bem como entrada no Azure.|GeoType, ApiName|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Este número inclui a saída de um cliente externo em armazenamento do Azure, bem como saída no Azure. Consequentemente, este número não reflete saída sujeito a faturação.|GeoType, ApiName|
|SuccessServerLatency|Latência de Servidor Com Êxito|Milissegundos|Média|A latência média utilizada pelo armazenamento do Azure para processar um pedido com êxito, em milissegundos. Este valor não inclui a latência de rede especificada no AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Latência de E2E Com Êxito|Milissegundos|Média|A ponto-a-ponto latência média de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada, em milissegundos com êxito. Este valor inclui o tempo de processamento necessário no armazenamento do Azure para o pedido de leitura, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName|
|Disponibilidade|Disponibilidade|Percentagem|Média|A percentagem de disponibilidade para o serviço de armazenamento ou a operação de API especificada. Disponibilidade é calculada ao colocar o valor de TotalBillableRequests e dividindo-lo pelo número de pedidos aplicáveis, incluindo os que produziu erros inesperados. Todos os erros inesperados resultam na disponibilidade reduzida para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|BlobCapacity|Capacidade do Blob|Bytes|Média|A quantidade de armazenamento utilizada pelo serviço Blob a conta de armazenamento, em bytes.|BlobType|
|BlobCount|Contagem de Blobs|Contagem|Média|O número de blobs no serviço Blob da conta de armazenamento.|BlobType|
|ContainerCount|Contagem do Contentor de Blobs|Contagem|Média|O número de contentores no serviço Blob da conta de armazenamento.|Não foram dimensões|
|Transações|Transações|Contagem|Total|O número de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada. Este número inclui pedidos com êxito ou falhados, bem como os pedidos que produziu erros. Utilize ResponseType dimensão para o número de tipo diferente de resposta.|ApiName ResponseType, GeoType,|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Este número inclui a entrada de um cliente externo em armazenamento do Azure, bem como entrada no Azure.|GeoType, ApiName|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Este número inclui a saída de um cliente externo em armazenamento do Azure, bem como saída no Azure. Consequentemente, este número não reflete saída sujeito a faturação.|GeoType, ApiName|
|SuccessServerLatency|Latência de Servidor Com Êxito|Milissegundos|Média|A latência média utilizada pelo armazenamento do Azure para processar um pedido com êxito, em milissegundos. Este valor não inclui a latência de rede especificada no AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Latência de E2E Com Êxito|Milissegundos|Média|A ponto-a-ponto latência média de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada, em milissegundos com êxito. Este valor inclui o tempo de processamento necessário no armazenamento do Azure para o pedido de leitura, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName|
|Disponibilidade|Disponibilidade|Percentagem|Média|A percentagem de disponibilidade para o serviço de armazenamento ou a operação de API especificada. Disponibilidade é calculada ao colocar o valor de TotalBillableRequests e dividindo-lo pelo número de pedidos aplicáveis, incluindo os que produziu erros inesperados. Todos os erros inesperados resultam na disponibilidade reduzida para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|TableCapacity|Capacidade de Tabelas|Bytes|Média|A quantidade de armazenamento utilizado pelo serviço Tabela da conta de armazenamento, em bytes.|Não foram dimensões|
|TableCount|Contagem de Tabelas|Contagem|Média|O número de tabelas no serviço Tabela da conta de armazenamento.|Não foram dimensões|
|TableEntityCount|Contagem de Entidade de Tabelas|Contagem|Média|O número de entidades de tabelas no serviço Tabela da conta de armazenamento.|Não foram dimensões|
|Transações|Transações|Contagem|Total|O número de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada. Este número inclui pedidos com êxito ou falhados, bem como os pedidos que produziu erros. Utilize ResponseType dimensão para o número de tipo diferente de resposta.|ApiName ResponseType, GeoType,|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Este número inclui a entrada de um cliente externo em armazenamento do Azure, bem como entrada no Azure.|GeoType, ApiName|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Este número inclui a saída de um cliente externo em armazenamento do Azure, bem como saída no Azure. Consequentemente, este número não reflete saída sujeito a faturação.|GeoType, ApiName|
|SuccessServerLatency|Latência de Servidor Com Êxito|Milissegundos|Média|A latência média utilizada pelo armazenamento do Azure para processar um pedido com êxito, em milissegundos. Este valor não inclui a latência de rede especificada no AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Latência de E2E Com Êxito|Milissegundos|Média|A ponto-a-ponto latência média de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada, em milissegundos com êxito. Este valor inclui o tempo de processamento necessário no armazenamento do Azure para o pedido de leitura, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName|
|Disponibilidade|Disponibilidade|Percentagem|Média|A percentagem de disponibilidade para o serviço de armazenamento ou a operação de API especificada. Disponibilidade é calculada ao colocar o valor de TotalBillableRequests e dividindo-lo pelo número de pedidos aplicáveis, incluindo os que produziu erros inesperados. Todos os erros inesperados resultam na disponibilidade reduzida para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|QueueCapacity|Capacidade de Fila|Bytes|Média|A quantidade de armazenamento utilizada pelo serviço Fila a conta de armazenamento, em bytes.|Não foram dimensões|
|QueueCount|Contagem de Filas|Contagem|Média|O número de filas no serviço Fila da conta de armazenamento.|Não foram dimensões|
|QueueMessageCount|Contagem de Mensagens em Fila|Contagem|Média|O número aproximado de mensagens em fila no serviço Fila da conta de armazenamento.|Não foram dimensões|
|Transações|Transações|Contagem|Total|O número de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada. Este número inclui pedidos com êxito ou falhados, bem como os pedidos que produziu erros. Utilize ResponseType dimensão para o número de tipo diferente de resposta.|ApiName ResponseType, GeoType,|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Este número inclui a entrada de um cliente externo em armazenamento do Azure, bem como entrada no Azure.|GeoType, ApiName|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Este número inclui a saída de um cliente externo em armazenamento do Azure, bem como saída no Azure. Consequentemente, este número não reflete saída sujeito a faturação.|GeoType, ApiName|
|SuccessServerLatency|Latência de Servidor Com Êxito|Milissegundos|Média|A latência média utilizada pelo armazenamento do Azure para processar um pedido com êxito, em milissegundos. Este valor não inclui a latência de rede especificada no AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Latência de E2E Com Êxito|Milissegundos|Média|A ponto-a-ponto latência média de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada, em milissegundos com êxito. Este valor inclui o tempo de processamento necessário no armazenamento do Azure para o pedido de leitura, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName|
|Disponibilidade|Disponibilidade|Percentagem|Média|A percentagem de disponibilidade para o serviço de armazenamento ou a operação de API especificada. Disponibilidade é calculada ao colocar o valor de TotalBillableRequests e dividindo-lo pelo número de pedidos aplicáveis, incluindo os que produziu erros inesperados. Todos os erros inesperados resultam na disponibilidade reduzida para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|FileCapacity|Capacidade de Ficheiros|Bytes|Média|A quantidade de armazenamento utilizada pelo serviço Ficheiro a conta de armazenamento, em bytes.|Não foram dimensões|
|FileCount|Contagem de ficheiros|Contagem|Média|O número de ficheiros no serviço Ficheiro da conta de armazenamento.|Não foram dimensões|
|FileShareCount|Contagem de Partilha de Ficheiros|Contagem|Média|O número de partilhas de ficheiros no serviço Ficheiro da conta de armazenamento.|Não foram dimensões|
|Transações|Transações|Contagem|Total|O número de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada. Este número inclui pedidos com êxito ou falhados, bem como os pedidos que produziu erros. Utilize ResponseType dimensão para o número de tipo diferente de resposta.|ApiName ResponseType, GeoType,|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Este número inclui a entrada de um cliente externo em armazenamento do Azure, bem como entrada no Azure.|GeoType, ApiName|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Este número inclui a saída de um cliente externo em armazenamento do Azure, bem como saída no Azure. Consequentemente, este número não reflete saída sujeito a faturação.|GeoType, ApiName|
|SuccessServerLatency|Latência de Servidor Com Êxito|Milissegundos|Média|A latência média utilizada pelo armazenamento do Azure para processar um pedido com êxito, em milissegundos. Este valor não inclui a latência de rede especificada no AverageE2ELatency.|GeoType, ApiName|
|SuccessE2ELatency|Latência de E2E Com Êxito|Milissegundos|Média|A ponto-a-ponto latência média de pedidos efetuados para um serviço de armazenamento ou a operação de API especificada, em milissegundos com êxito. Este valor inclui o tempo de processamento necessário no armazenamento do Azure para o pedido de leitura, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName|
|Disponibilidade|Disponibilidade|Percentagem|Média|A percentagem de disponibilidade para o serviço de armazenamento ou a operação de API especificada. Disponibilidade é calculada ao colocar o valor de TotalBillableRequests e dividindo-lo pelo número de pedidos aplicáveis, incluindo os que produziu erros inesperados. Todos os erros inesperados resultam na disponibilidade reduzida para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|ResourceUtilization|% de utilização SU|Percentagem|Máximo|% de utilização SU|Não foram dimensões|
|InputEvents|Eventos de Entrada|Contagem|Total|Eventos de Entrada|Não foram dimensões|
|InputEventBytes|Bytes de Evento de Entrada|Bytes|Total|Bytes de Evento de Entrada|Não foram dimensões|
|LateInputEvents|Eventos de Entrada atrasados|Contagem|Total|Eventos de Entrada atrasados|Não foram dimensões|
|OutputEvents|Eventos de Saída|Contagem|Total|Eventos de Saída|Não foram dimensões|
|ConversionErrors|Erros de Conversão de Dados|Contagem|Total|Erros de Conversão de Dados|Não foram dimensões|
|Erros|Erros de Tempo de Execução|Contagem|Total|Erros de Tempo de Execução|Não foram dimensões|
|DroppedOrAdjustedEvents|Eventos desordenados|Contagem|Total|Eventos desordenados|Não foram dimensões|
|AMLCalloutRequests|Pedidos de Função|Contagem|Total|Pedidos de Função|Não foram dimensões|
|AMLCalloutFailedRequests|Falha no pedido de funções|Contagem|Total|Falha no pedido de funções|Não foram dimensões|
|AMLCalloutInputEvents|Eventos de Função|Contagem|Total|Eventos de Função|Não foram dimensões|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|CpuPercentage|Percentagem de CPU|Percentagem|Média|Percentagem de CPU|Instância|
|MemoryPercentage|Percentagem de Memória|Percentagem|Média|Percentagem de Memória|Instância|
|DiskQueueLength|Comprimento da Fila de Discos|Contagem|Média|Comprimento da Fila de Discos|Instância|
|HttpQueueLength|Comprimento da Fila de http|Contagem|Média|Comprimento da Fila de http|Instância|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (excluindo as funções)

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|CpuTime|Tempo de CPU|Segundos|Total|Tempo de CPU|Instância|
|Pedidos|Pedidos|Contagem|Total|Pedidos|Instância|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|
|Http101|Http 101|Contagem|Total|Http 101|Instância|
|Http2xx|Http 2xx|Contagem|Total|Http 2xx|Instância|
|Http3xx|Http 3xx|Contagem|Total|Http 3xx|Instância|
|Http401|Http 401|Contagem|Total|Http 401|Instância|
|Http403|Http 403|Contagem|Total|Http 403|Instância|
|Http404|Http 404|Contagem|Total|Http 404|Instância|
|Http406|Http 406|Contagem|Total|Http 406|Instância|
|Http4xx|Http 4xx|Contagem|Total|Http 4xx|Instância|
|Http5xx|Erros de Servidor http|Contagem|Total|Erros de Servidor http|Instância|
|MemoryWorkingSet|Conjunto de trabalho de memória|Bytes|Média|Conjunto de trabalho de memória|Instância|
|AverageMemoryWorkingSet|Média do conjunto de trabalho de memória|Bytes|Média|Média do conjunto de trabalho de memória|Instância|
|AverageResponseTime|Tempo de Resposta Médio|Segundos|Média|Tempo de Resposta Médio|Instância|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funções)

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|
|Http5xx|Erros de Servidor http|Contagem|Total|Erros de Servidor http|Instância|
|MemoryWorkingSet|Conjunto de trabalho de memória|Bytes|Média|Conjunto de trabalho de memória|Instância|
|AverageMemoryWorkingSet|Média do conjunto de trabalho de memória|Bytes|Média|Média do conjunto de trabalho de memória|Instância|
|FunctionExecutionUnits|Unidades de Execução de Funções|Contagem|Total|Unidades de Execução de Funções|Instância|
|FunctionExecutionCount|Contagem de Execuções de Função|Contagem|Total|Contagem de Execuções de Função|Instância|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|CpuTime|Tempo de CPU|Segundos|Total|Tempo de CPU|Instância|
|Pedidos|Pedidos|Contagem|Total|Pedidos|Instância|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|
|Http101|Http 101|Contagem|Total|Http 101|Instância|
|Http2xx|Http 2xx|Contagem|Total|Http 2xx|Instância|
|Http3xx|Http 3xx|Contagem|Total|Http 3xx|Instância|
|Http401|Http 401|Contagem|Total|Http 401|Instância|
|Http403|Http 403|Contagem|Total|Http 403|Instância|
|Http404|Http 404|Contagem|Total|Http 404|Instância|
|Http406|Http 406|Contagem|Total|Http 406|Instância|
|Http4xx|Http 4xx|Contagem|Total|Http 4xx|Instância|
|Http5xx|Erros de Servidor http|Contagem|Total|Erros de Servidor http|Instância|
|MemoryWorkingSet|Conjunto de trabalho de memória|Bytes|Média|Conjunto de trabalho de memória|Instância|
|AverageMemoryWorkingSet|Média do conjunto de trabalho de memória|Bytes|Média|Média do conjunto de trabalho de memória|Instância|
|AverageResponseTime|Tempo de Resposta Médio|Segundos|Média|Tempo de Resposta Médio|Instância|
|FunctionExecutionUnits|Unidades de Execução de Funções|Contagem|Média|Unidades de Execução de Funções|Instância|
|FunctionExecutionCount|Contagem de Execuções de Função|Contagem|Média|Contagem de Execuções de Função|Instância|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|Pedidos|Pedidos|Contagem|Total|Pedidos|Instância|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|
|Http101|Http 101|Contagem|Total|Http 101|Instância|
|Http2xx|Http 2xx|Contagem|Total|Http 2xx|Instância|
|Http3xx|Http 3xx|Contagem|Total|Http 3xx|Instância|
|Http401|Http 401|Contagem|Total|Http 401|Instância|
|Http403|Http 403|Contagem|Total|Http 403|Instância|
|Http404|Http 404|Contagem|Total|Http 404|Instância|
|Http406|Http 406|Contagem|Total|Http 406|Instância|
|Http4xx|Http 4xx|Contagem|Total|Http 4xx|Instância|
|Http5xx|Erros de Servidor http|Contagem|Total|Erros de Servidor http|Instância|
|AverageResponseTime|Tempo de Resposta Médio|Segundos|Média|Tempo de Resposta Médio|Instância|
|CpuPercentage|Percentagem de CPU|Percentagem|Média|Percentagem de CPU|Instância|
|MemoryPercentage|Percentagem de Memória|Percentagem|Média|Percentagem de Memória|Instância|
|DiskQueueLength|Comprimento da Fila de Discos|Contagem|Média|Comprimento da Fila de Discos|Instância|
|HttpQueueLength|Comprimento da Fila de http|Contagem|Média|Comprimento da Fila de http|Instância|
|ActiveRequests|Pedidos Ativos|Contagem|Total|Pedidos Ativos|Instância|
|TotalFrontEnds|Total de Front Ends|Contagem|Média|Total de Front Ends|Instância|
|SmallAppServicePlanInstances|Trabalhadores do Plano de Serviço de Aplicações de Pequena Dimensão|Contagem|Média|Trabalhadores do Plano de Serviço de Aplicações de Pequena Dimensão|Instância|
|MediumAppServicePlanInstances|Trabalhadores do Plano de Serviço de Aplicações de Média Dimensão|Contagem|Média|Trabalhadores do Plano de Serviço de Aplicações de Média Dimensão|Instância|
|LargeAppServicePlanInstances|Trabalhadores do Plano de Serviço de Aplicações de Grande Dimensão|Contagem|Média|Trabalhadores do Plano de Serviço de Aplicações de Grande Dimensão|Instância|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Métrica|Nome a apresentar métrica|Unidade|Tipo de agregação|Descrição|Dimensões|
|---|---|---|---|---|---|
|WorkersTotal|Trabalhadores Totais|Contagem|Média|Trabalhadores Totais|Instância|
|WorkersAvailable|Trabalhadores Disponíveis|Contagem|Média|Trabalhadores Disponíveis|Instância|
|WorkersUsed|Trabalhadores Utilizados|Contagem|Média|Trabalhadores Utilizados|Instância|
|CpuPercentage|Percentagem de CPU|Percentagem|Média|Percentagem de CPU|Instância|
|MemoryPercentage|Percentagem de Memória|Percentagem|Média|Percentagem de Memória|Instância|

## <a name="next-steps"></a>Passos Seguintes
* [Leia sobre as métricas no Monitor do Azure](monitoring-overview-metrics.md)
* [Criar alertas nas métricas](insights-receive-alert-notifications.md)
* [Exportar as métricas para o armazenamento, Hub de eventos ou análise de registos](monitoring-overview-of-diagnostic-logs.md)
