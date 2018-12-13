---
title: Depurar vários módulos no Visual Studio Code - Azure IoT Edge | Documentos da Microsoft
description: Utilizar o Visual Studio Code para depurar vários módulos com o Azure IoT Edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0c421eb1532536a3d11013073f0474f5ac37311b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100460"
---
# <a name="use-visual-studio-code-to-debug-multiple-modules-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para depurar vários módulos com o Azure IoT Edge
Este artigo fornece instruções detalhadas para usar [código do Visual Studio (VS)](https://code.visualstudio.com/) para depurar vários módulos do IoT Edge.

## <a name="prerequisites"></a>Pré-requisitos
Concluir o tutorial [desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md) e certifique-se de que tem, pelo menos, dois módulos em execução no seu dispositivo IoT Edge.

## <a name="multi-target-and-remote-debugging-in-vs-code"></a>No VS Code de depuração remota e de destino multi
Com a extensão do VS Code e o Azure IoT Edge, pode anexar o processo de módulo num contentor, se o contentor está em execução no computador de desenvolvimento ou num dispositivo IoT Edge físico remoto. Módulos de vários conjuntos de depuração em execução em contentores, na verdade, é anexar mais do que um processo em contentores separados. Código VS [destino multi depuração](https://code.visualstudio.com/docs/editor/debugging#_multitarget-debugging) podem ser utilizados quando vários conjuntos módulos de depuração.

   > [!TIP]
   > Se o seu contentor de módulo for executada no dispositivo IoT Edge remoto, precisará de configuração [Docker Machine](https://docs.docker.com/machine/overview/) para que o motor do Docker no seu computador de desenvolvimento pode se comunicar com os anfitriões do Docker remotos.

### <a name="build-your-iot-edge-modules-for-debugging-purpose"></a>Criar o IoT Edge módulos para fins de depuração
1. Para iniciar a depuração de vários módulos, tem de utilizar **Dockerfile.amd64.debug** para reconstruir as imagens do docker e implemente a sua solução de Edge novamente. No Explorador do VS Code, navegue para `deployment.template.json` ficheiro. Atualizar os URLs de imagens ao adicionar um `.debug` no final da contas. Tem duas imagens de módulo com `.debug` pelo menos. Se estiver a trabalhar na solução do tutorial anterior, deve ter um C# módulo de funções e um C# módulo. Atualizar estes URLs de duas imagens ao adicionar um `.debug` no final da contas e guarde este ficheiro. 
2. Reconstrua a sua solução. Na paleta de comandos do VS Code, escreva e execute o comando **do Azure IoT Edge: solução de IoT Edge criar**.
3. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo do IoT Edge, em seguida, selecione **criar implementação para o dispositivo de limite**. Selecione o `deployment.json` de ficheiros sob o `config` pasta. Em seguida, pode ver que a implementação é criada com êxito com uma implantação integrado de ID no VS Code terminal.

Pode verificar o estado do contentor no Explorador do Docker de código do VS ou ao executar o `docker ps` comando no terminal.

### <a name="start-debugging-c-function-in-vs-code"></a>Iniciar a depuração C# função no VS Code
1. Mantém depuração de código de VS informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isso `launch.json` arquivo foi gerado quando criou uma nova solução de IoT Edge. Ele atualiza cada vez que adiciona um novo módulo que oferece suporte à depuração. Navegue para a vista de depuração e selecione o ficheiro de configuração de depuração correspondente para o C# depuração remota do funções módulo.
2. Navegue para `run.csx`. Adicione um ponto de interrupção na função.
3. Clique nas **iniciar depuração** botão ou prima **F5**e selecione o processo para anexar a.
4. Na vista de depuração de código do VS, pode ver as variáveis no painel esquerdo. 

### <a name="start-debugging-c-module-at-the-same-time-in-vs-code"></a>Iniciar a depuração C# módulo ao mesmo tempo no VS Code
1. Na paleta de comandos do VS Code, digite e execute o comando "Área de trabalho: duplicar área de trabalho na nova janela". Inicia uma nova janela do VS Code com a mesma área de trabalho.
2. Navegue para a vista de depuração e selecione o ficheiro de configuração de depuração correspondente para o C# depuração remota do módulo.
3. Navegue para `program.cs`. Adicionar um ponto de interrupção no C# módulo.
4. Clique nas **iniciar depuração** botão ou prima **F5**e selecione o processo para anexar a.
5. Na vista de depuração de código do VS, pode ver as variáveis no painel esquerdo. 

### <a name="see-variables-in-multiple-debugging-windows"></a>Ver variáveis em várias janelas de depuração
1. Agora tem, pelo menos, dois em dois janela do VS Code de sessão de depuração. Um ponto de interrupção deve ser atingido.
2. Prima `F10` ou clique no botão Pular no **barra de ferramentas de depuração**.
3. O ponto de interrupção na outra janela do VS Code deve ser atingido. 
4. Continue acima duas etapas, pode ver variáveis a partir de módulos de mutilple em vários VS Code, depuração windows.

> [!NOTE]
> Acima de exemplo mostra como a depuração de vários módulos com o Azure IoT Edge. Baseia-se a versão de depuração a `Dockerfile.amd64.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) na sua imagem de contentor ao criá-lo. É recomendável utilizar ou personalizar diretamente a `Dockerfile` sem VSDBG para a função de IoT Edge prontos para produção depois de concluir a depuração de seu C# função.

## <a name="next-steps"></a>Passos Seguintes

Assim que tiver o seu módulo criado, saiba como [módulos de implementar o Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md) 0
