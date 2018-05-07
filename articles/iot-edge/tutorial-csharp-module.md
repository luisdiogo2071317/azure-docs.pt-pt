---
title: Módulo do Azure IoT Edge c# | Microsoft Docs
description: Criar um módulo de limite de IoT com código c# e implementá-la para um dispositivo de limite
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 09e20d9a80b881075d9bb6be7d4daafc739340a1
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Desenvolver e implementar um módulo de limite de IoT c# para o seu dispositivo simulado – pré-visualização

Pode utilizar módulos de limite de IoT para implementar o código que implementa a lógica de negócio diretamente nos seus dispositivos de limite de IoT. Este tutorial explica-lhe como criar e implementar um módulo de limite de IoT que filtra os dados de sensores. Irá utilizar o dispositivo simulado contorno de IoT que criou na implementação do Azure IoT limite num dispositivo simulado [Windows] [ lnk-tutorial1-win] ou [Linux] [ lnk-tutorial1-lin]tutoriais. Neste tutorial, ficará a saber como:    

> [!div class="checklist"]
> * Utilize o Visual Studio Code para criar um módulo de limite de IoT com base no .NET core 2.0
> * Utilize o Visual Studio Code e Docker para criar uma imagem de docker e publicá-lo no seu registo 
> * Implementar o módulo no seu dispositivo IoT Edge
> * Ver os dados gerados


O módulo de limite de IoT que criou neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo. Só envia mensagens upstream se a temperatura for superior um limiar especificado. Este tipo de análise no limite é útil para reduzir a quantidade de dados comunicados para e armazenados na nuvem. 

## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo de limite de IoT do Azure que criou no início rápido ou primeiro tutorial.
* A cadeia de ligação da chave primária do dispositivo IoT Edge.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Extensão C# para Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/) no mesmo computador que tenha o Visual Studio Code. A edição de Comunidade (CE) é suficiente para este tutorial. 
* [SDK de .NET Core 2.0](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, vai utilizar a extensão Azure IoT Edge para VS Code para criar um módulo e criar uma **imagem de contentor** a partir dos ficheiros. Em seguida, vai enviar essa imagem para um **registo** que armazena e gere as suas imagens. Por fim, vai implementar a imagem a partir do registo para ser executada no seu dispositivo IoT Edge.  

Pode utilizar qualquer registo compatível com o Docker neste tutorial. O [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) são dois populares serviços de registo do Docker que estão disponíveis na cloud. Este tutorial utiliza o Azure Container Registry. 

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contentores** > **Azure Container Registry**.
2. Dê um nome ao registo, escolha uma subscrição, escolha um grupo de recursos e defina o SKU como **Básico**. 
3. Selecione **Criar**.
4. Assim que o registo de contentores for criado, navegue para o mesmo e selecione **Chaves de acesso**. 
5. Mude **Utilizador administrador** para **Ativar**.
6. Copie os valores de **Servidor de início de sessão**, **Nome de utilizador** e **Palavra-passe**. Irá utilizar estes valores mais tarde no tutorial quando publicar a imagem de Docker no seu registo e, quando adiciona as credenciais de registo para o tempo de execução do limite. 

## <a name="create-an-iot-edge-module-project"></a>Criar um projeto do módulo de limite de IoT
A mostrar os passos seguintes, como criar um módulo de limite de IoT com base no .NET core 2.0 utilizando o Visual Studio Code e a extensão de limite de IoT do Azure.
1. No Visual Studio Code, selecione **vista** > **integrada Terminal** para abrir o terminal integrado VS Code.
2. No terminal integrado, introduza o seguinte comando para instalar (ou atualizar) as **AzureIoTEdgeModule** modelo no dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

3. Crie um projeto para o módulo novo. O comando seguinte cria a pasta do projeto, **FilterModule**, com o seu repositório de contentor. Se estiver a utilizar um registo do Azure Container Registry, o segundo parâmetro deve ter o formato `<your container registry name>.azurecr.io`. Introduza o seguinte comando na pasta de trabalho atual:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule -r <your container registry address>/filtermodule
    ```
 
4. Selecione **ficheiro** > **Abrir pasta**.
5. Navegue para o **FilterModule** pasta e clique em **selecionar pasta** para abrir o projeto no VS Code.
6. No Explorador de VS Code, clique em **Program.cs** para abri-lo.

   ![Abertos Program. CS][1]

7. Na parte superior do **FilterModule** espaço de nomes, adicionar três `using` instruções para tipos de utilizados mais tarde:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
    ```

8. Adicionar o `temperatureThreshold` variável para o **programa** classe. Esta variável define o valor pode exceder a temperatura medida para que os dados a ser enviadas ao IoT Hub. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

9. Adicionar o `MessageBody`, `Machine`, e `Ambient` classes para o **programa** classe. Estas classes definem o schema esperado para o corpo de mensagens a receber.

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

10. No **Init** método, o código que cria e configura um **DeviceClient** objeto. Este objeto permite que o módulo ligar a execução do Azure IoT Edge local para enviar e receber mensagens. A cadeia de ligação utilizada no **Init** método é fornecido para o módulo pelo tempo de execução do limite de IoT. Depois de criar o **DeviceClient**, o código lê a TemperatureThreshold a partir das propriedades de pretendido o duplo módulo e regista uma chamada de retorno para a receção de mensagens do hub IoT limite através de **input1**ponto final. Substitua o `SetInputMessageHandlerAsync` método com um novo e adicione um `SetDesiredPropertyUpdateCallbackAsync` método para atualizações de propriedades pretendido. Para efetuar esta alteração, substitua a última linha do **Init** método com o seguinte código:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine("Proerty TemperatureThreshold not exist");
    }

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

