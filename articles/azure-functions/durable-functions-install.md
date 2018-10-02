---
title: Instalar a extensão Durable Functions e os exemplos - Azure
description: Saiba como instalar a extensão de funções duráveis para as funções do Azure, para o desenvolvimento de portais ou de desenvolvimento do Visual Studio.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: azfuncdf
ms.openlocfilehash: 2eb838bcb9d3f64d0bbf4657c516adb50d103223
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/01/2018
ms.locfileid: "47585310"
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Instalar a extensão Durable Functions e os exemplos (funções do Azure)

O [funções duráveis](durable-functions-overview.md) extensão para as funções do Azure é fornecido no pacote de NuGet [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Este artigo mostra como instalar o pacote e um conjunto de exemplos para os seguintes ambientes de desenvolvimento:

* Visual Studio 2017 (recomendado para c#) 
* Visual Studio Code (recomendado para JavaScript)
* Portal do Azure

## <a name="visual-studio-2017"></a>Visual Studio 2017

O Visual Studio fornece, atualmente, a melhor experiência para o desenvolvimento de aplicações que utilizam funções duráveis.  As suas funções podem ser executadas localmente e também podem ser publicadas para o Azure. Pode começar com um projeto vazio ou com um conjunto de funções de exemplo.

### <a name="prerequisites"></a>Pré-requisitos

* Instalar o [mais recente versão do Visual Studio](https://www.visualstudio.com/downloads/) (versão 15,6 ou superior). Incluir o **desenvolvimento do Azure** carga de trabalho em suas opções de configuração.

### <a name="start-with-sample-functions"></a>Iniciar com as funções de exemplo 

1. Transfira o [ficheiro. zip de aplicação de exemplo para o Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Não precisa de adicionar a referência do NuGet, porque o projeto de exemplo já com ele.
2. Instalar e executar [emulador de armazenamento do Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) versão 5.6 ou posterior. Em alternativa, pode atualizar o *Settings* ficheiro com cadeias de ligação real do armazenamento do Azure.
3. Abra o projeto no Visual Studio 2017. 
4. Para obter instruções sobre como executar o exemplo, começar com [funcionar encadeamento - exemplo de sequência de Olá](durable-functions-sequence.md). O exemplo pode ser executado localmente ou publicado para o Azure.

### <a name="start-with-an-empty-project"></a>Começar com um projeto vazio
 
Siga as indicações mesmo como para iniciar com o exemplo, mas faça o seguinte em vez de transferirem o *. zip* ficheiro:

1. Crie um projeto de aplicação de funções.
2. Procure o seguinte NuGet pacote referência usando *gerir pacotes NuGet* e adicioná-lo ao projeto: Microsoft.Azure.WebJobs.Extensions.DurableTask v1.6.0
   
## <a name="visual-studio-code"></a>Visual Studio Code

Código do Visual Studio fornece uma experiência de desenvolvimento local que abrange todas as plataformas principais - Windows, macOS e Linux.  As suas funções podem ser executadas localmente e também de ser publicadas para o Azure. Pode começar com um projeto vazio ou com um conjunto de funções de exemplo.

### <a name="prerequisites"></a>Pré-requisitos

* Instalar o [versão mais recente do Visual Studio Code](https://code.visualstudio.com/Download) 

* Siga as instruções em "Instalar o Azure funções principais ferramentas" no [codificar e testar as funções do Azure localmente](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

    >[!IMPORTANT]
    > Se já tiver as ferramentas de plataforma cruzada do Azure funções, atualizá-los para a versão mais recente disponível.

    >[!IMPORTANT]
    >Funções duráveis em JavaScript requer a versão 2.x das ferramentas de núcleo de funções do Azure.

*  Se estiver num computador Windows, instale e execute [emulador de armazenamento do Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) versão 5.6 ou posterior. Em alternativa, pode atualizar o *Settings* ficheiro com conexão real do armazenamento do Azure. 


### <a name="start-with-sample-functions"></a>Iniciar com as funções de exemplo

#### <a name="c"></a>C#

1. Clone o [repositório de funções duráveis](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navegue no seu computador para o [pasta de exemplos em C# script](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Instalar a extensão de durável de funções do Azure ao executar o seguinte num comando janela de terminal / linha de comandos:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.6.0
    ```
4. Instalar a extensão de Twilio de funções do Azure ao executar o seguinte num comando janela de terminal / linha de comandos:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.Twilio -v 3.0.0-beta8
    ```
5. Executar o emulador de armazenamento do Azure ou atualizar o *Settings* ficheiro com a cadeia de ligação real do armazenamento do Azure.
6. Abra o projeto no Visual Studio Code. 
7. Para obter instruções sobre como executar o exemplo, começar com [funcionar encadeamento - exemplo de sequência de Olá](durable-functions-sequence.md). O exemplo pode ser executado localmente ou publicado para o Azure.
8. Inicie o projeto ao executar no comando terminal / linha de comandos o seguinte comando:
    ```bash
    func host start
    ```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas para v2 de funções)

1. Clone o [repositório de funções duráveis](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navegue no seu computador para o [pasta de exemplos de JavaScript](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/javascript). 
3. Instalar a extensão de durável de funções do Azure ao executar o seguinte num comando janela de terminal / linha de comandos

    ```
    func extensions install
    ```
    > [!NOTE] 
    > Isto requer que o [.NET Core SDK](https://www.microsoft.com/net/download) para ser instalado na máquina
4. Restaurar os pacotes de npm, executando o seguinte num comando janela de terminal / linha de comandos:
    
    ```
    npm install
    ``` 
5. Atualização do *Settings* ficheiro com uma cadeia de ligação a partir de uma conta de armazenamento do Azure para `AzureWebJobsStorage`.  Esta conta de armazenamento será utilizada para o estado de função durável.
6. Abra o projeto num editor como o Visual Studio Code. 
7. Para obter instruções sobre como executar o exemplo, começar com [funcionar encadeamento - exemplo de sequência de Olá](durable-functions-sequence.md). O exemplo pode ser executado localmente ou publicado para o Azure.
8. Inicie o projeto ao executar no comando terminal / linha de comandos o seguinte comando:
    ```
    func start
    ```

### <a name="start-with-an-empty-project"></a>Começar com um projeto vazio
 
1. No comando terminal / linha de comandos, navegue para a pasta que irá alojar a sua aplicação de funções.
3. Crie um projeto de aplicação de funções, executando o seguinte comando:

    ```
    func init
    ``` 
4. Executar o emulador de armazenamento do Azure (apenas Windows) ou atualizar o *Settings* ficheiro pela cadeia de ligação de armazenamento do Azure real do `AzureWebJobsStorage`.
5. Em seguida, crie uma nova função ao executar o seguinte comando e siga os passos do assistente:

    ```
    func new
    ```
    >[!IMPORTANT]
    > Atualmente, o modelo de função durável não está disponível mas pode começar com uma das opções suportadas e, em seguida, modificar o código. Utilize para referência, os exemplos para [cliente de orquestração](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [orquestração acionador](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence), e [atividade acionador](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).
2. Instalar a extensão de durável de funções do Azure ao executar o seguinte num comando janela de terminal / linha de comandos no diretório da aplicação de função:

    ```
    func extensions install
    ```

6. Abra a pasta de projeto no Visual Studio Code e continuar modificando o código do modelo. 
7. Inicie o projeto ao executar no comando terminal / linha de comandos o seguinte comando:
    ```
    func start
    ```

## <a name="azure-portal"></a>Portal do Azure

Se preferir, pode utilizar o [portal do Azure](https://portal.azure.com) para o desenvolvimento de funções duráveis.

   > [!NOTE]
   > Funções duráveis em JavaScript ainda não estão disponíveis no portal.

### <a name="create-an-orchestrator-function"></a>Criar uma função de orquestrador

1. Criar uma nova aplicação de função no portal, como mostra a [artigo de início rápido das funções](functions-create-first-azure-function.md#create-a-function-app).

2. Configurar a aplicação de função para [utilizar a versão 2.0 runtime](set-runtime-version.md).

   A extensão de funções duráveis funciona no tempo de execução do 1.X e o tempo de execução 2.0, mas os modelos do Portal do Azure só estão disponíveis para criar aplicativos para o tempo de execução 2.0.

3. Criar uma nova função, selecionando **"criar sua própria função personalizada".** .

4. Alteração da **linguagem** para **c#**, **cenário** para **funções duráveis** e selecione o **Starter de Http de funções duráveis -C#** modelo.

5. Sob **extensões não instaladas**, clique em **instalar** para transferir a extensão da NuGet.org. 

6. Depois de concluída a instalação, continue com a criação de uma função de cliente de orquestração – **"HttpStart"** que é criado ao selecionar **durável funções Http Starter - c#** modelo.

7. Agora, crie uma função de orquestração **"HelloSequence"** partir **Orquestrador de funções duráveis - c#** modelo.

8. E a última função será chamada **"Olá"** partir **atividade de funções durável - c#** modelo.

9. Aceda a **"HttpStart"** de função e copie o URL.

10. Utilize o Postman ou o cURL para chamar a função durável. Antes de testar, substitua o URL **{functionName}** com o nome de função do orchestrator - **HelloSequence**.  Dados não são necessários, use apenas o verbo POST. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Em seguida, chamar o **"statusQueryGetUri"** ponto final e ver o estado atual da função durável

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Continuar a chamar o **"statusQueryGetUri"** ponto final até que o estado muda para **"Concluído"** 

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

Parabéns! Sua primeira função durável está em execução no Portal do Azure!

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Executar a função de encadeamento de exemplo](durable-functions-sequence.md)
