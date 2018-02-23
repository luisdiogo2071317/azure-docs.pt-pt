---
title: Carregar ficheiros de dispositivos para o IoT Hub do Azure com o Python | Microsoft Docs
description: "Como carregar ficheiros a partir de um dispositivo para a nuvem através de dispositivos IoT do Azure SDK para Python. Ficheiros carregados são armazenados num contentor do blob storage do Azure."
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2018
ms.author: v-masebo
ms.openlocfilehash: ac871a03ebb93dac3b91f7df2220cd5f4506498b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/21/2018
---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Carregar ficheiros do seu dispositivo para a nuvem com o IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Este tutorial segue como utilizar o [ficheiro capacidades de carregamento do IoT Hub](iot-hub-devguide-file-upload.md) para carregar um ficheiro para [blob storage do Azure](../storage/index.yml). O tutorial mostra como para:

- Forneça em segurança um contentor de armazenamento para carregar um ficheiro.
- Utilize o cliente do Python para carregar um ficheiro através do seu IoT hub.

O [introdução ao IoT Hub](iot-hub-node-node-getstarted.md) e [enviar mensagens da nuvem para o dispositivo com o IoT Hub](iot-hub-node-node-c2d.md) os tutoriais mostram a dispositivo para nuvem e da nuvem para o dispositivo mensagens funcionalidades básicas do IoT Hub. No entanto, em alguns cenários não pode facilmente mapear os dados que os dispositivos as enviam para as mensagens dispositivo-nuvem relativamente pequenas, que aceita o IoT Hub. Quando for necessário upland ficheiros de um dispositivo, pode continuar a utilizar a segurança e fiabilidade do IoT Hub.

> [!NOTE]
> SDK Python do Hub IoT atualmente apenas suporta carregar ficheiros baseada em carateres, tais como **. txt** ficheiros.

No final deste tutorial, executar a aplicação de consola Python:

* **FileUpload.py**, que carrega um ficheiro para o armazenamento utilizando o SDK de dispositivo do Python.

> [!NOTE]
> IoT Hub suporta várias plataformas de dispositivos e idiomas (incluindo C, .NET, Javascript, Python e Java) através de SDKs do dispositivo IoT do Azure. Consulte o [Centro de programadores do IoT do Azure] para obter instruções passo a passo sobre como ligar o seu dispositivo ao IoT Hub do Azure.

Para concluir este tutorial, precisa do seguinte:

* [Python 2.x ou 3.x][lnk-python-download]. Certifique-se de que utiliza a instalação de 32 ou 64 bits, conforme exigido pela sua configuração. Quando lhe for pedido durante a instalação, confirme que adiciona Python à variável de ambiente específica da sua plataforma. Se estiver a utilizar Python 2.x, poderá ter de [instalar ou atualizar o *pip*, o sistema de gestão de pacotes de Python][lnk-install-pip].
* Se estiver a utilizar o SO Windows, o [pacote redistribuível Visual C++][lnk-visual-c-redist], para permitir a utilização de DLLs nativas de Python.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar um [conta gratuita](http://azure.microsoft.com/pricing/free-trial/) em apenas alguns minutos.)


[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]


## <a name="upload-a-file-from-a-device-app"></a>Carregar um ficheiro a partir de uma aplicação de dispositivo

Nesta secção, vai criar a aplicação de dispositivo para carregar um ficheiro ao IoT hub.

1. Na sua linha de comandos, execute o seguinte comando para instalar o **azure iothub-dispositivo-cliente** pacote:

    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Com um editor de texto, crie um **FileUpload.py** ficheiros na sua pasta de trabalho.

1. Adicione o seguinte `import` instruções e variáveis no início do **FileUpload.py** ficheiro. Substitua `deviceConnectionString` com a cadeia de ligação do dispositivo IoT hub:

    ```python
    import time
    import sys
    import iothub_client
    import os
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.HTTP

    FILENAME = 'sample.txt'
    ```

1. Crie uma chamada de retorno para a **upload_blob** função:

    ```python
    def blob_upload_conf_callback(result, user_context):
        if str(result) == 'OK':
            print ( "...file uploaded successfully." )
        else:
            print ( "...file upload callback returned: " + str(result) )
    ```

1. Adicione o seguinte código para ligar o cliente e carregar o ficheiro. Também incluir o `main` rotina:

    ```python
    def iothub_file_upload_sample_run():
        try:
            print ( "IoT Hub file upload sample, press Ctrl-C to exit" )
        
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

            client.upload_blob_async(FILENAME, FILENAME, os.path.getsize(FILENAME), blob_upload_conf_callback, 0)
        
            print ( "" )
            print ( "File upload initiated..." )
        
            while True:
                time.sleep(30)

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
        except:
            print ( "generic error" )
        
    if __name__ == '__main__':
        print ( "Simulating a file upload using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_file_upload_sample_run()
    ```

1. Guarde e feche o **UploadFile.py** ficheiro.

1. Copie um ficheiro de texto de exemplo para a pasta de trabalho e renomeie- `sample.txt`.

    > [!NOTE]
    > SDK Python do Hub IoT atualmente apenas suporta carregar ficheiros baseada em carateres, tais como **. txt** ficheiros.


## <a name="run-the-application"></a>Executar a aplicação

Agora, está pronto para executar a aplicação.

1. Numa linha de comando na sua pasta de trabalho, execute o seguinte comando:

    ```cmd/sh
    python FileUpload.py
    ```

1. A seguinte captura de ecrã mostra a saída do **FileUpload** aplicação:

    ![Resultado da aplicação simulated-device](./media/iot-hub-python-python-file-upload/1.png)

1. Pode utilizar o portal para ver o ficheiro carregado no contentor de armazenamento que configurou:

    ![Ficheiro foi carregado](./media/iot-hub-python-python-file-upload/2.png)


## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, aprendeu a utilizar as capacidades de carregamento de ficheiros do IoT Hub para simplificar os carregamentos de ficheiros a partir de dispositivos. Pode continuar a explorar as funcionalidades do IoT hub e cenários nos seguintes artigos:

* [Criar um hub IoT através de programação][lnk-create-hub]
* [Introdução ao C SDK][lnk-c-sdk]
* [SDKs IoT do Azure][lnk-sdks]

<!-- Links -->
[Centro de programadores do IoT do Azure]: http://azure.microsoft.com/develop/iot

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/