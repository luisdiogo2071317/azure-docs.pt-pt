---
title: Gerir instâncias de funções duráveis - Azure
description: Saiba como gerir instâncias na extensão de funções duráveis para as funções do Azure.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: azfuncdf
ms.openlocfilehash: c3292651de7fba5a8f442f54f92d25fa6a97fe1a
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238702"
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Gerir instâncias de funções durável (funções do Azure)

[Funções duráveis](durable-functions-overview.md) instâncias de orquestração podem ser iniciadas, terminadas, consultadas e enviadas eventos de notificação. Todo o gerenciamento de instância é feito com o [ligação do cliente de orquestração](durable-functions-bindings.md). Este artigo vai os detalhes de cada operação de gestão de instância.

## <a name="starting-instances"></a>A iniciar as instâncias

O [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) método no [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) inicia uma nova instância de uma função de orquestrador. Instâncias desta classe podem ser adquiridas utilizando o `orchestrationClient` enlace. Internamente, esse método coloca em fila uma mensagem na fila de controle, que dispara o início de uma função com o nome especificado, que utiliza o `orchestrationTrigger` acionar a ligação. 

A tarefa é concluída quando o processo de orquestração é iniciado. O processo de orquestração deverá ser iniciado dentro de 30 segundos. Se demorar mais tempo, um `TimeoutException` é lançada. 

Os parâmetros a serem [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) são os seguintes:

* **Nome**: O nome da função de orquestrador para agendar.
* **Entrada**: todos os dados JSON serializável que devem ser passados como entrada para a função de orquestrador.
* **InstanceId**: (opcional) o exclusivo ID da instância. Se não for especificado, será gerado um ID de instância aleatório.

Eis um exemplo do c# simples:

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

Para ambientes não .NET idiomas, a função de enlace de saída pode ser utilizado para iniciar novas instâncias também. Neste caso, pode ser utilizado qualquer objeto serializável para o JSON que tenha os três parâmetros acima como campos. Por exemplo, considere a seguinte função de JavaScript:

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
O código acima parte do princípio de que, no ficheiro Function tem definido um enlace para fora com o nome como "starter" e o tipo como "orchestrationClient". Se a ligação não for definida, em seguida, a instância de função durável não será criada.

Para a função durável ser invocada a Function deve ser modificado para ter uma ligação para o cliente de orquestração, conforme descrito abaixo

```js
{
    "bindings": [{
        "name":"starter",
        "type":"orchestrationClient",
        "direction":"out"
    }]
}
```

> [!NOTE]
> Recomendamos que utilize um identificador aleatório para o ID de instância. Isto ajudará a garantir uma distribuição de carga igual ao dimensionar as funções do orchestrator em várias VMs. O tempo adequado para utilizar os IDs de instância não aleatórios é quando o ID tem de ser uma origem externa ou ao implementar o [singleton orchestrator](durable-functions-singletons.md) padrão.

## <a name="querying-instances"></a>Consultando instâncias

