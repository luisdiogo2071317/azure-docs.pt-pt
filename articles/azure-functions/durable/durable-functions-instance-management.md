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
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: dba5518df34fa4fb6c403a72aa40405bcde5c426
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104682"
---
# <a name="manage-instances-in-durable-functions-in-azure"></a>Gerir instâncias de funções duráveis no Azure

[Funções duráveis](durable-functions-overview.md) instâncias de orquestração podem ser iniciadas, terminadas, consultadas e enviadas eventos de notificação. Todo o gerenciamento de instância é feito com o [ligação do cliente de orquestração](durable-functions-bindings.md). Este artigo vai os detalhes de cada operação de gestão de instância.

## <a name="starting-instances"></a>A iniciar as instâncias

O [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) método no [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) ou `startNew` no `DurableOrchestrationClient` (JavaScript) inicia uma nova instância de uma função de orquestrador. Instâncias desta classe podem ser adquiridas utilizando o `orchestrationClient` enlace. Internamente, esse método coloca em fila uma mensagem na fila de controle, que dispara o início de uma função com o nome especificado, que utiliza o `orchestrationTrigger` acionar a ligação.

Esta operação assíncrona é concluída quando o processo de orquestração é agendado com êxito. O processo de orquestração deverá ser iniciado dentro de 30 segundos. Se demorar mais tempo, um `TimeoutException` é lançada.

