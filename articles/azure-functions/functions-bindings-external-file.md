---
title: "Enlaces de ficheiro externos para as funções do Azure (experimental)"
description: "Utilizar enlaces de ficheiro externo nas funções do Azure"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/27/2017
ms.author: alkarche
ms.openlocfilehash: 4e9c2c336df465d7488de84bd2a02cc5d9e42f30
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="azure-functions-external-file-bindings-experimental"></a>Azure enlaces de funções de ficheiro externo (experimental)
Este artigo mostra como manipular ficheiros a partir de diferentes fornecedores de SaaS (como o Dropbox ou Google Drive) nas funções do Azure. Funções do Azure suporta acionam, de entrada e saída enlaces para ficheiros externos. Destes enlaces criar ligações de API para fornecedores de SaaS, ou utilizem as ligações de API existentes do grupo de recursos da sua aplicação de função.

> [!IMPORTANT]
> Os enlaces de ficheiro externo são experimental e poderão nunca alcançam Estado geralmente disponível (GA). Estes são incluídas apenas no Azure funciona 1. x e não existem nenhum planos para adicioná-los para as funções do Azure 2. x. Para cenários que requerem acesso aos dados de fornecedores de SaaS, considere a utilização [as logic apps que chamam para funções](functions-twitter-email.md). Consulte o [conector sistema de ficheiros de aplicações lógicas](../logic-apps/logic-apps-using-file-connector.md).

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
|[Unidade do Google](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Ligações de ficheiro externas, também podem ser utilizadas no [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="trigger"></a>Acionador

O acionador de ficheiro externo permite-lhe monitorizar uma pasta remota e executar o código de função quando são detetadas alterações.

## <a name="trigger---example"></a>Acionador - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c#](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-script-example"></a>Acionador - exemplo de script do c#

O exemplo seguinte mostra um acionador de ficheiro externo enlace num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função regista o conteúdo de cada ficheiro que é adicionado à pasta monitorizada.

Segue-se os dados do enlace *function.json* ficheiro:

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

Eis o código de script do c#:

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

### <a name="trigger---javascript-example"></a>Acionador - exemplo de JavaScript

O exemplo seguinte mostra um acionador de ficheiro externo enlace num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função regista o conteúdo de cada ficheiro que é adicionado à pasta monitorizada.

Segue-se os dados do enlace *function.json* ficheiro:

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

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiro.

|propriedade de Function.JSON | Descrição|
|---------|---------|----------------------|
|**tipo** | tem de ser definido como `apiHubFileTrigger`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure.|
|**direção** | tem de ser definido como `in`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure. |
|**nome** | O nome da variável que representa o item de eventos no código da função. | 
|**ligação**| Identifica a definição de aplicação que armazena a cadeia de ligação. A definição de aplicação é criada automaticamente quando adicionar uma ligação na integrar IU no portal do Azure.|
|**caminho** | A pasta para monitorizar e, opcionalmente um padrão de nome.|

### <a name="name-patterns"></a>Padrões de nome

Pode especificar um padrão de nome de ficheiro no `path` propriedade. A pasta referenciada tem de existir no fornecedor de SaaS.

Exemplos:

```json
"path": "input/original-{name}",
```

Este caminho seria localizar um ficheiro denominado *original File1.txt* no *entrada* pasta e o valor do `name` variável no código da função seria `File1.txt`.

Outro exemplo:

```json
"path": "input/{filename}.{fileextension}",
```

Este caminho também poderia localizar um ficheiro denominado *original File1.txt*e o valor do `filename` e `fileextension` variáveis no código da função seria *original File1* e *txt* .

Pode restringir o tipo de ficheiro de ficheiros utilizando um valor fixo para a extensão de ficheiro. Por exemplo:

```json
"path": "samples/{name}.png",
```

Neste caso, apenas *PNG* ficheiros no *amostras* pasta acionar a função.

Chavetas são carateres especiais no nome. Para especificar os nomes de ficheiros que tenham chavetas no nome, faça duplo chavetas.
Por exemplo:

```json
"path": "images/{{20140101}}-{name}",
```

Este caminho seria localizar um ficheiro denominado *{20140101}-soundfile.mp3* no *imagens* pasta e o `name` valor da variável no código da função seria *soundfile.mp3*.

## <a name="trigger---usage"></a>Acionador - utilização

C# funções, vincular os dados de ficheiro de entrada através de um parâmetro com nome na sua assinatura da função, como `<T> <name>`.
Onde `T` é o tipo de dados que pretende anular a serialização de dados e `paramName` é o nome especificado no [acionar JSON](#trigger). Funções de Node.js, vai aceder a dados de ficheiro de entrada utilizando `context.bindings.<name>`.

O ficheiro pode ser desserializado em qualquer um dos seguintes tipos:

* Qualquer [objeto](https://msdn.microsoft.com/library/system.object.aspx) - útil para dados de ficheiro serializados para JSON.
  Se declarar um tipo de entrada personalizado (por exemplo, `FooType`), as funções do Azure tentar anular a serialização os dados JSON para o tipo especificado.
* Cadeia - útil para dados de ficheiro de texto.

C# funções, é também possível vincular a qualquer um dos seguintes tipos e o tempo de execução de funções tenta anular a serialização dos dados de ficheiros utilizando esse tipo:

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

## <a name="trigger---poison-files"></a>Acionador - nocivas ficheiros

Quando uma função de Acionador de ficheiro externo falha, as funções do Azure tentará novamente essa função até 5 vezes por predefinição (incluindo a primeira tentativa) para um determinado ficheiro.
Se a todas as tentativas de 5 falharem, as funções adiciona uma mensagem para uma fila de armazenamento com o nome *webjobs-apihubtrigger-poison*. A mensagem da fila para ficheiros nocivas é um objeto JSON que contém as seguintes propriedades:

* FunctionId (no formato  *&lt;nome da aplicação de função >*. Funções.  *&lt;nome de função >*)
* FileType
* FolderName
* nome de ficheiro
* ETag (um identificador de versão do ficheiro, por exemplo: "0x8D1DC6E70A277EF")

## <a name="input"></a>Input

O enlace de entrada do ficheiro externo do Azure permite-lhe utilizar um ficheiro a partir de uma pasta na sua função externa.

## <a name="input---example"></a>Entrada - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c#](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Entrada - exemplo de script do c#

O exemplo seguinte mostra o ficheiro externo enlaces de entrada e de saída num *function.json* ficheiro e uma [função de script do c#](functions-reference-csharp.md) que utiliza o enlace. A função copia um ficheiro de entrada para um ficheiro de saída.

Segue-se os dados do enlace *function.json* ficheiro:

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

Eis o código de script do c#:

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

### <a name="input---javascript-example"></a>Entrada - exemplo de JavaScript

O exemplo seguinte mostra o ficheiro externo enlaces de entrada e de saída num *function.json* ficheiro e uma [JavaScript função](functions-reference-node.md) que utiliza o enlace. A função copia um ficheiro de entrada para um ficheiro de saída.

Segue-se os dados do enlace *function.json* ficheiro:

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

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiro.

|propriedade de Function.JSON | Descrição|
|---------|---------|----------------------|
|**tipo** | tem de ser definido como `apiHubFile`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure.|
|**direção** | tem de ser definido como `in`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure. |
|**nome** | O nome da variável que representa o item de eventos no código da função. | 
|**ligação**| Identifica a definição de aplicação que armazena a cadeia de ligação. A definição de aplicação é criada automaticamente quando adicionar uma ligação na integrar IU no portal do Azure.|
|**caminho** | Tem de conter o nome de pasta e o nome de ficheiro. Por exemplo, se tiver um [acionador de fila](functions-bindings-storage-queue.md) na sua função, pode utilizar `"path": "samples-workitems/{queueTrigger}"` para apontar para um ficheiro a `samples-workitems` pasta com um nome que corresponda ao nome de ficheiro especificado na mensagem de Acionador.   

## <a name="input---usage"></a>Entrada - utilização

C# funções, vincular os dados de ficheiro de entrada através de um parâmetro com nome na sua assinatura da função, como `<T> <name>`. `T`é o tipo de dados que pretende anular a serialização de dados e `name` é o nome especificado no vínculo da entrada. Funções de Node.js, vai aceder a dados de ficheiro de entrada utilizando `context.bindings.<name>`.

O ficheiro pode ser desserializado em qualquer um dos seguintes tipos:

* Qualquer [objeto](https://msdn.microsoft.com/library/system.object.aspx) - útil para dados de ficheiro serializados para JSON.
  Se declarar um tipo de entrada personalizado (por exemplo, `InputType`), as funções do Azure tentar anular a serialização os dados JSON para o tipo especificado.
* Cadeia - útil para dados de ficheiro de texto.

C# funções, é também possível vincular a qualquer um dos seguintes tipos e o tempo de execução de funções tenta anular a serialização dos dados de ficheiros utilizando esse tipo:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="output"></a>Saída

O ficheiro externo do Azure enlace de saída permite-lhe escrever ficheiros para uma pasta na sua função externa.

## <a name="output---example"></a>De saída - exemplo

Consulte o [exemplo de enlace de entrada](#input---example).

## <a name="output---configuration"></a>De saída - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiro.

|propriedade de Function.JSON | Descrição|
|---------|---------|----------------------|
|**tipo** | tem de ser definido como `apiHubFile`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure.|
|**direção** | tem de ser definido como `out`. Esta propriedade é definida automaticamente quando criar o acionador no portal do Azure. |
|**nome** | O nome da variável que representa o item de eventos no código da função. | 
|**ligação**| Identifica a definição de aplicação que armazena a cadeia de ligação. A definição de aplicação é criada automaticamente quando adicionar uma ligação na integrar IU no portal do Azure.|
|**caminho** | Tem de conter o nome de pasta e o nome de ficheiro. Por exemplo, se tiver um [acionador de fila](functions-bindings-storage-queue.md) na sua função, pode utilizar `"path": "samples-workitems/{queueTrigger}"` para apontar para um ficheiro a `samples-workitems` pasta com um nome que corresponda ao nome de ficheiro especificado na mensagem de Acionador.   

## <a name="output---usage"></a>Saída - utilização

C# funções, vincular o ficheiro de saída utilizando o nomeado `out` como parâmetro a assinatura da função, `out <T> <name>`, onde `T` é o tipo de dados que pretende serializar os dados para, e `name` é o nome especificado no o enlace de saída. Funções de Node.js, vai aceder a ficheiros de saída com `context.bindings.<name>`.

Pode escrever o ficheiro de saída com qualquer um dos seguintes tipos:

* Qualquer [objeto](https://msdn.microsoft.com/library/system.object.aspx) - útil para serialização JSON.
  Se declarar um tipo de saída personalizado (por exemplo, `out OutputType paramName`), as funções do Azure tentar serializar o objeto em JSON. Se o parâmetro de saída é nulo quando a função sai, o tempo de execução de funções cria um ficheiro como um objeto nulo.
* Cadeia - (`out string paramName`) útil para dados de ficheiro de texto. o tempo de execução de funções cria um ficheiro apenas se o parâmetro de cadeia não nulo quando a função sai.

C# funções também pode apresentar a qualquer um dos seguintes tipos:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)