O [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) método no [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe consulta o estado de uma instância de orquestração. É necessário um `instanceId` (obrigatório), `showHistory` (opcional), e `showHistoryOutput` (opcional) como parâmetros. Se `showHistory` está definido como `true`, a resposta irá conter o histórico de execução. Se `showHistoryOutput` está definido como `true` além disso, o histórico de execução irá conter as saídas de atividade. O método retorna um objeto com as seguintes propriedades:

* **Nome**: O nome da função de orquestrador.
* **InstanceId**: O ID de instância da orquestração (deve ser o mesmo que o `instanceId` entrada).
* **CreatedTime**: A hora em que a função de orquestrador iniciou a execução.
* **LastUpdatedTime**: A hora em que a orquestração último foi efetuada a verificação.
* **Entrada**: A entrada da função como um valor JSON.
* **CustomStatus**: Estado de orquestração personalizado no formato JSON. 
* **Saída**: A saída da função como um valor JSON (se a função foi concluída). Se a função de orquestrador falhou, esta propriedade incluirá os detalhes da falha. Se a função de orquestrador foi terminada, esta propriedade irá incluir o motivo fornecido para a finalização (se houver).
* **RuntimeStatus**: um dos seguintes valores:
    * **Pendente**: A instância foi agendada, mas ainda não foi iniciada em execução.
    * **Executar**: A instância foi iniciada em execução.
    * **Concluído**: A instância foi concluída normalmente.
    * **ContinuedAsNew**: A instância reiniciou-se com um histórico de novo. Este é um estado transitório.
    * **Falha ao**: A instância falhou com um erro.
    * **Terminada**: A instância abruptamente foi terminada.
* **Histórico de**: O histórico de execução da orquestração. Este campo só é preenchido caso `showHistory` está definido como `true`.
    
Este método devolve `null` se a instância não existe ou ainda não foi iniciada em execução.

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
## <a name="querying-all-instances"></a>Consultar todas as instâncias

Pode utilizar o `GetStatusAsync` método para consultar os Estados de todas as instâncias de orquestração. Ele não tem nenhum parâmetro, ou pode passar um `CancellationToken` objeto caso queira cancelá-lo. O método retorna objetos com as mesmas propriedades como o `GetStatusAsync` método com parâmetros, exceto que não retorna o histórico. 

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    TraceWriter log)
{
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.Info(JsonConvert.SerializeObject(instance));
    };
}
```

## <a name="terminating-instances"></a>Instâncias de terminação

Uma instância em execução da orquestração pode ser terminada com o [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) método o [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe. Os dois parâmetros são um `instanceId` e um `reason` cadeia de caracteres, que será escrita nos registos e para o estado de instância. Uma instância terminada deixa de ser executado assim que atingir o próximo `await` ponto ou encerrar imediatamente se já está ativada uma `await`. 

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
> Terminação de instância não propaga atualmente. Funções de atividade e orquestrações secundárias serão executado para conclusão, independentemente se a instância de orquestração que os chamou foi terminada.

## <a name="sending-events-to-instances"></a>Envio de eventos para instâncias

Notificações de eventos podem ser enviadas para a executar instâncias com o [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) método o [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe. Instâncias que podem lidar com esses eventos são aqueles que estão aguardando uma chamada para [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

Os parâmetros a serem [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) são os seguintes:

* **InstanceId**: O ID exclusivo da instância.
* **EventName**: O nome do evento para enviar.
* **EventData**: um payload JSON serializável para enviar para a instância.

```csharp
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
> Se não existir nenhuma instância de orquestração com especificado *ID da instância* ou se a instância não está à espera no especificado *nome do evento*, a mensagem de evento é rejeitada. Para obter mais informações sobre este comportamento, consulte a [problema do GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="wait-for-orchestration-completion"></a>Aguardar pela conclusão de orquestração

O [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe expõe um [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) API que pode ser utilizada para obter o resultado real de forma síncrona de uma instância de orquestração. O método usa o valor predefinido de 10 segundos para `timeout` e 1 segundo para `retryInterval` quando não estão definidos.  

Eis um exemplo de função de Acionador de HTTP que demonstra como utilizar esta API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

A função pode ser chamada com a seguinte linha com o tempo limite de 2 segundos e o intervalo entre tentativas de 0,5 segundo:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Dependendo do tempo necessário para obter a resposta da instância de orquestração, existem dois casos:

1. As instâncias de orquestração concluída no tempo limite definido (neste caso 2 segundos), a resposta é a saída de instância da orquestração real entregue de forma síncrona:

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

2. As instâncias de orquestração não é possível concluir no tempo limite definido (neste caso 2 segundos), a resposta é a predefinição um descrito em **deteção de URL da API HTTP**:

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
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}",
            "rewindPostUri": "https://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/rewind?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> O formato dos URLs de webhook poderá ser diferente dependendo de qual versão do anfitrião de funções do Azure está a executar. No exemplo anterior é para o anfitrião do 2.0 de funções do Azure.

## <a name="retrieving-http-management-webhook-urls"></a>Obter URLs de Webhook de gestão de HTTP

Sistemas externos podem comunicar com funções duráveis por meio dos URLs de webhook que fazem parte da resposta padrão descrita em [deteção de URL da API HTTP](durable-functions-http-api.md). No entanto, os URLs de webhook também podem ser acessados por meio de programação no cliente de orquestração ou numa função de atividade através de [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) método da [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)classe. 

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) tem um parâmetro:

* **InstanceId**: O ID exclusivo da instância.

O método retorna uma instância do [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) com as seguintes propriedades de cadeia de caracteres:

* **ID**: O ID de instância da orquestração (deve ser o mesmo que o `InstanceId` entrada).
* **StatusQueryGetUri**: O URL de estado da instância de orquestração.
* **SendEventPostUri**: O URL de "emitir um evento" da instância de orquestração.
* **TerminatePostUri**: O URL "terminar" da instância de orquestração.
* **RewindPostUri**: O URL de "o recuo" da instância de orquestração.

Funções de atividade podem enviar uma instância do [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) a sistemas externos para monitorizar ou acionem eventos para uma orquestração:

```csharp
[FunctionName("SendInstanceInfo")]
public static void SendInstanceInfo(
    [ActivityTrigger] DurableActivityContext ctx,
    [OrchestrationClient] DurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

## <a name="rewinding-instances-preview"></a>Avanço rápido instances (pré-visualização)

Pode ser uma instância da orquestração com falhas *rewound* num anteriormente bom estado de funcionamento por meio do [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) API. Funciona ao colocar a orquestração de volta para o *em execução* estado e executar novamente as falhas de execução de atividade de e/ou orquestração secundárias que causou a falha de orquestração.

> [!NOTE]
> Esta API não se destina a ser uma substituição para o tratamento de erros adequado e políticas de repetição. Em vez disso, destina-se a ser usado apenas em casos em que as instâncias de orquestração falharem por motivos de inesperado. Para obter mais detalhes sobre políticas de repetição e manipulação de erro, consulte a [tratamento de erros](durable-functions-error-handling.md) tópico.

Um exemplo utilizar maiúsculas e minúsculas para *Retroceder* é um fluxo de trabalho que envolve uma série de [aprovações humanas](durable-functions-overview.md#pattern-5-human-interaction). Suponhamos que haja uma série de funções de atividade que notifique a pessoa que é necessária a sua aprovação e aguarde que a resposta em tempo real. Afinal de contas da aprovação, actividades recebeu as respostas ou o tempo limite, a falha de outra atividade devido a uma configuração incorreta do aplicativo (por exemplo, uma cadeia de ligação de base de dados inválido). O resultado é uma falha de orquestração profundamente o fluxo de trabalho. Com o `RewindAsync` API, o administrador de uma aplicação pode corrigir o erro de configuração e *Retroceder* a orquestração com falha de volta para o estado de imediatamente antes da falha. Nenhum dos passos de interação humana precisa de ser novamente aprovados e a orquestração pode agora ser concluída com êxito.

> [!NOTE]
> O *Retroceder* funcionalidade não suporta e instâncias de orquestração de avanço rápido, que utilizam os temporizadores duráveis.

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como utilizar as APIs de HTTP por exemplo, gestão](durable-functions-http-api.md)
