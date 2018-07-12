---
title: Atualização de firmware do dispositivo com o Azure IoT Hub (.NET/.NET) | Documentos da Microsoft
description: Como utilizar a gestão de dispositivos no IoT Hub do Azure para iniciar uma atualização de firmware do dispositivo. Utilizar o Azure IoT device SDK para .NET para implementar uma aplicação de dispositivo simulado e o serviço de IoT do Azure SDK para .NET implementar uma aplicação de serviço que aciona a atualização de firmware.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 10/19/2017
ms.author: dobett
ms.openlocfilehash: 1bf7a647ab2fdc175231133b0cfd8abdd51b6d43
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723932"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-netnet"></a>Utilizar a gestão de dispositivos para iniciar uma atualização de firmware do dispositivo (.NET/.NET)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

## <a name="introduction"></a>Introdução
Na [introdução à gestão de dispositivos] [ lnk-dm-getstarted] tutorial, viu como usar o [dispositivo duplo] [ lnk-devtwin] e [métodos diretos ] [ lnk-c2dmethod] primitivos para reiniciar remotamente um dispositivo. Este tutorial utiliza os mesmo primitivos de IoT Hub e mostra-lhe como fazer uma atualização de firmware simulada do ponto-a-ponto.  Esse padrão é usado na implementação de atualização de firmware para o [exemplo de implementação do dispositivo Raspberry Pi][lnk-rpi-implementation].

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este tutorial mostrar-lhe como:

* Criar uma aplicação de consola .NET que chama o **firmwareUpdate** método direto na aplicação do dispositivo simulado através do IoT hub.
* Criar uma aplicação de dispositivo simulado que implementa uma **firmwareUpdate** método direto. Este método inicia um processo de vários estágio que aguarda a transferência da imagem de firmware, transfere a imagem de firmware e, finalmente, aplica-se a imagem de firmware. Durante cada fase da atualização, o dispositivo utiliza as propriedades comunicadas para comunicar o progresso.

