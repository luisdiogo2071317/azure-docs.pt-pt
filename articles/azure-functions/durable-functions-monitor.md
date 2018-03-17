---
title: "Monitores de funções duráveis - Azure"
description: "Saiba como implementar um monitor de estado utilizando a extensão de funções durável para as funções do Azure."
services: functions
author: kashimiz
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/16/2018
ms.author: azfuncdf
ms.openlocfilehash: 617b33a9f860ce3b06ff560de22824037eab8332
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/17/2018
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Cenário de monitor nas funções durável - exemplo de observador de Meteorologia

O padrão de monitor refere-se para uma flexível *periódica* processo num fluxo de trabalho - por exemplo, consulta até que certas condições sejam cumpridas. Este artigo explica um exemplo que utiliza [funções durável](durable-functions-overview.md) para implementar a monitorização.

## <a name="prerequisites"></a>Pré-requisitos

* Siga as instruções em [instalar funções durável](durable-functions-install.md) para configurar o exemplo.
* Este artigo pressupõe que já leu o [Hello sequência](durable-functions-sequence.md) instruções de exemplo.

## <a name="scenario-overview"></a>Descrição geral do cenário

Este exemplo monitoriza condições de Meteorologia atual de uma localização e alertas de um utilizador pelo SMS quando os skies encriptados. Pode utilizar uma função de acionada por temporizador regular para verificar a meteorologia e enviar alertas. No entanto, é um problema com esta abordagem **gestão duração**. Se apenas um alerta deve ser enviado, o monitor tem de desativar o próprio após limpar Meteorologia é detetada. O padrão de monitorização pode terminar a sua própria execução, entre outras vantagens:

* Monitores executadas em intervalos, as agendas não: um acionador de temporizador *executa* a cada hora; um monitor *aguarda* uma hora entre ações. Ações de um monitor irão não se podem sobrepor a menos que especificado, que pode ser importante para tarefas de longa execução.
* Os monitores podem ter intervalos dinâmicos: o tempo de espera, pode alterar com base em alguma condição.
* Monitores podem terminar quando alguma condição é cumprida ou terminada por outro processo.
* Monitores podem demorar parâmetros. O exemplo mostra como o mesmo processo de monitorização de Meteorologia pode ser aplicado a qualquer pedida de localização e o número de telefone.
* Monitores que são dimensionáveis. Uma vez cada monitor é uma instância de orquestração, vários monitores podem ser criados sem ter de criar novas funções ou definir mais de código.
* Monitores facilmente integram fluxos de trabalho maior. Um monitor pode ser uma secção de uma função de orquestração mais complexa, ou um [orchestration secundárias](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-sub-orchestrations).

## <a name="configuring-twilio-integration"></a>Configurar a integração do Twilio

[!INCLUDE [functions-twilio-integration](../../includes/functions-twilio-integration.md)]

## <a name="configuring-weather-underground-integration"></a>Configurar a integração Meteorologia Underground

Este exemplo envolve utilizar a API Underground Meteorologia para verificar as condições de Meteorologia atual para uma localização.

A primeira coisa que precisa é uma conta Meteorologia Underground. Pode criar uma gratuitamente em [ https://www.wunderground.com/signup ](https://www.wunderground.com/signup). Assim que tiver uma conta, terá de adquirir uma chave de API. Pode fazê-lo, visitando [ https://www.wunderground.com/weather/api ](https://www.wunderground.com/weather/api), em seguida, selecionar as definições de chave. O plano de Stratus programador está livre e suficiente para executar este exemplo.

Depois de ter uma chave de API, adicione o seguinte **definição de aplicação** à sua aplicação de função.

| Nome da definição de aplicação | Descrição de valor |
| - | - |
| **WeatherUndergroundApiKey**  | A chave de API Underground Meteorologia. |

## <a name="the-functions"></a>As funções

Este artigo explica as seguintes funções na aplicação de exemplo:

* `E3_Monitor`: Uma função do orchestrator que chama `E3_GetIsClear` periodicamente. Chama `E3_SendGoodWeatherAlert` se `E3_GetIsClear` devolve true.
* `E3_GetIsClear`: Uma função de atividade que verifica as condições de Meteorologia atual para uma localização.
* `E3_SendGoodWeatherAlert`: Uma função de atividade que envia uma mensagem SMS através do Twilio.

As secções seguintes explicam o código que são utilizados para programação portal do Azure e configuração. O código para o desenvolvimento de Visual Studio é apresentado no final do artigo.
 
## <a name="the-weather-monitoring-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>Meteorologia monitorização orchestration (código de exemplo portal Visual Studio Code e o Azure)

O **E3_Monitor** função utiliza a norma *function.json* para funções do orchestrator.

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_Monitor/function.json)]

Eis o código que implementa a função:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_Monitor/run.csx)]

Esta função do orchestrator efetua as seguintes ações:

