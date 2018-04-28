---
title: Agendar tarefas com o Azure IoT Hub (.NET/.NET) | Microsoft Docs
description: Como agendar um trabalho do IoT Hub do Azure para invocar um método direto em vários dispositivos. Utilizar o dispositivo IoT do Azure SDK para .NET para implementar as aplicações de dispositivo simulada e uma aplicação de serviço para executar a tarefa.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 012/16/2018
ms.author: v-masebo;dobett
ms.openlocfilehash: 76c8d3739b2af3c010cd80585c93c097fc9eb466
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="schedule-and-broadcast-jobs-netnet"></a>Tarefas de agendamento e de difusão (.NET/.NET)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Utilize o IoT Hub do Azure para agendar e controlar as tarefas que atualizam milhões de dispositivos. Utilize tarefas para:

* Atualizar as propriedades pretendidas
* Etiquetas de atualização
* Invocar métodos diretos

Uma tarefa encapsula num wrapper uma destas ações e controla a execução em relação a um conjunto de dispositivos que é definida por uma consulta do dispositivo duplo. Por exemplo, uma aplicação de back-end pode utilizar uma tarefa para invocar um método direto em 10 000 dispositivos que reinicia os dispositivos. Especifique o conjunto de dispositivos com uma consulta do dispositivo duplo e agendar a tarefa para ser executada num momento futuro. A evolução da tarefa controla como cada um dos dispositivos receber e executar o método direta de reinício.

Para obter mais informações sobre cada uma destas capacidades, consulte:

