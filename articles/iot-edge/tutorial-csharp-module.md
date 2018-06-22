---
title: Módulo C# do Azure IoT Edge | Microsoft Docs
description: Criar um módulo do IoT Edge com código C# e implementá-lo num dispositivo Edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1da3a246a2ad33a4563f491058f5d4d115f3954d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631076"
---
# <a name="develop-and-deploy-a-c-iot-edge-module-to-your-simulated-device---preview"></a>Desenvolver e implementar um módulo do IoT Edge do C# no seu dispositivo simulado – pré-visualização

Pode utilizar os módulos do IoT Edge para implementar código que aplica a sua lógica de negócio diretamente nos seus dispositivos IoT Edge. Este tutorial explica-lhe como criar e implementar um módulo do IoT Edge que filtra dados de sensores. Vai utilizar o dispositivo IoT Edge simulado que criou na Implementação do Azure IoT Edge num dispositivo simulado em tutoriais do [Windows][lnk-tutorial1-win] ou do [Linux][lnk-tutorial1-lin]. Neste tutorial, ficará a saber como:    

> [!div class="checklist"]
> * Utilize o Visual Studio Code para criar um módulo do IoT Edge com base no .NET core 2.0
> * Utilizar o Visual Studio Code e o Docker para criar uma imagem do Docker e publicá-la no seu registo 
> * Implementar o módulo no seu dispositivo IoT Edge
> * Ver os dados gerados


O módulo do IoT Edge que criou neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo. Envia apenas mensagens de origem se a temperatura for superior a um limiar especificado. Este tipo de análise no Edge é útil para reduzir a quantidade de dados comunicados e armazenados na cloud. 

## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo Azure IoT Edge que criou no início rápido ou no primeiro tutorial.
* A cadeia de ligação da chave primária do dispositivo IoT Edge.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Extensão C# para Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Docker](https://docs.docker.com/engine/installation/) no mesmo computador que tenha o Visual Studio Code. A Community Edition (CE) é suficiente para este tutorial. 
* [SDK de .NET Core 2.0](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, vai utilizar a extensão Azure IoT Edge para VS Code para criar um módulo e criar uma **imagem de contentor** a partir dos ficheiros. Em seguida, vai enviar essa imagem para um **registo** que armazena e gere as suas imagens. Por fim, vai implementar a imagem a partir do registo para ser executada no seu dispositivo IoT Edge.  

Pode utilizar qualquer registo compatível com o Docker neste tutorial. O [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) são dois populares serviços de registo do Docker que estão disponíveis na cloud. Este tutorial utiliza o Azure Container Registry. 

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contentores** > **Azure Container Registry**.
2. Dê um nome ao registo, escolha uma subscrição, escolha um grupo de recursos e defina o SKU como **Básico**. 
3. Selecione **Criar**.
4. Assim que o registo de contentores for criado, navegue para o mesmo e selecione **Chaves de acesso**. 
5. Mude **Utilizador administrador** para **Ativar**.
6. Copie os valores de **Servidor de início de sessão**, **Nome de utilizador** e **Palavra-passe**. Irá utilizar estes valores mais tarde no tutorial quando publicar a imagem do Docker no seu registo e, quando adicionar as credenciais de registo ao runtime do Edge. 

## <a name="create-an-iot-edge-module-project"></a>Criar um projeto do módulo do IoT Edge
Os passos seguintes mostram-lhe como criar um módulo do IoT Edge baseado no .Net core 2.0 com o Visual Studio Code e a extensão Azure IoT Edge.
1. No Visual Studio Code, selecione **Vista** > **Terminal Integrado** para abrir o terminal integrado do VS Code.
2. No terminal integrado, introduza o comando seguinte para instalar (ou atualizar) o modelo **AzureIoTEdgeModule** em dotnet:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Module
    ```

3. Crie um projeto para o módulo novo. O comando seguinte cria a pasta do projeto, **FilterModule**, com o repositório do contentor. Se estiver a utilizar um registo do Azure Container Registry, o segundo parâmetro deve ter o formato `<your container registry name>.azurecr.io`. Introduza o seguinte comando na pasta de trabalho atual:

    ```cmd/sh
    dotnet new aziotedgemodule -n FilterModule -r <your container registry address>/filtermodule
    ```
 
4. Selecione **Ficheiro** > **Abrir Pasta**.
5. Navegue para a pasta **FilterModule** e clique em **Selecionar Pasta** para abrir o projeto no VS Code.
6. No explorador do VS Code, clique em **Program.cs** para abri-lo.

   ![Abra o Program.cs][1]

7. Na parte superior do espaço de nomes **FilterModule**, adicione três declarações `using` para tipos utilizados mais tarde:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
    ```

8. Adicione a variável `temperatureThreshold` à classe **Programa**. Esta variável define o valor que a temperatura medida deve exceder, para que os dados sejam enviados para o Hub IoT. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

9. Adicione as classes `MessageBody`, `Machine` e `Ambient` à classe **Programa**. Estas classes definem o esquema esperado para o corpo das mensagens a receber.

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

10. No método **Init**, o código cria e configura um objeto **DeviceClient**. Este objeto permite que o módulo se ligue ao runtime do Azure IoT Edge local para enviar e receber mensagens. A cadeia de ligação utilizada no método **Init** é dada ao módulo através do runtime do IoT Edge. Depois de criar o **DeviceClient**, o código lê o TemperatureThreshold a partir das propriedades pretendidas do Módulo Duplo e regista uma chamada de retorno para receber mensagens do hub do IoT Edge através do ponto final **input1**. Substitua o método `SetInputMessageHandlerAsync` por um novo e adicione um método `SetDesiredPropertyUpdateCallbackAsync`, para obter as atualizações de propriedades pretendidas. Para fazer esta alteração, substitua a última linha do método **Init** pelo seguinte código:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine("Property TemperatureThreshold not exist");
    }

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

