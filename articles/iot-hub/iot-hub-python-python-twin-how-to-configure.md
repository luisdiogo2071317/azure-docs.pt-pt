---
title: Utilize as propriedades do IoT Hub do Azure dispositivo duplo (Python) | Microsoft Docs
description: Como utilizar dispositivos duplos do IoT Hub do Azure para configurar dispositivos. Utilize os SDKs IoT do Azure para Python para implementar uma aplicação de dispositivo simulada e uma aplicação de serviço que modifica uma configuração de dispositivo utilizando um dispositivo duplo.
services: iot-hub
documentationcenter: .net
author: kgremban
manager: timlt
editor: ''
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2018
ms.author: kgremban
ms.openlocfilehash: 0ba614ab26caad31a045849d85a1abcbb5a9f818
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="use-desired-properties-to-configure-devices-python"></a>Utilize as propriedades do pretendido para configurar dispositivos (Python)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

No final deste tutorial, terá de duas aplicações de consola do Python:

* **SimulateDeviceConfiguration.py**, uma aplicação de dispositivo simulado que aguarda uma atualização da configuração pretendida e reporta o estado de um processo de atualização da configuração simulada.
* **SetDesiredConfigurationAndQuery.py**, uma aplicação de back-end do Python, que define a configuração pretendida num dispositivo e consulta o processo de atualização da configuração.

> [!NOTE]
> O artigo [SDKs IoT do Azure] [ lnk-hub-sdks] fornece informações sobre os SDKs IoT do Azure que pode utilizar para criar aplicações de dispositivo e o back-end.
> 
> 

Para concluir este tutorial precisa do seguinte:

* [Python 2.x ou 3.x][lnk-python-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Quando lhe for pedido durante a instalação, confirme que adiciona Python à variável de ambiente específica da sua plataforma. Se estiver a utilizar Python 2.x, poderá ter de [instalar ou atualizar o *pip*, o sistema de gestão de pacotes de Python][lnk-install-pip].
* Se estiver a utilizar o SO Windows, o [pacote redistribuível Visual C++][lnk-visual-c-redist], para permitir a utilização de DLLs nativas de Python.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

Se seguiu o [começar a utilizar dispositivos duplos] [ lnk-twin-tutorial] tutorial, já tiver um IoT hub e uma identidade de dispositivo chamado **myDeviceId**; e poderá avançar para o [criar a aplicação de dispositivo simulado] [ lnk-how-to-configure-createapp] secção.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-simulated-device-app"></a>Criar a aplicação de dispositivo simulada
Nesta secção, vai criar uma aplicação de consola de Python que liga ao seu hub como **myDeviceId**, tem de aguardar durante uma atualização da configuração pretendida e, em seguida, os relatórios de atualizações no processo de atualização de configuração simulada.

1. Instalar o **SDK de dispositivo do Azure IoT Python** utilizando o seguinte comando na sua linha de comandos:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Com um editor de texto, crie um novo **SimulateDeviceConfiguration.py** ficheiro.

1. Adicione o seguinte código para o **SimulateDeviceConfiguration.py** de ficheiros e substitua o **{cadeia de ligação do dispositivo}** marcador de posição pela cadeia de ligação de dispositivo que copiou quando criou o **myDeviceId** identidade de dispositivo:

    ```python   
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0

    TWIN_CALLBACKS = 0
    SEND_REPORTED_STATE_CALLBACKS = 0

    CONFIG_ID = "1"
    TWIN_PAYLOAD = "{\"configId\":" + CONFIG_ID + ",\"sendFrequency\":\"24h\"}"
    ```

    O **cliente** objeto expõe todos os métodos necessários para interagir com dispositivos duplos do dispositivo. Obter código, em seguida, irá expor um processador para a atualização nas propriedades do pretendido, adicione um processador adicional para se certificar de que existe um pedido de alteração da configuração real comparando configIds, que, em seguida, invoca um método que inicia a alteração da configuração . Com vista à, simplicidade, do código anterior utiliza uma predefinição hard-coded para a configuração inicial. Uma aplicação real, provavelmente, seria carregar que a configuração de um local de armazenamento.
   
   > [!IMPORTANT]
   > Eventos de alteração de propriedade pretendido sempre são emitidos uma vez na ligação do dispositivo, certifique-se de que verifique se existe uma alteração real nas propriedades de pretendido antes de efetuar qualquer ação.
   > 

1. Adicione o seguinte código ao fim do **SimulateDeviceConfiguration.py** ficheiro:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global CONFIG_ID

        desired_configId = payload[payload.find("configId")+11:payload.find("configId")+12]
        if CONFIG_ID != desired_configId:
            print ( "" )
            print ( "Reported pending config change: %s" % payload)
        
            desired_sendFrequency = payload[payload.find("sendFrequency")+17:payload.find("sendFrequency")+19]
            print ( "...desired configId: " + desired_configId)
            print ( "...desired sendFrequency: " + desired_sendFrequency)
            new_payload = "{\"configId\":" + desired_configId + ",\"sendFrequency\":\"" + desired_sendFrequency + "\"}"
            CLIENT.send_reported_state(new_payload, len(new_payload), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        
        CONFIG_ID = desired_configId
    
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS
    
        print ( "" )
        print ( "Device twins updated." )
    ```
   
    O **device_twin_callback** método atualizações comunicadas propriedades no objecto de duplo do dispositivo local, com a configuração de atualização de pedido e define o _configId_. Depois de atualizar com êxito o dispositivo duplo, que imprima uma mensagem de actualização. Para poupar largura de banda, propriedades comunicadas são actualizadas, especificando apenas as propriedades de ser modificados (com o nome **TWIN_PAYLOAD** no código acima), em vez de substituir todo o documento.
   
   > [!NOTE]
   > Este tutorial não simular nenhum comportamento de atualizações de configuração em simultâneo. Alguns processos de atualização de configuração poderão acomodar as alterações de configuração de destino enquanto a atualização está em execução, outros poderão ter de fila-los e outros rejeitá-las com uma condição de erro. Certifique-se a ter em consideração o comportamento pretendido para o processo de configuração específica e adicione a lógica adequada antes de iniciar a alteração da configuração.
   > 

1. Adicione o seguinte código ao fim do **SimulateDeviceConfiguration.py** ficheiro: 

    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

    def iothub_client_sample_run():
        try:
            iothub_client_init()
        
            CLIENT.send_reported_state(TWIN_PAYLOAD, len(TWIN_PAYLOAD), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

1. Execute a aplicação de dispositivo:
   
    ```cmd/sh
    node SimulateDeviceConfiguration.py
    ```
   
    Deverá ver a mensagem `Device twins updated.`. Manter a aplicação em execução.


## <a name="create-the-service-app"></a>Criar a aplicação de serviço
Nesta secção, irá criar uma aplicação de consola de Python que atualiza o *pretendido propriedades* no dispositivo duplo associado **myDeviceId** com um novo objeto de configuração de telemetria. Em seguida, consulta os dispositivos duplos armazenados no IoT hub e mostra a diferença entre as configurações desejadas e comunicadas do dispositivo.

1. Instalar o **SDK do serviço do Azure IoT Python** utilizando o seguinte comando na sua linha de comandos:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Com um editor de texto, crie um novo **SetDesiredAndQuery.py** ficheiro.

1. Adicione o seguinte código para o **SetDesiredAndQuery.py** de ficheiros e substitua o **{IoTHubConnectionString}** marcador de posição pela cadeia de ligação do IoT Hub que copiou quando criou o seu hub e o **{"deviceId"}** marcador de posição pelo nome do seu dispositivo:

    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubError, IoTHubDeviceTwin

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"configId\":3, \"sendFrequency\":\"" + sys.argv[1:][0] + "\"}}}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

1. Adicione o seguinte código ao fim do **SetDesiredAndQuery.py** ficheiro:

    ```python
    def iothub_devicetwin_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
        
            twin_info = iothub_twin_method.get_twin(DEVICE_ID)
            print ( "" )
            print ( "Device Twin before update    :" )
            print ( "{0}".format(twin_info) )

            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)
            print ( "" )
            print ( "Device Twin after update     :" )
            print ( "{0}".format(twin_info) )
        
            while True:
                time.sleep(10)

                twin_info = iothub_twin_method.get_twin(DEVICE_ID)
                print ( "" )
                print ( "Device Twin after client change initiated    :" )
                print ( "{0}".format(twin_info) )
        
                print ( "" )
                print ( "IoT Hub service sample complete, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(30)
                
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceTwins Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicetwin_sample_run()
    ```

1. Com **SimulateDeviceConfiguration.py** em execução, executar a aplicação numa nova linha de comandos com:

    ```cmd/sh
    python SetDesiredAndQuery.py 5m
    ```
   
    Deverá ver o ID de configuração comunicado alterar **1** para **2** com o Active Directory novo enviar uma frequência de cinco minutos em vez de 24 horas.


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, configurou uma configuração desejada como *pretendido propriedades* a partir de uma aplicação de back-end e escreveu uma aplicação de dispositivo simulado para detetar essa alteração e simular um processo de atualização reporting o estado dele como *comunicado propriedades* para o dispositivo duplo.

Utilize os seguintes recursos para saber como:

* Enviar telemetria a partir de dispositivos com o [introdução ao IoT Hub] [ lnk-iothub-getstarted] tutorial,
* agendar ou efetuar operações em grandes conjuntos de dispositivos Consulte o [agenda e as tarefas de difusão] [ lnk-schedule-jobs] tutorial.
* controlar dispositivos interativamente (como ativar uma ventoinha a partir de uma aplicação controlados pelo utilizador), com o [utilizar métodos diretos] [ lnk-methods-tutorial] tutorial.

<!-- links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-python-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-python-getstarted.md
[lnk-methods-tutorial]: iot-hub-python-python-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