1. Obtém o **MonitorRequest** constituídas pelo *localização* para monitorizar e a *número de telefone* para que irá enviar uma notificação de SMS.
2. Determina a hora de expiração do monitor. Este exemplo utiliza um valor hard-coded de uma forma abreviada.
3. Chamadas **E3_GetIsClear** para determinar se existem skies encriptados na localização pedida.
4. Se o Meteorologia estiver desmarcada, chama **E3_SendGoodWeatherAlert** para enviar uma notificação de SMS para o número de telefone pedida.
5. Cria um temporizador durável para retomar a orquestração no próximo intervalo de consulta. Este exemplo utiliza um valor hard-coded de uma forma abreviada.
6. Continua a ser executado até o [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) transmite a hora de expiração do monitor ou um alerta SMS é enviada.

Várias instâncias do orchestrator podem executar em simultâneo, enviando vários **MonitorRequests**. A localização para monitorizar e o número de telefone para enviar um alerta SMS para podem ser especificados.

## <a name="strongly-typed-data-transfer"></a>Transferência de dados de tipo seguro

O orchestrator requer várias peças de dados, por isso, [partilhado POCO objetos](functions-reference-csharp.md#reusing-csx-code) são utilizados para a transferência de dados de tipo seguro: [!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/MonitorRequest.csx)]

[!code-csharp[Main](~/samples-durable-functions/samples/csx/shared/Location.csx)]

## <a name="helper-activity-functions"></a>Funções de atividade de programa auxiliar

Como com outros exemplos, as funções de atividade de programa auxiliar estão regulares funções que utilizam o `activityTrigger` acionar o enlace. O **E3_GetIsClear** função obtém as condições de Meteorologia atual utilizando a API Underground meteorologia e determina se o sky está desmarcada. O *function.json* é definidos do seguinte modo:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/function.json)]

E Eis a implementação. Como os POCOs utilizados para a transferência de dados, lógica para lidar com a API chamar e analisar a resposta JSON é abstracted para uma classe partilhada. Pode encontrá-lo como parte do [código de exemplo do Visual Studio](#run-the-sample).

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_GetIsClear/run.csx)]

O **E3_SendGoodWeatherAlert** função utiliza o enlace do Twilio para enviar uma mensagem SMS notificar o utilizador final que é uma boa altura para uma movimentação pela. O *function.json* é simples:

[!code-json[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/function.json)]

E Eis o código que envia a mensagem SMS:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E3_SendGoodWeatherAlert/run.csx)]

## <a name="run-the-sample"></a>Executar o exemplo

Utilizar as funções acionadas por HTTP incluídas no exemplo, pode iniciar a orquestração ao enviar o pedido de HTTP POST seguinte:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "Location": { "City": "Redmond", "State": "WA" }, "Phone": "+1425XXXXXXX" }
```
```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

O **E3_Monitor** instância é iniciado e as condições de Meteorologia atual para a localização do pedido de consulta. Se o Meteorologia estiver desmarcada, aquele invoca uma função de atividade para enviar um alerta; caso contrário, define um temporizador. Quando o temporizador expirarem, será retomada a orquestração.

Pode ver a atividade do orchestration observando a função inicia sessão no portal funções do Azure.

```
2018-03-01T01:14:41.649 Function started (Id=2d5fcadf-275b-4226-a174-f9f943c90cd1)
2018-03-01T01:14:42.741 Started orchestration with ID = '1608200bb2ce4b7face5fc3b8e674f2e'.
2018-03-01T01:14:42.780 Function completed (Success, Id=2d5fcadf-275b-4226-a174-f9f943c90cd1, Duration=1111ms)
2018-03-01T01:14:52.765 Function started (Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb)
2018-03-01T01:14:52.890 Received monitor request. Location: Redmond, WA. Phone: +1425XXXXXXX.
2018-03-01T01:14:52.895 Instantiating monitor for Redmond, WA. Expires: 3/1/2018 7:14:52 AM.
2018-03-01T01:14:52.909 Checking current weather conditions for Redmond, WA at 3/1/2018 1:14:52 AM.
2018-03-01T01:14:52.954 Function completed (Success, Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb, Duration=189ms)
2018-03-01T01:14:53.226 Function started (Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859)
2018-03-01T01:14:53.808 Function completed (Success, Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859, Duration=582ms)
2018-03-01T01:14:53.967 Function started (Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c)
2018-03-01T01:14:53.996 Next check for Redmond, WA at 3/1/2018 1:44:53 AM.
2018-03-01T01:14:54.030 Function completed (Success, Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c, Duration=62ms)
```

A orquestração será [terminar](durable-functions-instance-management.md#terminating-instances) depois do tempo limite é atingiu o número ou desmarque skies são detetados. Também pode utilizar `TerminateAsync` dentro de outra função ou da invocação de **terminatePostUri** webhook HTTP POST referenciada na resposta 202 acima, substituindo `{text}` com o motivo para terminação:

```
POST https://{host}/admin/extensions/DurableTaskExtension/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="visual-studio-sample-code"></a>Código de exemplo do Visual Studio

Eis o orchestration como um único c# ficheiro um projeto do Visual Studio:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs)]

## <a name="next-steps"></a>Passos Seguintes

Este exemplo foi demonstrado como utilizar funções durável para monitorizar o estado de uma origem externa utilizando [temporizadores duráveis](durable-functions-timers.md) e lógica condicional. O exemplo seguinte mostra como utilizar eventos externos e [temporizadores duráveis](durable-functions-timers.md) para lidar com uma interação humana.

> [!div class="nextstepaction"]
> [Executar o exemplo de interação humana](durable-functions-phone-verification.md)
