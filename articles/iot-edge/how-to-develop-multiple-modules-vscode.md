---
title: Trabalhar com vários módulos do Azure IoT Edge no VS Code | Documentos da Microsoft
description: Utilizar a extensão de IoT para o Visual Studio Code para desenvolver vários módulos de uma só vez para o Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: d895e10c6b667a05156b2ce0f83f7776b9ba4f0a
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568765"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code"></a>Desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code

Pode utilizar o Visual Studio Code para desenvolver a sua solução do Azure IoT Edge com vários módulos. Este artigo mostra-lhe como criar, atualizar e implementar uma solução do IoT Edge que encaminha dados de sensor num dispositivo simulado do IoT Edge no VS Code. 

## <a name="prerequisites"></a>Pré-requisitos

Para concluir os passos descritos neste artigo, tem de cumprir os seguintes requisitos:

- [Visual Studio Code](https://code.visualstudio.com/)
- A [extensão do Azure IoT Edge para o Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- A [extensão C# para o Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- O [.NET Core 2.1 SDK](https://www.microsoft.com/net/download)
- Um hub IoT ativo com, pelo menos, um dispositivo do IoT Edge

Também precisa do [Docker para o VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) com a integração do Device Explorer do Hub IoT do Azure para gerir imagens e contentores.

## <a name="create-your-iot-edge-solution"></a>Criar a sua solução de IoT Edge

1. No Visual Studio code, abra o terminal integrado selecionando **View** > **terminal integrado**. 

1. No código VS **paleta de comandos**, introduza e execute o comando **Azure IoT Edge: solução nova do IoT Edge**. Selecione a pasta da sua área de trabalho e indique o nome da solução (o nome predefinido é EdgeSolution). Criar um módulo de c# (com o nome **PipeModule**) como o primeiro módulo do utilizador nesta solução. O modelo padrão do módulo de c# é um módulo de pipe, que diretamente encaminha as mensagens da montante ao longo do caminho. Também tem de especificar o repositório de imagens do Docker para o seu primeiro módulo. O repositório de imagens predefinido baseia-se um registo local do Docker (**localhost:5000/<first module name>**). Pode alterá-lo para o Azure Container Registry ou para o Hub do Docker. 

2. A janela do VS Code carrega a área de trabalho da solução do IoT Edge. A pasta raiz contém a pasta **módulos**, a pasta **.vscode** e um ficheiro de modelo do manifesto de implementação. As configurações de depuração encontram-se na pasta .vscode. Todos os códigos do módulo de utilizador são subpastas da pasta módulos. O ficheiro deployment.template.json é o modelo do manifesto de implementação. Alguns dos parâmetros neste ficheiro são analisados a partir do ficheiro module.json, o qual existe em cada pasta de módulo.

3. Adicione o seu segundo módulo ao projeto desta solução. Existem várias formas para adicionar um novo módulo para a solução atual. Introduza e execute o comando **do Azure IoT Edge: módulo do IoT Edge/adicionar**. Selecione o ficheiro de modelo de implementação para atualizar. Ou clique com o botão direito na pasta de módulos ou o ficheiro de deployment.template.json com o botão direito e selecione **Adicionar módulo do IoT Edge**. Em seguida, haverá uma lista pendente para selecionar o tipo de módulo. Selecione um **as funções do Azure - c#** módulo com o nome **PipeFunction** e seu repositório de imagens do Docker. O modelo padrão do módulo de funções c# é um módulo de pipe, que diretamente encaminha as mensagens da montante ao longo do caminho.

4. Abra o ficheiro deployment.template.json. Certifique-se de que o ficheiro declara três módulos e o runtime. A mensagem é gerada a partir do módulo tempSensor. A mensagem é encaminhada diretamente através dos módulos SampleModule e SampleFunction e, em seguida, enviada para o seu hub IoT. 

5. Atualize as rotas destes módulos com o seguinte conteúdo:

   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/PipeModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/PipeModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/PipeFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/PipeFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Guarde este ficheiro.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Criar e implementar a sua solução do IoT Edge

1. No código VS **paleta de comandos**, introduza e execute o comando **Azure IoT Edge: solução de IoT Edge criar**. Com base no ficheiro module.json em cada pasta de módulo, o comando começa a criar, a colocar em contentores e a emitir cada imagem do Docker do módulo. Em seguida, o comando passa o valor necessário para o ficheiro deployment.template.json e gera o ficheiro deployment.json com informações da pasta de configuração. O terminal integrado no VS Code mostra o progresso da compilação. 

2. No **Device Explorer** do Azure IoT Hub IoT, clique com o botão direito do rato num ID de dispositivo do IoT Edge e, em seguida, selecione o comando **Create deployment for Edge device**. Selecione o ficheiro deployment.json na pasta de configuração. O terminal integrado no VS Code mostra a implementação criada com êxito com um ID de implementação.

3. Se estiver a simular um dispositivo IoT Edge no computador de desenvolvimento, pode ver ver que todos os contentores de imagem do módulo começar em poucos minutos.

## <a name="view-the-generated-data"></a>Ver os dados gerados

1. Para monitorizar os dados que chegam ao hub IoT, selecione **Vista** > **Paleta de Comandos**. Em seguida, selecione o comando **IoT: Start monitoring D2C message**. 
2. Para parar a monitorização dos dados, utilize o comando **IoT: Stop monitoring D2C message** na **Paleta de Comandos**. 

## <a name="next-steps"></a>Passos Seguintes

Saiba mais sobre outros cenários para programar com o Azure IoT Edge no Visual Studio Code:

* Desenvolver módulos no VS Code com [c#](how-to-develop-csharp-module.md) ou [node. js](how-to-develop-node-module.md).
* Desenvolver as funções do Azure no VS Code com [c#](how-to-develop-csharp-function.md).

Para desenvolver módulos para os seus dispositivos de IoT Edge [compreender e utilizar os SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).