---
title: Acionador de temporizador para as funções do Azure
description: Compreenda como utilizar os acionadores de temporizador nas funções do Azure.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: das funções do Azure, funções, processamento de eventos, computação dinâmica, arquitetura sem servidor
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: tdykstra
ms.custom: ''
ms.openlocfilehash: 4da1ed4e9424950c39f3eb255ead2b39094597fd
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34725450"
---
# <a name="timer-trigger-for-azure-functions"></a>Acionador de temporizador para as funções do Azure 

Este artigo explica como trabalhar com os acionadores de temporizador nas funções do Azure. Um acionador de temporizador permite-lhe executar uma função com base numa agenda. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacotes - funciona 1. x

O acionador de temporizador é fornecido no [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) pacote NuGet, versão 2. x. Código de origem para o pacote está a ser o [azure-webjobs-sdk-extensões](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Timers/) repositório do GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Pacotes - funciona 2. x

O acionador de temporizador é fornecido no [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) pacote NuGet, versão 3. Código de origem para o pacote está a ser o [azure-webjobs-sdk-extensões](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/) repositório do GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="example"></a>Exemplo

Veja o exemplo de específicas do idioma:

* [C#](#trigger---c-example)
* [Script do c# (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="c-example"></a>Exemplo do c#

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que é executado a cada cinco minutos:

```cs
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

### <a name="c-script-example"></a>Exemplo de script do c#

O exemplo seguinte mostra um acionador de temporizador enlace num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função escreve um registo que indica se esta invocação de função é devido a uma ocorrência de agenda em falta.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Eis o código de script do c#:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

### <a name="f-example"></a>O F # exemplo

O exemplo seguinte mostra um acionador de temporizador enlace num *function.json* ficheiro e uma [função de script F #](functions-reference-fsharp.md) que utiliza o enlace. A função escreve um registo que indica se esta invocação de função é devido a uma ocorrência de agenda em falta.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Eis o código de script F #:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

### <a name="javascript-example"></a>Exemplo de JavaScript

O exemplo seguinte mostra um acionador de temporizador enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função escreve um registo que indica se esta invocação de função é devido a uma ocorrência de agenda em falta.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Eis o código de script de JavaScript:

```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="attributes"></a>Atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs).

O construtor do atributo aceita uma expressão de CRON ou um `TimeSpan`. Pode utilizar `TimeSpan` apenas se a aplicação de função está em execução num plano de serviço de aplicações. O exemplo seguinte mostra uma expressão de CRON:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    if (myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
 ```

## <a name="configuration"></a>Configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `TimerTrigger` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo** | n/d | Tem de ser definida para "timerTrigger". Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure.|
|**direção** | n/d | Tem de ser definida para "em". Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure. |
|**name** | n/d | O nome da variável que representa o objeto temporizador no código da função. | 
|**schedule**|**ScheduleExpression**|A [expressão CRON](#cron-expressions) ou um [TimeSpan](#timespan) valor. A `TimeSpan` pode ser utilizado apenas para uma aplicação de função que é executado um plano do App Service. Pode colocar a expressão de agenda uma definição de aplicação e definir esta propriedade para a aplicação encapsulado do nome da definição **%** inicia, tal como neste exemplo: "% ScheduleAppSetting %". |
|**runOnStartup**|**RunOnStartup**|Se `true`, a função é invocada quando inicia o tempo de execução. Por exemplo, o tempo de execução é iniciado quando a aplicação de função reativado depois de ficar inativo devido a inatividade. Quando a aplicação de função reinicia devido a alterações de função e, quando a aplicação de função aumenta horizontalmente de forma. Por isso, **runOnStartup** deve raramente se alguma vez definir `true`, uma vez que este irá fazer código executar em alturas altamente imprevisíveis.|
|**useMonitor**|**UseMonitor**|Definido como `true` ou `false` para indicar se a agenda deve ser monitorizada. Agenda de monitorização mantém a agenda de ocorrências para ajudar a garantir que a agenda é mantida corretamente, mesmo quando reiniciar instâncias da aplicação de função. Se não definir explicitamente, a predefinição é `true` para agendas que tem um intervalo de periodicidade maior que 1 minuto. Para agendas acionam mais do que uma vez por minuto, a predefinição é `false`.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Utilização

Quando uma função de Acionador de temporizador é invocada, o [objeto temporizador](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) é transmitida para a função. O seguinte JSON é uma representação de exemplo do objeto temporizador. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00+00:00",
        "LastUpdated":"2016-10-04T10:16:00+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

O `IsPastDue` propriedade é `true` quando a invocação de função atual for posterior à agendada. Por exemplo, um reinício de aplicação de função pode causar uma invocação ser perdidos.

## <a name="cron-expressions"></a>Expressões de CRON 

Uma expressão de CRON para o acionador de temporizador de funções do Azure inclui seis campos: 

`{second} {minute} {hour} {day} {month} {day-of-week}`

Cada campo pode ter um dos seguintes tipos de valores:

|Tipo  |Exemplo  |Quando activado  |
|---------|---------|---------|
|Um valor específico |<nobr>"0 5 * * * *"</nobr>|em hh:05:00 em que hh significa a cada hora (uma vez a uma hora)|
|Todos os valores (`*`)|<nobr>"0 * 5 * * *"</nobr>|em 5:mm: 00 diariamente, onde é mm a cada minuto da hora (60 vezes por dia)|
|Um intervalo (`-` operador)|<nobr>"5-7 * * * * *"</nobr>|em hh:mm:05, hh:mm:06 e hh:mm:07 em que HH: mm é a cada minuto da hora em hora (3 vezes um minuto)|  
|Um conjunto de valores (`,` operador)|<nobr>"5,8,10 **** *"</nobr>|em hh:mm:05, hh:mm:08 e hh:mm:10 em que HH: mm é a cada minuto da hora em hora (3 vezes um minuto)|
|Um valor de intervalo (`/` operador)|<nobr>"0 */5 * * * *"</nobr>|no hh:05:00, hh:10:00, hh:15:00, e assim sucessivamente através de hh:55:00 em que hh significa a cada hora (12 vezes uma hora)|

Para especificar meses ou dias pode utilizar com três letras abreviaturas em vez de valores numéricos. Por exemplo, utilize Jan para Janeiro ou Sun para Domingo.

### <a name="cron-examples"></a>Exemplos CRON

Seguem-se alguns exemplos de expressões de CRON que pode utilizar para o acionador de temporizador de funções do Azure.

|Exemplo|Quando activado  |
|---------|---------|
|"0 */5 * * * *"|uma vez a cada cinco minutos|
|"0 0 * * * *"|uma vez na parte superior de cada hora|
|"0 0 */2 * * *"|uma vez a cada duas horas|
|"0 0 9-17 * * *"|uma vez a cada hora de 09: 00 para as 17: 00|
|"0 30 9 * * *"|às 9:30 AM todos os dias|
|"0 30 9 * * 1 a 5"|às 9:30 AM cada dia da semana|

>[!NOTE]   
>Pode encontrar exemplos de expressão de CRON online, mas muitos dos mesmos omitir o `{second}` campo. Se copiar a partir de um deles, adicione o falta `{second}` campo. Normalmente, é aconselhável um zero nesse campo, não um asterisco.

### <a name="cron-time-zones"></a>CRON fusos horários

Os números numa expressão de CRON fazer referência a uma hora e data, não é um intervalo de tempo. Por exemplo, um 5 no `hour` campo refere-se a 5 horas da Manhã, não a cada 5 horas.

O fuso horário predefinido utilizado com as expressões de CRON está Hora Universal Coordenada (UTC). Para que a expressão de CRON baseado no fuso horário outro, crie uma definição de aplicação para a sua aplicação de função com o nome `WEBSITE_TIME_ZONE`. Defina o valor para o nome do fuso horário pretendido, conforme mostrado no [Microsoft fuso horário índice](https://technet.microsoft.com/library/cc749073). 

Por exemplo, *hora padrão do Leste* é UTC-05:00. Ter o temporizador acionar fire em 10:00 EST diariamente, utilize a seguinte expressão CRON que as contas do fuso horário UTC:

```json
"schedule": "0 0 15 * * *",
``` 

Ou criar uma definição de aplicação para a sua aplicação de função com o nome `WEBSITE_TIME_ZONE` e defina o valor **hora padrão do Leste**.  Em seguida, utiliza a seguinte expressão de CRON: 

```json
"schedule": "0 0 10 * * *",
``` 

## <a name="timespan"></a>TimeSpan

 A `TimeSpan` pode ser utilizado apenas para uma aplicação de função que é executado um plano do App Service.

Ao contrário de uma expressão de CRON um `TimeSpan` valor Especifica o intervalo de tempo entre cada invocação de função. Quando uma função concluir após a execução mais do que o intervalo especificado, o temporizador imediatamente invoca a função novamente.

Expresso como uma cadeia, a `TimeSpan` formato é `hh:mm:ss` quando `hh` é inferior a 24. Quando os primeiros dois dígitos 24 ou superior, o formato é `dd:hh:mm`. Eis alguns exemplos:

|Exemplo |Quando activado  |
|---------|---------|
|"01:00:00" | a cada hora        |
|"00:01:00"|cada minuto         |
|"24: 00:00" | cada 24 dias        |

## <a name="scale-out"></a>Expandir

Se uma aplicação de função aumenta horizontalmente de forma a várias instâncias, apenas uma única instância de uma função acionada por temporizador é executada em todas as instâncias.

## <a name="function-apps-sharing-storage"></a>Armazenamento de partilha de aplicações de função

Se partilhar uma conta de armazenamento por várias aplicações de função, certifique-se de que cada aplicação de função tem um outro `id` no *host.json*. Pode omitir o `id` propriedade ou definir manualmente cada aplicação de função `id` para um valor diferente. O acionador de temporizador utiliza um bloqueio de armazenamento para se certificar de que vai haver apenas uma instância de temporizador quando uma aplicação de função aumenta horizontalmente de forma a várias instâncias. Se duas aplicações de função partilham o mesmo `id` e cada utiliza um acionador de temporizador, irá executar apenas um temporizador.

## <a name="retry-behavior"></a>Comportamento de tentativas

Ao contrário do acionador de fila, o acionador de temporizador não repita após a falha de uma função. Quando uma função falha, não é chamado novamente até à próxima vez na agenda.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Ir para um guia de introdução que utiliza um acionador de temporizador](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