> [!WARNING]
> Ao desenvolver localmente em JavaScript, terá de definir a variável de ambiente `WEBSITE_HOSTNAME` para `localhost:<port>`, por ex. `localhost:7071` Para utilizar os métodos em `DurableOrchestrationClient`. Para obter mais informações sobre este requisito, consulte a [problema do GitHub](https://github.com/Azure/azure-functions-durable-js/issues/28).

### <a name="net"></a>.NET

Os parâmetros a serem [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) são os seguintes:

* **Nome**: O nome da função de orquestrador para agendar.
* **Entrada**: Todos os dados JSON serializável que devem ser passados como entrada para a função de orquestrador.
* **InstanceId**: (Opcional) O ID exclusivo da instância. Se não for especificado, será gerado um ID de instância aleatório.

Eis um exemplo do c# simples:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

Os parâmetros para `startNew` são os seguintes:

* **Nome**: O nome da função de orquestrador para agendar.
* **InstanceId**: (Opcional) O ID exclusivo da instância. Se não for especificado, será gerado um ID de instância aleatório.
* **Entrada**: (Opcional) Todos os dados JSON serializável que devem ser passados como entrada para a função de orquestrador.

Eis um exemplo de JavaScript simples:

```javascript
const df = require("durable-functions");

module.exports = async function(context, input) {
    const client = df.getClient(context);

    const instanceId = await client.startNew("HelloWorld", undefined, input);
    context.log(`Started orchestration with ID = ${instanceId}.`);
};
```

> [!NOTE]
> Utilizar um identificador aleatório para o ID de instância. Isto ajudará a garantir uma distribuição de carga igual ao dimensionar as funções do orchestrator em várias VMs. O tempo adequado para utilizar os IDs de instância não aleatórios é quando o ID tem de ser uma origem externa ou ao implementar o [singleton orchestrator](durable-functions-singletons.md) padrão.

### <a name="using-core-tools"></a>Usando as ferramentas de núcleo

Também é possível iniciar uma instância diretamente através da [ferramentas de núcleo de funções do Azure](../functions-run-local.md) `durable start-new` comando. Ele usa os seguintes parâmetros:

* **`function-name` (obrigatório)** : Nome da função para começar
* **`input` (opcional)** : Para a função, qualquer um dos inline ou através de um ficheiro JSON de entrada. Para os ficheiros, o caminho para o ficheiro com o prefixo `@`, tais como `@path/to/file.json`.
* **`id` (opcional)** : ID da instância de orquestração. Se não for indicado, é gerado um GUID aleatório.
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

> [!NOTE]
> Principais ferramentas comandos partem do princípio de que estão sendo executados no diretório raiz de uma aplicação de funções. Se `connection-string-setting` e `task-hub-name` são explicitamente fornecido, os comandos podem ser executados a partir de qualquer diretório. Embora estes comandos podem ser executados sem um anfitrião de aplicação de função em execução, alguns efeitos não podem ser observados, a menos que o anfitrião está em execução. Por exemplo, o `start-new` comando irá colocar uma mensagem de início para o hub de tarefa de destino, mas a orquestração não serão executados, na verdade, a menos que exista um função anfitrião processo da aplicação em execução que pode processar a mensagem.

O seguinte comando seria iniciar a função com o nome HelloWorld e transmita os conteúdos do ficheiro ' contador-data.json' para o mesmo:

```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="querying-instances"></a>Consultando instâncias

O [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) método no [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe (.NET) ou o `getStatus` método no `DurableOrchestrationClient` classe (JavaScript) consulta o estado de uma orquestração instância.

É necessário um `instanceId` (obrigatório), `showHistory` (opcional), `showHistoryOutput` (opcional), e `showInput` (opcional, apenas o .NET) como parâmetros.

* **`showHistory`**: Se definido como `true`, a resposta irá conter o histórico de execução.
* **`showHistoryOutput`**: Se definido como `true`, o histórico de execução conterá saídas de atividade.
* **`showInput`**: Se definido como `false`, a resposta não irá conter a entrada da função. O valor predefinido é `true`. (Apenas para o .NET)

O método retorna um objeto JSON com as seguintes propriedades:

* **Nome**: O nome da função de orquestrador.
* **InstanceId**: O ID de instância da orquestração (deve ser o mesmo que o `instanceId` entrada).
* **CreatedTime**: A hora em que a função de orquestrador iniciou a execução.
* **LastUpdatedTime**: O tempo em que a orquestração último foi efetuada a verificação.
* **Entrada**: A entrada da função como um valor JSON. Este campo não será preenchido se `showInput` é false.
* **CustomStatus**: Estado de orquestração personalizado no formato JSON.
* **Saída**: A saída da função como um valor JSON (se a função foi concluída). Se a função de orquestrador falhou, esta propriedade incluirá os detalhes da falha. Se a função de orquestrador foi terminada, esta propriedade irá incluir o motivo fornecido para a finalização (se houver).
* **RuntimeStatus**: Um dos seguintes valores:
  * **Pendente**: A instância foi agendada, mas ainda não foi iniciada em execução.
  * **Executar**: A instância foi iniciado em execução.
  * **Concluído**: A instância foi concluída normalmente.
  * **ContinuedAsNew**: A instância reiniciou-se com um histórico de novo. Este é um estado transitório.
  * **Falha ao**: A instância falhou com um erro.
  * **Terminada**: A instância foi interrompida abruptamente.
* **Histórico de**: O histórico de execução da orquestração. Este campo só é preenchido caso `showHistory` está definido como `true`.

Este método devolve `null` se a instância não existe ou ainda não foi iniciada em execução.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const status = await client.getStatus(instanceId);
    // do something based on the current status.
}
```

### <a name="using-core-tools"></a>Usando as ferramentas de núcleo

Também é possível obter o estado de uma instância da orquestração diretamente através da [ferramentas de núcleo de funções do Azure](../functions-run-local.md) `durable get-runtime-status` comando. Ele usa os seguintes parâmetros:

* **`id` (obrigatório)** : ID da instância de orquestração
* **`show-input` (opcional)** : Se definido como `true`, a resposta irá conter a entrada da função. O valor predefinido é `false`.
* **`show-output` (opcional)** : Se definido como `true`, a resposta irá conter a saída da função. O valor predefinido é `false`.
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

O seguinte comando teria de obter o estado (incluindo a entrada e saída) de uma instância com um ID de instância da orquestração do 0ab8c55a66644d68a3a8b220b12d209c. Ele supõe que o `func` comando está a ser executado a partir do diretório de raiz da aplicação de função:

```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

O `durable get-history` comando pode ser utilizado para obter o histórico de uma instância de orquestração. Ele usa os seguintes parâmetros:

* **`id` (obrigatório)** : ID da instância de orquestração
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido no Host. JSON por meio de durableTask:HubName.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="querying-all-instances"></a>Consultar todas as instâncias

Pode utilizar o `GetStatusAsync` (.NET) ou `getStatusAll` método (JavaScript) para consultar os Estados de todas as instâncias de orquestração. No .NET pode passar um `CancellationToken` objeto caso queira cancelá-lo. O método retorna objetos com as mesmas propriedades como o `GetStatusAsync` método com parâmetros.

### <a name="c"></a>C#

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await client.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instances = await client.getStatusAll();
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

### <a name="using-core-tools"></a>Usando as ferramentas de núcleo

Também é possível para instâncias de consulta diretamente através da [ferramentas de núcleo de funções do Azure](../functions-run-local.md) `durable get-instances` comando. Ele usa os seguintes parâmetros:

* **`top` (opcional)** : Este comando suporta paginação. Este parâmetro corresponde ao número de instâncias obtida por cada pedido. A predefinição é 10.
* **`continuation-token` (opcional)** : Um token para indicar qual página de seção de instâncias para recuperar. Cada `get-instances` execução devolve um token para o próximo conjunto de instâncias.
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable get-instances
```

## <a name="querying-instances-with-filters"></a>Consultando instâncias com filtros

Também pode utilizar o `GetStatusAsync` (.NET) ou `getStatusBy` predefinidas de método (JavaScript) para obter uma lista de instâncias de orquestração que correspondem a um conjunto de filtros. Opções de filtro possíveis incluem o tempo de criação de orquestração e o estado de tempo de execução da orquestração.

### <a name="c"></a>C#

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IEnumerable<OrchestrationRuntimeStatus> runtimeStatus = new List<OrchestrationRuntimeStatus> {
        OrchestrationRuntimeStatus.Completed,
        OrchestrationRuntimeStatus.Running
    };
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(
        new DateTime(2018, 3, 10, 10, 1, 0),
        new DateTime(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    ); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const runtimeStatus = [
        df.OrchestrationRuntimeStatus.Completed,
        df.OrchestrationRuntimeStatus.Running,
    ];
    const instances = await client.getStatusBy(
        new Date(2018, 3, 10, 10, 1, 0),
        new Date(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    );
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

### <a name="using-the-functions-core-tools"></a>Usando as ferramentas de núcleo de funções

O `durable get-instances` comando também pode ser utilizado com filtros. Para além do mencionados anteriormente `top`, `continuation-token`, `connection-string-setting`, e `task-hub-name` parâmetros, três parâmetros de filtro (`created-after`, `created-before`, e `runtime-status`), pode ser utilizado.

* **`created-after` (opcional)** : Obter as instâncias criadas após esta data/hora (UTC). ISO 8601 formatado datetimes aceites.
* **`created-before` (opcional)** : Obter as instâncias criadas antes desta data/hora (UTC). ISO 8601 formatado datetimes aceites.
* **`runtime-status` (opcional)** : Obter as instâncias cujo estado corresponder a estas ("em execução", "concluído", etc.). Pode fornecer vários Estados de (espaço separado).
* **`top` (opcional)** : Número de instâncias de obter por pedido. A predefinição é 10.
* **`continuation-token` (opcional)** : Um token para indicar qual página de seção de instâncias para recuperar. Cada `get-instances` execução devolve um token para o próximo conjunto de instâncias.
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

Se não existem filtros (`created-after`, `created-before`, ou `runtime-status`) são fornecidos, em seguida, `top` instâncias serão obtidas sem considerar para o tempo de criação ou de estado de runtime.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
```

## <a name="terminating-instances"></a>Instâncias de terminação

Uma instância em execução da orquestração pode ser terminada com o [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) método da [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe (.NET), ou o `terminate` método o `DurableOrchestrationClient` classe ( JavaScript). Os dois parâmetros são um `instanceId` e um `reason` cadeia de caracteres, que será escrita nos registos e para o estado de instância. Uma instância terminada deixa de ser executado assim que atingir o próximo `await` (.NET) ou `yield` ponto (JavaScript) ou será encerrado imediatamente se já está ativada uma `await` (.NET) ou `yield` (JavaScript).

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "It was time to be done.";
    return client.terminate(instanceId, reason);
};
```

> [!NOTE]
> Terminação de instância não propaga atualmente. Funções de atividade e orquestrações secundárias serão executado para conclusão, independentemente se a instância de orquestração que os chamou foi terminada.

### <a name="using-core-tools"></a>Usando as ferramentas de núcleo

Também é possível terminar uma instância da orquestração diretamente através da [ferramentas de núcleo](../functions-run-local.md) `durable terminate` comando. Ele usa os seguintes parâmetros:

* **`id` (obrigatório)** : ID da instância de orquestração, para terminar
* **`reason` (opcional)** : Motivo de encerramento
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

Encerra uma instância de orquestração com o ID de 0ab8c55a66644d68a3a8b220b12d209c o seguinte comando:

```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="sending-events-to-instances"></a>Envio de eventos para instâncias

Notificações de eventos podem ser enviadas para a executar instâncias com o [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) método da [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe (.NET) ou o `raiseEvent` método da `DurableOrchestrationClient` (de classe JavaScript). Instâncias que podem lidar com esses eventos são aqueles que estão aguardando uma chamada para [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) (.NET) ou `waitForExternalEvent` (JavaScript).

Os parâmetros a serem [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) (.NET) e `raiseEvent` (JavaScript) são os seguintes:

* **InstanceId**: O ID exclusivo da instância.
* **EventName**: O nome do evento para enviar.
* **EventData**: Um payload JSON serializável para enviar para a instância.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const eventData = [ 1, 2, 3 ];
    return client.raiseEvent(instanceId, "MyEvent", eventData);
};
```

> [!WARNING]
> Se não existir nenhuma instância de orquestração com especificado *ID da instância* ou se a instância não está à espera no especificado *nome do evento*, a mensagem de evento é rejeitada. Para obter mais informações sobre este comportamento, consulte a [problema do GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/29).

### <a name="using-core-tools"></a>Usando as ferramentas de núcleo

Também é possível gerar um evento para uma instância da orquestração diretamente através da [ferramentas de núcleo](../functions-run-local.md) `durable raise-event` comando. Ele usa os seguintes parâmetros:

* **`id` (obrigatório)** : ID da instância de orquestração
* **`event-name` (opcional)** : Nome do evento para gerar. A predefinição é `$"Event_{RandomGUID}"`
* **`event-data` (opcional)** : Dados para enviar para a instância de orquestração. Isso pode ser o caminho para um ficheiro JSON ou os dados podem ser fornecidos diretamente na linha de comandos
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```

```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Aguardar pela conclusão de orquestração

O [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe expõe um [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) API em .NET que pode ser utilizado para obter o resultado real de forma síncrona de uma orquestração instância. No JavaScript, o `DurableOrchestrationClient` classe expõe um `waitForCompletionOrCreateCheckStatusResponse` API com o mesmo objetivo. Os métodos usam um valor predefinido de 10 segundos para `timeout` e 1 segundo para `retryInterval` quando não estão definidos.  

Eis um exemplo de função de Acionador de HTTP que demonstra como utilizar esta API:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpSyncStart/index.js)]

A função pode ser chamada com a seguinte linha com o tempo limite de 2 segundos e o intervalo entre tentativas de 0,5 segundo:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Dependendo do tempo necessário para obter a resposta da instância de orquestração, existem dois casos:

* As instâncias de orquestração concluída no tempo limite definido (neste caso 2 segundos), a resposta é a saída de instância da orquestração real entregue de forma síncrona:

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

* As instâncias de orquestração não é possível concluir no tempo limite definido (neste caso 2 segundos), a resposta é a predefinição um descrito em **deteção de URL da API HTTP**:

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
> O formato dos URLs de webhook poderá ser diferente dependendo de qual versão do anfitrião de funções do Azure está a executar. No exemplo anterior é para o anfitrião de 2.x as funções do Azure.

## <a name="retrieving-http-management-webhook-urls"></a>Obter URLs de Webhook de gestão de HTTP

Sistemas externos podem comunicar com funções duráveis por meio dos URLs de webhook que fazem parte da resposta padrão descrita em [deteção de URL da API HTTP](durable-functions-http-api.md). No entanto, os URLs de webhook também podem ser acessados por meio de programação no cliente de orquestração ou numa função de atividade através de [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) método da [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)classe (.NET) ou o `createHttpManagementPayload` método da `DurableOrchestrationClient` classe (JavaScript).

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) e `createHttpManagementPayload` ter um parâmetro:

* **instanceId**: O ID exclusivo da instância.

Os métodos de retornam uma instância de [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) (.NET) ou um objeto (JavaScript) com as seguintes propriedades de cadeia de caracteres:

* **Id**: O ID de instância da orquestração (deve ser o mesmo que o `InstanceId` entrada).
* **StatusQueryGetUri**: O URL de estado da instância de orquestração.
* **SendEventPostUri**: O URL de "emitir um evento" da instância de orquestração.
* **TerminatePostUri**: O URL da instância de orquestração "terminar".
* **RewindPostUri**: O URL de "o recuo" da instância de orquestração.

Funções de atividade podem enviar uma instância desses objetos para sistemas externos para monitorizar ou acionem eventos para uma orquestração:

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, ctx) {
    const client = df.getClient(context);

    const payload = client.createHttpManagementPayload(ctx.instanceId);

    // send the payload to Cosmos DB
    context.bindings.document = JSON.stringify({
        id: ctx.instanceId,
        payload,
    });
};
```

## <a name="rewinding-instances-preview"></a>Avanço rápido instances (pré-visualização)

Pode ser uma instância da orquestração com falhas *rewound* num anteriormente bom estado de funcionamento por meio do [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) (.NET) ou `rewindAsync` (JavaScript) API. Funciona ao colocar a orquestração de volta para o *em execução* estado e executar novamente as falhas de execução de atividade de e/ou suborchestration que causou a falha de orquestração.

> [!NOTE]
> Esta API não se destina a ser uma substituição para o tratamento de erros adequado e políticas de repetição. Em vez disso, destina-se a ser usado apenas em casos em que as instâncias de orquestração falharem por motivos de inesperado. Para obter mais detalhes sobre políticas de repetição e manipulação de erro, consulte a [tratamento de erros](durable-functions-error-handling.md) tópico.

Um exemplo utilizar maiúsculas e minúsculas para *Retroceder* é um fluxo de trabalho que envolve uma série de [aprovações humanas](durable-functions-concepts.md#human). Suponhamos que haja uma série de funções de atividade que notifique a pessoa que é necessária a sua aprovação e aguarde que a resposta em tempo real. Afinal de contas da aprovação, actividades recebeu as respostas ou o tempo limite, a falha de outra atividade devido a um erro de configuração do aplicativo, como uma cadeia de ligação de base de dados inválido. O resultado é uma falha de orquestração profundamente o fluxo de trabalho. Com o `RewindAsync` (.NET) ou `rewindAsync` (API de JavaScript), o administrador de uma aplicação pode corrigir o erro de configuração e *Retroceder* a orquestração com falha de volta para o estado de imediatamente antes da falha. Nenhum dos passos de interação humana tem de ser aprovado novamente e a orquestração pode agora ser concluída com êxito.

> [!NOTE]
> O *Retroceder* funcionalidade não suporta e instâncias de orquestração de avanço rápido, que utilizam os temporizadores duráveis.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "Orchestrator failed and needs to be revived.";
    return client.rewind(instanceId, reason);
};
```