No final deste tutorial, terá de uma aplicação de dispositivo de consola .NET (c#) e uma aplicação de back-end de consola .NET (c#):

* **SimulatedDeviceFwUpdate**, que liga ao seu hub IoT com a identidade de dispositivo que criou anteriormente, recebe o **firmwareUpdate** método, é executada através de um processo com múltiplos estado para simular uma atualização de firmware direto incluindo: a aguardar que o download da imagem, transferir a nova imagem e, finalmente, a aplicação da imagem.

* **TriggerFWUpdate**, que usa o SDK do serviço para invocar remotamente o **firmwareUpdate** direto método no dispositivo simulado, exibe a resposta, e periodicamente (cada 500 MS) apresenta a atualização comunicado Propriedades.

Para concluir este tutorial, precisa do seguinte:

* Visual Studio 2015 ou Visual Studio 2017.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

Siga os [introdução à gestão de dispositivos](iot-hub-csharp-csharp-device-management-get-started.md) artigo para criar o hub IoT e obter a cadeia de ligação do IoT Hub.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Acionar uma atualização de firmware remota no dispositivo com um método direto
Nesta secção, vai criar uma aplicação de consola do .NET (usando a linguagem c#) inicia uma atualização de firmware remoto num dispositivo. A aplicação utiliza um método direto para iniciar a atualização e utiliza consultas de gémeos de dispositivo para obter periodicamente o estado da atualização de firmware do Active Directory.

1. No Visual Studio, adicione um projeto Visual C# no Ambiente de Trabalho Clássico do Windows à atual solução, utilizando o modelo de projeto **Aplicação de Consola**. Nomeie o projeto **TriggerFWUpdate**.

    ![Novo projeto do Visual C# no Ambiente de Trabalho Clássico do Windows][img-createserviceapp]

2. No Solution Explorer, clique com botão direito a **TriggerFWUpdate** projeto e, em seguida, clique em **gerir pacotes NuGet**.
3. Na janela **Gestor de Pacote NuGet**, selecione **Procurar**, pesquise **microsoft.azure.devices**, selecione **Instalar** para instalar o pacote **Microsoft.Azure.Devices** e aceite os termos de utilização. Este procedimento transfere, instala e adiciona uma referência ao pacote NuGet do [SDK do serviço do Azure IoT][lnk-nuget-service-sdk] e às respetivas dependências.

    ![Janela Gestor de Pacote NuGet][img-servicenuget]
4. Adicione as seguinte declarações `using` na parte superior do ficheiro **Program.cs**:

    ```csharp   
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Adicione os seguintes campos à classe **Programa**. Substitua os vários valores de marcador de posição pela cadeia de ligação do IoT Hub para o hub que criou na secção anterior e o ID do seu dispositivo.
   
    ```csharp   
    static RegistryManager registryManager;
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static string targetDevice = "{deviceIdForTargetDevice}";
    ```
        
6. Adicione o seguinte método à classe **Programa**. Este método de consulta o dispositivo duplo para o estado atualizado a cada 500 milissegundos. Escreve para a consola apenas quando o estado, na verdade, foi alterado. Para este exemplo, para evitar consumir mensagens do IoT Hub Extras na sua subscrição, a consulta paradas, quando o dispositivo comunica um Estado de **applyComplete** ou um erro.  
   
    ```csharp   
    public static async Task QueryTwinFWUpdateReported(DateTime startTime)
    {
        DateTime lastUpdated = startTime;

        while (true)
        {
            Twin twin = await registryManager.GetTwinAsync(targetDevice);

            if (twin.Properties.Reported.GetLastUpdated().ToUniversalTime() > lastUpdated.ToUniversalTime())
            {
                lastUpdated = twin.Properties.Reported.GetLastUpdated().ToUniversalTime();
                Console.WriteLine("\n" + twin.Properties.Reported["iothubDM"].ToJson());

                var status = twin.Properties.Reported["iothubDM"]["firmwareUpdate"]["status"].Value;
                if ((status == "downloadFailed") || (status == "applyFailed") || (status == "applyComplete"))
                {
                    Console.WriteLine("\nStop polling.");
                    return;
                }
            }
            await Task.Delay(500);
        }
    }
    ```
        
7. Adicione o seguinte método à classe **Programa**:

    ```csharp   
    public static async Task StartFirmwareUpdate()
    {
        client = ServiceClient.CreateFromConnectionString(connString);
        CloudToDeviceMethod method = new CloudToDeviceMethod("firmwareUpdate");
        method.ResponseTimeout = TimeSpan.FromSeconds(30);
        method.SetPayloadJson(
            @"{
               fwPackageUri : 'https://someurl'
            }");

        CloudToDeviceMethodResult result = await client.InvokeDeviceMethodAsync(targetDevice, method);

        Console.WriteLine("Invoked firmware update on device.");
    }
    ```

8. Por fim, adicione as seguintes linhas para o **Main** método. Isto cria um registo manager para ler o dispositivo duplo com inicia a tarefa de consulta num thread de trabalho e, em seguida, aciona a atualização de firmware.
   
    ```csharp   
    registryManager = RegistryManager.CreateFromConnectionString(connString);

    Task queryTask = Task.Run(() => (QueryTwinFWUpdateReported(DateTime.Now)));

    StartFirmwareUpdate().Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```
        
9. Compilar a solução.

## <a name="create-a-simulated-device-app"></a>Criar uma aplicação de dispositivo simulada
Nesta secção, pode:

* Criar uma aplicação de consola .NET que responde a um método direto chamado pela cloud
* Simular uma atualização de firmware acionada por um serviço de back-end através de um método direto
* Utilize as propriedades comunicadas para ativar as consultas de dispositivo duplo, de forma a identificar os dispositivos e quando concluírem uma atualização de firmware pela última vez

1. No Visual Studio, adicione um projeto Visual C# no Ambiente de Trabalho Clássico do Windows à atual solução, utilizando o modelo de projeto **Aplicação de Consola**. Nomeie o projeto **SimulateDeviceFWUpdate**.
   
    ![Nova aplicação de dispositivo Visual c# Windows clássico][img-createdeviceapp]
    
2. No Solution Explorer, clique com botão direito a **SimulateDeviceFWUpdate** projeto e, em seguida, clique em **gerir pacotes NuGet**.
3. Na **Gestor de pacotes NuGet** janela, selecione **procurar** e procure **microsoft.azure.devices.client**. Selecione **instale** para instalar o **Microsoft.Azure.Devices.Client** empacotamento e a aceitar os termos de utilização. Este procedimento transfere, instala e adiciona uma referência para o [do Azure IoT device SDK] [ lnk-nuget-client-sdk] NuGet pacote e suas dependências.
   
    ![Aplicação de cliente de janela do Gestor de pacotes NuGet][img-clientnuget]
4. Adicione as seguinte declarações `using` na parte superior do ficheiro **Program.cs**:
   
    ```csharp   
    using Newtonsoft.Json.Linq;
    using Microsoft.Azure.Devices.Client;
    using Microsoft.Azure.Devices.Shared;
    ```

5. Adicione os seguintes campos à classe **Programa**. Substitua o valor de marcador de posição pela cadeia de ligação de dispositivo que anotou no **criar uma identidade de dispositivo** secção.
   
    ```csharp   
    static string DeviceConnectionString = "HostName=<yourIotHubName>.azure-devices.net;DeviceId=<yourIotDeviceName>;SharedAccessKey=<yourIotDeviceAccessKey>";
    static DeviceClient Client = null;
    ```

6. Adicione o seguinte método para comunicar o estado volta para a cloud através do dispositivo duplo: 

    ```csharp   
    static async Task reportFwUpdateThroughTwin(Twin twin, TwinCollection fwUpdateValue)
    {
        try
        {
            TwinCollection patch = new TwinCollection();
            TwinCollection iothubDM = new TwinCollection();

            iothubDM["firmwareUpdate"] = fwUpdateValue;
            patch["iothubDM"] = iothubDM;

            await Client.UpdateReportedPropertiesAsync(patch);
            Console.WriteLine("Twin state reported: {0}", fwUpdateValue["status"]);
        }
        catch 
        {
            Console.WriteLine("Error updating device twin");
            throw;
        }
    }
    ```

7. Adicione o seguinte método para simular a transferência da imagem de firmware:
        
    ```csharp   
    static async Task<byte[]> simulateDownloadImage(string imageUrl)
    {
        var image = "[fake image data]";

        Console.WriteLine("Downloading image from " + imageUrl);

        await Task.Delay(4000);

        return Encoding.ASCII.GetBytes(image);
            
    }
    ```

8. Adicione o seguinte método para simular a aplicar a imagem de firmware para o dispositivo:
        
    ```csharp   
    static async Task simulateApplyImage(byte[] imageData)
    {
        if (imageData == null)
        {
            throw new ArgumentNullException();
        }

        await Task.Delay(4000);

    }
    ```
 
9.  Adicione o seguinte método para simular a aguardar para transferir a imagem de firmware. Atualizar estado para **aguardando** e desmarque as outras propriedades de atualização de firmware no duplo. Estas propriedades estão desmarcadas para remover quaisquer valores existentes de atualizações de firmware anterior. Isso é necessário porque as propriedades comunicadas são enviadas como uma operação de correção (delta) e não uma operação PUT (um conjunto completo de propriedades que substitui todos os valores anteriores). Normalmente, os dispositivos são informados sobre uma atualização disponível e um administrador define as causas de política para o dispositivo começar a transferir e aplicar a atualização. Esta função é onde deve ser executada a lógica para ativar essa política. 
        
    ```csharp   
    static async Task waitToDownload(Twin twin, string fwUpdateUri)
    {
        var now = DateTime.Now;
        TwinCollection status = new TwinCollection();
        status["fwPackageUri"] = fwUpdateUri;
        status["status"] = "waiting";
        status["error"] = null;
        status["startedWaitingTime"] = DateTime.Now;
        status["downloadCompleteTime"] = null;
        status["startedApplyingImage"] = null;
        status["lastFirmwareUpdate"] = null;

        await reportFwUpdateThroughTwin(twin, status);

        await Task.Delay(2000);
    }
    ```

10. Adicione o seguinte método para efetuar a transferência. Ele atualiza o estado **baixar** através do dispositivo duplo, chama o método de transferência de simular e comunica um Estado de **downloadComplete** ou **downloadFailed** por meio do duplo consoante os resultados da operação de transferência. 
        
    ```csharp   
    static async Task<byte[]> downloadImage(Twin twin, string fwUpdateUri)
    {
        try
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloading";
            await reportFwUpdateThroughTwin(twin, statusUpdate);

            byte[] imageData = await simulateDownloadImage(fwUpdateUri);

            statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloadComplete";
            statusUpdate["downloadCompleteTime"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            return imageData;
        }
        catch (Exception ex)
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "downloadFailed";
            statusUpdate["error"] = new TwinCollection();
            statusUpdate["error"]["code"] = ex.GetType().ToString();
            statusUpdate["error"]["message"] = ex.Message;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            throw;
        }
    }
    ```

11. Adicione o seguinte método para aplicar a imagem. Ele atualiza o estado para **aplicar** por meio do dispositivo duplo, chamadas a simular aplicar imagem método e o estado das atualizações para **applyComplete** ou **applyFailed** através do duplo consoante os resultados da operação de aplicação. 
        
    ```csharp   
    static async Task applyImage(Twin twin, byte[] imageData)
    {
        try
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applying";
            statusUpdate["startedApplyingImage"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);

            await simulateApplyImage(imageData);

            statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applyComplete";
            statusUpdate["lastFirmwareUpdate"] = DateTime.Now;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
        }
        catch (Exception ex)
        {
            TwinCollection statusUpdate = new TwinCollection();
            statusUpdate["status"] = "applyFailed";
            statusUpdate["error"] = new TwinCollection();
            statusUpdate["error"]["code"] = ex.GetType().ToString();
            statusUpdate["error"]["message"] = ex.Message;
            await reportFwUpdateThroughTwin(twin, statusUpdate);
            throw;
        }
    }
    ```

12. Adicione o seguinte método para sequenciar a operação de atualização de firmware de aguardar o processamento para transferir a imagem por meio de aplicar a imagem no dispositivo:
        
    ```csharp   
    static async Task doUpdate(string fwUpdateUrl)
    {
        try
        {
            Twin twin = await Client.GetTwinAsync();
            await waitToDownload(twin, fwUpdateUrl);
            byte[] imageData = await downloadImage(twin, fwUpdateUrl);
            await applyImage(twin, imageData);
        }
        catch (Exception ex)
        {
            Console.WriteLine();
            Console.WriteLine("Error during update: {0}", ex.Message);
        }
    }
    ```

13. Adicione o seguinte método para lidar com o **updateFirmware** direcionar o método a partir da cloud. Extrai o URL para a atualização de firmware do payload de mensagem e transmite-o para o **doUpdate** tarefa, que começa em outro thread do threadpool. Ele, em seguida, devolve imediatamente a resposta do método para a cloud.
        
    ```csharp   
    static Task<MethodResponse> onFirmwareUpdate(MethodRequest methodRequest, object userContext)
    {
        string fwUpdateUrl = (string)JObject.Parse(methodRequest.DataAsJson)["fwPackageUri"];
        Console.WriteLine("\nMethod: {0} triggered by service, URI is: {1}", methodRequest.Name, fwUpdateUrl);

        Task updateTask = Task.Run(() => (doUpdate(fwUpdateUrl)));

        string result = "'FirmwareUpdate started.'";
        return Task.FromResult(new MethodResponse(Encoding.UTF8.GetBytes(result), 200));
    }
    ```
> [!NOTE]
> Este método aciona a atualização simulada para ser executado como um **tarefa** e, em seguida, responde de imediato para a chamada de método, informando o serviço que foi iniciada a atualização de firmware. Estado da atualização e de conclusão serão enviados para o serviço através das propriedades comunicadas do dispositivo duplo. Vamos responder para a chamada de método ao iniciar a atualização, em vez de depois da conclusão, porque:
> * Um processo de atualização real é muito provável que demorar mais do que o tempo limite de chamada de método.
> * Um processo de atualização real é muito provável que requerem um reinício, o que teria de reiniciar esta criação de aplicações do **MethodRequest** objeto indisponível. (Atualizar propriedades comunicadas, no entanto, é possível, mesmo após uma reinicialização.) 

14. Por fim, adicione o seguinte código para o **Main** método para abrir a ligação ao seu hub IoT e inicializar o serviço de escuta do método:
   
    ```csharp   
    try
    {
        Console.WriteLine("Connecting to hub");
        Client = DeviceClient.CreateFromConnectionString(DeviceConnectionString, TransportType.Mqtt);
        
        // setup callback for "firmware update" method
        Client.SetMethodHandlerAsync("firmwareUpdate", onFirmwareUpdate, null).Wait();
        Console.WriteLine("Waiting for firmware update direct method call\n Press enter to exit.");
        Console.ReadLine();

        Console.WriteLine("Exiting...");

        // as a good practice, remove the firmware update handler
        Client.SetMethodHandlerAsync("firmwareUpdate", null, null).Wait();
        Client.CloseAsync().Wait();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
15. Compilar a solução.       

> [!NOTE]
> Para facilitar, este tutorial não implementa nenhuma política de repetição. No código de produção, deve implementar as políticas de repetição (como um término exponencial), como sugerido no artigo [Transient Fault Handling (Processamento de Erros Transitórios)][lnk-transient-faults] da MSDN.


## <a name="run-the-apps"></a>Executar as aplicações
Já está pronto para executar as aplicações.
1. Executar a aplicação de dispositivo do .NET **SimulatedDeviceFWUpdate**.  Com o botão direito a **SimulatedDeviceFWUpdate** projeto, selecione **depurar**e, em seguida, selecione **iniciar nova instância**. Deve começar a ouvir as chamadas de método do seu IoT Hub: 

2. Depois do dispositivo está ligado e à espera de invocações de método, executar o .NET **TriggerFWUpdate** aplicação para invocar o método de atualização de firmware no aplicativo do dispositivo simulado. Com o botão direito a **TriggerFWUpdate** projeto, selecione **depurar**e, em seguida, selecione **iniciar nova instância**. Deverá ver atualizar a sequência de escritas no **SimulatedDeviceFWUpdate** consola e também a sequência de comunicadas através das propriedades comunicadas do dispositivo na **TriggerFWUpdate** consola. Tenha em atenção que o processo demora vários segundos a concluir. 
   
    ![Executar a aplicação de serviço e dispositivo][img-combinedrun]


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, utilizou um método direto para acionar uma atualização de firmware remoto num dispositivo e utilizadas as propriedades reportadas para seguir o progresso da atualização de firmware.

Para saber como expandir o seu IoT chama o método de solução e a agenda em vários dispositivos, veja a [agendar e transmitir tarefas] [ lnk-tutorial-jobs] tutorial.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-csharp-firmware-update/servicesdknuget.png
[img-clientnuget]: media/iot-hub-csharp-csharp-firmware-update/clientsdknuget.png
[img-createserviceapp]: media/iot-hub-csharp-csharp-firmware-update/createserviceapp.png
[img-createdeviceapp]: media/iot-hub-csharp-csharp-firmware-update/createdeviceapp.png
[img-combinedrun]: media/iot-hub-csharp-csharp-firmware-update/combinedrun.png

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-csharp-csharp-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-csharp-csharp-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-rpi-implementation]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_client/samples/iothub_client_sample_mqtt_dm/pi_device
[lnk-nuget-client-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
