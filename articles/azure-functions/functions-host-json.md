---
title: referência de Host. JSON para as funções do Azure
description: Documentação de referência para o ficheiro de Host. JSON das funções do Azure.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/08/2018
ms.author: glenga
ms.openlocfilehash: 085df618eb6d3eb78e42261d1b324c3a2374877b
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123390"
---
# <a name="hostjson-reference-for-azure-functions"></a>referência de Host. JSON para as funções do Azure

O *Host. JSON* ficheiro de metadados contém opções de configuração globais que afetam todas as funções para uma aplicação de funções. Este artigo lista as definições que estão disponíveis. O esquema JSON está no http://json.schemastore.org/host.

> [!NOTE]
> Existem diferenças significativas na *Host. JSON* entre versões v1 e v2 do runtime das funções do Azure. O `"version": "2.0"` é necessária para uma aplicação de função que tenha como destino o tempo de execução do v2.

Outras opções de configuração de aplicação de função são geridas no seu [as definições da aplicação](functions-app-settings.md).

Algumas definições de Host. JSON só são utilizadas quando em execução localmente na [Settings](functions-run-local.md#local-settings-file) ficheiro.

## <a name="sample-hostjson-file"></a>Ficheiro de Host. JSON de exemplo

O exemplo a seguir *Host. JSON* ficheiros têm todas as opções possíveis especificadas.

### <a name="version-2x"></a>Versão 2.x

```json
{
    "version": "2.0",
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "extensions": {
        "eventHubs": {
          "maxBatchSize": 64,
          "prefetchCount": 256,
          "batchCheckpointFrequency": 1
        },
        "http": {
            "routePrefix": "api",
            "maxConcurrentRequests": 5,
            "maxOutstandingRequests": 30
        },
        "queues": {
            "visibilityTimeout": "00:00:10",
            "maxDequeueCount": 3
        },
        "sendGrid": {
            "from": "Azure Functions <samples@functions.com>"
        },
        "serviceBus": {
          "maxConcurrentCalls": 16,
          "prefetchCount": 100,
          "autoRenewTimeout": "00:05:00"
        }
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logging": {
        "fileLoggingMode": "debugOnly",
        "logLevel": {
          "Function.MyFunction": "Information",
          "default": "None"
        },
        "applicationInsights": {
            "sampling": {
              "isEnabled": true,
              "maxTelemetryItemsPerSecond" : 5
            }
        }
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "watchDirectories": [ "Shared", "Test" ]
}
```

### <a name="version-1x"></a>Versão 1.x

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    },
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    },
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    },
    "functions": [ "QueueProcessor", "GitHubWebHook" ],
    "functionTimeout": "00:05:00",
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    },
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    },
    "id": "9f4ea53c5136457d883d685e57164f08",
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    },
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    },
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    },
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    },
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    },
    "watchDirectories": [ "Shared" ],
}
```

As secções seguintes deste artigo explicam cada propriedade de nível superior. Todas são opcionais, a menos que indicado de outra forma.

## <a name="aggregator"></a>agregador

Especifica quantos invocações de função são agregados quando [calcular métricas para o Application Insights](functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Propriedade |Predefinição  | Descrição |
|---------|---------|---------| 
|batchSize|1000|Número máximo de pedidos a agregar.| 
|flushTimeout|00:00:30|Tempo máximo de período a agregar.| 

Invocações de função são agregadas quando o primeiro dos dois limites são atingidos.

## <a name="applicationinsights"></a>applicationInsights

Controles a [funcionalidade de amostragem no Application Insights](functions-monitoring.md#configure-sampling). Na versão 2.x, esta definição é um elemento subordinado [registo](#log).

```json
{
    "applicationInsights": {
        "sampling": {
          "isEnabled": true,
          "maxTelemetryItemsPerSecond" : 5
        }
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|isEnabled|true|Ativa ou desativa a amostragem.| 
|maxTelemetryItemsPerSecond|5|O limiar em que amostragem começa.| 

## <a name="durabletask"></a>durableTask

Definições de configuração para [funções duráveis](durable-functions-overview.md).

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

Os nomes de hubs de tarefas devem começar com uma letra e consistir apenas letras e números. Se não for especificado, o nome do hub de tarefas padrão para uma aplicação de funções é **DurableFunctionsHub**. Para obter mais informações, consulte [os hubs de tarefas](durable-functions-task-hubs.md).

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------|
|HubName|DurableFunctionsHub|Alternativo [hub tarefas](durable-functions-task-hubs.md) nomes podem ser utilizados para isolar várias aplicações de funções durável entre si, mesmo que eles usando o mesmo back-end de armazenamento.|
|ControlQueueBatchSize|32|O número de mensagens para solicitar a partir da fila de controle de cada vez.|
|PartitionCount |4|O número de partições para a fila de controle. Pode ser um número inteiro positivo entre 1 e 16.|
|ControlQueueVisibilityTimeout |5 minutos|O tempo limite de visibilidade de mensagens de fila de retirada da fila de controle.|
|WorkItemQueueVisibilityTimeout |5 minutos|O tempo limite de visibilidade de mensagens de fila de item de trabalho retirada da fila.|
|MaxConcurrentActivityFunctions |10 vezes o número de processadores no computador atual|O número máximo de funções de atividade que podem ser processadas em simultâneo numa instância de anfitrião único.|
|MaxConcurrentOrchestratorFunctions |10 vezes o número de processadores no computador atual|O número máximo de funções de atividade que podem ser processadas em simultâneo numa instância de anfitrião único.|
|AzureStorageConnectionStringName |AzureWebJobsStorage|O nome da definição de aplicação que tenha a cadeia de ligação de armazenamento do Azure utilizada para gerir os recursos de armazenamento do Azure subjacentes.|
|TraceInputsAndOutputs |false|Um valor que indica se pretende rastrear as entradas e saídas de chamadas de função. É o comportamento predefinido durante o rastreamento de eventos de execução de função incluir o número de bytes na serializada entradas e saídas de chamadas de função. Isto proporciona informações mínimas sobre as entradas e saídas aparência sem bloating os registos ou inadvertidamente expor informações confidenciais nos registos. Definir esta propriedade como true faz com que o registo de função predefinido para todo o conteúdo da função entradas e saídas de registo.|
|LogReplayEvents|false|Um valor que indica se deve escrever eventos de repetição de orquestração no Application Insights.|
|EventGridTopicEndpoint ||O URL de um ponto de extremidade de um tópico personalizado do Azure Event Grid. Quando essa propriedade é definida, os eventos de notificação de ciclo de vida de orquestração são publicados para este ponto final. Esta propriedade suporta a resolução de definições da aplicação.|
|EventGridKeySettingName ||O nome da definição de aplicação que contém a chave utilizada para autenticar com o tópico personalizado do Azure Event Grid em `EventGridTopicEndpoint`.|
|EventGridPublishRetryCount|0|O número de vezes para repetir se publicar para o tópico do Event Grid falha.|
|EventGridPublishRetryInterval|5 minutos|O Event Grid publica o intervalo de repetição no *hh: mm:* formato.|

Muitos deles são para otimizar o desempenho. Para obter mais informações, consulte [desempenho e dimensionamento](durable-functions-perf-and-scale.md).

## <a name="eventhub"></a>eventHub

Definições de configuração para [Hub de eventos de acionadores e enlaces](functions-bindings-event-hubs.md). Na versão 2.x, isso é um elemento subordinado [extensões](#extensions).

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="extensions"></a>Extensões

*Versão 2.x apenas.*

Propriedade que retorna um objeto que contém todas as definições de ligação específica, como [http](#http) e [eventHub](#eventhub).

## <a name="functions"></a>functions

Uma lista de funções que o anfitrião de tarefa é executada. Uma matriz vazia significa executar todas as funções. Foi concebido para utilização apenas quando [em execução localmente](functions-run-local.md). Nas aplicações de função no Azure, em vez disso, deve seguir os passos em [como desativar funções nas funções do Azure](disable-function.md) para desativar funções específicas, em vez de utilizar esta definição.

```json
{
    "functions": [ "QueueProcessor", "GitHubWebHook" ]
}
```

## <a name="functiontimeout"></a>functionTimeout

Indica o período de tempo limite para todas as funções. No plano de consumo sem servidor, o intervalo válido é entre 1 segundo para 10 minutos e o valor predefinido é de 5 minutos. Num plano do serviço de aplicações, não existe nenhum limite global e o padrão depende da versão de runtime. Na versão 2.x, o valor predefinido para um serviço de aplicações plano é 30 minutos. Na versão 1.x, é *nulo*, que indica sem tempo limite.

```json
{
    "functionTimeout": "00:05:00"
}
```

## <a name="healthmonitor"></a>healthMonitor

Definições de configuração para [monitor de estado de funcionamento do anfitrião](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Host-Health-Monitor).

```
{
    "healthMonitor": {
        "enabled": true,
        "healthCheckInterval": "00:00:10",
        "healthCheckWindow": "00:02:00",
        "healthCheckThreshold": 6,
        "counterThreshold": 0.80
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|enabled|true|Especifica se a funcionalidade está ativada. | 
|healthCheckInterval|10 segundos|Verifica o intervalo de tempo entre o estado de funcionamento periódica de segundo plano. | 
|healthCheckWindow|2 minutos|Uma janela deslizante de tempo utilizada em conjunto com o `healthCheckThreshold` definição.| 
|healthCheckThreshold|6|Número máximo de vezes que a verificação de estado de funcionamento pode falhar antes de uma reciclagem de anfitrião é iniciada.| 
|counterThreshold|0.80|O limiar em que um contador de desempenho será considerado problemático.| 

## <a name="http"></a>http

Definições de configuração para [http acionadores e enlaces](functions-bindings-http-webhook.md). Na versão 2.x, isso é um elemento subordinado [extensões](#extensions).

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="id"></a>ID

*Versão 1.x apenas.*

O ID exclusivo para um anfitrião de tarefa. Pode ser uma minúscula GUID com travessões removida. É necessário quando em execução localmente. Quando em execução no Azure, recomendamos que não defina um valor de ID. Um ID é gerado automaticamente no Azure quando `id` for omitido. Não é possível definir um ID de aplicação de função personalizada ao utilizar a versão 2.x do runtime.

Se partilhar uma conta de armazenamento entre várias aplicações de funções, certifique-se de que cada aplicação de função tem outra `id`. Pode omitir os `id` propriedade ou definir manualmente cada aplicação de função `id` para um valor diferente. O acionador de temporizador utiliza um bloqueio de armazenamento para se certificar de que haverá apenas uma instância de temporizador quando uma aplicação de funções aumenta horizontalmente para várias instâncias. Se duas aplicações de função partilham o mesmo `id` e cada utiliza um acionador de temporizador, apenas um temporizador será executado.

```json
{
    "id": "9f4ea53c5136457d883d685e57164f08"
}
```

## <a name="logger"></a>agente de log

*Versão 1.x apenas; para utilização do versão 2.x [registo](#logging).*

Controlos de filtragem para os registos de escrita por um [objeto ILogger](functions-monitoring.md#write-logs-in-c-functions) ou pelo [context.log](functions-monitoring.md#write-logs-in-javascript-functions).

```json
{
    "logger": {
        "categoryFilter": {
            "defaultLevel": "Information",
            "categoryLevels": {
                "Host": "Error",
                "Function": "Error",
                "Host.Aggregator": "Information"
            }
        }
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|categoryFilter|n/d|Especifica a filtragem por categoria| 
|defaultLevel|Informações|Para as categorias não especificadas no `categoryLevels` matriz, enviar registos neste nível e superior para o Application Insights.| 
|categoryLevels|n/d|Uma matriz de categorias que especifica o nível de registo mínimo para enviar para o Application Insights para cada categoria. A categoria especificada aqui controla todas as categorias que começam com o mesmo valor e, mais tempo valores têm precedência. No exemplo anterior *Host. JSON* do ficheiro, todas as categorias que começam com "Host.Aggregator" registo em `Information` nível. Todas as outras categorias que começam com "Host", como "Host.Executor", inicie sessão em `Error` nível.| 

## <a name="logging"></a>Registro em log

*Versão 2.x apenas; para a versão 1.x uso [logger](#logger).*

Controla os comportamentos de registo da aplicação de função, incluindo o Application Insights.

```json
"logging": {
    "fileLoggingMode": "debugOnly",
    "logLevel": {
      "Function.MyFunction": "Information",
      "default": "None"
    },
    "applicationInsights": {
        ...
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------|
|fileLoggingMode|informações|Envia registos neste nível e superior para o Application Insights. |
|LogLevel|n/d|Objeto que define a categoria de registo de filtragem para as funções na aplicação. Versão 2.x segue o layout do ASP.NET Core para filtragem de categoria do registo. Isto permite-lhe filtrar o registo para funções específicas. Para obter mais informações, consulte [filtragem de Log](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering) na documentação do ASP.NET Core. |
|applicationInsights|n/d| O [Application Insights](#applicationinsights) definição. |

## <a name="queues"></a>filas

Definições de configuração para [acionadores de fila de armazenamento e enlaces](functions-bindings-storage-queue.md). Na versão 2.x, isso é um elemento subordinado [extensões](#extensions).

[!INCLUDE [functions-host-json-queues](../../includes/functions-host-json-queues.md)]

## <a name="servicebus"></a>serviceBus

Definição de configuração para [do Service Bus acionadores e enlaces](functions-bindings-service-bus.md). Na versão 2.x, isso é um elemento subordinado [extensões](#extensions).

[!INCLUDE [functions-host-json-service-bus](../../includes/functions-host-json-service-bus.md)]

## <a name="singleton"></a>singleton

Definições de configuração para o comportamento de bloqueio de Singleton. Para obter mais informações, consulte [problema do GitHub sobre o suporte de singleton](https://github.com/Azure/azure-webjobs-sdk-script/issues/912).

```json
{
    "singleton": {
      "lockPeriod": "00:00:15",
      "listenerLockPeriod": "00:01:00",
      "listenerLockRecoveryPollingInterval": "00:01:00",
      "lockAcquisitionTimeout": "00:01:00",
      "lockAcquisitionPollingInterval": "00:00:03"
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|lockPeriod|00:00:15|O período de que os bloqueios de nível de função são utilizados para. Os bloqueios de renovação automática.| 
|listenerLockPeriod|00:01:00|O período de que os bloqueios de serviço de escuta são utilizados para.| 
|listenerLockRecoveryPollingInterval|00:01:00|O intervalo de tempo utilizado para a recuperação de bloqueio do serviço de escuta, se um bloqueio do serviço de escuta não foi possível adquirir na inicialização.| 
|lockAcquisitionTimeout|00:01:00|A quantidade máxima de tempo, que o tempo de execução irá tentar adquirir um bloqueio.| 
|lockAcquisitionPollingInterval|n/d|O intervalo entre tentativas de aquisição de bloqueio.| 

## <a name="tracing"></a>rastreio

*Versão 1.x*

Definições de configuração para os registos que criar utilizando um `TraceWriter` objeto. Ver [c# registo](functions-reference-csharp.md#logging) e [registo de node. js](functions-reference-node.md#writing-trace-output-to-the-console). Na versão 2.x, todo o comportamento de log é controlado pelas [registo](#logging).

```json
{
    "tracing": {
      "consoleLevel": "verbose",
      "fileLoggingMode": "debugOnly"
    }
}
```

|Propriedade  |Predefinição | Descrição |
|---------|---------|---------| 
|consoleLevel|informações|O nível de rastreio para o registo da consola. As opções são: `off`, `error`, `warning`, `info`, e `verbose`.|
|fileLoggingMode|debugOnly|O nível de rastreio para o registo do ficheiro. As opções são `never`, `always`, `debugOnly`.| 

## <a name="watchdirectories"></a>watchDirectories

Um conjunto de [partilhados diretórios de código](functions-reference-csharp.md#watched-directories) que devem ser monitorizados para que as alterações.  Garante que quando o código desses diretórios é alterado, as alterações são captadas pelas suas funções.

```json
{
    "watchDirectories": [ "Shared" ]
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como atualizar o ficheiro de Host. JSON](functions-reference.md#fileupdate)

> [!div class="nextstepaction"]
> [Ver definições globais em variáveis de ambiente](functions-app-settings.md)
