---
title: Gerir vários módulos de limite de IoT do Azure no VS Code | Microsoft Docs
description: Utilize o Visual Studio Code para desenvolver soluções de IoT limite que utilizam vários módulos.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4d9caa7aa95c99fa30e8ec76c5b6362615725622
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code---preview"></a>Desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code - pré-visualização
Pode utilizar o Visual Studio Code para desenvolver a sua solução de IoT Edge com vários módulos. Este artigo explica como criar, atualizar e implementar uma solução de IoT Edge esses dados de sensor pipes no dispositivo simulado contorno de IoT no Visual Studio Code. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir todos os passos neste artigo, tem os seguintes pré-requisitos:

- [Visual Studio Code](https://code.visualstudio.com/) 
- [Extensão de limite de IoT do Azure para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [C# para a extensão do Visual Studio Code (com tecnologia OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 
- Modelo de AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Um IoT hub Active Directory com, pelo menos, um dispositivo de limite de IoT


* [Docker para o VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) com a integração do Explorador para gerir imagens e contentores.


## <a name="prepare-your-first-iot-edge-solution"></a>Preparar a sua primeira solução de IoT Edge
1. Na paleta do comando de VS Code, escreva e execute o comando **Edge: solução de ponta de IoT novo**. Em seguida, selecione a pasta à sua área de trabalho, forneça o nome de solução (o nome predefinido é **EdgeSolution**) e criar um módulo c# (**SampleModule**) como o módulo de utilizador primeiro nesta solução. Também tem de especificar o repositório de imagens do Docker para o seu módulo primeiro. O repositório de imagens predefinido baseia-se um registo de Docker local (`localhost:5000/<first module name>`). Também pode alterá-la para o registo de contentor do Azure ou o Hub de Docker.

   > [!NOTE]
   > Se estiver a utilizar um registo de Docker local, certifique-se o registo está em execução, escrevendo o comando `docker run -d -p 5000:5000 --restart=always --name registry registry:2` na sua janela de consola.

2. A janela de VS Code carrega a sua área de trabalho de solução IoT Edge. A pasta de raiz contém um `modules` pasta, um `.vscode` pasta e um ficheiro de manifesto do modelo de implementação. Pode ver configurações de depuração `.vscode` pasta. Todos os códigos de módulo de utilizador será subpastas na pasta `modules`. O `deployment.template.json` é o modelo de manifesto de implementação. Alguns dos parâmetros de neste ficheiro irão ser analisado a partir do `module.json`, que existe na pasta cada módulo.

3. Adicione o segundo módulo para este projeto de solução. Este tempo, escreva e executar **contorno: o módulo de limite de IoT adicionar** e selecione o ficheiro de modelo de implementação para atualizar. Em seguida, selecione um **função do Azure - c#** com nome **SampleFunction** e o repositório de imagens do Docker.

4. Abra o `deployment.template.json` de ficheiros e certifique-se de que declara três módulos para além do tempo de execução. A mensagem será gerada a partir de `tempSensor` módulo e irá ser direcionado diretamente através do `SampleModule` e `SampleFunction`, em seguida, enviado ao seu IoT hub. 
5. Atualize as rotas para estes módulos com o seguinte conteúdo:
   ```json
   "routes": {
      "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
      "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
      "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
   },
   ```

6. Guarde este ficheiro.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Criar e implementar a sua solução de IoT Edge
1. Na paleta do comando de VS Code, escreva e execute o comando **Edge: solução de ponta de IoT criar**. Com base no `module.json` ficheiro na pasta cada módulo, este início de comando para criar, containerize e push cada imagem de docker do módulo. Em seguida, estes passam o valor necessário para `deployment.template.json` e gera o `deployment.json` ficheiros com as informações do `config` pasta. Pode ver o progresso de compilação no terminal integrada VS Code.

2. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo de limite de IoT, em seguida, selecione **criar a implementação para o dispositivo de limite**. Selecione o `deployment.json` em `config` pasta. Em seguida, pode ver que a implementação foi criada com êxito com uma implementação que ID no VS Code integrado terminal.

3. Se estiver simulando um dispositivo de limite de IoT no computador de desenvolvimento, verá que todos os contentores de imagem de módulo serão iniciados dentro de alguns minutos.

## <a name="view-generated-data"></a>Ver os dados gerados

1. Para monitorizar os dados que chegam ao hub IoT, selecione **View** > **Command Palette...** e procure **IoT: Start monitoring D2C message** (IoT: Começar a monitorizar mensagens D2C). 
2. Para parar a monitorização de dados, utilize o comando **IoT: Stop monitoring D2C message** (IoT: Parar a monitorização de mensagens D2C) na paleta de comandos. 

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre outros cenários para o limite de IoT do Azure no Visual Studio Code de desenvolvimento:

* [Depurar um módulo c# no VS Code](how-to-vscode-debug-csharp-module.md)
* [Depurar uma função de c# no VS Code](how-to-vscode-debug-azure-function.md)