---
title: Mensagens de cloud para o dispositivo com o Azure IoT Hub (.NET) | Documentos da Microsoft
description: Como enviar mensagens da cloud para dispositivo a um dispositivo de um hub de IoT do Azure com os SDKs IoT do Azure para .NET. Modificar uma aplicação de dispositivo para receber mensagens da cloud para dispositivo e modificar uma aplicação de back-end para enviar as mensagens de cloud-para-dispositivo.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/24/2017
ms.author: elioda
ms.openlocfilehash: e2090977813f5335f2170ff543bde19cd8bf123b
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185799"
---
# <a name="send-messages-from-the-cloud-to-your-device-with-iot-hub-net"></a>Enviar mensagens a partir da cloud para o seu dispositivo com o Hub IoT (.NET)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Introdução
O IoT Hub do Azure é um serviço totalmente gerido que ajuda a habilitar comunicações bidirecionais fiáveis e seguras entre milhões de dispositivos e uma solução de back-end. O [introdução ao IoT Hub] tutorial mostra como criar um hub IoT, aprovisionar uma identidade de dispositivo no mesmo e código de uma aplicação de dispositivo que envia mensagens do dispositivo para a cloud.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este tutorial baseia-se nas [introdução ao IoT Hub]. Mostra-lhe como para:

* Da sua solução back-end, envie mensagens de cloud-para-dispositivo para um único dispositivo através do IoT Hub.
* Receba mensagens da cloud para o dispositivo num dispositivo.
* Da sua solução back-end, pedir confirmação de entrega (*comentários*) para mensagens enviadas para um dispositivo do IoT Hub.

Pode encontrar mais informações sobre mensagens de cloud para o dispositivo na [Guia do programador do IoT Hub][IoT Hub developer guide - C2D].

No final deste tutorial, executa duas aplicações de consola .NET:

* **SimulatedDevice**, uma versão modificada da aplicação que criou no [introdução ao IoT Hub], que liga ao seu hub IoT e recebe mensagens de cloud-para-dispositivo.
* **SendCloudToDevice**, que envia uma mensagem de cloud-para-dispositivo para a aplicação de dispositivo através do IoT Hub e, em seguida, recebe a confirmação de entrega.

> [!NOTE]
> IoT Hub tem suporte SDK para muitas plataformas de dispositivos e linguagens (incluindo C, Java e Javascript) por meio [SDKs de dispositivo de IoT do Azure]. Para obter instruções passo a passo sobre como ligar o seu dispositivo ao código do tutorial e, em geral, hub IoT do Azure, consulte a [guia para programadores do IoT Hub].
> 
> 

Para concluir este tutorial, precisa do seguinte:

* Visual Studio 2015 ou Visual Studio 2017
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

## <a name="receive-messages-in-the-device-app"></a>Receber mensagens na aplicação do dispositivo
Nesta secção, vai modificar a aplicação de dispositivo que criou no [introdução ao IoT Hub] para receber mensagens da cloud para dispositivo do IoT hub.

1. No Visual Studio, no **SimulatedDevice** do projeto, adicione o seguinte método para o **programa** classe.
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    O `ReceiveAsync` método assíncrona retorna a mensagem recebida no momento em que ele é recebido pelo dispositivo. Ele retorna *nulo* após um período de tempo limite especificáveis (neste caso, é utilizada a predefinição de um minuto). Quando a aplicação recebe uma *nulo*, ele deve continuar a aguardar para novas mensagens. Este requisito é o motivo para o `if (receivedMessage == null) continue` linha.
   
    A chamada para `CompleteAsync()` notifica o IoT Hub de que a mensagem foi processada com êxito. A mensagem pode ser removida com segurança a partir da fila de dispositivo. Se ocorreu algo que impediu que a aplicação de dispositivo concluir o processamento da mensagem, o IoT Hub disponibiliza novamente. Em seguida, é importante é que a mensagem lógica no aplicativo do dispositivo de processamento *idempotentes*, para que a mesma mensagem a receber várias vezes produz o mesmo resultado. Um aplicativo pode também temporariamente abandonar uma mensagem, o que resulta no hub IoT, mantendo a mensagem na fila para futuro consumo. Em alternativa, o aplicativo pode rejeitar uma mensagem, que remove permanentemente a mensagem da fila. Para obter mais informações sobre o ciclo de vida de mensagem da cloud para dispositivo, consulte a [Guia do programador do IoT Hub][IoT Hub developer guide - C2D].
   
   > [!NOTE]
   > Quando utilizar HTTPS em vez de MQTT ou AMQP como transporte, o `ReceiveAsync` método retorna imediatamente. O padrão de suporte para mensagens na cloud para o dispositivo com HTTPS é conectados intermitentemente dispositivos que verificarem para mensagens com pouca frequência (menos de 25 minutos). Emitir HTTPS mais recebe os resultados no IoT Hub, limitar os pedidos. Para obter mais informações sobre as diferenças entre o suporte para MQTT, AMQP e HTTPS e a limitação do IoT Hub, consulte a [Guia do programador do IoT Hub][IoT Hub developer guide - C2D].
   > 
   > 
2. Adicione o seguinte método na **Main** método, logo antes do `Console.ReadLine()` linha:
   
        ReceiveC2dAsync();

