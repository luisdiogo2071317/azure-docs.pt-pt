---
title: Implementar as Funções do Azure com o Azure IoT Edge | Microsoft Docs
description: Implementar as Funções do Azure como módulo para um dispositivo de periferia
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/26/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0445817f9ff403156025e38a1e14a3892a9a292b
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37031197"
---
# <a name="tutorial-deploy-azure-functions-as-iot-edge-modules---preview"></a>Tutorial: Implementar as Funções do Azure como módulos do IoT Edge – pré-visualização

Pode utilizar as Funções do Azure para implementar código que aplica a sua lógica de negócio diretamente nos seus dispositivos IoT Edge. Este tutorial orienta-o ao longo da criação e implementação de uma função das Funções do Azure que filtra dados de sensores no dispositivo IoT Edge simulado que criou no tutorial “Deploy Azure IoT Edge on a simulated device” (“Implementar o Azure IoT Edge num dispositivo simulado”) para[Windows][lnk-tutorial1-win] ou [Linux][lnk-tutorial1-lin]. Neste tutorial, ficará a saber como:     

> [!div class="checklist"]
> * Utilize o Visual Studio Code para criar uma função do Azure
> * Utilizar o VS Code e o Docker para criar uma imagem do Docker e publicá-la num registo de contentor 
> * Implementar o módulo a partir do registo de contentor no seu dispositivo IoT Edge
> * Ver dados filtrados

>[!NOTE]
>Os módulos Funções do Azure no Azure IoT Edge estão em [pré-visualização](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) pública. 

A função das Funções do Azure que criar neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo e só envia mensagens para o Hub IoT do Azure quando a temperatura for superior a um limiar especificado. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para testar o módulo Funções que vai criar neste tutorial, precisa de um dispositivo IoT Edge. Pode utilizar o dispositivo que configurou no início rápido para [Linux](quickstart-linux.md) ou [Windows](quickstart.md).

Tenha os seguintes pré-requisitos no computador de desenvolvimento: 
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão C# para Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) para Visual Studio Code.
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/) instalado no seu computador de desenvolvimento. 

## <a name="create-a-container-registry"></a>Criar um registo de contentores
Neste tutorial, vai utilizar a extensão Azure IoT Edge para VS Code para criar um módulo e criar uma **imagem de contentor** a partir dos ficheiros. Em seguida, vai enviar essa imagem para um **registo** que armazena e gere as suas imagens. Por fim, vai implementar a imagem a partir do registo para ser executada no seu dispositivo IoT Edge.  

Pode utilizar qualquer registo compatível com o Docker neste tutorial. O [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) e o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags) são dois populares serviços de registo do Docker que estão disponíveis na cloud. Este tutorial utiliza o Azure Container Registry. 

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contentores** > **Registo de Contentor**.

    ![Criar registo de contentor](./media/tutorial-deploy-function/create-container-registry.png)

2. Atribua um nome ao registo e selecione uma subscrição.
3. Para o grupo de recursos, é recomendado que utilize o mesmo nome do grupo de recursos que contém o seu Hub IoT. Ao manter todos os recursos no mesmo grupo, pode geri-los em conjunto. Por exemplo, eliminar o grupo de recursos utilizado para teste elimina todos os recursos de teste que se encontram no grupo. 
4. Defina o SKU como **Básico** e **Utilizador administrador** como **Ativar**. 
5. Clique em **Criar**.
6. Assim que o registo de contentores for criado, navegue para o mesmo e selecione **Chaves de acesso**. 
7. Copie os valores de **Servidor de início de sessão**, **Nome de utilizador** e **Palavra-passe**. Vai utilizá-los mais à frente no tutorial. 

## <a name="create-a-function-project"></a>Criar um projeto de função
Os passos seguintes mostram-lhe como criar uma função do IoT Edge com o Visual Studio Code e a extensão Azure IoT Edge.

