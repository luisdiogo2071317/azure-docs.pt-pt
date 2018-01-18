---
title: "Utilizar o Visual Studio Code para depurar um módulo de c# com o Azure IoT Edge | Microsoft Docs"
description: "Depurar um módulo de c# com o Azure IoT Edge no VS Code"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1ab67cd8aaf59cde3157fcb877ce13f10cb432bb
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-debug-c-module-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para depuração c# módulo com o limite de IoT do Azure
Este artigo fornece instruções detalhadas para utilizar [Visual Studio Code](https://code.visualstudio.com/) como a ferramenta de desenvolvimento principal para depurar os módulos de limite de IoT.

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT ser outro dispositivo físico ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

Certifique-se de que ter tutoriais seguintes concluídas antes de começar esta orientação.
- [Utilizar o Visual Studio Code para desenvolver c# módulo com o Azure IoT Edge](how-to-vscode-develop-csharp-module.md)

Depois de concluir o tutorial anterior, deve ter os seguintes itens estiver prontos,
- Um registo de Docker local em execução no computador de desenvolvimento. É sugerida para utilizar um registo de Docker local para criar protótipos e fins de teste.
- O `Program.cs` ficheiro com o código mais recente do módulo de filtro.
- Um atualizado `deployment.json` ficheiro para o módulo de sensor e o módulo de filtragem.
- Um tempo de execução de limite, em execução no computador de desenvolvimento.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Criar o módulo de limite de IoT para fins de depuração
1. Para iniciar a depuração, tem de utilizar o **dockerfile.debug** para reconstruir a imagem de docker e implementar novamente a sua solução de ponta. No Explorador de VS Code, clique na pasta de Docker para abri-lo. Em seguida, clique no `linux-x64` pasta, faça duplo clique o **Dockerfile.debug**e clique em **imagem de Docker do módulo de limite de IoT criar**.

    ![Compilar a imagem de depuração](./media/how-to-debug-csharp-module/build-debug-image.png)

3. No **selecionar pasta** janela, procurar ou introduzir `./bin/Debug/netcoreapp2.0/publish`. Clique em **selecionar pasta como EXE_DIR**.
4. Na caixa de texto de pop-up na parte superior da janela VS Code, introduza o nome da imagem. Por exemplo: `<your container registry address>/filtermodule:latest`. Se estiver a implementar no registo local, deve ser `localhost:5000/filtermodule:latest`.
5. A imagem de emissão para o seu repositório de Docker. Utilize o **Edge: imagem de Docker do módulo de limite de Push de IoT** de comandos e introduza o URL da imagem na caixa de texto de pop-up na parte superior da janela VS Code. Utilize o mesmo URL de imagem utilizada nos passos acima.
6. Pode reutilizar o `deployment.json` para voltar a implementar. No comando paleta, escreva e selecione **Edge: reiniciar Edge** para obter o módulo de filtro com a versão de depuração.

## <a name="start-debugging-in-vs-code"></a>Iniciar a depuração no VS Code
1. Ir para a janela de depuração do código de VS. Prima **F5** e selecione **IoT Edge(.Net Core)**

    ![Prima F5](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. No `launch.json`, navegue para **depurar IoT Edge personalizada módulo (.NET Core)** secção e preencha as `<container_name>`em `pipeArgs`. Deve ser `filtermodule` neste tutorial.

    ![Modificar pipeArgs](./media/how-to-debug-csharp-module/f5-debug-option.png)

3. Navegue para Program.cs. Adicionar um ponto de interrupção no `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Prima **F5** novamente. E selecionar o processo para anexar a. Neste tutorial, deve ser o nome do processo`FilterModule.dll`

    ![Anexar processo](./media/how-to-debug-csharp-module/attach-process.png)

5. Na janela de depuração do código de VS, pode ver as variáveis no painel esquerdo. 

> [!NOTE]
> Acima exemplo mostra como .net depuração módulos de limite de IoT Core nos contentores. Se baseia na versão de depuração do `Dockerfile.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) da imagem do contentor ao criá-lo. É recomendável utilizar ou personalizar diretamente o `Dockerfile` sem VSDBG para módulos de limite de IoT prontos para produção depois de concluir a depuração os módulos c#.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, é criado um módulo de limite de IoT implementado para fins de depuração e iniciar depuração-lo no VS Code. Pode continuar a sessão em qualquer um dos seguintes tutoriais para saber mais sobre outros cenários quando desenvolver Azure IoT Edge no VS Code. 

> [!div class="nextstepaction"]
> [Desenvolver e implementar c# módulo no VS Code](how-to-vscode-develop-csharp-module.md)