11. Adicione o método `onDesiredPropertiesUpdate` à classe **Programa**. Este método recebe atualizações nas propriedades pretendidas do módulo duplo e atualiza a variável **temperatureThreshold** para corresponder. Todos os módulos têm o seu próprio módulo duplo, que lhe permite configurar o código em execução no interior de um módulo diretamente a partir da cloud.

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

12. Substitua o método `PipeMessage` pelo método `FilterMessages`. Este método é chamado sempre que o módulo recebe uma mensagem do hub do IoT Edge. Ele filtra as mensagens que comunicam temperaturas inferiores ao limiar de temperatura definido através do módulo duplo. Também adiciona a propriedade **MessageType** à mensagem com o valor definido para **Alerta**. 

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

2. No explorador do VS Code, clique com botão direito do rato no ficheiro **module.json** e clique em **Compilar e Emitir imagem de Docker do módulo IoT Edge**. Na caixa pendente de pop-up na parte superior da janela do VS Code, selecione a plataforma do seu contentor, **amd64** para contentor do Linux ou **windows-amd64** para contentor do Windows. Em seguida, o VS Code compila o seu código, contentoriza os `FilterModule.dll` e envia-os para o registo de contentores que tiver especificado.


3. Pode obter o endereço da imagem de contentor completo com a etiqueta no terminal integrado do VS Code. Para obter mais informações sobre a definição de compilação e emissão, pode ver o ficheiro `module.json`.

## <a name="add-registry-credentials-to-edge-runtime"></a>Adicionar as credenciais do registo ao runtime do Edge
Adicione as credenciais do registo ao runtime do Edge no computador onde está a executar o dispositivo Edge. Estas credenciais oferecem ao runtime acesso para extrair o contentor. 

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
4. Verifique se o módulo **tempSensor** é preenchido automaticamente. Se não for, utilize os seguintes passos para adicioná-lo:
    1. Selecione **Adicionar módulo do IoT Edge**.
    2. No campo **Nome**, introduza `tempSensor`.
    3. No campo **URI da Imagem**, introduza `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Deixe as outras definições inalteradas e clique em **Guardar**.
5. Adicione o módulo **filterModule** que criou nas secções anteriores. 
    1. Selecione **Adicionar módulo do IoT Edge**.
    2. No campo **Nome**, introduza `filterModule`.
    3. No campo **URI da Imagem**, introduza o endereço da sua imagem, como, por exemplo, `<your container registry address>/filtermodule:0.0.1-amd64`. O endereço completo da imagem está disponível na secção anterior.
    4. Assinale a caixa **Ativar**, para que possa editar o módulo duplo. 
    5. Substitua o JSON na caixa de texto pelo módulo duplo pelo seguinte JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Clique em **Guardar**.
6. Clique em **Seguinte**.
7. No passo **Especificar Rotas**, copie o JSON abaixo para a caixa de texto. Os módulos publicam todas as mensagens no runtime do Edge. As regras declarativas no runtime definem onde as mensagens fluem. Neste tutorial, vai precisar de duas rotas. A primeira rota transporta mensagens do sensor de temperatura para o módulo de filtro através do ponto final "input1", que é o ponto final que configurou no processador **FilterMessages**. A segunda rota transporta mensagens do módulo de filtro para o Hub IoT. Nesta rota, `upstream` é um destino especial que diz ao Hub Edge para enviar mensagens para o Hub IoT. 

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
10. Regresse à página de detalhes do dispositivo IoT Edge e clique em **Atualizar**. Deverá ver o novo **filtermodule** em execução, juntamente com o módulo **tempSensor** e o **runtime do IoT Edge**. 

## <a name="view-generated-data"></a>Ver os dados gerados

Para monitorizar as mensagens do dispositivo para a cloud enviadas a partir do dispositivo IoT Edge para o hub IoT:
1. Configure a extensão Toolkit do Azure IoT com a cadeia de ligação do seu hub IoT: 
    1. Abra o explorador do VS Code ao selecionar **Vista** > **Explorador**. 
    2. No explorador, clique em **DISPOSITIVOS IOT HUB** e clique em **...**. Clique em **Definir Cadeia de Ligação do Hub IoT** e introduza a cadeia de ligação para o hub IoT que o seu dispositivo do IoT Edge liga na janela de pop-up. 

        Para encontrar a cadeia de ligação, clique no mosaico do hub IoT no portal do Azure e, em seguida, clique em **Políticas de acesso partilhado**. Em **Políticas de acesso partilhado**, clique na política **iothubowner** e copie a cadeia de ligação do Hub IoT na janela **iothubowner**.   

2. Para monitorizar os dados que chegam ao hub IoT, selecione **Vista** > **Palete de Comandos** e procure o comando de menu **IoT: Começar a monitorizar mensagens D2C**. 
3. Para parar a monitorização de dados, utilize o comando de menu **IoT: Parar a monitorização de mensagens D2C**. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma função do módulo do IoT Edge que contém código para filtrar dados não processados gerados pelo seu dispositivo IoT Edge. Pode continuar para qualquer um dos seguintes tutoriais para saber mais sobre outras formas em que o Azure IoT Edge o pode ajudar a transformar os dados em informações empresariais no Edge.

> [!div class="nextstepaction"]
> [Implementar a Função do Azure como um módulo](tutorial-deploy-function.md)
> [Implementar o Azure Stream Analytics como um módulo](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
