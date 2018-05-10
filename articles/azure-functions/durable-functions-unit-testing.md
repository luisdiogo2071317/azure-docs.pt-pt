---
title: Unidade de durável funções do Azure de teste
description: Saiba como a unidade de teste funções durável.
services: functions
author: kadimitr
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/28/2018
ms.author: kadimitr
ms.openlocfilehash: 7de9a6f0d4dfcb45932b89504c0d38c3c70283e9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="durable-functions-unit-testing"></a>Unidade de funções durável teste

Unidade de teste é uma parte importante das práticas de desenvolvimento de software moderna. Testes de unidade verificar o comportamento de lógica de negócio e proteger a introdução de alterações despercebidas no futuro. Funções duráveis facilmente podem aumentar em complexidade para que testes de unidade introdução às irão ajudar a evitar interrompendo as alterações. As secções seguintes explicam como a unidade testar os tipos de três função - cliente, o Orchestrator e atividade Orchestration funções. 

## <a name="prerequisites"></a>Pré-requisitos

Os exemplos neste artigo requerem conhecimentos dos conceitos e estruturas que se seguem: 

* Teste de unidade

* Funções Duráveis 

* [xUnit](https://xunit.github.io/) -teste framework

* [moq](https://github.com/moq/moq4) -Mocking framework


## <a name="base-classes-for-mocking"></a>Classes base para mocking 

Mocking é suportada através de duas classes abstratas durável funções:

* [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) 

* [DurableOrchestrationContextBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContextBase.html)

Estas classes são classes base para [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) e [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) que definir métodos Orchestration cliente e do Orchestrator. Os mocks irão definir o comportamento esperado para métodos de classe base para que o teste da unidade pode verificar a lógica de negócio. Há um fluxo de trabalho em dois passos para a unidade lógica de negócio de teste no cliente de orquestração e do Orchestrator:

1. Utilize as classes base em vez da implementação concreta quando se definem as assinaturas de cliente de orquestração e do Orchestrator.
2. Nos testes de unidade mock o comportamento das base classes e certifique-se a lógica de negócio. 

Obter mais detalhes nos parágrafos seguintes para testar as funções que utilizam o cliente de orquestração do enlace e o orchestrator acionam o enlace.

## <a name="unit-testing-trigger-functions"></a>Funções de Acionador de teste de unidade

Nesta secção, o teste da unidade irá validar a lógica da função de Acionador HTTP seguinte para iniciar nova orchestrations.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

A tarefa de teste da unidade será para verificar o valor da `Retry-After` cabeçalho fornecido no payload de resposta. Para o teste da unidade será mock algumas das [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html) métodos para garantir um comportamento previsível. 

Em primeiro lugar, é necessário um mock da classe base [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). O mock pode ser uma nova classe que implementa [DurableOrchestrationClientBase](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClientBase.html). No entanto, utilizar uma estrutura mocking como [moq](https://github.com/moq/moq4) simplifica o processo:    

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Em seguida, `StartNewAsync` método é mocked para devolver um ID de instância bem conhecidos.

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

Seguinte `CreateCheckStatusResponse` é mocked para sempre devolver uma resposta de HTTP 200 vazia.

```csharp
    // Mock CreateCheckStatusResponse method
    durableOrchestrationClientBaseMock
        .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId))
        .Returns(new HttpResponseMessage
        {
            StatusCode = HttpStatusCode.OK,
            Content = new StringContent(string.Empty),
        });
```

`TraceWriter` Também é mocked:

```csharp
    // Mock TraceWriter
    var traceWriterMock = new Mock<TraceWriter>(TraceLevel.Info);

```  

Agora a `Run` método é chamado a partir de teste da unidade:

```csharp
    // Call Orchestration trigger function
    var result = await HttpStart.Run(
        new HttpRequestMessage()
        {
            Content = new StringContent("{}", Encoding.UTF8, "application/json"),
            RequestUri = new Uri("http://localhost:7071/orchestrators/E1_HelloSequence"),
        },
        durableOrchestrationClientBaseMock.Object, 
        functionName,
        traceWriterMock.Object);
 ``` 

 O último passo consiste em comparar o resultado com o valor esperado:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Depois de combinar todos os passos, o teste da unidade terá o seguinte código: 

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Unidade testar as funções do orchestrator

As funções do Orchestrator são ainda mais interessantes para a unidade, uma vez que têm, normalmente, muito mais lógica de negócio de teste.

Nesta secção a unidade testes irão validar o resultado a `E1_HelloSequence` função do Orchestrator:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

O código de teste da unidade será comece por criar um mock:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Em seguida, irão ser mocked as chamadas de método da atividade:

```csharp
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

Em seguida o teste da unidade irá chamar `HelloSequence.Run` método:

```csharp
    var result = await HelloSequence.Run(durableOrchestrationContextMock.Object);
```

E, finalmente, o resultado será validado:

```csharp
    Assert.Equal(3, result.Count);
    Assert.Equal("Hello Tokyo!", result[0]);
    Assert.Equal("Hello Seattle!", result[1]);
    Assert.Equal("Hello London!", result[2]);
```

Depois de combinar todos os passos, o teste da unidade terá o seguinte código:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Funções de atividade de teste de unidade

Funções de atividade podem ser unidade testada da mesma forma como as funções não durável. Funções de atividade não tem uma classe base para mocking. Por isso, os testes de unidade utilizam diretamente os tipos de parâmetro.

Nesta secção, o teste da unidade irá validar o comportamento do `E1_SayHello` função da atividade:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

E o teste da unidade irá verificar o formato de saída:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre xUnit](http://xunit.github.io/docs/getting-started-dotnet-core)

> [Saiba mais sobre moq](https://github.com/Moq/moq4/wiki/Quickstart)