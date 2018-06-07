---
title: Atualização de firmware do dispositivo IoT hub do Azure (Python) | Microsoft Docs
description: Como utilizar a gestão de dispositivos no IoT Hub do Azure para iniciar uma atualização de firmware do dispositivo. Utilize os SDKs IoT do Azure para Python para implementar uma aplicação de dispositivo simulada e uma aplicação de serviço que aciona a atualização de firmware.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: kgremban
ms.openlocfilehash: d2ebdf54e595c2f02464c0c2446a6e5f5feefb9c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634646"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-pythonpython"></a>Gestão de dispositivos de utilização para iniciar um firmware do dispositivo atualizar (Python/Python)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

No [introdução à gestão de dispositivos] [ lnk-dm-getstarted] tutorial, vimos como utilizar o [dispositivo duplo] [ lnk-devtwin] e [direcionar métodos] [ lnk-c2dmethod] primitivos reiniciar remotamente um dispositivo. Este tutorial utiliza os mesmos primitivos do IoT Hub e fornece orientações e mostra como efetuar uma atualização de firmware simulada ponto-a-ponto.  Este padrão é utilizado na implementação de atualização de firmware para o exemplo de dispositivo Intel Edison.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este tutorial mostrar-lhe como:

* Crie uma aplicação de consola de Python que chama o método direto firmwareUpdate na aplicação do dispositivo simulado através do seu IoT hub.
* Criar uma aplicação de dispositivo simulado que implementa um **firmwareUpdate** método direto. Este método inicia um processo de fase multi que aguarda para transferir a imagem de firmware, transfere a imagem de firmware e, finalmente, aplica-se a imagem do firmware. Durante cada fase da atualização, o dispositivo utiliza as propriedades que relatados para elaborar relatórios sobre o progresso.

No final deste tutorial, tem duas aplicações de consola do Python:

**dmpatterns_fwupdate_service.PY**, que chama um método direto na aplicação do dispositivo simulado, mostra a resposta e periodicamente (cada 500ms) apresenta a atualização comunicado propriedades.

**dmpatterns_fwupdate_device.PY**, que liga ao seu IoT hub com a identidade de dispositivo que criou anteriormente, recebe um método direto firmwareUpdate, é executada através de um processo com múltiplos estado para simular a atualização de firmware, incluindo: a aguardar que a imagem transferir, transferir a nova imagem e, finalmente, aplicar a imagem.

Para concluir este tutorial, precisa do seguinte:

* [Python 2.x ou 3.x][lnk-python-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Quando lhe for pedido durante a instalação, confirme que adiciona Python à variável de ambiente específica da sua plataforma. Se estiver a utilizar Python 2.x, poderá ter de [instalar ou atualizar o *pip*, o sistema de gestão de pacotes de Python][lnk-install-pip].
* Se estiver a utilizar o SO Windows, o [pacote redistribuível Visual C++][lnk-visual-c-redist], para permitir a utilização de DLLs nativas de Python.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Acionar uma atualização de firmware remota no dispositivo com um método direto
Nesta secção, crie uma aplicação de consola de Python que inicia uma atualização de firmware remota num dispositivo. A aplicação utiliza um método direto para iniciar a atualização e utiliza consultas de duplo de dispositivo para obter periodicamente o estado da atualização de firmware do Active Directory.

1. Na sua linha de comandos, execute o seguinte comando para instalar o **azure iothub-service-cliente** pacote:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Com um editor de texto, no seu diretório de trabalho, crie um **dmpatterns_getstarted_service.py** ficheiro.

1. Adicione as declarações de importação' Seguinte' e variáveis no início do **dmpatterns_getstarted_service.py** ficheiro. Substitua `IoTHubConnectionString` e `deviceId` com os valores que apontou anteriormente:
   
    ```python
    import sys
    import time

    import iothub_service_client
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "firmwareUpdate"
    METHOD_PAYLOAD = "{\"fwPackageUri\":\"test.com\"}"
    TIMEOUT = 60
    MESSAGE_COUNT = 5
    ```

1. Adicione a seguinte função para chamar o método direto e apresentar o valor da firmwareUpdate comunicadas propriedade. Também adicionar o `main` rotina:
   
    ```python
    def iothub_firmware_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            print ( "" )
            print ( "Direct Method called." )
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)
        
            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)
            print ( response.payload )
        
            print ( "" )
            print ( "Device Twin queried, press Ctrl-C to exit" )
            while True:
                twin_info = iothub_twin_method.get_twin(DEVICE_ID)

                if "\"firmwareStatus\":\"standBy\"" in twin_info:
                    print ( "Waiting on device..." )
                elif "\"firmwareStatus\":\"downloading\"" in twin_info:
                    print ( "Downloading firmware..." )
                elif "\"firmwareStatus\":\"applying\"" in twin_info:
                    print ( "Download complete, applying firmware..." )
                elif "\"firmwareStatus\":\"completed\"" in twin_info:
                    print ( "Firmware applied" )
                    print ( "" )
                    print ( "Get reported properties from device twin:" )
                    print ( twin_info )
                    break
                else:
                    print ( "Unknown status" )

                status_counter = 0
                while status_counter <= MESSAGE_COUNT:
                    time.sleep(1)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubService sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub firmware update Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_firmware_sample_run()
    ```

1. Guarde e feche o **dmpatterns_fwupdate_service.py** ficheiro.


## <a name="create-a-simulated-device-app"></a>Criar uma aplicação de dispositivo simulada
Nesta secção, pode:

* Criar uma aplicação de consola de Python que responde a um método direto chamado pela nuvem
* Acionar uma atualização de firmware simulada
* Utilize as propriedades comunicadas para ativar as consultas de dispositivo duplo, de forma a identificar os dispositivos e quando concluírem uma atualização de firmware pela última vez

1. Na sua linha de comandos, execute o seguinte comando para instalar o **azure iothub-dispositivo-cliente** pacote:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Com um editor de texto, crie um **dmpatterns_fwupdate_device.py** ficheiro.

1. Adicione as declarações de importação' Seguinte' e variáveis no início do **dmpatterns_fwupdate_device.py** ficheiro. Substitua `deviceConnectionString` com a cadeia de ligação do dispositivo do seu IoT hub:
   
    ```python
    import time, datetime
    import sys
    import threading

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    MESSAGE_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)
    ```

1. Adicione as seguintes funções que são utilizadas para fornecer comunicadas atualizações de propriedades e implementa o método direto:
   
    ```python
    def send_reported_state_callback(status_code, user_context):
        print ( "Reported state updated." )

    def device_method_callback(method_name, payload, user_context):
        if method_name == "firmwareUpdate":
            print ( "Starting firmware update." )
            image_url = payload
            thr = threading.Thread(target=simulate_download_image, args=([payload]), kwargs={})
            thr.start()
    
            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"Firmware update started\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

1. Adicione as seguintes funções que simulam a transferência e aplicação da imagem de firmware:
   
    ```python
    def simulate_download_image(image_url):
        time.sleep(15)
        print ( "Downloading image from: " + image_url )

        reported_state = "{\"firmwareStatus\":\"downloading\", \"downloadComplete\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        time.sleep(15)
    
        simulate_apply_image(image_url)
    
    def simulate_apply_image(image_url):
        print ( "Applying image from: " + image_url )

        reported_state = "{\"firmwareStatus\":\"applying\", \"startedApplyingImage\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        time.sleep(15)
    
        simulate_complete_image()

    def simulate_complete_image():
        print ( "Image applied." )

        reported_state = "{\"firmwareStatus\":\"completed\", \"lastFirmwareUpdate\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
    ```

8. Adicione a seguinte função inicializa o dispositivo duplo comunicadas propriedades e aguarde que o método a chamar direto. Também adicionar o `main` rotina:
   
    ```python
    def iothub_firmware_sample_run():
        try:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)

            reported_state = "{\"firmwareStatus\":\"standBy\", \"logTime\":\"" + str(datetime.datetime.now()) + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
            print ( "Device twins initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )
        
            while True:
                status_counter = 0
                while status_counter <= MESSAGE_COUNT:
                    time.sleep(10)
                    status_counter += 1
            
        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python firmware update sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_firmware_sample_run()
    ```

> [!NOTE]
> Para facilitar, este tutorial não implementa nenhuma política de repetição. No código de produção, deve implementar as políticas de repetição (como um término exponencial), como sugerido no artigo do MSDN [processamento de erros transitórios](https://msdn.microsoft.com/library/hh675232.aspx).
> 


## <a name="run-the-apps"></a>Executar as aplicações
Já está pronto para executar as aplicações.

1. Na linha de comandos, execute o seguinte comando para começar a escutar o método direta de reinício.
   
    ```cmd/sh
    python dmpatterns_fwupdate_device.py
    ```

1. Outra linha de comandos, execute o seguinte comando para acionar a reiniciar o computador remoto e a consulta para o dispositivo duplo localizar a última hora de reinício.
   
    ```cmd/sh
    python dmpatterns_fwupdate_service.py
    ```

1. Pode ver a resposta de dispositivo para o método direto na consola do. Em seguida, tenha em atenção a alteração de propriedades comunicadas em toda a atualização de firmware.

    ![Saída do programa][1]


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, é utilizado um método direto para acionar uma atualização de firmware remota num dispositivo e utilizado as propriedades que relatados para seguir o progresso da atualização de firmware.

Para saber como expandir o seu IoT chama o método de solução e agenda em vários dispositivos, consulte o [agenda e as tarefas de difusão] [ lnk-tutorial-jobs] tutorial.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[1]: ./media/iot-hub-python-python-firmware-update/1.png
