---
title: Singletons para funções duráveis - Azure
description: Como utilizar singletons na extensão Functons durável para as funções do Azure.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 58e5b06d613ee3e3311b58af64abd2411c637449
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52642614"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Orquestradores de singleton nas funções durável (funções do Azure)

Para tarefas em segundo plano ou orquestrações de estilo de ator, muitas vezes, precisam garantir que apenas uma instância de um orquestrador específico é executado cada vez. Isso pode ser feito [funções duráveis](durable-functions-overview.md) atribuindo específicas de uma ID de instância para um orquestrador durante a criação.

## <a name="singleton-example"></a>Exemplo de singleton

O exemplo do c# seguinte mostra uma função de Acionador de HTTP que cria uma orquestração de tarefa em segundo plano singleton. O código garante que apenas uma instância não existe para um ID de instância especificada.

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

Por predefinição, a instância IDs são aleatoriamente gerado GUIDs. Mas, neste caso, o ID de instância é passado encaminhar os dados da URL. O código chama [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) para verificar se uma instância com o ID especificado já está em execução. Se não for, é criada uma instância com esse ID.

> [!NOTE]
> Neste exemplo, existe uma condição de corrida potencial. Se duas instâncias da **HttpStartSingle** executadas simultaneamente, o resultado foi possível dois diferentes criar instâncias de singleton, que substitui o outro. Dependendo dos requisitos, isso pode ter efeitos de colaterais indesejáveis. Por esse motivo, é importante certificar-se de que não existem dois pedidos podem executar esta função de Acionador em simultâneo.

Os detalhes da implementação da função de orquestrador, na verdade, não importa. Pode ser uma função de orquestrador regular que é iniciada e concluída, ou pode ser que seja executado Eternamente (ou seja, uma [orquestração externas](durable-functions-eternal-orchestrations.md)). O ponto importante é que há apenas nunca uma instância em execução ao mesmo tempo.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como chamar orquestrações secundárias](durable-functions-sub-orchestrations.md)
