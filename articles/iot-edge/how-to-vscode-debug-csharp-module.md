---
title: "Utilizar o Visual Studio Code para depurar um módulo de c# com o Azure IoT Edge | Microsoft Docs"
description: "Faça a depuração de um módulo de c# com o limite de IoT do Azure no Visual Studio Code."
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 46d9ca0bf6c9ddf95c147fc2eb62d275c973845e
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para depurar um módulo de c# com o Azure IoT Edge
Este artigo fornece instruções detalhadas para utilizar [Visual Studio Code](https://code.visualstudio.com/) como a ferramenta de desenvolvimento principal para os módulos do Azure IoT contorno de depuração.

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT pode ser outro dispositivo físico, ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

Antes de começar esta orientação, conclua o tutorial seguinte:
- [Utilizar o Visual Studio Code para desenvolver c# módulo com o Azure IoT Edge](how-to-vscode-develop-csharp-module.md)

Depois de concluir o tutorial anterior, deve ter os seguintes itens prontos:
- Um registo de Docker local em execução no computador de desenvolvimento. Isto é para fazer o protótipo e fins de teste.
- O `Program.cs` ficheiros, com o código de módulo de filtro mais recente.
- Um atualizado `deployment.json` ficheiro para os módulos do sensor e filtro.
- Runtime de limite de IoT que é executado no computador de desenvolvimento.

## <a name="build-your-iot-edge-module-for-debugging"></a>Criar o módulo de limite de IoT para depuração
1. Para iniciar a depuração, utilize **dockerfile.debug** para reconstruir a imagem de Docker e implementar a sua solução de IoT Edge novamente. No Explorador de código do Visual Studio, selecione a pasta de Docker para abri-lo. Em seguida, selecione o **linux x64** pasta, faça duplo clique **Dockerfile.debug**e selecione **imagem de Docker do módulo de limite de IoT criar**.

    ![Captura de ecrã do Explorador de código de VS](./media/how-to-debug-csharp-module/build-debug-image.png)

3. No **selecionar pasta** janela, procurar ou introduzir **./bin/Debug/netcoreapp2.0/publish**. Em seguida, selecione **selecionar pasta como EXE_DIR**.
4. Na caixa de texto de pop-up na parte superior da janela VS Code, introduza o nome da imagem. Por exemplo: `<your container registry address>/filtermodule:latest`. Se estiver a implementar no registo local, deve ser: `localhost:5000/filtermodule:latest`.
5. A imagem de emissão para o seu repositório de Docker. Utilize o **Edge: imagem de Docker do módulo de limite de Push de IoT** de comandos e introduza o URL da imagem na caixa de texto de pop-up na parte superior da janela VS Code. Utilize o mesmo URL da imagem que utilizou no passo anterior.
6. Pode reutilizar o `deployment.json` para voltar a implementar. Na paleta do comando, escreva e selecione **Edge: reiniciar Edge** para obter o módulo de filtro com a versão de depuração.

## <a name="start-debugging-in-vs-code"></a>Iniciar a depuração no VS Code
1. Ir para a janela de depuração do código de VS. Prima **F5**e selecione **IoT Edge(.NET Core)**.

    ![Janela de depuração de captura de ecrã do VS Code](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. No `launch.json`, navegue para o **depurar IoT Edge personalizada módulo (.NET Core)** secção. Em **pipeArgs**, preencha o `<container_name>`. Deve ser `filtermodule` neste tutorial.

    ![Janela de depuração de captura de ecrã do VS Code](./media/how-to-debug-csharp-module/f5-debug-option.png)

3. Navegue até à **Program.cs**. Adicionar um ponto de interrupção no `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Prima **F5** novamente e selecionar o processo para anexar a. Neste tutorial, deve ser o nome do processo `FilterModule.dll`.

    ![Janela de depuração de captura de ecrã do VS Code](./media/how-to-debug-csharp-module/attach-process.png)

5. Na janela de depuração VS Code, pode ver as variáveis no painel esquerdo. 

> [!NOTE]
> O exemplo anterior mostra como depurar módulos .NET Core IoT Edge nos contentores. Se baseia na versão de depuração do `Dockerfile.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) da imagem do contentor ao criá-lo. Depois de concluir a depuração os módulos c#, é recomendável utilizar ou personalizar diretamente `Dockerfile` sem VSDBG para módulos de limite de IoT prontos para produção.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou um módulo de limite de IoT e implementado para depuração. Iniciar a depuração-lo no VS Code. Para mais informações sobre outros cenários quando estiver a desenvolver o limite de IoT do Azure no VS Code, consulte: 

> [!div class="nextstepaction"]
> [Desenvolver e implementar c# módulo no VS Code](how-to-vscode-develop-csharp-module.md)
