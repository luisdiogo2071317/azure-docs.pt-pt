---
title: APIs de HTTP nas funções duráveis - Azure
description: Saiba como implementar HTTP APIs na extensão de funções duráveis para as funções do Azure.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 3fa4f230f5e2d15e815c47792c3955aa93d29fc4
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094744"
---
# <a name="http-apis-in-durable-functions-azure-functions"></a>APIs de HTTP nas funções duráveis (funções do Azure)

A extensão de tarefas durável expõe um conjunto de APIs HTTP que podem ser utilizados para realizar as seguintes tarefas:

* Obter o estado de uma instância de orquestração.
* Envie um evento para uma instância de orquestração espera.
* Termine uma instância de orquestração em execução.


Cada uma dessas APIs de HTTP é uma operação de webhook diretamente manipulada pela extensão de tarefas durável. Eles não são específicos para qualquer função na function app.

> [!NOTE]
> Estas operações também podem ser invocadas diretamente usando as APIs de gestão de instância no [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe. Para obter mais informações, consulte [gerenciamento de instância](durable-functions-instance-management.md).

## <a name="http-api-url-discovery"></a>Deteção de URL da API de HTTP

O [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) classe expõe um [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_) API que pode ser utilizado para gerar um payload de resposta HTTP que contém ligações para todas as operações suportadas. Eis um exemplo de função de Acionador de HTTP que demonstra como utilizar esta API:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

Esta função de exemplo produz os seguintes dados de resposta JSON. O tipo de dados de todos os campos é `string`.

| Campo             |Descrição                           |
|-------------------|--------------------------------------|
| ID                |O ID da instância de orquestração. |
| statusQueryGetUri |O URL de estado da instância de orquestração. |
| sendEventPostUri  |O URL de "emitir um evento" da instância de orquestração. |
| terminatePostUri  |O URL da instância de orquestração "terminar". |

Esta é uma resposta de exemplo:

```http
HTTP/1.1 202 Accepted
Content-Length: 923
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX

{
    "id":"34ce9a28a6834d8492ce6a295f1a80e2",
    "statusQueryGetUri":"https://{host}/runtime/webhooks/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "sendEventPostUri":"https://{host}/runtime/webhooks/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection=Storage&code=XXX",
    "terminatePostUri":"https://{host}/runtime/webhooks/DurableTaskExtension/instances/34ce9a28a6834d8492ce6a295f1a80e2/terminate?reason={text}&taskHub=DurableFunctionsHub&connection=Storage&code=XXX"
}
```
> [!NOTE]
> O formato dos URLs de webhook poderá ser diferente dependendo de qual versão do anfitrião de funções do Azure está a executar. O exemplo acima é para o anfitrião do 2.0 de funções do Azure.

## <a name="async-operation-tracking"></a>Operação assíncrona de controlo

A resposta HTTP mencionada anteriormente foi concebida para ajudar a implementar longa HTTP async APIs com funções duráveis. Isso às vezes, é referido como o *de consulta padrão de consumidor*. O fluxo de cliente/servidor funciona da seguinte forma:

1. O cliente emite uma solicitação HTTP para iniciar um processo de execução longa, como uma função de orquestrador.
2. O acionador HTTP de destino devolve uma resposta de HTTP 202 com um `Location` cabeçalho com o `statusQueryGetUri` valor.
3. O cliente consulta no URL no `Location` cabeçalho. Ele continua ver respostas HTTP 202 com um `Location` cabeçalho.
4. Quando a instância é concluída (ou falha), o ponto final no `Location` retorna de cabeçalho HTTP 200.

Esse protocolo permite coordenar processos de execução longa com clientes externos ou serviços que suportam um ponto final HTTP a consultar e seguir o `Location` cabeçalho. As partes fundamentais já são criadas para as APIs de HTTP de funções durável.

> [!NOTE]
> Por predefinição, todas as ações com base em HTTP fornecida pela [do Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) suportam o padrão de operação assíncrona padrão. Esta capacidade torna possível incorporar uma função de durável de longa execução como parte de um fluxo de trabalho do Logic Apps. Obter mais detalhes sobre o Logic Apps suportam para padrões HTTP assíncronos podem ser encontrados no [documentação de ações e acionadores de fluxo de trabalho do Azure Logic Apps](../logic-apps/logic-apps-workflow-actions-triggers.md#asynchronous-patterns).

## <a name="http-api-reference"></a>Referência da API de HTTP

Todas as APIs de HTTP implementados, com a extensão take, os seguintes parâmetros. O tipo de dados de todos os parâmetros é `string`.

| Parâmetro  | Tipo de parâmetro  | Descrição |
|------------|-----------------|-------------|
| instanceId | do IdP             | O ID da instância de orquestração. |
| taskHub    | Cadeia de consulta    | O nome da [hub tarefas](durable-functions-task-hubs.md). Se não for especificado, é assumido o nome do hub de tarefas da aplicação de função atual. |
| ligação | Cadeia de consulta    | O **nome** a cadeia de ligação para a conta de armazenamento. Se não for especificado, é assumida a cadeia de ligação predefinido para a aplicação de funções. |
| systemKey  | Cadeia de consulta    | A chave de autorização necessária para invocar a API. |
| showHistory| Cadeia de consulta    | Parâmetro opcional. Se definido como `true`, o histórico de execução da orquestração será incluído no payload de resposta.| 
| showHistoryOutput| Cadeia de consulta    | Parâmetro opcional. Se definido como `true`, a atividade produz serão incluídos no histórico de execução de orquestração.| 

`systemKey` é uma chave de autorização gerado automaticamente pelo host as funções do Azure. Concede acesso para a extensão de tarefas durável APIs especificamente e podem ser gerida da mesma forma que [outras chaves de autorização](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Key-management-API). A forma mais simples para detetar os `systemKey` valor é usando o `CreateCheckStatusResponse` API mencionado anteriormente.

As secções seguintes abordam específicos de HTTP APIs suportada pela extensão e fornecem exemplos de como pode ser utilizados.

### <a name="get-instance-status"></a>Obter estado da instância

Obtém o estado de uma instância da orquestração especificado.

#### <a name="request"></a>Pedir

Para as funções 1.0, o formato do pedido é o seguinte:

```http
GET /admin/extensions/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}
```

O formato do Functions 2.0 tem todos os mesmos parâmetros, mas tem um prefixo de URL ligeiramente diferente:

```http
GET /runtime/webhooks/DurableTaskExtension/instances/{instanceId}?taskHub={taskHub}&connection={connection}&code={systemKey}&showHistory={showHistory}&showHistoryOutput={showHistoryOutput}
```

#### <a name="response"></a>Resposta

Vários valores de código de estado possível podem ser devolvidos.

* **HTTP 200 (OK)**: A instância especificada é um estado concluído.
* **HTTP 202 (aceite)**: A instância especificada está em curso.
* **O HTTP 400 (pedido incorreto)**: A instância especificada falhou ou foi terminada.
* **HTTP 404 (não encontrado)**: A instância especificada não existe ou não iniciado em execução.

O payload de resposta para o **HTTP 200** e **HTTP 202** casos é um objeto JSON com os seguintes campos:

| Campo           | Tipo de dados | Descrição |
|-----------------|-----------|-------------|
| runtimeStatus   | cadeia    | O estado de tempo de execução da instância. Os valores incluem *em execução*, *pendente*, *falha*, *cancelado*, *Terminated*, *Concluída*. |
| entrada           | JSON      | Os dados JSON utilizados para inicializar a instância. |
| customStatus    | JSON      | Os dados JSON utilizados para o estado de orquestração personalizado. Este campo é `null` se não for definido. |
| saída          | JSON      | A saída JSON da instância. Este campo é `null` se a instância não é um estado concluído. |
| createdTime     | cadeia    | A hora em que a instância foi criada. Utiliza o ISO 8601 estendido notação. |
| lastUpdatedTime | cadeia    | A hora em que a instância mantidas pela última vez. Utiliza o ISO 8601 estendido notação. |
| historyEvents   | JSON      | Uma matriz JSON que contém o histórico de execução da orquestração. Este campo é `null` , a menos que o `showHistory` parâmetro de cadeia de caracteres de consulta está definido como `true`.  | 

Aqui está um payload de resposta de exemplo incluindo as orquestração histórico e a atividade saídas de execução (formatadas para legibilidade):

```json
{
  "createdTime": "2018-02-28T05:18:49Z",
  "historyEvents": [
      {
          "EventType": "ExecutionStarted",
          "FunctionName": "E1_HelloSequence",
          "Timestamp": "2018-02-28T05:18:49.3452372Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Tokyo!",
          "ScheduledTime": "2018-02-28T05:18:51.3939873Z",
          "Timestamp": "2018-02-28T05:18:52.2895622Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello Seattle!",
          "ScheduledTime": "2018-02-28T05:18:52.8755705Z",
          "Timestamp": "2018-02-28T05:18:53.1765771Z"
      },
      {
          "EventType": "TaskCompleted",
          "FunctionName": "E1_SayHello",
          "Result": "Hello London!",
          "ScheduledTime": "2018-02-28T05:18:53.5170791Z",
          "Timestamp": "2018-02-28T05:18:53.891081Z"
      },
      {
          "EventType": "ExecutionCompleted",
          "OrchestrationStatus": "Completed",
          "Result": [
              "Hello Tokyo!",
              "Hello Seattle!",
              "Hello London!"
          ],
          "Timestamp": "2018-02-28T05:18:54.3660895Z"
      }
  ],
  "input": null,
  "customStatus": { "nextActions": ["A", "B", "C"], "foo": 2 },
  "lastUpdatedTime": "2018-02-28T05:18:54Z",
  "output": [
      "Hello Tokyo!",
      "Hello Seattle!",
      "Hello London!"
  ],
  "runtimeStatus": "Completed"
}
```

O **HTTP 202** resposta também inclui um **localização** cabeçalho de resposta que referencia o mesmo URL que o `statusQueryGetUri` campo mencionado anteriormente.

### <a name="get-all-instances-status"></a>Obter o estado de todas as instâncias

Também pode consultar todos os Estados de instâncias. Remover o `instanceId` no pedido "Obter estado da instância". Os parâmetros são os mesmos que o "Estado de instância Get". 

#### <a name="request"></a>Pedir

Para as funções 1.0, o formato do pedido é o seguinte:

```http
GET /admin/extensions/DurableTaskExtension/instances/?taskHub={taskHub}&connection={connection}&code={systemKey}
```

O formato do Functions 2.0 tem todos os mesmos parâmetros, mas um prefixo de URL ligeiramente diferente: 

```http
GET /runtime/webhooks/DurableTaskExtension/instances/?taskHub={taskHub}&connection={connection}&code={systemKey}
```

#### <a name="response"></a>Resposta

Eis um exemplo de payloads de resposta, incluindo o estado de orquestração (formatado para legibilidade):

```json
[
    {
        "instanceId": "7af46ff000564c65aafbfe99d07c32a5",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:39Z",
        "lastUpdatedTime": "2018-06-04T10:46:47Z"
    },
    {
        "instanceId": "80eb7dd5c22f4eeba9f42b062794321e",
        "runtimeStatus": "Running",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:28Z",
        "lastUpdatedTime": "2018-06-04T15:18:38Z"
    },
    {
        "instanceId": "9124518926db408ab8dfe84822aba2b1",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-06-04T10:46:54Z",
        "lastUpdatedTime": "2018-06-04T10:47:03Z"
    },
    {
        "instanceId": "d100b90b903c4009ba1a90868331b11b",
        "runtimeStatus": "Pending",
        "input": null,
        "customStatus": null,
        "output": null,
        "createdTime": "2018-06-04T15:18:39Z",
        "lastUpdatedTime": "2018-06-04T15:18:39Z"
    }
]
```

> [!NOTE]
> Esta operação pode ser muito cara em termos de e/s de armazenamento de Azure se existirem muitas linhas da tabela de instâncias. Podem encontrar mais detalhes na tabela de instância na [desempenho e dimensionamento nas funções durável (funções do Azure)](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-perf-and-scale#instances-table) documentação.
> 

### <a name="raise-event"></a>Gerar evento

Envia uma mensagem de notificação de evento para uma instância de orquestração em execução.

#### <a name="request"></a>Pedir

Para as funções 1.0, o formato do pedido é o seguinte:

```http
POST /admin/extensions/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

O formato do Functions 2.0 tem todos os mesmos parâmetros, mas tem um prefixo de URL ligeiramente diferente:

```http
POST /runtime/webhooks/DurableTaskExtension/instances/{instanceId}/raiseEvent/{eventName}?taskHub=DurableFunctionsHub&connection={connection}&code={systemKey}
```

Parâmetros para esta API incluem o conjunto padrão mencionado anteriormente, bem como os seguintes parâmetros exclusivos do pedido:

| Campo       | Tipo de parâmetro  | TType de dados | Descrição |
|-------------|-----------------|-----------|-------------|
| eventName   | do IdP             | cadeia    | O nome do evento que esteja aguardando a instância de orquestração de destino. |
| {content}   | Conteúdo do pedido | JSON      | O payload do evento formatada em JSON. |

#### <a name="response"></a>Resposta

Vários valores de código de estado possível podem ser devolvidos.

* **HTTP 202 (aceite)**: O evento registado foi aceite para processamento.
* **O HTTP 400 (pedido incorreto)**: O conteúdo do pedido não era do tipo `application/json` ou não era um JSON válido.
* **HTTP 404 (não encontrado)**: A instância especificada não foi encontrada.
* **HTTP 410 (já não existe)**: A instância especificada foi concluída ou falhada e não pode processar todos os eventos gerados.

Eis um exemplo de solicitação que envia a cadeia de caracteres do JSON `"incr"` a uma instância à espera de um evento chamado **operação**:

```
POST /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/raiseEvent/operation?taskHub=DurableFunctionsHub&connection=Storage&code=XXX
Content-Type: application/json
Content-Length: 6

"incr"
```

As respostas para esta API não contêm qualquer conteúdo.

### <a name="terminate-instance"></a>Encerrar instância

Termina uma instância de orquestração em execução.

#### <a name="request"></a>Pedir

Para as funções 1.0, o formato do pedido é o seguinte:

```http
DELETE /admin/extensions/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

O formato do Functions 2.0 tem todos os mesmos parâmetros, mas tem um prefixo de URL ligeiramente diferente:

```http
DELETE /runtime/webhooks/DurableTaskExtension/instances/{instanceId}/terminate?reason={reason}&taskHub={taskHub}&connection={connection}&code={systemKey}
```

O pedido parâmetros para esta API incluem o conjunto padrão mencionado anteriormente, bem como o seguinte parâmetro exclusivo.

| Campo       | Tipo de parâmetro  | Tipo de Dados | Descrição |
|-------------|-----------------|-----------|-------------|
| reason      | Cadeia de consulta    | cadeia    | Opcional. O motivo para a instância da orquestração a terminar. |

#### <a name="response"></a>Resposta

Vários valores de código de estado possível podem ser devolvidos.

* **HTTP 202 (aceite)**: O pedido de encerramento foi aceite para processamento.
* **HTTP 404 (não encontrado)**: A instância especificada não foi encontrada.
* **HTTP 410 (já não existe)**: A instância especificada foi concluída ou falhada.

Eis um exemplo de solicitação que termina uma instância em execução e especifica um motivo de **buggy**:

```
DELETE /admin/extensions/DurableTaskExtension/instances/bcf6fb5067b046fbb021b52ba7deae5a/terminate?reason=buggy&taskHub=DurableFunctionsHub&connection=Storage&code=XXX
```

As respostas para esta API não contêm qualquer conteúdo.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como lidar com erros](durable-functions-error-handling.md)