* Dispositivo duplo e propriedades: [começar a utilizar dispositivos duplos] [ lnk-get-started-twin] e [Tutorial: como utilizar as propriedades do dispositivo duplo][lnk-twin-props]
* Direcionar métodos: [guia para programadores do IoT Hub - métodos diretas] [ lnk-dev-methods] e [Tutorial: utilizar métodos diretos][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este tutorial mostrar-lhe como:

* Criar uma aplicação de dispositivo que implementa um método direto chamado **LockDoor** que pode ser chamado pela aplicação de back-end.
* Criar uma aplicação de back-end que cria uma tarefa para chamar o **LockDoor** método direto em vários dispositivos. Outra tarefa envia atualizações da propriedade pretendido para vários dispositivos.

No final deste tutorial, tem duas aplicações de consola .NET (c#):

**SimulateDeviceMethods** que estabelece ligação ao seu IoT hub e implementa o **LockDoor** método direto.

**ScheduleJob** tarefas que utiliza para chamar o **LockDoor** método direto e atualizar as propriedades do dispositivo duplo pretendida em vários dispositivos.

Para concluir este tutorial, precisa do seguinte:

* Visual Studio 2015 ou Visual Studio 2017.
* Uma conta ativa do Azure. Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Criar uma aplicação de dispositivo simulada
Nesta secção, crie uma aplicação de consola .NET que responde a um método direto chamado pela solução de back-end.

1. No Visual Studio, adicione um projeto Visual C# no Ambiente de Trabalho Clássico do Windows à atual solução, utilizando o modelo de projeto **Aplicação de Consola**. Nomeie o projeto **SimulateDeviceMethods**.
   
    ![Nova aplicação de dispositivo Visual c# clássico do Windows][img-createdeviceapp]
    
1. No Explorador de soluções, clique com botão direito do **SimulateDeviceMethods** projeto e, em seguida, clique em **gerir pacotes NuGet...** .

1. No **Gestor de pacotes NuGet** janela, selecione **procurar** e procure **microsoft.azure.devices.client**. Selecione **instalar** para instalar o **Microsoft.Azure.Devices.Client** do pacote e aceite os termos de utilização. Este procedimento transfere, instala e adiciona uma referência para o [dispositivos IoT do Azure SDK] [ lnk-nuget-client-sdk] NuGet pacote e as respetivas dependências.
   
    ![Aplicação de cliente de janela do Gestor de pacotes NuGet][img-clientnuget]

1. Adicione as seguinte declarações `using` na parte superior do ficheiro **Program.cs**:
   
    ```csharp
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;

    using Newtonsoft.Json;
    ```

1. Adicione os seguintes campos à classe **Programa**. Substitua o valor do marcador de posição pela cadeia de ligação de dispositivo que anotou na secção anterior:

    ```csharp
    static string DeviceConnectionString = "<yourDeviceConnectionString>";
    static DeviceClient Client = null;

1. Add the following to implement the direct method on the device:

    ```csharp
    static Task<MethodResponse> LockDoor(MethodRequest methodRequest, object userContext)
    {
        Console.WriteLine();
        Console.WriteLine("Locking Door!");
        Console.WriteLine("\nReturning response for method {0}", methodRequest.Name);
            
        string result = "'Door was locked.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }

1. Add the following to implement the device twins listener on the device:

    ```csharp
    private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
    {
        Console.WriteLine("Desired property change:");
        Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
    }
    ```

1. Por fim, adicione o seguinte código para o **Main** método para abrir a ligação ao seu IoT hub e inicializar o serviço de escuta do método:
   
    ```csharp
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);

        Client.SetMethodHandlerAsync("LockDoor", LockDoor, null);
        Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null);

        Console.WriteLine("Waiting for direct method call and device twin update\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        Client.SetMethodHandlerAsync("LockDoor", null, null);
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
1. Guarde o trabalho e criar a sua solução.         

> [!NOTE]
> Para facilitar, este tutorial não implementa nenhuma política de repetição. No código de produção, deve implementar as políticas de repetição (como tentativas de ligação), como sugerido no artigo do MSDN [processamento de erros transitórios][lnk-transient-faults].
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a>Agenda de tarefas para chamar um método direto e enviar atualizações ao dispositivo duplo

Nesta secção, vai criar uma aplicação de consola do .NET (utilizando c#) utiliza as tarefas para chamar o **LockDoor** método direto e enviar atualizações da propriedade pretendido para vários dispositivos.

1. No Visual Studio, adicione um projeto Visual C# no Ambiente de Trabalho Clássico do Windows à atual solução, utilizando o modelo de projeto **Aplicação de Consola**. Nomeie o projeto **ScheduleJob**.

    ![Novo projeto do Visual C# no Ambiente de Trabalho Clássico do Windows][img-createapp]

1. No Explorador de soluções, clique com botão direito do **ScheduleJob** projeto e, em seguida, clique em **gerir pacotes NuGet...** .

1. Na janela **Gestor de Pacote NuGet**, selecione **Procurar**, pesquise **microsoft.azure.devices**, selecione **Instalar** para instalar o pacote **Microsoft.Azure.Devices** e aceite os termos de utilização. Este passo transfere, instala e adiciona uma referência para o [SDK do serviço do Azure IoT] [ lnk-nuget-service-sdk] NuGet pacote e as respetivas dependências.

    ![Janela Gestor de Pacote NuGet][img-servicenuget]

1. Adicione as seguinte declarações `using` na parte superior do ficheiro **Program.cs**:
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. Adicione o seguinte `using` instrução se não estiver já presente nas instruções predefinido.

    ```csharp
    using System.Threading;
    using System.Threading.Tasks;
    ```

1. Adicione os seguintes campos à classe **Programa**. Substitua os marcadores de posição pela cadeia de ligação do IoT Hub para o hub que criou na secção anterior e o nome do seu dispositivo.

    ```csharp
    static string connString = "<yourIotHubConnectionString>";
    static string deviceId = "<yourDeviceId>";
    ```

1. Adicione o seguinte método à classe **Programa**:

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. Adicione o seguinte método à classe **Programa**:

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("LockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));
       
        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            $"DeviceId IN ['{deviceId}']",
            directMethod,
            DateTime.UtcNow,
            (long)TimeSpan.FromMinutes(2).TotalSeconds);

        Console.WriteLine("Started Method Job");
    }
    ```

1. Adicionar outro método para o **programa** classe:

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        Twin twin = new Twin(deviceId);
        twin.Tags = new TwinCollection();
        twin.Tags["Building"] = "43";
        twin.Tags["Floor"] = "3";
        twin.ETag = "*";

        twin.Properties.Desired["LocationUpdate"] = DateTime.UtcNow;

        JobResponse createJobResponse = jobClient.ScheduleTwinUpdateAsync(
            jobId,
            $"DeviceId IN ['{deviceId}']", 
            twin, 
            DateTime.UtcNow, 
            (long)TimeSpan.FromMinutes(2).TotalSeconds).Result;

        Console.WriteLine("Started Twin Update Job");
    }
    ```

    > [!NOTE]
    > Para obter mais informações sobre a sintaxe de consulta, consulte [idioma de consulta do IoT Hub][lnk-query].
    > 

1. Por fim, adicione as seguintes linhas ao método **Main**:

    ```csharp
    Console.WriteLine("Press ENTER to start running jobs.");
    Console.ReadLine();

    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

1. Guarde o trabalho e criar a sua solução. 


## <a name="run-the-apps"></a>Executar as aplicações

Já está pronto para executar as aplicações.

1. No Visual Studio Explorador de soluções, faça duplo clique sua solução e, em seguida, clique em **criar**. **Vários projetos de arranque**. Certifique-se `SimulateDeviceMethods` está no topo da lista seguido `ScheduleJob`. Definir as respetivas ações **iniciar** e clique em **OK**.

1. Execute os projetos ao clicar em **iniciar** ou vá para o **depurar** menu e **iniciar depuração**.

1. Pode ver o resultado do dispositivo e aplicações de back-end.

    ![Executar as aplicações para agendar tarefas][img-schedulejobs]


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, uma tarefa que utilizou para agendar um método direto para um dispositivo e a atualização das propriedades do dispositivo duplo.

Para continuar a introdução ao IoT Hub e padrões de gestão de dispositivos como remota através da atualização de firmware ondas eletromagnéticas, leia [Tutorial: como efetuar uma atualização de firmware][lnk-fwupdate].

Para saber mais sobre a implementação AI em dispositivos de limite com limite de IoT do Azure, consulte [introdução ao IoT Edge][lnk-iot-edge].

<!-- images -->
[img-createdeviceapp]: ./media/iot-hub-csharp-csharp-schedule-jobs/create-device-app.png
[img-clientnuget]: ./media/iot-hub-csharp-csharp-schedule-jobs/device-app-nuget.png
[img-servicenuget]: media/iot-hub-csharp-csharp-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-csharp-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-csharp-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-csharp-csharp-twin-getstarted.md
[lnk-twin-props]: iot-hub-csharp-csharp-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-csharp-csharp-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-csharp-csharp-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://docs.microsoft.com/azure/architecture/best-practices/transient-faults
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-query]: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-query-language
