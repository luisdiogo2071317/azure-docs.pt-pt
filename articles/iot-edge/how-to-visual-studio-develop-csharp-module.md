---
title: Desenvolver e depurar C# módulos no Visual Studio – Azure IoT Edge | Documentos da Microsoft
description: Utilizar o Visual Studio 2017 para desenvolver e depurar o módulo de c# para o Azure IoT Edge
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/24/2018
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 547989152320678ec195c4e8a93965cfbbd0f341
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/08/2018
ms.locfileid: "53097850"
---
# <a name="use-visual-studio-2017-to-develop-and-debug-c-modules-for-azure-iot-edge-preview"></a>Utilizar o Visual Studio 2017 para desenvolver e depurar c# módulos do Azure IoT Edge (pré-visualização)

Pode transformar sua lógica de negócio em módulos do Azure IoT Edge. Este artigo mostra-lhe como utilizar o Visual Studio 2017 como a principal ferramenta para desenvolver e depurar os módulos do c#.

As ferramentas de Edge IoT do Azure para Visual Studio fornece as seguintes vantagens:

- Criar, editar, criar, executar e depurar as soluções do Azure IoT Edge e os módulos no seu computador de desenvolvimento local.
- Implemente a sua solução do Azure IoT Edge no dispositivo Azure IoT Edge através do IoT Hub do Azure.
- Código de seus módulos do IoT do Azure em c# enquanto tem todos os benefícios de desenvolvimento do Visual Studio.
- Gerir dispositivos Azure IoT Edge e os módulos com interface do Usuário. 

Este artigo mostra-lhe como utilizar as ferramentas de Edge IoT do Azure para Visual Studio 2017 para desenvolver seus módulos do IoT Edge em c#. Também irá aprender a implementar o seu projeto para o seu dispositivo Azure IoT Edge.

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que utilize um computador ou máquina virtual a executar o Windows como computador de desenvolvimento. Dispositivo IoT Edge pode ser outro dispositivo físico.

Uma vez que este artigo utiliza o Visual Studio 2017 como a ferramenta de desenvolvimento principal, instale o Visual Studio. E certifique-se de incluir **carga de trabalho de desenvolvimento do Azure** na instalação do Visual Studio 2017. Pode [modificar o Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) e adicione a carga de trabalho de desenvolvimento do Azure.

Depois do Visual Studio 2017 está pronta, também terá de:

