---
title: Descrição geral de funções duráveis - Azure
description: Introdução para a extensão de funções duráveis para as funções do Azure.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: azfuncdf
ms.openlocfilehash: 1d6160f8c66fd749942be581cb2992977da82911
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44377994"
---
# <a name="durable-functions-overview"></a>Descrição geral de funções durável

*Funções duráveis* é uma extensão da [as funções do Azure](functions-overview.md) e [WebJobs do Azure](../app-service/web-sites-create-web-jobs.md) que permite que escreva funções com monitoração de estado num ambiente sem servidor. A extensão gere o estado, os pontos de verificação e reinicializações por si.

A extensão permite definir fluxos de trabalho com monitorização de estado num novo tipo de função chamada uma [ *função de orquestrador*](durable-functions-types-features-overview.md#orchestrator-functions). Aqui estão algumas das vantagens das funções do orchestrator:

* Elas definem os fluxos de trabalho no código. Não existem esquemas JSON ou designers são necessários.
* Podem chamar outras funções de modo síncrono e assíncrono. Saída de funções chamadas pode ser guardada para variáveis locais.
* Eles automaticamente ponto de verificação o progresso, sempre que a função awaits. Estado local nunca é perdido, se o processo é reciclado ou a VM é reiniciada.

> [!NOTE]
> Funções duráveis é uma extensão avançada para funções do Azure que não é adequado para todos os aplicativos. O restante deste artigo parte do princípio de que tem uma forte familiaridade com [as funções do Azure](functions-overview.md) conceitos e os desafios envolvidos no desenvolvimento de aplicações sem servidor.

O principal motivo para as funções durável é simplificar problemas de coordenação complexa, com monitoração de estado em aplicações sem servidor. As secções seguintes descrevem alguns padrões de aplicativo típico que podem se beneficiar de funções duráveis.

## <a name="pattern-1-function-chaining"></a>Padrão #1: Encadeamento de função

*Encadeamento de função* refere-se para o padrão de uma seqüência de funções em execução numa determinada ordem. Muitas vezes, a saída de uma função tem de ser aplicadas para a entrada de outra função.

![Diagrama de encadeamento de função](media/durable-functions-overview/function-chaining.png)

Funções duráveis permite-lhe implementar este padrão de forma concisa no código.

#### <a name="c-script"></a>Script C#

```cs
public static async Task<object> Run(DurableOrchestrationContext ctx)
{
    try
    {
        var x = await ctx.CallActivityAsync<object>("F1");
        var y = await ctx.CallActivityAsync<object>("F2", x);
        var z = await ctx.CallActivityAsync<object>("F3", y);
        return  await ctx.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // error handling/compensation goes here
    }
}
```
> [!NOTE]
> Existem diferenças sutis ao escrever uma função durável pré-compilados no c# vs, o exemplo de script c# mostrado anteriormente. Uma função c# pré-compiladas exigiria duráveis parâmetros a ser decorada com os respetivos atributos. Um exemplo é `[OrchestrationTrigger]` atributo para `DurableOrchestrationContext` parâmetro. Se os parâmetros não são decorados corretamente, o tempo de execução não seria capaz de inserir as variáveis para a função e forneceria o erro. Visite [exemplo](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples) para obter mais exemplos.

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas para v2 de funções)

```js
const df = require("durable-functions");

module.exports = df(function*(ctx) {
    const x = yield ctx.df.callActivityAsync("F1");
    const y = yield ctx.df.callActivityAsync("F2", x);
    const z = yield ctx.df.callActivityAsync("F3", y);
    return yield ctx.df.callActivityAsync("F4", z);
});
```

Os valores "F1", "F2", "F3" e "F4" são os nomes de outras funções na aplicação de função. Fluxo de controle é implementado com imperativo normal construções de codificação. Ou seja, o código é executado de cima para baixo e pode envolver existente semântica de fluxo de controle idioma, como condicionais e loops.  Lógica de manipulação de erros podem ser incluídos nos blocos try/catch/finally.

O `ctx` parâmetro ([DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)) fornece métodos para invocar a outras funções por nome, passando parâmetros e retornar a saída da função. Sempre que o código chama `await`, a estrutura de funções duráveis *pontos de verificação* o progresso da instância de função atual. Se o processo ou VM recicla midway através da execução, a instância de função sai do modo de anterior `await` chamar. Obter mais informações sobre este comportamento de reinício mais tarde.

## <a name="pattern-2-fan-outfan-in"></a>Padrão #2: Fan-out/fan-in

