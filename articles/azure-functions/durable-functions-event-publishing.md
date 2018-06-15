---
title: Funciona durável publicação à grelha de eventos do Azure (pré-visualização)
description: Saiba como configurar a publicação automática grelha de eventos do Azure para funções durável.
services: functions
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/20/2018
ms.author: tdykstra
ms.openlocfilehash: 50e517e5719fb102fd91072abe59d3908176278e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33762467"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Funciona durável publicação à grelha de eventos do Azure (pré-visualização)

Este artigo mostra como configurar as durável funções do Azure para publicar eventos de ciclo de vida de orquestração (como criados, concluída ou falhada) personalizadas [Azure eventos grelha tópico](https://docs.microsoft.com/en-us/azure/event-grid/overview). 

Seguem-se alguns cenários em que esta funcionalidade é útil:

* **Cenários de DevOps como implementações de blue/verde**: pode querer saber se todas as tarefas estão em execução antes de implementar o [estratégia de implementação lado a lado](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-versioning#side-by-side-deployments).

* **Suporte de monitorização e diagnóstico avançada**: pode manter um registo dos informações de estado de orquestração num arquivo externo otimizada para consultas, tais como a base de dados SQL ou CosmosDB.

* **Atividade de em segundo plano de execução longa**: Se utilizar funções durável para uma atividade de fundo de longa execução, esta funcionalidade ajuda-o a saber o estado atual.

## <a name="prerequisites"></a>Pré-requisitos

* Instalar [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 1.3.0-rc ou posterior no seu projeto de funções durável.
* Instalar [emulador do Storage do Azure](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator).
* Instalar [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest) ou utilize [Shell de nuvem do Azure](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)

## <a name="create-a-custom-event-grid-topic"></a>Criar um tópico da grelha de evento personalizado

Crie um tópico de grelha de eventos para enviar eventos de funções durável. As instruções seguintes mostram como criar um tópico, utilizando a CLI do Azure. Para obter informações sobre como fazê-lo utilizando o PowerShell ou o portal do Azure, consulte os artigos seguintes:

* [Inícios rápidos de EventGrid: Criar evento personalizado - PowerShell](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart-powershell)
* [Inícios rápidos de EventGrid: Criar o evento personalizado - portal do Azure](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart-portal)

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Criar um grupo de recursos com o `az group create` comando. Atualmente, o evento grelha não suporta todas as regiões. Para obter informações sobre as regiões suportadas, consulte o [descrição geral da grelha de evento](https://docs.microsoft.com/en-us/azure/event-grid/overview). 

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Criar um tópico personalizado

Um tópico de grelha de evento fornece um ponto de final definido pelo utilizador que publicar o evento. Substitua `<topic_name>` por um nome exclusivo para o seu tópico. O nome do tópico tem de ser exclusivo porque torna-se uma entrada DNS.

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup 
```

## <a name="get-the-endpoint-and-key"></a>Obter o ponto final e a chave

Obter o ponto final do tópico. Substitua `<topic_name>` com o nome que escolheu.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Obter a chave de tópico. Substitua `<topic_name>` com o nome que escolheu.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Agora pode enviar eventos para o tópico.

## <a name="configure-azure-event-grid-publishing"></a>Configurar a publicação de grelha de eventos do Azure

O projeto de funções durável, localize o `host.json` ficheiro.

Adicionar `EventGridTopicEndpoint` e `EventGridKeySettingName` num `durableTask` propriedade.

```json
{
    "durableTask": {
        "EventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "EventGridKeySettingName": "EventGridKey"
    }
}
```

* **EventGridTopicEndpoint** -o ponto final do tópico de grelha de eventos.
* **EventGridKeySettingName** -chave da definição de aplicação na sua função do Azure. Funções duráveis irão obter a chave de tópico de grelha de eventos a partir do valor.

Depois de configurar o `host.json` funções durável seu projeto inicia para enviar eventos de ciclo de vida para o tópico de grelha de eventos, de ficheiros. Isto funciona quando executar a aplicação de função e executar localmente.

Configure a definição de aplicação para a chave de tópico na aplicação de função e `local.setting.json`. O seguinte JSON é apresentado um exemplo de `local.settings.json` para depuração local. Substitua `<topic_key>` com a chave de tópico.  

```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
        "EventGridKey": "<topic_key>"
    }
}
```

Certifique-se de que [emulador do Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator) está a funcionar. É uma boa ideia para executar o `AzureStorageEmulator.exe clear all` comando antes de executar.

## <a name="create-functions-that-listen-for-events"></a>Criar funções escutam eventos

Crie uma aplicação de função. É melhor a localizá-la na mesma região que o tópico de grelha de eventos.

### <a name="create-an-event-grid-trigger-function"></a>Criar uma função de Acionador de grelha de eventos

Crie uma função para receber os eventos de ciclo de vida. Selecione **função personalizada**. 

![Selecione um criar uma função personalizada.](media/durable-functions-event-publishing/functions-portal.png)

Escolha o evento acionador de grelha e selecione `C#`.

![Selecione o acionador de grelha de eventos.](media/durable-functions-event-publishing/eventgrid-trigger.png)

Introduza o nome da função e, em seguida, selecione `Create`.

![Crie o acionador de grelha de eventos.](media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

É criada uma função com o seguinte código: 

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

Selecione `Add Event Grid Subscription`. Esta operação adiciona uma subscrição de evento grelha para o tópico de grelha de eventos que criou. Para obter mais informações, consulte [conceitos na grelha de eventos do Azure](https://docs.microsoft.com/en-us/azure/event-grid/concepts)

![Selecione a ligação de Acionador de grelha de eventos.](media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Selecione `Event Grid Topics` para **tipo de tópico**. Selecione o grupo de recursos que criou para o tópico de grelha de eventos. Em seguida, selecione a instância do tópico grelha de eventos. Prima `Create`.

![Crie uma subscrição do Event Grid.](media/durable-functions-event-publishing/eventsubscription.png)

Agora está pronto para receber eventos de ciclo de vida. 

## <a name="create-durable-functions-to-send-the-events"></a>Crie funções durável para enviar os eventos.

No seu projeto de funções durável, inicie a depuração no seu computador local.  O seguinte código é o mesmo que o código de modelo para as funções durável. Já configurado `host.json` e `local.settings.json` no seu computador local. 

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace LifeCycleEventSpike
{
    public static class Sample
    {
    {
        [FunctionName("Sample")]
        public static async Task<List<string>> RunOrchestrator(
            [OrchestrationTrigger] DurableOrchestrationContext context)
        {
            var outputs = new List<string>();

            // Replace "hello" with the name of your Durable Activity Function.
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Tokyo"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Seattle"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "London"));

            // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
            return outputs;
        }

        [FunctionName("Sample_Hello")]
        public static string SayHello([ActivityTrigger] string name, TraceWriter log)
        {
            log.Info($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
            [OrchestrationClient]DurableOrchestrationClient starter,
            TraceWriter log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.Info($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

Se chama o `Sample_HttpStart` Postman ou o seu browser, durável função começa a enviar eventos de ciclo de vida. O ponto final está normalmente `http://localhost:7071/api/Sample_HttpStart` para depuração local.

Consulte os registos da função que criou no portal do Azure.

```
2018-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2018-04-20T09:28:21.104 [Info] {
    "id": "054fe385-c017-4ce3-b38a-052ac970c39d",    
    "subject": "durable/orchestrator/Running",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Running"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2018-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2018-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2018-04-20T09:28:37.098 [Info] {
    "id": "8cf17246-fa9c-4dad-b32a-5a868104f17b",
    "subject": "durable/orchestrator/Completed",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Completed"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2018-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Esquema de eventos

A lista seguinte explica o esquema de eventos de ciclo de vida:

* **ID**: Identificador exclusivo para o evento de grelha de eventos.
* **requerente**: caminho para o assunto do evento. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}` será `Running`, `Completed`, `Failed`, e `Terminated`.  
* **dados**: durável parâmetros específicos de funções.
    * **hubName**: [TaskHub](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-task-hubs) nome.
    * **functionName**: nome de função do Orchestrator.
    * **instanceId**: instanceId funções durável.
    * **motivo**: dados adicionais associados com o evento de controlo. Para obter mais informações, consulte [diagnóstico nas funções durável (funções do Azure)](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-diagnostics)
    * **runtimeStatus**: Estado de Runtime de orquestração. Em execução, concluída, falha, cancelada. 
* **o eventType**: "orchestratorEvent"
* **eventTime**: tempo do evento (UTC).
* **dataVersion**: versão de esquema de eventos de ciclo de vida.
* **metadataVersion**: versão de metadados.
* **tópico**: recursos EventGrid tópico.

## <a name="how-to-test-locally"></a>Como testar localmente

Para testar localmente, utilize [ngrok](functions-bindings-event-grid.md#local-testing-with-ngrok).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba de gestão de instância nas funções durável](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Saiba durável funções de controlo de versões](durable-functions-versioning.md)
