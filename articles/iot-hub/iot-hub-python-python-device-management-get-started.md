---
title: Introdução à gestão de dispositivos do IoT Hub do Azure (Python) | Microsoft Docs
description: Como utilizar a gestão de dispositivos do IoT Hub para iniciar um reinício do dispositivo remoto. Utilizar o Azure IoT SDK para Python para implementar uma aplicação de dispositivo simulado que inclui um método direto e uma aplicação de serviço que invoca o método direto.
services: iot-hub
documentationcenter: .net
author: kgremban
manager: timlt
editor: ''
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/02/2018
ms.author: v-masebo;kgremban
ms.openlocfilehash: 001a5d180e2b97894174ba7c63ec3947c9968364
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="get-started-with-device-management-python"></a>Introdução à gestão de dispositivos (Python)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

Este tutorial mostrar-lhe como:

* Utilize o portal do Azure para criar um IoT Hub e criar uma identidade de dispositivo no seu IoT hub.
* Crie uma aplicação de dispositivo simulado que contém um método direto que reinicia nesse dispositivo. Métodos diretos são invocados a partir da nuvem.
* Crie uma aplicação de consola de Python que chama o método direto reinício na aplicação do dispositivo simulado através do seu IoT hub.

No final deste tutorial, tem duas aplicações de consola do Python:

**dmpatterns_getstarted_device.PY**, que liga ao seu IoT hub com a identidade de dispositivo que criou anteriormente, recebe um método direta de reinício, simula reiniciar o computador físico, relatórios e a hora para o último reinício.

**dmpatterns_getstarted_service.PY**, que chama um método direto na aplicação do dispositivo simulado, mostra a resposta e apresenta a atualização comunicado propriedades.

Para concluir este tutorial, precisa do seguinte:

* [Python 2.x ou 3.x][lnk-python-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Quando lhe for pedido durante a instalação, confirme que adiciona Python à variável de ambiente específica da sua plataforma. Se estiver a utilizar Python 2.x, poderá ter de [instalar ou atualizar o *pip*, o sistema de gestão de pacotes de Python][lnk-install-pip].
    * Instalar o [azure iothub-dispositivo-cliente](https://pypi.org/project/azure-iothub-device-client/) do pacote, utilizando o comando   `pip install azure-iothub-device-client`
    * Instalar o [azure iothub-service-cliente](https://pypi.org/project/azure-iothub-service-client/) do pacote, utilizando o comando   `pip install azure-iothub-service-client`
* Se estiver a utilizar o SO Windows, o [pacote redistribuível Visual C++][lnk-visual-c-redist], para permitir a utilização de DLLs nativas de Python.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-a-simulated-device-app"></a>Criar uma aplicação de dispositivo simulada
Nesta secção, irá:

* Criar uma aplicação de consola de Python que responde a um método direto chamado pela nuvem
* Simular um reinício de dispositivo
* Utilize as propriedades que relatados para ativar as consultas de duplo de dispositivo identificar dispositivos e quando duram reiniciado

1. Com um editor de texto, crie um **dmpatterns_getstarted_device.py** ficheiro.

1. Adicione o seguinte `import` as instruções no início do **dmpatterns_getstarted_device.py** ficheiro.
   
    ```python
    import random
    import time, datetime
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    ```

1. Adicionar variáveis, incluindo um **CONNECTION_STRING** variável e a inicialização do cliente.  Substitua a cadeia de ligação pela cadeia de ligação do dispositivo.  
   
    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    SEND_REPORTED_STATE_CALLBACKS = 0
    METHOD_CALLBACKS = 0
    ```

1. Adicione as seguinte função as chamadas de retorno para implementar o método direto no dispositivo.
   
    ```python
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS
    
        print ( "Device twins updated." )

    def device_method_callback(method_name, payload, user_context):
        global METHOD_CALLBACKS
    
        if method_name == "rebootDevice":
            print ( "Rebooting device..." )
        
            time.sleep(20)
        
            print ( "Device rebooted." )
        
            current_time = str(datetime.datetime.now())
            reported_state = "{\"rebootTime\":\"" + current_time + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        
            print ( "Updating device twins: rebootTime" )
            
        device_method_return_value = DeviceMethodReturnValue()
        device_method_return_value.response = "{ \"Response\": \"This is the response from the device\" }"
        device_method_return_value.status = 200
    
        return device_method_return_value
    ```

1. Iniciar o serviço de escuta do método direto e de espera.
   
    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)
        
    def iothub_client_sample_run():
        try:
            iothub_client_init()

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

1. Guarde e feche o **dmpatterns_getstarted_device.py** ficheiro.

> [!NOTE]
> Para facilitar, este tutorial não implementa nenhuma política de repetição. No código de produção, deve implementar as políticas de repetição (como um término exponencial), como sugerido no artigo [Transient Fault Handling (Processamento de Erros Transitórios)][lnk-transient-faults] da MSDN.


## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Acionar um reinício remoto no dispositivo com um método direto
Nesta secção, crie uma aplicação de consola de Python que inicia um reinício remoto num dispositivo utilizando um método direto. A aplicação utiliza consultas de duplo de dispositivo para detetar a última vez de reiniciar o computador para que o dispositivo.

1. Com um editor de texto, crie um **dmpatterns_getstarted_service.py** ficheiro.

1. Adicione o seguinte `import` as instruções no início do **dmpatterns_getstarted_service.py** ficheiro.
   
    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubDeviceMethod, IoTHubError, IoTHubDeviceTwin
    ```

1. Adicione as seguintes declarações de variável. Substitua os valores de marcador de posição para _IoTHubConnectionString_ e _deviceId_.
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

1. Adicione a seguinte função para invocar o método de dispositivo para reiniciar o dispositivo de destino, em seguida, consultar dispositivos duplos e obter a última vez de reiniciar o computador.
   
    ```python
    def iothub_devicemethod_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)
        
            print ( "" )
            print ( "Invoking device to reboot..." )

            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Successfully invoked the device to reboot." )

            print ( "" )
            print ( response.payload )
        
            while True:
                print ( "" )
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    twin_info = iothub_twin_method.get_twin(DEVICE_ID)
                
                    if twin_info.find("rebootTime") != -1:
                        print ( "Last reboot time: " + twin_info[twin_info.find("rebootTime")+11:twin_info.find("rebootTime")+37])
                    else:
                        print ("Waiting for device to report last reboot time...")

                    time.sleep(5)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceManagement Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicemethod_sample_run()
    ```

1. Guarde e feche o **dmpatterns_getstarted_service.py** ficheiro.


## <a name="run-the-apps"></a>Executar as aplicações
Já está pronto para executar as aplicações.

1. Na linha de comandos, execute o seguinte comando para começar a escutar o método direta de reinício.
   
    ```
    python dmpatterns_getstarted_device.py
    ```

1. Outra linha de comandos, execute o seguinte comando para acionar a reiniciar o computador remoto e a consulta para o dispositivo duplo localizar a última hora de reinício.
   
    ```
    python dmpatterns_getstarted_service.py
    ```

1. Pode ver a resposta de dispositivo para o método direto na consola do.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
