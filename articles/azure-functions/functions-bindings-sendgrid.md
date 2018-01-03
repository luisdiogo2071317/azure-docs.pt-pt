---
title: "Enlaces de funções SendGrid do Azure"
description: "Referência de enlaces de funções SendGrid do Azure."
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/29/2017
ms.author: tdykstra
ms.openlocfilehash: aa64bbc764fe96d7c15d62660114222458e5acf4
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/02/2018
---
# <a name="azure-functions-sendgrid-bindings"></a>Enlaces de funções SendGrid do Azure

Este artigo explica como enviar correio eletrónico com [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) enlaces de funções do Azure. As funções do Azure suporta um enlace de saída do SendGrid.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Exemplo

Veja o exemplo de específicas do idioma:

* [C#](#c-example)
* [Script do c# (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>Exemplo do c#

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que utiliza uma fila do Service Bus acionar e um SendGrid vínculo de saída.

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

Pode omitir a definição do atributo `ApiKey` propriedade se tiver a chave de API uma definição de aplicação com o nome "AzureWebJobsSendGridApiKey".

### <a name="c-script-example"></a>Exemplo de script do c#

O exemplo seguinte mostra uma saída de SendGrid enlace num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace.

Segue-se os dados do enlace *function.json* ficheiro:

```json 
{
    "bindings": [
        {
            "name": "message",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

O [configuração](#configuration) secção explica estas propriedades.

Eis o código de script do c#:

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

### <a name="javascript-example"></a>Exemplo de JavaScript

O exemplo seguinte mostra uma saída de SendGrid enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace.

Segue-se os dados do enlace *function.json* ficheiro:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

O [configuração](#configuration) secção explica estas propriedades.

Eis o código JavaScript:

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};
```

## <a name="attributes"></a>Atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.SendGrid](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid).

Para obter informações sobre as propriedades de atributo que pode configurar, consulte [configuração](#configuration). Eis um `SendGrid` exemplo de atributo na assinatura do método:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

Para obter um exemplo completado, consulte [c# exemplo](#c-example).

## <a name="configuration"></a>Configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `SendGrid` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**tipo**|| Necessário - tem de ser definido como `sendGrid`.|
|**direção**|| Necessário - tem de ser definido como `out`.|
|**nome**|| Necessário - o nome da variável utilizado no código de função para o corpo do pedido ou o pedido. Este valor é ```$return``` quando existe apenas um valor de retorno. |
|**apiKey**|**ApiKey**| O nome de uma definição de aplicação que contém a chave de API. Se não conjunto, a aplicação de predefinição definir o nome é "AzureWebJobsSendGridApiKey".|
|**para**|**Para**| endereço de e-mail do destinatário. |
|**do**|**Do**| endereço de correio eletrónico do remetente. |
|**requerente**|**Assunto**| o assunto do e-mail. |
|**texto**|**Texto**| o conteúdo de e-mail. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)