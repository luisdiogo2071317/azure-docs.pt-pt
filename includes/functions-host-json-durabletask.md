---
title: incluir ficheiro
description: incluir ficheiro
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 92eb13165326f44432f09322ea97f3cee5ccec2b
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251248"
---
Definições de configuração para [funções duráveis](../articles/azure-functions/durable-functions-overview.md).

```json
{
  "durableTask": {
    "HubName": "MyTaskHub",
    "ControlQueueBatchSize": 32,
    "PartitionCount": 4,
    "ControlQueueVisibilityTimeout": "00:05:00",
    "WorkItemQueueVisibilityTimeout": "00:05:00",
    "MaxConcurrentActivityFunctions": 10,
    "MaxConcurrentOrchestratorFunctions": 10,
    "AzureStorageConnectionStringName": "AzureWebJobsStorage",
    "TraceInputsAndOutputs": false,
    "LogReplayEvents": false,
    "EventGridTopicEndpoint": "https://topic_name.westus2-1.eventgrid.azure.net/api/events",
    "EventGridKeySettingName":  "EventGridKey",
    "EventGridPublishRetryCount": 3,
    "EventGridPublishRetryInterval": "00:00:30"
  }
}
```

Os nomes de hubs de tarefas devem começar com uma letra e consistir apenas letras e números. Se não for especificado, o nome do hub de tarefas padrão para uma aplicação de funções é **DurableFunctionsHub**. Para obter mais informações, consulte [os hubs de tarefas](../articles/azure-functions/durable-functions-task-hubs.md).

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternativo [hub tarefas](../articles/azure-functions/durable-functions-task-hubs.md) nomes podem ser utilizados para isolar várias aplicações de funções durável entre si, mesmo que eles usando o mesmo back-end de armazenamento.|
|ControlQueueBatchSize|32|O número de mensagens para solicitar a partir da fila de controle de cada vez.|
|PartitionCount |4|O número de partições para a fila de controle. Pode ser um número inteiro positivo entre 1 e 16.|
|ControlQueueVisibilityTimeout |5 minutos|O tempo limite de visibilidade de mensagens de fila de retirada da fila de controle.|
|WorkItemQueueVisibilityTimeout |5 minutos|O tempo limite de visibilidade de mensagens de fila de item de trabalho retirada da fila.|
|MaxConcurrentActivityFunctions |10 vezes o número de processadores no computador atual|O número máximo de funções de atividade que podem ser processadas em simultâneo numa instância de anfitrião único.|
|MaxConcurrentOrchestratorFunctions |10 vezes o número de processadores no computador atual|O número máximo de funções do orchestrator que podem ser processadas em simultâneo numa instância de anfitrião único.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|O nome da definição de aplicação que tenha a cadeia de ligação de armazenamento do Azure utilizada para gerir os recursos de armazenamento do Azure subjacentes.|
|TraceInputsAndOutputs |false|Um valor que indica se pretende rastrear as entradas e saídas de chamadas de função. É o comportamento predefinido durante o rastreamento de eventos de execução de função incluir o número de bytes na serializada entradas e saídas de chamadas de função. Isto proporciona informações mínimas sobre as entradas e saídas aparência sem bloating os registos ou inadvertidamente expor informações confidenciais nos registos. Definir esta propriedade como true faz com que o registo de função predefinido para todo o conteúdo da função entradas e saídas de registo.|
|LogReplayEvents|false|Um valor que indica se deve escrever eventos de repetição de orquestração no Application Insights.|
|EventGridTopicEndpoint ||O URL de um ponto de extremidade de um tópico personalizado do Azure Event Grid. Quando essa propriedade é definida, os eventos de notificação de ciclo de vida de orquestração são publicados para este ponto final. Esta propriedade suporta a resolução de definições da aplicação.|
|EventGridKeySettingName ||O nome da definição de aplicação que contém a chave utilizada para autenticar com o tópico personalizado do Azure Event Grid em `EventGridTopicEndpoint`.|
|EventGridPublishRetryCount|0|O número de vezes para repetir se publicar para o tópico do Event Grid falha.|
|EventGridPublishRetryInterval|5 minutos|O Event Grid publica o intervalo de repetição no *hh: mm:* formato.|

Muitos deles são para otimizar o desempenho. Para obter mais informações, consulte [desempenho e dimensionamento](../articles/azure-functions/durable-functions-perf-and-scale.md).