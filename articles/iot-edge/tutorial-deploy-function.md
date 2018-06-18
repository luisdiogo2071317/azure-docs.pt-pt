---
title: Implementar as Funções do Azure com o Azure IoT Edge | Microsoft Docs
description: Implementar as Funções do Azure como módulo para um dispositivo de periferia
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 04/02/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 6102a28ec92f841fe32652e4dac36848d69e389c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631705"
---
# <a name="deploy-azure-function-as-an-iot-edge-module---preview"></a>Implementar as Funções do Azure como módulo do IoT Edge - pré-visualização
Pode utilizar as Funções do Azure para implementar código que aplica a sua lógica de negócio diretamente nos seus dispositivos IoT Edge. Este tutorial orienta-o ao longo da criação e implementação de uma função das Funções do Azure que filtra dados de sensores no dispositivo IoT Edge simulado que criou no tutorial “Deploy Azure IoT Edge on a simulated device” (“Implementar o Azure IoT Edge num dispositivo simulado”) para[Windows][lnk-tutorial1-win] ou [Linux][lnk-tutorial1-lin]. Neste tutorial, ficará a saber como:     

> [!div class="checklist"]
> * Utilize o Visual Studio Code para criar uma função do Azure
> * Utilizar o VS Code e o Docker para criar uma imagem do Docker e publicá-la no seu registo 
> * Implementar o módulo no seu dispositivo IoT Edge
> * Ver os dados gerados


A função das Funções do Azure que criar neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo e só envia mensagens para o Hub IoT do Azure quando a temperatura for superior a um limiar especificado. 

## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo Azure IoT Edge criou no início rápido ou no tutorial anterior.
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão C# para Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
* [Extensão Azure IoT Edge para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Docker](https://docs.docker.com/engine/installation/). A Community Edition (CE) da plataforma é suficiente para este tutorial. 
* [SDK de .NET Core 2.0](https://www.microsoft.com/net/core#windowscmd). 

## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, vai utilizar a extensão Azure IoT Edge para VS Code para criar um módulo e criar uma **imagem de contentor** a partir dos ficheiros. Em seguida, vai enviar essa imagem para um **registo** que armazena e gere as suas imagens. Por fim, vai implementar a imagem a partir do registo para ser executada no seu dispositivo IoT Edge.  

Pode utilizar qualquer registo compatível com o Docker neste tutorial. O [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) são dois populares serviços de registo do Docker que estão disponíveis na cloud. Este tutorial utiliza o Azure Container Registry. 

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contentores** > **Azure Container Registry**.
2. Dê um nome ao registo, escolha uma subscrição, escolha um grupo de recursos e defina o SKU como **Básico**. 
3. Selecione **Criar**.
4. Assim que o registo de contentores for criado, navegue para o mesmo e selecione **Chaves de acesso**. 
5. Mude **Utilizador administrador** para **Ativar**.
6. Copie os valores de **Servidor de início de sessão**, **Nome de utilizador** e **Palavra-passe**. Vai utilizá-los mais à frente no tutorial. 

## <a name="create-a-function-project"></a>Criar um projeto de função
Os passos seguintes mostram-lhe como criar uma função do IoT Edge com o Visual Studio Code e a extensão Azure IoT Edge.
1. Abra o Visual Studio Code.
2. Para abrir o terminal integrado do VS Code, selecione **View** (Ver) > **Integrated Terminal** (Terminal Integrado).
3. Para instalar (ou atualizar) o modelo **AzureIoTEdgeFunction** em dotnet, execute o comando seguinte no terminal integrado:

    ```cmd/sh
    dotnet new -i Microsoft.Azure.IoT.Edge.Function
    ```
2. Crie um projeto para o módulo novo. O comando seguinte cria a pasta do projeto, **FilterFunction**, com o repositório do contentor. Se estiver a utilizar um registo do Azure Container Registry, o segundo parâmetro deve ter o formato `<your container registry name>.azurecr.io`. Introduza o seguinte comando na pasta de trabalho atual:

    ```cmd/sh
    dotnet new aziotedgefunction -n FilterFunction -r <your container registry address>/filterfunction
    ```

3. Selecione **File** (Ficheiro) > **Open File** (Abrir Pasta), procure a pasta **FilterFunction** e abra o projeto no VS Code.
4. No explorador do VS Code, expanda a pasta **EdgeHubTrigger-Csharp** e abra o ficheiro **run.csx**.
5. Substitua os conteúdos do ficheiro pelo seguinte código:

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
                    filteredMessage.Properties.Add(prop.Key, prop.Value);                }
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

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Criar uma imagem do Docker e publicá-la no seu registo

1. Introduza o comando seguinte no terminal integrado do VS Code e inicie sessão no Docker. 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Para localizar o nome de utilizador, a palavra-passe e o servidor de início de sessão para utilizar neste comando, aceda ao [Portal do Azure] (https://portal.azure.com). Em **Todos os recursos**, clique no mosaico relativo ao seu registo do Azure Container Registry para abrir as respetivas propriedades e clique em **Chaves de acesso**. Copie os valores existentes nos campos **Nome e utilizador**, **Palavra-passe** e **Servidor de início de sessão**. 

2. Abra o **module.json**. Opcionalmente, pode atualizar o `"version"` para, por ex. **"1.0"**. O nome do repositório é também mostrado como introduziu no parâmetro `-r` de `dotnet new aziotedgefunction`.

3. Guarde o ficheiro **module.json**.

4. No explorador do VS Code, clique com botão direito do rato no ficheiro **module.json** e clique em **Compilar e Emitir imagem de Docker do módulo IoT Edge**. Na caixa pendente de pop-up na parte superior da janela do VS Code, selecione a plataforma do seu contentor, **amd64** para contentor do Linux ou **windows-amd64** para contentor do Windows. Em seguida, o VS Code contentoriza os códigos de função e envia-os para o registo de contentores que tiver especificado.

5. Pode obter o endereço da imagem de contentor completo com a etiqueta no terminal integrado do VS Code. Para obter mais informações sobre a definição de compilação e emissão, pode ver o ficheiro `module.json`.

## <a name="add-registry-credentials-to-your-edge-device"></a>Adicionar as credenciais do registo ao seu dispositivo Edge
Adicione as credenciais do registo ao runtime do Edge no computador onde está a executar o dispositivo Edge. Desta forma, o runtime tem acesso para extrair o contentor. 

- No Windows, execute o seguinte comando:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- No Linux, execute o seguinte comando:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>Executar a solução

1. No **Portal do Azure**, navegue para o seu hub IoT.
2. Aceda a **IoT Edge (pré-visualização)** e selecione o seu dispositivo IoT Edge.
1. Selecione **Definir Módulos**. 
2. Se já tiver implementado o módulo **tempSensor** neste dispositivo, aquele poderá ser preenchido automaticamente. Caso contrário, siga estes passos para adicioná-lo:
    1. Selecione **Adicionar módulo do IoT Edge**.
    2. No campo **Nome**, introduza `tempSensor`.
    3. No campo **URI da Imagem**, introduza `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Deixe as outras definições inalteradas e clique em **Guardar**.
1. Adicione o módulo **filterFunction**.
    1. Selecione novamente **Adicionar módulo do IoT Edge**.
    2. No campo **Nome**, introduza `filterFunction`.
    3. No campo **URI da Imagem**, introduza o endereço da sua imagem, como, por exemplo, `<your container registry address>/filterfunction:0.0.1-amd64`. O endereço completo da imagem está disponível na secção anterior.
    74. Clique em **Guardar**.
2. Clique em **Seguinte**.
3. No passo **Especificar Rotas**, copie o JSON abaixo para a caixa de texto. A primeira rota transporta mensagens do sensor de temperatura para o módulo de filtro através do ponto final "input1". A segunda rota transporta mensagens do módulo de filtro para o Hub IoT. Nesta rota, `$upstream` é um destino especial que diz ao Hub Edge para enviar mensagens para o Hub IoT. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterFunction/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterFunction/outputs/* INTO $upstream"
       }
    }
    ```

4. Clique em **Seguinte**.
5. No passo **Rever Modelo**, clique em **Submeter**. 
6. Regresse à página de detalhes do dispositivo IoT Edge e clique em **Atualizar**. Deverá ver o módulo **filterfunction** novo em execução, juntamente com o módulo **tempSensor** e o **runtime do IoT Edge**. 

## <a name="view-generated-data"></a>Ver os dados gerados

Para monitorizar as mensagens do dispositivo para a cloud enviadas a partir do dispositivo IoT Edge para o hub IoT:
1. Configure a extensão Toolkit do Azure IoT com a cadeia de ligação do seu hub IoT: 
    1. No Portal do Azure, navegue para o hub IoT e selecione **Políticas de acesso partilhado**. 
    2. Selecione **iothubowner** e copie o valor de **Cadeia de ligação - chave primária**.
    3. No explorador do VS Code, clique em **IOT HUB DEVICES** (DISPOSITIVOS IOT HUB) e clique em **...**. 
    4. Selecione **Set IoT Hub Connection String** (Definir Cadeia de Ligação do Hub IoT) e introduza a cadeia de ligação do Hub IoT na janela de pop-up. 

2. Para monitorizar os dados que chegam ao hub IoT, selecione **View** > **Command Palette...** e procure **IoT: Start monitoring D2C message** (IoT: Começar a monitorizar mensagens D2C). 
3. Para parar a monitorização de dados, utilize o comando **IoT: Stop monitoring D2C message** (IoT: Parar a monitorização de mensagens D2C) na paleta de comandos. 

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma função das Funções Azure que contém código para filtrar dados não processados gerados pelo seu dispositivo IoT Edge. Para continuar a explorar o Azure IoT Edge, saiba como utilizar um dispositivo IoT Edge como gateway. 

> [!div class="nextstepaction"]
> [Create an IoT Edge gateway device](how-to-create-transparent-gateway.md) (Criar um dispositivo de gateway IoT Edge)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