1. Abra o Visual Studio Code.
2. Abra o terminal integrado do VS Code ao selecionar **Ver** > **Terminal Integrado**. 
2. Abra a paleta de comandos do VS Code ao selecionar **Ver** > **Paleta de Comandos**.
3. Na paleta de comandos, escreva e execute o comando **Azure: Iniciar sessão** e siga as instruções para iniciar sessão na sua conta do Azure. Se já iniciou sessão, pode ignorar este passo.
3. Na paleta de comandos, escreva e execute o comando **Azure IoT Edge: Nova solução do IoT Edge**. Na paleta de comandos, indique as seguintes informações para criar a sua solução: 

   1. Selecione a pasta onde quer criar a solução. 
   2. Forneça um nome para a sua solução ou aceite a predefinição **EdgeSolution**.
   3. Selecione **Funções do Azure - C#** como modelo de módulo. 
   4. Atribua o nome **CSharpFunction** ao módulo. 
   5. Especifique o Azure Container Registry que criou na secção anterior como repositório de imagens para o primeiro módulo. Substitua **localhost:5000** pelo valor do servidor de início de sessão que copiou. A cadeia final é semelhante a **\<nome do registo\>.azurecr.io/csharpfunction**.

4. A janela do VS Code carrega a área de trabalho da solução do IoT Edge. Existe uma pasta **.vscode**, uma pasta **modules** ficheiro de modelo do manifesto de implementação e um ficheiro **.env**. Abra **modules** > **CSharpFunction** > **EdgeHubTrigger-Csharp** > **run.csx**.

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
        public Machine machine {get; set;}
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

6. Guarde o ficheiro.

## <a name="build-your-iot-edge-solution"></a>Criar a sua solução do IoT Edge

Na secção anterior, criou uma solução do IoT Edge e adicionou código ao CSharpFunction que filtrará mensagens onde a temperatura comunicada da máquina é inferior ao limiar aceitável. Agora, tem de criar a solução como uma imagem de contentor e enviá-la para o registo de contentor.

1. Inicie sessão no Docker ao introduzir o comando seguinte no terminal integrado do Visual Studio Code, para que possa enviar a imagem do módulo para o ACR: 
     
    ```csh/sh
    docker login -u <ACR username> <ACR login server>
    ```
    Utilize o nome do utilizador e o servidor de início de sessão que copiou do Azure Container Registry anteriormente. Ser-lhe-á solicitada a palavra-passe. Cole a palavra-passe na linha de comandos e prima **Enter**.

    ```csh/sh
    Password: <paste in the ACR password and press enter>
    Login Succeeded
    ```

2. No explorador do VS Code, abra o ficheiro **deployment.template.json** na área de trabalho da solução do IoT Edge. Este ficheiro indica ao runtime do IoT Edge que módulos deve implementar num dispositivo. Para saber mais sobre os manifestos de implementação, veja [Compreender como os módulos do IoT Edge podem ser utilizados, configurados e reutilizados](module-composition.md).

3. Localize a secção **registryCredentials** no manifesto de implementação. Atualize o **nome de utilizador**, a **palavra-passe** e o **endereço** com as credenciais do seu registo de contentor. Esta secção dá ao runtime do IoT Edge no seu dispositivo permissão para extrair as imagens de contentor armazenadas no seu registo privado. Os pares de nome de utilizador e palavra-passe reais são armazenados no ficheiro .env, ignorado pelo git.

5. Guarde este ficheiro.

6. No explorador do VS Code, clique com o botão direito do rato no ficheiro **deployment.template.json** e selecione **Criar solução do IoT Edge**. 

Quando indicar ao Visual Studio Code para criar a solução, este utiliza primeiro as informações no modelo de implementação e gera um ficheiro `deployment.json` numa nova pasta **config**. Em seguida, executa dois comandos no terminal integrado: `docker build` e `docker push`. Estes dois comandos criam o código, colocam as funções em contentores e enviam-no para o registo de contentor que especificou quando inicializou a solução. 

## <a name="view-your-container-image"></a>Ver a imagem de contentor

O Visual Studio Code apresenta uma mensagem de êxito quando a imagem de contentor é enviada para o seu registo de contentor. Se quiser confirmar a operação bem-sucedida, pode ver a imagem no registo. 

1. No portal do Azure, navegue para o Azure Container Registry. 
2. Selecione **Repositórios**.
3. Deverá ver **csharpfunction** listado nos repositórios. Selecione o repositório para ver mais detalhes.
4. Na secção **Etiquetas**, deverá ver **0.0.1-amd64**. Esta etiqueta reflete a versão e a plataforma da imagem criada. Estes valores estão definidos no ficheiro **module.json** na pasta CSharpFunction. 

