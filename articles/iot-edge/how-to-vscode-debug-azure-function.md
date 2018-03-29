---
title: Utilizar o Visual Studio Code para depurar as funções do Azure com o Azure IoT Edge | Microsoft Docs
description: Depurar c# as funções do Azure com o Azure IoT Edge no VS Code
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 3/20/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 8da16ffe72ad265f0201c2fe7e00e585dfa255e8
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para depurar as funções do Azure com o Azure IoT Edge

Este artigo fornece instruções detalhadas para utilizar [Visual Studio Code](https://code.visualstudio.com/) como a ferramenta de desenvolvimento principal para depurar as suas funções do Azure no limite de IoT.

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT ser outro dispositivo físico ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

Certifique-se de que ter tutoriais seguintes concluídas antes de começar esta orientação.
- [Desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md)

Depois de concluir o tutorial anterior, deve ter os seguintes itens estiver prontos,
- Um registo de Docker local em execução no computador de desenvolvimento. É sugerida para utilizar um registo de Docker local para criar protótipos e fins de teste. Pode atualizar o registo de contentor do `module.json` ficheiro na pasta cada módulo.
- Uma limite de IoT solução projeto área de trabalho com uma subpasta do módulo de função do Azure no mesmo.
- O `run.csx` ficheiro com o código de função.
- Um tempo de execução de limite, em execução no computador de desenvolvimento.

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Criar o módulo de função de limite de IoT para fins de depuração
1. Para iniciar a depuração, tem de utilizar o **Dockerfile.amd64.debug** para reconstruir a imagem de docker e implementar novamente a sua solução de ponta. No Explorador de VS Code, navegue para `deployment.template.json` ficheiro. Atualize o seu URL de imagem de função ao adicionar um `.debug` no fim.

    ![Compilar a imagem de depuração](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Reconstrua a sua solução. Na paleta do comando de VS Code, escreva e execute o comando **Edge: solução de ponta de IoT criar**.

3. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo de limite de IoT, em seguida, selecione **criar a implementação para o dispositivo de limite**. Selecione o `deployment.json` em `config` pasta. Em seguida, pode ver que a implementação foi criada com êxito com uma implementação que ID no VS Code integrado terminal.

> [!NOTE]
> Pode verificar o estado do contentor no Explorador do Docker de código de VS ou ao executar o `docker images` comando no terminal.

## <a name="start-debugging-c-function-in-vs-code"></a>Iniciar a depuração de função de c# no VS Code
1. O VS Code mantém depuração informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isto `launch.json` ficheiro ter sido gerado ao criar uma nova solução de IoT Edge. E será atualizada sempre que adicionar um novo módulo que suportam a depuração. Navegue para a vista de depuração e selecione o ficheiro de configuração de depuração correspondente.
    ![Configuração de depuração selecione](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navegue para `run.csx`. Adicione um ponto de interrupção na função.

3. Clique no botão Iniciar depuração ou prima **F5**e selecionar o processo para anexar a.

4. Na vista de depuração do código de VS, pode ver as variáveis no painel esquerdo. 


> [!NOTE]
> Acima exemplo mostra como depuração .net Core IoT Edge função nos contentores. Se baseia na versão de depuração do `Dockerfile.amd64.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) da imagem do contentor ao criá-lo. É recomendável utilizar ou personalizar diretamente o `Dockerfile` sem VSDBG para a função de limite de IoT prontos para produção depois de concluir a depuração da função de c#.

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou uma função do Azure e implementado a extremidade de IoT para fins de depuração e iniciar depuração-lo no VS Code. Pode continuar a sessão em qualquer um dos seguintes tutoriais para saber mais sobre outros cenários quando desenvolver Azure IoT Edge no VS Code. 

> [!div class="nextstepaction"]
> [Desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md)

