---
title: "Instale a extensão de funções durável e amostras - Azure"
description: "Saiba como instalar a extensão de funções durável para as funções do Azure, para desenvolvimento portal ou de desenvolvimento de Visual Studio."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 58f23406f20f6b0e75ec65197e4cebb6e4e788ac
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2017
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Instale a extensão de funções durável e amostras (funções do Azure)

O [funções durável](durable-functions-overview.md) extensão para as funções do Azure é fornecido no pacote NuGet [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Este artigo mostra como instalar o pacote e um conjunto de exemplos para os seguintes ambientes de desenvolvimento:

* Visual Studio 2017, (recomendado) 

* Portal do Azure

## <a name="visual-studio-2017"></a>Visual Studio 2017

O Visual Studio atualmente fornece a melhor experiência para desenvolver aplicações que utilizam funções durável.  As suas funções podem ser executadas localmente e também podem ser publicadas para o Azure. Pode começar com um projeto vazio ou com um conjunto de funções de exemplo.

### <a name="prerequisites"></a>Pré-requisitos

* Instalar o [versão mais recente do Visual Studio](https://www.visualstudio.com/downloads/) (versão 15.3 ou superior). Incluir o **programação do Azure** as opções de configuração de carga de trabalho.

### <a name="start-with-sample-functions"></a>Começar a utilizar funções de exemplo

1. Transferir o [ficheiro. zip de aplicação de exemplo para o Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Não terá de adicionar a referência do NuGet porque já tem o projeto de exemplo-lo.
2. Instalar e executar [emulador do Storage do Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) versão 5.2 ou posterior. Em alternativa, pode atualizar o *local.appsettings.json* ficheiro com cadeias de ligação reais do armazenamento do Azure.
3. Abra o projeto no Visual Studio 2017. 
4. Para obter instruções sobre como executar o exemplo, começar a utilizar [funcionar encadeamento - exemplo de sequência de Olá](durable-functions-sequence.md). O exemplo pode ser executado localmente ou publicado para o Azure.

### <a name="start-with-an-empty-project"></a>Começar com um projeto vazio
 
Siga as instruções para começar com o exemplo mesmas, mas efetuar os passos seguintes em vez de transferirem o *. zip* ficheiro:

1. Crie um projeto de aplicação de função.
2. Adicione a seguinte referência de pacote NuGet ao seu *. csproj* ficheiro:

   ```xml
   <PackageReference Include="Microsoft.Azure.WebJobs.Extensions.DurableTask" Version="1.0.0-beta" />
   ```
   
## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code fornece uma experiência de desenvolvimento local que abrangem todas as plataformas principais - Windows, macOS e Linux.  As suas funções podem ser executadas localmente e também podem ser publicadas para o Azure. Pode começar com um projeto vazio ou com um conjunto de funções de exemplo.

### <a name="prerequisites"></a>Pré-requisitos

* Instalar o [versão mais recente do Visual Studio Code](https://code.visualstudio.com/Download) 

* Siga as instruções em "Instalar o Azure funções principais ferramentas" em [código e teste das funções do Azure localmente](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

    >[!IMPORTANT]
    > Se já tiver as ferramentas de plataforma cruzada do Azure funções, atualizá-las para a versão mais recente disponível.

*  Instalar e executar [emulador do Storage do Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) versão 5.2 ou posterior. Em alternativa, pode atualizar o *local.appsettings.json* ficheiro com uma ligação de armazenamento do Azure real. 


### <a name="start-with-sample-functions"></a>Começar a utilizar funções de exemplo

1. Clone o [repositório funções durável](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navegar no seu computador para o [pasta amostras script c#](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Instalar a extensão de durável de funções do Azure executando o seguinte num comando de janela de terminal / linha de comandos:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.1.0-beta2
    ```
4. Executar o emulador do Storage do Azure ou atualizar o *local.appsettings.json* ficheiro com a cadeia de ligação real do armazenamento do Azure.
3. Abra o projeto no Visual Studio Code. 
5. Para obter instruções sobre como executar o exemplo, começar a utilizar [funcionar encadeamento - exemplo de sequência de Olá](durable-functions-sequence.md). O exemplo pode ser executado localmente ou publicado para o Azure.
6. Inicie o projeto executando o comando prompt / terminal o seguinte comando:
    ```bash
    func host start
    ```

### <a name="start-with-an-empty-project"></a>Começar com um projeto vazio
 
1. No comando terminal / linha de comandos, navegue para a pasta que irá alojar a sua aplicação de função.
2. Instalar a extensão de durável de funções do Azure executando o seguinte num comando de janela de terminal / linha de comandos:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.1.0-beta2
    ```
3. Crie um projeto de aplicação de função, executando o seguinte comando:

    ```bash
    func init
    ``` 
4. Executar o emulador do Storage do Azure ou atualizar o *local.appsettings.json* ficheiro com a cadeia de ligação real do armazenamento do Azure.
5. Em seguida, crie uma nova função executando o seguinte comando e siga os passos do assistente:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > Atualmente, o modelo de função durável não está disponível mas pode começar com uma das opções suportadas e, em seguida, modificar o código. Utilize para referência, os exemplos para [Orchestration cliente](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [Orchestration acionador](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence), e [acionador de atividade](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).

6. Abra a pasta do projeto no Visual Studio Code e continuar ao modificar o código de modelo. 
7. Inicie o projeto executando o comando prompt / terminal o seguinte comando:
    ```bash
    func host start
    ```

## <a name="azure-portal"></a>Portal do Azure

Se preferir, pode utilizar o portal do Azure para desenvolvimento de funções durável.

### <a name="create-an-orchestrator-function"></a>Criar uma função do orchestrator

1. Criar uma nova aplicação de função no [functions.azure.com](https://functions.azure.com/signin).

2. Configurar a aplicação de função para [utilizar a versão de 2.0 runtime](functions-versions.md).

3. Criar uma nova função selecionando **"criar a sua própria função personalizada".** .

4. Alterar o **idioma** para **c#**, **cenário** para **funções durável** e selecione o **durável Starter de Http de funções -C#** modelo.

5. Em **extensões instaladas não**, clique em **instalar** para transferir a extensão de NuGet.org. 

6. Após a instalação estiver concluída, continuar com a criação de uma função de cliente de orquestração – **"HttpStart"** que é criado selecionando **durável funções Http Starter - c#** modelo.

7. Agora, crie uma função de orquestração **"HelloSequence"** de **durável funções Orchestrator - C #** modelo.

8. E a última função será chamada **"Olá"** de **durável atividade funções - c#** modelo.

9. Aceda a **"HttpStart"** funcionar e copie o URL.

10. Utilize o Postman ou cURL para chamar a função durável. Antes de testar, substitua no URL **{functionName}** com o nome de função do orchestrator - **HelloSequence**.  Não existem dados são necessários, basta utilizar verbo POST. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Em seguida, chame o **"statusQueryGetUri"** endpoint e ver o estado atual da função durável

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Continuar a chamar o **"statusQueryGetUri"** endpoint até que o estado muda para **"Concluída"** 

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Parabéns! Sua primeira função durável se encontra em execução no Portal do Azure!

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Executar a função de encadeamento de exemplo](durable-functions-sequence.md)
