---
title: Integração contínua do IoT Edge do Azure e a implementação contínua | Documentos da Microsoft
description: Descrição geral da integração contínua e implementação contínua para o Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 5099ca70503ba2ed4ae8f4969a9199816c4986fb
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302576"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Integração contínua e implementação contínua para o Azure IoT Edge

Este artigo demonstra como pode utilizar a integração contínua e os recursos de implementação contínua do Microsoft Team Foundation Server (TFS) e serviços de DevOps do Azure para criar, testar e implementar aplicações rápida e eficiente para o Azure IoT Edge. 

Neste artigo, ficará a saber como:
* Criar e verificar num exemplo de unidade que contém de solução de IoT Edge testes.
* Instale a extensão Azure IoT Edge para sua programação e operações do Azure.
* Configure a integração contínua (CI) para criar a solução e executar os testes de unidade.
* Configure a implementação contínua (CD) para implementar a solução e ver respostas.

Irá demorar 30 minutos para concluir os passos neste artigo.

![CI e CD](./media/how-to-ci-cd/cd.png)

## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Criar uma solução do Azure IoT Edge de exemplo com o Visual Studio Code

Nesta secção, irá criar um exemplo do IoT Edge solução que contém testes de unidade que poderá ser executado como parte do processo de compilação. Antes de seguir as orientações nesta secção, conclua os passos na [desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md).

1. Na paleta de comandos do VS Code, escreva e execute o comando **Edge: solução nova do IoT Edge**. Em seguida, selecione a pasta de área de trabalho, forneça o nome da solução (o nome predefinido é **EdgeSolution**) e criar um módulo de c# (**FilterModule**) como o primeiro módulo do utilizador nesta solução. Também tem de especificar o repositório de imagens do Docker para o seu primeiro módulo. O repositório de imagens padrão baseia-se um registo do Docker local (`localhost:5000/filtermodule`). Terá de alterá-lo para o Azure Container Registry (`<your container registry address>/filtermodule`) ou o Hub do Docker para ainda mais a integração contínua.

    ![Configuração ACR](./media/how-to-ci-cd/acr.png)

2. Janela do VS Code carregará a sua área de trabalho de solução de IoT Edge. Além disso, pode, opcionalmente, escreva e execute **Edge: módulo do IoT Edge/adicionar** para adicionar mais módulos. Há uma `modules` pasta, um `.vscode` pasta e um arquivo de manifesto do modelo de implementação na pasta raiz. Todos os códigos de módulo de utilizador será subpastas na pasta `modules`. O `deployment.template.json` é o modelo de manifesto de implantação. Alguns dos parâmetros existentes neste ficheiro vão ser analisado a partir do `module.json`, que existe na pasta de cada módulo.

