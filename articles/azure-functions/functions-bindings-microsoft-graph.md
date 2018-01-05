---
title: "Enlaces de Microsoft Graph para as funções do Azure"
description: "Compreenda como utilizar o Microsoft Graph acionadores e enlaces das funções do Azure."
services: functions
author: mattchenderson
manager: cfowler
editor: 
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 12/20/2017
ms.author: mahender
ms.openlocfilehash: 63b94c0a9b77a3f3a6fd394a130bf8f132d51369
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/04/2018
---
# <a name="microsoft-graph-bindings-for-azure-functions"></a>Enlaces de Microsoft Graph para as funções do Azure

Este artigo explica como configurar e trabalhar com o Microsoft Graph acionadores e enlaces de funções do Azure. Com estas, pode utilizar as funções do Azure para trabalhar com dados, insights e eventos a partir de [Microsoft Graph](https://graph.microsoft.io).

A extensão de Microsoft Graph fornece os enlaces do seguintes:
- Um [enlace de entrada do token de autenticação](#token-input) permite-lhe interagir com quaisquer Microsoft Graph API.
- Um [enlace de entrada de tabela de Excel](#excel-input) permite-lhe ler dados a partir do Excel.
- Um [vínculo de saída do tabela do Excel](#excel-output) permite-lhe modificar a dados do Excel.
- A [enlace de entrada do ficheiro do OneDrive](#onedrive-input) permite-lhe ler os ficheiros do OneDrive.
- A [vínculo de saída do ficheiro do OneDrive](#onedrive-output) permite-lhe escrever nos ficheiros no OneDrive.
- Um [vínculo de saída de mensagem do Outlook](#outlook-output) permite-lhe enviar correio eletrónico através do Outlook.
- Uma coleção de [Microsoft Graph webhook acionadores e enlaces](#webhooks) permite-lhe reagir a eventos a partir do Microsoft Graph.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!Note]
> Enlaces de Microsoft Graph estão atualmente em pré-visualização.

## <a name="setting-up-the-extensions"></a>Configurar as extensões

Enlaces de Microsoft Graph estão disponíveis através de _enlace extensões_. Extensões de enlace estão componentes opcionais para o tempo de execução das funções do Azure. Esta secção mostra como configurar o Microsoft Graph e extensões de token de autenticação.

### <a name="enabling-functions-20-preview"></a>Ativar funções 2.0 pré-visualização

Extensões de enlace estão disponíveis apenas para a pré-visualização do Azure funções 2.0. 

Para obter informações sobre como definir uma aplicação de função para utilizar a versão de pré-visualização 2.0 do tempo de execução de funções, consulte [visar o tempo de execução da versão 2.0](functions-versions.md#target-the-version-20-runtime).

### <a name="installing-the-extension"></a>Instalação da extensão

Para instalar uma extensão do portal do Azure, navegue para um modelo ou do enlace que referencia-lo. Criar uma nova função e, no ecrã de seleção de modelo, escolha o cenário de "Microsoft Graph". Selecione um dos modelos neste cenário. Em alternativa, pode navegar para o separador "Integrar" de uma função existente e selecione uma dos enlaces abordados neste artigo.

Em ambos os casos, será apresentado um aviso que especifica a extensão para ser instalada. Clique em **instalar** para obter a extensão.

> [!Note] 
> Cada extensão só tem de ser instalado uma vez por aplicação de função. O processo de instalação no portal pode demorar até 10 minutos num plano de consumo.

Se estiver a utilizar o Visual Studio, pode obter as extensões instalando estes pacotes de NuGet:
- [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/)
- [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/)

### <a name="configuring-authentication--authorization"></a>Configurar a autenticação / autorização

Os enlaces descritos neste artigo requerem uma identidade para ser utilizado. Isto permite que o Microsoft Graph impor permissões e as interações de auditoria. A identidade pode ser um utilizador aceder à sua aplicação ou a própria aplicação. Para configurar esta identidade, configurar [aplicação serviço de autenticação / autorização](https://docs.microsoft.com/azure/app-service/app-service-authentication-overview) com o Azure Active Directory. Também terá de pedir as permissões do recurso que requerem as suas funções.

> [!Note] 
> A extensão de Microsoft Graph só suporta a autenticação do Azure AD. Os utilizadores têm de iniciar sessão com uma conta escolar ou profissional.

Se estiver a utilizar o portal do Azure, verá um aviso abaixo a linha de comandos para instalar a extensão. O aviso pede-lhe para configurar a aplicação serviço de autenticação / autorização e pedido necessita de permissões de modelo ou do enlace. Clique em **agora configurar do Azure AD** ou **adicionar permissões agora** conforme adequado.



<a name="token-input"></a>
## <a name="auth-token"></a>Token de autenticação

O enlace de entrada de token de autenticação obtém um Azure AD token para um determinado recurso e fornece-o para o seu código como uma cadeia. O recurso pode ser nenhum para o qual a aplicação tem as permissões. 

Esta secção contém as seguintes subsecções:

* [Exemplo](#auth-token---example)
* [Atributos](#auth-token---attributes)
* [Configuração](#auth-token---configuration)
* [Utilização](#auth-token---usage)

### <a name="auth-token---example"></a>Token de autenticação - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#auth-token---c-script-example)
* [JavaScript](#auth-token---javascript-example)

#### <a name="auth-token---c-script-example"></a>Token de autenticação - exemplo de script do c#

O exemplo seguinte obtém as informações do perfil de utilizador.

O *function.json* ficheiro define um acionador HTTP com um enlace de entrada token:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O código de script do c# utiliza o token para efetuar uma chamada HTTP para o Microsoft Graph e devolve o resultado:

```csharp
using System.Net; 
using System.Net.Http; 
using System.Net.Http.Headers; 

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, string graphToken, TraceWriter log)
{
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", graphToken);
    return await client.GetAsync("https://graph.microsoft.com/v1.0/me/");
}
```

#### <a name="auth-token---javascript-example"></a>Token de autenticação - exemplo de JavaScript

O exemplo seguinte obtém as informações do perfil de utilizador.

O *function.json* ficheiro define um acionador HTTP com um enlace de entrada token:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "token",
      "direction": "in",
      "name": "graphToken",
      "resource": "https://graph.microsoft.com",
      "identity": "userFromRequest"
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

O código JavaScript utiliza o token para efetuar uma chamada HTTP para o Microsoft Graph e devolve o resultado.

```js
const rp = require('request-promise');

module.exports = function (context, req) {
    let token = "Bearer " + context.bindings.graphToken;

    let options = {
        uri: 'https://graph.microsoft.com/v1.0/me/',
        headers: {
            'Authorization': token
        }
    };
    
    rp(options)
        .then(function(profile) {
            context.res = {
                body: profile
            };
            context.done();
        })
        .catch(function(err) {
            context.res = {
                status: 500,
                body: err
            };
            context.done();
        });
};
```

### <a name="auth-token---attributes"></a>Token de autenticação - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [Token](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/TokenBinding/TokenAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.AuthTokens](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.AuthTokens/).

### <a name="auth-token---configuration"></a>Token de autenticação - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `Token` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para o token de autenticação. Consulte [utilizar um token de autenticação de entrada do enlace a partir do código](#token-input-code).|
|**tipo**||Necessário - tem de ser definido como `token`.|
|**direção**||Necessário - tem de ser definido como `in`.|
|**identidade**|**Identidade**|Necessário - a identidade que será utilizada para executar a ação. Pode ser um dos seguintes valores:<ul><li><code>userFromRequest</code>-Só é válido com [acionador HTTP]. Utiliza a identidade do utilizador chamada.</li><li><code>userFromId</code>-Utiliza a identidade de um utilizador com sessão iniciada no anteriormente com o ID especificado. Consulte o <code>userId</code> propriedade.</li><li><code>userFromToken</code>-Utiliza a identidade representada pelo token especificado. Consulte o <code>userToken</code> propriedade.</li><li><code>clientCredentials</code>-Utiliza a identidade da aplicação de função.</li></ul>|
|**ID de utilizador**|**ID de utilizador**  |Se necessário e apenas se _identidade_ está definido como `userFromId`. ID principal de utilizador associado um utilizador anteriormente com sessão iniciada.|
|**userToken**|**UserToken**|Se necessário e apenas se _identidade_ está definido como `userFromToken`. Um token válido para a aplicação de função. |
|**Recurso**|**recursos**|Necessário - um URL de recursos do Azure AD para o qual está a ser pedido o token.|

<a name="token-input-code"></a>
### <a name="auth-token---usage"></a>Token de autenticação - utilização

O enlace em si não necessita de permissões do Azure AD, mas, dependendo de como o token é utilizado, poderá ser necessário solicitar permissões adicionais. Verifique os requisitos do recurso que pretende aceder com o token.

O token é sempre apresentado para código como uma cadeia.




<a name="excel-input"></a>
## <a name="excel-input"></a>Entrada de Excel

O enlace de entrada de tabela de Excel lê os conteúdos de uma tabela de Excel armazenados no OneDrive.

Esta secção contém as seguintes subsecções:

* [Exemplo](#excel-input---example)
* [Atributos](#excel-input---attributes)
* [Configuração](#excel-input---configuration)
* [Utilização](#excel-input---usage)

### <a name="excel-input---example"></a>Excel entrada - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#excel-input---c-script-example)
* [JavaScript](#excel-input---javascript-example)

#### <a name="excel-input---c-script-example"></a>Excel de entrada - exemplo de script do c#

O seguinte *function.json* ficheiro define um acionador HTTP com um enlace de entrada do Excel:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O seguinte código de script do c# lê o conteúdo da tabela especificada e devolve-os para o utilizador:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives; 

public static IActionResult Run(HttpRequest req, string[][] excelTableData, TraceWriter log)
{
    return new OkObjectResult(excelTableData);
}
```

#### <a name="excel-input---javascript-example"></a>Excel de entrada - exemplo de JavaScript

O seguinte *function.json* ficheiro define um acionador HTTP com um enlace de entrada do Excel:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "excel",
      "direction": "in",
      "name": "excelTableData",
      "path": "{query.workbook}",
      "identity": "UserFromRequest",
      "tableName": "{query.table}"
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

O seguinte código JavaScript lê o conteúdo da tabela especificada e devolve-os para o utilizador.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.excelTableData
    };
    context.done();
};
```

### <a name="excel-input---attributes"></a>Excel de entrada - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="excel-input---configuration"></a>Excel entrada - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `Excel` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para a tabela de Excel. Consulte [utilizando uma tabela de Excel de entrada do enlace a partir do código](#excel-input-code).|
|**tipo**||Necessário - tem de ser definido como `excel`.|
|**direção**||Necessário - tem de ser definido como `in`.|
|**identidade**|**Identidade**|Necessário - a identidade que será utilizada para executar a ação. Pode ser um dos seguintes valores:<ul><li><code>userFromRequest</code>-Só é válido com [acionador HTTP]. Utiliza a identidade do utilizador chamada.</li><li><code>userFromId</code>-Utiliza a identidade de um utilizador com sessão iniciada no anteriormente com o ID especificado. Consulte o <code>userId</code> propriedade.</li><li><code>userFromToken</code>-Utiliza a identidade representada pelo token especificado. Consulte o <code>userToken</code> propriedade.</li><li><code>clientCredentials</code>-Utiliza a identidade da aplicação de função.</li></ul>|
|**ID de utilizador**|**ID de utilizador**  |Se necessário e apenas se _identidade_ está definido como `userFromId`. ID principal de utilizador associado um utilizador anteriormente com sessão iniciada.|
|**userToken**|**UserToken**|Se necessário e apenas se _identidade_ está definido como `userFromToken`. Um token válido para a aplicação de função. |
|**caminho**|**Caminho**|Necessário - o caminho no OneDrive para o livro do Excel.|
|**worksheetName**|**WorksheetName**|A folha de cálculo que é possível localizar a tabela.|
|**tableName**|**TableName**|O nome da tabela. Se não for especificado, será utilizado o conteúdo da folha de cálculo.|

<a name="excel-input-code"></a>
### <a name="excel-input---usage"></a>Excel de entrada - utilização

Este enlace requer as seguintes permissões do Azure AD:
|Recurso|Permissão|
|--------|--------|
|Microsoft Graph|Ler ficheiros do utilizador|

O enlace expõe os seguintes tipos de funções de .NET:
- String [] []
- Microsoft.Graph.WorkbookTable
- Tipos de objetos personalizados (utilizando o enlace do modelo estruturais)










<a name="excel-output"></a>
## <a name="excel-output"></a>Saída do Excel

O Excel enlace de saída modifica o conteúdo de uma tabela de Excel armazenados no OneDrive.

Esta secção contém as seguintes subsecções:

* [Exemplo](#excel-output---example)
* [Atributos](#excel-output---attributes)
* [Configuração](#excel-output---configuration)
* [Utilização](#excel-output---usage)

### <a name="excel-output---example"></a>Excel resultado - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#excel-output---c-script-example)
* [JavaScript](#excel-output---javascript-example)

#### <a name="excel-output---c-script-example"></a>Excel de saída - exemplo de script do c#

O exemplo seguinte adiciona linhas para uma tabela do Excel.

O *function.json* ficheiro define um acionador HTTP com um Excel vínculo de saída:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O código de script do c# adiciona uma nova linha à tabela (pressupõe-se que única coluna) com base na entrada da cadeia de consulta:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, IAsyncCollector<object> newExcelRow, TraceWriter log)
{
    string input = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await newExcelRow.AddAsync(new {
        Text = input
        // Add other properties for additional columns here
    });
    return;
}
```

#### <a name="excel-output---javascript-example"></a>Excel de saída - exemplo de JavaScript

O exemplo seguinte adiciona linhas para uma tabela do Excel.

O *function.json* ficheiro define um acionador HTTP com um Excel vínculo de saída:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "newExcelRow",
      "type": "excel",
      "direction": "out",
      "identity": "userFromRequest",
      "updateType": "append",
      "path": "{query.workbook}",
      "tableName": "{query.table}"
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

O seguinte código JavaScript adiciona uma nova linha à tabela (pressupõe-se que única coluna) com base na entrada da cadeia de consulta.

```js
module.exports = function (context, req) {
    context.bindings.newExcelRow = {
        text: req.query.text
        // Add other properties for additional columns here
    }
    context.done();
};
```

### <a name="excel-output---attributes"></a>Excel de saída - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [Excel](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/ExcelAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="excel-output---configuration"></a>Excel resultado - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `Excel` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para o token de autenticação. Consulte [através de uma tabela do Excel enlace a partir do código de saída](#excel-output-code).|
|**tipo**||Necessário - tem de ser definido como `excel`.|
|**direção**||Necessário - tem de ser definido como `out`.|
|**identidade**|**Identidade**|Necessário - a identidade que será utilizada para executar a ação. Pode ser um dos seguintes valores:<ul><li><code>userFromRequest</code>-Só é válido com [acionador HTTP]. Utiliza a identidade do utilizador chamada.</li><li><code>userFromId</code>-Utiliza a identidade de um utilizador com sessão iniciada no anteriormente com o ID especificado. Consulte o <code>userId</code> propriedade.</li><li><code>userFromToken</code>-Utiliza a identidade representada pelo token especificado. Consulte o <code>userToken</code> propriedade.</li><li><code>clientCredentials</code>-Utiliza a identidade da aplicação de função.</li></ul>|
|**ID de utilizador** |**ID de utilizador** |Se necessário e apenas se _identidade_ está definido como `userFromId`. ID principal de utilizador associado um utilizador anteriormente com sessão iniciada.|
|**userToken**|**UserToken**|Se necessário e apenas se _identidade_ está definido como `userFromToken`. Um token válido para a aplicação de função. |
|**caminho**|**Caminho**|Necessário - o caminho no OneDrive para o livro do Excel.|
|**worksheetName**|**WorksheetName**|A folha de cálculo que é possível localizar a tabela.|
|**tableName**|**TableName**|O nome da tabela. Se não for especificado, será utilizado o conteúdo da folha de cálculo.|
|**Tipodeatualização**|**Tipodeatualização**|Necessário - o tipo de alteração para tornar à tabela. Pode ser um dos seguintes valores:<ul><li><code>update</code>-Substitui o conteúdo da tabela no OneDrive.</li><li><code>append</code>-Adiciona o payload ao fim da tabela no OneDrive, criando novas linhas.</li></ul>|

<a name="excel-output-code"></a>
### <a name="excel-output---usage"></a>Excel de saída - utilização

Este enlace requer as seguintes permissões do Azure AD:
|Recurso|Permissão|
|--------|--------|
|Microsoft Graph|Tem acesso total aos ficheiros de utilizador|

O enlace expõe os seguintes tipos de funções de .NET:
- String [] []
- Newtonsoft.Json.Linq.JObject
- Microsoft.Graph.WorkbookTable
- Tipos de objetos personalizados (utilizando o enlace do modelo estruturais)





<a name="onedrive-input"></a>
## <a name="file-input"></a>Entrada de ficheiro

O enlace de entrada do ficheiro do OneDrive lê os conteúdos de um ficheiro armazenados no OneDrive.

Esta secção contém as seguintes subsecções:

* [Exemplo](#file-input---example)
* [Atributos](#file-input---attributes)
* [Configuração](#file-input---configuration)
* [Utilização](#file-input---usage)

### <a name="file-input---example"></a>Ficheiro de entrada - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#file-input---c-script-example)
* [JavaScript](#file-input---javascript-example)

#### <a name="file-input---c-script-example"></a>Ficheiro de entrada - exemplo de script do c#

O exemplo seguinte lê um ficheiro que é armazenado no OneDrive.

O *function.json* ficheiro define um acionador HTTP com um enlace de entrada de ficheiro do OneDrive:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O código de script do c# lê o ficheiro especificado na cadeia de consulta e regista o seu comprimento:

```csharp
using System.Net;

public static void Run(HttpRequestMessage req, Stream myOneDriveFile, TraceWriter log)
{
    log.Info(myOneDriveFile.Length.ToString());
}
```

#### <a name="file-input---javascript-example"></a>Ficheiro de entrada - exemplo de JavaScript

O exemplo seguinte lê um ficheiro que é armazenado no OneDrive.

O *function.json* ficheiro define um acionador HTTP com um enlace de entrada de ficheiro do OneDrive:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "in",
      "path": "{query.filename}",
      "identity": "userFromRequest"
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

O seguinte código JavaScript lê o ficheiro especificado na cadeia de consulta e devolve o comprimento.

```js
module.exports = function (context, req) {
    context.res = {
        body: context.bindings.myOneDriveFile.length
    };
    context.done();
};
```

### <a name="file-input---attributes"></a>Ficheiro de entrada - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="file-input---configuration"></a>Ficheiro de entrada - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `OneDrive` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para o ficheiro. Consulte [utilizando um ficheiro do OneDrive de entrada do enlace a partir do código](#onedrive-input-code).|
|**tipo**||Necessário - tem de ser definido como `onedrive`.|
|**direção**||Necessário - tem de ser definido como `in`.|
|**identidade**|**Identidade**|Necessário - a identidade que será utilizada para executar a ação. Pode ser um dos seguintes valores:<ul><li><code>userFromRequest</code>-Só é válido com [acionador HTTP]. Utiliza a identidade do utilizador chamada.</li><li><code>userFromId</code>-Utiliza a identidade de um utilizador com sessão iniciada no anteriormente com o ID especificado. Consulte o <code>userId</code> propriedade.</li><li><code>userFromToken</code>-Utiliza a identidade representada pelo token especificado. Consulte o <code>userToken</code> propriedade.</li><li><code>clientCredentials</code>-Utiliza a identidade da aplicação de função.</li></ul>|
|**ID de utilizador**|**ID de utilizador**  |Se necessário e apenas se _identidade_ está definido como `userFromId`. ID principal de utilizador associado um utilizador anteriormente com sessão iniciada.|
|**userToken**|**UserToken**|Se necessário e apenas se _identidade_ está definido como `userFromToken`. Um token válido para a aplicação de função. |
|**caminho**|**Caminho**|Necessário - o caminho do ficheiro no OneDrive.|

<a name="onedrive-input-code"></a>
### <a name="file-input---usage"></a>Ficheiro de entrada - utilização

Este enlace requer as seguintes permissões do Azure AD:
|Recurso|Permissão|
|--------|--------|
|Microsoft Graph|Ler ficheiros do utilizador|

O enlace expõe os seguintes tipos de funções de .NET:
- Byte]
- Stream
- string
- Microsoft.Graph.DriveItem






<a name="onedrive-output"></a>
## <a name="file-output"></a>Ficheiro de saída

O ficheiro do OneDrive enlace de saída modifica o conteúdo de um ficheiro armazenados no OneDrive.

Esta secção contém as seguintes subsecções:

* [Exemplo](#file-output---example)
* [Atributos](#file-output---attributes)
* [Configuração](#file-output---configuration)
* [Utilização](#file-output---usage)

### <a name="file-output---example"></a>Ficheiro de saída - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#file-output---c-script-example)
* [JavaScript](#file-output---javascript-example)

#### <a name="file-output---c-script-example"></a>Ficheiro de saída - exemplo de script do c#

O exemplo seguinte escreve num ficheiro que é armazenado no OneDrive.

O *function.json* ficheiro define um acionador HTTP com um OneDrive vínculo de saída:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O código de script do c# obtém o texto da cadeia de consulta e escreve-a para um ficheiro de texto (FunctionsTest.txt conforme definido no exemplo anterior) na raiz do OneDrive do emissor:

```csharp
using System.Net;
using System.Text;

public static async Task Run(HttpRequest req, TraceWriter log, Stream myOneDriveFile)
{
    string data = req.Query
        .FirstOrDefault(q => string.Compare(q.Key, "text", true) == 0)
        .Value;
    await myOneDriveFile.WriteAsync(Encoding.UTF8.GetBytes(data), 0, data.Length);
    return;
}
```

#### <a name="file-output---javascript-example"></a>Ficheiro de saída - exemplo de JavaScript

O exemplo seguinte escreve num ficheiro que é armazenado no OneDrive.

O *function.json* ficheiro define um acionador HTTP com um OneDrive vínculo de saída:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "myOneDriveFile",
      "type": "onedrive",
      "direction": "out",
      "path": "FunctionsTest.txt",
      "identity": "userFromRequest"
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

O código JavaScript obtém o texto da cadeia de consulta e escreve-a para um ficheiro de texto (FunctionsTest.txt como definido na configuração acima) na raiz do OneDrive da função invocadora.

```js
module.exports = function (context, req) {
    context.bindings.myOneDriveFile = req.query.text;
    context.done();
};
```

### <a name="file-output---attributes"></a>Ficheiro de saída - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [OneDrive](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OneDriveAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="file-output---configuration"></a>Ficheiro de saída - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `OneDrive` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para o ficheiro. Consulte [utilizando um ficheiro do OneDrive enlace a partir do código de saída](#onedrive-output-code).|
|**tipo**||Necessário - tem de ser definido como `onedrive`.|
|**direção**||Necessário - tem de ser definido como `out`.|
|**identidade**|**Identidade**|Necessário - a identidade que será utilizada para executar a ação. Pode ser um dos seguintes valores:<ul><li><code>userFromRequest</code>-Só é válido com [acionador HTTP]. Utiliza a identidade do utilizador chamada.</li><li><code>userFromId</code>-Utiliza a identidade de um utilizador com sessão iniciada no anteriormente com o ID especificado. Consulte o <code>userId</code> propriedade.</li><li><code>userFromToken</code>-Utiliza a identidade representada pelo token especificado. Consulte o <code>userToken</code> propriedade.</li><li><code>clientCredentials</code>-Utiliza a identidade da aplicação de função.</li></ul>|
|**ID de utilizador** |**ID de utilizador** |Se necessário e apenas se _identidade_ está definido como `userFromId`. ID principal de utilizador associado um utilizador anteriormente com sessão iniciada.|
|**userToken**|**UserToken**|Se necessário e apenas se _identidade_ está definido como `userFromToken`. Um token válido para a aplicação de função. |
|**caminho**|**Caminho**|Necessário - o caminho do ficheiro no OneDrive.|

<a name="onedrive-output-code"></a>
#### <a name="file-output---usage"></a>Ficheiro de saída - utilização

Este enlace requer as seguintes permissões do Azure AD:
|Recurso|Permissão|
|--------|--------|
|Microsoft Graph|Tem acesso total aos ficheiros de utilizador|

O enlace expõe os seguintes tipos de funções de .NET:
- Byte]
- Stream
- string
- Microsoft.Graph.DriveItem





<a name="outlook-output"></a>
## <a name="outlook-output"></a>Saída de Outlook

A mensagem do Outlook saída enlace envia uma mensagem de correio através do Outlook.

Esta secção contém as seguintes subsecções:

* [Exemplo](#outlook-output---example)
* [Atributos](#outlook-output---attributes)
* [Configuração](#outlook-output---configuration)
* [Utilização](#outlook-outnput---usage)

### <a name="outlook-output---example"></a>Saída do Outlook - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#outlook-output---c-script-example)
* [JavaScript](#outlook-output---javascript-example)

#### <a name="outlook-output---c-script-example"></a>Outlook de saída - exemplo de script do c#

O exemplo seguinte envia uma mensagem de e-mail através do Outlook.

O *function.json* ficheiro define um acionador HTTP com um Outlook vínculo de saída de mensagem:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

O código de script do c# envia um e-mail do autor da chamada para um destinatário especificado na cadeia de consulta:

```csharp
using System.Net;

public static void Run(HttpRequest req, out Message message, TraceWriter log)
{ 
    string emailAddress = req.Query["to"];
    message = new Message(){
        subject = "Greetings",
        body = "Sent from Azure Functions",
        recipient = new Recipient() {
            address = emailAddress
        }
    };
}

public class Message {
    public String subject {get; set;}
    public String body {get; set;}
    public Recipient recipient {get; set;}
}

public class Recipient {
    public String address {get; set;}
    public String name {get; set;}
}
```

#### <a name="outlook-output---javascript-example"></a>Outlook de saída - exemplo de JavaScript

O exemplo seguinte envia uma mensagem de e-mail através do Outlook.

O *function.json* ficheiro define um acionador HTTP com um Outlook vínculo de saída de mensagem:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "message",
      "type": "outlook",
      "direction": "out",
      "identity": "userFromRequest"
    }
  ],
  "disabled": false
}
```

O código JavaScript envia um e-mail do autor da chamada para um destinatário especificado na cadeia de consulta:

```js
module.exports = function (context, req) {
    context.bindings.message = {
        subject: "Greetings",
        body: "Sent from Azure Functions with JavaScript",
        recipient: {
            address: req.query.to 
        } 
    };
    context.done();
};
```

### <a name="outlook-output---attributes"></a>Outlook de saída - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [Outlook](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/OutlookAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="outlook-output---configuration"></a>Saída do Outlook - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `Outlook` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para a mensagem de correio. Consulte [através de uma mensagem do Outlook enlace a partir do código de saída](#outlook-output-code).|
|**tipo**||Necessário - tem de ser definido como `outlook`.|
|**direção**||Necessário - tem de ser definido como `out`.|
|**identidade**|**Identidade**|Necessário - a identidade que será utilizada para executar a ação. Pode ser um dos seguintes valores:<ul><li><code>userFromRequest</code>-Só é válido com [acionador HTTP]. Utiliza a identidade do utilizador chamada.</li><li><code>userFromId</code>-Utiliza a identidade de um utilizador com sessão iniciada no anteriormente com o ID especificado. Consulte o <code>userId</code> propriedade.</li><li><code>userFromToken</code>-Utiliza a identidade representada pelo token especificado. Consulte o <code>userToken</code> propriedade.</li><li><code>clientCredentials</code>-Utiliza a identidade da aplicação de função.</li></ul>|
|**ID de utilizador**|**ID de utilizador**  |Se necessário e apenas se _identidade_ está definido como `userFromId`. ID principal de utilizador associado um utilizador anteriormente com sessão iniciada.|
|**userToken**|**UserToken**|Se necessário e apenas se _identidade_ está definido como `userFromToken`. Um token válido para a aplicação de função. |

<a name="outlook-output-code"></a>
### <a name="outlook-output---usage"></a>Outlook de saída - utilização

Este enlace requer as seguintes permissões do Azure AD:
|Recurso|Permissão|
|--------|--------|
|Microsoft Graph|Enviar correio eletrónico como utilizador|

O enlace expõe os seguintes tipos de funções de .NET:
- Microsoft.Graph.Message
- Newtonsoft.Json.Linq.JObject
- string
- Tipos de objetos personalizados (utilizando o enlace do modelo estruturais)






## <a name="webhooks"></a>Webhooks

Webhooks permitem-lhe reagir a eventos na Microsoft Graph. Para suportar webhooks, as funções são necessárias para criar, atualizar e reagir a _subscrições de webhook_. Uma solução completa de webhook requer uma combinação dos enlaces seguintes:
- A [acionador de webhook Microsoft Graph](#webhook-trigger) permite-lhe reagir a um webhook de entrada.
- A [enlace de entrada de subscrição do Microsoft Graph webhook](#webhook-input) permite-lhe a lista de subscrições existentes e, opcionalmente, atualize-los.
- A [vínculo de saída de subscrição do Microsoft Graph webhook](#webhook-output) permite-lhe criar ou eliminar subscrições de webhook.

Os enlaces próprios necessita de permissões do Azure AD, mas é necessário solicitar permissões relevantes para o tipo de recurso que pretende reagir a. Para obter uma lista que são necessárias permissões para cada tipo de recurso, consulte [permissões de subscrição](https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/subscription_post_subscriptions#permissions).

Para mais informações sobre webhooks, consulte [trabalhar com webhooks no Microsoft Graph].





## <a name="webhook-trigger"></a>Webhook acionador

O acionador de webhook Microsoft Graph permite que uma função de reagir a um webhook de entrada da Microsoft Graph. Cada instância deste acionador possam reagir a um tipo de recurso do Microsoft Graph.

Esta secção contém as seguintes subsecções:

* [Exemplo](#webhook-trigger---example)
* [Atributos](#webhook-trigger---attributes)
* [Configuração](#webhook-trigger---configuration)
* [Utilização](#webhook-trigger---usage)

### <a name="webhook-trigger---example"></a>Acionador de Webhook - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#webhook-trigger---c-script-example)
* [JavaScript](#webhook-trigger---javascript-example)

#### <a name="webhook-trigger---c-script-example"></a>Acionador de Webhook - exemplo de script do c#

O exemplo seguinte processa webhooks para mensagens a receber Outlook. Para utilizar um webhook acionar [criar uma subscrição](#webhook-output---example), e pode [atualizar a subscrição](#webhook-subscription-refresh) para os impedir de expirar.

O *function.json* ficheiro define um acionador de webhook:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

O código de script do c# reage a receber mensagens de correio e regista o corpo desses enviados pelo destinatário e que contêm "Das funções do Azure" no assunto:

```csharp
#r "Microsoft.Graph"
using Microsoft.Graph;
using System.Net;

public static async Task Run(Message msg, TraceWriter log)  
{
    log.Info("Microsoft Graph webhook trigger function processed a request.");

    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if (msg.Subject.Contains("Azure Functions") && msg.From.Equals(msg.Sender)) {
        log.Info($"Processed email: {msg.BodyPreview}");
    }
}
```

#### <a name="webhook-trigger---javascript-example"></a>Acionador de Webhook - exemplo de JavaScript

O exemplo seguinte processa webhooks para mensagens a receber Outlook. Para utilizar um webhook acionar [criar uma subscrição](#webhook-output---example), e pode [atualizar a subscrição](#webhook-subscription-refresh) para os impedir de expirar.

O *function.json* ficheiro define um acionador de webhook:

```json
{
  "bindings": [
    {
      "name": "msg",
      "type": "GraphWebhookTrigger",
      "direction": "in",
      "resourceType": "#Microsoft.Graph.Message"
    }
  ],
  "disabled": false
}
```

O código JavaScript reage a receber mensagens de correio e regista o corpo desses enviados pelo destinatário e que contêm "Das funções do Azure" no assunto:

```js
module.exports = function (context) {
    context.log("Microsoft Graph webhook trigger function processed a request.");
    const msg = context.bindings.msg
    // Testable by sending oneself an email with the subject "Azure Functions" and some text body
    if((msg.subject.indexOf("Azure Functions") > -1) && (msg.from === msg.sender) ) {
      context.log(`Processed email: ${msg.bodyPreview}`);
    }
    context.done();
};
```

### <a name="webhook-trigger---attributes"></a>Acionador de Webhook - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [GraphWebHookTrigger](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookTriggerAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="webhook-trigger---configuration"></a>Acionador de Webhook - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `GraphWebHookTrigger` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para a mensagem de correio. Consulte [através de uma mensagem do Outlook enlace a partir do código de saída](#outlook-output-code).|
|**tipo**||Necessário - tem de ser definido como `graphWebhook`.|
|**direção**||Necessário - tem de ser definido como `trigger`.|
|**resourceType**|**ResourceType**|Necessário - o recurso de gráfico para os quais esta função deve responder para webhooks. Pode ser um dos seguintes valores:<ul><li><code>#Microsoft.Graph.Message</code>-as alterações efetuadas às mensagens do Outlook.</li><li><code>#Microsoft.Graph.DriveItem</code>-as alterações efetuadas aos itens de raiz do OneDrive.</li><li><code>#Microsoft.Graph.Contact</code>-as alterações efetuadas ao pessoais contactos no Outlook.</li><li><code>#Microsoft.Graph.Event</code>-as alterações efetuadas aos itens de calendário do Outlook.</li></ul>|

> [!Note]
> Uma aplicação de função só pode ter uma função que está registada em relação a um determinado `resourceType` valor.

### <a name="webhook-trigger---usage"></a>Acionador de Webhook - utilização

O enlace expõe os seguintes tipos de funções de .NET:
- SDK do Microsoft Graph tipos relevantes para o tipo de recurso, tal como `Microsoft.Graph.Message` ou `Microsoft.Graph.DriveItem`.
- Tipos de objetos personalizados (utilizando o enlace do modelo estruturais)




<a name="webhook-input"></a>
## <a name="webhook-input"></a>Entrada de Webhook

O enlace de entrada de webhook Microsoft Graph permite-lhe obter a lista de subscrições geridos por esta aplicação de função. O enlace lê a partir do armazenamento de aplicação de função, pelo que não reflete outras subscrições criadas a partir de fora da aplicação.

Esta secção contém as seguintes subsecções:

* [Exemplo](#webhook-input---example)
* [Atributos](#webhook-input---attributes)
* [Configuração](#webhook-input---configuration)
* [Utilização](#webhook-input---usage)

### <a name="webhook-input---example"></a>Entrada de Webhook - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#webhook-input---c-script-example)
* [JavaScript](#webhook-input---javascript-example)

#### <a name="webhook-input---c-script-example"></a>Webhook de entrada - exemplo de script do c#

O exemplo seguinte obtém todas as subscrições para o utilizador chamada e elimina-los.

O *function.json* ficheiro define um acionador HTTP com um enlace de entrada de subscrição e uma saída de subscrição de enlace que utiliza a ação de eliminação:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O código de script do c# obtém as subscrições e elimina-los:

```csharp
using System.Net;

public static async Task Run(HttpRequest req, string[] existingSubscriptions, IAsyncCollector<string> subscriptionsToDelete, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");
    foreach (var subscription in existingSubscriptions)
    {
        log.Info($"Deleting subscription {subscription}");
        await subscriptionsToDelete.AddAsync(subscription);
    }
}
```

#### <a name="webhook-input---javascript-example"></a>Webhook de entrada - exemplo de JavaScript

O exemplo seguinte obtém todas as subscrições para o utilizador chamada e elimina-los.

O *function.json* ficheiro define um acionador HTTP com um enlace de entrada de subscrição e uma saída de subscrição de enlace que utiliza a ação de eliminação:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in",
      "filter": "userFromRequest"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToDelete",
      "direction": "out",
      "action": "delete",
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "res",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O código JavaScript obtém as subscrições e elimina-los:

```js
module.exports = function (context, req) {
    const existing = context.bindings.existingSubscriptions;
    var toDelete = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToDelete = toDelete;
    context.done();
};
```

### <a name="webhook-input---attributes"></a>Webhook de entrada - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="webhook-input---configuration"></a>Entrada de Webhook - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `GraphWebHookSubscription` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para a mensagem de correio. Consulte [através de uma mensagem do Outlook enlace a partir do código de saída](#outlook-output-code).|
|**tipo**||Necessário - tem de ser definido como `graphWebhookSubscription`.|
|**direção**||Necessário - tem de ser definido como `in`.|
|**filtro**|**Filtro**| Se definido como `userFromRequest`, em seguida, o enlace só irá obter subscrições pertencentes ao utilizador chamado (apenas válida com [acionador HTTP]).| 

### <a name="webhook-input---usage"></a>Webhook de entrada - utilização

O enlace expõe os seguintes tipos de funções de .NET:
- String]
- Matrizes do tipo de objeto personalizado
- Newtonsoft.Json.Linq.JObject []
- Microsoft.Graph.Subscription []





## <a name="webhook-output"></a>Saída do Webhook

A subscrição de webhook enlace de saída permite-lhe criar, eliminar e atualizar subscrições de webhook no Microsoft Graph.

Esta secção contém as seguintes subsecções:

* [Exemplo](#webhook-output---example)
* [Atributos](#webhook-output---attributes)
* [Configuração](#webhook-output---configuration)
* [Utilização](#webhook-output---usage)

### <a name="webhook-output---example"></a>Saída de Webhook - exemplo

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#webhook-output---c-script-example)
* [JavaScript](#webhook-output---javascript-example)

#### <a name="webhook-output---c-script-example"></a>Webhook de saída - exemplo de script do c#

O exemplo seguinte cria uma subscrição. Pode [atualizar a subscrição](#webhook-subscription-refresh) para os impedir de expirar.

O *function.json* ficheiro define um acionador HTTP com uma saída de subscrição utilizando a ação de criação de enlace:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphwebhook",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "listen": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O código de script do c# regista um webhook que irá notificar esta aplicação de função quando o utilizador chamado recebe uma mensagem do Outlook:

```csharp
using System;
using System.Net;

public static HttpResponseMessage run(HttpRequestMessage req, out string clientState, TraceWriter log)
{
  log.Info("C# HTTP trigger function processed a request.");
    clientState = Guid.NewGuid().ToString();
    return new HttpResponseMessage(HttpStatusCode.OK);
}
```

#### <a name="webhook-output---javascript-example"></a>Webhook de saída - exemplo de JavaScript

O exemplo seguinte cria uma subscrição. Pode [atualizar a subscrição](#webhook-subscription-refresh) para os impedir de expirar.

O *function.json* ficheiro define um acionador HTTP com uma saída de subscrição utilizando a ação de criação de enlace:

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "type": "graphwebhook",
      "name": "clientState",
      "direction": "out",
      "action": "create",
      "listen": "me/mailFolders('Inbox')/messages",
      "changeTypes": [
        "created"
      ],
      "identity": "userFromRequest"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

O código JavaScript regista um webhook que irá notificar esta aplicação de função quando o utilizador chamado recebe uma mensagem do Outlook:

```js
const uuidv4 = require('uuid/v4');

module.exports = function (context, req) {
    context.bindings.clientState = uuidv4();
    context.done();
};
```

### <a name="webhook-output---attributes"></a>Webhook de saída - atributos

No [bibliotecas de classes do c#](functions-dotnet-class-library.md), utilize o [GraphWebHookSubscription](https://github.com/Azure/azure-functions-microsoftgraph-extension/blob/master/src/MicrosoftGraphBinding/Bindings/GraphWebHookSubscriptionAttribute.cs) atributo, que está definido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MicrosoftGraph/).

### <a name="webhook-output---configuration"></a>Saída de Webhook - configuração

A tabela seguinte explica as propriedades de configuração de enlace que definir no *function.json* ficheiros e o `GraphWebHookSubscription` atributo.

|propriedade de Function.JSON | Propriedade de atributo |Descrição|
|---------|---------|----------------------|
|**nome**||Necessário - o nome da variável utilizado no código de função para a mensagem de correio. Consulte [através de uma mensagem do Outlook enlace a partir do código de saída](#outlook-output-code).|
|**tipo**||Necessário - tem de ser definido como `graphWebhookSubscription`.|
|**direção**||Necessário - tem de ser definido como `out`.|
|**identidade**|**Identidade**|Necessário - a identidade que será utilizada para executar a ação. Pode ser um dos seguintes valores:<ul><li><code>userFromRequest</code>-Só é válido com [acionador HTTP]. Utiliza a identidade do utilizador chamada.</li><li><code>userFromId</code>-Utiliza a identidade de um utilizador com sessão iniciada no anteriormente com o ID especificado. Consulte o <code>userId</code> propriedade.</li><li><code>userFromToken</code>-Utiliza a identidade representada pelo token especificado. Consulte o <code>userToken</code> propriedade.</li><li><code>clientCredentials</code>-Utiliza a identidade da aplicação de função.</li></ul>|
|**ID de utilizador**|**ID de utilizador**  |Se necessário e apenas se _identidade_ está definido como `userFromId`. ID principal de utilizador associado um utilizador anteriormente com sessão iniciada.|
|**userToken**|**UserToken**|Se necessário e apenas se _identidade_ está definido como `userFromToken`. Um token válido para a aplicação de função. |
|**ação**|**Ação**|É necessário - Especifica a ação de enlace deve efetuar. Pode ser um dos seguintes valores:<ul><li><code>create</code>-Regista uma nova subscrição.</li><li><code>delete</code>-Elimina uma subscrição especificada.</li><li><code>refresh</code>-Atualiza uma subscrição especificada para mantê-los de expirar.</li></ul>|
|**subscriptionResource**|**SubscriptionResource**|Se necessário e apenas se o _ação_ está definido como `create`. Especifica o recurso de Microsoft Graph que será monitorizado para as alterações. Consulte [trabalhar com webhooks no Microsoft Graph]. |
|**changeType**|**ChangeType**|Se necessário e apenas se o _ação_ está definido como `create`. Indica o tipo de alteração no recurso subscrito que irá emitir uma notificação. Os valores suportados são: `created`, `updated`, `deleted`. Podem ser combinados vários valores utilizando uma lista separada por vírgulas.|

### <a name="webhook-output---usage"></a>Webhook de saída - utilização

O enlace expõe os seguintes tipos de funções de .NET:
- string
- Microsoft.Graph.Subscription




<a name="webhook-examples"></a>
## <a name="webhook-subscription-refresh"></a>Atualização de subscrição do Webhook

Existem duas abordagens para atualizar as subscrições:

- Utilize a identidade da aplicação para lidar com todas as subscrições. Esta opção requer consentimento um administrador do Azure Active Directory. Isto pode ser utilizado por todos os idiomas suportados pelas funções do Azure.
- Utilizar a identidade associada a cada subscrição pelo enlace manualmente cada ID de utilizador. Isto irá exigir algum código personalizado para efetuar o enlace. Só pode ser utilizado pelas funções de .NET.

Esta secção contém um exemplo para cada uma destas abordagens:

* [Exemplo de identidade da aplicação](#webhook-subscription-refresh---app-identity-example)
* [Exemplo de identidade do utilizador](#webhook-subscription-refresh---user-identity-example)

### <a name="webhook-subscription-refresh---app-identity-example"></a>Atualização de subscrição de Webhook - exemplo de identidade da aplicação

Veja o exemplo de específicas do idioma:

* [Script do c# (.csx)](#app-identity-refresh---c-script-example)
* [JavaScript](#app-identity-refresh---javascript-example)

### <a name="app-identity-refresh---c-script-example"></a>Atualização de identidade de aplicação - exemplo de script do c#

O exemplo seguinte utiliza a identidade da aplicação para atualizar uma subscrição.

O *function.json* define um acionador de temporizador com uma subscrição de enlace de entrada e de uma subscrição de enlace de saída:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

O código de script do c# atualiza as subscrições:

```csharp
using System;

public static void Run(TimerInfo myTimer, string[] existingSubscriptions, ICollector<string> subscriptionsToRefresh, TraceWriter log)
{
    // This template uses application permissions and requires consent from an Azure Active Directory admin.
    // See https://go.microsoft.com/fwlink/?linkid=858780
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
      log.Info($"Refreshing subscription {subscription}");
      subscriptionsToRefresh.Add(subscription);
    }
}
```

### <a name="app-identity-refresh---c-script-example"></a>Atualização de identidade de aplicação - exemplo de script do c#

O exemplo seguinte utiliza a identidade da aplicação para atualizar uma subscrição.

O *function.json* define um acionador de temporizador com uma subscrição de enlace de entrada e de uma subscrição de enlace de saída:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "subscriptionsToRefresh",
      "direction": "out",
      "action": "refresh",
      "identity": "clientCredentials"
    }
  ],
  "disabled": false
}
```

O código JavaScript atualiza as subscrições:

```js
// This template uses application permissions and requires consent from an Azure Active Directory admin.
// See https://go.microsoft.com/fwlink/?linkid=858780

module.exports = function (context) {
    const existing = context.bindings.existingSubscriptions;
    var toRefresh = [];
    for (var i = 0; i < existing.length; i++) {
        context.log(`Deleting subscription ${existing[i]}`);
        todelete.push(existing[i]);
    }
    context.bindings.subscriptionsToRefresh = toRefresh;
    context.done();
};
```

### <a name="webhook-subscription-refresh---user-identity-example"></a>Atualização de subscrição de Webhook - exemplo de identidade do utilizador

O exemplo seguinte utiliza a identidade do utilizador para atualizar uma subscrição.

O *function.json* ficheiro define um acionador de temporizador e defers o enlace de entrada de subscrição para o código de função:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 * * */2 * *"
    },
    {
      "type": "graphWebhookSubscription",
      "name": "existingSubscriptions",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

O código de script do c# atualiza as subscrições e cria o enlace de saída no código, utilizando a identidade de cada utilizador:

```csharp
using System;

public static async Task Run(TimerInfo myTimer, UserSubscription[] existingSubscriptions, IBinder binder, TraceWriter log)
{
  log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    foreach (var subscription in existingSubscriptions)
    {
        // binding in code to allow dynamic identity
        using (var subscriptionsToRefresh = await binder.BindAsync<IAsyncCollector<string>>(
            new GraphWebhookSubscriptionAttribute() {
                Action = "refresh",
                Identity = "userFromId",
                UserId = subscription.UserId
            }
        ))
        {
            log.Info($"Refreshing subscription {subscription}");
            await subscriptionsToRefresh.AddAsync(subscription);
        }

    }
}

public class UserSubscription {
    public string UserId {get; set;}
    public string Id {get; set;}
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre as funções do Azure acionadores e enlaces](functions-triggers-bindings.md)

[acionador HTTP]: functions-bindings-http-webhook.md
[trabalhar com webhooks no Microsoft Graph]: https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/webhooks
