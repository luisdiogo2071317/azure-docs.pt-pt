---
title: Tutorial C# do Azure IoT Edge | Microsoft Docs
description: Este tutorial mostra como criar um módulo do IoT Edge com código C# e implementá-lo num dispositivo Edge
services: iot-edge
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/27/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 8cd4f87e690d64e1f69ef39f3422ebc73d79974a
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37053097"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Tutorial: Desenvolver um módulo do IoT Edge do C# e implementar no seu dispositivo simulado

Pode utilizar os módulos do IoT Edge para implementar código que aplica a sua lógica de negócio diretamente nos seus dispositivos IoT Edge. Este tutorial explica-lhe como criar e implementar um módulo do IoT Edge que filtra dados de sensores. Vai utilizar o dispositivo IoT Edge simulado que criou na Implementação do Azure IoT Edge num dispositivo simulado em tutoriais do [Windows][lnk-tutorial1-win] ou do [Linux][lnk-tutorial1-lin]. Neste tutorial, ficará a saber como:    

> [!div class="checklist"]
> * Utilize o Visual Studio Code para criar um módulo do IoT Edge com base no .NET core 2.0
> * Utilizar o Visual Studio Code e o Docker para criar uma imagem do Docker e publicá-la no seu registo 
> * Implementar o módulo no seu dispositivo IoT Edge
> * Ver os dados gerados


O módulo do IoT Edge que criou neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo. Envia apenas mensagens de origem caso a temperatura seja superior a um limiar especificado. Este tipo de análise no Edge é útil para reduzir a quantidade de dados comunicados e armazenados na cloud. 

Se não tiver uma subscrição do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

