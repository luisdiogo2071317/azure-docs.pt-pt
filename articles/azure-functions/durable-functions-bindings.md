---
title: Enlaces de funções duráveis - Azure
description: Como utilizar os acionadores e enlaces para a extensão de Functons durável para as funções do Azure.
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
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 370e6e2c569aaf6d9289bddccde2174b4dd2ee97
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33763361"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Enlaces de funções duráveis (funções do Azure)

O [funções durável](durable-functions-overview.md) extensão introduz dois novos enlaces de Acionador que controlam a execução das funções do orchestrator e a atividade. Também introduz um enlace de saída que age como um cliente para o tempo de execução durável funções.

## <a name="orchestration-triggers"></a>Acionadores de orquestração

O acionador orchestration permite-lhe criar as funções do orchestrator durável. Este acionador suporta iniciar novas instâncias de função do orchestrator e retomar a instâncias de função do orchestrator existente "aguardam" uma tarefa.

Quando utilizar as ferramentas do Visual Studio para as funções do Azure, o acionador de orquestração for configurado utilizando o [OrchestrationTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html) atributo .NET.

Quando escrever funções do orchestrator no linguagens de scripts (por exemplo, no portal do Azure), o acionador de orquestração é definido do seguinte objeto JSON no `bindings` matriz do *function.json* ficheiro:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration` é o nome da orquestração. Este é o valor que os clientes devem utilizar quando pretendem inicie novas instâncias desta função do orchestrator. Esta propriedade é opcional. Se não for especificado, o nome da função é utilizado.

Internamente este enlace de Acionador consulta uma série de filas na conta de armazenamento predefinido para a aplicação de função. Estas filas são detalhes de implementação interno da extensão, sendo o motivo não estão explicitamente configurados nas propriedades do enlace.

### <a name="trigger-behavior"></a>Comportamento de Acionador

Seguem-se algumas notas sobre o acionador orchestration:

* **Single-threading** -um thread dispatcher único é utilizado para a execução de função do orchestrator todos os numa instância de anfitrião único. Por este motivo, é importante certificar-se de que o código de função do orchestrator é eficiente e não efetuar qualquer e/s. Também é importante certificar-se de que este thread não qualquer trabalho async, exceto quando a aguardar em tipos de tarefas específicas de funções durável.
* **Processamento de mensagens poison** -não são suportadas mensagens nocivas em acionadores de orquestração.
* **Mensagem de visibilidade** -as mensagens de Acionador de orquestração são removidas e mantidas invisíveis durante um período de tempo configurável. A visibilidade destas mensagens é renovar automaticamente, desde que a aplicação de função está em execução e bom estado de funcionamento.
* **Valores de retorno** -devolver valores são serializados para JSON e persistente para a tabela de histórico de orquestração do Table storage do Azure. Estes valores de retorno podem ser consultados pelo cliente de orquestração de enlace, descrito mais à frente.

> [!WARNING]
> As funções do Orchestrator nunca devem utilizar qualquer entrada ou saída enlaces que não seja o orchestration acionar o enlace. Se o fizer, tem o potencial de causar problemas com a extensão de tarefa durável porque os enlaces não podem obedecer o thread de único e as regras de e/s.

### <a name="trigger-usage"></a>Utilização de Acionador

O acionador de orquestração enlace suporta entradas e saídas. Seguem-se alguns aspetos a conhecer a entrada e saída do processamento de:

* **entradas** -as funções de orquestração suportam apenas [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) como um tipo de parâmetro. A anulação da serialização de entradas diretamente na assinatura da função não é suportada. Código tem de utilizar o [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1) método de obtenção do orchestrator entradas de função. Estas entradas tem de ser tipos serializáveis JSON.
* **produz** -os accionadores de orquestração suportam valores de saída, bem como entradas. O valor de retorno da função é utilizado para atribuir o valor de saída e tem de ser serializáveis JSON. Se uma função devolve `Task` ou `void`, um `null` valor será guardado como saída.

### <a name="trigger-sample"></a>Exemplo de Acionador

Segue-se um exemplo de que a função de orchestrator "Olá mundo" mais simples pode ter o seguinte aspeto:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas no funções v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const name = context.df.getInput();
    return `Hello ${name}!`;
});
```

> [!NOTE]
> Devem utilizar JavaScript orchestrators `return`. O `durable-functions` biblioteca encarrega-se de chamar o `context.done` método.