11. Adicionar o `onDesiredPropertiesUpdate` método para o **programa** classe. Este método recebe atualizações nas propriedades de pretendido de duplo o módulo e atualiza o **temperatureThreshold** variável para corresponder. Todos os módulos têm os seus próprios duplo de módulo, que lhe permite configurar o código em execução no interior de um módulo diretamente a partir da nuvem.

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

12. Substitua o `PipeMessage` método com o `FilterMessages` método. Este método é denominado sempre que o módulo recebe uma mensagem do hub IoT Edge. Filtra de mensagens em fila que temperatures inferior ao limiar de temperatura definidas através de duplo o módulo de relatórios. Também adiciona o **MessageType** propriedade à mensagem com o valor definido **alerta**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);

        try {
            DeviceClient deviceClient = (DeviceClient)userContext;

            var messageBytes = message.GetBytes();
            var messageString = Encoding.UTF8.GetString(messageBytes);
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
            var deviceClient = (DeviceClient)userContext;
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

13. Guarde este ficheiro.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Criar uma imagem do Docker e publicá-la no seu registo

1. Introduza o comando seguinte no terminal integrado do VS Code e inicie sessão no Docker. 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Para localizar o nome de utilizador, a palavra-passe e o servidor de início de sessão para utilizar neste comando, aceda ao [Portal do Azure] (https://portal.azure.com). Em **Todos os recursos**, clique no mosaico relativo ao seu registo do Azure Container Registry para abrir as respetivas propriedades e clique em **Chaves de acesso**. Copie os valores existentes nos campos **Nome e utilizador**, **Palavra-passe** e **Servidor de início de sessão**. 

2. No explorador do VS Code, clique com botão direito do rato no ficheiro **module.json** e clique em **Compilar e Emitir imagem de Docker do módulo IoT Edge**. Na caixa pendente de pop-up na parte superior da janela do VS Code, selecione a plataforma do seu contentor, **amd64** para contentor do Linux ou **windows-amd64** para contentor do Windows. O VS Code, em seguida, cria o seu código, containerize o `FilterModule.dll` e enviá-lo para o registo de contentor especificado.


3. Pode obter o endereço da imagem de contentor completo com a etiqueta no terminal integrado do VS Code. Para obter mais informações sobre a definição de compilação e emissão, pode ver o ficheiro `module.json`.

## <a name="add-registry-credentials-to-edge-runtime"></a>Adicionar credenciais de registo ao tempo de execução do contorno
Adicione as credenciais do registo ao runtime do Edge no computador onde está a executar o dispositivo Edge. Estas credenciais conceder o acesso de tempo de execução para o contentor de extração. 

- No Windows, execute o seguinte comando:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- No Linux, execute o seguinte comando:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Executar a solução

1. No [Portal do Azure](https://portal.azure.com), navegue para o seu hub IoT.
2. Aceda a **IoT Edge (pré-visualização)** e selecione o seu dispositivo IoT Edge.
3. Selecione **Definir Módulos**. 
4. Verifique se o **tempSensor** módulo é preenchido automaticamente. Se não for, utilize os seguintes passos para adicioná-lo:
    1. Selecione **Adicionar módulo do IoT Edge**.
    2. No campo **Nome**, introduza `tempSensor`.
    3. No campo **URI da Imagem**, introduza `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Deixe as outras definições inalteradas e clique em **Guardar**.
5. Adicionar o **filterModule** módulo que criou nas secções anteriores. 
    1. Selecione **Adicionar módulo do IoT Edge**.
    2. No campo **Nome**, introduza `filterModule`.
    3. No campo **URI da Imagem**, introduza o endereço da sua imagem, como, por exemplo, `<your container registry address>/filtermodule:0.0.1-amd64`. O endereço completo da imagem está disponível na secção anterior.
    4. Verifique o **ativar** caixa para que pode editar o duplo de módulo. 
    5. Substitua o JSON na caixa de texto para o duplo módulo com o seguinte JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Clique em **Guardar**.
6. Clique em **Seguinte**.
7. No passo **Especificar Rotas**, copie o JSON abaixo para a caixa de texto. Módulos publicar todas as mensagens para o tempo de execução do limite. As regras de declarativas em tempo de execução definem onde as mensagens de fluxo. Neste tutorial, precisa de dois rotas. A rota primeiro transportes mensagens do sensor de temperatura para o módulo de filtro através de ponto final "input1", que é o ponto final que configurou com o **FilterMessages** processador. A segunda rota transporta mensagens do módulo de filtro para o Hub IoT. Nesta rota, `upstream` é um destino especial que diz ao Hub Edge para enviar mensagens para o Hub IoT. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

8. Clique em **Seguinte**.
9. No passo **Rever Modelo**, clique em **Submeter**. 
10. Regresse à página de detalhes do dispositivo IoT Edge e clique em **Atualizar**. Deverá ver o novo **filtermodule** executar juntamente com o **tempSensor** módulo e a **IoT Edge runtime**. 

## <a name="view-generated-data"></a>Ver os dados gerados

Para monitorizar as mensagens do dispositivo para a cloud enviadas a partir do dispositivo IoT Edge para o hub IoT:
1. Configure a extensão Toolkit do Azure IoT com a cadeia de ligação do seu hub IoT: 
    1. Abra o Explorador de VS Code selecionando **vista** > **Explorer**. 
    2. No Explorador de, clique em **dispositivos do HUB IOT** e, em seguida, clique em **...** . Clique em **definir a cadeia de ligação do IoT Hub** e introduza a cadeia de ligação para o IoT hub que o dispositivo de limite de IoT liga na janela de pop-up. 

        Para localizar a cadeia de ligação, clique no mosaico do hub IoT no portal do Azure e, em seguida, clique em **políticas de acesso partilhado**. No **políticas de acesso partilhado**, clique em de **iothubowner** política e copie a ligação do IoT Hub da cadeia no **iothubowner** janela.   

2. Para monitorizar os dados que chegam ao IoT hub, selecione **vista** > **paleta de comando** e procure o **IoT: iniciar a monitorização de mensagem D2C** comandos de menu. 
3. Para parar a monitorização de dados, utilize o **IoT: parar a monitorização D2C mensagem** comandos de menu. 

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, vai criar um módulo de limite de IoT que contém o código para filtrar dados não processados gerados pelo seu dispositivo de limite de IoT. Pode continuar a sessão em qualquer um dos seguintes tutoriais para saber mais sobre outras formas de limite de IoT do Azure pode ajudar a que ativar dados em informações empresariais no limite.

> [!div class="nextstepaction"]
> [Implementar a função do Azure como um módulo](tutorial-deploy-function.md)
> [implementar o Azure Stream Analytics como um módulo](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
