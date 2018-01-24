---
title: Encaminhamento de mensagens com o IoT Hub do Azure (Python) | Microsoft Docs
description: "Como processar mensagens de dispositivo para a nuvem do IoT Hub do Azure utilizando regras de encaminhamento e os pontos finais personalizados para emitir mensagens a outros serviços de back-end."
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: v-masebo
ms.openlocfilehash: 24c556c8ea585a522c890dab16ae9fb96dc51c22
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/23/2018
---
# <a name="routing-messages-with-iot-hub-python"></a>Encaminhamento de mensagens com o IoT Hub (Python)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Este tutorial baseia-se a [introdução ao IoT Hub] tutorial.  Tutorial:

* Mostra como utilizar as regras de encaminhamento para emitir mensagens do dispositivo para nuvem de uma forma fácil, baseadas na configuração.
* Ilustra a isolar mensagens interativas que requerem ação imediata da solução de back-end para processamento adicional.  Por exemplo, um dispositivo pode enviar uma mensagem de alarme que aciona a inserir um pedido de um sistema CRM.  Em contrapartida, as mensagens de ponto de dados, tais como a telemetria de temperatura, feed para um motor de análise.

No final deste tutorial, execute três aplicações de consola do Python:

* **SimulatedDevice.py**, uma versão modificada da aplicação criada no [introdução ao IoT Hub] tutorial, envia mensagens do dispositivo para a nuvem de ponto de dados de todas as mensagens de dispositivo para a nuvem segundo e interativa por aleatório intervalo. Esta aplicação utiliza o protocolo MQTT para comunicar com IoT Hub.
* **ReadCriticalQueue.py** remove as mensagens de evento crítico da fila de barramento de serviço ligado ao IoT hub.

> [!NOTE]
> IoT Hub tem suporte SDK para várias plataformas de dispositivos e idiomas, incluindo C, Java e JavaScript. Para obter instruções sobre como substituir o dispositivo neste tutorial com um dispositivo físico e como ligar dispositivos para um IoT Hub, consulte o [Centro de programadores do IoT do Azure].

Para concluir este tutorial, precisa do seguinte:

* Uma versão completa do trabalho do [introdução ao IoT Hub] tutorial.
* [Python 2.x ou 3.x][lnk-python-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Quando lhe for pedido durante a instalação, confirme que adiciona Python à variável de ambiente específica da sua plataforma. Se estiver a utilizar Python 2.x, poderá ter de [instalar ou atualizar o *pip*, o sistema de gestão de pacotes de Python][lnk-install-pip].
* Se estiver a utilizar o SO Windows, o [pacote redistribuível Visual C++][lnk-visual-c-redist], para permitir a utilização de DLLs nativas de Python.
* [Node.js 4.0 ou posterior][lnk-node-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Esta é necessária para instalar a [ferramenta Explorador do Hub IoT][lnk-iot-hub-explorer].
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

Também recomendamos ler sobre [Storage do Azure] e [Service Bus do Azure].


## <a name="send-interactive-messages-from-a-device-app"></a>Enviar mensagens interativas de uma aplicação de dispositivo
Nesta secção, modificar a aplicação de dispositivo que criou no [introdução ao IoT Hub] tutorial ocasionalmente enviar mensagens que necessitam de processamento de imediato.

1. Utilize um editor de texto para abrir o **SimulatedDevice.py** ficheiro. Este ficheiro contém o código para o **SimulatedDevice** aplicação que criou no [introdução ao IoT Hub] tutorial.

2. Substitua o **iothub_client_telemetry_sample_run** função com o seguinte código:

    ```python
    def iothub_client_telemetry_sample_run():

    try:
        client = iothub_client_init()
        print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
        message_counter = 0

        while True:
            random_seed = random.random()
            msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random_seed * 4 + 2))
            # messages can be encoded as string or bytearray
            if (message_counter & 1) == 1:
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
            else:
                message = IoTHubMessage(msg_txt_formatted)
            # optional: assign ids
            message.message_id = "message_%d" % message_counter
            message.correlation_id = "correlation_%d" % message_counter
            # optional: assign properties
            prop_map = message.properties()
            if random_seed > .5:
                if random_seed > .8:
                    prop_map.add("level", 'critical')
                else:
                    prop_map.add("level", 'storage')

            client.send_event_async(message, send_confirmation_callback, message_counter)
            print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

            status = client.get_send_status()
            print ( "Send status: %s" % status )
            time.sleep(30)

            status = client.get_send_status()
            print ( "Send status: %s" % status )

            message_counter += 1

    except IoTHubError as iothub_error:
        print ( "Unexpected error %s from IoTHub" % iothub_error )
        return
    except KeyboardInterrupt:
        print ( "IoTHubClient sample stopped" )
    ```
   
    Este método aleatoriamente adiciona a propriedade `"level": "critical"` e `"level": "storage"` às mensagens enviadas pelo dispositivo, que simula uma mensagem que necessita de uma ação imediata, o back-end da aplicação ou que têm ser armazenados permanentemente. A aplicação suporta encaminhamento de mensagens com base no corpo da mensagem.
   
   > [!NOTE]
   > Pode utilizar as propriedades da mensagem para encaminhar mensagens para vários cenários, incluindo o processamento de frio caminho, para além do exemplo de caminho frequente apresentado aqui.

1. Guarde e feche o ficheiro **SimulatedDevice.py**.

    > [!NOTE]
    > Com vista à, simplicidade, este tutorial não implementa nenhuma política de repetição. No código de produção, deve implementar uma política de repetição como término exponencial, como sugerido no artigo do MSDN [processamento de erros transitórios].


## <a name="add-queues-to-your-iot-hub-and-route-messages-to-them"></a>Adicionar filas ao seu IoT hub e rota mensagens aos mesmos

Nesta secção, criar uma fila de barramento de serviço e uma conta de armazenamento, ligue-os ao seu IoT hub e configurar o seu IoT hub para enviar mensagens para a fila com base na presença de uma propriedade de mensagem e todas as mensagens para a conta de armazenamento. Para obter mais informações sobre como processar mensagens de filas do Service Bus, consulte [introdução às filas] [ lnk-sb-queues-node] e como gerir o armazenamento, consulte [introdução ao Storage do Azure] [Storage do azure].

1. Criar uma fila do Service Bus, conforme descrito em [introdução às filas][lnk-sb-queues-node]. Tome nota do nome do espaço de nomes e a fila.

1. No portal do Azure, abra o seu IoT hub e clique em **pontos finais**.

    ![Pontos finais do IoT hub][30]

1. No **pontos finais** painel, clique em **adicionar** na parte superior para adicionar uma fila ao seu IoT hub. O ponto final de nome **CriticalQueue** e utilizar listas pendentes para selecionar **fila do Service Bus**, o espaço de nomes de barramento de serviço em que reside a sua fila e o nome da sua fila. Quando tiver terminado, clique em **OK** na parte inferior.  

    ![Adicionar um ponto final][31]

1. Agora, clique em **rotas** no seu IoT Hub. Clique em **adicionar** na parte superior do painel para criar uma regra de encaminhamento encaminha mensagens para a fila acabou de adicionar. 

    ![Adicionar uma rota][34]

    Introduza um nome e selecione **mensagens do dispositivo** como origem de dados. Escolha **CriticalQueue** como um ponto final personalizado como o encaminhamento de regras de ponto final e introduza `level="critical"` como a cadeia de consulta. Clique em **guardar** na parte inferior.

    ![Detalhes de rota][32]

    Certifique-se a rota de contingência está definida como **ON**. Esta definição é a configuração predefinida de um hub IoT.

    ![Rota de contingência][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(Opcional) Ler a partir do ponto final de fila

Nesta secção, crie uma aplicação de consola de Python que lê mensagens de evento crítico de barramento de serviço de IoT. Ver mais informações no [introdução às filas][lnk-sb-queues-node]. 

1. Abra uma linha de comandos nova e instale o SDK de Dispositivo do Hub IoT do Azure para Python, da seguinte forma. Após a instalação, feche a linha de comandos.

    ```cmd/sh
    pip install azure-servicebus
    ```

    > [!NOTE]
    > Para problemas de instalar o **servicebus do azure** pacote ou para obter mais opções de instalação, consulte o [pacote do Python servicebus do azure][lnk-python-service-bus].

1. Crie um ficheiro denominado **ReadCriticalQueue.py**. Abra-o num editor/IDE para Python à sua escolha (por exemplo, o IDLE).

1. Adicione o seguinte código para importar os módulos necessários do dispositivo SDK:

    ```python
    from azure.servicebus import ServiceBusService, Message, Queue
    ```

1. Adicione o seguinte código e substitua os marcadores de posição de dados de ligação para o barramento de serviço:

    ```python
    SERVICE_BUS_NAME = {serviceBusName}
    SHARED_ACCESS_POLICY_NAME = {sharedAccessPolicyName}
    SHARED_ACCESS_POLICY_KEY_VALUE = {sharedAccessPolicyKeyValue}
    QUEUE_NAME = {queueName}    
    ```

5. Adicione o seguinte código para ligar a e ler o barramento de serviço: 

    ```python
    def setup_client():
        bus_service = ServiceBusService(
        service_namespace=SERVICE_BUS_NAME,
        shared_access_key_name=SHARED_ACCESS_POLICY_NAME,
        shared_access_key_value=SHARED_ACCESS_POLICY_KEY_VALUE)

        while True:
            msg = bus_service.receive_queue_message(QUEUE_NAME, peek_lock=False)
            print(msg.body)
    ```

1. Por fim, adicione a função principal. 

    ```python
    if __name__ == '__main__':
        setup_client()
    ```

1. Guarde e feche o **ReadCriticalQueue.py** ficheiro. Pode agora executar as aplicações.


## <a name="run-the-applications"></a>Executar as aplicações

Agora pode executar as aplicações.

1. Abra uma linha de comandos e instale o Explorador do Hub IoT. 

    ```cmd/sh
    npm install -g iothub-explorer
    ```

1. Execute o comando seguinte na linha de comandos para começar a monitorizar as mensagens do dispositivo para a cloud a partir do seu dispositivo. Utilize a cadeia de ligação do hub IoT no marcador de posição a seguir a `--login`.

    ```cmd/sh
    iothub-explorer monitor-events [deviceId] --login "[IoTHub connection string]"
    ```

1. Abra uma nova linha de comandos e navegue para o diretório que contém o ficheiro **SimulatedDevice.py**.

1. Execute o ficheiro **SimulatedDevice.py**, que envia periodicamente dados de telemetria para o seu hub IoT. 
   
    ```cmd/sh
    python SimulatedDevice.py
    ```

1. Para executar o **ReadCriticalQueue** aplicação, numa linha de comandos ou shell, navegue para **ReadCriticalQueue.py** do ficheiro e execute o seguinte comando:

   ```cmd/sh
   python ReadCriticalQueue.py
   ```

1. Execute o Explorador do Hub IoT da secção anterior para ver as mensagens do dispositivo na linha de comandos. Observar o `critical` mensagens no **ReadCriticalQueue** aplicação.

    ![Mensagens do dispositivo para a cloud Python][2]


## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(Opcional) Adicionar o contentor de armazenamento ao seu IoT hub e rota mensagens ao mesmo

Nesta secção, criar uma conta de armazenamento, ligue-o ao seu IoT hub e configurar o seu IoT hub para enviar mensagens para a conta com base na presença de uma propriedade na mensagem. Para obter mais informações sobre como gerir o armazenamento, consulte [introdução ao Storage do Azure][Storage do Azure].

 > [!NOTE]
   > Contas do IoT Hub criadas o _F1 gratuito_ camada é limitado a um **Endpoint**. Se não está limitado a um **Endpoint**, pode configurar o **StorageContainer** para além de **CriticalQueue** e executar ambos os simulatneously.

1. Criar uma conta de armazenamento, conforme descrito em [documentação do Storage do Azure][lnk-storage]. Anote o nome da conta.

2. No portal do Azure, abra o seu IoT hub e clique em **pontos finais**.

3. No **pontos finais** painel, selecione o **CriticalQueue** ponto final e clique em **eliminar**. Clique em **Sim**e, em seguida, clique em **adicionar**. O ponto final de nome **StorageContainer** e utilizar listas pendentes para selecionar **contentor de armazenamento do Azure**e criar um **conta de armazenamento** e um **armazenamento contentor**.  Tome nota dos nomes.  Quando tiver terminado, clique em **OK** na parte inferior. 

 > [!NOTE]
   > Contas do IoT Hub criadas o _F1 gratuito_ camada é limitado a um **Endpoint**. Se não está limitado a um **Endpoint**, não terá de eliminar o **CriticalQueue**.

4. Clique em **rotas** no seu IoT Hub. Clique em **adicionar** na parte superior do painel para criar uma regra de encaminhamento encaminha mensagens para a fila acabou de adicionar. Selecione **mensagens do dispositivo** como origem de dados. Introduza `level="storage"` como condição e escolha **StorageContainer** como um ponto final personalizado como o encaminhamento ponto final da regra. Clique em **guardar** na parte inferior.  

    Certifique-se a rota de contingência está definida como **ON**. Esta definição é a configuração predefinida de um hub IoT.

1. Certifique-se a aplicação anterior **SimulatedDevice.py** ainda está em execução. 

1. No Portal do Azure, aceda à sua conta do storage em **serviço Blob**, clique em **procurar blobs...** .  Selecione o contentor, navegue para e clique no ficheiro JSON e clique em **transferir** para ver os dados.


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a fiável emitir mensagens do dispositivo para nuvem utilizando a funcionalidade de encaminhamento de mensagens do IoT Hub.

Para ver os exemplos de soluções ponto-a-ponto completas que utilizam o IoT Hub, consulte [Azure IoT Suite][lnk-suite].

Para obter mais informações sobre como desenvolver soluções de IoT hub, consulte o [guia para programadores do IoT Hub].

Para saber mais sobre o encaminhamento de mensagens no IoT Hub, veja [enviar e receber mensagens com o IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
[2]: ./media/iot-hub-python-python-process-d2c/output.png

[30]: ./media/iot-hub-python-python-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-python-python-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-python-python-process-d2c/route-creation.png
[33]: ./media/iot-hub-python-python-process-d2c/fallback-route.png
[34]: ./media/iot-hub-python-python-process-d2c/click-routes.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-sb-queues-node]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md

[Storage do Azure]: https://azure.microsoft.com/documentation/services/storage/
[Service Bus do Azure]: https://azure.microsoft.com/documentation/services/service-bus/

[guia para programadores do IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[introdução ao IoT Hub]: iot-hub-python-getstarted.md
[Centro de programadores do IoT do Azure]: https://azure.microsoft.com/develop/iot

[processamento de erros transitórios]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
[lnk-storage]: https://docs.microsoft.com/en-us/azure/storage/
[lnk-python-service-bus]: https://pypi.python.org/pypi/azure-servicebus