A maioria das funções do orchestrator chamar funções de atividade, pelo que este é um exemplo de "Olá mundo" que demonstra como chamar uma função de atividade:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas no funções v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const name = context.df.getInput();
    const result = yield context.df.callActivityAsync("SayHello", name);
    return result;
});
```

## <a name="activity-triggers"></a>Acionadores de atividade

O acionador de atividade permite-lhe criar as funções que são denominadas pelas funções do orchestrator.

Se estiver a utilizar o Visual Studio, o acionador de atividade é configurado utilizando o [ActvityTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) atributo .NET. 

Se estiver a utilizar o portal do Azure para desenvolvimento, o acionador de atividade é definido através do seguinte objeto JSON no `bindings` matriz de *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity` é o nome da atividade. Este é o valor que as funções do orchestrator utilizarem para invocar esta função de atividade. Esta propriedade é opcional. Se não for especificado, o nome da função é utilizado.

Internamente este enlace de Acionador consulta uma fila na conta de armazenamento predefinido para a aplicação de função. Esta fila é um detalhe de implementação interno da extensão, que é a razão não for explicitamente configurado nas propriedades do enlace.

### <a name="trigger-behavior"></a>Comportamento de Acionador

Seguem-se algumas notas sobre o acionador de atividade:

* **Thread** -ao contrário do acionador de orquestração, acionadores de atividade não tem quaisquer restrições à volta de threading ou e/s. Pode tratados como funções regulares.
* **Processamento de mensagens poising** -não são suportadas mensagens nocivas em acionadores de atividade.
* **Mensagem de visibilidade** -as mensagens de Acionador de atividade são removidas e mantidas invisíveis durante um período de tempo configurável. A visibilidade destas mensagens é renovar automaticamente, desde que a aplicação de função está em execução e bom estado de funcionamento.
* **Valores de retorno** -devolver valores são serializados para JSON e persistente para a tabela de histórico de orquestração do Table storage do Azure.

> [!WARNING]
> O back-end de armazenamento para as funções de atividade é um detalhe de implementação e código de utilizador não deve interagir com estas entidades de armazenamento diretamente.

### <a name="trigger-usage"></a>Utilização de Acionador

O acionador de atividade enlace suporta entradas e saídas, tal como o acionador de orquestração. Seguem-se alguns aspetos a conhecer a entrada e saída do processamento de:

* **entradas** -nativamente utilizarem funções de atividade [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) como um tipo de parâmetro. Em alternativa, uma função de atividade pode ser declarada com qualquer tipo de parâmetro que é JSON serializável. Quando utiliza `DurableActivityContext`, pode chamar [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) obter e anular a serialização a função de atividade de entrada.
* **produz** -as funções de atividade suportam valores de saída, bem como entradas. O valor de retorno da função é utilizado para atribuir o valor de saída e tem de ser serializáveis JSON. Se uma função devolve `Task` ou `void`, um `null` valor será guardado como saída.
* **Metadados** -as funções de atividade podem vincular a um `string instanceId` parâmetro para obter o ID de instância da orquestração do principal.

### <a name="trigger-sample"></a>Exemplo de Acionador

Segue-se um exemplo de uma função de atividade "Olá mundo" simple poderá aspeto:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas no funções v2)

```javascript
module.exports = function(context) {
    context.done(null, `Hello ${context.bindings.name}!`);
};
```

O tipo de parâmetro predefinidos para o `ActivityTriggerAttribute` enlace é `DurableActivityContext`. No entanto, os acionadores de atividade também tipos de suporte do enlace diretamente para o JSON serializeable (incluindo tipos primitivos), para que a mesma função pode ser simplificada como segue:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas no funções v2)

```javascript
module.exports = function(context, name) {
    context.done(null, `Hello ${name}!`);
};
```

### <a name="passing-multiple-parameters"></a>Vários parâmetros a transmitir 

Não é possível passar diretamente de vários parâmetros para uma função de atividade. A recomendação é neste caso, passa uma matriz de objetos ou utilizar [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) objetos.

