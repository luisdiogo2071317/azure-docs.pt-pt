---
title: Gerir vários módulos do Azure IoT Edge no VS Code | Microsoft Docs
description: Utilize o Visual Studio Code para desenvolver soluções do Azure IoT Edge que utilizam vários módulos.
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 4a624994f93e7a3cbb04db2a0ec0b2b823a12ee7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763039"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code-preview"></a>Desenvolver uma solução do IoT Edge com vários módulos no Visual Studio Code (pré-visualização)

Pode utilizar o Visual Studio Code para desenvolver a sua solução do Azure IoT Edge com vários módulos. Este artigo mostra-lhe como criar, atualizar e implementar uma solução do IoT Edge que encaminha dados de sensor num dispositivo simulado do IoT Edge no VS Code. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os passos descritos neste artigo, tem de cumprir os seguintes requisitos:

- [Visual Studio Code](https://code.visualstudio.com/)
- A [extensão do Azure IoT Edge para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- A [extensão C# para o Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- O [SDK .NET Core 2.0](https://www.microsoft.com/net/core#windowscmd)
- O modelo AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Um hub IoT ativo com, pelo menos, um dispositivo do IoT Edge

Também precisa do [Docker para o VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) com a integração do Device Explorer do Hub IoT do Azure para gerir imagens e contentores.

## <a name="prepare-your-first-iot-edge-solution"></a>Preparar a sua primeira solução do IoT Edge

1. Na **Paleta de Comandos** do VS Code, introduza e execute o comando **Edge: New IoT Edge solution**. Selecione a pasta da sua área de trabalho e indique o nome da solução (o nome predefinido é EdgeSolution). Crie um módulo C# (com o nome **SampleModule**) como o primeiro módulo de utilizador nesta solução. Também tem de especificar o repositório de imagens do Docker para o seu primeiro módulo. O repositório de imagens predefinido baseia-se um registo local do Docker (**localhost:5000/<first module name>**). Pode alterá-lo para o Azure Container Registry ou para o Hub do Docker.

   > [!NOTE]
   > Se estiver a utilizar um registo local do Docker, certifique-se de que o registo está em execução. Introduza o seguinte comando na janela da consola:
   > 
   > `docker run -d -p 5000:5000 --restart=always --name registry registry:2`

2. A janela do VS Code carrega a área de trabalho da solução do IoT Edge. A pasta raiz contém a pasta **módulos**, a pasta **.vscode** e um ficheiro de modelo do manifesto de implementação. As configurações de depuração encontram-se na pasta .vscode. Todos os códigos do módulo de utilizador são subpastas da pasta módulos. O ficheiro deployment.template.json é o modelo do manifesto de implementação. Alguns dos parâmetros neste ficheiro são analisados a partir do ficheiro module.json, o qual existe em cada pasta de módulo.

3. Adicione o seu segundo módulo ao projeto desta solução. Introduza e execute o comando **Edge: Add IoT Edge module**. Selecione o ficheiro de modelo de implementação para atualizar. Selecione um módulo **Função do Azure - C#** com o nome **SampleFunction** e o repositório de imagens do Docker.

4. Abra o ficheiro deployment.template.json. Certifique-se de que o ficheiro declara três módulos e o runtime. A mensagem é gerada a partir do módulo tempSensor. A mensagem é encaminhada diretamente através dos módulos SampleModule e SampleFunction e, em seguida, enviada para o seu hub IoT. 

5. Atualize as rotas destes módulos com o seguinte conteúdo:

   ```json
   "routes": {
      "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
      "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
      "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
   },
   ```

6. Guarde este ficheiro.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Criar e implementar a sua solução do IoT Edge

1. Na **Paleta de Comandos** do VS Code, introduza e execute o comando **Edge: Build IoT Edge solution**. Com base no ficheiro module.json em cada pasta de módulo, o comando começa a criar, a colocar em contentores e a emitir cada imagem do Docker do módulo. Em seguida, o comando passa o valor necessário para o ficheiro deployment.template.json e gera o ficheiro deployment.json com informações da pasta de configuração. O terminal integrado no VS Code mostra o progresso da compilação.

2. No **Device Explorer** do Azure IoT Hub IoT, clique com o botão direito do rato num ID de dispositivo do IoT Edge e, em seguida, selecione o comando **Create deployment for Edge device**. Selecione o ficheiro deployment.json na pasta de configuração. O terminal integrado no VS Code mostra a implementação criada com êxito com um ID de implementação.

3. Se estiver a simular um dispositivo do IoT Edge no seu computador de programação, tenha em atenção que todos os contentores de imagens de módulo iniciam no período de poucos minutos.

## <a name="view-the-generated-data"></a>Ver os dados gerados

1. Para monitorizar os dados que chegam ao hub IoT, selecione **Vista** > **Paleta de Comandos**. Em seguida, selecione o comando **IoT: Start monitoring D2C message**. 
2. Para parar a monitorização dos dados, utilize o comando **IoT: Stop monitoring D2C message** na **Paleta de Comandos**. 

## <a name="next-steps"></a>Passos seguintes

Saiba mais sobre outros cenários para programar com o Azure IoT Edge no Visual Studio Code:

* [Depurar um módulo C# no VS Code](how-to-vscode-debug-csharp-module.md)
* [Depurar uma função C# no VS Code](how-to-vscode-debug-azure-function.md)
