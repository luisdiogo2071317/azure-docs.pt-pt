---
title: Agendar tarefas IoT hub do Azure (Python) | Microsoft Docs
description: Como agendar um trabalho do IoT Hub do Azure para invocar um método direto em vários dispositivos. Utilize os SDKs IoT do Azure para Python para implementar as aplicações de dispositivo simulada e uma aplicação de serviço para executar a tarefa.
services: iot-hub
documentationcenter: .net
author: kgremban
manager: timlt
editor: ''
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2018
ms.author: v-masebo;kgremban
ms.openlocfilehash: bb087d3fce8d663995a3878951477df8343dca00
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/05/2018
---
# <a name="schedule-and-broadcast-jobs-python"></a>Tarefas de agendamento e de difusão (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

IoT Hub do Azure é um serviço completamente gerido que permite uma aplicação de back-end criar e controlar as tarefas agendadas e atualizar milhões de dispositivos.  As tarefas podem ser utilizadas para as seguintes ações:

* Atualizar as propriedades pretendidas
* Etiquetas de atualização
* Invocar métodos diretos

Concecionais, uma tarefa encapsula num wrapper uma destas ações e controla o progresso de execução face a um conjunto de dispositivos, que é definido por uma consulta do dispositivo duplo.  Por exemplo, uma aplicação de back-end pode utilizar uma tarefa para invocar um método de reinício 10 000 dispositivos, especificado por uma consulta do dispositivo duplo e agendadas num momento futuro.  Essa aplicação, em seguida, pode acompanhar o progresso como cada um desses dispositivos receber e executar o método de reinício.

Saiba mais sobre cada uma destas capacidades nestes artigos:

* Dispositivo duplo e propriedades: [começar a utilizar dispositivos duplos] [ lnk-get-started-twin] e [Tutorial: como utilizar as propriedades do dispositivo duplo][lnk-twin-props]
* Direcionar métodos: [guia para programadores do IoT Hub - métodos diretas] [ lnk-dev-methods] e [Tutorial: direcionar métodos][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este tutorial mostrar-lhe como:

* Criar uma aplicação de dispositivo simulado de Python que tem um método direto, o que lhe permite **lockDoor**, que pode ser chamada pelo solução de back-end.
* Criar uma aplicação de consola de Python que chama o **lockDoor** método direto na aplicação do dispositivo simulado utilizando uma tarefa e atualizações as propriedades de pretendido utilizando uma tarefa de dispositivo.

No final deste tutorial, tem duas aplicações de Python:

**simDevice.py**, que liga ao seu IoT hub com a identidade de dispositivo e recebe um **lockDoor** método direto.

**scheduleJobService.py**, que chama um método direto na aplicação do dispositivo simulado e atualiza o dispositivo duplo pretendido propriedades utilizando uma tarefa.

Para concluir este tutorial, precisa do seguinte:

* [Python 2.x ou 3.x][lnk-python-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Quando lhe for pedido durante a instalação, confirme que adiciona Python à variável de ambiente específica da sua plataforma. Se estiver a utilizar Python 2.x, poderá ter de [instalar ou atualizar o *pip*, o sistema de gestão de pacotes de Python][lnk-install-pip].
* Se estiver a utilizar o SO Windows, o [pacote redistribuível Visual C++][lnk-visual-c-redist], para permitir a utilização de DLLs nativas de Python.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

> [!NOTE]
> O **Azure IoT SDK para Python** não suporta diretamente **tarefas** funcionalidade. Em vez disso, este tutorial oferece uma solução alternativa utilizar threads assíncronos e temporizadores. Para obter atualizações, consulte o **SDK de cliente do serviço** lista de funcionalidades no [Azure IoT SDK para Python](https://github.com/Azure/azure-iot-sdk-python) página. 
> 
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Criar uma aplicação de dispositivo simulada
Nesta secção, vai criar uma aplicação de consola de Python que responde a um método direto chamado pela nuvem, o que aciona um simulada **lockDoor** método.

1. Na sua linha de comandos, execute o seguinte comando para instalar o **azure-iot-dispositivo-cliente** pacote:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Com um editor de texto, crie um novo **simDevice.py** ficheiro no seu diretório de trabalho.

1. Adicione o seguinte `import` instruções e variáveis no início do **simDevice.py** ficheiro. Substitua `deviceConnectionString` com a cadeia de ligação do dispositivo que criou acima:
   
    ```python
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    METHOD_CONTEXT = 0
    TWIN_CONTEXT = 0
    WAIT_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

1. Adicione a seguinte chamada de retorno de função para processar o **lockDoor** método:
   
    ```python
    def device_method_callback(method_name, payload, user_context):
        if method_name == "lockDoor":
            print ( "Locking Door!" )

            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"lockDoor called successfully\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

1. Adicione outra chamada de retorno de função para lidar com dispositivos duplos atualizações:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "")
        print ( "Twin callback called with:")
        print ( "payload: %s" % payload )
    ```

1. Adicione o seguinte código para registar o processador para o **lockDoor** método. Também incluir o `main` rotina:
   
    ```python
    def iothub_jobs_sample_run():
        try:
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
            client.set_device_method_callback(device_method_callback, METHOD_CONTEXT)
            client.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

            print ( "Direct method initialized." )
            print ( "Device twin callback initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )
        
            while True:
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
        print ( "Starting the IoT Hub Python jobs sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_jobs_sample_run()
    ```

1. Guarde e feche o **simDevice.py** ficheiro.

> [!NOTE]
> Para facilitar, este tutorial não implementa nenhuma política de repetição. No código de produção, deve implementar as políticas de repetição (como um término exponencial), como sugerido no artigo [Transient Fault Handling (Processamento de Erros Transitórios)][lnk-transient-faults] da MSDN.
> 
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Agendar tarefas para chamar um método direto e atualizar as propriedades de um dispositivo duplo
Nesta secção, vai criar uma aplicação de consola de Python que inicia um remoto **lockDoor** num dispositivo utilizando um método direto e atualizar as propriedades do dispositivo duplo.

1. Na sua linha de comandos, execute o seguinte comando para instalar o **azure-iot--o cliente do serviço** pacote:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Com um editor de texto, crie um novo **scheduleJobService.py** ficheiro no seu diretório de trabalho.

1. Adicione o seguinte `import` instruções e variáveis no início do **scheduleJobService.py** ficheiro:
   
    ```python
    import sys
    import time
    import threading
    import uuid

    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "lockDoor"
    METHOD_PAYLOAD = "{\"lockTime\":\"10m\"}"
    UPDATE_JSON = "{\"properties\":{\"desired\":{\"building\":43,\"floor\":3}}}"
    TIMEOUT = 60
    WAIT_COUNT = 5
    ```

1. Adicione a seguinte função que é utilizada para consultar dispositivos:
   
    ```python
    def query_condition(device_id):
        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
    
        number_of_devices = 10
        dev_list = iothub_registry_manager.get_device_list(number_of_devices)
    
        for device in range(0, number_of_devices):
            if dev_list[device].deviceId == device_id:
                return 1

        print ( "Device not found" )
        return 0
    ```

1. Adicione os seguintes métodos para executar as tarefas que se chame o duplo direto de método e o dispositivo:
   
    ```python
    def device_method_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job: " + str(job_id) )
        time.sleep(wait_time)
    
        if query_condition(device_id):
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)
    
            response = iothub_device_method.invoke(device_id, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)
        
            print ( "" )
            print ( "Direct method " + METHOD_NAME + " called." )
        
    def device_twin_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job " + str(job_id) )
        time.sleep(wait_time)
    
        if query_condition(device_id):
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
    
            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)
        
            print ( "" )
            print ( "Device twin updated." )
    ```

1. Adicione o seguinte código para agendar tarefas e atualizar o estado da tarefa. Também incluir o `main` rotina:
   
    ```python
    def iothub_jobs_sample_run():
        try:
            method_thr_id = uuid.uuid4()
            method_thr = threading.Thread(target=device_method_job, args=(method_thr_id, DEVICE_ID, 20, TIMEOUT), kwargs={})
            method_thr.start()
        
            print ( "" )
            print ( "Direct method called with Job Id: " + str(method_thr_id) )
        
            twin_thr_id = uuid.uuid4()
            twin_thr = threading.Thread(target=device_twin_job, args=(twin_thr_id, DEVICE_ID, 10, TIMEOUT), kwargs={})
            twin_thr.start()
        
            print ( "" )
            print ( "Device twin called with Job Id: " + str(twin_thr_id) )
        
            while True:
                print ( "" )
            
                if method_thr.is_alive():
                    print ( "...job " + str(method_thr_id) + " still running." )
                else:
                    print ( "...job " + str(method_thr_id) + " complete." )
            
                if twin_thr.is_alive():
                    print ( "...job " + str(twin_thr_id) + " still running." )
                else:
                    print ( "...job " + str(twin_thr_id) + " complete." )
                
                print ( "Job status posted, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
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
        print ( "Starting the IoT Hub jobs Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_jobs_sample_run()
    ```

1. Guarde e feche o **scheduleJobService.py** ficheiro.


## <a name="run-the-applications"></a>Executar as aplicações
Pode agora executar as aplicações.

1. Na linha de comandos no seu diretório de trabalho, execute o seguinte comando para começar a escutar o método direta de reinício:
   
    ```cmd/sh
    python simDevice.py
    ```

1. Na linha de comandos outra no seu diretório de trabalho, execute o seguinte comando para acionar as tarefas para bloquear a porta e atualizar o duplo:
   
    ```cmd/sh
    python scheduleJobService.py
    ```

1. Consulte as respostas de dispositivo para o método direto e dispositivos duplos atualização na consola do.

    ![saída de dispositivo][1]

    ![saída de serviço][2]


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, uma tarefa que utilizou para agendar um método direto para um dispositivo e a atualização das propriedades do dispositivo duplo.

Para continuar a introdução ao IoT Hub e padrões de gestão de dispositivos como remota através da atualização de firmware ondas eletromagnéticas, consulte:

[Tutorial: Como efetuar uma atualização de firmware][lnk-fwupdate]

Para continuar a introdução ao IoT Hub, consulte [introdução ao Azure IoT Edge][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-python-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-python-python-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-transient-faults]: https://docs.microsoft.com/azure/architecture/best-practices/transient-faults

[1]: ./media/iot-hub-python-python-schedule-jobs/1.png
[2]: ./media/iot-hub-python-python-schedule-jobs/2.png
