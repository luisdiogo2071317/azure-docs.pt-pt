---
title: Módulos de funções de depuração para o limite do Azure IoT | Microsoft Docs
description: Utilizar o Visual Studio Code para depurar c# das funções do Azure com o Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/26/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5ee5d368bde0f176442a01ba266791f8a9c2cbc3
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37052832"
---
# <a name="use-visual-studio-code-to-develop-and-debug-azure-functions-for-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver e depuração das funções do Azure para o limite de IoT do Azure

Este artigo fornece instruções detalhadas para utilizar [código Visual Studio (VS)](https://code.visualstudio.com/) para depurar as suas funções do Azure no limite de IoT.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT ser outro dispositivo físico ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

> [!NOTE]
> Este tutorial de depuração descreve como anexar um processo no contentor de um módulo e depurá-lo com o código de VS. Apenas pode depurar módulos c# nos contentores do linux amd64. Se não estiver familiarizado com as funcionalidades de depuração do Visual Studio Code, leia sobre [a depuração](https://code.visualstudio.com/Docs/editor/debugging). 

Este artigo utiliza o Visual Studio Code como a ferramenta de desenvolvimento principal. Instalar o VS Code, em seguida, adicionar as extensões necessárias: 

* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão de limite de IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensão de c#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Extensão de docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para criar um módulo, terá de .NET para criar a pasta do projeto, Docker para criar a imagem de módulo e um registo de contentor para armazenar a imagem de módulo:
* [.NET core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/) no computador de desenvolvimento. 
* [Registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) ou [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   > [!TIP]
   > Pode utilizar um registo de Docker local para criar protótipos e efeitos de teste, em vez de um registo de nuvem. 

Para testar o seu módulo num dispositivo, terá um IoT hub Active Directory com, pelo menos, um dispositivo de limite de IoT. Se pretender utilizar o computador como um dispositivo de limite de IoT, pode fazê-seguindo os passos nos tutoriais para [Windows](quickstart.md) ou [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Criar um novo modelo de solução

Os passos seguintes mostram como criar uma solução de IoT limite que contém o módulo de função um c#. Cada solução pode conter vários módulos.

1. No Visual Studio Code, selecione **vista** > **Terminal integrada**.
3. Selecione **vista** > **comando paleta**.
4. Na paleta do comando, escreva e execute o comando **contorno de IoT do Azure: solução de ponta IoT novo**. 

   ![Executar a nova solução de IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Procure a pasta onde pretende criar a nova solução e clique em **pasta selecione**. 
6. Forneça um nome para a sua solução. 
7. Escolha **das funções do Azure - c#** como modelo para o módulo primeiro na solução.
8. Forneça um nome para o módulo. Escolha um nome exclusivo no seu registo de contentor. 
9. Forneça o repositório de imagens para o módulo. Código de VS autopopulates o módulo name, pelo que apenas terá de substituir **localhost:5000** pelas suas informações de registo. Se utilizar um registo de Docker local para fins de teste, é adequado localhost. Se utilizar o registo de contentor do Azure, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão aspeto  **\<nome do registo\>. azurecr.io**.

O VS Code tem as informações fornecidas, cria uma solução de IoT limite com um projeto de função e carrega-os numa nova janela.

Na solução de tem três itens: 

* A **.vscode** pasta que contém as configurações de depuração.
* A **módulos** pasta que contém as subpastas para cada módulo. Com o botão direito agora tiver apenas uma, mas pode adicionar mais através de paleta de comando com o comando **limite do Azure IoT: Adicionar módulo de limite de IoT**.
* A **.env** ficheiro apresenta uma lista de variáveis do seu ambiente. Se forem ACR como o registo correto agora tiver ACR nome de utilizador e palavra-passe no mesmo. 
* A **deployment.template.json** ficheiro de lista do novo módulo, juntamente com uma amostra **tempSensor** módulo, que simula dados que pode utilizar para fins de teste. Para obter mais informações sobre como implementação manifestos de trabalho, consulte [compreender como os módulos de IoT limite podem ser utilizados, configurados e reutilizados](module-composition.md).

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Criar o módulo de função de limite de IoT para fins de depuração
1. Para iniciar a depuração, tem de utilizar **Dockerfile.amd64.debug** para reconstruir a imagem de docker e implementar novamente a sua solução de ponta. No Explorador de VS Code, navegue para `deployment.template.json` ficheiro. Atualize o seu URL de imagem de função ao adicionar um `.debug` no fim.

    ![Compilar a imagem de depuração](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Reconstrua a sua solução. Na paleta do comando de VS Code, escreva e execute o comando **contorno de IoT do Azure: solução de ponta de IoT criar**.
3. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo de limite de IoT, em seguida, selecione **criar a implementação para o dispositivo de limite**. Selecione o `deployment.json` ficheiro sob o `config` pasta. Em seguida, pode ver que a implementação foi criada com êxito com uma implementação que ID no VS Code integrado terminal.

Pode verificar o estado do contentor no Explorador do Docker de código de VS ou executando o `docker images` comando no terminal.

## <a name="start-debugging-c-function-in-vs-code"></a>Iniciar a depuração de função de c# no VS Code
1. O VS Code mantém depuração informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isto `launch.json` ficheiro foi gerado quando criou uma nova solução de IoT Edge. Atualiza sempre que adicionar um novo módulo que suporta a depuração. Navegue para a vista de depuração e selecione o ficheiro de configuração de depuração correspondente. O nome da opção de depuração deve ser como **ModuleName depuração remota (.NET Core)** ![depuração selecione configuração](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. Navegue para `run.csx`. Adicione um ponto de interrupção na função.
3. Clique em de **iniciar depuração** botão ou prima **F5**e selecionar o processo para anexar a.
4. Na vista de depuração do código de VS, pode ver as variáveis no painel esquerdo. 


> [!NOTE]
> A exemplo acima mostra como depuração .net Core IoT Edge função nos contentores. Se baseia na versão de depuração do `Dockerfile.amd64.debug`, que inclui VSDBG (o depurador da linha de comandos do .NET Core) da imagem do contentor ao criá-lo. É recomendável utilizar ou personalizar diretamente o `Dockerfile` sem VSDBG para a função de limite de IoT prontos para produção depois de concluir a depuração da função de c#.

## <a name="next-steps"></a>Passos Seguintes

Depois de ter o módulo incorporado, saiba como [módulos de implementar o Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md)
