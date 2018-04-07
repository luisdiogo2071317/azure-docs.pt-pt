---
title: Utilizar o Visual Studio Code para depurar um módulo de c# com o Azure IoT Edge | Microsoft Docs
description: Faça a depuração de um módulo de c# com o limite de IoT do Azure no Visual Studio Code.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 65f2fb4526f1048ae88193f85a552a2202afa1d9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para depurar um módulo de c# com o Azure IoT Edge
Este artigo fornece instruções detalhadas para utilizar [Visual Studio Code](https://code.visualstudio.com/) como a ferramenta de desenvolvimento principal para os módulos do Azure IoT contorno de depuração.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT pode ser outro dispositivo físico, ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

Antes de seguir as orientações neste artigo, concluir os passos [desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md). Depois disso, deve ter os seguintes itens prontos:
- Um registo de Docker local em execução no computador de desenvolvimento. É sugerida para utilizar um registo de Docker local para criar protótipos e fins de teste. Pode atualizar o registo de contentor do `module.json` ficheiro na pasta cada módulo.
- Uma limite de IoT solução projeto área de trabalho com c# módulo subpasta na mesma.
- O `Program.cs` ficheiros, com o código mais recente do módulo.
- Um tempo de execução de limite, em execução no computador de desenvolvimento.

## <a name="build-your-iot-edge-c-module-for-debugging"></a>Criar o módulo de IoT Edge c# para depuração
1. Para iniciar a depuração, tem de utilizar o **Dockerfile.amd64.debug** para reconstruir a imagem de docker e implementar novamente a sua solução de ponta. No Explorador de VS Code, navegue para `deployment.template.json` ficheiro. Atualize o seu URL de imagem de função ao adicionar um `.debug` no fim.

2. Reconstrua a sua solução. Na paleta do comando de VS Code, escreva e execute o comando **Edge: solução de ponta de IoT criar**.

3. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo de limite de IoT, em seguida, selecione **criar a implementação para o dispositivo de limite**. Selecione o `deployment.json` em `config` pasta. Em seguida, pode ver que a implementação foi criada com êxito com uma implementação que ID no VS Code integrado terminal.

> [!NOTE]
> Pode verificar o estado do contentor no Explorador do Docker de código de VS ou ao executar o `docker images` comando no terminal.

## <a name="start-debugging-c-module-in-vs-code"></a>Iniciar a depuração c# módulo no VS Code
1. O VS Code mantém depuração informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isto `launch.json` ficheiro ter sido gerado ao criar uma nova solução de IoT Edge. E será atualizada sempre que adicionar um novo módulo que suportam a depuração. Navegue para a vista de depuração e selecione o ficheiro de configuração de depuração correspondente.
    ![Configuração de depuração selecione](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navegue para `program.cs`. Adicione um ponto de interrupção neste ficheiro.

3. Clique no botão Iniciar depuração ou prima **F5**e selecionar o processo para anexar a.

4. Na vista de depuração do código de VS, pode ver as variáveis no painel esquerdo. 

> [!NOTE]
> O exemplo anterior mostra como depurar módulos .NET Core IoT Edge nos contentores. Se baseia na versão de depuração do `Dockerfile.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) da imagem do contentor ao criá-lo. Depois de concluir a depuração os módulos c#, é recomendável utilizar ou personalizar diretamente `Dockerfile` sem VSDBG para módulos de limite de IoT prontos para produção.

## <a name="next-steps"></a>Passos Seguintes

[Utilizar o Visual Studio Code para depurar as funções do Azure com o Azure IoT Edge](how-to-vscode-debug-azure-function.md)

