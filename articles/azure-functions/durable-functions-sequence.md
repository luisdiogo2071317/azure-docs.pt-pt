---
title: Função encadeamento nas funções durável - Azure
description: Saiba como executar um exemplo de funções durável que executa uma sequência de funções.
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
ms.openlocfilehash: e53b38bf336816ca670fad3ab70a43e5cc8b3437
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33763565"
---
# <a name="function-chaining-in-durable-functions---hello-sequence-sample"></a>Encadeamento nas funções durável - exemplo de sequência de Olá de função

Encadeamento de função refere-se o padrão de executar uma sequência de funções por uma ordem específica. Muitas vezes, o resultado de uma função tem de ser aplicada para a entrada de outra função. Este artigo explica um exemplo que utiliza [funções durável](durable-functions-overview.md) para implementar o encadeamento de função.

## <a name="prerequisites"></a>Pré-requisitos

* [Instalar funções duráveis](durable-functions-install.md).

## <a name="the-functions"></a>As funções

Este artigo explica as seguintes funções na aplicação de exemplo:

* `E1_HelloSequence`: Uma função do orchestrator que chama `E1_SayHello` várias vezes numa sequência. Armazena as saídas do `E1_SayHello` chama e regista os resultados.
* `E1_SayHello`: Uma função de atividade que prepends uma cadeia com "Olá".

As secções seguintes explicam o código que são utilizadas para c# scripting e JavaScript e configuração. O código para o desenvolvimento de Visual Studio é apresentado no final do artigo.

> [!NOTE]
> Duráveis funções está disponível em JavaScript apenas o runtime de funções de v2.

## <a name="e1hellosequence"></a>E1_HelloSequence
### <a name="functionjson-file"></a>ficheiro Function.JSON

Se utilizar o Visual Studio Code ou o portal do Azure para desenvolvimento, eis o conteúdo a *function.json* ficheiro para a função do orchestrator. A maioria das orchestrator *function.json* ficheiros quase exatamente este aspeto.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/function.json)]

O mais importante é o `orchestrationTrigger` tipo de enlace. Todas as funções do orchestrator tem de utilizar este tipo de Acionador.

> [!WARNING]
> Para cumprir a regra "nenhum e/s" das funções do orchestrator, não utilize qualquer entrada ou saída enlaces ao utilizar o `orchestrationTrigger` acionar o enlace.  Se outras de entrada ou saída enlaces são necessários, estes em vez disso, devem ser utilizadas no contexto de `activityTrigger` funções, que são chamadas do orchestrator.

### <a name="c-script-visual-studio-code-and-azure-portal-sample-code"></a>Script do c# (código de exemplo portal Visual Studio Code e o Azure) 

Eis o código de origem:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_HelloSequence/run.csx)]

Todas as funções de orquestração da c# tem de ter um parâmetro do tipo `DurableOrchestrationContext`, qual existe a `Microsoft.Azure.WebJobs.Extensions.DurableTask` assemblagem. Se estiver a utilizar o script do c#, a assemblagem pode ser referenciada através de `#r` notação. Este objeto de contexto permite-lhe chamar outros *atividade* funções e transmita os parâmetros de entrada com o respetivo [CallActivityAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_) método.

As chamadas de código `E1_SayHello` três vezes numa sequência com valores de parâmetro diferentes. O valor de cada chamada de retorno é adicionado ao `outputs` lista, que é devolvida no final da função.

### <a name="javascript"></a>Javascript

Eis o código de origem:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

Todas as funções de orquestração do JavaScript tem de incluir o `durable-functions` módulo. Esta é uma biblioteca de JavaScript traduz ações a função de orquestração em protocolo de execução do durável para os idiomas de out-of-proc. Existem três diferenças significativas entre uma função de orquestração e outras funções de JavaScript:

