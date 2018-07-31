---
title: Enlaces de funções HTTP e webhook do Azure
description: Aprenda a utilizar HTTP e webhook acionadores e enlaces nas funções do Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: das funções do Azure, funções, eventos de processamento, webhooks, dinâmicos REST de computação, arquitetura sem servidor, HTTP, API,
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 5f6538c69139b8cd254b44cb9875e18a14c8fa8b
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344152"
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Enlaces de funções HTTP e webhook do Azure

Este artigo explica como trabalhar com associações HTTP nas funções do Azure. Acionadores HTTP suporta funções do Azure e enlaces de saída.

Um acionador HTTP pode ser personalizado para responder às [webhooks](https://en.wikipedia.org/wiki/Webhook). Um acionador de webhook aceita apenas um payload JSON e valida o JSON. Existem versões especiais do acionador de webhook que seja mais fácil lidar com webhooks de alguns fornecedores, como o GitHub e Slack.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="packages---functions-1x"></a>Pacotes - funções 1.x

Os enlaces de HTTP são fornecidos na [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) pacote NuGet, versão 1.x. Código-fonte para o pacote está no [azure-webjobs-sdk-extensões](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http) repositório do GitHub.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Pacotes - funções 2.x

Os enlaces de HTTP são fornecidos na [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) pacote NuGet, versão 3.x. Código-fonte para o pacote está no [azure-webjobs-sdk-extensões](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) repositório do GitHub.

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Acionador

O acionador HTTP permite-lhe invocar uma função com uma solicitação HTTP. Pode utilizar um acionador HTTP para criar APIs sem servidor e responder a webhooks. 

Por predefinição, um acionador HTTP devolve HTTP 200 OK com um corpo vazio nas funções 1.x ou HTTP 204 sem conteúdo com um corpo vazio nas funções 2.x. Para modificar a resposta, configure uma [enlace de saída HTTP](#http-output-binding).

## <a name="trigger---example"></a>Acionador - exemplo

Veja o exemplo de idioma específico:

* [C#](#trigger---c-example)
* [Script do c# (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Acionador - exemplo do c#

A exemplo a seguir mostra um [função c#](functions-dotnet-class-library.md) que procura um `name` parâmetro na cadeia de consulta ou o corpo da solicitação HTTP. Tenha em atenção que o valor de retorno é usado para o enlace de saída, mas um atributo de valor de retorno não é necessário.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequestMessage req, 
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

### <a name="trigger---c-script-example"></a>Acionador - exemplo de script do c#

O exemplo seguinte mostra uma ligação de Acionador num *Function* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função procura um `name` parâmetro na cadeia de consulta ou o corpo da solicitação HTTP.

Aqui está o *Function* ficheiro:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

O [configuração](#trigger---configuration) seção explica essas propriedades.

Aqui está o script código c# que liga `HttpRequestMessage`:

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

É possível ligar a um objeto personalizado em vez de `HttpRequestMessage`. Este objeto é criado no corpo do pedido, analisado como JSON. Da mesma forma, um tipo pode ser transmitido para a resposta HTTP de enlace de saída e retornado como o corpo da resposta, juntamente com um código de 200 estado.

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
```

### <a name="trigger---f-example"></a>Acionador - exemplo do F #

O exemplo seguinte mostra uma ligação de Acionador num *Function* ficheiro e uma [função F #](functions-reference-fsharp.md) que utiliza o enlace. A função procura um `name` parâmetro na cadeia de consulta ou o corpo da solicitação HTTP.

Aqui está o *Function* ficheiro:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O [configuração](#trigger---configuration) seção explica essas propriedades.

Eis o código F #:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

É necessário um `project.json` ficheiro que utiliza o NuGet para fazer referência a `FSharp.Interop.Dynamic` e `Dynamitey` assemblies, conforme mostrado no exemplo a seguir:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

### <a name="trigger---javascript-example"></a>Acionador - exemplo de JavaScript

O exemplo seguinte mostra uma ligação de Acionador num *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace. A função procura um `name` parâmetro na cadeia de consulta ou o corpo da solicitação HTTP.

Aqui está o *Function* ficheiro:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

O [configuração](#trigger---configuration) seção explica essas propriedades.

Eis o código JavaScript:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```
     
## <a name="trigger---webhook-example"></a>Acionador - exemplo de webhook

Veja o exemplo de idioma específico:

* [C#](#webhook---c-example)
* [Script do c# (.csx)](#webhook---c-script-example)
* [F#](#webhook---f-example)
* [JavaScript](#webhook---javascript-example)

### <a name="webhook---c-example"></a>Webhook - exemplo do c#

A exemplo a seguir mostra um [função c#](functions-dotnet-class-library.md) que envia um HTTP 200 em resposta a um pedido JSON genérica.

```cs
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

### <a name="webhook---c-script-example"></a>Webhook - exemplo de script do c#

O exemplo seguinte mostra um acionador de webhook ligando uma *Function* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função registos de comentários de problema do GitHub.

Aqui está o *Function* ficheiro:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

O [configuração](#trigger---configuration) seção explica essas propriedades.

Aqui está o código de script do c#:

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

### <a name="webhook---f-example"></a>Webhook - exemplo do F #

O exemplo seguinte mostra um acionador de webhook ligando uma *Function* ficheiro e uma [função F #](functions-reference-fsharp.md) que utiliza o enlace. A função registos de comentários de problema do GitHub.

Aqui está o *Function* ficheiro:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

O [configuração](#trigger---configuration) seção explica essas propriedades.

Eis o código F #:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

### <a name="webhook---javascript-example"></a>Webhook - JavaScript de exemplo

O exemplo seguinte mostra um acionador de webhook ligando um *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace. A função registos de comentários de problema do GitHub.

Eis a vinculação de dados a *Function* ficheiro:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

O [configuração](#trigger---configuration) seção explica essas propriedades.

Eis o código JavaScript:

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="trigger---attributes"></a>Acionador - atributos

Na [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) atributo.

A autorização pode definir métodos HTTP de nível e permitidos nos parâmetros do construtor de atributo e existem propriedades para o modelo de tipo e a rota do webhook. Para obter mais informações sobre estas definições, consulte [acionador - configuração](#trigger---configuration). Aqui está um `HttpTrigger` atributo numa assinatura do método:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    ...
}
 ```

Para obter um exemplo completo, consulte [acionador - exemplo do c#](#trigger---c-example).

## <a name="trigger---configuration"></a>Acionador - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro e o `HttpTrigger` atributo.

|propriedade de Function | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
| **tipo** | n/d| Necessário - tem de ser definido como `httpTrigger`. |
| **direção** | n/d| Necessário - tem de ser definido como `in`. |
| **name** | n/d| Necessário - o nome da variável no código de função para a pedido ou corpo do pedido. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Determina o chaves, se houver, precisam de estar presente no pedido para invocar a função. O nível de autorização pode ser um dos seguintes valores: <ul><li><code>anonymous</code>&mdash;Nenhuma chave de API é necessário.</li><li><code>function</code>&mdash;É necessária uma chave de API específicas. Este é o valor predefinido se não for fornecido nenhum.</li><li><code>admin</code>&mdash;A chave mestra é necessária.</li></ul> Para obter mais informações, consulte a secção [chaves de autorização](#authorization-keys). |
| **Métodos** |**Métodos** | Uma matriz dos métodos HTTP para o qual a função responde. Se não for especificado, a função responde a todos os métodos HTTP. Ver [personalizar o ponto de extremidade http](#customize-the-http-endpoint). |
| **rota** | **rota** | Define o modelo de rota, controlar a que URLs responde de sua função de pedido. O valor predefinido se não for fornecida nenhuma é `<functionname>`. Para obter mais informações, consulte [personalizar o ponto de extremidade http](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** |Configura o acionador HTTP para atuar como um [webhook](https://en.wikipedia.org/wiki/Webhook) recetor para o fornecedor especificado. Não definir o `methods` propriedade se definir esta propriedade. O tipo de webhook pode ser um dos seguintes valores:<ul><li><code>genericJson</code>&mdash;Um ponto final do webhook para fins gerais sem lógica para um provedor específico. Esta definição limita pedidos apenas às através de HTTP POST e com o `application/json` tipo de conteúdo.</li><li><code>github</code>&mdash;A função responde às [GitHub webhooks](https://developer.github.com/webhooks/). Não utilize o _authLevel_ propriedade com o GitHub webhooks. Para obter mais informações, consulte a secção de webhooks do GitHub neste artigo.</li><li><code>slack</code>&mdash;A função responde às [Slack webhooks](https://api.slack.com/outgoing-webhooks). Não utilize o _authLevel_ propriedade com Slack webhooks. Para obter mais informações, consulte a secção de Slack webhooks neste artigo.</li></ul>|

## <a name="trigger---usage"></a>Acionador - utilização

Para funções c# e F #, pode declarar o tipo de Acionador de entrada de ser um `HttpRequestMessage` ou um tipo personalizado. Se escolher `HttpRequestMessage`, obtém acesso total para o objeto de solicitação. Para um tipo personalizado, as funções tenta analisar o corpo do pedido JSON para definir as propriedades do objeto. 

Para funções de JavaScript, o runtime das funções fornece o corpo do pedido em vez do objeto de solicitação. Para obter mais informações, consulte a [exemplo de Acionador de JavaScript](#trigger---javascript-example).

### <a name="github-webhooks"></a>GitHub webhooks

Para responder a webhooks do GitHub, primeiro crie a sua função com um acionador HTTP e defina a **webHookType** propriedade `github`. Em seguida, copie a chave de API e o URL para o **adicionar webhook** página do seu repositório do GitHub. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Por exemplo, veja [Create a function triggered by a GitHub webhook (Criar uma função acionada por um webhook do GitHub)](functions-create-github-webhook-triggered-function.md).

### <a name="slack-webhooks"></a>Slack webhooks

O webhook Slack gera um token para, em vez de permitir que especifique, pelo que tem de configurar uma chave específica de função com o token de Slack. Ver [chaves de autorização](#authorization-keys).

### <a name="customize-the-http-endpoint"></a>Personalizar o ponto final HTTP

Por predefinição quando cria uma função de um acionador HTTP ou WebHook, a função é endereçável com uma rota do formulário:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Pode personalizar esta rota com o opcional `route` propriedade ao acionador de HTTP do enlace de entrada. Por exemplo, o seguinte procedimento *Function* arquivo define um `route` propriedade para um acionador HTTP:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Utilizando esta configuração, a função está agora acessível com a seguinte rota em vez da rota original.

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

Isso permite que o código de função oferecer suporte a dois parâmetros no endereço _categoria_ e _id_. Pode usar qualquer [restrição de rota da API Web](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) com os parâmetros. O seguinte código de função do c# faz uso de ambos os parâmetros.

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

Aqui está o código de função de node. js que utiliza os mesmos parâmetros de rota.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Por predefinição, todas as rotas de função têm o prefixo *api*. Também pode personalizar ou remover a prefixo com o `http.routePrefix` propriedade em seu [Host. JSON](functions-host-json.md) ficheiro. O exemplo seguinte remove o *api* prefixo de rota, utilizando uma cadeia vazia para o prefixo no *Host. JSON* ficheiro.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="authorization-keys"></a>Chaves de autorização

Acionadores HTTP permitem-lhe utilizar as chaves para maior segurança. Um acionador HTTP padrão pode utilizá-los como uma chave de API, que requerem a chave de estar presente no pedido. Webhooks pode utilizar as chaves para autorizar os pedidos de diversas formas, consoante o que o fornecedor suporta.

> [!NOTE]
> Ao executar as funções localmente, a autorização está desabilitada quer o `authLevel` definido no `function.json`. Assim que publicar no funções do Azure, o `authLevel` entra em vigor imediatamente.

As chaves são encriptadas em inatividade e são armazenadas como parte da sua aplicação de função no Azure. Para ver as suas chaves, criar novos, ou distribuir as chaves para novos valores, navegue para uma das suas funções no portal e selecione "Gerir". 

Existem dois tipos de chaves:

- **Das chaves de anfitrião**: estas chaves são partilhadas por todas as funções dentro da aplicação de função. Quando utilizado como uma chave de API, elas permitem o acesso a qualquer função dentro da aplicação de função.
- **Chaves de função**: estas chaves são aplicadas apenas a funções específicas, sob a qual são definidas. Quando utilizado como uma chave de API, estes apenas permitem o acesso a essa função.

Cada chave é chamado para referência e há uma chave de padrão (denominada "predefinido") ao nível da função e o anfitrião. Teclas de função têm precedência sobre as chaves de anfitrião. Quando duas chaves forem definidas com o mesmo nome, a tecla de função é sempre usada.

O **chave mestra** é uma chave de anfitrião predefinido com o nome "_master" que está definido para cada aplicação de função. Esta chave não pode ser revogada. Ele fornece acesso administrativo para o tempo de execução APIs. Usando `"authLevel": "admin"` na ligação exigida pela JSON esta chave a serem apresentados na solicitação; qualquer outra chave resulta numa falha de autorização.

> [!IMPORTANT]  
> Devido a permissões elevadas concedidas pela chave mestra, não deve partilhar esta chave com terceiros ou distribuí-la em aplicativos de cliente nativo. Tenha cuidado ao escolher o nível de autorização de administrador.

### <a name="api-key-authorization"></a>Autorização da chave de API

Por padrão, um acionador HTTP exige uma chave de API na solicitação HTTP. Portanto, o seu pedido HTTP normalmente é semelhante ao seguinte:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

A chave pode ser incluída numa variável de cadeia de caracteres de consulta nomeada `code`, como anteriormente, ou pode ser incluído num `x-functions-key` cabeçalho de HTTP. O valor da chave pode ser qualquer tecla de função definido para a função, ou qualquer chave de anfitrião.

Pode permitir pedidos anónimos, que não necessitam de chaves. Também pode exigir que a chave mestra de ser utilizada. Alterar o nível de autorização predefinido utilizando o `authLevel` propriedade no vínculo da JSON. Para obter mais informações, consulte [acionador - configuração](#trigger---configuration).

### <a name="keys-and-webhooks"></a>As chaves e webhooks

Autorização de Webhook é processada pelo componente de destinatário do webhook, parte do acionador HTTP, e o mecanismo varia consoante o tipo de webhook. Faz de cada mecanismo de, no entanto contam com uma chave. Por predefinição, é utilizada a tecla de função com o nome "predefinição". Para utilizar uma chave diferente, configure o fornecedor de webhook para enviar o nome da chave com o pedido de uma das seguintes formas:

- **Cadeia de consulta**: O fornecedor transmite o nome da chave no `clientid` consulta, como o parâmetro de cadeia de caracteres, `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **Cabeçalho do pedido**: O fornecedor transmite o nome da chave no `x-functions-clientid` cabeçalho.

## <a name="trigger---limits"></a>Acionador - limites

O comprimento do pedido HTTP está limitado a 100MB (104,857,600 bytes) e o comprimento de URL está limitado a 4KB (4,096 bytes). Estes limites são especificados pela `httpRuntime` elemento do tempo de execução [arquivo Web. config](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

Se uma função que usa o acionador HTTP não concluída no prazo de cerca de 2,5 minutos, o tempo limite do gateway irá e devolver um erro de HTTP 502. A função vai continuar em execução, mas será possível devolver uma resposta HTTP. Para as funções de execução longa, recomendamos que siga os padrões do async e devolver uma localização onde pode efetuar o ping do Estado do pedido. Para obter informações sobre o tempo que pode executar uma função, veja [plano de consumo de dimensionamento e alojamento -](functions-scale.md#consumption-plan). 

## <a name="trigger---hostjson-properties"></a>Acionador - propriedades de Host. JSON

O [Host. JSON](functions-host-json.md) ficheiro contém as definições que controlam o comportamento de Acionador HTTP.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Saída

Utilize a enlace de responder para o remetente de pedido HTTP de saída HTTP. Essa ligação requer um acionador HTTP e permite-lhe personalizar a resposta associada à solicitação do acionador. Se o enlace de saída de um HTTP não for fornecido, um acionador HTTP retorna HTTP 200 OK, com um corpo vazio nas funções 1.x ou HTTP 204 sem conteúdo com um corpo vazio nas funções 2.x.

## <a name="output---configuration"></a>Saída - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro. Para a classe c# há bibliotecas são sem propriedades de atributo que correspondam a estas *Function* propriedades. 

|Propriedade  |Descrição  |
|---------|---------|
| **tipo** |Tem de ser definido como `http`. |
| **direção** | Tem de ser definido como `out`. |
|**name** | O nome da variável no código de função para a resposta, ou `$return` para utilizar o valor de retorno. |

## <a name="output---usage"></a>Saída - utilização

Para enviar uma resposta HTTP, use os padrões de resposta de idioma padrão. Em c# ou de script c#, tornar a função de tipo de retorno `HttpResponseMessage` ou `Task<HttpResponseMessage>`. No c#, um atributo de valor de retorno não a é necessário.

Por exemplo respostas, consulte a [exemplo de Acionador](#trigger---example) e o [exemplo de webhook](#trigger---webhook-example).

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
