---
title: Gerir instâncias nas funções durável - Azure
description: Saiba como gerir instâncias na extensão do durável funções para as funções do Azure.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 0e573b4973ea30b990043b54c5cdcf0805135a40
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Gerir instâncias nas funções durável (funções do Azure)

[Funções duráveis](durable-functions-overview.md) instâncias de orquestração podem ser iniciadas, terminadas, consultar e enviar eventos de notificação. Toda a gestão de instância é feita utilizando o [cliente orchestration enlace](durable-functions-bindings.md). Este artigo entra os detalhes de cada operação de gestão de instância.

## <a name="starting-instances"></a>A partir de instâncias

O [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) método no [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) inicia uma nova instância de uma função do orchestrator. As instâncias desta classe podem ser adquiridas utilizando o `orchestrationClient` enlace. Internamente, este método enqueues uma mensagem na fila de controlo, em seguida, aciona o início de uma função com o nome especificado que utiliza o `orchestrationTrigger` acionar o enlace. 

A tarefa estiver concluída quando iniciar o processo de orquestração. Deve começar o processo de orquestração dentro de 30 segundos. Se este demora mais tempo, um `TimeoutException` é emitida. 

Os parâmetros de [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) são os seguintes:

* **Nome**: O nome da função do orchestrator a agendar.
* **Entrada**: quaisquer dados JSON serializável que devem ser transmitidos como entrada para a função do orchestrator.
* **InstanceId**: (opcional) o exclusivo ID da instância. Se não for especificado, será gerado um ID de instância aleatório.

Eis um exemplo simples c#:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}
```

Para idiomas não .NET, a função de enlace de saída pode ser utilizado para iniciar, bem como novas instâncias. Neste caso, pode ser utilizado qualquer objeto serializável para o JSON que tem os parâmetros de três acima como campos. Por exemplo, considere a função de JavaScript seguinte:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null);
};
```

> [!NOTE]
> Recomendamos que utilize um identificador aleatório para o ID de instância. Isto irá ajudar a assegurar uma distribuição de carga igual quando dimensionamento funções do orchestrator em várias VMs. O tempo adequado para utilizar os IDs de instância não aleatórias é quando o ID tem de ser de uma origem externa ou quando implementar o [singleton orchestrator](durable-functions-singletons.md) padrão.

## <a name="querying-instances"></a>Consulta de instâncias

O [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) método no [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe consulta o estado de uma instância de orquestração. Demora um `instanceId` (obrigatório), `showHistory` (opcional), e `showHistoryOutput` (opcional) como parâmetros. Se `showHistory` está definido como `true`, a resposta irá conter o histórico de execução. Se `showHistoryOutput` está definido como `true` bem, o histórico de execução irá conter saídas da atividade. O método devolve um objeto com as seguintes propriedades:

* **Nome**: O nome da função do orchestrator.
* **InstanceId**: O ID de instância do orchestration (deve ser o mesmo que o `instanceId` entrada).
* **CreatedTime**: A hora em que a função do orchestrator começou a ser executada.
* **LastUpdatedTime**: A hora em que o orchestration último checkpointed.
* **Entrada**: A entrada da função como um valor JSON.
* **CustomStatus**: Estado de orquestração personalizado no formato JSON. 
* **Saída**: O resultado da função como um valor JSON (se a função foi concluída). Se a função do orchestrator falhou, esta propriedade incluirá os detalhes da falha. Se a função do orchestrator foi terminada, esta propriedade incluirá o motivo fornecido para a terminação (se aplicável).
* **RuntimeStatus**: um dos seguintes valores:
    * **Executar**: A instância começar a ser executada.
    * **Concluir**: A instância concluída normalmente.
    * **ContinuedAsNew**: A instância reiniciou-se com um histórico de novo. Este é um estado transitório.
    * **Não foi possível**: A instância falhou com um erro.
    * **Terminada**: A instância abruptamente foi terminada.
* **Histórico**: O histórico de execução da orquestração. Este campo só está povoado se `showHistory` está definido como `true`.
    
Este método devolve `null` se a instância não existe ou ainda não foi iniciado em execução.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

## <a name="terminating-instances"></a>Instâncias de terminação

Uma instância de orquestração em execução pode ser terminada utilizando o [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) método o [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe. Os dois parâmetros são um `instanceId` e um `reason` cadeia, que será escrita nos registos e o estado de instância. Uma instância terminada deixa de ser executado assim que chegar a próxima `await` ponto ou terminará imediatamente se já está ativada uma `await`. 

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Terminação de instância não propagar atualmente. Funções de atividade e orchestrations secundárias serão executada para conclusão independentemente se a instância de orquestração chamado-los foi terminada.

## <a name="sending-events-to-instances"></a>Envio de eventos para instâncias

As notificações de eventos podem ser enviadas para executar instâncias utilizando o [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) método o [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe. As instâncias que podem processar estes eventos são aqueles que estão a aguardar uma chamada para [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

Os parâmetros de [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) são os seguintes:

* **InstanceId**: O ID exclusivo da instância.
* **EventName**: O nome do evento para enviar.
* **EventData**: um payload JSON serializável para enviar para a instância.

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!WARNING]
> Se não houver nenhuma instância de orquestração com especificado *ID de instância* ou se a instância não está à espera que o especificado na *nome do evento*, a mensagem de evento é rejeitada. Para obter mais informações sobre este comportamento, consulte o [problema no GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="wait-for-orchestration-completion"></a>Aguarde pela conclusão de orquestração

O [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe expõe um [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) API que pode ser utilizado para obter de forma síncrona a saída real de uma instância de orquestração. O método utiliza o valor predefinido de 10 segundos para `timeout` e 1 segundo para `retryInterval` quando não estão definidos.  

Eis um exemplo de função de Acionador de HTTP que demonstra como utilizar esta API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

A função pode ser chamada com a seguinte linha com o tempo limite de 2 segundos e o intervalo entre tentativas de 0,5 segundo:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Consoante o tempo necessário para obter a resposta da instância de orquestração, existem dois cenários:

1. As instâncias de orquestração concluída dentro do tempo limite definido (neste caso 2 segundos), a resposta é o resultado de instância de orquestração real fornecido de forma síncrona:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

2. As instâncias de orquestração não é possível concluir dentro do tempo limite definido (neste caso 2 segundos), a resposta é a predefinição um descrito **deteção de URL de HTTP da API**:

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> O formato do URL do webhook pode divergir, consoante a versão de anfitrião das funções do Azure está a executar. O exemplo anterior é para o anfitrião 2.0 de funções do Azure.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como utilizar as APIs de HTTP para a instância de gestão](durable-functions-http-api.md)