1. A função é um [função gerador de dimensões.](https://docs.microsoft.com/en-us/scripting/javascript/advanced/iterators-and-generators-javascript)
2. A função é moldada numa chamada para o `durable-functions` módulo (aqui `df`).
3. A função termina chamando `return`, não `context.done`.

O `context` objeto contém uma `df` objeto permite-lhe chamar outros *atividade* funções e transmita os parâmetros de entrada com o respetivo `callActivityAsync` método. As chamadas de código `E1_SayHello` três vezes numa sequência com valores de parâmetro diferentes, utilizando `yield` para indicar a execução deve aguardar nas chamadas de função de atividade assíncronos devem ser devolvidos. O valor de cada chamada de retorno é adicionado ao `outputs` lista, que é devolvida no final da função.

## <a name="e1sayhello"></a>E1_SayHello
### <a name="functionjson-file"></a>ficheiro Function.JSON

O *function.json* ficheiro para a função de atividade `E1_SayHello` é semelhante de `E1_HelloSequence` exceto que utiliza um `activityTrigger` enlace de tipo em vez de um `orchestrationTrigger` tipo de enlace.

[!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

> [!NOTE]
> Qualquer função chamada por uma função de orquestração tem de utilizar o `activityTrigger` enlace.

A implementação de `E1_SayHello` é uma cadeia relativamente trivial formatação a operação.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E1_SayHello/run.csx)]

Esta função tem um parâmetro do tipo [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html), que utiliza para obter a entrada que foi transmitida para o mesmo por chamada da função do orchestrator para [ `CallActivityAsync<T>` ](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallActivityAsync_).

### <a name="javascript"></a>JavaScript

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

Ao contrário de uma função de orquestração do JavaScript, uma função de atividade de JavaScript não precisa de nenhuma configuração especial. A entrada transmitida ao mesmo pela função do orchestrator estiver localizada no `context.bindings` objeto sob o nome do `activitytrigger` enlace - neste caso, `context.bindings.name`. O nome do enlace pode ser definido como um parâmetro da função exportado e acedidos diretamente, que é o que faz o código de exemplo.

## <a name="run-the-sample"></a>Executar o exemplo

Para executar o `E1_HelloSequence` orchestration, o HTTP POST seguinte pedido de envio.

```
POST http://{host}/orchestrators/E1_HelloSequence
```

Por exemplo, se estiver a executar o exemplo de uma aplicação de função com o nome "myfunctionapp", substitua "{anfitrião}" com "myfunctionapp.azurewebsites.net".

O resultado é uma resposta de HTTP 202, como esta (cortado de uma forma abreviada):

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Neste momento, a orquestração é colocada na fila cópias de segurança e começa a executar imediatamente. O URL no `Location` cabeçalho pode ser utilizado para verificar o estado de execução.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/96924899c16d43b08a536de376ac786b?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

O resultado é o estado da orquestração. Este é executado e concluído rapidamente, para que vê-lo no *concluído* Estado com uma resposta semelhante (cortado de uma forma abreviada):

```
HTTP/1.1 200 OK
Content-Length: 179
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":null,"output":["Hello Tokyo!","Hello Seattle!","Hello London!"],"createdTime":"2017-06-29T05:24:57Z","lastUpdatedTime":"2017-06-29T05:24:59Z"}
```

Como pode ver, a `runtimeStatus` da instância é *concluído* e `output` contém o resultado da execução de função do orchestrator JSON serializado.

> [!NOTE]
> O ponto final de HTTP POST iniciado a função do orchestrator é implementado na aplicação de exemplo como um HTTP acionar a função com o nome "HttpStart". Pode implementar a lógica de arranque semelhantes para outros tipos de Acionador, como `queueTrigger`, `eventHubTrigger`, ou `timerTrigger`.

Consulte os registos de execução de função. O `E1_HelloSequence` função iniciada e concluída várias vezes devido ao comportamento de repetição descrito a [descrição geral](durable-functions-overview.md). Por outro lado, ocorreram apenas três execuções de `E1_SayHello` , uma vez que esses execuções de função não obter reproduzidas.

## <a name="visual-studio-sample-code"></a>Código de exemplo do Visual Studio

Eis o orchestration como um único c# ficheiro um projeto do Visual Studio:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

## <a name="next-steps"></a>Passos Seguintes

Este exemplo foi demonstrado uma orquestração de encadeamento de função simple. O exemplo seguinte mostra como implementar o padrão fan-out/fan-em. 

> [!div class="nextstepaction"]
> [Executar o exemplo Fan-out/fan-em](durable-functions-cloud-backup.md)
