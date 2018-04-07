---
title: Trabalhar com vários módulos de limite de IoT no Visual Studio Code | Microsoft Docs
description: Implementar o Azure Machine Learning como um módulo para um dispositivo de limite
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 6c94701507f86f6ecab2875f952215cc3e4cc719
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code---preview"></a>Desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code - pré-visualização
Pode utilizar o Visual Studio Code para desenvolver a sua solução de IoT Edge com vários módulos. Este artigo explica como criar, atualizar e implementar uma solução de IoT Edge esses dados de sensor pipes no dispositivo simulado contorno de IoT no Visual Studio Code. Neste artigo, saiba como:

* Utilize o Visual Studio Code para criar uma solução de IoT Edge
* Utilizar o VS Code para adicionar um novo módulo ao seu trabalho solução de IoT Edge. 
* Implementar a solução de IoT Edge (vários módulos) para o seu dispositivo de limite de IoT
* Ver os dados gerados

## <a name="prerequisites"></a>Pré-requisitos
* Concluir abaixo tutoriais
  * [Implementar o módulo de c#](tutorial-csharp-module.md)
  * [Implementar a função de c#](tutorial-deploy-function.md)
  * [Implementar o módulo de Python](tutorial-python-module.md)
* [Docker para o VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) com a integração do Explorador para gerir imagens e contentores.


## <a name="prepare-your-first-iot-edge-solution"></a>Preparar a sua primeira solução de IoT Edge
1. Na paleta do comando de VS Code, escreva e execute o comando **Edge: solução de ponta de IoT novo**. Em seguida, selecione a pasta à sua área de trabalho, forneça o nome de solução (o nome predefinido é **EdgeSolution**) e criar um módulo c# (**SampleModule**) como o módulo de utilizador primeiro nesta solução. Também tem de especificar o repositório de imagens do Docker para o seu módulo primeiro. O repositório de imagens predefinido baseia-se um registo de Docker local (`localhost:5000/<first module name>`). Também pode alterá-la para o registo de contentor do Azure ou o Hub de Docker.

> [!NOTE]
> Se estiver a utilizar um registo de Docker local, certifique-se o registo está a executar, escrevendo o comando `docker run -d -p 5000:5000 --restart=always --name registry registry:2` na sua janela de consola.

2. A janela de VS Code carregará a sua área de trabalho de solução IoT Edge. Não existe um `modules` pasta, um `.vscode` pasta e uma implementação de manifestam do ficheiro de modelo na pasta raiz. Pode ver configurações de depuração `.vscode` pasta. Todos os códigos de módulo de utilizador será subpastas na pasta `modules`. O `deployment.template.json` é o modelo de manifesto de implementação. Alguns dos parâmetros de neste ficheiro irão ser analisado a partir do `module.json`, que existe na pasta cada módulo.

3. Adicione o segundo módulo para este projeto de solução. Este tempo, escreva e executar **contorno: o módulo de limite de IoT adicionar** e selecione o ficheiro de modelo de implementação para atualizar. Em seguida, selecione um **função do Azure - c#** com nome **SampleFunction** e o repositório de imagens do Docker para adicionar.

4. Agora a sua primeira solução de IoT Edge com dois módulos básicos está pronta. O módulo c# predefinido funciona como um módulo de mensagem de pipe enquanto Funtion c# atua como uma função de mensagem de pipe. No `deployment.template.json`, verá esta solução contém três módulos. A mensagem será gerada a partir de `tempSensor` módulo e irá ser direcionado diretamente através do `SampleModule` e `SampleFunction`, em seguida, enviado ao seu IoT hub. Atualize as rotas para estes módulos com abaixo conteúdo. 
   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Guarde este ficheiro.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Criar e implementar a sua solução de IoT Edge
1. Na paleta do comando de VS Code, escreva e execute o comando **Edge: solução de ponta de IoT criar**. Com base no `module.json` ficheiro na pasta cada módulo, este comando irá verificar e começar a criar, containerize e push cada imagem de docker do módulo. Em seguida, irá analisar o valor necessário para `deployment.template.json`, gerar o `deployment.json` com o valor real em `config` pasta. Pode ver o progresso de compilação no terminal integrada VS Code.

2. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo de limite de IoT, em seguida, selecione **criar a implementação para o dispositivo de limite**. Selecione o `deployment.json` em `config` pasta. Em seguida, pode ver que a implementação foi criada com êxito com uma implementação que ID no VS Code integrado terminal.

3. Se estiver [simulando um dispositivo de limite de IoT](tutorial-simulate-device-linux.md) no computador de desenvolvimento. Verá que todos os contentores de imagem de módulo serão iniciados dentro de alguns minutos.

## <a name="view-generated-data"></a>Ver os dados gerados

1. Para monitorizar os dados que chegam ao hub IoT, selecione **View** > **Command Palette...** e procure **IoT: Start monitoring D2C message** (IoT: Começar a monitorizar mensagens D2C). 
2. Para parar a monitorização de dados, utilize o comando **IoT: Stop monitoring D2C message** (IoT: Parar a monitorização de mensagens D2C) na paleta de comandos. 

## <a name="next-steps"></a>Passos Seguintes

Pode continuar a sessão em qualquer um dos seguintes artigos para saber mais sobre outros cenários quando desenvolver o limite de IoT do Azure no Visual Studio Code:

* [Depurar um módulo c# no VS Code](how-to-vscode-debug-csharp-module.md)
* [Depurar uma função de c# no VS Code](how-to-vscode-debug-azure-function.md)