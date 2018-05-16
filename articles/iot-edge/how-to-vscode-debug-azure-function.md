---
title: Módulos de funções de depuração para o limite do Azure IoT | Microsoft Docs
description: Utilizar o Visual Studio Code para depurar c# das funções do Azure com o Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 3/20/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: cd870d8f5c3fff87b121ab777a086f21df07cfbc
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/14/2018
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para depurar as funções do Azure com o Azure IoT Edge

Este artigo fornece instruções detalhadas para utilizar [código Visual Studio (VS)](https://code.visualstudio.com/) para depurar as suas funções do Azure no limite de IoT.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT ser outro dispositivo físico ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

> [!NOTE]
> Apenas pode depurar c# funções nos contentores do linux amd64.

Antes de seguir as orientações neste artigo, concluir os passos [desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md). Depois disso, deve ter os seguintes itens prontos:
- Um registo de Docker local em execução no computador de desenvolvimento. É sugerida para utilizar um registo de Docker local para criar protótipos e fins de teste. Pode atualizar o registo de contentor do `module.json` ficheiro na pasta cada módulo.
- Uma limite de IoT solução projeto área de trabalho com uma subpasta do módulo de função do Azure no mesmo.
- O `run.csx` ficheiro com o código de função.
- Um tempo de execução de limite, em execução no computador de desenvolvimento.

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Criar o módulo de função de limite de IoT para fins de depuração
1. Para iniciar a depuração, tem de utilizar **Dockerfile.amd64.debug** para reconstruir a imagem de docker e implementar novamente a sua solução de ponta. No Explorador de VS Code, navegue para `deployment.template.json` ficheiro. Atualize o seu URL de imagem de função ao adicionar um `.debug` no fim.

    ![Compilar a imagem de depuração](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Reconstrua a sua solução. Na paleta do comando de VS Code, escreva e execute o comando **Edge: solução de ponta de IoT criar**.
3. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo de limite de IoT, em seguida, selecione **criar a implementação para o dispositivo de limite**. Selecione o `deployment.json` ficheiro sob o `config` pasta. Em seguida, pode ver que a implementação foi criada com êxito com uma implementação que ID no VS Code integrado terminal.

Pode verificar o estado do contentor no Explorador do Docker de código de VS ou executando o `docker images` comando no terminal.

## <a name="start-debugging-c-function-in-vs-code"></a>Iniciar a depuração de função de c# no VS Code
1. O VS Code mantém depuração informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isto `launch.json` ficheiro foi gerado quando criou uma nova solução de IoT Edge. Atualiza sempre que adicionar um novo módulo que suporta a depuração. Navegue para a vista de depuração e selecione o ficheiro de configuração de depuração correspondente.
    ![Configuração de depuração selecione](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navegue para `run.csx`. Adicione um ponto de interrupção na função.
3. Clique em de **iniciar depuração** botão ou prima **F5**e selecionar o processo para anexar a.
4. Na vista de depuração do código de VS, pode ver as variáveis no painel esquerdo. 


> [!NOTE]
> Acima exemplo mostra como depuração .net Core IoT Edge função nos contentores. Se baseia na versão de depuração do `Dockerfile.amd64.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) da imagem do contentor ao criá-lo. É recomendável utilizar ou personalizar diretamente o `Dockerfile` sem VSDBG para a função de limite de IoT prontos para produção depois de concluir a depuração da função de c#.

## <a name="next-steps"></a>Passos Seguintes


[Utilizar o Visual Studio Code para depurar um módulo de c# com o Azure IoT Edge](how-to-vscode-debug-csharp-module.md)

