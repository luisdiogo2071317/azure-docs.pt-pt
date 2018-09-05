---
title: Ligação do Twilio de funções do Azure
description: Compreenda como utilizar o Twilio enlaces com as funções do Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: das funções do Azure, funções, processamento de eventos, computação dinâmica, arquitetura sem servidor
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/09/2018
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d973e4003f8501fb3b4d1b461703bd0b869c205
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669159"
---
# <a name="twilio-binding-for-azure-functions"></a>Ligação do Twilio para as funções do Azure

Este artigo explica como enviar mensagens de texto usando [Twilio](https://www.twilio.com/) enlaces no funções do Azure. Azure funções suporta enlaces de saída do Twilio.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pacotes - funções 1.x

Os enlaces do Twilio são fornecidos na [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) pacote NuGet, versão 1.x. Código-fonte para o pacote está no [sdk do webjobs do azure](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.Twilio/) repositório do GitHub.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pacotes - funções 2.x

Os enlaces do Twilio são fornecidos na [Microsoft.Azure.WebJobs.Extensions.Twilio](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio) pacote NuGet, versão 3.x. Código-fonte para o pacote está no [sdk do webjobs do azure](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/) repositório do GitHub.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="example---functions-1x"></a>Exemplo - funções 1.x

Veja o exemplo de idioma específico:

* [C#](#c-example)
* [Script do c# (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>Exemplo do c#

A exemplo a seguir mostra um [função c#](functions-dotnet-class-library.md) que envia uma mensagem de texto quando acionado por uma mensagem de fila.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

Este exemplo utiliza o `TwilioSms` atributo com o valor de retorno do método. Uma alternativa é usar o atributo com um `out SMSMessage` parâmetro ou uma `ICollector<SMSMessage>` ou `IAsyncCollector<SMSMessage>` parâmetro.

### <a name="c-script-example"></a>Exemplo de script do c#

O exemplo seguinte mostra um enlace em de saída do Twilio uma *Function* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função usa um `out` parâmetro para enviar uma mensagem de texto.

Eis a vinculação de dados *Function* ficheiro:

Function de exemplo:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Aqui está o código de script do c#:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

Não é possível utilizar os parâmetros no código assíncrono. Aqui está um assíncrono c# script exemplo de código:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="javascript-example"></a>Exemplo de JavaScript

O exemplo seguinte mostra um enlace em de saída do Twilio uma *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace.

Eis a vinculação de dados *Function* ficheiro:

Function de exemplo:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Eis o código JavaScript:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="example---functions-2x"></a>Exemplo - funções 2.x

Veja o exemplo de idioma específico:

* [2.x c#](#2x-c-example)
* [2.x c# script (. csx)](#2x-c-script-example)
* [2.x JavaScript](#2x-javascript-example)

### <a name="2x-c-example"></a>exemplo de 2.x c#

A exemplo a seguir mostra um [função c#](functions-dotnet-class-library.md) que envia uma mensagem de texto quando acionado por uma mensagem de fila.

```cs
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static CreateMessageOptions Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new CreateMessageOptions(new PhoneNumber(order["mobileNumber"].ToString()))
    {
        Body = $"Hello {order["name"]}, thanks for your order!"
    };

    return message;
}
```

Este exemplo utiliza o `TwilioSms` atributo com o valor de retorno do método. Uma alternativa é usar o atributo com um `out CreateMessageOptions` parâmetro ou uma `ICollector<CreateMessageOptions>` ou `IAsyncCollector<CreateMessageOptions>` parâmetro.

### <a name="2x-c-script-example"></a>exemplo de script 2.x c#

O exemplo seguinte mostra um enlace em de saída do Twilio uma *Function* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função usa um `out` parâmetro para enviar uma mensagem de texto.

Eis a vinculação de dados *Function* ficheiro:

Function de exemplo:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Aqui está o código de script do c#:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static void Run(string myQueueItem, out CreateMessageOptions message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the CreateMessageOptions variable.
    message = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

Não é possível utilizar os parâmetros no código assíncrono. Aqui está um assíncrono c# script exemplo de código:

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio.Rest.Api.V2010.Account;
using Twilio.Types;

public static async Task Run(string myQueueItem, IAsyncCollector<CreateMessageOptions> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the CreateMessageOptions variable.
    CreateMessageOptions smsText = new CreateMessageOptions(new PhoneNumber("+1704XXXXXXX"));

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

### <a name="2x-javascript-example"></a>exemplo de JavaScript 2.x

O exemplo seguinte mostra um enlace em de saída do Twilio uma *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace.

Eis a vinculação de dados *Function* ficheiro:

Function de exemplo:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```

Eis o código JavaScript:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="attributes"></a>Atributos

Na [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [TwilioSms](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) atributo.

Para obter informações sobre as propriedades do atributo que pode configurar, consulte [configuração](#configuration). Aqui está um `TwilioSms` exemplo de atributo numa assinatura do método:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(
    AccountSidSetting = "TwilioAccountSid", 
    AuthTokenSetting = "TwilioAuthToken", 
    From = "+1425XXXXXXX" )]
public static SMSMessage Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order,
    TraceWriter log)
{
    ...
}
 ```

Para obter um exemplo completo, consulte [exemplo c#](#c-example).

## <a name="configuration"></a>Configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro e o `TwilioSms` atributo.

|propriedade de Function | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo**|| Tem de ser definido como `twilioSms`.|
|**direção**|| Tem de ser definido como `out`.|
|**name**|| Nome da variável no código de função para a mensagem de texto Twilio SMS. |
|**accountSid**|**AccountSid**| Este valor tem de ser definido para o nome de uma definição de aplicação que contém o Sid de conta do Twilio.|
|**authToken**|**AuthToken**| Este valor tem de ser definido para o nome de uma definição de aplicação que contém o token de autenticação de Twilio.|
|**Para**|**Para**| Este valor é definido como o número de telefone que o texto SMS é enviado para.|
|**De**|**De**| Este valor é definido como o número de telefone que o texto SMS é enviado do.|
|**Corpo**|**Corpo**| Este valor pode ser usado para codificar duro a mensagem de texto SMS se não precisa defini-la dinamicamente no código para a função. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)