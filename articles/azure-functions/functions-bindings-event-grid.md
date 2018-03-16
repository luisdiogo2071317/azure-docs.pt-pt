---
title: "Acionador de grelha de eventos para as funções do Azure"
description: "Compreenda como processar eventos de grelha de evento nas funções do Azure."
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/26/2018
ms.author: tdykstra
ms.openlocfilehash: 5039798d76017d93b77d724b2e6bca6712af0370
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/16/2018
---
# <a name="event-grid-trigger-for-azure-functions"></a>Acionador de grelha de eventos para as funções do Azure

Este artigo explica como lidar com [eventos grelha](../event-grid/overview.md) eventos das funções do Azure.

Grelha de eventos é um serviço Azure que envia pedidos HTTP para o notificar sobre eventos que ocorrem no *publicadores*. O serviço ou o recurso que origina o evento, é um publicador. Por exemplo, uma conta de armazenamento de Blobs do Azure é um publicador, não sendo um carregamento de blob ou eliminação de um evento. Alguns [serviços do Azure têm suporte incorporado para publicar eventos à grelha de evento](../event-grid/overview.md#event-publishers). 

Evento *processadores* receber e a processar eventos. As funções do Azure é um dos vários [serviços do Azure que têm suporte incorporado para processar eventos de evento grelha](../event-grid/overview.md#event-handlers). Neste artigo, irá aprender a utilizar um acionador de grelha de eventos para invocar uma função quando é recebido um evento de grelha de eventos.

Se preferir, pode utilizar um acionador HTTP para processar eventos de grelha de eventos; consulte [utilizar um acionador HTTP como um acionador de grelha de evento](#use-an-http-trigger-as-an-event-grid-trigger) posteriormente neste artigo.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Pacotes

O acionador de grelha de eventos é fornecido no [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) pacote NuGet. Código de origem para o pacote está a ser o [eventgrid-extensão de azure-funções](https://github.com/Azure/azure-functions-eventgrid-extension) repositório do GitHub.

<!--
If you want to bind to the `Microsoft.Azure.EventGrid.Models.EventGridEvent` type instead of `JObject`, install the [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) package.
-->

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>Exemplo

Consulte o exemplo de específicas de idiomas para um acionador de grelha de evento:

* [C#](#c-example)
* [Script do c# (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

Para obter um exemplo de Acionador HTTP, consulte [como utilizar o acionador HTTP](#use-an-http-trigger-as-an-event-grid-trigger) posteriormente neste artigo.

### <a name="c-example"></a>Exemplo do c#

O seguinte exemplo mostra um [c# função](functions-dotnet-class-library.md) que está vinculado a `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, TraceWriter log)
        {
            log.Info(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

<!--
The following example shows a [C# function](functions-dotnet-class-library.md) that binds to `EventGridEvent`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
            public static void EventGridTest([EventGridTrigger] Microsoft.Azure.EventGrid.Models.EventGridEvent eventGridEvent, TraceWriter log)
        {
            log.Info("C# Event Grid function processed a request.");
            log.Info($"Subject: {eventGridEvent.Subject}");
            log.Info($"Time: {eventGridEvent.EventTime}");
            log.Info($"Data: {eventGridEvent.Data.ToString()}");
        }
    }
}
```
-->

Para obter mais informações, consulte [pacotes](#packages), [atributos](#attributes), [configuração](#configuration), e [utilização](#usage).

### <a name="c-script-example"></a>Exemplo de script do c#

O exemplo seguinte mostra um enlace de Acionador num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Eis o script código c# que está vinculado a `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

<!--
Here's C# script code that binds to `EventGridEvent`:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.Azure.WebJobs.Extensions.EventGrid"
#r "Microsoft.Azure.EventGrid"

using Microsoft.Azure.WebJobs.Extensions.EventGrid;
Using Microsoft.Azure.EventGrid.Models;

public static void Run(EventGridEvent eventGridEvent, TraceWriter log)
{
    log.Info("C# Event Grid function processed a request.");
    log.Info($"Subject: {eventGridEvent.Subject}");
    log.Info($"Time: {eventGridEvent.EventTime}");
    log.Info($"Data: {eventGridEvent.Data.ToString()}");
}
```
-->

Para obter mais informações, consulte [pacotes](#packages), [atributos](#attributes), [configuração](#configuration), e [utilização](#usage).

### <a name="javascript-example"></a>Exemplo de JavaScript

O exemplo seguinte mostra um enlace de Acionador num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace.

Segue-se os dados do enlace *function.json* ficheiro:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Eis o código JavaScript:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```
     
## <a name="attributes"></a>Atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/EventGridTriggerAttribute.cs) atributo.

Eis um `EventGridTrigger` atributo na assinatura do método:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, TraceWriter log)
{
    ...
}
 ```

Para obter um exemplo completado, consulte [c# exemplo](#c-example).

## <a name="configuration"></a>Configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiro. Não são os parâmetros do construtor ou propriedades a definir `EventGridTrigger` atributo.

|propriedade de Function.JSON |Descrição|
|---------|---------|----------------------|
| **type** | Necessário - tem de ser definido como `eventGridTrigger`. |
| **direction** | Necessário - tem de ser definido como `in`. |
| **name** | Necessário - o nome da variável utilizado no código de função para o parâmetro que recebe os dados do evento. |

## <a name="usage"></a>Utilização

Para c# e as funções F #, pode utilizar os seguintes tipos de parâmetro para o acionador de grelha de evento:

* `JObject`
* `string`
* `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`-Define as propriedades para os campos comuns a todos os tipos de eventos. **Este tipo é preterido**, mas a sua substituição não está publicada NuGet ainda.

Para funções de JavaScript, o parâmetro com o nome de *function.json* `name` propriedade tem uma referência ao objeto de eventos.

## <a name="event-schema"></a>Esquema de eventos

Como um objeto JSON no corpo do pedido de HTTP recebidos dados de um evento de grelha de eventos. O JSON semelhante ao seguinte exemplo:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

O exemplo apresentado é uma matriz de um elemento. Grelha de evento sempre envia uma matriz e pode enviar mais do que um evento na matriz. O tempo de execução invoca a função, uma vez para cada elemento de matriz.

As propriedades de nível superior no evento dados JSON são os mesmos entre todos os tipos de eventos, enquanto o conteúdo a `data` propriedade são específicas para cada tipo de evento. O exemplo apresentado é um evento de armazenamento de Blobs.

Para uma explicação das propriedades do evento específico e comuns, consulte [as propriedades de evento](../event-grid/event-schema.md#event-properties) na documentação do evento grelha.

O `EventGridEvent` tipo define apenas as propriedades de nível superior; o `Data` propriedade é um `JObject`. 

## <a name="create-a-subscription"></a>Criar uma subscrição

Para começar a receber pedidos de HTTP da grelha de evento, crie uma subscrição de evento grelha que especifica o URL de ponto final que invoca a função.

### <a name="azure-portal"></a>Portal do Azure

Para as funções que desenvolver no portal do Azure com o acionador de grelha de evento, selecione **subscrição de adicionar o evento grelha**.

![Criar subscrição no portal](media/functions-bindings-event-grid/portal-sub-create.png)

Quando seleciona esta ligação, o portal abre o **criar subscrição de evento** prefilled de página com o URL de ponto final.

![URL de ponto final prefilled](media/functions-bindings-event-grid/endpoint-url.png)

Para obter mais informações sobre como criar subscrições através do portal do Azure, consulte [evento personalizado do criar - portal do Azure](../event-grid/custom-event-quickstart-portal.md) na documentação do evento grelha.

### <a name="azure-cli"></a>CLI do Azure

Para criar uma subscrição utilizando [a CLI do Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), utilize o [criar subscrição de evento do eventgrid az](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create) comando.

O comando exige o URL de ponto final que invoca a função. O exemplo seguinte mostra o padrão de URL:

```
https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}
```

A chave de sistema é uma chave de autorização que tem de ser incluído no URL do ponto final de um acionador de grelha de eventos. A secção seguinte explica como obter a chave de sistema.

Eis um exemplo que subscreve para uma conta de armazenamento de BLOBs (com um marcador de posição para a chave do sistema):

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name glengablobstorage --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://glengastorageevents.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=LUwlnhIsNtSiUjv/sNtSiUjvsNtSiUjvsNtSiUjvYb7XDonDUr/RUg==
```

Para obter mais informações sobre como criar uma subscrição, consulte [o guia de introdução do armazenamento de BLOBs](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) ou os outros eventos grelha inícios rápidos de.

### <a name="get-the-system-key"></a>Obter a chave de sistema

Pode obter a chave de sistema utilizando a seguinte API (HTTP GET):

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={adminkey}
```

Este é um administrador de API, pelo que requer o [chave admin](functions-bindings-http-webhook.md#authorization-keys). Não confunda a chave de sistema (para invocar uma função de Acionador de grelha de evento) com a chave de administrador (para efetuar tarefas administrativas na aplicação de função). Ao subscrever um tópico da grelha de evento, lembre-se de que utiliza a chave de sistema.

Eis um exemplo da resposta que fornece a chave de sistema:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

Para obter mais informações, consulte [chaves de autorização](functions-bindings-http-webhook.md#authorization-keys) no artigo de referência de Acionador HTTP. 

Em alternativa, pode enviar um HTTP PUT para especificar o valor da chave por si.

## <a name="local-testing-with-requestbin"></a>Testar com RequestBin local

Para testar um acionador de grelha de evento localmente, terá de obter os pedidos de HTTP da grelha de evento entregues da respetiva origem na nuvem no seu computador local. Uma forma de fazê-lo é ao capturar pedidos online e manualmente reenviá-los no seu computador local:

2. [Criar um ponto final RequestBin](#create-a-RequestBin-endpoint).
3. [Criar uma subscrição de evento grelha](#create-an-event-grid-subscription) que envia eventos para o ponto final RequestBin.
4. [Gerar um pedido](#generate-a-request) e copie o corpo do pedido a partir do RequestBin site.
5. [Publique manualmente o pedido](#manually-post-the-request) para o URL de localhost da grelha evento acionar a função.

Quando tiver terminado de teste, pode utilizar a mesma subscrição para produção, atualizando o ponto final. Utilize o [atualização de subscrição de evento az eventgrid](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update) comando da CLI do Azure.

### <a name="create-a-requestbin-endpoint"></a>Criar um ponto final RequestBin

RequestBin é uma ferramenta open-source que aceita os pedidos HTTP e mostra-lhe o corpo do pedido. O http://requestb.in URL obtém um tratamento especial através da grelha de eventos do Azure. Para facilitar a testar, grelha de eventos envia eventos para o URL de RequestBin sem necessidade de uma resposta a pedidos de validação de subscrição correta. Duas outras ferramentas de testes recebem o tratamento mesmo: http://webhookinbox.com e http://hookbin.com.

RequestBin não se destina a utilização de débito elevado. Se emitir mais do que um evento simultaneamente, não poderá ver todos os eventos na ferramenta.

Crie um ponto final.

![Criar o ponto final de RequestBin](media/functions-bindings-event-grid/create-requestbin.png)

Copie o URL de ponto final.

![Ponto final de RequestBin de cópia](media/functions-bindings-event-grid/save-requestbin-url.png)

### <a name="create-an-event-grid-subscription"></a>Criar uma subscrição de evento grelha

Criar uma subscrição de grelha de eventos do tipo que pretende testar e atribua o ponto final RequestBin. Para obter informações sobre como criar uma subscrição, consulte [criar uma subscrição](#create-a-subscription) anteriormente neste artigo.

### <a name="generate-a-request"></a>Gerar um pedido

Acione um evento que irá gerar tráfego HTTP para o ponto final RequestBin.  Por exemplo, se tiver criado uma subscrição de armazenamento de BLOBs, carregar ou eliminar um blob. Quando um pedido aparece na sua página de RequestBin, copie o corpo do pedido.

O pedido de validação da subscrição será recebido pela primeira vez; ignorar quaisquer pedidos de validação e copie o pedido de eventos.

![Copiar o corpo do pedido de RequestBin](media/functions-bindings-event-grid/copy-request-body.png)

### <a name="manually-post-the-request"></a>Publique manualmente o pedido

Execute localmente a sua função de grelha de eventos.

Utilizar uma ferramenta como [Postman](https://www.getpostman.com/) ou [curl](https://curl.haxx.se/docs/httpscripting.html) para criar um pedido de HTTP POST:

* Definir um `Content-Type: application/json` cabeçalho.
* Definir um `aeg-event-type: Notification` cabeçalho.
* Cole os dados de RequestBin no corpo do pedido. 
* Publicar o URL da sua função de Acionador de grelha de eventos, utilizando o padrão do seguinte:

```
http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

O `functionName` parâmetro tem de ser o nome especificado no `FunctionName` atributo.

As capturas de ecrã seguintes mostram os cabeçalhos e corpo no Postman do pedido:

![Cabeçalhos de Postman](media/functions-bindings-event-grid/postman2.png)

![Corpo do pedido em Postman](media/functions-bindings-event-grid/postman.png)

A função de Acionador de grelha de evento executa e mostra os registos semelhante ao seguinte exemplo:

![Registos de funções de Acionador de grelha de eventos de exemplo](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Testar com ngrok local

Outra forma de testar localmente um acionador de grelha de eventos é para automatizar a ligação de HTTP entre a Internet e o computador de desenvolvimento. Pode fazê-lo com uma ferramenta open-source denominada [ngrok](https://ngrok.com/):

3. [Criar um ponto final ngrok](#create-an-ngrok-endpoint).
4. [Executar a função de Acionador de grelha de evento](#run-the-event-grid-trigger-function).
5. [Criar uma subscrição de evento grelha](#create-a-subscription) que envia eventos para o ponto final ngrok.
6. [Acionar um evento](#trigger-an-event).

Quando tiver terminado de teste, pode utilizar a mesma subscrição para produção, atualizando o ponto final. Utilize o [atualização de subscrição de evento az eventgrid](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update) comando da CLI do Azure.

### <a name="create-an-ngrok-endpoint"></a>Criar um ponto final ngrok

Transferir *ngrok.exe* de [ngrok](https://ngrok.com/)e execute o seguinte comando:

```
ngrok http -host-header=localhost 7071
```

-Anfitrião-parâmetro de cabeçalho é necessário porque o tempo de execução de funções espera pedidos de localhost quando é executada no localhost. 7071 é o número de porta predefinido quando o tempo de execução é executada localmente.

O comando cria o resultado como o seguinte:

```
Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://263db807.ngrok.io -> localhost:7071
Forwarding                    https://263db807.ngrok.io -> localhost:7071

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

Irá utilizar o URL de https://{subdomain}.ngrok.io para a sua subscrição de evento grelha.

### <a name="run-the-event-grid-trigger-function"></a>Executar a função de Acionador de grelha de eventos

O URL de ngrok não obter tratamento especial através da grelha de eventos, pelo que a sua função tem de ser executado localmente quando é criada a subscrição. Se não for, a resposta de validação não obter enviada e a criação de subscrição falhar.

### <a name="create-a-subscription"></a>Criar uma subscrição

Criar uma subscrição de grelha de eventos do tipo que pretende testar e atribua o ponto final de ngrok, utilizando o padrão do seguinte:

```
https://{subdomain}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

O `functionName` parâmetro tem de ser o nome especificado no `FunctionName` atributo.

Eis um exemplo utilizando a CLI do Azure:

```
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName=EventGridTrigger
```

Para obter informações sobre como criar uma subscrição, consulte [criar uma subscrição](#create-a-subscription) anteriormente neste artigo.

### <a name="trigger-an-event"></a>Acionar um evento

Acione um evento que irá gerar tráfego HTTP para o ponto final ngrok.  Por exemplo, se tiver criado uma subscrição de armazenamento de BLOBs, carregar ou eliminar um blob.

A função de Acionador de grelha de evento executa e mostra os registos semelhante ao seguinte exemplo:

![Registos de funções de Acionador de grelha de eventos de exemplo](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>Utilizar um acionador HTTP como um acionador de grelha de eventos

Eventos de grelha de eventos que são recebidos como pedidos HTTP, pelo que pode processar eventos utilizando um acionador HTTP em vez de um acionador de grelha de eventos. Uma razão possível para o fazer é obter mais controlo sobre o URL de ponto final que invoca a função. 

Se utilizar um acionador HTTP, terá de escrever código para que o acionador de grelha de evento faz automaticamente:

* Envia uma resposta de validação para um [pedido de validação de subscrição](../event-grid/security-authentication.md#webhook-event-delivery).
* Invoca a função, uma vez por elemento da matriz de evento contida no corpo do pedido.

O seguinte exemplo código c# para um acionador HTTP simula o comportamento de Acionador de grelha de evento:

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"], 
        "Microsoft.EventGrid.SubscriptionValidationEvent", 
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.Info("Validate request received");
        return req.CreateResponse<object>(new
        {
            validationResponse = messages[0]["data"]["validationCode"]
        });
    }

    // The request is not for subscription validation, so it's for one or more events.
    foreach (JObject message in messages)
    {
        // Handle one event.
        EventGridEvent eventGridEvent = message.ToObject<EventGridEvent>();
        log.Info($"Subject: {eventGridEvent.Subject}");
        log.Info($"Time: {eventGridEvent.EventTime}");
        log.Info($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

O seguinte código de exemplo JavaScript para um acionador HTTP simula o comportamento de Acionador de grelha de evento:

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        context.res = { status: 200, body: messages[0].data.validationCode }
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        for (var i = 0; i < messages.length; i++) {
            // Handle one event.
            var message = messages[i];
            context.log('Subject: ' + message.subject);
            context.log('Time: ' + message.eventTime);
            context.log('Data: ' + JSON.stringify(message.data));
        }
    }
    context.done();
};
```

O código de processamento de eventos fica no interior do ciclo através de `messages` matriz.

Para obter informações sobre o URL a utilizar para invocar a função localmente ou quando for executada no Azure, consulte o [documentação de referência de enlace de Acionador HTTP](functions-bindings-http-webhook.md) 

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Saiba mais sobre a grelha de eventos](../event-grid/overview.md)
