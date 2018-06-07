---
title: Integração contínua de limite de IoT do Azure e a implementação contínua | Microsoft Docs
description: Descrição geral da integração contínua e a implementação contínua para limite de IoT do Azure
author: shizn
manager: ''
ms.author: xshi
ms.date: 04/30/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a8b58eae9aa08d8f6539370fa6e78a7a4813c18f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631025"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge---preview"></a>A integração contínua e a implementação contínua para limite de IoT do Azure - pré-visualização
Este tutorial demonstra como pode utilizar a integração contínua e funcionalidades de implementação contínua dos serviços de equipa do Visual Studio (VSTS) e Microsoft Team Foundation Server (TFS) para criar, testar e implementar aplicações de forma rápida e a sua Limite de IoT do Azure. 

Neste tutorial, vai aprender a:
> [!div class="checklist"]
> * Crie e verificar um exemplo de unidade que contém de solução IoT Edge testes.
> * Instale a extensão de limite de IoT do Azure para a sua VSTS.
> * Configure a integração contínua (CI) para compilar a solução e os testes de unidade.
> * Configure a implementação contínua (CD) para implementar a solução e ver respostas.

Irá demorar 30 minutos para concluir este tutorial.

![CI e do CD](./media/how-to-ci-cd/cd.png)

## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Criar uma solução de limite de IoT do Azure de exemplo utilizando o Visual Studio Code

Nesta secção, irá criar um exemplo de limite de IoT solução que contém unidade os testes que pode ser executado como parte do processo de compilação. Antes de seguir as orientações nesta secção, concluir os passos [desenvolver uma solução de IoT Edge com vários módulos no Visual Studio Code](tutorial-multiple-modules-in-vscode.md).

1. Na paleta do comando de VS Code, escreva e execute o comando **Edge: solução de ponta de IoT novo**. Em seguida, selecione a pasta à sua área de trabalho, forneça o nome de solução (o nome predefinido é **EdgeSolution**) e criar um módulo c# (**FilterModule**) como o módulo de utilizador primeiro nesta solução. Também tem de especificar o repositório de imagens do Docker para o seu módulo primeiro. O repositório de imagens predefinido baseia-se um registo de Docker local (`localhost:5000/filtermodule`). Terá de alterá-la no registo de contentor do Azure (`<your container registry address>/filtermodule`) ou o Hub de Docker para obter mais integração contínua.

    ![Configuração ACR](./media/how-to-ci-cd/acr.png)

2. A janela de VS Code carregará a sua área de trabalho de solução IoT Edge. Pode, opcionalmente, escreva e executar **contorno: o módulo de limite de IoT adicionar** para adicionar mais módulos. Não existe um `modules` pasta, um `.vscode` pasta e um ficheiro de manifesto do modelo de implementação na pasta raiz. Todos os códigos de módulo de utilizador será subpastas na pasta `modules`. O `deployment.template.json` é o modelo de manifesto de implementação. Alguns dos parâmetros de neste ficheiro irão ser analisado a partir do `module.json`, que existe na pasta cada módulo.

