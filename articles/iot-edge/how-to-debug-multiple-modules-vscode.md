---
title: Vários módulos de depuração para o Azure IoT Edge no VS Code | Microsoft Docs
description: Utilize o Visual Studio Code para vários módulos de depuração com o Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 38c66129ac8244d18b0f94c1485c785015e29ab4
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049596"
---
# <a name="use-visual-studio-code-to-debug-multiple-modules-with-azure-iot-edge"></a>Utilize o Visual Studio Code para vários módulos de depuração com o Azure IoT Edge
Este artigo fornece instruções detalhadas para utilizar [código Visual Studio (VS)](https://code.visualstudio.com/) para depurar vários módulos no limite de IoT.

## <a name="prerequisites"></a>Pré-requisitos
Concluir o tutorial [desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md) e certifique-se de que tem, pelo menos, dois módulos em execução no seu dispositivo de limite de IoT.

## <a name="multi-target-and-remote-debugging-in-vs-code"></a>A depuração remota e de destino multi no VS Code
Com a extensão VS Code e o limite de IoT do Azure, pode anexar o processo do módulo num contentor, se o contentor está em execução no computador de desenvolvimento ou um dispositivo de limite de IoT físico remoto. Depuração mutiple módulos em execução em contentores, na verdade, está a expor o mais do que um processo em contentores separados. O VS Code [destino multi depuração](https://code.visualstudio.com/docs/editor/debugging#_multitarget-debugging) podem ser utilizados quando a depuração mutiple módulos.

   > [!TIP]
   > Se o contentor do módulo está a ser executado um dispositivo de limite de IoT físico remoto, poderá ter de configuração [Docker máquina](https://docs.docker.com/machine/overview/) para que o motor de Docker no computador de desenvolvimento pode comunicar com os anfitriões de Docker remotos.

### <a name="build-your-iot-edge-modules-for-debugging-purpose"></a>Criar o seu limite de IoT módulos para fins de depuração
1. Para iniciar a depuração de múltiplos módulos, tem de utilizar **Dockerfile.amd64.debug** para reconstruir as imagens de docker e implementar novamente a sua solução de ponta. No Explorador de VS Code, navegue para `deployment.template.json` ficheiro. Atualizar o seu URL de imagem, adicionando um `.debug` no fim. Terá de duas imagens de módulo com `.debug` pelo menos. Se estiver a trabalhar na solução de tutorial anterior, deverá ter um módulo de funções do c# e um módulo do c#. Atualizar dois imagem URLs, adicionando um `.debug` no fim e guarde este ficheiro. 
2. Reconstrua a sua solução. Na paleta do comando de VS Code, escreva e execute o comando **contorno de IoT do Azure: solução de ponta de IoT criar**.
3. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo de limite de IoT, em seguida, selecione **criar a implementação para o dispositivo de limite**. Selecione o `deployment.json` ficheiro sob o `config` pasta. Em seguida, pode ver que a implementação foi criada com êxito com uma implementação que ID no VS Code integrado terminal.

Pode verificar o estado do contentor no Explorador do Docker de código de VS ou executando o `docker ps` comando no terminal.

### <a name="start-debugging-c-function-in-vs-code"></a>Iniciar a depuração de função de c# no VS Code
1. O VS Code mantém depuração informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isto `launch.json` ficheiro foi gerado quando criou uma nova solução de IoT Edge. Atualiza sempre que adicionar um novo módulo que suporta a depuração. Navegue para a vista de depuração e selecione o ficheiro de configuração de depuração correspondente para c# funções módulo depuração remota.
2. Navegue para `run.csx`. Adicione um ponto de interrupção na função.
3. Clique em de **iniciar depuração** botão ou prima **F5**e selecionar o processo para anexar a.
4. Na vista de depuração do código de VS, pode ver as variáveis no painel esquerdo. 

### <a name="start-debugging-c-module-at-the-same-time-in-vs-code"></a>Iniciar a depuração c# módulo ao mesmo tempo no VS Code
1. Na paleta do comando de VS Code, escreva e execute o comando "Área de trabalho: duplicado área de trabalho na nova janela". Inicia uma nova janela do VS Code com o mesmo espaço de trabalho.
2. Navegue para a vista de depuração e selecione o ficheiro de configuração de depuração correspondente para c# módulo depuração remota.
3. Navegue para `program.cs`. Adicione um ponto de interrupção no módulo c#.
4. Clique em de **iniciar depuração** botão ou prima **F5**e selecionar o processo para anexar a.
5. Na vista de depuração do código de VS, pode ver as variáveis no painel esquerdo. 

### <a name="see-variables-in-multiple-debugging-windows"></a>Consulte as variáveis em várias janelas de depuração
1. Agora, tem pelo menos, duas em execução na janela de código de VS duas de sessão de depuração. Deve ser atingiu um o ponto de interrupção.
2. Prima `F10` ou clique no botão do passo no **barra de ferramentas de depuração**.
3. Deve ser atingiu o ponto de interrupção na outra janela do VS Code. 
4. Continue acima dois passos, pode ver as variáveis de módulos de mutilple no vários VS Code depuração do windows.

> [!NOTE]
> Acima exemplo mostra como a depuração de vários módulos com limite de IoT do Azure. Se baseia na versão de depuração do `Dockerfile.amd64.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) da imagem do contentor ao criá-lo. É recomendável utilizar ou personalizar diretamente o `Dockerfile` sem VSDBG para a função de limite de IoT prontos para produção depois de concluir a depuração da função de c#.

## <a name="next-steps"></a>Passos Seguintes

Depois de ter o módulo incorporado, saiba como [módulos de implementar o Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md) 0
