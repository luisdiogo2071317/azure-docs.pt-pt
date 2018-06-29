---
title: Depurar módulos do Node.js para o limite do Azure IoT | Microsoft Docs
description: Utilizar o Visual Studio Code para desenvolver e depurar módulos do Node.js para o limite de IoT do Azure
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 6b217690b88f303268f5abe66abb7868711d3125
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045097"
---
# <a name="develop-and-debug-nodejs-modules-with-azure-iot-edge-for-visual-studio-code"></a>Desenvolver e depurar módulos do Node.js com limite de IoT do Azure para Visual Studio Code

Pode enviar a lógica de negócio a funcionar no limite, ativá-la em módulos de limite de IoT do Azure. Este artigo fornece instruções detalhadas para utilizar o Visual Studio Code (VS Code) como a ferramenta de desenvolvimento principal para desenvolver módulos c#.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT pode ser outro dispositivo físico, ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

> [!NOTE]
> Este tutorial de depuração descreve como anexar um processo no contentor de um módulo e depurá-lo com o código de VS. Pode depurar módulos do Node.js em linux amd64, windows e arm32 contentores. Se não estiver familiarizado com as funcionalidades de depuração do Visual Studio Code, leia sobre [a depuração](https://code.visualstudio.com/Docs/editor/debugging). 

Uma vez que este artigo utiliza o Visual Studio Code como a ferramenta de desenvolvimento principal, instale o VS Code e, em seguida, adicionar as extensões necessárias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão de limite de IoT do Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensão de docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para criar um módulo, terá de Node.js que inclui npm para criar a pasta do projeto, Docker para criar a imagem de módulo e um registo de contentor para armazenar a imagem de módulo:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) ou [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   >[!TIP]
   >Pode utilizar um registo de Docker local para criar protótipos e efeitos de teste, em vez de um registo de nuvem. 

Para testar o seu módulo num dispositivo, terá um IoT hub Active Directory com, pelo menos, um dispositivo de limite de IoT. Se pretender utilizar o computador como um dispositivo de limite de IoT, pode fazê-seguindo os passos nos tutoriais para [Windows](quickstart.md) ou [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Criar um novo modelo de solução

Os passos seguintes mostram-lhe como criar um módulo de limite de IoT com base no .NET Core 2.0 utilizando o Visual Studio Code e a extensão de limite de IoT do Azure. Iniciar criação de uma solução e, em seguida, a gerar o módulo primeiro essa solução. Cada solução pode conter vários módulos. 

1. No Visual Studio Code, selecione **vista** > **Terminal integrada**.
2. No terminal integrado, introduza o seguinte comando para instalar (ou Atualize) a versão mais recente do modelo de módulo de limite de IoT do Azure para Node.js:

   ```cmd/sh
   npm install -g yo generator-azure-iot-edge-module
   ```
3. No Visual Studio Code, selecione **Vista** > **Paleta de Comando**. 
4. Na paleta do comando, escreva e execute o comando **contorno de IoT do Azure: solução de ponta IoT novo**.

   ![Executar a nova solução de IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Procure a pasta onde pretende criar a nova solução e clique em **pasta selecione**. 
6. Forneça um nome para a sua solução. 
7. Escolha **Node.js módulo** como modelo para o módulo primeiro na solução.
8. Forneça um nome para o módulo. Escolha um nome exclusivo no seu registo de contentor. 
9. Forneça o repositório de imagens para o módulo. Código de VS autopopulates o módulo name, pelo que apenas terá de substituir **localhost:5000** pelas suas informações de registo. Se utilizar um registo de Docker local para fins de teste, é adequado localhost. Se utilizar o registo de contentor do Azure, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão aspeto  **\<nome do registo\>. azurecr.io**.

O VS Code tem as informações fornecidas, cria uma solução de IoT Edge e carrega-os numa nova janela.

Na solução de tem três itens: 
* A **.vscode** pasta contém configurações de depuração.
* A **módulos** pasta contém subpastas para cada módulo. Com o botão direito agora tiver apenas uma, mas pode adicionar mais na paleta do comando com o comando **limite do Azure IoT: Adicionar módulo de limite de IoT**. 
* A **.env** ficheiro apresenta uma lista de variáveis do seu ambiente. Se forem ACR como o registo correto agora tiver ACR nome de utilizador e palavra-passe no mesmo. 
* A **deployment.template.json** ficheiro de lista do novo módulo, juntamente com uma amostra **tempSensor** módulo que simula dados que pode utilizar para fins de teste. Para obter mais informações sobre como implementação manifestos de trabalho, consulte [compreender como os módulos de IoT limite podem ser utilizados, configurados e reutilizados](module-composition.md).

## <a name="build-and-deploy-your-module-for-debugging"></a>Criar e implementar o módulo de depuração

Na pasta cada módulo, existem vários ficheiros de Docker para tipos de contentor diferente. Pode utilizar qualquer um destes ficheiros que terminem com a extensão **.debug** para criar o módulo para fins de teste. Atualmente, o c# módulos só suportam a depuração em contentores de linux amd64.

1. No VS Code, navegue para o `deployment.template.json` ficheiro. Substitua o createOptions do módulo de Node.js no **deployment.template.json** com abaixo conteúdo e guarde este ficheiro: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

2. Na paleta de comando VS Code, escreva e execute o comando **contorno de IoT do Azure: solução de ponta de IoT criar**.
3. Selecione o `deployment.template.json` ficheiro para a sua solução de paleta do comando. 
4. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo de limite de IoT, em seguida, selecione **criar a implementação para o dispositivo de limite de IoT**. 
5. Abra o **configuração** pasta da sua solução, em seguida, selecione o `deployment.json` ficheiro. Clique em **selecionar o manifesto de implementação de limite**. 

Em seguida, pode ver que a implementação foi criada com êxito com uma implementação que ID no VS Code integrado terminal.

Pode verificar o estado do contentor no Explorador do Docker de código de VS ou ao executar o `docker ps` comando no terminal.

## <a name="start-debugging-nodejs-module-in-vs-code"></a>Iniciar a depuração do módulo de Node.Js no VS Code

O VS Code mantém depuração informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isto `launch.json` ficheiro foi gerado quando criou uma nova solução de IoT Edge. Atualiza sempre que adicionar um novo módulo que suporta a depuração. 

1. Navegue para a vista de depuração VS Code e selecione o ficheiro de configuração de depuração para o módulo.

2. Navegue para `app.js`. Adicione um ponto de interrupção neste ficheiro.

3. Clique em de **iniciar depuração** botão ou prima **F5**e selecionar o processo para anexar a.

4. Na vista de depuração do código de VS, pode ver as variáveis no painel esquerdo. 

O exemplo anterior mostra como depurar módulos do Node.js IoT Edge nos contentores. -Adicionar portas expostas no seu createOptions de contentor do módulo. Depois de concluir os módulos do Node.js a depuração, recomendamos que remova estas portas expostas para módulos de limite de IoT prontos para produção.

## <a name="next-steps"></a>Passos Seguintes

Depois de ter o módulo incorporado, saiba como [módulos de implementar o Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md)

