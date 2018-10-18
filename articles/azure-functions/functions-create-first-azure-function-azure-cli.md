---
title: Criar a sua primeira função com a CLI do Azure
description: Saiba como criar a sua primeira Função do Azure para execução sem servidor através da CLI do Azure e das Ferramentas Base das Funções do Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 09/10/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: ef5459b2b31b67afe187612ffc1ab079a5045a8c
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114915"
---
# <a name="create-your-first-function-from-the-command-line"></a>Criar a primeira função a partir da linha de comandos

Este tópico do guia de introdução mostra-lhe como criar a sua primeira função a partir da linha de comandos ou do terminal. Vai utilizar a CLI do Azure para criar uma aplicação de funções, que é a infraestrutura [sem servidor](https://azure.microsoft.com/solutions/serverless/) que aloja a sua função. O projeto de código de função é gerado a partir de um modelo com as [Ferramentas Base das Funções do Azure](functions-run-local.md), que também é utilizado para implementar o projeto de aplicação de funções no Azure.

Pode seguir os passos abaixo num computador Mac, Windows ou Linux.

## <a name="prerequisites"></a>Pré-requisitos

Antes de executar este exemplo, tem de ter o seguinte:

+ Instale a [versão 2.x das Ferramentas Base do Azure](functions-run-local.md#v2).

+ Instale a [CLI do Azure]( /cli/azure/install-azure-cli). Este artigo requer a versão 2.0 ou posterior da CLI do Azure. Execute `az --version` para localizar a versão atual. Pode também utilizar o [Azure Cloud Shell](https://shell.azure.com/bash).

+ Uma subscrição ativa do Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Criar o projeto de aplicação de funções local

Execute o seguinte comando na linha de comandos para criar um projeto de aplicação de funções na pasta `MyFunctionProj` do diretório local atual. Também é criado um repositório do GitHub em `MyFunctionProj`.

```bash
func init MyFunctionProj
```

Quando lhe for pedido, utilize as teclas de seta para selecionar um runtime de trabalho a partir das seguintes opções de linguagem:

+ `dotnet`: cria um projeto de biblioteca de classes .NET (.csproj).
+ `node`: cria um projeto do JavaScript.

Quando o comando for executado, verá algo parecido com o seguinte resultado:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

## <a name="create-a-function"></a>Criar uma função

O seguinte comando navega para o novo projeto e cria uma função acionada por HTTP com o nome `MyHtpTrigger`.

```bash
cd MyFunctionProj
func new --name MyHttpTrigger --template "HttpTrigger"
```

Quando o comando for executado, verá algo parecido com o seguinte resultado, que é uma função JavaScript:

```output
Writing C:\functions\MyFunctionProj\MyHttpTrigger\index.js
Writing C:\functions\MyFunctionProj\MyHttpTrigger\sample.dat
Writing C:\functions\MyFunctionProj\MyHttpTrigger\function.json
```

## <a name="edit-the-function"></a>Editar a função

Por predefinição, o modelo cria uma função que requer uma tecla de função ao efetuar pedidos. Para que seja mais fácil testar a função no Azure, terá de atualizar a função para permitir acesso anónimo. A forma como efetua esta alteração depende da sua linguagem de projeto de funções.

### <a name="c"></a>C\#

Abra o ficheiro de código MyHttpTrigger.cs que é a sua nova função e atualize o atributo **AuthorizationLevel** na definição de função para um valor de `anonymous` e guarde as alterações.

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
```

### <a name="javascript"></a>JavaScript

Abra o ficheiro function.json da sua nova função, abra-o num editor de texto, atualize a propriedade **authLevel** em **bindings.httpTrigger** para `anonymous` e guarde as alterações.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
```

Agora, pode chamar a função no Azure sem ter de indicar a tecla de função. A tecla de função nunca é necessária na execução local.

## <a name="run-the-function-locally"></a>Executar localmente a função

O comando seguinte inicia a aplicação de funções. A aplicação é executada com o mesmo runtime de Funções do Azure que está no Azure.

```bash
func host start --build
```

A opção `--build` é necessária para compilar projetos C#. Esta opção não é necessária para um projeto de JavaScript.

Quando o anfitrião de Funções começar, será apresentado algo semelhante ao resultado seguinte, que foi truncado para facilitar a leitura:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Copie o URL da função `HTTPTrigger` do resultado do runtime e cole-o na barra de endereço do browser. Anexe a cadeia de consulta `?name=<yourname>` a este URL e execute o pedido. O exemplo apresentado em seguida mostra a resposta no browser relativamente ao pedido GET devolvido retornado pela função local:

![Testar localmente no browser](./media/functions-create-first-azure-function-azure-cli/functions-test-local-browser.png)

Agora que executou a função localmente, pode criar a aplicação de funções e outros recursos necessários no Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>Criar uma aplicação de função

Precisa de uma aplicação Function App para alojar a execução das suas funções. A aplicação Function App proporciona um ambiente para a execução sem servidor do código da sua função. Permite-lhe agrupar funções como unidades lógicas para uma gestão, implementação e partilha de recursos mais fácil. Utilize o comando [az functionapp create](/cli/azure/functionapp#az-functionapp-create) para criar uma aplicação Function App. 

No comando seguinte, substitua o nome da sua aplicação de funções exclusivo onde vir o marcador de posição `<app_name>` e o nome da conta de armazenamento para `<storage_name>`. O `<app_name>` vai ser utilizado como o domínio DNS predefinido para a aplicação Function App, daí que o nome tenha de ser exclusivo em todas as aplicações no Azure. O parâmetro _deployment-source-url_ é um repositório de exemplo no GitHub que contém uma função acionada por HTTP "Hello World".

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <app_name> --storage-account  <storage_name>  
```

Definir o parâmetro _consumption-plan-location_ significa que a aplicação de funções é alojada num plano de alojamento de Consumo. Neste plano sem servidor, os recursos são adicionados dinamicamente conforme exigido pelas suas funções e paga apenas quando as funções estão em execução. Para obter mais informações, veja [Choose the correct hosting plan](functions-scale.md) (Escolher o plano de alojamento certo).

Depois de a aplicação Function App ter sido criada, a CLI do Azure mostra informações semelhantes às do exemplo seguinte:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

## <a name="configure-the-function-app"></a>Configurar a aplicação de funções

A versão 2.x das Ferramentas Base cria projetos com modelos para o runtime das Funções do Azure 2.x. Por este motivo, terá de confirmar que o runtime da versão 2.x é utilizado no Azure. Definir a configuração de aplicação `FUNCTIONS_WORKER_RUNTIME` para `~2` afixa a aplicação de funções à versão 2.x mais recente. Configure as definições da aplicação com o comando [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set).

No seguinte comando da CLI do Azure, <app_name> é o nome da sua aplicação de funções.

```azurecli-interactive
az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings FUNCTIONS_WORKER_RUNTIME=~2
```

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