*Fan-out/fan-in* refere-se para o padrão de várias funções em execução em paralelo e então aguardar para que tudo para concluir.  Muitas vezes algum trabalho de agregação é feito nos resultados devolvidos das funções.

![Diagrama de fan-out/fan-in](media/durable-functions-overview/fan-out-fan-in.png)

Com as funções normais, pode ser feito fanning fazendo com que a função de enviar várias mensagens para uma fila. No entanto, o fanning em é muito mais desafiador. Teria de escrever código para controlar quando as funções de acionada por fila terminam e armazenam as saídas de função. A extensão de funções duráveis lida com esse padrão com o código relativamente simples.

#### <a name="c-script"></a>Script C#

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    var parallelTasks = new List<Task<int>>();
 
    // get a list of N work items to process in parallel
    object[] workBatch = await ctx.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = ctx.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }
 
    await Task.WhenAll(parallelTasks);
 
    // aggregate all N outputs and send result to F3
    int sum = parallelTasks.Sum(t => t.Result);
    await ctx.CallActivityAsync("F3", sum);
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas para v2 de funções)

```js
const df = require("durable-functions");

module.exports = df(function*(ctx) {
    const parallelTasks = [];

    // get a list of N work items to process in parallel
    const workBatch = yield ctx.df.callActivityAsync("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(ctx.df.callActivityAsync("F2", workBatch[i]));
    }

    yield ctx.df.task.all(parallelTasks);

    // aggregate all N outputs and send result to F3
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield ctx.df.callActivityAsync("F3", sum);
});
```

O trabalho de fan-Out é distribuído para várias instâncias de função `F2`, e o trabalho é controlado com uma lista dinâmica de tarefas. O .NET `Task.WhenAll` API é chamada para aguardar que todas as funções de chamadas para concluir. Em seguida, o `F2`saídas de função são agregados da lista de tarefas dinâmico e passado para o `F3` função.

O ponto de verificação automática que ocorre à `await` chamar no `Task.WhenAll` garante que qualquer falha ou a reinicialização enquanto não requer um reinício de qualquer já concluiu a tarefas.

## <a name="pattern-3-async-http-apis"></a>Padrão #3: APIs de HTTP de Async

O padrão de terceiro é tudo sobre o problema de coordenar o estado das operações de execução longa com clientes externos. Uma forma comum de implementar este padrão é fazendo com que a ação de execução longa acionada por uma chamada HTTP e, em seguida, redirecionar o cliente para um ponto de extremidade de estado que eles podem sondar para saber quando a operação for concluída.

![Diagrama da API de HTTP](media/durable-functions-overview/async-http-api.png)

Funções duráveis fornece APIs internas que simplificam o código de escrita para interagir com execuções de função de execução longa. O [amostras](durable-functions-install.md) mostrar um comando REST simples que pode ser utilizado para iniciar novas instâncias de função de orquestrador. Assim que uma instância é iniciada, a extensão expõe webhook APIs HTTP que consulta o estado de função do orchestrator. O exemplo seguinte mostra os comandos REST para iniciar um orquestrador e para consultar o seu estado. Para maior clareza, alguns detalhes são omitidos do exemplo.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2017-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2017-03-16T21:20:57Z", ...}
```

Uma vez que o estado for gerido pelo tempo de execução de funções duráveis, não tem de implementar seu próprio mecanismo de controlo de estado.

Mesmo que a extensão de funções duráveis tem webhooks incorporados para o gerenciamento de orquestrações de execução longa, pode implementar este padrão usando seus próprios acionadores de função (por exemplo, HTTP, fila ou Hub de eventos) e o `orchestrationClient` enlace. Por exemplo, pode utilizar uma mensagem de fila para acionar a terminação.  Ou poderia usar um acionador HTTP protegido por uma política de autenticação do Azure Active Directory em vez dos webhooks incorporadas que utilizam uma chave gerada para a autenticação. 

```cs
// HTTP-triggered function to start a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    TraceWriter log)
{
    // Function name comes from the request URL.
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);
    
    log.Info($"Started orchestration with ID = '{instanceId}'.");
    
    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

