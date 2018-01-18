---
title: "Utilizar o Visual Studio Code para desenvolver um módulo de c# com o Azure IoT Edge | Microsoft Docs"
description: "Desenvolver e implementar um módulo de c# com o Azure IoT Edge no VS Code sem mudar de contexto"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 01/11/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: cad28b4e6d4e46058641da19795cd71efdbd0c92
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-develop-c-module-with-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver c# módulo com o Azure IoT Edge
Este artigo fornece instruções detalhadas para utilizar [Visual Studio Code](https://code.visualstudio.com/) como a ferramenta de desenvolvimento principal para desenvolver e implementar os módulos de limite de IoT. 

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT ser outro dispositivo físico ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

Certifique-se de que ter tutoriais seguintes concluídas antes de começar esta orientação.
- Implementar o Azure IoT Edge num dispositivo simulado [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) ou [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Desenvolver e implementar um módulo de limite de IoT c# para o seu dispositivo simulado](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module)

Eis uma lista de verificação que mostra os itens que deve ter depois de concluir os tutoriais anteriores.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Extensão de limite de IoT do Azure para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
- [C# para a extensão do Visual Studio Code (com tecnologia OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 
- [Python 2.7](https://www.python.org/downloads/)
- [Script de controlo contorno de IoT](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- AzureIoTEdgeModule template (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Um hub IoT Active Directory com, pelo menos, um dispositivo de limite de IoT.

-Também é sugerida para instalar [suporte de Docker para o VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para melhor gerir imagens do módulo e contentores.

## <a name="deploy-an-azure-iot-edge-module-in-vs-code"></a>Implementar um módulo do Azure IoT Edge no VS Code

### <a name="list-your-iot-hub-devices"></a>Lista os dispositivos do hub IoT
Existem duas formas para listar os dispositivos do hub IoT no seu código de VS. Pode escolher qualquer forma para continuar.

#### <a name="sign-in-your-azure-account-in-vscode-and-choose-your-iot-hub"></a>Inicie sessão na sua conta do Azure no VSCode e escolha o seu IoT hub
1. Na paleta de comando (F1 ou Ctrl + Shift + P), escreva e selecione **Azure: a iniciar sessão**. Em seguida, clique em  **cópia* & abra** no pop-up. Cole (Ctrl + V) o código no seu browser e clique o botão para continuar. Em seguida, inicie sessão com a sua conta do Azure. Pode ver as suas informações de conta na barra de estado do código de VS.
2. Na paleta de comando (F1 ou Ctrl + Shift + P), escreva e selecione **IoT: selecione o IoT Hub**. Primeiro selecione a subscrição onde criou o seu IoT hub tutorial anterior. Em seguida, escolha o IoT hub que contém o dispositivo de limite de IoT.

    ![Lista de dispositivos](./media/how-to-vscode-develop-csharp-module/device-list.png)

#### <a name="set-iot-hub-connection-string"></a>Definir a cadeia de ligação do hub IoT
Na paleta de comando (F1 ou Ctrl + Shift + P), escreva e selecione **IoT: definir a cadeia de ligação do IoT Hub**. Certifique-se de que cole a cadeia de ligação pela política **iothubowner** (pode encontrá-lo em políticas de acesso partilhado do seu IoT hub no portal do Azure).
 
Pode ver a lista de dispositivos no Explorador de dispositivos do IoT Hub na barra do lado esquerdo.

### <a name="start-your-iot-edge-runtime-and-deploy-a-module"></a>Iniciar o tempo de execução do limite de IoT e implementar um módulo
Instalar e iniciar o tempo de execução do Azure IoT Edge no seu dispositivo. E implementar um módulo de sensor simulado que envia dados de telemetria ao IoT Hub.
1. Na paleta do comando, selecione **Edge: limite de configuração** e escolha o limite de IoT ID do dispositivo. Ou o ID de dispositivo de limite na lista de dispositivos com o botão direito e selecione **configuração Edge**.

    ![Tempo de execução do programa de configuração Edge](./media/how-to-vscode-develop-csharp-module/setup-edge.png)

2. Na paleta do comando, selecione **Edge: Iniciar Edge** para iniciar o tempo de execução do limite. Pode ver as saídas correspondentes no terminal integrada.

    ![Tempo de execução de limite de início](./media/how-to-vscode-develop-csharp-module/start-edge.png)

3. Verifique o estado de runtime Edge no Explorador do Docker. Verde significa está em execução. O tempo de execução do limite de IoT é iniciado com êxito.

    ![Tempo de execução de limite está em execução](./media/how-to-vscode-develop-csharp-module/edge-runtime.png)

4. Agora está a executar o tempo de execução de limite, o que significa agora o seu PC simula um dispositivo de limite. Passo seguinte é para simular um sensorthing que mantém a enviar mensagens para o seu dispositivo de limite. Na paleta de comandos, escreva e selecione **contorno: o ficheiro de configuração de gerar Edge**. E selecione uma pasta para criar este ficheiro. No ficheiro deployment.json gerado, substitua o conteúdo `<registry>/<image>:<tag>` com `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` e guarde o ficheiro.

    ![Módulo do sensor](./media/how-to-vscode-develop-csharp-module/sensor-module.png)

5. Selecione **Edge: criar a implementação para o dispositivo de limite** e escolha o ID de dispositivo de limite para criar uma nova implementação. Ou o ID de dispositivo de limite na lista de dispositivos com o botão direito e selecione **criar a implementação para o dispositivo de limite**. 

6. Deverá ver o seu limite de IoT iniciar a execução no Explorador do Docker com o sensor simulado. Clique com botão direito do contentor no Explorador de Docker. Pode ver os registos de docker para cada módulo. Além disso, pode ver a lista de módulos na lista de dispositivos.

    ![Lista de módulo](./media/how-to-vscode-develop-csharp-module/module-list.png)

7. O ID do dispositivo de limite com o botão direito e pode monitorizar as mensagens de D2C no VS Code.
8. Para parar o runtime de IoT Edge e o módulo de sensor, escreva e selecione **Edge: parar Edge** na paleta do comando.

## <a name="develop-and-deploy-a-c-module-in-vs-code"></a>Desenvolver e implementar um módulo c# no VS Code
Tutorial [desenvolver um módulo c#](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module), atualizar, criar e publicar a imagem de módulo no VS Code e, em seguida, visitar o portal do Azure para implementar o seu módulo c#. Esta secção apresenta como utilizar o VS Code para implementar e monitorizar o seu módulo c#.

### <a name="start-a-local-docker-registry"></a>Iniciar um registo de local docker
Pode utilizar qualquer registo compatível com o Docker para este tutorial. Dois populares Docker registo serviços disponíveis na nuvem são [registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) e [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Esta secção utiliza um [registo Docker local](https://docs.docker.com/registry/deploying/), que é mais fácil para fins de teste durante o desenvolvimento antecipado.
No VS Code **terminal integrada**(Ctrl + '), execute os seguintes comandos para iniciar um registo local.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Acima exemplo mostra as configurações de registo que apenas são adequadas para teste. Um registo de prontos para produção deve ser protegido de TLS e Idealmente, deve utilizar um mecanismo de controlo de acesso. Recomendamos que utilize [registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) ou [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) para implementar os módulos de limite de IoT prontos para produção.

### <a name="create-an-iot-edge-module-project"></a>Criar um projeto do módulo de limite de IoT
A mostrar os passos seguintes, como criar um módulo de limite de IoT com base no .NET core 2.0 utilizando o Visual Studio Code e a extensão de limite de IoT do Azure. Se tiver concluído esta secção tutorial anterior, pode ignorar com segurança nesta secção.
1. No Visual Studio Code, selecione **vista** > **integrada Terminal** para abrir o terminal integrado VS Code.
3. No terminal integrado, introduza o seguinte comando para instalar (ou atualizar) as **AzureIoTEdgeModule** modelo no dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

2. Crie um projeto para o módulo de novo. O comando seguinte cria a pasta do projeto, **FilterModule**, na pasta de trabalho atual:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule
    ```
 
3. Selecione **ficheiro** > **Abrir pasta**.
4. Navegue para o **FilterModule** pasta e clique em **selecionar pasta** para abrir o projeto no VS Code.
5. No Explorador de VS Code, clique em **Program.cs** para abri-lo. Na parte superior do **program.cs**, incluir abaixo espaços de nomes.
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

8. No **Init** método, o código que cria e configura um **DeviceClient** objeto. Este objeto permite que o módulo ligar a execução do Azure IoT Edge local para enviar e receber mensagens. A cadeia de ligação utilizada no **Init** método é fornecido para o módulo pelo tempo de execução do limite de IoT. Depois de criar o **DeviceClient**, o código regista uma chamada de retorno para a receção de mensagens do hub IoT limite através de **input1** ponto final. Substitua o `SetInputMessageHandlerAsync` método com um novo e adicione um `SetDesiredPropertyUpdateCallbackAsync` método para atualizações de propriedades pretendido. Para efetuar esta alteração, substitua a última linha do **Init** método com o seguinte código:

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

10. Substitua o `PipeMessage` método com o `FilterMessages` método. Este método é denominado sempre que o módulo recebe uma mensagem do hub IoT Edge. Filtra de mensagens em fila que temperatures inferior ao limiar de temperatura definidas através de duplo o módulo de relatórios. Também adiciona o **MessageType** propriedade à mensagem com o valor definido **alerta**. 

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

11. Para compilar o projeto, clique com botão direito do **FilterModule.csproj** ficheiro no Explorer e clique **módulo criar limite de IoT**. Este processo compila o módulo e exporta o binário e as respetivas dependências para uma pasta que é utilizado para criar uma imagem de Docker. 

    ![Criar o módulo](./media/how-to-vscode-develop-csharp-module/build-module.png)

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Criar uma imagem de Docker e publicá-lo no seu registo

1. No Explorador de VS Code, expanda o **Docker** pasta. Em seguida, expanda a pasta para a sua plataforma de contentor, quer **linux x64** ou **windows nano**.
2. Clique com botão direito do **Dockerfile** do ficheiro e clique em **imagem de Docker do módulo de limite de IoT criar**. 

    ![Compilar a imagem do docker](./media/how-to-vscode-develop-csharp-module/build-docker-image.png)

3. No **selecionar pasta** janela, procurar ou introduzir `./bin/Debug/netcoreapp2.0/publish`. Clique em **selecionar pasta como EXE_DIR**.
4. Na caixa de texto de pop-up na parte superior da janela VS Code, introduza o nome da imagem. Por exemplo: `<your container registry address>/filtermodule:latest`. Se estiver a implementar no registo local, deve ser `localhost:5000/filtermodule:latest`.
5. A imagem de emissão para o seu repositório de Docker. Utilize o **Edge: imagem de Docker do módulo de limite de Push de IoT** de comandos e introduza o URL da imagem na caixa de texto de pop-up na parte superior da janela VS Code. Utilize o mesmo URL de imagem utilizada nos passos acima. Verifique o registo da consola e certifique-se que a imagem foi instalada com êxito.

    ![Push imagem docker](./media/how-to-vscode-develop-csharp-module/push-image.png) ![imagem premido docker](./media/how-to-vscode-develop-csharp-module/pushed-image.png)

### <a name="deploy-your-iot-edge-modules"></a>Implementar os módulos de limite de IoT

1. Abra o `deployment.json` de ficheiros, substitua **módulos** secção com abaixo conteúdo:
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

2. Substitua o **rotas** secção com abaixo conteúdo:
    ```json
    "sensorToFilter": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filtermodule/inputs/input1\")",
    "filterToIoTHub": "FROM /messages/modules/filtermodule/outputs/output1 INTO $upstream"
    ```
   > [!NOTE]
   > As regras de declarativas em tempo de execução definem onde as mensagens de fluxo. Neste tutorial, precisa de dois rotas. A rota primeiro transportes mensagens do sensor de temperatura para o módulo de filtro através de ponto final "input1", que é o ponto final que configurou com o processador de FilterMessages. A segunda rota transportes mensagens do módulo de filtro ao IoT Hub. Esta rota montante é um destino especial que diz ao Hub Edge para enviar mensagens para o IoT Hub.

3. Guarde este ficheiro.
4. Na paleta do comando, selecione **Edge: criar a implementação para o dispositivo de limite**. Em seguida, selecione o ID do dispositivo IoT limite para criar uma implementação. Ou o ID de dispositivo na lista de dispositivos com o botão direito e selecione **criar a implementação para o dispositivo de limite**.

    ![Criar implementação](./media/how-to-vscode-develop-csharp-module/create-deployment.png)

5. Selecione o `deployment.json` -lo. Na janela de resultados, pode ver saídas correspondentes para a sua implementação.

    ![Implementação efetuada com êxito](./media/how-to-vscode-develop-csharp-module/deployment-succeeded.png)

6. Inicie o tempo de execução do limite na paleta do comando. **Limite: Limite de início**
7. Pode ver o seu limite de IoT runtime iniciar a execução no Explorador do Docker com os sensores simulados e o módulo de filtragem.

    ![Solução de IoT ponta está em execução](./media/how-to-vscode-develop-csharp-module/solution-running.png)

8. O ID do dispositivo de limite com o botão direito e pode monitorizar as mensagens de D2C no VS Code.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou um módulo de limite de IoT e implementado no dispositivo de limite de IoT no VS Code. Pode continuar a sessão em qualquer um dos seguintes tutoriais para saber mais sobre outros cenários quando desenvolver Azure IoT Edge no VS Code.

> [!div class="nextstepaction"]
> [Depurar c# módulo no VS Code](how-to-vscode-debug-csharp-module.md)
