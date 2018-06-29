---
title: Trabalhar com as funções do Azure Core ferramentas | Microsoft Docs
description: Saiba como code e testar as funções do Azure a partir da linha de comandos ou terminal no seu computador local antes de executar em funções do Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/26/2018
ms.author: glenga
ms.openlocfilehash: 5c582b080ec6f2cff801758fc4bff4f7d07fd7df
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083074"
---
# <a name="work-with-azure-functions-core-tools"></a>Trabalhar com as funções do Azure, ferramentas de núcleo

Ferramentas de núcleos de funções do Azure permite-lhe desenvolver e testar as suas funções no seu computador local a partir da linha de comandos ou terminal. As suas funções locais podem ligar a serviços do Azure live e pode depurar as suas funções no seu computador local utilizando o tempo de execução de funções completo. Ainda pode implementar uma aplicação de função para a sua subscrição do Azure.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

## <a name="core-tools-versions"></a>Versões de ferramentas de núcleo

Existem duas versões das ferramentas de núcleos de funções do Azure. A versão que utilizar depende do seu ambiente de desenvolvimento local, a escolha de idioma e o nível de suporte necessário:

+ [Versão 1. x](#v1): suporta a versão 1. x do tempo de execução, o que é geralmente disponível (GA). Esta versão das ferramentas só é suportada em computadores com o Windows e está instalada uma [pacote npm](https://docs.npmjs.com/getting-started/what-is-npm). Com esta versão, pode criar funções experimental idiomas que não são suportados oficialmente. Para obter mais informações, consulte [idiomas suportados das funções do Azure](supported-languages.md)

+ [Versão 2. x](#v2): suporta a versão 2 do tempo de execução. Esta versão suporta [Windows](#windows-npm), [macOS](#brew), e [Linux](#linux). Utiliza os gestores de pacote específico da plataforma ou npm para instalação. Como o tempo de execução de 2. x, esta versão das ferramentas core está atualmente em pré-visualização.

Exceto indicação em contrário, os exemplos neste artigo destinam-se a versão 2. x.

## <a name="install-the-azure-functions-core-tools"></a>Instalar as Ferramentas de Núcleo de Funções do Azure

[Ferramentas de núcleos de funções do Azure] inclui uma versão do tempo de execução mesma que está na base de tempo de execução das funções do Azure que pode executar no seu computador de desenvolvimento local. Fornece também comandos para criar funções, ligar ao Azure e implementar projetos de função.

### <a name="v1"></a>Versão 1. x

A versão das ferramentas original utiliza o tempo de execução de 1. x de funções. Esta versão utiliza o .NET Framework (4.7.1) e só é suportada em computadores Windows. Antes de instalar as ferramentas de 1. x da versão, terá [instalar NodeJS](https://docs.npmjs.com/getting-started/installing-node), que inclui npm.

Utilize o seguinte comando para instalar as ferramentas de 1. x da versão:

```bash
npm install -g azure-functions-core-tools
```

### <a name="v2"></a>Versão 2. x

>[!NOTE]
> Tempo de execução funções do Azure 2.0 está em pré-visualização e, atualmente nem todas as funcionalidades das funções do Azure são suportadas. Para obter mais informações, consulte [versões das funções do Azure](functions-versions.md) 

Versão 2. x das ferramentas utiliza o tempo de execução das funções do Azure 2 que está incorporada no .NET Core. Esta versão é suportada em todas as plataformas suporta de 2. x .NET Core, incluindo [Windows](#windows-npm), [macOS](#brew), e [Linux](#linux).

#### <a name="windows-npm"></a>Windows

Os seguintes passos utilizem npm para instalar as ferramentas de núcleos no Windows. Também pode utilizar [Chocolatey](https://chocolatey.org/). Para obter mais informações, consulte o [Leia-me de ferramentas de núcleos](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Instalar [2.0 do .NET Core para o Windows](https://www.microsoft.com/net/download/windows).

2. Instalar [Node.js], que inclui npm. Para a versão 2. x das ferramentas, apenas Node.js 8.5 e versões posteriores são suportados.

3. Instale o pacote de ferramentas de núcleo:

    ```bash
    npm install -g azure-functions-core-tools@core
    ```

#### <a name="brew"></a>MacOS com Homebrew

Os passos seguintes utilizam Homebrew para instalar as ferramentas de núcleos no macOS.

1. Instalar [.NET Core 2.0 para macOS](https://www.microsoft.com/net/download/macos).

2. Instalar [Homebrew](https://brew.sh/), se ainda não estiver instalado.

3. Instale o pacote de ferramentas de núcleo:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools 
    ```

#### <a name="linux"></a> Linux (Ubuntu/Debian) com APT

Os passos seguintes utilize [APT](https://wiki.debian.org/Apt) para instalar ferramentas de núcleos na sua distribuição Ubuntu/Debian Linux. Para outras as distribuições do Linux, consulte o [Leia-me de ferramentas de núcleos](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Instalar [2.0 do .NET Core para Linux](https://www.microsoft.com/net/download/linux).

2. Registe a chave de produto da Microsoft como fidedigna:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

3. Certifique-se de que o seu servidor Ubuntu executar uma das versões adequadas da tabela abaixo. Para adicionar a origem apt, execute:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Distribuição de Linux | Versão |
    | --------------- | ----------- |
    | Ubuntu 17.10    | `artful`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux Mint 18    | `xenial`  |

4. Instale o pacote de ferramentas de núcleo:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

## <a name="create-a-local-functions-project"></a>Criar um projeto de funções local

Um diretório de projeto funções contém os ficheiros [host.json](functions-host-json.md) e [local.settings.json](#local-settings-file), ao longo de subpastas que contêm o código para funções individuais. Este diretório é o equivalente a uma aplicação de função no Azure. Para saber mais sobre a estrutura de pastas de funções, consulte o [guia de programadores das funções do Azure](functions-reference.md#folder-structure).

Versão 2. x necessita de selecionar um idioma predefinido para o seu projeto quando é inicializado e todas as funções adicionadas modelos de idiomas predefinidos de utilização. Na versão 1. x, especificar a linguagem de cada vez que criar uma função.

Na janela de terminal ou numa linha de comandos, execute o seguinte comando para criar o projeto e o repositório de Git local:

```bash
func init MyFunctionProj
```

Versão 2, quando executar o comando tem de escolher um tempo de execução para o seu projeto. Se planeia desenvolver funções JavaScript, escolha **nó**:

```output
Select a worker runtime:
dotnet
node
```

Utilize a cópia de segurança/para baixo teclas de seta para escolher um idioma, em seguida, prima Enter. Procura o resultado semelhante ao seguinte exemplo para um projeto de JavaScript:

```output
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
```

Para criar o projeto sem um repositório de Git local, utilize o `--no-source-control [-n]` opção.

## <a name="register-extensions"></a>Registar extensões

Versão 2 do tempo de execução das funções do Azure, tem de registar explicitamente as extensões de enlace (tipos de enlace) que utiliza na sua aplicação de função.

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

Para obter mais informações, consulte [acionadores de funções do Azure e conceitos de enlaces](functions-triggers-bindings.md#register-binding-extensions).

## <a name="local-settings-file"></a>Ficheiro de definições do local

O ficheiro local.settings.json armazena as definições de aplicação, cadeias de ligação e definições de ferramentas de núcleos de funções do Azure. Tem a estrutura seguinte:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*"
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```

| Definição      | Descrição                            |
| ------------ | -------------------------------------- |
| **IsEncrypted** | Quando definido como **verdadeiro**, todos os valores são encriptados utilizando uma chave de computador local. Utilizado com `func settings` comandos. Valor predefinido é **falso**. |
| **Valores** | Coleção de definições da aplicação e cadeias de ligação utilizadas ao executar localmente. Estes valores correspondem às definições de aplicação na sua aplicação de função no Azure, tais como **AzureWebJobsStorage** e **AzureWebJobsDashboard**. Muitos acionadores e enlaces tem uma propriedade que se refere a uma definição de aplicação de cadeia de ligação, tal como **ligação** para o [acionador de armazenamento de BLOBs](functions-bindings-storage-blob.md#trigger---configuration). Para essas propriedades, precisa de uma definição de aplicação definida no **valores** matriz. <br/>**AzureWebJobsStorage** é uma definição de aplicação necessária para acionadores diferentes de HTTP. Se tiver o [emulador do storage do Azure](../storage/common/storage-use-emulator.md) instalado localmente, pode definir **AzureWebJobsStorage** para `UseDevelopmentStorage=true` e ferramentas principais utiliza o emulador. Isto é útil durante o desenvolvimento, mas deverá testar com uma ligação de armazenamento real antes da implementação. |
| **Anfitrião** | As definições nesta secção personalizar o processo de anfitrião de funções ao executar localmente. |
| **LocalHttpPort** | Define a porta predefinida utilizada ao executar o anfitrião de funções local (`func host start` e `func run`). O `--port` opção da linha de comandos tem precedência sobre este valor. |
| **CORS** | Define as origens permitidas para [recursos de várias origens (CORS) de partilha](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Origens são fornecidas como uma lista de valores separados por vírgulas de mensagens em fila sem espaços. O valor de caráter universal (\*) é suportada, que permite que os pedidos de qualquer origem. |
| **ConnectionStrings** | Utilize esta coleção para as cadeias de ligação utilizadas pelo seu enlaces de funções. Esta coleção só é utilizada pelo estruturas que tem de obter as cadeias de ligação do **ConnectionStrings** secção de configuração de uma ficheiro, tal como [do Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Cadeias de ligação neste objeto são adicionadas para o ambiente com o tipo de fornecedor de [SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Itens desta coleção não são publicadas no Azure com outras definições de aplicação. Tem de adicionar explicitamente estes valores para o **cadeias de ligação** secção o **definições da aplicação** para a sua aplicação de função. |

Os valores de definições de aplicação de função também podem ser lidos no seu código como variáveis de ambiente. Para obter mais informações, consulte a secção de variáveis de ambiente destes tópicos de referência de específicas do idioma:

+ [Pré-compilada com c#](functions-dotnet-class-library.md#environment-variables)
+ [Script do c# (.csx)](functions-reference-csharp.md#environment-variables)
+ [F#](functions-reference-fsharp.md#environment-variables)
+ [Java](functions-reference-java.md#environment-variables) 
+ [JavaScript](functions-reference-node.md#environment-variables)

As definições no ficheiro local.settings.json apenas são utilizadas pelas ferramentas de funções ao executar localmente. Por predefinição, estas definições não são migradas automaticamente quando o projeto é publicado para o Azure. Utilize o `--publish-local-settings` comutador [quando publica](#publish) para se certificar de que estas definições são adicionadas para a aplicação de função no Azure. Os valores no **ConnectionStrings** nunca são publicadas.

Quando nenhuma cadeia de ligação de armazenamento válido está definida para **AzureWebJobsStorage** e não está a ser utilizado o emulador, é apresentada a seguinte mensagem de erro:  

>Falta o valor para AzureWebJobsStorage no local.settings.json. Isto é necessário para todos os acionadores diferentes de HTTP. Pode executar ' func azure functionapp obtenção--as definições de aplicação <functionAppName>' ou especifique uma cadeia de ligação na local.settings.json.

### <a name="get-your-storage-connection-strings"></a>Obter as cadeias de ligação de armazenamento

Mesmo quando utilizar o emulador do storage para o desenvolvimento, poderá ser útil de teste com uma ligação de armazenamento real. Partindo do princípio de que já tem [criou uma conta de armazenamento](../storage/common/storage-create-storage-account.md), pode obter uma cadeia de ligação de armazenamento válida de uma das seguintes formas:

+ Do [portal do Azure]. Navegue até à sua conta de armazenamento, selecione **chaves de acesso** no **definições**, em seguida, copie um do **cadeia de ligação** valores.

  ![Copie a cadeia de ligação do portal do Azure](./media/functions-run-local/copy-storage-connection-portal.png)

+ Utilize [Explorador de armazenamento do Azure](http://storageexplorer.com/) para ligar à sua conta do Azure. No **Explorer**, expanda a sua subscrição, selecione a sua conta de armazenamento e copie a cadeia de ligação primária ou secundária. 

  ![Copie a cadeia de ligação a partir do Explorador de armazenamento](./media/functions-run-local/storage-explorer.png)

+ Utilize ferramentas de núcleos para transferir a cadeia de ligação a partir do Azure com um dos seguintes comandos:

    + Transferir todas as definições a partir de uma aplicação de função existente:

    ```bash
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    + Obter a cadeia de ligação para uma conta de armazenamento específico:

    ```bash
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    
    Quando lhe não já têm sessão iniciada Azure, lhe for pedido para fazê-lo.

## <a name="create-func"></a>Criar uma função

Para criar uma função, execute o seguinte comando:

```bash
func new
```

Versão 2, quando executa `func new` lhe for pedido para escolher um modelo no idioma predefinido da sua aplicação de função, em seguida, também lhe para escolher um nome para a sua função. Na versão 1. x, também lhe para escolher o idioma.

```output
Select a language: Select a template:
Blob trigger
Cosmos DB trigger
Event Grid trigger
HTTP trigger
Queue trigger
SendGrid
Service Bus Queue trigger
Service Bus Topic trigger
Timer trigger
```

Código de função é gerado numa subpasta com o nome de função fornecidos, como pode ver no resultado de Acionador de fila do seguinte:

```output
Select a language: Select a template: Queue trigger
Function name: [QueueTriggerJS] MyQueueTrigger
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\index.js
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\readme.md
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\sample.dat
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\function.json
```

Também pode especificar estas opções no comando utilizando os argumentos seguintes:

| Argumento     | Descrição                            |
| ------------------------------------------ | -------------------------------------- |
| **`--language -l`**| O modelo de programação idioma, como c#, F # ou JavaScript. Esta opção é necessária na versão 1. x. Versão 2, não utilize esta opção ou escolha o idioma predefinido do seu projeto. |
| **`--template -t`** | O nome do modelo, que pode ser um dos valores:<br/><ul><li>`Blob trigger`</li><li>`Cosmos DB trigger`</li><li>`Event Grid trigger`</li><li>`HTTP trigger`</li><li>`Queue trigger`</li><li>`SendGrid`</li><li>`Service Bus Queue trigger`</li><li>`Service Bus Topic trigger`</li><li>`Timer trigger`</li></ul> |
| **`--name -n`** | O nome da função. |

Por exemplo, para criar um acionador de HTTP de JavaScript num comando único, execute:

```bash
func new --template "Http Trigger" --name MyHttpTrigger
```

Para criar uma função acionada pela fila de um comando único, execute:

```bash
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="start"></a>Executar funções localmente

Para executar um projeto de funções, execute o anfitrião de funções. O anfitrião permite acionadores para todas as funções no projeto:

```bash
func host start
```

`func host start` suporta as seguintes opções:

| Opção     | Descrição                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | A porta local a escutar. Valor predefinido: 7071. |
| **`--debug <type>`** | Inicia o anfitrião com a porta de depuração abrir, de modo a que pode anexar ao **func.exe** processos de [Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started) ou [Visual Studio 2017](functions-dotnet-class-library.md). O *\<tipo\>* opções são `VSCode` e `VS`.  |
| **`--cors`** | Uma lista separada por vírgulas de origens CORS, sem espaços. |
| **`--nodeDebugPort -n`** | A porta para o depurador do nó utilizar. : Um valor predefinido De launch.json ou 5858. |
| **`--debugLevel -d`** | O nível de rastreio de consola (desativado, verboso, info, warning ou error). Predefinição: as informações.|
| **`--timeout -t`** | O tempo limite para o anfitrião de funções iniciar, em segundos. Predefinição: 20 segundos.|
| **`--useHttps`** | Vincular ao `https://localhost:{port}` em vez da `http://localhost:{port}`. Por predefinição, esta opção cria um certificado fidedigno no computador.|
| **`--pause-on-error`** | Colocar em pausa para a entrada adicional antes de sair do processo. Utilizado quando iniciar ferramentas de núcleos a partir do Visual Studio ou o VS Code.|

Quando o anfitrião de funções é iniciado, produz as funções acionadas por URL de HTTP:

```bash
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="passing-test-data-to-a-function"></a>Transmissão de dados de teste para uma função

Para testar as suas funções localmente, [iniciar o anfitrião de funções](#start) e chame pontos finais no servidor local através de pedidos de HTTP. O ponto final que tem de chamar depende da função.

>[!NOTE]  
> Os exemplos neste tópico utilizam a ferramenta de cURL para enviar pedidos de HTTP de terminal ou uma linha de comandos. Pode utilizar uma ferramenta de sua escolha para enviar pedidos HTTP para o servidor local. A ferramenta de cURL está disponível por predefinição em sistemas baseados em Linux. No Windows, tem primeiro de transferir e instalar o [cURL ferramenta](https://curl.haxx.se/).

Para obter informações mais gerais sobre testar as funções, consulte [estratégias para testar o seu código das funções do Azure](functions-test-a-function.md).

#### <a name="http-and-webhook-triggered-functions"></a>HTTP e webhook acionada funções

Chamar o seguinte ponto final para executar localmente HTTP e webhook acionada funções:

    http://localhost:{port}/api/{function_name}

Certifique-se utilizar o mesmo nome de servidor e a porta que o anfitrião de funções está a escutar. Pode ver na saída gerada ao iniciar o anfitrião de função. Pode ligar a este URL com qualquer método HTTP suportado pelo acionador. 

O seguinte cURL acionadores de comando de `MyHttpTrigger` função de início rápido de um pedido GET com o _nome_ parâmetro transmitido na cadeia de consulta. 

```bash
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
O exemplo seguinte é a mesma função chamada a partir de um pedido POST transmitir _nome_ no corpo do pedido:

```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

Pode efetuar pedidos de obtenção de um browser passar dados na cadeia de consulta. Para todos os outros métodos HTTP, tem de utilizar cURL, Fiddler, Postman ou uma ferramenta de teste HTTP semelhante.  

#### <a name="non-http-triggered-functions"></a>Não HTTP acionada funções

Para todos os tipos de funções que não sejam acionadores HTTP e webhooks, pode testar as suas funções localmente ao chamar um ponto final de administração. Este ponto final com um pedido POST de HTTP ao chamar no servidor local aciona a função. Opcionalmente, pode passar dados de teste para a execução no corpo do pedido POST. Esta funcionalidade é semelhante para o **teste** separador no portal do Azure.  

Chamar o seguinte ponto final do administrador para acionar não HTTP funções:

    http://localhost:{port}/admin/functions/{function_name}

Para transmitir dados de teste para o ponto final de administrador de uma função, tem de fornecer os dados no corpo de uma mensagem de pedido POST. O corpo da mensagem é necessário ter o seguinte formato JSON:

```JSON
{
    "input": "<trigger_input>"
}
````

O `<trigger_input>` valor contém dados num formato esperado pela função. O seguinte exemplo de cURL é um pedido POST para um `QueueTriggerJS` função. Neste caso, a entrada é uma cadeia que é equivalente à mensagem deve ser encontrada na fila.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Utilizar o `func run` comando versão 1. x

>[!IMPORTANT]  
> O `func run` comando não é suportado na versão 2. x das ferramentas. Para obter mais informações, consulte o tópico [como destino a versões de tempo de execução das funções do Azure](set-runtime-version.md).

Pode também invocar uma função utilizando diretamente `func run <FunctionName>` e fornecer dados de entrada para a função. Este comando é semelhante à execução de uma função utilizando o **teste** separador no portal do Azure. 

`func run` suporta as seguintes opções:

| Opção     | Descrição                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Conteúdo de inline. |
| **`--debug -d`** | Anexe um depurador para o processo de anfitrião antes de executar a função.|
| **`--timeout -t`** | Tempo de espera (em segundos) até que o anfitrião de funções local está pronto.|
| **`--file -f`** | O nome de ficheiro a utilizar como o conteúdo.|
| **`--no-interactive`** | Não solicita de entrada. É útil para cenários de automatização.|

Por exemplo, para chamar uma função acionada por HTTP e passar o corpo de conteúdo, execute o seguinte comando:

```bash
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

### <a name="viewing-log-files-locally"></a>Ver ficheiros de registo localmente

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="publish"></a>Publicar no Azure

Para publicar um projeto de funções para uma aplicação de função no Azure, utilize o `publish` comando:

```bash
func azure functionapp publish <FunctionAppName>
```

Pode utilizar as seguintes opções:

| Opção     | Descrição                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Definições de publicação local.settings.json para o Azure, que lhe pede para substituir se a definição já existe. Se estiver a utilizar o emulador do storage, altere a definição de aplicação para um [ligação de armazenamento real](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | Tem de ser utilizado com `-i`. Substitui AppSettings no Azure com o valor local, se for diferente. Predefinição é de linha de comandos.|

Este comando publica uma aplicação de função existente no Azure. Ocorre um erro quando o `<FunctionAppName>` não existe na sua subscrição. Para saber como criar uma aplicação de função a partir da linha de comandos ou janela de terminal utilizando a CLI do Azure, consulte [criar uma aplicação de função para execução sem servidor](./scripts/functions-cli-create-serverless.md).

O `publish` comando carrega o conteúdo do diretório do projeto de funções. Se eliminar os ficheiros localmente, a `publish` comando não eliminá-los a partir do Azure. Pode eliminar ficheiros no Azure utilizando o [ferramenta Kudu](functions-how-to-use-azure-function-app-settings.md#kudu) no [portal do Azure].  

>[!IMPORTANT]  
> Quando cria uma aplicação de função no Azure, este utilizará a versão 1. x do tempo de execução de função, por predefinição. Para tornar a versão de utilização da aplicação de função 2 do tempo de execução, adicione a definição de aplicação `FUNCTIONS_EXTENSION_VERSION=beta`.  
Utilize o seguinte código para a CLI do Azure para adicionar esta definição para a sua aplicação de função:

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings FUNCTIONS_EXTENSION_VERSION=beta   
```

## <a name="next-steps"></a>Passos Seguintes

Ferramentas de núcleos de funções do Azure é [abrir a origem e está lojado no GitHub](https://github.com/azure/azure-functions-cli).  
No ficheiro de um pedido de erros ou funcionalidade, [abrir um problema no GitHub](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Ferramentas de núcleos de funções do Azure]: https://www.npmjs.com/package/azure-functions-core-tools
[Portal do Azure]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