3. O exemplo de solução IoT Edge está agora pronto. O módulo c# predefinido funciona como um módulo de mensagem de pipe. No `deployment.template.json`, verá esta solução contém dois módulos. A mensagem será gerada a partir de `tempSensor` módulo e irá ser direcionado diretamente através do `FilterModule`, em seguida, enviado ao seu IoT hub. Substitua a toda a **Program.cs** ficheiros com abaixo conteúdo. Para obter mais informações sobre este fragmento de código, pode fazer referência a [criar um projeto de módulo do IoT Edge c#](https://docs.microsoft.com/azure/iot-edge/tutorial-csharp-module#create-an-iot-edge-module-project).

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
                // The Edge runtime gives us the connection string we need -- it is injected as an environment variable
                string connectionString = Environment.GetEnvironmentVariable("EdgeHubConnectionString");

                // Cert verification is not yet fully functional when using Windows OS for the container
                bool bypassCertVerification = RuntimeInformation.IsOSPlatform(OSPlatform.Windows);
                if (!bypassCertVerification) InstallCert();
                Init(connectionString, bypassCertVerification).Wait();

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
            /// Add certificate in local cert store for use by client for secure connection to IoT Edge runtime
            /// </summary>
            static void InstallCert()
            {
                string certPath = Environment.GetEnvironmentVariable("EdgeModuleCACertificateFile");
                if (string.IsNullOrWhiteSpace(certPath))
                {
                    // We cannot proceed further without a proper cert file
                    Console.WriteLine($"Missing path to certificate collection file: {certPath}");
                    throw new InvalidOperationException("Missing path to certificate file.");
                }
                else if (!File.Exists(certPath))
                {
                    // We cannot proceed further without a proper cert file
                    Console.WriteLine($"Missing path to certificate collection file: {certPath}");
                    throw new InvalidOperationException("Missing certificate file.");
                }
                X509Store store = new X509Store(StoreName.Root, StoreLocation.CurrentUser);
                store.Open(OpenFlags.ReadWrite);
                store.Add(new X509Certificate2(X509Certificate2.CreateFromCertFile(certPath)));
                Console.WriteLine("Added Cert: " + certPath);
                store.Close();
            }
            /// <summary>
            /// Initializes the DeviceClient and sets up the callback to receive
            /// messages containing temperature information
            /// </summary>
            static async Task Init(string connectionString, bool bypassCertVerification = false)
            {
                Console.WriteLine("Connection String {0}", connectionString);

                MqttTransportSettings mqttSetting = new MqttTransportSettings(TransportType.Mqtt_Tcp_Only);
                // During dev you might want to bypass the cert verification. It is highly recommended to verify certs systematically in production
                if (bypassCertVerification)
                {
                    mqttSetting.RemoteCertificateValidationCallback = (sender, certificate, chain, sslPolicyErrors) => true;
                }
                ITransportSettings[] settings = { mqttSetting };

                // Open a connection to the Edge runtime
                DeviceClient ioTHubModuleClient = DeviceClient.CreateFromConnectionString(connectionString, settings);
                await ioTHubModuleClient.OpenAsync();
                Console.WriteLine("IoT Hub module client initialized.");

                // Register callback to be called when a message is received by the module
                // await ioTHubModuleClient.SetImputMessageHandlerAsync("input1", PipeMessage, iotHubModuleClient);

                // Read TemperatureThreshold from Module Twin Desired Properties
                var moduleTwin = await ioTHubModuleClient.GetTwinAsync();
                var moduleTwinCollection = moduleTwin.Properties.Desired;
                try {
                    temperatureThreshold = moduleTwinCollection["TemperatureThreshold"];
                } catch(ArgumentOutOfRangeException) {
                    Console.WriteLine("Proerty TemperatureThreshold not exist");
                }

                // Attach callback for Twin desired properties updates
                await ioTHubModuleClient.SetDesiredPropertyUpdateCallbackAsync(onDesiredPropertiesUpdate, null);

                // Register callback to be called when a message is received by the module
                await ioTHubModuleClient.SetInputMessageHandlerAsync("input1", FilterMessages, ioTHubModuleClient);
            }

            static Task onDesiredPropertiesUpdate(TwinCollection desiredProperties, object userContext)
            {
                try
                {
                    Console.WriteLine("Desired property change:");
                    Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));

                    if (desiredProperties["TemperatureThreshold"] != null)
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

            static async Task<MessageResponse> FilterMessages(Message message, object userContext)
            {
                try
                {
                    DeviceClient deviceClient = (DeviceClient)userContext;

                    var filteredMessage = filter(message);
                    if (filteredMessage != null)
                    {
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
        }
    }
    ```

4. Criar um .Net projeto de teste de unidade principal. No Explorador de ficheiros VS Code, crie uma nova pasta **tests\FilterModuleTest** na sua área de trabalho. Em seguida, no terminal integrada VS Code (**Ctrl + '**), execute os seguintes comandos para criar um projeto de teste xunit e adicionar a referência para o **FilterModule** projeto.

    ```cmd
    cd tests\FilterModuleTest
    dotnet new xunit
    dotnet add reference ../../modules/FilterModule/FilterModule.csproj
    ```

    ![Estrutura de pastas](./media/how-to-ci-cd/add-test-project.png)

5. No **FilterModuleTest** pasta, o nome do ficheiro de atualização **UnitTest1.cs** para **FilterModuleTest.cs**. Selecione e abra **FilterModuleTest.cs**, substitua o código completo com abaixo fragmento de código, que contém os testes de unidade contra o projeto de FilterModule.

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

6. No terminal integrada, pode introduzir os seguintes comandos para executar testes de unidade localmente. 
    ```cmd
    dotnet test
    ```

    ![Teste de unidade](./media/how-to-ci-cd/unit-test.png)

7. Guardar estas projetos, em seguida, registá-lo no seu repositório VSTS ou TFS.
    

> [!NOTE]
> Para obter mais informações sobre como utilizar VSTS repositórios de código, consulte [partilhar o seu código com o Visual Studio e VSTS Git](https://docs.microsoft.com/vsts/git/share-your-code-in-git-vs?view=vsts).


## <a name="configure-continuous-integration"></a>Configurar a integração contínua
Nesta secção, irá criar uma definição de compilação que está configurada para ser executada automaticamente quando der entrada do alterações para o exemplo de solução IoT Edge e executará automaticamente os testes de unidade que contém.

1. Inicie sessão na sua conta VSTS (**https://**_sua conta_**. visualstudio.com**) e abra o projeto em que a opção estiver marcada na aplicação de exemplo.

    ![Código de verificação](./media/how-to-ci-cd/init-project.png)

1. Visite [contorno de IoT do Azure para VSTS](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) no VSTS Marketplace. Clique em **obter livre** e siga o Assistente para instalar esta extensão para a sua conta VSTS ou transferência para o TFS.

    ![Instalar a extensão](./media/how-to-ci-cd/install-extension.png)

1. No seu VSTS, abra o **criar &amp; versão** hub e, no **baseia-se** separador, escolha **+ nova definição**. Ou, se já tiver definições de compilação, escolha o **+ novo** botão. 

    ![Nova compilação](./media/how-to-ci-cd/add-new-build.png)

1. Se lhe for pedido, selecione o **VSTS Git** tipo de origem; em seguida, selecione o projeto, repositório e sucursal onde está localizado o seu código. Escolha **continuar**.

    ![Selecione VSTS git](./media/how-to-ci-cd/select-vsts-git.png)

1. No **selecionar um modelo** janela, escolha **começar com um processo vazio**.

    ![Iniciar vazio](./media/how-to-ci-cd/start-with-empty.png)

1. Clique em **+** no lado direito da **fase 1** para adicionar uma tarefa para a fase. Em seguida, procure e selecione **.Net Core**e clique em **adicionar** para adicionar esta tarefa para a fase.

    ![Teste do DotNet](./media/how-to-ci-cd/add-dot-net-core.png)

1. Atualização do **nome a apresentar** para **dotnet teste**e, no **comando** na lista pendente, selecione **testar**. Adicionar abaixo do caminho para o **caminho project(s)**.

    ```
    tests/FilterModuleTest/*.csproj
    ```

    ![Configurar o teste do dotnet](./media/how-to-ci-cd/dotnet-test.png)

1. Clique em **+** no lado direito da **fase 1** para adicionar uma tarefa para a fase. Em seguida, procure e selecione **Azure IoT Edge**e clique em **adicionar** botão **duas vezes** para adicionar estas tarefas para a fase.

    ![IoT Edge](./media/how-to-ci-cd/add-azure-iot-edge.png)

1. Na primeira tarefa do Azure IoT Edge, atualize o **nome a apresentar** para **Push e de compilação do módulo**e, no **ação** na lista pendente, selecione **criar e emitir**. No **Module.json ficheiro** caixa de texto, adicione-lhe abaixo do caminho. Em seguida, escolha **o tipo de registo do contentor**, certifique-se de que configura e selecione o mesmo registo no seu código. Esta tarefa de compilação e push de todos os seus módulos na solução e publicar no registo de contentor especificado. 

    ```
    **/module.json
    ```

    ![Push e de compilação do módulo](./media/how-to-ci-cd/module-build-push.png)

1. A segunda tarefa de limite de IoT do Azure, atualizar o **nome a apresentar** para **implementar no dispositivo de limite de IoT**e, no **ação** na lista pendente, selecione **implementar no limite de IoT dispositivo**. Selecione a sua subscrição do Azure e introduza o nome do seu IoT Hub. Pode especificar um ID de implementação de limite de IoT e a prioridade de implementação. Também pode optar por implementar único ou vários dispositivos. Se estiver a implementar vários dispositivos, tem de especificar a condição de destino do dispositivo. Por exemplo, se pretender utilizar etiquetas de dispositivo como a condição, terá de atualizar os dispositivos correspondentes etiquetas antes da implementação. 

    ![Implementar Edge](./media/how-to-ci-cd/deploy-to-edge.png)

1. Clique no **processo** e certifique-se a **fila agente** é **alojado pré-visualização do Linux**.

    ![Configurar](./media/how-to-ci-cd/configure-env.png)

1. Abra o **Acionadores** separador e ative o **integração contínua** acionador. Certifique-se o ramo que contém o código incluído.

    ![Acionador](./media/how-to-ci-cd/configure-trigger.png)

1. Guardar a nova definição de compilação e uma nova compilação de fila de espera. Clique em de **Guardar & fila** botão.

1. Escolha a ligação para a compilação na barra de mensagens que aparece. Ou vá para criar a definição para ver a tarefa mais recente compilação em fila.

    ![Compilação](./media/how-to-ci-cd/build-def.png)

1. Após concluir a compilação, pode ver o resumo para cada tarefa e os resultados no ficheiro de registo em direto. 
    
    ![Concluir](./media/how-to-ci-cd/complete.png)

1. Pode voltar para o VS Code e verifique o Explorador do dispositivo IoT Hub. O dispositivo de limite com o módulo deve iniciar a execução (Certifique-se de que adicionou as credenciais do registo ao tempo de execução de limite).

    ![Limite em execução](./media/how-to-ci-cd/edge-running.png)

## <a name="continuous-deployment-to-iot-edge-devices"></a>Implementação contínua para dispositivos de limite de IoT

Para ativar a implementação contínua, basicamente tem de configurar tarefas de CI com dispositivos de limite de IoT adequados, ativar o **Acionadores** para seus ramos no seu projeto. Na prática de DevOps clássica, um projeto contém dois ramos principais. O ramo principal deve ser a versão estável do código e o ramo desenvolver contém as últimas alterações de código. Cada programador que no agrupamento deve bifurcar desenvolver ramo para a ou a sua própria ramo funcionalidade quando iniciar a atualizar o código, o que significa que todos os consolida acontece funcionalidade ramos desativar o ramo desenvolver. E cada consolidação premida deve ser testada através do sistema de CI. Depois de testado completamente o código localmente, deve ser intercalado o ramo de funcionalidade para o ramo de desenvolver através de um pedido de solicitação. Quando o código no ramo do programador é testado através do sistema de CI, possam ser intercalada para o ramo principal através de um pedido de solicitação.

Por isso, ao implementar em dispositivos de IoT Edge, existem três ambientes principais.
- No ramo de funcionalidade, pode utilizar o dispositivo de limite de IoT simulado no computador de desenvolvimento ou implementar um dispositivo de limite de IoT físico.
- Desenvolver na sucursal, deve implementar num dispositivo de limite de IoT físico.
- No ramo principal, os dispositivos de IoT extremidade de destino devem ser os dispositivos de produção.

## <a name="next-steps"></a>Passos Seguintes

Este tutorial demonstra como pode utilizar a integração contínua e a implementação contínua as funcionalidades de VSTS ou TFS. 

* Compreender a implementação de limite de IoT no [implementações de limite de IoT compreender para dispositivos único ou à escala](module-deployment-monitoring.md)
* Siga os passos para criar, atualizar ou eliminar uma implementação no [implementar e monitorizar os módulos de limite de IoT à escala] [how-to-implementar-monitor.md].









