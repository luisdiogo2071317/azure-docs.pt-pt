---
title: Criar a sua primeira função no Azure com o Java e o Maven | Microsoft Docs
description: Crie e publique uma função acionada por HTTP simples no Azure com o Java e o Maven.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: funções do azure, funções, processamento de eventos, computação, arquitetura sem servidor
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: f0dd3b276adb815723673042060b2ad5d54a1ac7
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382651"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Crie a sua primeira função com o Java e o Maven (Pré-visualização)

> [!NOTE] 
> O Java para as Funções do Azure encontra-se atualmente em pré-visualização.

Este guia de introdução orienta ao longo da criação de um [sem servidor](https://azure.microsoft.com/solutions/serverless/) projeto de função com o Maven, testá-lo localmente e implementá-lo para o Azure. Quando tiver terminado, o código da função de Java está em execução na cloud e pode ser acionado de um pedido HTTP.

![Aceder a uma função do Hello World, a partir da linha de comandos com cURL](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos
Para desenvolver uma aplicação de funções com o Java, tem de ter o seguinte instalado:

-  [Java Development Kit](https://www.azul.com/downloads/zulu/), versão 8.
-  [Apache Maven](https://maven.apache.org), versão 3.0 ou superior.
-  [CLI do Azure](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> A variável de ambiente do JAVA_HOME tem de ser definida para a localização de instalação do JDK para concluir este guia de introdução.

## <a name="install-the-azure-functions-core-tools"></a>Instalar as Ferramentas de Núcleo de Funções do Azure

As [Ferramentas de Núcleo das Funções do Azure 2.0](https://www.npmjs.com/package/azure-functions-core-tools) oferecem um ambiente de desenvolvimento local para escrever, executar e depurar Funções do Azure. 

Para instalar, visite a secção [Instalar](https://github.com/azure/azure-functions-core-tools#installing) do projeto Ferramentas de Núcleo das Funções do Azure para localizar as instruções específicas para o seu sistema operativo.

Pode também instalar manualmente com o [npm](https://www.npmjs.com/), incluído com o [Node.js](https://nodejs.org/), após instalar os seguintes requisitos:

-  [.NET Core](https://www.microsoft.com/net/core), versão mais recente.
-  [Node.js](https://nodejs.org/download/) versão 8.6 ou superior.

Para avançar com uma instalação baseada em npm, consulte:

```
npm install -g azure-functions-core-tools
```

> [!NOTE]
> Se tiver dificuldades em instalar as Ferramentas de Núcleo das Funções do Azure versão 2.0, veja [Versão 2.x do runtime](/azure/azure-functions/functions-run-local#version-2x-runtime).

## <a name="generate-a-new-functions-project"></a>Gerar um novo projeto das Funções

Numa pasta vazia, execute o seguinte comando para gerar o projeto das Funções a partir de um [arquétipo do Maven](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

### <a name="linuxmacos"></a>Linux/MacOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

O Maven pede-lhe os valores necessários para terminar de gerar o projeto. Para os valores de _groupId_, _artifactId_ e _versão_, veja a referência [Maven naming conventions (Convenções de nomenclatura do Maven)](https://maven.apache.org/guides/mini/guide-naming-conventions.html). O valor de _appName_ tem de ser exclusivo em todo o Azure, para que o Maven gere um nome de aplicação com base no _artifactId_ anteriormente introduzido por predefinição. O valor de _packageName_ determina o pacote de Java para o código de função gerado.

Os identificadores `com.fabrikam.functions` e `fabrikam-functions` abaixo são utilizados como exemplo e para facilitar a leitura dos passos posteriores deste início rápido. É encorajado a indicar os seus próprios valores para o Maven neste passo.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Confirm properties configuration: Y
```

O Maven cria os ficheiros de projeto numa pasta nova, com o nome _artifactId_, `fabrikam-functions` neste exemplo. O código pronto a ser executado no projeto é uma função [acionada por HTTP](/azure/azure-functions/functions-bindings-http-webhook) simples que ecoa o corpo do pedido:

```java
public class Function {
    /**
     * This function listens at endpoint "/api/hello". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/hello
     * 2. curl {your host}/api/hello?name=HTTP%20Query
     */
    @FunctionName("hello")
    public HttpResponseMessage<String> hello(
            @HttpTrigger(name = "req", methods = {"get", "post"}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponse(400, "Please pass a name on the query string or in the request body");
        } else {
            return request.createResponse(200, "Hello, " + name);
        }
    }
}

```

## <a name="run-the-function-locally"></a>Executar localmente a função

Altere o diretório para a pasta do projeto criado recentemente, e crie e execute a função com o Maven:

```
cd fabrikam-function
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Se estiver a ocorrer esta exceção: `javax.xml.bind.JAXBException` com o Java 9, veja a solução no [GitHub](https://github.com/jOOQ/jOOQ/issues/6477).

Verá esta saída quando a função estiver a ser executada localmente no seu sistema e pronta para responder a pedidos HTTP:

```Output
Listening on http://localhost:7071
Hit CTRL-C to exit...

Http Functions:

   hello: http://localhost:7071/api/hello
```

Acione a função na linha de comandos com o curl numa janela de terminal nova:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/hello
```

```Output
Hello LocalFunction!
```

Utilize `Ctrl-C` no terminal para parar o código de função.

## <a name="deploy-the-function-to-azure"></a>Implementar a função no Azure

O processo de implementação para as Funções do Azure utiliza credenciais de conta a partir da CLI do Azure. [Inicie sessão com a CLI do Azure](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) antes de continuar.

```azurecli
az login
```

Implemente o código numa aplicação de funções nova com o destino do Maven `azure-functions:deploy`.

```
mvn azure-functions:deploy
```

Quando a implementação estiver concluída, verá o URL que pode utilizar para aceder à sua aplicação de funções do Azure:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Teste a aplicação de funções em execução no Azure com `cURL`: Terá de alterar o URL do exemplo abaixo para corresponder ao URL implementado da sua própria aplicação de funções do passo anterior.

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="make-changes-and-redeploy"></a>Fazer alterações e reimplementar

Edite o ficheiro de origem `src/main.../Function.java` no projeto gerado para alterar o texto devolvido pela sua aplicação de funções. Altere esta linha:

```java
return request.createResponse(200, "Hello, " + name);
```

Para a seguinte:

```java
return request.createResponse(200, "Hi, " + name);
```

Guarde as alterações e implemente novamente ao executar `azure-functions:deploy` a partir do terminal, como fez anteriormente. A aplicação de funções será atualizada e este pedido:

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

Terá resultados atualizados:

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>Passos Seguintes

Criou uma aplicação de funções do Java com um acionador HTTP simples e implementou o mesmo nas Funções do Azure.

- Reveja o [Guia para programadores das Funções do Java](functions-reference-java.md) para obter mais informações sobre a programação de funções do Java.
- Adicione mais funções com diferentes acionadores ao seu projeto com o destino do Maven `azure-functions:add`.
- Escreva e depure funções localmente com o [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) e [Eclipse](functions-create-maven-eclipse.md). 
- Depure funções implementadas no Azure com o Visual Studio Code. Veja a documentação do Visual Studio Code sobre [aplicações Java sem servidor](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) para obter instruções.