O exemplo seguinte está a utilizar novas funcionalidades de [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) adicionada com [c# 7](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7#tuples):

```csharp
[FunctionName("GetCourseRecommendations")]
public static async Task<dynamic> RunOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string major = "ComputerScience";
    int universityYear = context.GetInput<int>();

    dynamic courseRecommendations = await context.CallActivityAsync<dynamic>("CourseRecommendations", (major, universityYear));
    return courseRecommendations;
}

[FunctionName("CourseRecommendations")]
public static async Task<dynamic> Mapper([ActivityTrigger] DurableActivityContext inputs)
{
    // parse input for student's major and year in university 
    (string Major, int UniversityYear) studentInfo = inputs.GetInput<(string, int)>();

    // retrieve and return course recommendations by major and university year
    return new {
        major = studentInfo.Major,
        universityYear = studentInfo.UniversityYear,
        recommendedCourses = new []
        {
            "Introduction to .NET Programming",
            "Introduction to Linux",
            "Becoming an Entrepreneur"
        }
    };
}
```

## <a name="orchestration-client"></a>Cliente de orquestração

O cliente de orquestração de enlace permite-lhe escrever funções que interagem com as funções do orchestrator. Por exemplo, pode agir em instâncias de orquestração das seguintes formas:
* Iniciá-los.
* Consulta o respetivo estado.
* Terminá-los.
* Envie eventos aos mesmos, enquanto que estiver a executar.

Se estiver a utilizar o Visual Studio, é possível vincular ao cliente orchestration utilizando o [OrchestrationClientAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) atributo .NET.

Se estiver a utilizar linguagens de scripts (por exemplo, *.csx* ficheiros) para o desenvolvimento, o acionador de orquestração é definido do seguinte objeto JSON no `bindings` matriz de *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "out"
}
```

* `taskHub` -Utilizar em cenários em que várias aplicações de função partilham a mesma conta de armazenamento, mas tem de ser isolados uns dos outros. Se não for especificado, o valor predefinido de `host.json` é utilizado. Este valor tem de corresponder ao valor utilizado pelas funções do orchestrator de destino.
* `connectionName` -O nome de uma definição de aplicação que contém uma cadeia de ligação da conta de armazenamento. A conta de armazenamento representada por esta cadeia de ligação tem de ser a mesma utilizado pelas funções do orchestrator de destino. Se não for especificado, a cadeia de ligação de conta de armazenamento predefinido para a aplicação de função é utilizada.

> [!NOTE]
> Na maioria dos casos, recomendamos que omitir estas propriedades e dependem o comportamento predefinido.

### <a name="client-usage"></a>Utilização do cliente

C# funções, normalmente, vincular ao `DurableOrchestrationClient`, que lhe dê acesso total a todos os clientes APIs suportados pelas funções durável. APIs no objeto de cliente incluem:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)

Em alternativa, é possível vincular ao `IAsyncCollector<T>` onde `T` é [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) ou `JObject`.

Consulte o [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) documentação da API para obter detalhes adicionais sobre estas operações.

### <a name="client-sample-visual-studio-development"></a>Exemplo de cliente (desenvolvimento de Visual Studio)

Segue-se uma função de acionada pela fila de exemplo que inicia uma orquestração "Olámundo".

```csharp
[FunctionName("QueueStart")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [OrchestrationClient] DurableOrchestrationClient starter)
{
    // Orchestration input comes from the queue message content.
    return starter.StartNewAsync("HelloWorld", input);
}
```

### <a name="client-sample-not-visual-studio"></a>Exemplo de cliente (não o Visual Studio)

Se não estiver a utilizar o Visual Studio para desenvolvimento, pode criar o seguinte *function.json* ficheiro. Este exemplo mostra como configurar uma função de acionada pela fila de mensagens em fila que utiliza o cliente de orquestração durável enlace:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "queueTrigger",
      "queueName": "durable-function-trigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "out"
    }
  ],
  "disabled": false
} 
```

Seguem-se exemplos de específicas do idioma que inicie novas instâncias de função do orchestrator.

#### <a name="c-sample"></a>Exemplo do c#

O exemplo seguinte mostra como utilizar o cliente de orquestração durável enlace para iniciar uma nova instância de função a partir de uma função de script do c#:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static Task<string> Run(string input, DurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

#### <a name="javascript-sample"></a>Exemplo de JavaScript

O exemplo seguinte mostra como utilizar o cliente de durável orchestration para iniciar uma nova instância de função a partir de uma função de JavaScript de enlace:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null, id);
};
```

Obter mais detalhes sobre a partir de instâncias podem ser encontrados na [instância gestão](durable-functions-instance-management.md).

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre os comportamentos de pontos de verificação e repetição](durable-functions-checkpointing-and-replay.md)

