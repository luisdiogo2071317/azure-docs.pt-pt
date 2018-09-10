---
title: Trabalhar com as funções do Azure, as ferramentas de núcleo | Documentos da Microsoft
description: Aprenda a codificar e testar as funções do Azure a partir da linha de comandos ou terminal no seu computador local antes de executá-las nas funções do Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/14/2018
ms.author: glenga
ms.openlocfilehash: 294dea695f120ff47712e0ccf79072a41baca07b
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092823"
---
# <a name="work-with-azure-functions-core-tools"></a>Trabalhar com as funções do Azure, as ferramentas de núcleo

As ferramentas de núcleo das funções do Azure permite-lhe desenvolver e testar as suas funções no seu computador local a partir da linha de comandos ou terminal. As suas funções locais podem ligar-se ao vivo de serviços do Azure e pode depurar as suas funções no seu computador local com o tempo de execução de funções completo. Pode até mesmo implementar uma aplicação de funções para a sua subscrição do Azure.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

## <a name="core-tools-versions"></a>Versões de ferramentas de núcleo

Existem duas versões de ferramentas de núcleo de funções do Azure. A versão que utilizar depende do seu ambiente de desenvolvimento local, a escolha de idioma e o nível de suporte necessário:

+ [Versão 1.x](#v1): suporta a versão 1.x do runtime, o que está geralmente disponível (GA). Esta versão das ferramentas só é suportado em computadores Windows e é instalado a partir um [pacote npm](https://docs.npmjs.com/getting-started/what-is-npm). Com esta versão, pode criar funções em linguagens experimentais que não são suportadas oficialmente. Para obter mais informações, consulte [idiomas suportados nas funções do Azure](supported-languages.md)

+ [Versão 2.x](#v2): suporta [versão 2.x do runtime](functions-versions.md). Esta versão suporta [Windows](#windows-npm), [macOS](#brew), e [Linux](#linux). Utiliza os gestores de pacotes específicos da plataforma ou npm para instalação. Como o runtime 2.x, esta versão das ferramentas de núcleo está atualmente em pré-visualização. 

Salvo indicação em contrário, os exemplos neste artigo são para a versão 2.x. Para receber atualizações importantes na versão 2.x, incluindo a última hora altera anúncios, veja a [anúncios de serviço de aplicações do Azure](https://github.com/Azure/app-service-announcements/issues) repositório.

## <a name="install-the-azure-functions-core-tools"></a>Instalar as Ferramentas de Núcleo de Funções do Azure

[Ferramentas de núcleo das funções do Azure] inclui uma versão do runtime mesmo que alimenta o tempo de execução de funções do Azure que pode executar no seu computador de desenvolvimento local. Ele também fornece comandos para criar funções, ligar para o Azure e implementar projetos de função.

### <a name="v1"></a>Versão 1.x

A versão original das ferramentas utiliza o runtime 1.x de funções. Esta versão utiliza o .NET Framework (4.7.1) e só é suportada em computadores Windows. Antes de instalar as ferramentas do versão 1.x, deve [instalar o NodeJS](https://docs.npmjs.com/getting-started/installing-node), que inclui o npm.

Utilize o seguinte comando para instalar as ferramentas de 1.x da versão:

```bash
npm install -g azure-functions-core-tools
```

### <a name="v2"></a>Versão 2.x

>[!NOTE]
> O runtime das funções do Azure 2.0 está em pré-visualização e são suportadas atualmente nem todas as funcionalidades das funções do Azure. Para obter mais informações, consulte [versões de funções do Azure](functions-versions.md) 

Versão 2.x das ferramentas utiliza o runtime das funções do Azure 2.x está incorporada no .NET Core. Esta versão é suportada em todas as plataformas suporta de 2.x do .NET Core, incluindo [Windows](#windows-npm), [macOS](#brew), e [Linux](#linux).

#### <a name="windows-npm"></a>Windows

Os seguintes passos utilizam npm para instalar as ferramentas de núcleo no Windows. Também pode utilizar [Chocolatey](https://chocolatey.org/). Para obter mais informações, consulte a [Leiame de ferramentas de núcleo](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Instale [2.1 do .NET Core para Windows](https://www.microsoft.com/net/download/windows).

2. Instale [Node.js], que inclui o npm. Para a versão 2.x das ferramentas, apenas 8.5 de node. js e versões posteriores são suportados.

3. Instale o pacote de ferramentas de núcleo:

    ```bash
    npm install -g azure-functions-core-tools@core
    ```

#### <a name="brew"></a>MacOS com o Homebrew

Os passos seguintes utilizam o Homebrew para instalar as ferramentas de núcleo no macOS.

1. Instale [.NET Core 2.1 para macOS](https://www.microsoft.com/net/download/macos).

2. Instale [Homebrew](https://brew.sh/), se ainda não estiver instalado.

3. Instale o pacote de ferramentas de núcleo:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools 
    ```

#### <a name="linux"></a> Linux (Ubuntu/Debian) com APT

Os passos seguintes utilizam [APT](https://wiki.debian.org/Apt) para instalar as ferramentas de núcleo na sua distribuição Ubuntu/Debian Linux. Para outras distribuições do Linux, consulte a [Leiame de ferramentas de núcleo](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Instale [2.1 do .NET Core para Linux](https://www.microsoft.com/net/download/linux).

2. Registe-se a chave de produto da Microsoft como fidedigna:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

3. Certifique-se de que o servidor Ubuntu está em execução uma das versões apropriadas da tabela abaixo. Para adicionar a origem de apt, execute:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Distribuição do Linux | Versão |
    | --------------- | ----------- |
    | Ubuntu 17.10    | `artful`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux e menta 18    | `xenial`  |

4. Instale o pacote de ferramentas de núcleo:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

## <a name="create-a-local-functions-project"></a>Criar um projeto de funções local

Um diretório de projeto de funções contém os ficheiros [Host. JSON](functions-host-json.md) e [Settings](#local-settings-file), juntamente com as subpastas que contêm o código para funções individuais. Este diretório é o equivalente a uma aplicação de funções no Azure. Para saber mais sobre a estrutura de pastas de funções, consulte a [guia de programadores do funções do Azure](functions-reference.md#folder-structure).

Versão 2.x exige que selecione um idioma padrão do seu projeto, quando é inicializado, e todas as funções adicionadas a modelos de linguagem de padrão de utilização. Na versão 1.x, especificar o idioma de cada vez que criar uma função.

Na janela de terminal ou a partir de um prompt de comando, execute o seguinte comando para criar o projeto e o repositório de Git local:

```bash
func init MyFunctionProj
```

Quando fornecer um nome de projeto, uma nova pasta com esse nome é criada e inicializada. Caso contrário, a pasta atual é inicializada.  
Na versão 2.x, quando executar o comando tem de escolher um tempo de execução para o seu projeto. Se planeja desenvolver funções de JavaScript, escolha **nó**:

```output
Select a worker runtime:
dotnet
node
```

Utilizar a cópia de segurança/para baixo de teclas de seta para selecionar um idioma, em seguida, prima Enter. O resultado tem um aspeto semelhante ao seguinte exemplo para um projeto do JavaScript:

```output
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
```

Para criar o projeto sem um repositório de Git local, utilize o `--no-source-control [-n]` opção.

> [!IMPORTANT]
> Por predefinição, versão 2.x das ferramentas de núcleo cria função os projetos de aplicativos para o tempo de execução do .NET como [projetos de classe c#](functions-dotnet-class-library.md) (arquivo. csproj). Esses projetos do c#, que podem ser utilizados com o Visual Studio 2017 ou Visual Studio Code, são compilados durante o teste e ao publicar no Azure. Se pretender em vez disso, criar e trabalhar com o mesmo script c# (. csx) ficheiros criados na versão 1.x e no portal, tem de incluir o `--csx` parâmetro ao criar e implementar as funções.

## <a name="register-extensions"></a>Registe-se as extensões

Na versão 2.x do runtime das funções do Azure, precisa registrar explicitamente as extensões de vinculação (tipos de ligação) que utiliza na sua aplicação de função.

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

Para obter mais informações, consulte [acionadores de funções do Azure e conceitos de enlaces](functions-triggers-bindings.md#register-binding-extensions).

## <a name="local-settings-file"></a>Arquivo de configurações local

O ficheiro Settings armazena as definições da aplicação, as cadeias de ligação e as definições para as ferramentas de núcleo de funções do Azure. Ele tem a seguinte estrutura:

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
    "SQLConnectionString": "<sqlclient-connection-string>"
  }
}
```

| Definição      | Descrição                            |
| ------------ | -------------------------------------- |
| **IsEncrypted** | Quando definido como **true**, todos os valores são criptografados usando uma chave de computador local. Utilizado com `func settings` comandos. Valor predefinido é **false**. |
| **Valores** | Coleção de definições da aplicação e as cadeias de ligação utilizadas ao executar localmente. Esses valores correspondem às definições de aplicação na sua aplicação de função no Azure, tal como **AzureWebJobsStorage** e **AzureWebJobsDashboard**. Muitos acionadores e enlaces de ter uma propriedade que se refere a uma definição de aplicação de cadeia de ligação, tal como **conexão** para o [acionador do armazenamento de BLOBs](functions-bindings-storage-blob.md#trigger---configuration). Para essas propriedades, precisa de uma definição da aplicação definida no **valores** matriz. <br/>**AzureWebJobsStorage** é uma definição de aplicação necessária para acionadores que não seja o HTTP. Quando tem o [emulador de armazenamento do Azure](../storage/common/storage-use-emulator.md) instalados localmente, pode definir **AzureWebJobsStorage** para `UseDevelopmentStorage=true` e ferramentas de núcleo utiliza o emulador. Isto é útil durante o desenvolvimento, mas deve testar com uma ligação de armazenamento real antes da implantação. |
| **Anfitrião** | As definições nesta secção personalizar o processo de host de funções ao executar localmente. |
| **LocalHttpPort** | Define a porta predefinida utilizada ao executar o anfitrião local de funções (`func host start` e `func run`). O `--port` opção da linha de comandos tem precedência sobre este valor. |
| **CORS** | Define as origens permitidas para [recursos de várias origens (CORS) de partilha](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Origens são fornecidas como uma lista separada por vírgulas, sem espaços. O valor de caráter universal (\*) é suportado, que permite que os pedidos a partir de qualquer origem. |
| **ConnectionStrings** | Não utilize esta coleção para as cadeias de ligação utilizadas pelo seu enlaces de funções. Esta coleção só é utilizada por estruturas que normalmente obtém cadeias de ligação do **ConnectionStrings** secção de uma configuração de ficheiro, tal como [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Cadeias de ligação desse objeto são adicionadas ao ambiente com o tipo de fornecedor de [SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Itens dessa coleção não são publicadas no Azure com outras definições de aplicação. Tem de adicionar explicitamente esses valores para o **cadeias de ligação** coleção das definições de aplicação de função. Se estiver a criar uma [SqlConnection](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(v=vs.110).aspx) no código da função, deve armazenar o valor da cadeia de ligação **configurações de aplicativo** com as outras ligações. |

Os valores de definições de aplicação de função também podem ser lidos em seu código como variáveis de ambiente. Para obter mais informações, consulte a secção de variáveis de ambiente destes tópicos de referência de idioma específico:

+ [C# pré-compiladas](functions-dotnet-class-library.md#environment-variables)
+ [Script do c# (.csx)](functions-reference-csharp.md#environment-variables)
+ [F#](functions-reference-fsharp.md#environment-variables)
+ [Java](functions-reference-java.md#environment-variables) 
+ [JavaScript](functions-reference-node.md#environment-variables)

Definições no arquivo Settings só são utilizadas pelas ferramentas de funções ao executar localmente. Por predefinição, estas definições não são migradas automaticamente quando o projeto é publicado para o Azure. Utilize o `--publish-local-settings` mude [ao publicar](#publish) para se certificar de que estas definições são adicionadas à aplicação de funções no Azure. Os valores na **ConnectionStrings** nunca são publicados.

Quando nenhuma cadeia de ligação de armazenamento válida é definida para **AzureWebJobsStorage** e o emulador não está a ser utilizado, é apresentada a seguinte mensagem de erro:  

>Valor em falta para AzureWebJobsStorage em Settings. Isto é necessário para todos os disparadores diferente de HTTP. É possível executar ' func azure functionapp busca--definições da aplicação <functionAppName>' ou especifique uma cadeia de ligação no Settings.

### <a name="get-your-storage-connection-strings"></a>Obter as cadeias de ligação de armazenamento

Mesmo ao usar o emulador de armazenamento para o desenvolvimento, talvez queira testar com uma ligação de armazenamento real. Partindo do princípio de que já tiver [criou uma conta de armazenamento](../storage/common/storage-create-storage-account.md), pode obter uma cadeia de ligação de armazenamento válido de uma das seguintes formas:

+ Partir do [portal do Azure]. Navegue até à sua conta de armazenamento, selecione **chaves de acesso** na **definições**, em seguida, copie uma do **cadeia de ligação** valores.

  ![Copie a cadeia de ligação do portal do Azure](./media/functions-run-local/copy-storage-connection-portal.png)

+ Uso [Explorador de armazenamento do Azure](http://storageexplorer.com/) para ligar à sua conta do Azure. Na **Explorer**, expanda a sua subscrição, selecione a sua conta de armazenamento e copie a cadeia de ligação primária ou secundária. 

  ![Copie a cadeia de ligação do Explorador de armazenamento](./media/functions-run-local/storage-explorer.png)

+ Utilize as ferramentas de núcleo para transferir a cadeia de ligação do Azure com um dos seguintes comandos:

    + Transferir todas as definições a partir de uma aplicação de função existente:

    ```bash
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    + Obter a cadeia de ligação para uma conta de armazenamento específica:

    ```bash
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    
    Quando não já têm sessão iniciada Azure, lhe for pedido para fazer isso.

## <a name="create-func"></a>Criar uma função

Para criar uma função, execute o seguinte comando:

```bash
func new
```

Na versão 2.x, quando executa `func new` lhe for pedido para escolher um modelo no idioma padrão da sua aplicação de função, em seguida, também deve escolher um nome para a sua função. Na versão 1.x, também deve escolher o idioma.

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

Código de função é gerado numa subpasta com o nome da função fornecido, como pode ver na seguinte saída de Acionador de fila:

```output
Select a language: Select a template: Queue trigger
Function name: [QueueTriggerJS] MyQueueTrigger
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\index.js
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\readme.md
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\sample.dat
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\function.json
```

Também pode especificar estas opções no comando utilizando os argumentos a seguir:

| Argumento     | Descrição                            |
| ------------------------------------------ | -------------------------------------- |
| **`--language -l`**| O modelo de programação de linguagem, como c#, F # ou JavaScript. Esta opção é necessária na versão 1.x. Na versão 2.x, não utilize esta opção ou escolha o idioma padrão do seu projeto. |
| **`--template -t`** | Utilize o `func templates list` comando para ver a lista completa dos modelos disponíveis para cada idioma suportado.   |
| **`--name -n`** | O nome da função. |
| **`--csx`** | (Versão 2.x) Gera os mesmos c# script (. csx) modelos usados na versão 1.x e no portal. |

Por exemplo, para criar um acionador de HTTP de JavaScript num único comando, execute:

```bash
func new --template "Http Trigger" --name MyHttpTrigger
```

Para criar uma função acionada por fila num único comando, execute:

```bash
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="start"></a>Executar funções localmente

Para executar um projeto de funções, execute o anfitrião de funções. O anfitrião permite que os acionadores para todas as funções no projeto:

```bash
func host start
```
O `host` comando apenas é necessária na versão 1.x.

`func host start` suporta as seguintes opções:

| Opção     | Descrição                            |
| ------------ | -------------------------------------- |
| **`--cors`** | Uma lista separada por vírgulas de origens CORS, sem espaços. |
| **`--debug <type>`** | É iniciado no anfitrião com a porta de depuração abrir para que pode anexar aos **func.exe** processos de [Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started) ou [Visual Studio 2017](functions-dotnet-class-library.md). O *\<tipo\>* opções são `VSCode` e `VS`.  |
| **`--port -p`** | A porta local a escutar. Valor predefinido: 7071. |
| **`--timeout -t`** | O tempo limite para o anfitrião de funções iniciar, em segundos. Predefinição: 20 segundos.|
| **`--useHttps`** | Vincular `https://localhost:{port}` em vez da `http://localhost:{port}`. Por predefinição, esta opção cria um certificado fidedigno no seu computador.|
| **`--build`** | Compile o projeto atual antes de executar. Versão 2.x e projetos do c# só. |
| **`--cert`** | O caminho para um ficheiro. pfx que contém uma chave privada. Só é utilizada com `--useHttps`. Versão 2.x apenas. | 
| **`--password`** | A palavra-passe ou um ficheiro que contém a palavra-passe para um ficheiro. pfx. Só é utilizada com `--cert`. Versão 2.x apenas. |
| **`--language-worker`** | Argumentos para configurar a função de trabalho do idioma. Versão 2.x apenas. |
| **`--nodeDebugPort -n`** | A porta para o depurador de nó utilizar. Predefinição: Um valor de Launch ou 5858. Versão 1.x apenas. |

Para um classe biblioteca projeto c# (arquivo. csproj), tem de incluir o `--build` opção para gerar o ficheiro. dll de biblioteca.

Quando o anfitrião de funções é iniciado, ele gera as funções acionadas por URL de HTTP:

```bash
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="passing-test-data-to-a-function"></a>Passando dados de teste para uma função

Para testar as suas funções localmente, [iniciar o host de funções](#start) e chamar pontos finais no servidor local com pedidos HTTP. O ponto final que chamar depende do tipo de função.

>[!NOTE]  
> Exemplos neste tópico utilizam a ferramenta cURL para enviar pedidos de HTTP a partir do terminal ou uma linha de comandos. Pode usar uma ferramenta à sua escolha para enviar pedidos HTTP para o servidor local. A ferramenta cURL está disponível por predefinição em sistemas baseados em Linux. No Windows, primeiro tem de transferir e instalar o [ferramenta cURL](https://curl.haxx.se/).

Para obter mais informações sobre testes de funções, consulte [estratégias para testar seu código nas funções do Azure](functions-test-a-function.md).

#### <a name="http-and-webhook-triggered-functions"></a>HTTP e webhook das funções de acionado

Chama o seguinte acionada de ponto final para executar localmente o HTTP e webhook das funções:

    http://localhost:{port}/api/{function_name}

Certifique-se utilizar o mesmo nome de servidor e a porta que está a escutar o anfitrião de funções. Verá isto no resultado gerado ao iniciar o anfitrião de função. Pode chamar este URL usando qualquer método HTTP suportado pelo acionador. 

O seguinte cURL acionadores de comando a `MyHttpTrigger` função de início rápido de um pedido GET com o _nome_ parâmetro transmitido na cadeia de consulta. 

```bash
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
O exemplo seguinte é a mesma função de chamada a partir de um pedido POST passando _nome_ no corpo do pedido:

```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

Pode fazer pedidos de obtenção de um navegador da passagem de dados na cadeia de consulta. Para todos os outros métodos HTTP, tem de utilizar cURL, Fiddler, Postman ou uma ferramenta de teste HTTP semelhante.  

#### <a name="non-http-triggered-functions"></a>Funções de não-HTTP acionadas

Para todos os tipos de funções diferentes de acionadores HTTP e webhooks, pode testar as suas funções localmente ao chamar um ponto de final de administração. Chamar este ponto final com um pedido HTTP POST no servidor local aciona a função. Opcionalmente, pode passar dados de teste para a execução no corpo da solicitação POST. Esta funcionalidade é semelhante para o **teste** separador no portal do Azure.  

Chamar o seguinte ponto de extremidade de administrador para acionar as funções de não-HTTP:

    http://localhost:{port}/admin/functions/{function_name}

Para passar dados de teste para o ponto final de administrador de uma função, deve fornecer os dados no corpo de uma mensagem de pedido POST. O corpo da mensagem é necessário ter o seguinte formato JSON:

```JSON
{
    "input": "<trigger_input>"
}
````

O `<trigger_input>` valor contém dados num formato esperado pela função. O exemplo cURL seguinte é uma POSTAGEM a uma `QueueTriggerJS` função. Neste caso, a entrada é uma cadeia que é equivalente a mensagem esperada para ser encontrada na fila.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Usando o `func run` comando na versão 1.x

>[!IMPORTANT]  
> O `func run` comando não é suportado na versão 2.x das ferramentas. Para obter mais informações, consulte o tópico [como versões de tempo de execução de funções do Azure de destino](set-runtime-version.md).

Também é possível invocar uma função diretamente, usando `func run <FunctionName>` e fornecer dados de entrada para a função. Este comando é semelhante à execução de uma função utilizando o **teste** separador no portal do Azure. 

`func run` suporta as seguintes opções:

| Opção     | Descrição                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Conteúdo inline. |
| **`--debug -d`** | Anexe um depurador ao processo do host antes de executar a função.|
| **`--timeout -t`** | Tempo de espera (em segundos) até que o anfitrião de funções local está pronto.|
| **`--file -f`** | O nome de ficheiro a utilizar como conteúdo.|
| **`--no-interactive`** | Não é solicitada para entrada. Útil para cenários de automação.|

Por exemplo, para chamar uma função acionada por HTTP e passar o corpo do conteúdo, execute o seguinte comando:

```bash
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

### <a name="viewing-log-files-locally"></a>Ficheiros de registo de visualização localmente

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="publish"></a>Publicar no Azure

Para publicar um projeto de funções para uma aplicação de funções no Azure, utilize o `publish` comando:

```bash
func azure functionapp publish <FunctionAppName>
```

Pode utilizar as seguintes opções:

| Opção     | Descrição                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Definições de publicação no Settings para o Azure, pedir para substituir se a definição já existe. Se estiver a utilizar o emulador de armazenamento, altere a definição de aplicação para um [ligação de armazenamento real](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | Tem de ser utilizado com `-i`. Substitui AppSettings no Azure com o valor local se for diferente. A predefinição é a linha de comandos.|

Este comando publica uma aplicação de função existente no Azure. Ocorre um erro quando o `<FunctionAppName>` não existe na sua subscrição. Para saber como criar uma aplicação de funções a partir da linha de comandos ou janela de terminal com a CLI do Azure, veja [criar uma aplicação de funções para execução sem servidor](./scripts/functions-cli-create-serverless.md).

O `publish` comando carrega o conteúdo do diretório do projeto de funções. Se eliminar ficheiros localmente, o `publish` comando não exclui-los do Azure. Pode eliminar ficheiros no Azure com o [ferramenta de Kudu](functions-how-to-use-azure-function-app-settings.md#kudu) no [portal do Azure].  

>[!IMPORTANT]  
> Quando cria uma aplicação de funções no Azure, utiliza a versão 1.x do runtime de função, por predefinição. Para tornar a versão de utilização da aplicação de função 2.x do runtime, adicione a definição de aplicação `FUNCTIONS_EXTENSION_VERSION=beta`.  
Utilize o seguinte código de CLI do Azure para adicionar esta definição para a sua aplicação de função:

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings FUNCTIONS_EXTENSION_VERSION=beta   
```

## <a name="next-steps"></a>Passos Seguintes

As ferramentas de núcleo das funções do Azure é [código-fonte aberto e alojada no GitHub](https://github.com/azure/azure-functions-cli).  
Para um pedido de bug ou a funcionalidade de ficheiros [abra um problema do GitHub](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Ferramentas de núcleo das funções do Azure]: https://www.npmjs.com/package/azure-functions-core-tools
[Portal do Azure]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