* O dispositivo Azure IoT Edge que criou no início rápido para dispositivos [Linux](quickstart-linux.md) ou [Windows](quickstart.md).
* A cadeia de ligação da chave primária do dispositivo IoT Edge.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão C# para Visual Studio Code (com tecnologia da OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) para Visual Studio Code.
* [Extensão Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code. 
* [SDK de .NET Core 2.1](https://www.microsoft.com/net/download).
* [Docker CE](https://docs.docker.com/install/) instalado no seu computador de desenvolvimento. 


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
Os passos seguintes mostram-lhe como criar um projeto do módulo do IoT Edge baseado no .Net core 2.0 com o Visual Studio Code e a extensão Azure IoT Edge.
1. No Visual Studio Code, selecione **Vista** > **Terminal Integrado** para abrir o terminal integrado do VS Code.
2. Selecione **Ver** > **Paleta de Comandos** para abrir a paleta de comandos do VS Code. 
3. Na paleta de comandos, escreva e execute o comando **Azure: Sign in** e siga as instruções para iniciar sessão na sua conta do Azure. Se já iniciou sessão, pode ignorar este passo.
4. Na paleta de comandos, escreva e execute o comando **Azure IoT Edge: New IoT Edge solution**. Na paleta de comandos, indique as seguintes informações para criar a sua solução: 
   1. Selecione a pasta onde quer criar a solução. 
   2. Indique um nome para a sua solução ou aceite a predefinição **EdgeSolution**.
   3. Selecione **Módulo C#** como modelo de módulo. 
   4. Atribua o nome **CSharpModule** ao módulo. 
   5. Especifique o Azure Container Registry que criou na secção anterior como repositório de imagens para o primeiro módulo. Substitua **localhost:5000** pelo valor do servidor de início de sessão que copiou. A cadeia final é semelhante a **\<nome do registo\>.azurecr.io/csharpmodule**.
 
4. A janela do VS Code carrega a área de trabalho da solução do IoT Edge. Existe uma pasta **modules**, uma pasta **.vscode**, um ficheiro de modelo do manifesto de implementação e um ficheiro **.env**. Abra **modules** > **CSharpModule** > **Program.cs**.

5. Na parte superior do espaço de nomes **CSharpModule**, adicione três declarações `using` para tipos utilizados mais tarde:

    ```csharp
    using System.Collections.Generic;     // for KeyValuePair<>
    using Microsoft.Azure.Devices.Shared; // for TwinCollection
    using Newtonsoft.Json;                // for JsonConvert
    ```

6. Adicione a variável `temperatureThreshold` à classe **Programa**. Esta variável define o valor que a temperatura medida deve exceder, para que os dados sejam enviados para o Hub IoT. 

    ```csharp
    static int temperatureThreshold { get; set; } = 25;
    ```

7. Adicione as classes `MessageBody`, `Machine` e `Ambient` à classe **Programa**. Estas classes definem o esquema esperado para o corpo das mensagens a receber.

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

8. No método **Init**, o código cria e configura um objeto **ModuleClient**. Este objeto permite que o módulo se ligue ao runtime do Azure IoT Edge local para enviar e receber mensagens. A cadeia de ligação utilizada no método **Init** é dada ao módulo através do runtime do IoT Edge. Depois de criar o **ModuleClient**, o código lê o TemperatureThreshold a partir das propriedades pretendidas do Módulo Duplo e regista uma chamada de retorno para receber mensagens do hub do IoT Edge através do ponto final **input1**. Substitua o método `SetInputMessageHandlerAsync` por um novo e adicione um método `SetDesiredPropertyUpdateCallbackAsync`, para obter as atualizações de propriedades pretendidas. Para fazer esta alteração, substitua a última linha do método **Init** pelo seguinte código:

    ```csharp
    // Register callback to be called when a message is received by the module
    // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

    // Read TemperatureThreshold from Module Twin Desired Properties
    var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
    var moduleTwinCollection = moduleTwin.Properties.Desired;
    try {
        temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
    } catch(ArgumentOutOfRangeException e) {
        Console.WriteLine($"Property TemperatureThreshold not exist: {e.Message}"); 
    }

    // Attach callback for Twin desired properties updates
    await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

    // Register callback to be called when a message is received by the module
    await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
    ```

9. Adicione o método `onDesiredPropertiesUpdate` à classe **Programa**. Este método recebe atualizações nas propriedades pretendidas do módulo duplo e atualiza a variável **temperatureThreshold** para corresponder. Todos os módulos têm o seu próprio módulo duplo, que lhe permite configurar o código em execução no interior de um módulo diretamente a partir da cloud.

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

10. Substitua o método `PipeMessage` pelo método `FilterMessages`. Este método é chamado sempre que o módulo recebe uma mensagem do hub do IoT Edge. Ele filtra as mensagens que comunicam temperaturas inferiores ao limiar de temperatura definido através do módulo duplo. Também adiciona a propriedade **MessageType** à mensagem com o valor definido para **Alerta**. 

    ```csharp
    static async Task<MessageResponse> FilterMessages(Message message, object userContext)
    {
        var counterValue = Interlocked.Increment(ref counter);
        try
        {
            ModuleClient moduleClient = (ModuleClient)userContext;
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
                await moduleClient.SendEventAsync("output1", filteredMessage);
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
            var moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error in sample: {0}", ex.Message);
            // Indicate that the message treatment is not completed
            ModuleClient moduleClient = (ModuleClient)userContext;
            return MessageResponse.Abandoned;
        }
    }
    ```

11. Guarde este ficheiro.

## <a name="build-your-iot-edge-solution"></a>Criar a sua solução do IoT Edge

Na secção anterior, criou uma solução do IoT Edge e adicionou código ao CSharpModule que filtrará mensagens onde a temperatura comunicada da máquina é inferior ao limiar aceitável. Agora, tem de criar a solução como uma imagem de contentor e enviá-la para o registo de contentor. 

1. Inicie sessão no Docker ao introduzir o comando seguinte no terminal integrado do Visual Studio Code, para que possa enviar a imagem do módulo para o ACR: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Utilize o nome do utilizador, palavra-passe e servidor de início de sessão que copiou do Azure Container Registry na primeira secção. Também pode obtê-los novamente a partir da secção **Chaves de acesso** do registo no portal do Azure.

2. No explorador do VS Code, abra o ficheiro **deployment.template.json** na área de trabalho da solução do IoT Edge. Este ficheiro indica ao `$edgeAgent` para implementar dois módulos: **tempSensor** e **CSharpModule**. O valor `CSharpModule.image` está definido para uma versão Linux amd64 da imagem. Para saber mais sobre os manifestos de implementação, veja [Compreender como os módulos do IoT Edge podem ser utilizados, configurados e reutilizados](module-composition.md).

3. No ficheiro **deployment.template.json**, existe uma secção **registryCredentials** que armazena as credenciais de registo do Docker. Os pares de nome de utilizador e palavra-passe reais são armazenados no ficheiro .env, ignorado pelo git.

4. Adicione o módulo duplo CSharpModule ao manifesto de implementação. Insira o seguinte conteúdo JSON na parte inferior da secção `moduleContent`, após o módulo duplo `$edgeHub`: 
    ```json
        "CSharpModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Guarde este ficheiro.
5. No explorador do VS Code, clique com o botão direito do rato no ficheiro **deployment.template.json** e selecione **Criar solução do IoT Edge**. 

Quando indicar ao Visual Studio Code para criar a solução, este utiliza primeiro as informações no modelo de implementação e gera um ficheiro `deployment.json` numa nova pasta **config**. Em seguida, executa dois comandos no terminal integrado: `docker build` e `docker push`. Estes dois comandos criam o código, colocam `CSharpModule.dll` em contentores e enviam-no para o registo de contentor que especificou quando inicializou a solução. 

Pode ver o endereço da imagem de contentor completo com a etiqueta no terminal integrado do VS Code. O endereço da imagem baseia-se nas informações no ficheiro `module.json`, com o formato **\<repositório\>:\<versão\>-\<plataforma\>**. Para este tutorial, deve ser semelhante a **registryname.azurecr.io/csharpmodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>Implementar e executar a solução

1. Configure a extensão Toolkit do Azure IoT com a cadeia de ligação do seu hub IoT: 
    1. Abra o explorador do VS Code ao selecionar **Vista** > **Explorador**. 
    2. No explorador, clique em **DISPOSITIVOS DO HUB IOT DO AZURE** e clique em **...**. Clique em **Selecionar Hub IoT**. Siga as instruções para iniciar sessão na sua conta do Azure e selecione o seu hub IoT. 
       Tenha em atenção que também pode configurar ao clicar em **Definir Cadeia de Ligação do Hub IoT**. Introduza a cadeia de ligação para o hub IoT ao qual o seu dispositivo IoT Edge se liga na janela de pop-up.

2. No explorador de dispositivos do Hub IoT do Azure, clique com botão direito do rato no seu dispositivo IoT Edge e, em seguida, clique em **Criar Implementação para o dispositivo IoT Edge**. Selecione o ficheiro **deployment.json** na pasta **config** e, em seguida, clique em **Selecionar Manifesto de Implementação do Edge**.

3. Clique no botão Atualizar. Deverá ver o novo **CSharpModule** em execução, juntamente com o módulo **TempSensor**, bem como **$edgeAgent** e **$edgeHub**. 

## <a name="view-generated-data"></a>Ver os dados gerados

1. Para monitorizar os dados que chegam ao hub IoT, clique em **...** e selecione **Iniciar Monitorização de Mensagens D2C**.
2. Para monitorizar a mensagem D2C para um dispositivo específico, clique com o botão direito do rato na lista e selecione **Iniciar Monitorização de Mensagens D2C**.
3. Para parar a monitorização de dados, execute o comando **Azure IoT Hub: Stop monitoring D2C message** na paleta de comandos. 
4. Para ver ou atualizar o módulo duplo, clique com o botão direito do rato no módulo na lista e selecione **Editar módulo duplo**. Para atualizar o módulo duplo, guarde o ficheiro JSON duplo e clique com o botão direito do rato na área de editor e selecione **Atualizar Módulo Duplo**.
5. Para ver os registos do Docker, pode instalar o [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para o VS Code e localizar os módulos em execução localmente no explorador do Docker. No menu de contexto, clique em **Mostrar Registos** para ver no terminal integrado.
 
## <a name="clean-up-resources"></a>Limpar recursos 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Se avançar para o próximo artigo recomendado, pode manter os recursos e as configurações que já criou e reutilizá-los.

Caso contrário, pode eliminar as configurações locais e os recursos do Azure criados neste artigo para evitar custos. 

> [!IMPORTANT]
> A eliminação de recursos do Azure e do grupo de recursos é irreversível. Após a eliminação, o grupo de recursos e todos os recursos nele contidos são eliminados permanentemente. Confirme que não elimina acidentalmente o grupo de recursos ou recursos errados. Se tiver criado o Hub IoT no interior de um grupo de recursos existente que contenha recursos que pretende manter, elimine apenas o recurso do Hub IoT, em vez de eliminar o grupo de recursos.
>

Para eliminar apenas o Hub IoT, execute o seguinte comando com o nome do hub e o nome do grupo de recursos:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Para eliminar todo o grupo de recursos por nome:

1. Inicie sessão no [Portal do Azure](https://portal.azure.com) e clique em **Grupos de recursos**.

2. Na caixa de texto **Filtrar por nome...**, escreva o nome do grupo de recursos que contém o seu Hub IoT. 

3. À direita do seu grupo de recursos na lista de resultados, clique em **...** e em **Eliminar grupo de recursos**.

4. É-lhe pedido que confirme a eliminação do grupo de recursos. Escreva o nome do grupo de recursos novamente para confirmar e, em seguida, clique em **Eliminar**. Após alguns instantes, o grupo de recursos e todos os recursos contidos no mesmo são eliminados.



## <a name="next-steps"></a>Passos seguintes

Neste tutorial, criou uma função do módulo do IoT Edge que contém código para filtrar dados não processados gerados pelo seu dispositivo IoT Edge. Quando estiver pronto para criar os seus próprios módulos, pode saber mais sobre como [Desenvolver um módulo de C# com o Azure IoT Edge para Visual Studio Code](how-to-develop-csharp-module.md). Pode continuar para os próximos tutoriais para saber mais sobre outras formas em que o Azure IoT Edge o pode ajudar a transformar os dados em informações empresariais na periferia.

> [!div class="nextstepaction"]
> [Armazenar dados na periferia com bases de dados do SQL Server](tutorial-store-data-sql-server.md)

<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
