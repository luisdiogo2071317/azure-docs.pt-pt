---
title: "Implementar a função do Azure com o limite de IoT do Azure | Microsoft Docs"
description: "Implementar a função do Azure como um módulo para um dispositivo de limite"
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: v-jamebr
ms.date: 11/15/2017
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1dfe46d307a076ae02362c4bba292602001ed915
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/09/2017
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Implementar a função do Azure como um módulo de limite de IoT – pré-visualização
Pode utilizar as funções do Azure para implementar o código que implementa a lógica de negócio diretamente nos seus dispositivos de limite de IoT. Este tutorial explica-lhe como criar e implementar uma função do Azure, que filtra os dados de sensores no dispositivo simulado contorno de IoT que criou na implementação do Azure IoT limite num dispositivo simulado no [Windows] [ lnk-tutorial1-win]ou [Linux] [ lnk-tutorial1-lin] tutoriais. Neste tutorial, ficará a saber como:     

> [!div class="checklist"]
> * Utilize o Visual Studio Code para criar uma função do Azure
> * Utilizar o VS Code e Docker para criar uma imagem de Docker e publicá-lo no seu registo 
> * Implementar o módulo para o seu dispositivo de limite de IoT
> * Dados de vista gerada


A função do Azure que criar neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo e apenas mensagens upstream quando envia para o IoT Hub do Azure a temperatura for superior um limiar especificado. 

## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo de limite de IoT do Azure que criou no início rápido ou tutorial anterior.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [C# para a extensão do Visual Studio Code (com tecnologia OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Extensão de limite de IoT do Azure para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). A edição de Comunidade (CE) para a sua plataforma é suficiente para este tutorial. 
* [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, utilize a extensão de limite de IoT do Azure para o VS Code para construir um módulo e criar um **imagem contentor** partir dos ficheiros. Em seguida, push esta imagem para um **registo** que armazena e gere as imagens. Por fim, implementar a imagem a partir do seu registo para executar no seu dispositivo de limite de IoT.  

Pode utilizar qualquer registo compatível com o Docker para este tutorial. Dois populares Docker registo serviços disponíveis na nuvem são [registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) e [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Este tutorial utiliza o registo de contentor do Azure. 

1. No [portal do Azure](https://portal.azure.com), selecione **crie um recurso** > **contentores** > **registo de contentor do Azure** .
2. Atribua o seu registo de um nome, escolha uma subscrição, escolha um grupo de recursos e o SKU de conjunto **básico**. 
3. Selecione **Criar**.
4. Assim que for criado o seu registo de contentor, navegue até ao mesmo e selecione **chaves de acesso**. 
5. Ativar/desativar **utilizador de Admin** para **ativar**.
6. Copie os valores para **servidor de início de sessão**, **Username**, e **palavra-passe**. Irá utilizar estes valores mais tarde no tutorial. 

## <a name="create-a-function-project"></a>Criar um projeto de função
Os passos seguintes mostram como criar uma função de limite de IoT utilizando o Visual Studio Code e a extensão de limite de IoT do Azure.
1. Abra o Visual Studio Code.
2. Para abrir o terminal integrado VS Code, selecione **vista** > **integrada Terminal**.
3. Para instalar (ou atualizar) as **AzureIoTEdgeFunction** modelo no dotnet, execute o seguinte comando no terminal integrado:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Crie um projeto para o módulo de novo. O comando seguinte cria a pasta do projeto, **FilterFunction**, na pasta de trabalho atual:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction
    ```

3. Selecione **ficheiro** > **Abrir pasta**, em seguida, procure o **FilterFunction** pasta e abra o projeto no VS Code.
4. No Explorador de VS Code, expanda o **EdgeHubTrigger Csharp** pasta, em seguida, abra o **run.csx** ficheiro.
5. Substitua o conteúdo do ficheiro com o seguinte código:

   ```csharp
   #r "Microsoft.Azure.Devices.Client"
   #r "Newtonsoft.Json"

   using System.IO;
   using Microsoft.Azure.Devices.Client;
   using Newtonsoft.Json;

   // Filter messages based on the temperature value in the body of the message and the temperature threshold value.
   public static async Task Run(Message messageReceived, IAsyncCollector<Message> output, TraceWriter log)
   {
        const int temperatureThreshold = 25;
        byte[] messageBytes = messageReceived.GetBytes();
        var messageString = System.Text.Encoding.UTF8.GetString(messageBytes);

        if (!string.IsNullOrEmpty(messageString))
        {
            // Get the body of the message and deserialize it
            var messageBody = JsonConvert.DeserializeObject<MessageBody>(messageString);

            if (messageBody != null && messageBody.machine.temperature > temperatureThreshold)
            {
                // Send the message to the output as the temperature value is greater than the threashold
                var filteredMessage = new Message(messageBytes);
                // Copy the properties of the original message into the new Message object
                foreach (KeyValuePair<string, string> prop in messageReceived.Properties)
                {
                    filteredMessage.Properties.Add(prop.Key, prop.Value);
                }
                // Add a new property to the message to indicate it is an alert
                filteredMessage.Properties.Add("MessageType", "Alert");
                // Send the message        
                await output.AddAsync(filteredMessage);
                log.Info("Received and transferred a message with temperature above the threshold");
            }
        }
    }

    //Define the expected schema for the body of incoming messages
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

11. Guarde o ficheiro.

## <a name="publish-a-docker-image"></a>Publicar uma imagem de Docker

1. Compilar a imagem do Docker.
    1. No Explorador de VS Code, expanda o **Docker** pasta. Em seguida, expanda a pasta para a sua plataforma de contentor, quer **linux x64** ou **windows nano**. 
    2. Clique com botão direito do **Dockerfile** do ficheiro e clique em **imagem de Docker do módulo de limite de IoT criar**. 
    3. Navegue para o **FilterFunction** pasta do projeto e clique em **selecionar pasta como EXE_DIR**. 
    4. Na caixa de texto de pop-up na parte superior da janela VS Code, introduza o nome da imagem. Por exemplo: `<your container registry address>/filterfunction:latest`. O endereço de registo do contentor é o mesmo que o servidor de início de sessão que copiou do seu registo. Deverá estar no formato `<your container registry name>.azurecr.io`.
 
4. Inicie sessão no Docker. No terminal integrado, introduza o seguinte comando: 

   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Para localizar o nome de utilizador, o servidor de início de sessão e palavra-passe para utilizar neste comando, vá a [portal do Azure] (https://portal.azure.com). De **todos os recursos**, clique no mosaico para o registo de contentor do Azure para abrir as respetivas propriedades, em seguida, clique em **chaves de acesso**. Copie os valores existentes no **Username**, **palavra-passe**, e **servidor de início de sessão** campos. 

3. A imagem de emissão para o seu repositório de Docker. Selecione **vista** > **comando paleta...**  , em seguida, procure **Edge: imagem de Docker do módulo de limite de Push de IoT**.
4. Na caixa de texto de pop-up, introduza o mesmo nome de imagem que utilizou no passo 1.d.

## <a name="add-registry-credentials-to-your-edge-device"></a>Adicione as credenciais do registo para o seu dispositivo de limite
Adicione as credenciais para o registo para o tempo de execução do Edge no computador onde está a executar o dispositivo de limite. Isto permite que o acesso de tempo de execução para o contentor de extração. 

- Para o Windows, execute o seguinte comando:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Para Linux, execute o seguinte comando:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Executar a solução

1. No **portal do Azure**, navegue até ao seu IoT hub.
2. Aceda a **IoT Edge (pré-visualização)** e selecione o seu dispositivo de limite de IoT.
1. Selecione **definir módulos**. 
2. Se já tiver implementado o **tempSensor** módulo para este dispositivo, poderá ser automaticamente preenchido. Caso contrário, siga estes passos para adicioná-lo:
    1. Selecione **Adicionar módulo IoT Edge**.
    2. No **nome** campo, introduza `tempSensor`.
    3. No **URI de imagem** campo, introduza `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Deixe as outras definições inalteradas e clique em **guardar**.
1. Adicionar o **filterFunction** módulo.
    1. Selecione **Adicionar módulo de limite de IoT** novamente.
    2. No **nome** campo, introduza `filterFunction`.
    3. No **imagem** campo, introduza o seu endereço de imagem; por exemplo `<docker registry address>/filterfunction:latest`.
    74. Clique em **Guardar**.
2. Clique em **Seguinte**.
3. No **especificar rotas** passo, copie o JSON abaixo na caixa de texto. A rota primeiro transportes mensagens do sensor de temperatura para o módulo de filtro através do ponto final "input1". A segunda rota transportes mensagens do módulo de filtro ao IoT Hub. Nesta rota `$upstream` é um destino especial que diz ao Hub Edge para enviar mensagens para o IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Clique em **Seguinte**.
5. No **rever modelo** passo, clique em **submeter**. 
6. Regressar à página de detalhes do dispositivo IoT limite e clique em **atualizar**. Deverá ver o novo **filterfunction** módulo em execução juntamente com o **tempSensor** módulo e a **IoT Edge runtime**. 

## <a name="view-generated-data"></a>Dados de vista gerada

Para monitorizar as mensagens da nuvem enviados a partir do seu dispositivo de limite de IoT ao seu IoT hub do dispositivo:
1. Configure a extensão do Toolkit de IoT do Azure com a cadeia de ligação do IoT hub: 
    1. No portal do Azure, navegue até ao seu IoT hub e selecione **políticas de acesso partilhado**. 
    2. Selecione **iothubowner** , em seguida, copie o valor da **chave primária de cadeia de ligação**.
    1. No Explorador de VS Code, clique em **dispositivos do HUB IOT** e, em seguida, clique em **...** . 
    1. Selecione **definir a cadeia de ligação do IoT Hub** e introduza a cadeia de ligação do Iot Hub na janela de pop-up. 

1. Para monitorizar os dados que chegam ao IoT hub, selecione o **vista** > **paleta de comando...**  e procure **IoT: iniciar a monitorização de mensagem D2C**. 
2. Para parar a monitorização de dados, utilize o **IoT: parar a monitorização D2C mensagem** comando na paleta do comando. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, vai criar uma função do Azure que contém o código para filtrar dados não processados gerados pelo seu dispositivo de limite de IoT. Para manter explorar o Azure IoT Edge, saiba como utilizar um dispositivo de limite de IoT como um gateway. 

> [!div class="nextstepaction"]
> [Criar um dispositivo de gateway do limite de IoT](how-to-create-transparent-gateway.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