- Transfira e instale [extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) no mercado do Visual Studio para criar o IoT Edge project no Visual Studio 2017.
- [Docker Community Edition](https://docs.docker.com/install/) no computador de desenvolvimento para compilar e executar as imagens de módulo. Tem de definir corretamente o Docker CE em execução no modo de contentor do Linux ou Windows contentor.
- Para configurar o ambiente de desenvolvimento local para depuração, executar e testar a sua solução de IoT Edge, precisa [ferramenta de desenvolvimento do Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Instale [Python (2.7/3.6) e o Pip](https://www.python.org/). Em seguida, instale **iotedgehubdev** executando o comando no seu terminal abaixo. Certifique-se de que a sua versão da ferramenta de desenvolvimento do Azure IoT EdgeHub é maior que 0.3.0.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) ou [Hub do Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   - Pode utilizar um registo do Docker local para o protótipo e fins de testes em vez de um registo de cloud. 

- Para testar seu módulo, precisa de um hub IoT Active Directory com pelo menos um ID de dispositivo IoT Edge criou. Se estiver a executar o daemon de segurança de IoT Edge no computador de desenvolvimento, precisará parar EdgeHub e EdgeAgent antes de começar o desenvolvimento no Visual Studio. 

### <a name="check-your-tools-version"></a>Verificar a sua versão de ferramentas

1. Partir do **ferramentas** menu, escolha **extensões e atualizações**. Expanda **instalada > ferramentas** e pode encontrar **Azure IoT Edge** e **Cloud Explorer**.

2. Tenha em atenção a versão instalada. Pode comparar essa versão com a versão mais recente no Visual Studio Marketplace ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge))

3. Se tiver uma versão mais antiga, Atualize as suas ferramentas no Visual Studio, conforme mostrado na seção a seguir.

### <a name="update-your-tools"></a>Atualizar as suas ferramentas

1. Na **extensões e atualizações** caixa de diálogo, expanda **atualizações > Visual Studio Marketplace**, escolha **Azure IoT Edge** ou **Cloud Explorer**e selecione **atualização**.

2. Após a atualização das ferramentas é transferida, feche o Visual Studio ao acionar que as ferramentas de atualização através do instalador do VSIX.

3. No instalador, escolha **OK** para iniciar e, em seguida, modificar para atualizar as ferramentas.

4. Depois de concluída a atualização, escolha o feche e reinicie o Visual Studio.

### <a name="create-an-azure-iot-edge-project"></a>Criar um projeto do Azure IoT Edge

O modelo de projeto do Azure IoT Edge no Visual Studio cria um projeto que pode ser implementado em dispositivos do Azure IoT Edge no IoT Hub do Azure. Em primeiro lugar cria uma solução de Azure IoT Edge e, em seguida, gerar o primeiro módulo de c# dessa forma. Cada solução de IoT Edge pode conter mais de um módulo. 

1. No Visual Studio, selecione **Novo** > **Projeto**, no menu **Ficheiro**.

2. Na **novo projeto** caixa de diálogo, selecione **instalado**, expanda **Visual c# > Cloud**, selecione **Azure IoT Edge**, escreva um  **Nome** para o seu projeto e especifique a localização e clique em **OK**. O nome de projeto predefinido é **AzureIoTEdgeApp1**. 

   ![Novo Projeto](./media/how-to-visual-studio-develop-csharp-module/create-new.jpg)

3. Na **configuração do módulo do IoT Edge** janela, selecione **módulo de c#** escreva e especifique o nome do módulo e o repositório de imagens do módulo.  O módulo do VS autopopulates nome com **localhost:5000**. Substitua-o com as suas próprias informações de registo. Se utilizar um registo do Docker local para fins de teste, é bem localhost. Se utilizar o Azure Container Registry, em seguida, utilize o servidor de início de sessão a partir das definições do seu registo. O servidor de início de sessão é semelhante  **<registry name>. azurecr.io**. Substitua apenas a parte localhost da cadeia de carateres, não elimine o nome do módulo. O nome de módulo predefinido é **IoTEdgeModule1**

4. Clique em **OK** para criar o projeto do Azure IoT Edge com um módulo de c#.

Agora tem um **AzureIoTEdgeApp1** projeto e um **IoTEdgeModule1** projeto em nossa solução. O **AzureIoTEdgeApp1** projeto tem um **deployment.template.json** ficheiro. Esse arquivo define os módulos que pretende criar e implementar para a sua solução de IoT Edge e define as rotas entre módulos. A solução padrão tem um **tempSensor** módulo e uma **IoTEdgeModule1** módulo. O **tempSensor** módulo gera dados simulados para **IoTEdgeModule1** módulo, enquanto o código de predefinição na **IoTEdgeModule1** módulo é um módulo de mensagem de pipe, que diretamente pipe recebida mensagens ao IoT Hub.

O **IoTEdgeModule1** projeto é um .net Core 2,1 aplicação de consola. Ele contém necessários **Dockerfiles** precisa para o seu dispositivo IoT Edge em execução com o contentor do Windows ou o contentor do Linux. O **Module** ficheiro descreve os metadados de um módulo. O **program.cs** é o código de módulo real, que usa o SDK de dispositivo do IoT do Azure como uma dependência.

## <a name="develop-your-module"></a>Desenvolver o seu módulo

O padrão módulo código c# que vem com a solução está localizado em **IoTEdgeModule1** > **Program.cs**. O módulo e o ficheiro de deployment.template.json são configuradas para que possa criar a solução, enviá-la para o seu registo de contentor e implementá-la para um dispositivo para começar a testar sem tocar em nenhum código. O módulo baseia-se simplesmente utilizam os dados de uma origem (no caso, o módulo de tempSensor que simula dados) e encaminhá-la para o IoT Hub. 

Quando estiver pronto para personalizar o modelo do c# com o seu próprio código, utilize o [SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para criar módulos esse endereço tem da chave para soluções de IoT, como segurança, gestão de dispositivos e confiabilidade. 

## <a name="initialize-iotegehubdev-with-iot-edge-device-connection-string"></a>Inicializar **iotegehubdev** pela cadeia de ligação do dispositivo IoT Edge

1. Tem de obter a cadeia de ligação de qualquer dispositivo IoT Edge, pode copiar o valor de "Cadeia de ligação primária" do Cloud Explorer no Visual Studio 2017, como a seguir. Não copie a cadeia de ligação do dispositivo não Edge, o ícone do dispositivo IoT Edge é diferente do dispositivo não Edge.

   ![Copie a cadeia de ligação de dispositivo do Edge](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

2. Terá de clique com o botão direito do rato em **AzureIoTEdgeApp1** projeto para abrir o menu de contexto, clique em **definir cadeia de ligação de dispositivo do Edge**, será apresentada a janela de configuração do Azure IoT Edge.

   ![Abra a janela de cadeia de ligação do conjunto de borda](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

3. Na janela de configuração a cadeia de ligação que obteve no primeiro passo de entrada e clique em **OK** botão.

>[!NOTE]
>Este é o trabalho de uma única vez, apenas tem de executar este passo uma vez num computador, todos os subsequentes do Azure IoT Edge soluções irão obter se gratuitamente. É claro que pode voltar a executar este passo se a cadeia de ligação é inválida ou tem de alterar para outra cadeia de ligação.

## <a name="build-and-debug-single-c-module"></a>Criar e depurar o módulo de c# único

Normalmente, queremos teste/depuração cada módulo antes de fazermos-lo em execução dentro de uma solução inteira com vários módulos.

1. Selecione **IoTEdgeModule1** como o projeto de arranque no menu de contexto.

   ![Definir o projeto de arranque](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

2. Prima **F5** ou clique no botão abaixo para executar o módulo, poderá demorar 10 ~ 20 segundos pela primeira vez.

   ![Executar o módulo](./media/how-to-visual-studio-develop-csharp-module/run-module.png)


3. Deverá ver um .net Core aplicação de consola a utilizar se o módulo foi inicializado com êxito.

   ![Módulo em execução](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

4. Agora pode definir um ponto de interrupção no **PipeMessage** na **Program.cs**, em seguida, enviar mensagem ao executar o seguinte comando na sua **Git Bash** ou **WSL Bash**  (não o execute em CMD ou Powershell) (também pode encontrar este comando na janela de saída do VS):

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Depurar o módulo único](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    O ponto de interrupção deve ser acionado. Pode ver variáveis na janela locais do Visual Studio.

   > [!TIP]
   > Também pode utilizar [PostMan](https://www.getpostman.com/) ou outras ferramentas de API para enviar mensagens através de, em vez de `curl`.

5. Prima **Ctrl + F5** ou clique no botão Parar para interromper a depuração.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Criar e depurar a solução de IoT Edge com vários módulos

Depois de concluir a desenvolver o módulo único, em seguida, queremos executar e depurar a solução completa com vários módulos.

1. Adicione o módulo de c# segundo na solução. Com o botão direito **AzureIoTEdgeApp1** e selecione **Add** -> **novo módulo do IoT Edge**. É o nome predefinido do módulo do segundo **IoTEdgeModule2** e ainda é um módulo de pipe.

2. Navegue para **deployment.template.json**. Verá **IoTEdgeModule2** foi adicionada no **módulos** secção. Substitua a **rotas** secção com o seguinte. Se tiver personalizado seus nomes de módulos, certifique-se de que atualiza os nomes na seguinte depois de substituição.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

3. Definir **AzureIoTEdgeApp1** projeto como projeto de arranque no menu de contexto.

4. Defina pontos de interrupção e prima F5, em seguida, poderia executar e depurar vários módulos simultaneamente. Deverá ver .net várias janelas de aplicação da consola central, cada janela indica o módulo de c#. 

   ![Depurar vários módulos](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

5. Prima **Ctrl + F5** ou clique no botão Parar para interromper a depuração.

## <a name="build-and-push-images"></a>Criar e enviar imagens

1. Certifique-se **AzureIoTEdgeApp1** é o seu projeto de arranque. Selecione **depurar** ou **versão** configuração para as imagens de módulo criar.

    > [!NOTE]
    > Ao escolher **depurar**, irá utilizar o VS `Dockerfile.debug` para criar imagens do Docker. Isto inclui o depurador de linha de comandos do .NET Core VSDBG em sua imagem de contentor ao criá-lo. Recomendamos que utilize **lançamento** configuração de que utiliza `Dockerfile` sem VSDBG de módulos do IoT Edge prontos para produção.

2. Se estiver a utilizar o registo privado, como o Azure Container Registry, execute o registo de Docker com o seguinte comando no seu terminal. Se estiver a utilizar o registo local, pode [executar um registro local](https://docs.docker.com/registry/deploying/#run-a-local-registry).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server> 
    ```

3. Se estiver a utilizar o registo privado, como o Azure Container Registry, precisa colocar o nome de utilizador de registo e a palavra-passe no `deployment.template.json` em definições de tempo de execução com o seguinte conteúdo. Não se esqueça de substituir o marcador de posição pelo seu nome de utilizador administrador real e a palavra-passe.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

4. Clique com botão direito **AzureIoTEdgeApp1** e escolha o item de menu de contexto **compilação e solução de ponta de Push**, irá criar e enviar imagem do docker para cada módulo.

   ![Criar e enviar imagens](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)


## <a name="deploy-the-solution"></a>Implementar a solução

No artigo de início rápido que utilizou para configurar o seu dispositivo IoT Edge, implementou um módulo com o portal do Azure. Também pode implementar módulos com o Cloud Explorer para Visual Studio. Já tem um manifesto de implantação preparado para o seu cenário, o `deployment.json` ficheiro. Agora tudo o que precisa de fazer é selecionar um dispositivo para receber a implementação.

1. Open **Cloud Explorer** ao clicar em **vista** > **Cloud Explorer**. Certifique-se de que iniciou no Visual Studio 2017 com a sua conta.

2. Na **Cloud Explorer**, expanda a sua subscrição, encontrar o seu IoT Hub do Azure e o dispositivo Azure IoT Edge que pretende implementar.

3. Com o botão direito no dispositivo IoT Edge para criar a implementação para o mesmo, tem de escolher o ficheiro de manifesto de implantação sob o `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`.

>>[!NOTE]
>>Não tem de selecionar `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json`

4. Clique no botão Atualizar. Deverá ver os novos módulos em execução juntamente com o **TempSensor** módulo e o **$edgeAgent** e **$edgeHub**.

## <a name="view-generated-data"></a>Ver os dados gerados

1. Para monitorizar a mensagem D2C, de um dispositivo específico, clique no dispositivo na lista e clique em **iniciar Monitoring D2C Messages** na **ação** janela.

2. Para parar a monitorização dos dados, clique com o dispositivo na lista e selecione **parar Monitoring D2C Messages** na **ação** janela.

## <a name="next-steps"></a>Passos Seguintes

Para desenvolver módulos para os seus dispositivos de IoT Edge [compreender e utilizar os SDKs do Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
