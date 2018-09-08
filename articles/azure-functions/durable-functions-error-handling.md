---
title: Tratamento de erros nas funções duráveis - Azure
description: Saiba como lidar com erros na extensão de funções duráveis para as funções do Azure.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: azfuncdf
ms.openlocfilehash: 0b19fe7441d3c2c5222095c31d9c3677b8c9cf34
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092722"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Tratamento de erros nas funções durável (funções do Azure)

Duráveis orquestrações de função são implementadas no código e podem utilizar as capacidades de manipulação de erros de linguagem de programação. Com isso em mente, existem não são realmente quaisquer novos conceitos que precisa saber sobre o quando incorporar o tratamento de erros e de compensação em suas orquestrações. No entanto, existem alguns comportamentos que deve estar atento.

## <a name="errors-in-activity-functions"></a>Erros nas funções de atividade

Qualquer exceção que é lançada numa função de atividade é marshalled volta para a função de orquestrador e lançada como uma `FunctionFailedException`. Pode escrever o código de compensação e manipulação de erro que atenda às suas necessidades na função de orquestrador.

Por exemplo, considere a seguinte função de orquestrador que transfira fundos entre contas para outro:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task Run(DurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        { 
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",         
            new
            { 
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",         
            new
            { 
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

Se a chamada para o **CreditAccount** falha de função para a conta de destino, a função de orquestrador compensa isso por é inserida creditando os fundos de volta para a conta de origem.

## <a name="automatic-retry-on-failure"></a>Repetição automática em caso de falha

Quando chama as funções de atividade ou funções de frações de orquestração, pode especificar uma política de repetição automática. O exemplo seguinte tenta chamar uma função até três vezes e aguarda 5 segundos entre cada repetição:

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);
    
    // ...
}
```

O `CallActivityWithRetryAsync` API utiliza um `RetryOptions` parâmetro. Suborchestration chama usando o `CallSubOrchestratorWithRetryAsync` API pode utilizar estas mesmas políticas de repetição.

Existem várias opções para personalizar a política de repetição automática. Entre eles, incluem-se:

* **Número máximo de tentativas**: O número máximo de tentativas de repetição.
* **Primeiro intervalo de repetição**: A quantidade de tempo de espera antes de tentar da primeira repetição.
* **Coeficiente de término**: O coeficiente utilizado para determinar a taxa de aumento do número de término. Predefinição é 1.
* **Intervalo de repetição máximo**: A quantidade máxima de tempo de espera entre tentativas de repetição.
* **Repetição do tempo limite**: A quantidade máxima de tempo para fazer tentativas. O comportamento padrão é repetir indefinidamente.
* **Custom**: pode ser especificado um retorno de chamada definida pelo utilizador que determina se é ou não uma chamada de função deve ser repetida.

## <a name="function-timeouts"></a>Tempos limite de função

Pode querer abandonar uma chamada de função dentro de uma função de orquestrador, se estiver a demorar demasiado tempo a concluir. O modo adequado para fazer isso hoje é criar um [temporizador durável](durable-functions-timers.md) usando `context.CreateTimer` em conjunto com `Task.WhenAny`, como no exemplo a seguir:

```csharp
public static async Task<bool> Run(DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

> [!NOTE]
> Esse mecanismo não encerra, na verdade, a execução de função da atividade em curso. Em vez disso, ele simplesmente permite que a função de orquestrador ignorar o resultado e avançar. Para obter mais informações, consulte a [temporizadores](durable-functions-timers.md#usage-for-timeout) documentação.

## <a name="unhandled-exceptions"></a>Exceções sem tratamento

Se uma função de orquestrador falhar com uma exceção não processada, os detalhes da exceção são registados e a instância é concluída com um `Failed` estado.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como diagnosticar problemas](durable-functions-diagnostics.md)
