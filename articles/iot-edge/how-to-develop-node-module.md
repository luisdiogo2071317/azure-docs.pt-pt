---
title: Depurar módulos node. js para o Azure IoT Edge | Documentos da Microsoft
description: Utilizar o Visual Studio Code para desenvolver e depurar módulos node. js para o Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: fbf8a0302ae8414f49a055ab260a52222425ab7c
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287199"
---
# <a name="develop-and-debug-nodejs-modules-with-azure-iot-edge-for-visual-studio-code"></a>Desenvolver e depurar módulos node. js com o Azure IoT Edge para Visual Studio Code

Pode enviar a lógica de negócio a funcionar no limite, ativá-la em módulos do Azure IoT Edge. Este artigo fornece instruções detalhadas para utilizar o Visual Studio Code (código de VS) como a ferramenta de desenvolvimento principal para desenvolver módulos do node. js.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que está a utilizar um computador ou máquina virtual a executar o Windows ou Linux da máquina de desenvolvimento. Dispositivo IoT Edge pode ser outro dispositivo físico ou pode simular o seu dispositivo IoT Edge no computador de desenvolvimento.

> [!NOTE]
> Este tutorial de depuração descreve como anexar um processo num contêiner de módulo e depurá-lo com o VS Code. Pode depurar módulos do node. js no linux-amd64, windows e arm32 contentores. Se não estiver familiarizado com as capacidades de depuração do Visual Studio Code, ler sobre [Debugging](https://code.visualstudio.com/Docs/editor/debugging). 

Uma vez que este artigo usa código do Visual Studio como a ferramenta de desenvolvimento principal, instalar o VS Code e, em seguida, adicione as extensões necessárias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensão do docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para criar um módulo, terá de node. js, que inclui o npm para criar a pasta de projeto, o Docker para criar a imagem do módulo e um registo de contentores para armazenar a imagem de módulo:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ou [Hub do Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

   >[!TIP]
   >Pode utilizar um registo do Docker local para o protótipo e fins de testes, em vez de um registo de cloud. 

Para testar seu módulo num dispositivo, precisa de um hub IoT Active Directory com, pelo menos, um dispositivo do IoT Edge. Se pretender utilizar o seu computador como um dispositivo IoT Edge, pode fazê-ao seguir os passos nos tutoriais para [Windows](quickstart.md) ou [Linux](quickstart-linux.md). 

## <a name="create-a-new-solution-template"></a>Criar um novo modelo de solução

Os seguintes passos mostram como criar um módulo do IoT Edge com base em node. js usando o Visual Studio Code e a extensão Azure IoT Edge. Comece por criar uma solução e, em seguida, gerar primeiro módulo nessa solução. Cada solução pode conter vários módulos. 

1. No Visual Studio Code, selecione **View** > **Terminal integrado**.
2. No terminal integrado, introduza o seguinte comando para instalar (ou atualizar) a versão mais recente do modelo de módulo do Azure IoT Edge para node. js:

   ```cmd/sh
   npm install -g yo generator-azure-iot-edge-module
   ```
3. No Visual Studio Code, selecione **Vista** > **Paleta de Comando**. 
4. Na paleta de comandos, escreva e execute o comando **do Azure IoT Edge: nova solução do IoT Edge**.

   ![Executar a nova solução de IoT Edge](./media/how-to-develop-csharp-module/new-solution.png)

5. Navegue até à pasta onde pretende criar a nova solução e clique em **selecionar pasta**. 
6. Forneça um nome para a sua solução. 
7. Escolher **módulo de node. js** como o modelo para o primeiro módulo na solução.
8. Forneça um nome para seu módulo. Escolha um nome que é exclusivo no seu registo de contentor. 
9. Indique o repositório de imagens para o módulo. O nome de código de VS autopopulates o módulo, portanto, precisa apenas substituir **localhost:5000** pelas suas informações de registo. Se utilizar um registo do Docker local para fins de teste, é bem localhost. Se utilizar o Azure Container Registry, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão é semelhante  **\<nome do registo\>. azurecr.io**.

Código VS usa as informações fornecidas, cria uma solução de IoT Edge, e carrega-o numa nova janela.

Dentro da solução tem três itens: 
* R **.vscode** pasta contém as configurações de depuração.
* R **módulos** pasta contém subpastas para cada módulo. Neste momento tiver apenas uma, mas pode adicionar mais na paleta de comandos com o comando **do Azure IoT Edge: Adicionar módulo do IoT Edge**. 
* R **. env** ficheiro apresenta uma lista de variáveis do seu ambiente. Se o Azure Container Registry é o seu registo, terá um nome de utilizador do Azure Container Registry e a palavra-passe no mesmo.

   >[!NOTE]
   >Só é criado o ficheiro de ambiente se fornecer um repositório de imagens para o módulo. Se aceitou os padrões de localhost para testar e depurar localmente, não tem de declarar as variáveis de ambiente. 

* R **deployment.template.json** seu novo módulo juntamente com um exemplo apresenta uma lista de ficheiros **tempSensor** módulo que simula dados que pode utilizar para fins de teste. Para obter mais informações sobre como o trabalho de manifestos de implantação, consulte [compreender como os módulos do IoT Edge podem ser utilizados, configurados e reutilizados](module-composition.md).

## <a name="develop-your-module"></a>Desenvolver o seu módulo

O código de node. js predefinida que é fornecido com a solução está localizado em **módulos** > **\<seu nome de módulo\>** > **App. js** . O módulo e o ficheiro de deployment.template.json são configuradas para que possa criar a solução, enviá-la para o seu registo de contentor e implementá-la para um dispositivo para começar a testar sem tocar em nenhum código. O módulo baseia-se simplesmente utilizam os dados de uma origem (no caso, o módulo de tempSensor que simula dados) e encaminhá-la para o IoT Hub. 

Quando estiver pronto para personalizar o modelo de node. js com o seu próprio código, utilize o [SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para criar módulos esse endereço tem da chave para soluções de IoT, como segurança, gestão de dispositivos e confiabilidade. 

## <a name="build-and-deploy-your-module-for-debugging"></a>Crie e implemente o seu módulo para depuração

Em cada pasta de módulo, existem vários ficheiros de Docker para tipos de contentor diferente. Pode usar qualquer um destes ficheiros que terminam com a extensão **.debug** para criar o seu módulo para fins de teste. Atualmente, o módulos node. js só suportam a depuração em contentores de linux-amd64, windows-amd64 e arm32v7 do linux.

1. No VS Code, navegue para o `deployment.template.json` ficheiro. Atualizar o seu URL de imagem do módulo adicionando **.debug** ao fim.
2. Substitua o createOptions do módulo de node. js em **deployment.template.json** com abaixo conteúdo e guarde este ficheiro: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

2. Na paleta de comandos VS Code, escreva e execute o comando **do Azure IoT Edge: solução de IoT Edge criar**.
3. Selecione o `deployment.template.json` ficheiro para a sua solução da paleta de comandos. 
4. No Explorador de dispositivos do Azure IoT Hub, clique com botão direito um ID de dispositivo do IoT Edge, em seguida, selecione **criar a implementação de único dispositivo**. 
5. Abra o **config** pasta da sua solução, em seguida, selecione o `deployment.json` ficheiro. Clique em **Selecionar Manifesto de Implementação do Edge**. 

Em seguida, pode ver que a implementação é criada com êxito com uma implantação integrado de ID no VS Code terminal.

Pode verificar o estado do contentor no Explorador do Docker de código do VS ou ao executar o `docker ps` comando no terminal.

## <a name="start-debugging-nodejs-module-in-vs-code"></a>Iniciar a depuração de módulo de node. js no VS Code

Mantém depuração de código de VS informações de configuração num `launch.json` ficheiros localizados num `.vscode` pasta na sua área de trabalho. Isso `launch.json` arquivo foi gerado quando criou uma nova solução de IoT Edge. Ele atualiza cada vez que adiciona um novo módulo que oferece suporte à depuração. 

1. Navegue para a vista de depuração do VS Code e selecionar o ficheiro de configuração de depuração para seu módulo.

2. Navegue para `app.js`. Adicione um ponto de interrupção nesse arquivo.

3. Clique nas **iniciar depuração** botão ou prima **F5**e selecione o processo para anexar a.

4. Na vista de depuração de código do VS, pode ver as variáveis no painel esquerdo. 

O exemplo anterior mostra como depurar os módulos do IoT Edge do node. js em contentores. Ele adicionou portas expostas no seu createOptions de contentor do módulo. Depois de concluir a depuração de seus módulos do node. js, recomendamos que remova estas portas expostas de módulos do IoT Edge prontos para produção.

## <a name="next-steps"></a>Passos Seguintes

Assim que tiver o seu módulo criado, saiba como [módulos de implementar o Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md)

Para desenvolver módulos para os seus dispositivos de IoT Edge [compreender e utilizar os SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
