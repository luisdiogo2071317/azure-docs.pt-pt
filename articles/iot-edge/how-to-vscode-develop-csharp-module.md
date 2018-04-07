---
title: Utilizar o Visual Studio Code para desenvolver um módulo de c# com o Azure IoT Edge | Microsoft Docs
description: Desenvolver e implementar um módulo de c# com o limite de IoT do Azure no Visual Studio Code, sem mudar de contexto.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 48c6cacebdeb7505c8dc2bcaed099c33862589ac
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="use-visual-studio-code-to-develop-a-c-module-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver um módulo de c# com o Azure IoT Edge
Este artigo fornece instruções detalhadas para utilizar [Visual Studio Code](https://code.visualstudio.com/) como a ferramenta de desenvolvimento principal para desenvolver e implementar os módulos do Azure IoT Edge. 

## <a name="prerequisites"></a>Pré-requisitos
Este artigo pressupõe que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT pode ser outro dispositivo físico, ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

Antes de começar esta orientação, conclua os seguintes tutoriais:
- Implementar o Azure IoT Edge num dispositivo simulado [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) ou [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Desenvolver e implementar um módulo de limite de IoT c# para o seu dispositivo simulado](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Eis uma lista de verificação que mostra os itens que deve ter depois de concluir os tutoriais anteriores:

- [Visual Studio Code](https://code.visualstudio.com/) 
- [Extensão de limite de IoT do Azure para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [C# para a extensão do Visual Studio Code (com tecnologia OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd) 
- [Python 2.7](https://www.python.org/downloads/)
- [Script de controlo contorno de IoT](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- AzureIoTEdgeModule template (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Um IoT hub Active Directory com, pelo menos, um dispositivo de limite de IoT

Também é útil instalar [suporte de Docker para o VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para melhor gerir imagens do módulo e contentores.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>Implementar um módulo do Azure IoT Edge no VS Code

### <a name="list-your-iot-hub-devices"></a>Lista os dispositivos do hub IoT
Existem duas formas para listar os dispositivos do hub IoT no VS Code. Pode escolher qualquer forma para continuar.

#### <a name="sign-in-to-your-azure-account-in-vs-code-and-choose-your-iot-hub"></a>Inicie sessão na sua conta do Azure no VS Code e escolha o seu IoT hub
1. Na paleta do comando (F1 ou Ctrl + Shift + P), escreva e selecione **Azure: a iniciar sessão**. Em seguida, selecione **Copiar & abrir**. Cole (Ctrl + V) o código no seu browser e selecione **continuar**. Em seguida, inicie sessão com a sua conta do Azure. Pode ver as suas informações de conta na barra de estado de VS Code.
2. Na paleta do comando, escreva e selecione **IoT: selecione o IoT Hub**. Em primeiro lugar, selecione a subscrição onde criou o seu IoT hub no tutorial anterior. Em seguida, escolha o IoT hub que contém o dispositivo de limite de IoT.

    ![Captura de ecrã da lista de dispositivos](./media/how-to-vscode-develop-csharp-module/device-list.png)

#### <a name="set-the-iot-hub-connection-string"></a>Definir a cadeia de ligação do hub IoT
Na paleta do comando, escreva e selecione **IoT: definir a cadeia de ligação do IoT Hub**. Certifique-se de que cole a cadeia de ligação pela política **iothubowner**. (Pode encontrar este nas políticas de acesso partilhado do seu IoT hub no portal do Azure.)
 
Pode ver a lista de dispositivos no Explorador de dispositivos do IoT Hub, na barra lateral no lado esquerdo.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>Iniciar o tempo de execução do limite de IoT e implementar um módulo
Instalar e iniciar o tempo de execução do Azure IoT Edge no seu dispositivo. Implemente um módulo de sensor simulado que envia dados de telemetria ao IoT Hub do Azure.
1. Na paleta do comando, selecione **Edge: limite de configuração** e escolha o limite de IoT ID do dispositivo. Em alternativa, clique com botão direito o ID de dispositivo de limite de IoT no **lista de dispositivos**e selecione **configuração Edge**.

    ![Tempo de execução do limite de captura de ecrã de configuração](./media/how-to-vscode-develop-csharp-module/setup-edge.png)

2. Na paleta do comando, selecione **Edge: Iniciar Edge** para iniciar o tempo de execução do limite de IoT. Pode ver as saídas correspondentes no terminal integrada.

    ![Tempo de execução de captura de ecrã de início Edge](./media/how-to-vscode-develop-csharp-module/start-edge.png)

3. Verifique o estado de runtime de limite de IoT no Explorador do Docker. Verde significa que está em execução e o tempo de execução do limite de IoT foi iniciada com êxito. O computador agora simula um dispositivo de limite de IoT.

    ![Estado de runtime de captura de ecrã do contorno](./media/how-to-vscode-develop-csharp-module/edge-runtime.png)

4. Simule um sensor que mantém a enviar mensagens para o seu dispositivo de limite de IoT. Na paleta do comando, escreva e selecione **contorno: o ficheiro de configuração de gerar Edge**. Selecione uma pasta para criar este ficheiro. No ficheiro deployment.json que é gerado, substitua o conteúdo `<registry>/<image>:<tag>` com `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`e guarde o ficheiro.

    ![Captura de ecrã do módulo do sensor](./media/how-to-vscode-develop-csharp-module/sensor-module.png)

5. Selecione **Edge: criar a implementação para o dispositivo de limite**e escolha o limite de IoT ID de dispositivo para criar uma nova implementação. Em alternativa, pode o ID de dispositivo de limite de IoT na lista de dispositivos com o botão direito e selecione **criar a implementação para o dispositivo de limite**. 

6. Deverá ver o seu limite de IoT iniciar a execução no Explorador do Docker com o sensor simulado. Clique com botão direito do contentor no Explorador de Docker. Pode ver os registos de Docker para cada módulo. Além disso, pode ver a lista de módulos na lista de dispositivos.

    ![Captura de ecrã da lista de módulo](./media/how-to-vscode-develop-csharp-module/module-list.png)

7. O ID do dispositivo IoT limite com o botão direito e pode monitorizar as mensagens de D2C no VS Code.
8. Para parar o runtime de IoT Edge e o módulo de sensor, escreva e selecione **Edge: parar Edge** na paleta do comando.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Desenvolver e implementar um módulo c# no VS Code
O tutorial [desenvolver um módulo c#](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module), atualizar, criar e publicar a imagem de módulo no VS Code. Em seguida, aceda ao portal do Azure para implementar o seu módulo c#. Esta secção apresenta como utilizar o VS Code para implementar e monitorizar o seu módulo c#.

### <a name="start-a-local-docker-registry"></a>Iniciar um registo de Docker local
Pode utilizar qualquer registo compatível com o Docker para este artigo. O [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) são dois populares serviços de registo do Docker que estão disponíveis na cloud. Esta secção utiliza um [registo Docker local](https://docs.docker.com/registry/deploying/), que é mais fácil para fins de teste durante o desenvolvimento antecipado.
No VS Code **terminal integrada** (Ctrl + '), execute o seguinte comando para iniciar um registo local:  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Este exemplo mostra as configurações de registo que apenas são adequadas para teste. Um registo de prontos para produção deve ser protegido de TLS e Idealmente, deve utilizar um mecanismo de controlo de acesso. Recomendamos que utilize [registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) ou [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) para implementar os módulos de limite de IoT prontos para produção.

### <a name="create-an-iot-edge-module-project"></a>Criar um projeto do módulo de limite de IoT
Os passos seguintes mostram como criar um módulo de limite de IoT com base no .NET Core 2.0, utilizando o Visual Studio Code e a extensão de limite de IoT do Azure. Se tiver concluído esta secção do tutorial anterior, pode ignorar com segurança nesta secção.
1. No Visual Studio Code, selecione **vista** > **integrada Terminal** para abrir o terminal integrado VS Code.
3. No terminal integrado, introduza o seguinte comando para instalar (ou atualizar) as **AzureIoTEdgeModule** modelo no dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Crie um projeto para o módulo novo. O comando seguinte cria a pasta do projeto, **FilterModule**, na pasta de trabalho atual:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```
 
3. Selecione **ficheiro** > **Abrir pasta**.
4. Navegue para o **FilterModule** pasta e clique em **selecionar pasta** para abrir o projeto no VS Code.
5. No Explorador de código de VS, selecione **Program.cs** para abri-lo. Na parte superior do **program.cs**, incluem os seguintes espaços de nomes:
   ```csharp
   using Microsoft.Azure.Devices.Shared;
   using System.Collections.Generic;  
   using Newtonsoft.Json;
   ```

6. Adicionar o `temperatureThreshold` variável para o **programa** classe. Esta variável define o valor pode exceder a temperatura medida para que os dados a ser enviadas ao IoT Hub. 

   ```csharp
   static int temperatureThreshold { get; set; } = 25;
   ```

7. Adicionar o `MessageBody`, `Machine`, e `Ambient` classes para o **programa** classe. Estas classes definem o schema esperado para o corpo de mensagens a receber.

    ```csharp
    class MessageBody
    {
        public Machine machine {get;set;}
        public Ambient ambient {get; set;}
        public string timeCreated {get; set;}
    }
    class Machine
    {
       public double temperature {get; set;}
       public double pressure {get; set;}         
    }
    class Ambient
    {
       public double temperature {get; set;}
       public int humidity {get; set;}         
    }
    ```

8. No **Init** método, o código que cria e configura um **DeviceClient** objeto. Este objeto permite que o módulo estabelecer ligação com o runtime de limite de IoT local para enviar e receber mensagens. Fornece o tempo de execução do limite de IoT para o módulo a cadeia de ligação utilizada no **Init** método. Depois de criar o **DeviceClient** objeto, o código regista uma chamada de retorno para a receção de mensagens do hub IoT limite através de **input1** ponto final. Substitua o `SetInputMessageHandlerAsync` método com um novo e adicione um `SetDesiredPropertyUpdateCallbackAsync` método para atualizações de propriedades pretendido. Para efetuar esta alteração, substitua a última linha do **Init** método com o seguinte código:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Adicionar o `onDesiredPropertiesUpdate` método para o **programa** classe. Este método recebe atualizações nas propriedades de pretendido de duplo o módulo e atualiza o **temperatureThreshold** variável para corresponder. Todos os módulos têm os seus próprios duplo de módulo, que lhe permite configurar o código em execução no interior de um módulo diretamente a partir da nuvem.

    ```csharp
    static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
    {
        try
        {
            Console.WriteLine("Desired property change:");
            Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

            if (desiredProperties["TemperatureThreshold"]!=null)
                temperatureThreshold = desiredProperties["TemperatureThreshold"];

        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error when receiving desired property: {0}", exception);
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error when receiving desired property: {0}", ex.Message);
        }
        return Task.CompletedTask;
    }
    ```

10. Substitua o `PipeMessage` método com o `FilterMessages` método. Este método é denominado sempre que o módulo recebe uma mensagem a partir do IoT Hub do limite. Filtra de mensagens em fila que temperatures inferior ao limiar de temperatura definidas através de duplo o módulo de relatórios. Também adiciona o **MessageType** propriedade à mensagem com o valor definido **alerta**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        int counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            byte[] messageBytes = message.GetBytes();
            string messageString = Encoding.UTF8.GetString(messageBytes);
            Console.WriteLine($"Received message {counterValue}: [{messageString}]");

            // Get message body
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                Console.WriteLine($"Machine temperature {messageBody.machine.temperature} " +
                    $"exceeds threshold {temperatureThreshold}");
                var filteredMessage = new Message(messageBytes);
                foreach (KeyValuePair<string, string> prop in message.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }

                filteredMessage.Properties.Add("MessageType", "Alert");
                await deviceClient.SendEventAsync("output1", filteredMessage);
            }

            // Indicate that the message treatment is completed
            return MessageResponse.Completed;
        }
        catch (AggregateException ex)
        {
            foreach (Exception exception in ex.InnerExceptions)
            {
                Console.WriteLine();
                Console.WriteLine("Error in sample: {0}", exception);
            }
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            DeviceClient deviceClient = (DeviceClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Para compilar o projeto, clique com botão direito do **FilterModule.csproj** no Explorador de ficheiros e selecione **módulo criar limite de IoT**. Este processo compila o módulo e exporta o binário e as respetivas dependências para uma pasta que é utilizado para criar uma imagem de Docker. 

    ![Captura de ecrã do Explorador de código de VS](./media/how-to-vscode-develop-csharp-module/build-module.png)

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Criar uma imagem de Docker e publicá-lo no seu registo

1. No Explorador de código de VS, expanda o **Docker** pasta. Em seguida, expanda a pasta da sua plataforma de contentores, **linux-x64** ou **windows-nano**.
2. Clique com botão direito do **Dockerfile** do ficheiro e selecione **imagem de Docker do módulo de limite de IoT criar**. 

    ![Captura de ecrã do Explorador de código de VS](./media/how-to-vscode-develop-csharp-module/build-docker-image.png)

3. No **selecionar pasta** janela, procurar ou introduzir `./bin/Debug/netcoreapp2.0/publish`. Selecione **selecionar pasta como EXE_DIR**.
4. Na caixa de texto de pop-up na parte superior da janela do VS Code, introduza o nome da imagem. Por exemplo: `<your container registry address>/filtermodule:latest`. Se estiver a implementar no registo local, deve ser `localhost:5000/filtermodule:latest`.
5. Envie a imagem para o seu repositório do Docker. Utilize o **Edge: imagem de Docker do módulo de limite de Push de IoT** de comandos e introduza o URL da imagem na caixa de texto de pop-up na parte superior da janela VS Code. Utilize o mesmo URL de imagem utilizada no passo anterior. Verifique o registo de consola para se certificar de que a imagem foi instalada com êxito.

    ![Captura de ecrã de enviar a imagem de Docker](./media/how-to-vscode-develop-csharp-module/push-image.png) ![captura de ecrã do registo da consola](./media/how-to-vscode-develop-csharp-module/pushed-image.png)

### <a name="deploy-your-iot-edge-modules"></a>Implementar os módulos de limite de IoT

1. Abra o `deployment.json` de ficheiros e substitua o **módulos** secção com o seguinte:
    ```json
    "tempSensor": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview",
            "createOptions": ""
        }
    },
    "filtermodule": {
        "version": "1.0",
        "type": "docker",
        "status": "running",
        "restartPolicy": "always",
        "settings": {
            "image": "localhost:5000/filtermodule:latest",
            "createOptions": ""
        }
    }
    ```

2. Substitua o **rotas** secção com o seguinte:
    ```json
    "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
    "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
    ```
   > [!NOTE]
   > As regras de declarativas em tempo de execução definem onde as mensagens de fluxo. Neste artigo, tem duas rotas. A primeira rota transporta mensagens do sensor de temperatura para o módulo de filtro através do ponto final "input1". Este é o ponto final que configurou com o processador de FilterMessages. A segunda rota transporta mensagens do módulo de filtro para o Hub IoT. Esta rota montante é um destino especial que informa o limite de IoT Hub para enviar mensagens para o IoT Hub.

3. Guarde este ficheiro.
4. Na paleta do comando, selecione **Edge: criar a implementação para o dispositivo de limite**. Em seguida, selecione o ID do dispositivo IoT limite para criar uma implementação. Em alternativa, o ID de dispositivo na lista de dispositivos com o botão direito e selecione **criar a implementação para o dispositivo de limite**.

    ![Captura de ecrã de criar a opção de implementação](./media/how-to-vscode-develop-csharp-module/create-deployment.png)

5. Selecione o `deployment.json` -lo. Na janela de resultados, pode ver saídas correspondentes para a sua implementação.

    ![Captura de ecrã da janela de saída](./media/how-to-vscode-develop-csharp-module/deployment-succeeded.png)

6. Iniciar o tempo de execução do limite de IoT na paleta do comando (selecione **Edge: Iniciar Edge**).
7. Pode ver o seu limite de IoT runtime iniciar a execução no Explorador de Docker, com o sensor simulada e o módulo de filtragem.

    ![Captura de ecrã do Explorador do Docker](./media/how-to-vscode-develop-csharp-module/solution-running.png)

8. O ID do dispositivo IoT limite com o botão direito e pode monitorizar as mensagens de D2C no VS Code.


## <a name="next-steps"></a>Passos Seguintes

[Depurar c# módulo no VS Code](how-to-vscode-debug-csharp-module.md)
