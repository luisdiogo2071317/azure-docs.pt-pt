---
title: Descrição geral dos tipos de função e funcionalidades para funções duráveis - Azure
description: Saiba os tipos de funções que permitem a comunicação de função para a função como parte de uma orquestração de função durável.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 2885ce740fab58e675c529dfab8d0dadeed2904c
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301626"
---
# <a name="overview-of-function-types-and-features-for-durable-functions-azure-functions"></a>Descrição geral dos tipos de função e funcionalidades para funções duráveis (funções do Azure)

Funções duráveis proporciona uma orquestração com monitoração de estado de execução de função. Uma função durável é uma solução composta por diferentes funções do Azure. Cada uma dessas funções pode desempenhar funções diferentes, como parte de uma orquestração. O documento seguinte fornece uma descrição geral dos tipos de funções envolvidas numa orquestração de função durável. Ele também inclui alguns padrões comuns na conexão funções em conjunto.  Para começar agora mesmo, criar a primeira função durável na [ C# ](durable-functions-create-first-csharp.md) ou [JavaScript](quickstart-js-vscode.md).

![Tipos de funções duráveis][1]  

## <a name="types-of-functions"></a>Tipos de funções

### <a name="activity-functions"></a>Funções de atividade

Funções de atividade são a unidade básica de trabalho numa orquestração durável.  Funções de atividade são as funções e tarefas que está a ser orquestradas no processo.  Por exemplo, pode criar uma função durável para processar um pedido - Verifique o inventário, o cliente de cobrança e criar uma remessa.  Cada uma dessas tarefas seria uma função de atividade.  Funções de atividade não tem quaisquer restrições no tipo de trabalho, que pode fazer nos mesmos.  Eles podem ser escritos em qualquer [suportado pela funções duráveis](durable-functions-overview.md#language-support). A estrutura de tarefa durável garante que cada função de chamada de atividade será executada pelo menos uma vez durante uma orquestração.

Uma função de atividade tem de ser acionada por um [acionador de atividade](durable-functions-bindings.md#activity-triggers).  Funções de .NET irão receber uma [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) como um parâmetro. Também é possível vincular o acionador para qualquer outro objeto para passar nas entradas para a função. No JavaScript, a entrada pode ser acessada por meio da `<activity trigger binding name>` propriedade no [ `context.bindings` objeto](../functions-reference-node.md#bindings).

A função de atividade também pode retornar valores de volta para o orchestrator.  Se enviar ou retornar vários valores de uma função de atividade, pode [tirar partido das cadeias de identificação ou matrizes](durable-functions-bindings.md#passing-multiple-parameters).  Funções de atividade apenas podem ser acionadas a partir de uma instância de orquestração.  Enquanto parte do código pode ser compartilhado entre uma função de atividade e a outra função (como uma função acionada por HTTP), cada função só pode ter um acionador.

Obter mais informações e exemplos podem ser encontrados no [artigo de associação de funções duráveis](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Funções do Orchestrator

As funções do Orchestrator são o coração de uma função durável.  As funções do Orchestrator descrevem a forma e a ordem ações são executadas.  As funções do Orchestrator descrevem a orquestração no código (C# ou JavaScript) como mostra o [padrões de funções duráveis e conceitos técnicos](durable-functions-concepts.md).  Uma orquestração pode ter vários tipos de ações, como [funções de atividade](#activity-functions), [orquestrações secundárias](#sub-orchestrations), [aguardar por eventos externos](#external-events), e [ temporizadores](#durable-timers).  

Uma função de orquestrador tem de ser acionada por um [acionador de orquestração](durable-functions-bindings.md#orchestration-triggers).

Um orquestrador é iniciado por um [cliente do orchestrator](#client-functions) qual podiam em si ser disparadas de qualquer fonte (HTTP, filas, fluxos de eventos).  Cada instância de uma orquestração tem um identificador de instância, que pode ser gerado automaticamente (recomendado) ou gerados pelo utilizador.  Este identificador pode ser utilizado para [gerir instâncias](durable-functions-instance-management.md) da orquestração.

Obter mais informações e exemplos podem ser encontrados no [artigo de associação de funções duráveis](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>Funções do cliente

As funções do cliente são as funções acionadas que irão criar novas instâncias de uma orquestração.  Eles são o ponto de entrada para a criação de uma instância de uma orquestração durável.  As funções do cliente podem ser acionadas por qualquer acionador (HTTP, filas, os fluxos de eventos, etc.) e escritas em qualquer linguagem suportada pela aplicação.  Para além do acionador, as funções do cliente tem um [cliente de orquestração](durable-functions-bindings.md#orchestration-client) enlace que permite-lhes criar e gerir orquestrações duráveis.  O exemplo mais básico de uma função de cliente é uma função acionada por HTTP, que inicia uma função de orquestrador e retorna uma resposta da verificação de estado como [mostrado neste exemplo seguinte](durable-functions-http-api.md#http-api-url-discovery).

Obter mais informações e exemplos podem ser encontrados no [artigo de associação de funções duráveis](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Recursos e padrões

### <a name="sub-orchestrations"></a>Sub-orquestrações

Para além de chamar funções de atividade, as funções do orchestrator podem chamar outras funções do orchestrator. Por exemplo, pode criar uma orquestração maior fora de uma biblioteca de funções do orchestrator. Em alternativa, pode executar várias instâncias de uma função de orquestrador em paralelo.

Obter mais informações e exemplos podem ser encontrados no [artigo de orquestração secundárias](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Temporizadores duráveis

[Funções duráveis](durable-functions-overview.md) fornece *temporizadores duráveis* para utilização nas funções do orchestrator para implementar atrasos ou configurar tempos limite de ações de async. Temporizadores duráveis devem ser utilizados nas funções do orchestrator, em vez de `Thread.Sleep` e `Task.Delay` (c#), ou `setTimeout()` e `setInterval()` (JavaScript).

Obter mais informações e exemplos de temporizadores duráveis podem ser encontrados no [artigo de temporizadores duráveis](durable-functions-timers.md)

### <a name="external-events"></a>Eventos externos

As funções do Orchestrator podem aguardar por eventos externos atualizar uma instância de orquestração. Esta funcionalidade de funções durável, muitas vezes, é útil para lidar com interação humana ou outros retornos de chamada externos.

Obter mais informações e exemplos podem ser encontrados no [artigo de eventos externos](durable-functions-external-events.md).

### <a name="error-handling"></a>Processamento de erros

Duráveis orquestrações de função são implementadas no código e podem usar os recursos de tratamento de erros de linguagem de programação.  Isso significa que os padrões como "try/catch" que funcionarão na sua orquestração.  Funções duráveis também vêm com algumas políticas de repetição incorporado.  Uma ação pode atrasar e repita a atividades automaticamente em exceções.  As repetições permitem-lhe lidar com exceções transitórias sem terem de abandonar a orquestração.

Obter mais informações e exemplos podem ser encontrados no [artigo de tratamento de erros](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Comunicação de aplicação de várias funções

Embora uma orquestração durável geralmente reside num contexto de uma aplicação de função única, existem padrões que lhe permite coordenar orquestrações em muitas aplicações de funções.  Mesmo que a comunicação entre aplicações poderá estar a ocorrer através de HTTP, usando a estrutura de durável para cada atividade significa que ainda pode manter um processo durável entre duas aplicações.

Exemplos de uma orquestração de aplicação de várias funções no C# e o JavaScript são apresentadas abaixo.  Uma atividade iniciará a orquestração externa. Outra atividade, em seguida, irá obter e devolver o estado.  O orchestrator espera que o estado seja concluída antes de continuar.

#### <a name="c"></a>C#

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now go do more work...
}

[FunctionName("StartRemoteOrchestration")]
public static async Task<string> StartRemoteOrchestration([ActivityTrigger] string orchestratorName)
{
    using (var response = await HttpClient.PostAsync(
        $"https://appB.azurewebsites.net/orchestrations/{orchestratorName}",
        new StringContent("")))
    {
        string statusUrl = await response.Content.ReadAsAsync<string>();
        return statusUrl;
    }
}

[FunctionName("CheckIsComplete")]
public static async Task<bool> CheckIsComplete([ActivityTrigger] string statusUrl)
{
    using (var response = await HttpClient.GetAsync(statusUrl))
    {
        // 200 = Complete, 202 = Running
        return response.StatusCode == HttpStatusCode.OK;
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (funciona apenas 2.x)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    // Do some work...

    // Call a remote orchestration
    const statusUrl = yield context.df.callActivity("StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true) {
        const isComplete = yield context.df.callActivity("CheckIsComplete", statusUrl);
        if (isComplete) {
            break;
        }

        const waitTime = moment(context.df.currentUtcDateTime).add(1, "m").toDate();
        yield context.df.createTimer(waitTime);
    }

    // B is done. Now go do more work...
});
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, orchestratorName) {
    const options = {
        method: "POST",
        uri: `https://appB.azurewebsites.net/orchestrations/${orchestratorName}`,
        body: ""
    };

    const statusUrl = await request(options);
    return statusUrl;
};
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, statusUrl) {
    const options = {
        method: "GET",
        uri: statusUrl,
        resolveWithFullResponse: true,
    };

    const response = await request(options);
    // 200 = Complete, 202 = Running
    return response.statusCode === 200;
};
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Continue a ler a documentação das funções duráveis](durable-functions-bindings.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