O [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parâmetro é um valor entre o `orchestrationClient` de saída de ligação, que faz parte da extensão de funções duráveis. Ele fornece métodos para eventos de partida, enviando, terminar e a consulta para instâncias de função de orquestrador de novo ou existente. No exemplo anterior, uma HTTP acionada-função aceita um `functionName` valor do URL de entrada e os passes esse valor à [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). Esta API de associação, em seguida, devolve uma resposta que contém um `Location` cabeçalho e informações adicionais sobre a instância que pode ser utilizada mais tarde para ver o estado da instância introdução a cópia de segurança ou terminá-lo.

## <a name="pattern-4-monitoring"></a>Padrão de #4: monitorização

O padrão de monitor se refere a um flexível *recorrentes* processo num fluxo de trabalho - por exemplo, a consulta até que forem cumpridas determinadas condições. Um acionador de temporizador regular os pode resolver um cenário simple, como uma tarefa de limpeza periódica, mas o intervalo é estático e durações de instância de gestão torna-se complexo. Funções duráveis permite que os intervalos de periodicidade flexível, gerenciamento de duração de tarefa e a capacidade de criar o monitor de vários processos de uma única orquestração.

Um exemplo seria possível invertendo o cenário de API de HTTP de async anterior. Em vez de expor um ponto final para um cliente externo monitorizar uma operação de longa execução, o monitor de execução longa consome um ponto final externo, a aguardar que algumas alterações de estado.

![Diagrama de monitor](media/durable-functions-overview/monitor.png)

Usando funções duráveis, podem ser criados vários monitores que cumprem os pontos de extremidade arbitrários em algumas linhas de código. Os monitores podem terminar a execução quando alguma condição for cumprida, ou terminar com o [DurableOrchestrationClient](durable-functions-instance-management.md), e o intervalo de espera pode ser alterado com base em alguma condição (ou seja, término exponencial.) O seguinte código implementa um monitor básico.

#### <a name="c-script"></a>Script C#

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    int jobId = ctx.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();
    
    while (ctx.CurrentUtcDateTime < expiryTime) 
    {
        var jobStatus = await ctx.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform action when condition met
            await ctx.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        var nextCheck = ctx.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await ctx.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform further work here, or let the orchestration end
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas para v2 de funções)

```js
const df = require("durable-functions");
const df = require("moment");

module.exports = df(function*(ctx) {
    const jobId = ctx.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(ctx.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield ctx.df.callActivityAsync("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform action when condition met
            yield ctx.df.callActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        const nextCheck = moment.utc(ctx.df.currentUtcDateTime).add(pollingInterval, 's');
        yield ctx.df.createTimer(nextCheck.toDate());
    }

    // Perform further work here, or let the orchestration end
});
```

Quando é recebido um pedido, é criada uma nova instância de orquestração para esse ID de tarefa. A instância de consulta um estado até que a condição é cumprida e o loop é encerrado. Um temporizador durável é utilizado para controlar o intervalo de consulta. Mais trabalho, em seguida, pode ser executado ou a orquestração pode terminar. Quando o `ctx.CurrentUtcDateTime` excede o `expiryTime`, as extremidades do monitor.

## <a name="pattern-5-human-interaction"></a>Padrão #5: Interação humana

Muitos processos envolvem algum tipo de interação humana. O complicadas que envolvem seres humanos num processo automatizado é que as pessoas não são sempre como sendo de elevada disponível e é reativo como serviços em nuvem. Processos automatizados devem permitir isso, e, muitas vezes, fazem isso usando uma lógica de tempos limite e de compensação.

Um exemplo de um processo comercial que envolve a interação humana é um processo de aprovação. Por exemplo, aprovação de um gestor pode ser necessária para um relatório de despesas que exceda um determinado período. Se o gestor não aprova dentro de 72 horas (talvez se tornaram férias), um processo de escalonamento entra em ação para obter a aprovação de outra pessoa (talvez do gestor).

![Diagrama de interação humana](media/durable-functions-overview/approval.png)

Este padrão pode ser implementado usando uma função de orquestrador. O orchestrator usaria um [temporizador durável](durable-functions-timers.md) para solicitar a aprovação e escalar em caso de tempo limite. Ele deve esperar para um [evento externo](durable-functions-external-events.md), qual seria a notificação gerada por alguma interação humana.

#### <a name="c-script"></a>Script C#

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    await ctx.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = ctx.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = ctx.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = ctx.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await ctx.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await ctx.CallActivityAsync("Escalate");
        }
    }
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas para v2 de funções)

```js
const df = require("durable-functions");
const df = require('moment');

module.exports = df(function*(ctx) {
    yield ctx.df.callActivityAsync("RequestApproval");

    const dueTime = moment.utc(ctx.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = ctx.df.createTimer(dueTime.toDate());

    const approvalEvent = ctx.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield ctx.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield ctx.df.callActivityAsync("ProcessApproval", approvalEvent.result);
    } else {
        yield ctx.df.callActivityAsync("Escalate");
    }
});
```