> [!NOTE]
> Para simplificar, este tutorial não implementa nenhuma política de repetição. No código de produção, deve implementar as políticas de repetição (como um término exponencial), como sugerido no artigo do MSDN [Processamento de falhas transitórias].
> 
> 

## <a name="send-a-cloud-to-device-message"></a>Enviar uma mensagem de cloud-para-dispositivo
Nesta secção, escrever uma aplicação de consola .NET que envia mensagens de cloud-para-dispositivo para a aplicação de dispositivo.

1. A solução atual do Visual Studio, criar um projeto do Visual c# de aplicação ambiente de trabalho com o **aplicação de consola** modelo de projeto. Nomeie o projeto **SendCloudToDevice**.
   
    ![Novo projeto no Visual Studio][20]
2. No Solution Explorer, clique com o botão direito a solução e, em seguida, clique em **gerir pacotes NuGet para solução de...** . 
   
    Esta ação abre o **gerir pacotes NuGet** janela.
3. Procure **Microsoft.Azure.Devices**, clique em **instalar**e aceite os termos de utilização. 
   
    Isto transfere, instala e adiciona uma referência para o [Pacote de NuGet do SDK do serviço de IoT do Azure].

4. Adicione a seguinte declaração `using` na parte superior do ficheiro **Program.cs**:
   
        using Microsoft.Azure.Devices;
5. Adicione os seguintes campos à classe **Programa**. Substitua o valor do marcador de posição pela cadeia de ligação do hub IoT partir [introdução ao IoT Hub]:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Adicione o seguinte método à classe **Programa**:
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    Este método envia uma nova mensagem de cloud-para-dispositivo para o dispositivo com o ID, `myFirstDevice`. Alterar este parâmetro, apenas se a modificação da utilizada na [introdução ao IoT Hub].
7. Por fim, adicione as seguintes linhas ao método **Main**:
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. A partir do Visual Studio, sua solução com o botão direito e, selecione **definir projetos de arranque...** . Selecione **vários projetos de arranque**, em seguida, selecione a **iniciar** ação para **ReadDeviceToCloudMessages**, **SimulatedDevice**, e **SendCloudToDevice**.
9. Prima **F5**. Todas as três aplicações devem começar. Selecione o **SendCloudToDevice** windows e prima **Enter**. Deverá ver a mensagem a ser recebida pela aplicação de dispositivo.
   
   ![Mensagem de recebimento de aplicação][21]

## <a name="receive-delivery-feedback"></a>Receber comentários de entrega
É possível para as confirmações de entrega (ou expiração) do pedido do IoT Hub para cada mensagem de cloud-para-dispositivo. Esta opção permite que a solução de back-end informar facilmente a lógica de repetição ou de compensação. Para obter mais informações sobre os comentários de cloud para o dispositivo, consulte a [Guia do programador do IoT Hub][IoT Hub developer guide - C2D].

Nesta secção, modificar os **SendCloudToDevice** aplicação para solicitar os comentários e recebê-la a partir do IoT Hub.

1. No Visual Studio, no **SendCloudToDevice** do projeto, adicione o seguinte método para o **programa** classe.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    Tenha em atenção que adrão receive é o mesmo utilizado para receber mensagens da cloud para dispositivo a partir da aplicação de dispositivo.
2. Adicione o seguinte método na **Main** método, certo após o `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` linha:
   
        ReceiveFeedbackAsync();
3. Para solicitar os comentários para o fornecimento de sua mensagem de cloud para o dispositivo, tem de especificar uma propriedade no **SendCloudToDeviceMessageAsync** método. Adicione a seguinte linha, logo após o `var commandMessage = new Message(...);` linha:
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. Executar as aplicações, premindo **F5**. Deverá ver todas as três aplicações de iniciar. Selecione o **SendCloudToDevice** windows e prima **Enter**. Deverá ver a mensagem a ser recebida pela aplicação de dispositivo e, após alguns segundos, a mensagem de comentários a ser recebida pelo seu **SendCloudToDevice** aplicação.
   
   ![Mensagem de recebimento de aplicação][22]

> [!NOTE]
> Para simplificar, este tutorial não implementa nenhuma política de repetição. No código de produção, deve implementar as políticas de repetição (como um término exponencial), como sugerido no artigo do MSDN [Processamento de falhas transitórias].
> 
> 

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a enviar e receber mensagens da cloud para o dispositivo. 

Para ver exemplos de soluções ponto-a-ponto completas que utilizam o IoT Hub, veja [Acelerador de solução monitorização remota do IoT do Azure].

Para saber mais sobre o desenvolvimento de soluções com o IoT Hub, veja a [guia para programadores do IoT Hub].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Pacote de NuGet do SDK do serviço de IoT do Azure]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Processamento de falhas transitórias]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md

[guia para programadores do IoT Hub]: iot-hub-devguide.md
[Introdução ao IoT Hub]: quickstart-send-telemetry-dotnet.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Acelerador de solução monitorização remota do IoT do Azure]: https://docs.microsoft.com/azure/iot-suite/
[SDKs de dispositivo de IoT do Azure]: iot-hub-devguide-sdks.md