---
title: Orquestrações secundária para funções duráveis - Azure
description: Como chamar orquestrações de orquestrações na extensão de funções duráveis para as funções do Azure.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 59e8eb41b7e9fe3d57196f6844d1a768c3ef598b
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44094444"
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Orquestrações secundárias nas funções duráveis (funções do Azure)

Para além de chamar funções de atividade, as funções do orchestrator podem chamar outras funções do orchestrator. Por exemplo, pode criar uma orquestração maior fora de uma biblioteca de funções do orchestrator. Em alternativa, pode executar várias instâncias de uma função de orquestrador em paralelo.

Uma função de orquestrador pode chamar a função de orquestrador de outro ao chamar o [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) ou o [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) método. O [tratamento de erro & compensação](durable-functions-error-handling.md#automatic-retry-on-failure) artigo tem mais informações sobre a repetição automática.

As funções do orchestrator secundárias se comportam como funções de atividade do ponto de vista do chamador. Eles podem retornar um valor, lance uma exceção e podem ser colocadas em espera pela função de orquestrador principal.

> [!NOTE]
> O `CallSubOrchestratorAsync` e `CallSubOrchestratorWithRetryAsync` métodos ainda não estão disponíveis em JavaScript.

## <a name="example"></a>Exemplo

O exemplo a seguir ilustra um cenário de IoT ("Internet das coisas") em que existem vários dispositivos que têm de ser aprovisionado. Há uma orquestração específica que precisa para ocorrer para cada um dos dispositivos, que podem ser semelhante ao seguinte:

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string deviceId = ctx.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await ctx.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await ctx.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await ctx.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

Esta função de orquestrador pode ser usada como-é para aprovisionamento de dispositivos pontuais ou ele pode fazer parte de uma orquestração maior. No último caso, a função de orquestrador principal pode agendar a instâncias de `DeviceProvisioningOrchestration` usando o `CallSubOrchestratorAsync` API.

Eis um exemplo que mostra como executar várias funções do orchestrator em paralelo.

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string[] deviceIds = await ctx.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = ctx.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba quais são os hubs de tarefas e como configurá-las](durable-functions-task-hubs.md)