### <a name="using-core-tools"></a>Usando as ferramentas de núcleo

Também é possível retroceder uma instância da orquestração diretamente através da [ferramentas de núcleo](../functions-run-local.md) `durable rewind` comando. Ele usa os seguintes parâmetros:

* **`id` (obrigatório)** : ID da instância de orquestração
* **`reason` (opcional)** : Motivo para o avanço rápido a instância de orquestração
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Histórico de instância

> [!NOTE]
> O `PurgeInstanceHistoryAsync` API está atualmente disponível apenas para C#. Este será adicionado ao JavaScript numa versão futura.

Histórico de orquestração pode ser removido utilizando [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_). A funcionalidade irá remover todos os dados associados a uma orquestração - linhas de tabelas do Azure e blobs de mensagem grande caso existam. O método tem duas sobrecargas. Primeiro remove histórico pelo ID da instância da orquestração:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

O segundo exemplo mostra uma função acionada por temporizador que remove o histórico de todas as instâncias de orquestração concluída após o intervalo de tempo especificado. Neste caso, ele irá remover dados para todas as instâncias concluídas 30 ou mais dias atrás. Está agendado para ser executado uma vez por dia às 12h, mas:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [TimerTrigger("0 0 12 * * *")]TimerInfo myTimer)
{
    return client.PurgeInstanceHistoryAsync(
                    DateTime.MinValue,
                    DateTime.UtcNow.AddDays(-30),  
                    new List<OrchestrationStatus>
                    {
                        OrchestrationStatus.Completed
                    });
}
```

> [!NOTE]
> O *PurgeInstanceHistory* sobrecarga aceitar o período de tempo, como o parâmetro irá processar apenas as instâncias de orquestração num do Estado de tempo de execução - concluído, Terminated ou com falhas.

### <a name="using-core-tools"></a>Usando as ferramentas de núcleo

É possível remover o histórico de uma instância da orquestração com o [ferramentas de núcleo](../functions-run-local.md) `durable purge-history` comando. Semelhante ao segundo C# exemplo acima, ele remove o histórico de todas as instâncias de orquestração criados durante um intervalo de tempo especificado. As instâncias removidas podem ser mais filtradas por Estado de runtime. O comando tem vários parâmetros:

* **`created-after` (opcional)** : Remover o histórico de instâncias criadas após esta data/hora (UTC). ISO 8601 formatado datetimes aceites.
* **`created-before` (opcional)** : Remover o histórico de instâncias criadas antes desta data/hora (UTC). ISO 8601 formatado datetimes aceites.
* **`runtime-status` (opcional)** : Remover o histórico de instâncias cujo estado corresponder a estas ("em execução", "concluído", etc.). Pode fornecer vários Estados de (espaço separado).
* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

O seguinte comando teria de eliminar o histórico de todas as instâncias com falha criada antes de 14 de Novembro de 2018 em 7:35 PM (UTC).

```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="deleting-a-task-hub"></a>A eliminar um Hub de tarefa

Utilizar o [ferramentas de núcleo](../functions-run-local.md) `durable delete-task-hub` de comando, é possível eliminar todos os artefactos de armazenamento associados a um hub de tarefa específica. Isto inclui as tabelas de armazenamento do Azure, filas e blobs. O comando tem dois parâmetros:

* **`connection-string-setting` (opcional)** : Nome da definição de aplicação que contém a cadeia de ligação de armazenamento a utilizar. A predefinição é AzureWebJobsStorage.
* **`task-hub-name` (opcional)** : Nome do hub durável tarefas a utilizar. A predefinição é DurableFunctionsHub. Também pode ser definido nas [Host. JSON](durable-functions-bindings.md#host-json) via durableTask:HubName.

O comando seguinte eliminar todos os dados de armazenamento do Azure associados com o hub de tarefa 'UserTest'.

```bash
func durable delete-task-hub --task-hub-name UserTest
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como utilizar as APIs de HTTP por exemplo, gestão](durable-functions-http-api.md)
