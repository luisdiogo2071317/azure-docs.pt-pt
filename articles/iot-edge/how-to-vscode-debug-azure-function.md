---
title: "Utilizar o Visual Studio Code para depurar as funções do Azure com o Azure IoT Edge | Microsoft Docs"
description: "Depurar c# as funções do Azure com o Azure IoT Edge no VS Code"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: db86a08a19e97f8f415849aa060fe87d77cccf68
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para depurar as funções do Azure com o Azure IoT Edge

Este artigo fornece instruções detalhadas para utilizar [Visual Studio Code](https://code.visualstudio.com/) como a ferramenta de desenvolvimento principal para depurar as suas funções do Azure no limite de IoT.

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT ser outro dispositivo físico ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

Certifique-se de que ter tutoriais seguintes concluídas antes de começar esta orientação.
- [Utilizar o Visual Studio Code para desenvolver e implementar as funções do Azure para o limite de IoT do Azure](how-to-vscode-develop-azure-function.md)

Depois de concluir o tutorial anterior, deve ter os seguintes itens estiver prontos,
- Um registo de Docker local em execução no computador de desenvolvimento. É sugerida para utilizar um registo de Docker local para criar protótipos e fins de teste.
- O `run.csx` ficheiro com o código de função de filtro mais recente.
- Um atualizado `deployment.json` ficheiro para o módulo de sensor e o módulo de filtragem de função.
- Um tempo de execução de limite, em execução no computador de desenvolvimento.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Criar o módulo de limite de IoT para fins de depuração
1. Para iniciar a depuração, tem de utilizar o **dockerfile.debug** para reconstruir a imagem de docker e implementar novamente a sua solução de ponta. No Explorador de VS Code, clique na pasta de Docker para abri-lo. Em seguida, clique no `linux-x64` pasta, faça duplo clique o **Dockerfile.debug**e clique em **imagem de Docker do módulo de limite de IoT criar**.

    ![Compilar a imagem de depuração](./media/how-to-debug-csharp-function/build-debug-image.png)

2. No **selecionar pasta** janela, navegue para o **FilterFunction** projeto e clique em **selecionar pasta como EXE_DIR**.
3. Na caixa de texto de pop-up na parte superior da janela VS Code, introduza o nome da imagem. Por exemplo: `<your container registry address>/filterfunction:latest`. Se estiver a implementar no registo local, deve ser `localhost:5000/filterfunction:latest`.

    ![Imagem de push](./media/how-to-debug-csharp-function/push-image.png)

4. A imagem de emissão para o seu repositório de Docker. Utilize o **Edge: imagem de Docker do módulo de limite de Push de IoT** de comandos e introduza o URL da imagem na caixa de texto de pop-up na parte superior da janela VS Code. Utilize o mesmo URL de imagem utilizada nos passos acima.
5. Pode reutilizar o `deployment.json` para voltar a implementar. No comando paleta, escreva e selecione **Edge: reiniciar Edge** para obter a função de filtro com a versão de depuração.

## <a name="start-debugging-in-vs-code"></a>Iniciar a depuração no VS Code
1. Ir para a janela de depuração do código de VS. Prima **F5** e selecione **IoT Edge(.Net Core)**

    ![Prima F5](./media/how-to-debug-csharp-function/f5-debug-option.png)

2. No `launch.json`, navegue para **depurar a função de limite IoT (.NET Core)** secção e preencha as `<container_name>`em `pipeArgs`. Deve ser `filterfunction` neste tutorial.

    ![Atualizar launch.json](./media/how-to-debug-csharp-function/update-launch-json.png)

3. Navegue para run.csx. Adicione um ponto de interrupção na função.
4. Navegue até à janela de depuração (Ctrl + Shift + D), escolha **depurar a função de limite IoT (.NET Core)** na lista pendente. 

    ![Selecione o modo de depuração](./media/how-to-debug-csharp-function/choose-debug-mode.png)

5. Clique no botão Iniciar depuração ou prima **F5**e selecionar o processo para anexar a.

    ![Anexar o processo de função](./media/how-to-debug-csharp-function/attach-function-process.png)

6. Na janela de depuração do código de VS, pode ver as variáveis no painel esquerdo. 

> [!NOTE]
> Acima exemplo mostra como depuração .net Core IoT Edge função nos contentores. Se baseia na versão de depuração do `Dockerfile.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) da imagem do contentor ao criá-lo. É recomendável utilizar ou personalizar diretamente o `Dockerfile` sem VSDBG para a função de limite de IoT prontos para produção depois de concluir a depuração da função de c#.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou uma função do Azure e implementado a extremidade de IoT para fins de depuração e iniciar depuração-lo no VS Code. Pode continuar a sessão em qualquer um dos seguintes tutoriais para saber mais sobre outros cenários quando desenvolver Azure IoT Edge no VS Code. 

> [!div class="nextstepaction"]
> [Desenvolver e implementar c# módulo no VS Code](how-to-vscode-develop-csharp-module.md)