3. Agora seu exemplo de solução de IoT Edge está pronto. O módulo c# padrão funciona como um módulo de mensagem de pipe. Na `deployment.template.json`, verá essa solução contém dois módulos. A mensagem será gerada a partir da `tempSensor` módulo e será ser encaminhado diretamente através de `FilterModule`, em seguida, enviadas ao seu hub IoT. Substitua todo o **Program.cs** de ficheiros com abaixo conteúdo. Para obter mais informações sobre este fragmento de código, pode consultar [criar um projeto de módulo do IoT Edge c#](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module#create-an-iot-edge-module-project).

    ```csharp
    namespace FilterModule
    {
        using System;
        using System.IO;
        using System.Runtime.InteropServices;
        using System.Runtime.Loader;
        using System.Security.Cryptography.X509Certificates;
        using System.Text;
        using System.Threading;
        using System.Threading.Tasks;
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Client.Transport.Mqtt;
        using System.Collections.Generic;     // for KeyValuePair<>
        using Microsoft.Azure.Devices.Shared; // for TwinCollection
        using Newtonsoft.Json;                // for JsonConvert

        public class MessageBody
        {
            public Machine machine { get; set; }
            public Ambient ambient { get; set; }
            public string timeCreated { get; set; }
        }
        public class Machine
        {
            public double temperature { get; set; }
            public double pressure { get; set; }
        }
        public class Ambient
        {
            public double temperature { get; set; }
            public int humidity { get; set; }
        }

        public class Program
        {
            static int counter;
            static int temperatureThreshold { get; set; } = 25;

            static void Main(string[] args)
            {
                Init().Wait();

                // Wait until the app unloads or is cancelled
                var cts = new CancellationTokenSource();
                AssemblyLoadContext.Default.Unloading += (ctx) => cts.Cancel();
                Console.CancelKeyPress += (sender, cpe) => cts.Cancel();
                WhenCancelled(cts.Token).Wait();
            }

            /// <summary>
            /// Handles cleanup operations when app is cancelled or unloads
            /// </summary>
            public static Task WhenCancelled(CancellationToken cancellationToken)
            {
                var tcs = new TaskCompletionSource<bool>();
                cancellationToken.Register(s => ((TaskCompletionSource<bool>)s).SetResult(true), tcs);
                return tcs.Task;
            }

            /// <summary>
            /// Initializes the ModuleClient and sets up the callback to receive
            /// messages containing temperature information
            /// </summary>
            static async Task Init()
            {
                MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                ITransportSettings[] settings = { mqttSetting };

                // Open a connection to the Edge runtime
                ModuleClient ioTHubModuleClient = await ModuleClient.CreateFromEnvironmentAsync(settings);
                await ioTHubModuleClient.OpenAsync();
                Console.WriteLine("IoT Hub module client initialized.");

                // Register callback to be called when a message is received by the module
                await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessage, ioTHubModuleClient);
            }

            /// <summary>
            /// This method is called whenever the module is sent a message from the EdgeHub. 
            /// It just pipe the messages without any change.
            /// It prints all the incoming messages.
            /// </summary>
            static async Task<MessageResponse> FilterMessage(Message message, object userContext)
            {
                int counterValue = Interlocked.Increment(ref counter);

                var moduleClient = userContext as ModuleClient;
                if (moduleClient == null)
                {
                    throw new InvalidOperationException("UserContext doesn't contain " + "expected values");
                }

                byte[] messageBytes = message.GetBytes();
                string messageString = Encoding.UTF8.GetString(messageBytes);
                Console.WriteLine($"Received message: {counterValue}, Body: [{messageString}]");

                var filteredMessage = filter(message);

                if (filteredMessage != null && !string.IsNullOrEmpty(messageString))
                {
                    var pipeMessage = new Message(messageBytes);
                    foreach (var prop in message.Properties)
                    {
                        pipeMessage.Properties.Add(prop.Key, prop.Value);
                    }
                    await moduleClient.SendEventAsync("output1", pipeMessage);
                    Console.WriteLine("Received message sent");
                }
                return MessageResponse.Completed;
            }

            public static Message filter(Message message)
            {
                var counterValue = Interlocked.Increment(ref counter);

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
                    return filteredMessage;
                }
                return null;
            }
        }
    }
    ```

