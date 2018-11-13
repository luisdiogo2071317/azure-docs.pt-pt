---
title: Depurar módulos node. js para o Azure IoT Edge | Documentos da Microsoft
description: Utilizar o Visual Studio Code para desenvolver e depurar módulos node. js para o Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/21/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: b05492941defc6ac3aa252d6bb29043d55e6b66c
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/07/2018
ms.locfileid: "51261779"
---
# <a name="use-visual-studio-code-to-develop-and-debug-nodejs-modules-for-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver e depurar módulos node. js para o Azure IoT Edge

Pode enviar a lógica de negócio a funcionar no limite, ativá-la em módulos do Azure IoT Edge. Este artigo fornece instruções detalhadas para utilizar o Visual Studio Code (código de VS) como a ferramenta de desenvolvimento principal para desenvolver módulos do node. js.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que utilize um computador ou máquina virtual com o Windows, macOS ou Linux como o computador de desenvolvimento. Dispositivo IoT Edge pode ser outro dispositivo físico.

> [!NOTE]
> Este artigo depuração demonstra duas maneiras típicas para depurar seu módulo de node. js no VS Code. Uma forma é anexar um processo num contêiner de módulo, enquanto a outra é lanuch o código de módulo no modo de depuração. Se não estiver familiarizado com as capacidades de depuração do Visual Studio Code, ler sobre [Debugging](https://code.visualstudio.com/Docs/editor/debugging).

Uma vez que este artigo usa código do Visual Studio como a ferramenta de desenvolvimento principal, instalar o VS Code e, em seguida, adicione as extensões necessárias:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Extensão do docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Para criar um módulo, terá de node. js, que inclui o npm para criar a pasta de projeto, o Docker para criar a imagem do módulo e um registo de contentores para armazenar a imagem de módulo:
* [Node.js](https://nodejs.org)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ou [Hub do Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * Pode utilizar um registo do Docker local para o protótipo e fins de testes, em vez de um registo de cloud. 

Para configurar o local ambiente de desenvolvimento para depurar, executar e testar a sua solução de IoT Edge, precisa [ferramenta de desenvolvimento do Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Instale [Python (2.7/3.6) e o Pip](https://www.python.org/). PIP está incluído com o instalador do Python. Em seguida, instale **iotedgehubdev** executando o comando no seu terminal abaixo.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Para testar seu módulo num dispositivo, precisa de um hub IoT Active Directory com pelo menos um ID de dispositivo IoT Edge criou. Se estiver a executar o daemon de IoT Edge no computador de desenvolvimento, poderá ter de parar EdgeHub e EdgeAgent antes de prosseguir para o passo seguinte. 

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
9. Indique o repositório de imagens para o módulo. O nome de código de VS autopopulates o módulo, portanto, precisa apenas substituir **localhost:5000** pelas suas informações de registo. Se utilizar um registo do Docker local para fins de teste, é bem localhost. Se utilizar o Azure Container Registry, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão é semelhante  **\<nome do registo\>. azurecr.io**. Substitua apenas a parte localhost da cadeia de carateres, não elimine o nome do módulo.

   ![Fornecer repositório de imagens do Docker](./media/how-to-develop-node-module/repository.png)

Código VS usa as informações fornecidas, cria uma solução de IoT Edge, e carrega-o numa nova janela.

Dentro da solução tem três itens: 
* R **.vscode** pasta contém as configurações de depuração.
* R **módulos** pasta contém subpastas para cada módulo. Neste momento tiver apenas uma, mas pode adicionar mais na paleta de comandos com o comando **do Azure IoT Edge: Adicionar módulo do IoT Edge**. 
* R **. env** ficheiro apresenta uma lista de variáveis do seu ambiente. Se o Azure Container Registry é o seu registo, terá um nome de utilizador do Azure Container Registry e a palavra-passe no mesmo.

   >[!NOTE]
   >Só é criado o ficheiro de ambiente se fornecer um repositório de imagens para o módulo. Se aceitou os padrões de localhost para testar e depurar localmente, não tem de declarar as variáveis de ambiente. 

* R **deployment.template.json** seu novo módulo juntamente com um exemplo apresenta uma lista de ficheiros **tempSensor** módulo que simula dados que pode utilizar para fins de teste. Para obter mais informações sobre como o trabalho de manifestos de implantação, consulte [compreender como os módulos do IoT Edge podem ser utilizados, configurados e reutilizados](module-composition.md).

## <a name="develop-your-module"></a>Desenvolver o seu módulo

O código de node. js predefinida que é fornecido com a solução está localizado em **módulos** > [nome da sua módulo] > **js**. O módulo e o ficheiro de deployment.template.json são configuradas para que possa criar a solução, enviá-la para o seu registo de contentor e implementá-la para um dispositivo para começar a testar sem tocar em nenhum código. O módulo baseia-se simplesmente utilizam os dados de uma origem (no caso, o módulo de tempSensor que simula dados) e encaminhá-la para o IoT Hub. 

Quando estiver pronto para personalizar o modelo de node. js com o seu próprio código, utilize o [SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para criar módulos esse endereço tem da chave para soluções de IoT, como segurança, gestão de dispositivos e confiabilidade. 

Visual Studio Code tem suporte para node. js. Saiba mais sobre [como trabalhar com node. js no VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial).

## <a name="launch-and-debug-module-code-without-container"></a>Inicie e depurar o código de módulo sem contentor

O módulo de node. js do IoT Edge depende do Azure IoT SDK de dispositivo node. js. No código do módulo padrão, inicializar uma **ModuleClient** com definições de ambiente e o nome de entrada, que significa que o módulo de node. js do IoT Edge requer as definições de ambiente iniciar e executar e, também terá de enviar ou encaminhar mensagens para os canais de entrada. O módulo de node. js predefinida contém apenas um canal de entrada e o nome é **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>Simulador de IoT Edge do programa de configuração para a aplicação de módulo único

1. Para configurar e iniciar o simulador, na paleta de comandos do VS Code, escreva e selecione **do Azure IoT Edge: iniciar o IoT Edge Hub simulador de módulo único**. Também tem de especificar os nomes de entrada para a sua aplicação de módulo único, tipo **input1** e prima Enter. O comando irá disparar **iotedgehubdev** CLI e iniciar o simulador do IoT Edge e um contentor de módulo de utilitário teste. Pode ver os resultados abaixo no terminal integrado se o simulator foi iniciado no modo de módulo único com êxito. Também pode ver um `curl` comando para o ajudar a enviar mensagem por meio de. Irá utilizá-lo mais tarde.

   ![Simulador de IoT Edge do programa de configuração para a aplicação de módulo único](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   Pode mover para o Docker Explorer e ver o módulo de estado de execução.

   ![Estado do módulo de simulador](media/how-to-develop-csharp-module/simulator-status.png)

   O **edgeHubDev** contentor é o núcleo do simulador do IoT Edge local. Ele pode ser executado no computador de desenvolvimento sem o daemon de segurança de IoT Edge e fornecer definições de ambiente para a sua aplicação de módulo nativo ou contentores de módulo. O **entrada** contentor exposta restAPIs para ajudar a mensagens de bridge ao canal de entrada em seu módulo de destino.

2. Na paleta de comandos do VS Code, escreva e selecione **do Azure IoT Edge: Definir credenciais de módulo em configurações do usuário** para configurar o módulo de definições de ambiente em `azure-iot-edge.EdgeHubConnectionString` e `azure-iot-edge.EdgeModuleCACertificateFile` nas definições de utilizador. Pode encontrar estas definições de ambiente são referenciadas numa **.vscode** > **Launch** e [definições de utilizador do VS Code](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="debug-nodejs-module-in-launch-mode"></a>Depurar o módulo de node. js no modo de inicialização

1. No terminal integrado, altere o diretório para **NodeModule** pasta, execute o seguinte comando para instalar pacotes de nó

   ```cmd
   npm install
   ```

2. Navegue para `app.js`. Adicione um ponto de interrupção nesse arquivo.

3. Navegue para a vista de depuração do VS Code. Selecione a configuração de depuração **ModuleName depurar Local (node. js)**. 

4. Clique em **iniciar depuração** ou prima **F5**. Começa a sessão de depuração.

5. No terminal integrado do VS Code, execute o seguinte comando para enviar um **Olá, mundo** mensagem ao módulo. Este é o comando mostrado nos passos anteriores quando configurado com êxito o simulador do IoT Edge. Poderá ter de criar ou mudar para outro terminal integrado se atual está bloqueado.

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Se estiver a utilizar o Windows, certificar-se de que o shell do seu terminal integrado do VS Code está **Git Bash** ou **WSL Bash**. Não é possível executar `curl` comando no PowerShell ou a linha de comandos. 
   
   > [!TIP]
   > Também pode utilizar [PostMan](https://www.getpostman.com/) ou outras ferramentas de API para enviar mensagens através de, em vez de `curl`.

6. Na vista de depuração de código do VS, verá as variáveis no painel esquerdo. 

7. Para parar a sessão de depuração, clique no botão Parar ou o press **Shift + F5**. E na paleta de comandos do VS Code, escreva e selecione **do Azure IoT Edge: parar simulador do IoT Edge** para parar e limpar o simulador.


## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Criar contentor de módulo para depuração e de depuração no modo de anexar

A solução predefinida contém dois módulos, um é um módulo de sensor de temperatura simulada e o outro é o módulo de pipe de node. js. O sensor de temperatura simulada envie mensagens ao módulo de pipe de node. js e, em seguida, as mensagens são direcionadas para o IoT Hub. Na pasta de módulo que criou, existem vários arquivos de Docker para tipos de contentor diferente. Utilizar qualquer um destes ficheiros que terminam com a extensão **.debug** para criar o seu módulo para fins de teste. Atualmente, o módulos node. js só suportam a depuração em contentores de linux-amd64, windows-amd64 e arm32v7 do linux.

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>Simulador de IoT Edge do programa de configuração para a solução de IoT Edge

No computador de desenvolvimento, pode iniciar o simulador do IoT Edge em vez de instalar o daemon de segurança de IoT Edge para executar a sua solução de IoT Edge. 

1. No Explorador de dispositivos no lado esquerdo, faça duplo clique no seu ID de dispositivo do IoT Edge, selecione **configuração do IoT Edge simulador** para iniciar o simulador com a cadeia de ligação do dispositivo.

2. Pode ver que o simulador do IoT Edge tem sido com êxito a configuração no terminal integrado.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Criar e executar o contentor para depuração e de depuração no modo de anexar

1. No VS Code, navegue para o `deployment.template.json` ficheiro. Atualizar o seu URL de imagem do módulo adicionando **.debug** ao fim.

2. Substitua o createOptions do módulo de node. js em **deployment.template.json** com abaixo conteúdo e guarde este ficheiro: 
    ```json
    "createOptions": "{\"ExposedPorts\":{\"9229/tcp\":{}},\"HostConfig\":{\"PortBindings\":{\"9229/tcp\":[{\"HostPort\":\"9229\"}]}}}"
    ```

3. Navegue para a vista de depuração do VS Code. Selecione o ficheiro de configuração de depuração para seu módulo. O nome de opção de depuração deve ser semelhante à **ModuleName depuração remota (node. js)** ou **ModuleName depuração remota (node. js num contentor do Windows)**, que depende de seu tipo de contentor no computador de desenvolvimento.

4. Selecione **iniciar depuração** ou selecione **F5**. Selecione o processo para anexar a.

5. Na vista de depuração de código do VS, verá as variáveis no painel esquerdo.

6. Para parar a sessão de depuração, clique no botão Parar ou o press **Shift + F5**. E na paleta de comandos do VS Code, escreva e selecione **do Azure IoT Edge: parar simulador do IoT Edge**.

> [!NOTE]
> O exemplo anterior mostra como depurar os módulos do IoT Edge do node. js em contentores. Ele adicionou portas expostas no seu createOptions de contentor do módulo. Depois de concluir a depuração de seus módulos do node. js, recomendamos que remova estas portas expostas de módulos do IoT Edge prontos para produção.

## <a name="next-steps"></a>Passos Seguintes

Assim que tiver o seu módulo criado, saiba como [módulos de implementar o Azure IoT Edge do Visual Studio Code](how-to-deploy-modules-vscode.md)

Para desenvolver módulos para os seus dispositivos de IoT Edge [compreender e utilizar os SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