O temporizador durável é criado chamando `ctx.CreateTimer`. A notificação é recebida pelo `ctx.WaitForExternalEvent`. E `Task.WhenAny` denomina-se para decidir se deve escalar (tempo limite acontece pela primeira vez) ou processar a aprovação (aprovação é recebida antes do tempo limite).

Um cliente externo pode enviar a notificação de evento para uma função de orquestrador de espera através de um a [APIs internas de HTTP](durable-functions-http-api.md#raise-event) ou utilizando [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) API do outra função:

```csharp
public static async Task Run(string instanceId, DurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

## <a name="the-technology"></a>A tecnologia

Nos bastidores, a extensão de funções duráveis baseia-se do [durável de tarefa de Framework](https://github.com/Azure/durabletask), uma biblioteca de código-fonte aberto no GitHub para a criação de orquestrações de tarefa durável. Muito, como a forma como as funções do Azure é a evolução sem servidor do Azure WebJobs, funções duráveis é a evolução sem servidor da estrutura de tarefa durável. A estrutura de tarefa durável é utilizada intensamente dentro da Microsoft e de fora também para automatizar processos de missão crítica. É uma opção natural para o ambiente de funções do Azure sem servidor.

### <a name="event-sourcing-checkpointing-and-replay"></a>Origem do evento, pontos de verificação e repetição

As funções do Orchestrator com fiabilidade mantêm seu estado de execução usando um padrão de design, conhecido como [origem do evento](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing). Em vez de armazenar diretamente a *atual* estado de uma orquestração, a extensão durável utiliza um arquivo só de acréscimo para registar o *completa de série de ações* tomada pela orquestração de função. Isso tem muitas vantagens adicionais, incluindo a melhorar o desempenho, escalabilidade e capacidade de resposta em comparação com o estado do tempo de execução completa de "despejo". Os outros benefícios estão fornecendo consistência eventual para os dados transacionais e manter registos de auditoria completos e histórico. Os registos de auditoria próprios permitem ações de compensação fiáveis.

A utilização de origem do evento por esta extensão é transparente. Nos bastidores, o `await` operador numa função de orquestrador produz o controle do orchestrator thread para o dispatcher do Framework de tarefa durável. O dispatcher, em seguida, consolida todas as novas ações que a função de orquestrador agendada (por exemplo, chamar uma ou mais funções de subordinado ou agendamento de um temporizador durável) para o armazenamento. Esta ação de consolidação transparente acrescenta para o *histórico de execução* da instância de orquestração. O histórico é armazenado numa tabela de armazenamento. A ação de consolidação, em seguida, adiciona uma mensagem numa fila para agendar o trabalho real. Neste momento, a função de orquestrador pode ser baixada da memória. A faturação para o mesmo deixa de se estiver a utilizar o plano de consumo de funções do Azure.  Quando existe mais de trabalho, a função é reiniciada e seu estado é reconstruído.

Depois de uma função de orquestração recebe mais de trabalho (por exemplo, é recebida uma mensagem de resposta ou expira um temporizador durável), o orchestrator reativado novamente e novamente executa a função completa desde o início para reconstruir o estado local. Se durante esta repetição, o código tenta chamar uma função (ou fazer qualquer outra async trabalho), a estrutura de tarefa durável atua com o *histórico de execução* da orquestração atual. Se achar que o [função de atividade](durable-functions-types-features-overview.md#activity-functions) já foi executado e gerou um resultado, ele replays resultado essa função e o código do orchestrator continua a ser executado. Isso continua até que o código de função obtém a um ponto onde seja concluído ou tem trabalho agendado de async novo a acontecer.

### <a name="orchestrator-code-constraints"></a>Restrições de código do Orchestrator

O comportamento de repetição cria restrições no tipo de código que pode ser escrito numa função de orquestrador. Por exemplo, código do orchestrator tem de ser determinístico, pois vai ser repetida várias vezes e deve produzir o mesmo resultado cada vez. A lista completa de restrições pode ser encontrada no [restrições de código do Orchestrator](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints) secção a **pontos de verificação e reinício** artigo.

## <a name="language-support"></a>Suporte de idiomas

Atualmente em C# (funções v1 e v2), F # e JavaScript (apenas para v2 de funções) são os idiomas com suporte apenas para funções duráveis. Isto inclui as funções do orchestrator e funções de atividade. No futuro, será adicionado suporte para todos os idiomas que suporta as funções do Azure. Veja as funções do Azure [lista de problemas do repositório de GitHub](https://github.com/Azure/azure-functions-durable-extension/issues) para ver o estado mais recente da nossa linguagem adicional de suporte profissional.

## <a name="monitoring-and-diagnostics"></a>Monitorização e diagnóstico

A extensão de funções duráveis emite automaticamente dados de controlo estruturados para [Application Insights](functions-monitoring.md) quando a aplicação de função está configurada com uma chave de instrumentação do Application Insights. Estes dados de controlo podem ser utilizados para monitorizar o comportamento e o progresso das suas orquestrações.

Eis um exemplo da aparência funções duráveis, acompanhar eventos no Application Insights portal com [Application Insights Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics):

![Resultados de consulta do App Insights](media/durable-functions-overview/app-insights-1.png)

Há muitos dados estruturados útil incluídos no `customDimensions` campo em cada entrada de log. Eis um exemplo de uma entrada que tais totalmente expandida.

![campo de customDimensions na consulta do App Insights](media/durable-functions-overview/app-insights-2.png)

Devido ao comportamento de repetição do dispatcher durável Framework de tarefas, pode esperar ver as entradas de registo com redundância de ações repetidas. Isso pode ser útil para entender o comportamento de repetição do mecanismo central. O [diagnóstico](durable-functions-diagnostics.md) artigo mostra-lhe consultas de exemplo que filtram os registos de repetição para que possa ver apenas os registos de "em tempo real".

## <a name="storage-and-scalability"></a>Armazenamento e escalabilidade

A extensão de funções duráveis utiliza blobs, tabelas e filas de armazenamento do Azure para manter a execução histórico acionador e estado de execução de função. A conta de armazenamento predefinida para a aplicação de função pode ser utilizada ou pode configurar uma conta de armazenamento separada. Pode desejar uma conta separada devido a limites de débito de armazenamento. O código de orchestrator escrito por não precisa de (e não deve) interagir com as entidades nestas contas de armazenamento. As entidades são geridas diretamente pela estrutura de tarefa durável como um detalhe de implementação.

As funções do Orchestrator agendar as funções de atividade e recebem suas respostas através de mensagens da fila interna. Quando uma aplicação de função é executada no plano de consumo de funções do Azure, estas filas são monitorizadas pelos [controlador de dimensionamento de funções do Azure](functions-scale.md#how-the-consumption-plan-works) e novo cálculo instâncias são adicionadas conforme necessário. Ao aumentar horizontalmente para várias VMs, uma função de orquestrador pode ser executadas numa VM durante a execução de funções de atividade, que ele chama em várias VMs diferentes. Pode encontrar mais detalhes sobre o comportamento de dimensionamento de funções durável nas [desempenho e dimensionamento](durable-functions-perf-and-scale.md).

Armazenamento de tabelas é utilizado para armazenar o histórico de execução para contas do orchestrator. Sempre que uma instância rehydrates numa VM específica, obtém o histórico de execução do armazenamento de tabela para que ele pode recriar seu estado local. Uma das coisas convenientes relacionada com o histórico disponível no armazenamento de tabelas é que pode dar uma olhada e ver o histórico de suas orquestrações com ferramentas, como [Explorador de armazenamento do Microsoft Azure](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

Blobs de armazenamento são utilizados principalmente como um mecanismo de locação para coordenar o Escalamento horizontal de instâncias de orquestração de várias VMs. Eles também são utilizados para armazenar os dados para mensagens grandes que não não possível armazenar diretamente em tabelas ou filas.

![Captura de ecrã de Explorador de armazenamento do Azure](media/durable-functions-overview/storage-explorer.png)

> [!WARNING]
> Embora seja fácil e conveniente para ver o histórico de execuções no armazenamento de tabelas, evite colocar qualquer dependência nesta tabela. Eles podem mudar à medida que a extensão de funções duráveis evolui.

## <a name="known-issues-and-faq"></a>Problemas conhecidos e FAQ

Devem haver um controle em todos os problemas conhecidos a [problemas do GitHub](https://github.com/Azure/azure-functions-durable-extension/issues) lista. Se se deparar com um problema e não é possível localizar o problema no GitHub, abra um novo problema e incluir uma descrição detalhada do problema.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Continue a ler a documentação das funções duráveis](durable-functions-types-features-overview.md)

> [!div class="nextstepaction"]
> [Instalar a extensão Durable Functions e os exemplos](durable-functions-install.md)