4. Criar um .Net projeto de teste de unidade de núcleo. No Explorador de ficheiros do VS Code, crie uma nova pasta **tests\FilterModuleTest** na sua área de trabalho. Em seguida, no terminal integrado do VS Code (**Ctrl + '**), execute seguintes comandos para criar um projeto de teste xunit e adicionar a referência para o **FilterModule** projeto.

    ```cmd
    cd tests\FilterModuleTest
    dotnet new xunit
    dotnet add reference ../../modules/FilterModule/FilterModule.csproj
    ```

    ![Estrutura de pastas](./media/how-to-ci-cd/add-test-project.png)

5. Na **FilterModuleTest** pasta, atualizar o nome de ficheiro **UnitTest1.cs** para **FilterModuleTest.cs**. Selecione e abra **FilterModuleTest.cs**, substitua todo o código com abaixo trecho de código, que contém os testes de unidade ao projeto de FilterModule.

    ```csharp
    using Xunit;
    using FilterModule;
    using Newtonsoft.Json;
    using System;
    using System.IO;
    using System.Runtime.InteropServices;
    using System.Runtime.Loader;
    using System.Security.Cryptography.X509Certificates;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Client.Transport.Mqtt;

    namespace FilterModuleTest
    {
        public class FilterModuleTest
        {
            [Fact]
            public void filterLessThanThresholdTest()
            {
                var source = createMessage(25 - 1);
                var result = Program.filter(source);
                Assert.True(result == null);
            }

            [Fact]
            public void filterMoreThanThresholdAlertPropertyTest()
            {
                var source = createMessage(25 + 1);
                var result = Program.filter(source);
                Assert.True(result.Properties["MessageType"] == "Alert");
            }

            [Fact]
            public void filterMoreThanThresholdCopyPropertyTest()
            {
                var source = createMessage(25 + 1);
                source.Properties.Add("customTestKey", "customTestValue");
                var result = Program.filter(source);
                Assert.True(result.Properties["customTestKey"] == "customTestValue");
            }

            private Message createMessage(int temperature)
            {
                var messageBody = createMessageBody(temperature);
                var messageString = JsonConvert.SerializeObject(messageBody);
                var messageBytes = Encoding.UTF8.GetBytes(messageString);
                return new Message(messageBytes);
            }

            private MessageBody createMessageBody(int temperature)
            {
                var messageBody = new MessageBody
                {
                    machine = new Machine
                    {
                        temperature = temperature,
                        pressure = 0
                    },
                    ambient = new Ambient
                    {
                        temperature = 0,
                        humidity = 0
                    },
                    timeCreated = string.Format("{0:O}", DateTime.Now)
                };

                return messageBody;
            }
        }
    }
    ```

6. No terminal integrado, pode inserir seguintes comandos para executar testes de unidade localmente. 
    ```cmd
    dotnet test
    ```

    ![Teste de unidade](./media/how-to-ci-cd/unit-test.png)

7. Guardar esses projetos, em seguida, verificá-lo no seu repositório de DevOps do Azure ou do TFS.
    

> [!NOTE]
> Para obter mais informações sobre como utilizar o Azure repositórios, consulte [compartilhar seu código com o Visual Studio e Azure repositórios](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Configure a integração contínua
Nesta secção, irá criar um pipeline de compilação que está configurado para ser executada automaticamente quando fizer check-in todas as alterações para o exemplo de solução de IoT Edge e executará automaticamente os testes de unidade que contém.

1. Inicie sessão na sua organização de DevOps do Azure (**https://**_sua conta_**. visualstudio.com**) e abra o projeto em que tiver selecionado na aplicação de exemplo.

    ![Código de verificação](./media/how-to-ci-cd/init-project.png)

1. Visite [do Azure IoT Edge, para o Azure DevOps](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) no mercado de DevOps do Azure. Clique em **obter gratuitamente** e siga o Assistente para instalar esta extensão para a sua organização de DevOps do Azure ou a transferência para o TFS.

    ![Instalar a extensão](./media/how-to-ci-cd/install-extension.png)

1. No seu Azure DevOps, abra a **crie &amp; versão** hub e, no **baseia-se** separador, escolha **+ novo pipeline**. Ou, se já tiver de criação de pipelines, escolha o **+ novo** botão. 

    ![Nova compilação](./media/how-to-ci-cd/add-new-build.png)

1. Se lhe for pedido, selecione o **Git de DevOps do Azure** tipo de origem; em seguida, selecione o projeto, o repositório e o ramo em que o seu código está localizado. Escolher **continuar**.

    ![Selecione o git do Azure DevOps](./media/how-to-ci-cd/select-vsts-git.png)

1. Na **selecionar um modelo** janela, escolha **começar com um processo vazio**.

    ![Iniciar vazio](./media/how-to-ci-cd/start-with-empty.png)

1. Clique em **+** no lado direito da **fase 1** para adicionar uma tarefa para a fase. Em seguida, procure e selecione **.Net Core**e clique em **Add** adicionar esta tarefa para a fase.

    ![Teste do DotNet](./media/how-to-ci-cd/add-dot-net-core.png)

1. Atualização do **nome a apresentar** para **dotnet teste**e, no **comando** lista pendente, selecione **testar**. Adicionar o caminho para o **caminho para descreveram**.

    ```
    tests/FilterModuleTest/*.csproj
    ```

    ![Configurar o teste do dotnet](./media/how-to-ci-cd/dotnet-test.png)

1. Clique em **+** no lado direito da **fase 1** para adicionar uma tarefa para a fase. Em seguida, procure e selecione **do Azure IoT Edge**e clique em **Add** botão **duas vezes** para adicionar estas tarefas para a fase.

    ![IoT Edge](./media/how-to-ci-cd/add-azure-iot-edge.png)

1. Na primeira tarefa do Azure IoT Edge, atualize o **nome a apresentar** para **Push e de criar o módulo**e, no **ação** lista pendente, selecione **criar e enviar por Push**. Na **Module ficheiro** caixa de texto, adicionar caminho ao mesmo. Em seguida, escolha **o tipo de registo de contentor**, certifique-se de configurar e selecionar o mesmo registo no seu código. Esta tarefa criar e enviar por push todos os seus módulos na solução e publicar no registo de contentor especificado. Se seus módulos são enviados aos registos diferentes, pode ter vários **Push e de criar o módulo** tarefas.

    ```
    **/module.json
    ```

    ![Módulo de compilação e emissão](./media/how-to-ci-cd/module-build-push.png)

1. Na segunda tarefa do Azure IoT Edge, atualize o **nome a apresentar** ao **implementar no dispositivo IoT Edge**e, no **ação** lista pendente, selecione **implementar no IoT Edge dispositivo**. Selecione a sua subscrição do Azure e introduza o nome do seu IoT Hub. Pode especificar um ID de implementação do IoT Edge e a prioridade de implantação. Também pode optar por implementar um ou vários dispositivos. Se estiver a implementar vários dispositivos, tem de especificar a condição de destino do dispositivo. Por exemplo, se pretender utilizar etiquetas do dispositivo como a condição, terá de atualizar os seus dispositivos correspondentes etiquetas antes da implantação. 

    ![Implementar no Edge](./media/how-to-ci-cd/deploy-to-edge.png)

1. Clique nas **processo** e certifique-se sua **fronta Agenta** é **alojado pré-visualização do Linux**.

    ![Configurar](./media/how-to-ci-cd/configure-env.png)

1. Abra o **Acionadores** separador e ativar o **integração contínua** acionador. Certifique-se de que o ramo que contém o código está incluído.

    ![Acionador](./media/how-to-ci-cd/configure-trigger.png)

1. Guarde o novo pipeline de compilação e uma nova compilação na fila. Clique nas **guardar e colocar em fila** botão.

1. Selecione a ligação para a compilação na barra de mensagem que aparece. Em alternativa, aceda a criar o pipeline para ver a tarefa mais recente compilação em fila.

    ![Compilação](./media/how-to-ci-cd/build-def.png)

1. Após a compilação estiver concluída, verá o resumo para cada tarefa e os resultados no ficheiro de registo em direto. 
    
    ![Concluir](./media/how-to-ci-cd/complete.png)

1. Pode voltar atrás para o VS Code e verificar o Explorador de dispositivos do IoT Hub. O dispositivo de limite com o módulo deve iniciar a execução (Certifique-se de adicionar as credenciais do registo ao runtime do Edge).

    ![Edge a ser executado](./media/how-to-ci-cd/edge-running.png)

## <a name="continuous-deployment-to-iot-edge-devices"></a>Implementação contínua para dispositivos do IoT Edge

Para ativar a implementação contínua, basicamente, tem de definir os trabalhos de CI com dispositivos IoT Edge adequados, permitindo que o **Acionadores** para os seus ramos do seu projeto. Na prática de DevOps clássica, um projeto contém duas ramificações principais. O ramo principal deve ser a versão estável do código e o ramo de desenvolver contém as alterações de código mais recente. Todo desenvolvedor na equipe deve bifurcar o ramo de desenvolver seu blog ou sua própria ramificação recursos quando começar a atualizar o código, que significa que todas as consolidações acontece na funcionalidade ramos desativar o ramo de desenvolver. E cada consolidação enviada por push deve ser testada através do sistema de CI. Após testar totalmente o código localmente, a ramificação recursos deve ser intercalada no ramo de desenvolver através de um pedido pull. Quando o código no ramo de programador é testado por meio de sistema de CI, podem ser mesclada para o ramo principal através de um pedido pull.

Ao implementar em dispositivos IoT Edge, há três principais ambientes.
- Na ramificação recursos, pode utilizar o dispositivo IoT Edge simulado no seu computador de desenvolvimento ou implementar um dispositivo físico do IoT Edge.
- Desenvolver no ramo, deve implementar um dispositivo físico do IoT Edge.
- No ramo principal, os dispositivos do IoT Edge de destino devem ser os dispositivos de produção.

## <a name="next-steps"></a>Passos Seguintes

* Compreender a implementação de IoT Edge no [implementações de compreender o IoT Edge para dispositivos individuais ou em escala](module-deployment-monitoring.md)
* Siga os passos para criar, atualizar ou eliminar uma implementação no [implementar e monitorizar os módulos do IoT Edge em escala](how-to-deploy-monitor.md).
