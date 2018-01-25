---
title: "Utilizar o Visual Studio Code para desenvolver e implementar as funções do Azure para o limite do Azure IoT | Microsoft Docs"
description: "Desenvolver e implementar uma c# das funções do Azure com o Azure IoT Edge no VS Code sem mudar de contexto"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 219474a4577a76f5ceb9a9efaa3c349d633de047
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/24/2018
---
# <a name="use-visual-studio-code-to-develop-and-deploy-azure-functions-to-azure-iot-edge"></a>Utilizar o Visual Studio Code para desenvolver e implementar as funções do Azure para o limite de IoT do Azure

Este artigo fornece instruções detalhadas para utilizar [Visual Studio Code](https://code.visualstudio.com/) como a ferramenta de desenvolvimento principal para desenvolver e implementar as funções do Azure no limite de IoT. 

## <a name="prerequisites"></a>Pré-requisitos
Este tutorial parte do princípio de que está a utilizar um computador ou máquina virtual com o Windows ou Linux como computador de desenvolvimento. O dispositivo de limite de IoT ser outro dispositivo físico ou pode simular o seu dispositivo de limite de IoT no computador de desenvolvimento.

Certifique-se de que ter tutoriais seguintes concluídas antes de começar esta orientação.
- Implementar o Azure IoT Edge num dispositivo simulado [Windows](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-windows) ou [Linux](https://docs.microsoft.com/azure/iot-edge/tutorial-simulate-device-linux)
- [Implementar as Funções do Azure](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-function)

Eis uma lista de verificação que mostra os itens que deve ter depois de concluir os tutoriais anteriores.

- [Visual Studio Code](https://code.visualstudio.com/). 
- [Extensão de limite de IoT do Azure para Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
- [C# para a extensão do Visual Studio Code (com tecnologia OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp). 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd). 
- [Python 2.7](https://www.python.org/downloads/)
- [Script de controlo contorno de IoT](https://pypi.python.org/pypi/azure-iot-edge-runtime-ctl)
- Modelo de AzureIoTEdgeFunction (`dotnet new -i Microsoft.Azure.IoT.Edge.Function`)
- Um hub IoT Active Directory com, pelo menos, um dispositivo de limite de IoT.

-Também é sugerida para instalar [suporte de Docker para o VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para melhor gerir imagens do módulo e contentores.

> [!NOTE]
> Atualmente, as funções do Azure no limite de IoT só suporta c#.

## <a name="deploy-azure-iot-functions-in-vs-code"></a>Implementar funções do Azure IoT no VS Code
Tutorial [implementar funções do Azure](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-function), atualizar, criar e publicar as imagens de módulo de função no VS Code e, em seguida, visitar o portal do Azure para implementar as funções do Azure. Esta secção apresenta como utilizar o VS Code para implementar e monitorizar as suas funções do Azure.

### <a name="start-a-local-docker-registry"></a>Iniciar um registo de local docker
Pode utilizar qualquer registo compatível com o Docker para este tutorial. Dois populares Docker registo serviços disponíveis na nuvem são [registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) e [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Esta secção utiliza um [registo Docker local](https://docs.docker.com/registry/deploying/), que é mais fácil para fins de teste durante o desenvolvimento antecipado.
No VS Code **terminal integrada**(Ctrl + '), execute os seguintes comandos para iniciar um registo local.  

```cmd/sh
docker run -d -p 5000:5000 --name registry registry:2 
```

> [!NOTE]
> Acima exemplo mostra as configurações de registo que apenas são adequadas para teste. Um registo de prontos para produção deve ser protegido de TLS e Idealmente, deve utilizar um mecanismo de controlo de acesso. Recomendamos que utilize [registo de contentor do Azure](https://docs.microsoft.com/azure/container-registry/) ou [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) para implementar os módulos de limite de IoT prontos para produção.

### <a name="create-a-function-project"></a>Criar um projeto de função
A mostrar os passos seguintes, como criar um módulo de limite de IoT com base no .NET core 2.0 utilizando o Visual Studio Code e a extensão de limite de IoT do Azure. Se tiver concluído esta secção tutorial anterior, pode ignorar com segurança nesta secção.

1. No Visual Studio Code, selecione **vista** > **integrada Terminal** para abrir o terminal integrado VS Code.
2. Para instalar (ou atualizar) as **AzureIoTEdgeFunction** modelo no dotnet, execute o seguinte comando no terminal integrado:

   ```cmd/sh
   dotnet new -i Microsoft.Azure.IoT.Edge.Function
   ```
3. Crie um projeto para o módulo de novo. O comando seguinte cria a pasta do projeto, **FilterFunction**, na pasta de trabalho atual:

   ```cmd/sh
   dotnet new aziotedgefunction -n FilterFunction
   ```
 
4. Selecione **ficheiro > Abrir pasta**, em seguida, procure o **FilterFunction** pasta e abra o projeto no VS Code.
5. Navegue para o **FilterFunction** pasta e clique em **selecionar pasta** para abrir o projeto no VS Code.
6. No Explorador de VS Code, expanda o **EdgeHubTrigger Csharp** pasta, em seguida, abra o **run.csx** ficheiro.
7. Substitua o conteúdo do ficheiro com o seguinte código:

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

8. Guarde o ficheiro.

### <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Criar uma imagem de Docker e publicá-lo no seu registo

1. No Explorador de VS Code, expanda o **Docker** pasta. Em seguida, expanda a pasta para a sua plataforma de contentor, quer **linux x64** ou **windows nano**.
2. Clique com botão direito do **Dockerfile** do ficheiro e clique em **imagem de Docker do módulo de limite de IoT criar**. 

    ![Compilar a imagem do docker](./media/how-to-vscode-develop-csharp-function/build-docker-image.png)

3. Navegue para o **FilterFunction** pasta do projeto e clique em **selecionar pasta como EXE_DIR**. 
4. Na caixa de texto de pop-up na parte superior da janela VS Code, introduza o nome da imagem. Por exemplo: `<your container registry address>/filterfunction:latest`. Se estiver a implementar no registo local, deve ser `localhost:5000/filterfunction:latest`.
5. A imagem de emissão para o seu repositório de Docker. Utilize o **Edge: imagem de Docker do módulo de limite de Push de IoT** de comandos e introduza o URL da imagem na caixa de texto de pop-up na parte superior da janela VS Code. Utilize o mesmo URL de imagem utilizada nos passos acima.
    ![Imagem de docker Push](./media/how-to-vscode-develop-csharp-function/push-image.png)

### <a name="deploy-your-function-to-iot-edge"></a>Implementar a função IoT Edge

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
   "filterfunction": {
      "version": "1.0",
      "type": "docker",
      "status": "running",
      "restartPolicy": "always",
      "settings": {
         "image": "localhost:5000/filterfunction:latest",
         "createOptions": ""
      }
   }
   ```

2. Substitua o **rotas** secção com abaixo conteúdo:
   ```json
       "routes":{
           "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterfunction/inputs/input1\")",
           "filterToIoTHub":"FROM /messages/modules/filterfunction/outputs/* INTO $upstream"
       }
   ```
   > [!NOTE]
   > As regras de declarativas em tempo de execução definem onde as mensagens de fluxo. Neste tutorial, precisa de dois rotas. A rota primeiro transportes mensagens do sensor de temperatura para a função de filtro através de ponto final "input1", que é o ponto final que configurou com o processador de FilterMessages. A segunda rota transportes mensagens na função de filtro ao IoT Hub. Esta rota montante é um destino especial que diz ao Hub Edge para enviar mensagens para o IoT Hub.

3. Guarde este ficheiro.
4. Na paleta do comando, selecione **Edge: criar a implementação para o dispositivo de limite**. Em seguida, selecione o ID do dispositivo IoT limite para criar uma implementação. Ou o ID de dispositivo na lista de dispositivos com o botão direito e selecione **criar a implementação para o dispositivo de limite**.

    ![Criar implementação](./media/how-to-vscode-develop-csharp-function/create-deployment.png)

5. Selecione o `deployment.json` -lo. Na janela de resultados, pode ver saídas correspondentes para a sua implementação.
6. Inicie o tempo de execução do limite na paleta do comando. **Limite: Limite de início**
7. Pode ver o tempo de execução de limite de IoT iniciar a execução no Explorador do Docker com a função de sensor e filtrar simulada.

    ![Solução em execução](./media/how-to-vscode-develop-csharp-function/solution-running.png)

8. O ID do dispositivo de limite com o botão direito e pode monitorizar as mensagens de D2C no VS Code.

    ![Mensagens de monitor](./media/how-to-vscode-develop-csharp-function/monitor-d2c-messages.png)


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criou uma função do Azure no limite de IoT e implementado no dispositivo de limite de IoT no VS Code. Pode continuar a sessão em qualquer um dos seguintes tutoriais para saber mais sobre outros cenários quando desenvolver Azure IoT Edge no VS Code.

> [!div class="nextstepaction"]
> [Depurar as funções do Azure no VS Code](how-to-vscode-debug-azure-function.md)
