---
title: Referência do desenvolvedor de JavaScript para funções do Azure | Documentos da Microsoft
description: Aprenda a desenvolver as funções com JavaScript.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: funções do azure, funções, processamento de eventos, webhooks, computação dinâmica, arquitetura sem servidor
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: azure-functions
ms.devlang: nodejs
ms.topic: reference
ms.date: 03/04/2018
ms.author: glenga
ms.openlocfilehash: 72b93de029af750f55bf53fcc82e22ad91b45f69
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296350"
---
# <a name="azure-functions-javascript-developer-guide"></a>Guia do Programador de JavaScript de funções do Azure
Este guia contém informações sobre as complicações de escrever as funções do Azure com o JavaScript.

Uma função de JavaScript é um exportado `function` que será executado quando acionado ([acionadores são configurados na Function](functions-triggers-bindings.md)). Cada função é passada uma `context` objeto que é utilizado para envio e recebimento vinculação de dados, o registo e a comunicação com o tempo de execução.

Este artigo pressupõe que já leu a [referência para programadores do funções do Azure](functions-reference.md). Também é recomendável que seguiu um tutorial em "Inícios rápidos" para [criar a primeira função](functions-create-first-function-vs-code.md).

## <a name="folder-structure"></a>estrutura de pastas

A estrutura de pasta necessária para um projeto de JavaScript é semelhante ao seguinte. Tenha em atenção que este padrão pode ser alterado: consulte os [scriptFile](functions-reference-node.md#using-scriptfile) secção abaixo para obter mais detalhes.

```
FunctionsProject
 | - MyFirstFunction
 | | - index.js
 | | - function.json
 | - MySecondFunction
 | | - index.js
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.js
 | | - mySecondHelperFunction.js
 | - node_modules
 | - host.json
 | - package.json
 | - extensions.csproj
 | - bin
```

Na raiz do projeto, há um partilhada [Host. JSON](functions-host-json.md) ficheiro que pode ser utilizado para configurar a aplicação de função. Cada função tem uma pasta com o seu próprio arquivo de código (. js) e o ficheiro de configuração de vinculação (Function).

As extensões de vinculação necessárias [versão 2.x](functions-versions.md) runtime das funções definidas no `extensions.csproj` arquivo, com os ficheiros de biblioteca real no `bin` pasta. Ao desenvolver localmente, deve [registar as extensões de vinculação](functions-triggers-bindings.md#local-development-azure-functions-core-tools). Ao desenvolver funções no portal do Azure, este registo é feito para.

## <a name="exporting-a-function"></a>Exportar uma função

As funções JavaScript tem de ser exportadas [ `module.exports` ](https://nodejs.org/api/modules.html#modules_module_exports) (ou [ `exports` ](https://nodejs.org/api/modules.html#modules_exports)). Na situação padrão, a função exportada deve ser a exportação única do seu arquivo, a exportação com o nome `run`, ou a exportação com o nome `index`. A localização predefinida da sua função é `index.js`, onde `index.js` compartilha o mesmo diretório do pai correspondente `function.json`. Tenha em atenção que o nome do `function.json`do diretório principal é sempre o nome da sua função. 

Para configurar a localização do ficheiro e exportar o nome da sua função, leia sobre [configurar o ponto de entrada da sua função](functions-reference-node.md#configure-function-entry-point) abaixo.

O ponto de entrada da função exportada deverá tomar um `context` objeto como o primeiro parâmetro.

```javascript
// You must include a context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
// You can also use 'arguments' to dynamically handle inputs
module.exports = function(context) {
    context.log('Number of inputs: ' + arguments.length);
    // Iterates through trigger and input binding data
    for (i = 1; i < arguments.length; i++){
        context.log(arguments[i]);
    }
    context.done();
};
```

Acionadores e enlaces de entrada (enlaces de `direction === "in"`) pode ser transmitido para a função como parâmetros. Elas são passadas para a função na mesma ordem em que elas estão definidas na *Function*. Pode lidar com entradas usando o JavaScript também dinamicamente [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) objeto. Por exemplo, se tiver `function(context, a, b)` e altere-o para `function(context, a)`, ainda pode obter o valor de `b` no código de função fazendo referência a `arguments[2]`.

Todos os enlaces, independentemente da direção, também serão transmitidos ao longo da `context` objeto usando o `context.bindings` propriedade.

## <a name="context-object"></a>objeto de contexto
O tempo de execução usa um `context` objeto para passar dados de e para sua função e permitir-lhe comunicar com o tempo de execução.

O `context` objeto é sempre o primeiro parâmetro para uma função e tem de ser incluído porque tem métodos como `context.done` e `context.log`, que são necessários para usar corretamente o tempo de execução. Pode nomear o objeto que quer que gostaria de ter (por exemplo, `ctx` ou `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(ctx) {
    // function logic goes here :)
    ctx.done();
};
```

### <a name="contextbindings-property"></a>propriedade de Context.Bindings

```
context.bindings
```
Devolve um objeto nomeado que contém todos os seus dados de entrada e saídos. Por exemplo, as seguintes definições de ligação no seu *Function* permite que acesse o conteúdo de uma fila de `context.bindings.myInput` e atribua saídas para uma fila usando `context.bindings.myOutput`.

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
},
{
    "type":"queue",
    "direction":"out",
    "name":"myOutput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

Tenha em atenção que pode optar por definir através de dados de enlace de saída a `context.done` método em vez do `context.binding` objeto (ver abaixo).

### <a name="contextbindingdata-property"></a>propriedade de context.bindingData

```
context.bindingData
```
Devolve um objeto nomeado que contém dados de invocação de metadados e a função de Acionador (`invocationId`, `sys.methodName`, `sys.utcNow`, `sys.randGuid`). Para obter um exemplo de metadados de Acionador, ver isso [exemplo de hubs de eventos](functions-bindings-event-hubs.md#trigger---javascript-example).

### <a name="contextdone-method"></a>método Context.Done
```
context.done([err],[propertyBag])
```

Informa o tempo de execução que terminou a seu código. Se a sua função usa o JavaScript [ `async function` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) declaração (disponível através do nó 8 + na versão de funções 2.x), não é necessário utilizar `context.done()`. O `context.done` implicitamente é chamado de retorno de chamada.

Se a sua função não é uma função de async **tem de chamar** `context.done` para informar o tempo de execução que sua função está concluída. A execução será o tempo limite, se estiver em falta.

O `context.done` método permite que passe tanto um erro definidas pelo usuário de volta para o tempo de execução e um objeto JSON que contém dados de enlace de saída. Propriedades transmitidos para `context.done` irá substituir qualquer item definido no `context.bindings` objeto.

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>método Context.log  

```
context.log(message)
```
Pode escrever nos registos de função de transmissão em fluxo no nível de rastreio predefinido. No `context.log`, métodos de registo adicionais estão disponíveis, que permitem que escreva registos de função em outros níveis de rastreio:


| Método                 | Descrição                                |
| ---------------------- | ------------------------------------------ |
| **error(_message_)**   | Grava em nível de erro, registo ou inferior.   |
| **warn(_message_)**    | Grava em nível de aviso de registo ou inferior. |
| **info(_message_)**    | Grava em nível de informações registo ou inferior.    |
| **verbose(_message_)** | Escreve no registo de nível verboso.           |

O exemplo a seguir registra um log no nível de rastreio de aviso:

```javascript
context.log.warn("Something has happened."); 
```
Pode [configurar o limiar de nível de rastreio para o registo de](#configure-the-trace-level-for-console-logging) no ficheiro de Host. JSON. Para obter mais informações sobre como escrever os registos, consulte [escrever saídas de rastreio](#writing-trace-output-to-the-console) abaixo.

Leia [monitorizar as funções do Azure](functions-monitoring.md) para saber mais sobre ver e consultar os registos da função.

## <a name="binding-data-type"></a>Tipo de dados de ligação

Para definir o tipo de dados para um enlace de entrada, utilize o `dataType` propriedade na definição de ligação. Por exemplo, para ler o conteúdo de uma solicitação HTTP no formato binário, utilize o tipo `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Opções de `dataType` são: `binary`, `stream`, e `string`.

## <a name="writing-trace-output-to-the-console"></a>Escrever os resultados de rastreio na consola 

As funções, vai utilizar o `context.log` métodos para escrever os resultados de rastreio no console. No v2.x de funções, rastrear ouputs via `console.log` são capturados ao nível da aplicação de função. Isso significa que produz de `console.log` não estão associadas a uma invocação de função específica e, por conseguinte, não são apresentados nos registos de uma função específica. Eles, no entanto, se propagará para o Application Insights. Não pode utilizar as funções v1.x, `console.log` para escrever no console. 

Quando chama `context.log()`, a mensagem é escrita para a consola no nível de rastreio predefinido, que é o _informações_ rastrear nível. Escreve o código a seguir para a consola no nível de rastreio de informações:

```javascript
context.log({hello: 'world'});  
```

Esse código é equivalente para o código acima:

```javascript
context.log.info({hello: 'world'});  
```

Esse código grava no console no nível de erro:

```javascript
context.log.error("An error has occurred.");  
```

Uma vez _erro_ se o rastreio de mais alto nível, este rastreio é escrito para a saída em todos os níveis de rastreio, desde que o registo está ativado.

Todos os `context.log` métodos suportam o mesmo formato de parâmetro que é suportado pelo node. js [util.format método](https://nodejs.org/api/util.html#util_util_format_format). Considere o código a seguir, que escreve os registos da função utilizando o nível de rastreio predefinido:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Também pode escrever o mesmo código no seguinte formato:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Configurar o nível de rastreio para o registo da consola

As funções permitem-lhe definir o nível de rastreio de limiar para escrever na consola, o que torna mais fácil para controlar os rastreios de forma são escritos na consola das suas funções. Para definir o limiar para todos os rastreios escritos na consola, utilize o `tracing.consoleLevel` propriedade no arquivo de Host. JSON. Esta definição aplica-se a todas as funções na sua aplicação de função. O exemplo seguinte define o limiar de rastreio para ativar o registo verboso:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Os valores da **consoleLevel** correspondem aos nomes do `context.log` métodos. Para desativar todos os registos de rastreio para a consola, defina **consoleLevel** ao _desativar_. Para obter mais informações, consulte [referência de Host. JSON](functions-host-json.md).

## <a name="http-triggers-and-bindings"></a>HTTP acionadores e enlaces

HTTP e acionadores de webhook e HTTP de saída ligações usam os objetos request e response para representar a mensagem de HTTP.  

### <a name="request-object"></a>Objeto de solicitação

O `context.req` (pedido) objeto tem as seguintes propriedades:

| Propriedade      | Descrição                                                    |
| ------------- | -------------------------------------------------------------- |
| _Corpo_        | Um objeto que contém o corpo do pedido.               |
| _Cabeçalhos_     | Um objeto que contém os cabeçalhos de pedido.                   |
| _Método_      | O método HTTP do pedido.                                |
| _originalUrl_ | O URL do pedido.                                        |
| _params_      | Um objeto que contém os parâmetros de encaminhamento do pedido. |
| _query_       | Um objeto que contém os parâmetros de consulta.                  |
| _rawBody_     | O corpo da mensagem como uma cadeia de caracteres.                           |


### <a name="response-object"></a>Objeto de resposta

O `context.res` objeto (resposta) tem as seguintes propriedades:

| Propriedade  | Descrição                                               |
| --------- | --------------------------------------------------------- |
| _Corpo_    | Um objeto que contém o corpo da resposta.         |
| _Cabeçalhos_ | Um objeto que contém os cabeçalhos de resposta.             |
| _isRaw_   | Indica que a formatação é ignorada para a resposta.    |
| _status_  | O código de estado HTTP da resposta.                     |

### <a name="accessing-the-request-and-response"></a>Aceder a solicitação e resposta 

Ao trabalhar com acionadores HTTP, pode acessar os objetos de solicitação e resposta HTTP de diversas formas:

+ Partir `req` e `res` das propriedades no `context` objeto. Dessa forma, pode usar o padrão convencional para dados de acesso HTTP do objeto de contexto, em vez de precisar usar toda a `context.bindings.name` padrão. O exemplo seguinte mostra como aceder a `req` e `res` objetos no `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Na entrada nomeada e enlaces de saída. Dessa forma, o acionador HTTP e os enlaces funcionam da mesma forma como qualquer outra ligação. O exemplo seguinte define o objeto de resposta utilizando uma determinada `response` enlace: 

    ```json
    {
        "type": "http",
        "direction": "out",
        "name": "response"
    }
    ```
    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```
+ _[Apenas de resposta]_  Chamando `context.res.send(body?: any)`. Uma resposta HTTP é criada com entrada `body` como o corpo da resposta. `context.done()` é chamado implicitamente.

+ _[Apenas de resposta]_  Chamando `context.done()`. Um tipo especial de enlace HTTP retorna a resposta que é passada para o `context.done()` método. Enlace de saída HTTP seguinte define uma `$return` parâmetro de saída:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version"></a>Versão do nó

A tabela seguinte mostra a versão do node. js utilizada por cada versão principal do runtime das funções:

| Versão de funções | Versão Node.js | 
|---|---|
| 1.x | 6.11.2 (bloqueados pelo tempo de execução) |
| 2.x  | _Active Directory LTS_ e _atual_ versões de node. js (8.11.1 e 10.6.0 recomendado). Definir a versão com o WEBSITE_NODE_DEFAULT_VERSION [definição de aplicação](functions-how-to-use-azure-function-app-settings.md#settings).|

Pode ver a versão atual que está a utilizar o tempo de execução ao verificar a definição de aplicação acima ou imprimindo `process.version` de qualquer função.

## <a name="dependency-management"></a>Gestão de dependências
Para poder utilizar as bibliotecas de Comunidade no seu código JavaScript, como mostrado no exemplo abaixo, precisa garantir que todas as dependências estão instaladas na sua aplicação de função no Azure.

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

Tenha em atenção que deve definir um `package.json` ficheiro na raiz da sua aplicação de funções. Definir o ficheiro permite que todas as funções na aplicação, partilhar os mesmos pacotes em cache, que proporciona o melhor desempenho. Se surgir um conflito de versão, pode resolvê-lo ao adicionar um `package.json` ficheiro na pasta de uma função específica.  

Existem duas formas de instalar os pacotes em sua aplicação de funções: 

### <a name="deploying-with-dependencies"></a>Implementar com dependências
1. Instalar todos os pacotes necessários localmente ao executar `npm install`.

2. Implementar o seu código e certifique-se de que o `node_modules` pasta está incluída na implementação. 


### <a name="using-kudu"></a>Utilizar o Kudu
1. Aceda a `https://<function_app_name>.scm.azurewebsites.net`.

2. Clique em **consola de depuração** > **CMD**.

3. Aceda a `D:\home\site\wwwroot`e, em seguida, arraste o ficheiro Package. JSON para o **wwwroot** pasta na metade superior da página.  
    Também pode carregar ficheiros para a sua aplicação de função de outras formas. Para obter mais informações, consulte [como atualizar os ficheiros de aplicação de função](functions-reference.md#fileupdate). 

4. Após o carregamento do ficheiro Package JSON, execute o `npm install` comando no **consola de execução remota de Kudu**.  
    Esta ação transfere os pacotes indicados no ficheiro Package. JSON e reinicia a aplicação de funções.

## <a name="environment-variables"></a>Variáveis de ambiente
Para obter uma variável de ambiente ou uma aplicação de definir o valor, utilize `process.env`, conforme mostrado aqui no `GetEnvironmentVariable` função:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));

    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="configure-function-entry-point"></a>Configurar o ponto de entrada da função

O `function.json` propriedades `scriptFile` e `entryPoint` pode ser utilizado para configurar a localização e o nome da sua função exportada. Estes podem ser importantes se o JavaScript é transpiled.

### <a name="using-scriptfile"></a>Utilizar o `scriptFile`

Por predefinição, uma função de JavaScript é executada a partir `index.js`, um ficheiro que partilha o mesmo diretório principal de seu correspondente `function.json`.

`scriptFile` pode ser usado para obter uma estrutura de pastas é semelhante a este:
```
FunctionApp
 | - host.json
 | - myNodeFunction
 | | - function.json
 | - lib
 | | - nodeFunction.js
 | - node_modules
 | | - ... packages ...
 | - package.json
```

O `function.json` para `myNodeFunction` deve incluir um `scriptFile` propriedade apontando para o arquivo com a função exportada para executar.
```json
{
  "scriptFile": "../lib/nodeFunction.js",
  "bindings": [
    ...
  ]
}
```

### <a name="using-entrypoint"></a>Utilizar o `entryPoint`

Na `scriptFile` (ou `index.js`), uma função tem de ser exportada usando `module.exports` para ser encontrado e executar. Por predefinição, a função que é executado quando acionada está a exportação apenas a partir desse arquivo, a exportação com o nome `run`, ou a exportação com o nome `index`.

Isso pode ser configurado usando `entryPoint` em `function.json`:
```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

V2.x de funções, que suporta o `this` parâmetro nas funções de utilizador, o código de função, em seguida, pode ser o seguinte:
```javascript
class MyObj {
    constructor() {
        this.foo = 1;
    };
    
    function logFoo(context) { 
        context.log("Foo is " + this.foo); 
        context.done(); 
    }
}

const myObj = new MyObj();
module.exports = myObj;
```

Neste exemplo, é importante observar que embora um objeto está a ser exportado, não há nenhum garante em torno de preservação de estado entre execuções.

## <a name="considerations-for-javascript-functions"></a>Considerações para funções de JavaScript

Ao trabalhar com as funções de JavaScript, lembre-se de que as considerações nas seções a seguir.

### <a name="choose-single-vcpu-app-service-plans"></a>Escolha único vCPU planos de serviço de aplicações

Quando cria uma aplicação de funções que utiliza o plano do serviço de aplicações, recomendamos que selecione um plano de vCPU de único, em vez de um plano com vários vCPUs. Hoje em dia, funções executa funções JavaScript com mais eficiência em VMs de vCPU de único e utilizar VMs maiores não produz os aprimoramentos de desempenho esperadas. Quando for necessário, manualmente pode aumentar horizontalmente ao adicionar mais instâncias VM de vCPU de único ou pode ativar o dimensionamento automático. Para obter mais informações, consulte [dimensionar a contagem de instâncias manual ou automaticamente](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>Suporte do typeScript e CoffeeScript
Uma vez que o suporte direto ainda não existe para compilar o automática TypeScript ou CoffeeScript via o tempo de execução, esse suporte deve ser tratado fora o tempo de execução, no momento da implementação. 

### <a name="cold-start"></a>Arranque a frio
Quando começar a desenvolver funções do Azure no modelo de alojamento sem servidor, cold são uma realidade. "A frio" refere-se ao fato de que quando a aplicação Function App é iniciado pela primeira vez após um período de inatividade, irá demorar mais tempo a iniciar cópia de segurança. Em particular, para as funções de JavaScript com árvores de dependência grandes isso pode causar lentidão principais. Para acelerar o processo, se possível, [executar as suas funções como um ficheiro de pacote](run-functions-from-deployment-package.md). Vários métodos de implementação escolher a esse modelo, por predefinição, mas se estiver a sofrer grandes arranques a frio e não estiver a executar um ficheiro de pacote, isso pode ser uma enorme melhoria.

## <a name="next-steps"></a>Passos Seguintes
Para obter mais informações, consulte os seguintes recursos:

* [Best Practices for Azure Functions (Melhores Práticas para as Funções do Azure)](functions-best-practices.md)
* [Referência para programadores das Funções do Azure](functions-reference.md)
* [Acionadores de funções do Azure e enlaces](functions-triggers-bindings.md)

