---
title: Enlaces de ficheiro externos para as funções do Azure (experimental)
description: Utilizar enlaces de ficheiro externo nas funções do Azure
services: functions
author: craigshoemaker
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: cshoe
ms.openlocfilehash: 39036f17b8ceafebbe3660f2074e2a6c84c4df03
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50248698"
---
# <a name="azure-functions-external-file-bindings-experimental"></a>Enlaces de ficheiro externo de funções do Azure (experimentais)
Este artigo mostra como manipular ficheiros a partir de diferentes fornecedores de SaaS (como o Dropbox ou Google Drive) nas funções do Azure. Funções do Azure suporta acionaram, de entrada e saída enlaces para arquivos externos. Essas associações criar ligações de API para fornecedores SaaS ou utilizam as ligações de API existentes do grupo de recursos da sua aplicação de função.

> [!IMPORTANT]
> Os enlaces de ficheiro externo estão experimentais e nunca atinja o estado de geralmente disponível (GA). Eles estão incluídos apenas no Azure funciona 1.x, e não existirem não existem planos para adicioná-los para as funções do Azure 2.x. Para cenários que exigem acesso a dados em fornecedores de SaaS, considere a utilização [aplicações lógicas que chamam funções](functions-twitter-email.md). Consulte a [conector de sistema de ficheiros de aplicações lógicas](../logic-apps/logic-apps-using-file-connector.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="available-file-connections"></a>Ligações de ficheiros disponíveis

|Conector|Acionador|Input|Saída|
|:-----|:---:|:---:|:---:|
|[Caixa](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive para Empresas](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google Drive](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Ligações externas do ficheiro também podem ser utilizadas no [do Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="trigger"></a>Acionador

O acionador de ficheiro externo permite-lhe monitorizar uma pasta remota e executar o código de função quando forem detetadas alterações.

## <a name="trigger---example"></a>Acionador - exemplo

Veja o exemplo de idioma específico:

* [Script c#](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-script-example"></a>Acionador - exemplo de script do c#

O exemplo seguinte mostra um acionador de ficheiro externo ligando uma *Function* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função regista o conteúdo de cada ficheiro que é adicionado à pasta monitorizada.

Eis a vinculação de dados a *Function* ficheiro:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Aqui está o código de script do c#:

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

### <a name="trigger---javascript-example"></a>Acionador - exemplo de JavaScript

O exemplo seguinte mostra um acionador de ficheiro externo ligando uma *Function* ficheiro e um [função JavaScript](functions-reference-node.md) que utiliza o enlace. A função regista o conteúdo de cada ficheiro que é adicionado à pasta monitorizada.

Eis a vinculação de dados a *Function* ficheiro:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

Eis o código JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

## <a name="trigger---configuration"></a>Acionador - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro.

|propriedade de Function | Descrição|
|---------|---------|----------------------|
|**tipo** | Tem de ser definido como `apiHubFileTrigger`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure.|
|**direção** | Tem de ser definido como `in`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure. |
|**name** | O nome da variável que representa o item de evento no código de função. | 
|**ligação**| Identifica a definição de aplicação que armazena a cadeia de ligação. A definição de aplicação é criada automaticamente quando adiciona uma ligação de integrar da interface do Usuário, no portal do Azure.|
|**path** | A pasta para monitorizar e, opcionalmente, um padrão de nome.|

### <a name="name-patterns"></a>Padrões de nome

Pode especificar um padrão de nome de ficheiro no `path` propriedade. A pasta referenciada tem de existir no fornecedor SaaS.

Exemplos:

```json
"path": "input/original-{name}",
```

Este caminho seria encontrar um ficheiro denominado *original File1.txt* no *entrada* pasta e o valor da `name` variável no código de função seria `File1.txt`.

Outro exemplo:

```json
"path": "input/{filename}.{fileextension}",
```

Este caminho encontrará um arquivo chamado *original File1.txt*e o valor da `filename` e `fileextension` variáveis no código de função seria *original File1* e *txt* .

Pode restringir o tipo de ficheiro de ficheiros ao utilizar um valor fixo para a extensão de ficheiro. Por exemplo:

```json
"path": "samples/{name}.png",
```

Neste caso, apenas *. PNG* arquivos no *exemplos* pasta acione a função.

Chavetas são carateres especiais em padrões de nome. Para especificar os nomes de ficheiros que têm chaves no nome, faça duplo das chaves.
Por exemplo:

```json
"path": "images/{{20140101}}-{name}",
```

Este caminho seria encontrar um ficheiro denominado  *{20140101}-soundfile.mp3* no *imagens* pasta e a `name` valor da variável no código de função seria *soundfile.mp3*.

## <a name="trigger---usage"></a>Acionador - utilização

No c# das funções, ligar para os dados de ficheiro de entrada usando um parâmetro com nome em sua assinatura de função, como `<T> <name>`.
Em que `T` é o tipo de dados que pretende anular a serialização de dados, e `paramName` é o nome que especificou no [acionar JSON](#trigger). Funções de node. js, vai aceder os dados de ficheiro de entrada com `context.bindings.<name>`.

O ficheiro possa ser desserializado em qualquer um dos seguintes tipos:

* Qualquer [objeto](https://msdn.microsoft.com/library/system.object.aspx) - útil para dados de ficheiro JSON serializado.
  Se declarar um tipo de entrada personalizado (por exemplo, `FooType`), as funções do Azure tenta desserializar os dados JSON em seu tipo especificado.
* Cadeia de caracteres - útil para dados de ficheiro de texto.

Também pode ligar a qualquer um dos seguintes tipos de funções c#, e o runtime das funções tenta desserializar os dados de arquivo usando esse tipo:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

<!--- ## Trigger - file receipts
The Azure Functions runtime makes sure that no external file trigger function gets called more than once for the same new or updated file.
It does so by maintaining *file receipts* to determine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in the Azure storage account for your function app
(specified by the `AzureWebJobsStorage` app setting). A file receipt has the following information:

* The triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* The folder name
* The file type ("BlockFile" or "PageFile")
* The file name
* The ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

To force reprocessing of a file, delete the file receipt for that file from the *azure-webjobs-hosts* folder manually.
--->

## <a name="trigger---poison-files"></a>Acionador - ficheiros não processáveis

Quando ocorre uma falha numa função de Acionador de ficheiro externo, as funções do Azure repete essa função até 5 vezes por predefinição (incluindo a primeira tentativa) para um determinado ficheiro.
Se todas as tentativas de 5 falharem, as funções adiciona uma mensagem numa fila de armazenamento com o nome *webjobs-apihubtrigger-veneno*. A mensagem de fila de ficheiros não processáveis é um objeto JSON que contém as seguintes propriedades:

* FunctionId (no formato  *&lt;nome da aplicação >*. Funções.  *&lt;nome da função >*)
* fileType
* FolderName
* Nome de ficheiro
* ETag (um identificador de versão do ficheiro, por exemplo: "0x8D1DC6E70A277EF")

## <a name="input"></a>Input

O enlace de entrada de ficheiro externo do Azure permite-lhe utilizar um ficheiro a partir de uma pasta externa na sua função.

## <a name="input---example"></a>Entrada - exemplo

Veja o exemplo de idioma específico:

* [Script c#](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Introdução - exemplo de script do c#

O exemplo seguinte mostra o arquivo externo enlaces de entrada e saída numa *Function* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função copia um ficheiro de entrada para um ficheiro de saída.

Eis a vinculação de dados a *Function* ficheiro:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Aqui está o código de script do c#:

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

### <a name="input---javascript-example"></a>Introdução - exemplo de JavaScript

O exemplo seguinte mostra o arquivo externo enlaces de entrada e saída numa *Function* ficheiro e uma [função JavaScript](functions-reference-node.md) que utiliza o enlace. A função copia um ficheiro de entrada para um ficheiro de saída.

Eis a vinculação de dados a *Function* ficheiro:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Eis o código JavaScript:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="input---configuration"></a>Entrada - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro.

|propriedade de Function | Descrição|
|---------|---------|----------------------|
|**tipo** | Tem de ser definido como `apiHubFile`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure.|
|**direção** | Tem de ser definido como `in`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure. |
|**name** | O nome da variável que representa o item de evento no código de função. | 
|**ligação**| Identifica a definição de aplicação que armazena a cadeia de ligação. A definição de aplicação é criada automaticamente quando adiciona uma ligação de integrar da interface do Usuário, no portal do Azure.|
|**path** | Tem de conter o nome da pasta e o nome de ficheiro. Por exemplo, se tem um [acionador de fila](functions-bindings-storage-queue.md) na sua função, pode usar `"path": "samples-workitems/{queueTrigger}"` para apontar para um ficheiro no `samples-workitems` pasta com um nome correspondente ao nome de ficheiro especificado na mensagem de Acionador.   

## <a name="input---usage"></a>Introdução - utilização

No c# das funções, ligar para os dados de ficheiro de entrada usando um parâmetro com nome em sua assinatura de função, como `<T> <name>`. `T` é o tipo de dados que pretende anular a serialização de dados, e `name` é o nome que especificou no enlace de entrada. Funções de node. js, vai aceder os dados de ficheiro de entrada com `context.bindings.<name>`.

O ficheiro possa ser desserializado em qualquer um dos seguintes tipos:

* Qualquer [objeto](https://msdn.microsoft.com/library/system.object.aspx) - útil para dados de ficheiro JSON serializado.
  Se declarar um tipo de entrada personalizado (por exemplo, `InputType`), as funções do Azure tenta desserializar os dados JSON em seu tipo especificado.
* Cadeia de caracteres - útil para dados de ficheiro de texto.

Também pode ligar a qualquer um dos seguintes tipos de funções c#, e o runtime das funções tenta desserializar os dados de arquivo usando esse tipo:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="output"></a>Saída

O ficheiro externo do Azure enlace de saída permite-lhe escrever ficheiros para uma pasta externa na sua função.

## <a name="output---example"></a>Saída - exemplo

Consulte a [exemplo de enlace de entrada](#input---example).

## <a name="output---configuration"></a>Saída - configuração

A tabela seguinte explica as propriedades de configuração de ligação definida no *Function* ficheiro.

|propriedade de Function | Descrição|
|---------|---------|----------------------|
|**tipo** | Tem de ser definido como `apiHubFile`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure.|
|**direção** | Tem de ser definido como `out`. Esta propriedade é definida automaticamente ao criar o acionador no portal do Azure. |
|**name** | O nome da variável que representa o item de evento no código de função. | 
|**ligação**| Identifica a definição de aplicação que armazena a cadeia de ligação. A definição de aplicação é criada automaticamente quando adiciona uma ligação de integrar da interface do Usuário, no portal do Azure.|
|**path** | Tem de conter o nome da pasta e o nome de ficheiro. Por exemplo, se tem um [acionador de fila](functions-bindings-storage-queue.md) na sua função, pode usar `"path": "samples-workitems/{queueTrigger}"` para apontar para um ficheiro no `samples-workitems` pasta com um nome correspondente ao nome de ficheiro especificado na mensagem de Acionador.   

## <a name="output---usage"></a>Saída - utilização

Em c# funções, ligar para o ficheiro de saída usando o nomeado `out` parâmetro na assinatura de função, como `out <T> <name>`, onde `T` é o tipo de dados que deseja serializar os dados, e `name` é o nome que especificou no o enlace de saída. Nas funções do node. js, acessar o arquivo de saída com `context.bindings.<name>`.

Pode escrever o ficheiro de saída usando qualquer um dos seguintes tipos:

* Qualquer [objeto](https://msdn.microsoft.com/library/system.object.aspx) - útil para a serialização do JSON.
  Se declarar um tipo de saída personalizado (por exemplo, `out OutputType paramName`), as funções do Azure tenta serializar o objeto em JSON. Se o parâmetro de saída é nulo quando a função sai, o runtime das funções cria um ficheiro como um objeto nulo.
* Cadeia de caracteres - (`out string paramName`) útil para dados de ficheiro de texto. o runtime das funções cria um ficheiro apenas se o parâmetro de cadeia de caracteres é não nulo quando a função é fechado.

No c# funções também pode emitir para qualquer um dos seguintes tipos:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