## <a name="deploy-and-run-the-solution"></a>Implementar e executar a solução

Pode utilizar o portal do Azure para implementar o seu módulo Funções para um dispositivo IoT Edge, tal como fez nos inícios rápidos, mas também pode implementar e monitorizar os módulos a partir do Visual Studio Code. As secções seguintes utilizam a extensão do Azure IoT Edge para o VS Code, que foi listado nos pré-requisitos. Instale-a agora, se ainda não o fez. 

1. Abra a paleta de comandos do VS Code ao selecionar **Ver** > **Paleta de Comandos**.

2. Procure e execute o comando **Azure: Iniciar sessão**. Siga as instruções para iniciar sessão na conta do Azure. 

3. Na paleta de comandos, procure e execute o comando **Hub IoT do Azure: Selecionar Hub IoT**. 

4. Selecione a subscrição que contém o seu hub IoT e, em seguida, selecione o hub IoT ao qual quer aceder.

5. No explorador do VS Code, expanda a secção **Dispositivos do Hub IoT do Azure**. 

6. Carregue com o botão direito do rato no nome do seu dispositivo do IoT Edge e, em seguida, selecione **Criar Implementação para o dispositivo do IoT Edge**. 

7. Navegue para a pasta de solução que contém CSharpFunction. Abra a pasta **config** e selecione o ficheiro **deployment.json**. Clique em **Selecionar Manifesto de Implementação do Edge**.

8. Atualize a secção **Dispositivos do Hub IoT do Azure**. Deverá ver o novo **CSharpFunction** em execução, juntamente com o módulo **TempSensor**, bem como **$edgeAgent** e **$edgeHub**. 

   ![Ver os módulos implementados no VS Code](./media/tutorial-deploy-function/view-modules.png)

## <a name="view-generated-data"></a>Ver os dados gerados

Pode ver todas as mensagens que chegam ao seu hub IoT ao executar **Azure IoT Hub: Start Monitoring D2C Message** na paleta de comandos.

Também pode filtrar para ver todas as mensagens que chegam ao seu hub IoT a partir de um dispositivo específico. Clique com o botão direito do rato na secção **Dispositivos do Hub IoT do Azure** e selecione **Iniciar Monitorização de Mensagens D2C**.

Para parar a monitorização de mensagens, execute o comando **Azure IoT Hub: Stop monitoring D2C message** na paleta de comandos. 


## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)]

Remova o runtime do serviço IoT Edge com base na sua plataforma de dispositivos IoT (Linux ou Windows).

#### <a name="windows"></a>Windows

Remova o runtime do IoT Edge.

```Powershell
stop-service iotedge -NoWait
sleep 5
sc.exe delete iotedge
```

Elimine os contentores criados no seu dispositivo. 

```Powershell
docker rm -f $(docker ps -a --no-trunc --filter "name=edge" --filter "name=tempSensor" --filter "name=CSharpFunction")
```

#### <a name="linux"></a>Linux

Remova o runtime do IoT Edge.

```bash
sudo apt-get remove --purge iotedge
```

Elimine os contentores criados no seu dispositivo. 

```bash
sudo docker rm -f $(sudo docker ps -a --no-trunc --filter "name=edge" --filter "name=tempSensor" --filter "name=CSharpFunction")
```

Remova o runtime de contentor.

```bash
sudo apt-get remove --purge moby
```



## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou um módulo das Funções do Azure que contém código para filtrar dados não processados gerados pelo seu dispositivo IoT Edge. Quando estiver pronto para criar os seus próprios módulos, pode saber mais sobre como [Desenvolver Funções do Azure com o Azure IoT Edge para Visual Studio Code](how-to-develop-csharp-function.md). 

Continue para os próximos tutoriais para saber mais sobre outras formas em que o Azure IoT Edge o pode ajudar a transformar os dados em informações empresariais na periferia.

> [!div class="nextstepaction"]
> [Localizar médias com um período flutuante no Azure Stream Analytics](tutorial-deploy-stream-analytics.md)

<!--Links-->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
