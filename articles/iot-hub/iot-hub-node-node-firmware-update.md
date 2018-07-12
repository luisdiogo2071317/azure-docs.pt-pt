---
title: Atualização de firmware do dispositivo com o IoT Hub do Azure (Node) | Documentos da Microsoft
description: Como utilizar a gestão de dispositivos no IoT Hub do Azure para iniciar uma atualização de firmware do dispositivo. Utilize os SDKs IoT do Azure para node. js para implementar uma aplicação de dispositivo simulado e uma aplicação de serviço que aciona a atualização de firmware.
author: juanjperez
manager: cberlin
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/07/2017
ms.author: juanpere
ms.openlocfilehash: 0cd8c019cf9a65e0e72227ba99c1995a45ed4067
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452435"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-nodenode"></a>Utilizar a gestão de dispositivos para iniciar uma atualização de firmware do dispositivo (nó)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

Na [introdução à gestão de dispositivos] [ lnk-dm-getstarted] tutorial, viu como usar o [dispositivo duplo] [ lnk-devtwin] e [métodos diretos ] [ lnk-c2dmethod] primitivos para reiniciar remotamente um dispositivo. Este tutorial utiliza os mesmo primitivos de IoT Hub e fornece orientação e mostra-lhe como fazer uma atualização de firmware simulada do ponto-a-ponto.  Esse padrão é usado na implementação de atualização de firmware para o exemplo de dispositivo do Intel Edison.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este tutorial mostrar-lhe como:

* Crie uma aplicação de consola node. js que chama o método direto firmwareUpdate na aplicação do dispositivo simulado através do IoT hub.
* Criar uma aplicação de dispositivo simulado que implementa uma **firmwareUpdate** método direto. Este método inicia um processo de vários estágio que aguarda a transferência da imagem de firmware, transfere a imagem de firmware e, finalmente, aplica-se a imagem de firmware. Durante cada fase da atualização, o dispositivo utiliza as propriedades comunicadas para comunicar o progresso.

No final deste tutorial, tem duas aplicações de consola node. js:

**dmpatterns_fwupdate_service.js**, que chama um método direto na aplicação do dispositivo simulado, apresenta a resposta, e periodicamente (cada 500 MS) apresenta a atualização propriedades comunicadas.

**dmpatterns_fwupdate_device. js**, que liga ao seu hub IoT com a identidade de dispositivo que criou anteriormente, recebe um método direto firmwareUpdate, é executado através de um processo com múltiplos estado para simular um incluindo de atualização de firmware: a aguardar que a imagem Baixe, transferir a nova imagem e, finalmente, a aplicação da imagem.

Para concluir este tutorial, precisa do seguinte:

* Versão node. js 4.0.x ou posterior, <br/>  [Preparar o ambiente de desenvolvimento] [ lnk-dev-setup] descreve como instalar o node. js para este tutorial no Windows ou Linux.
* Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)

Siga os [introdução à gestão de dispositivos](iot-hub-node-node-device-management-get-started.md) artigo para criar o hub IoT e obter a cadeia de ligação do IoT Hub.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Acionar uma atualização de firmware remota no dispositivo com um método direto
Nesta secção, vai criar uma aplicação de consola node. js que inicia uma atualização de firmware remoto num dispositivo. A aplicação utiliza um método direto para iniciar a atualização e utiliza consultas de gémeos de dispositivo para obter periodicamente o estado da atualização de firmware do Active Directory.

1. Criar uma pasta vazia designada **triggerfwupdateondevice**.  Na **triggerfwupdateondevice** pasta, crie um ficheiro Package. JSON com o seguinte comando na sua linha de comandos.  Aceite todas as predefinições:
   
    ```
    npm init
    ```
2. Sua linha de comandos do **triggerfwupdateondevice** pasta, execute o seguinte comando para instalar o **do azure-iot-hub** pacote:
   
    ```
    npm install azure-iothub --save
    ```
3. Com um editor de texto, crie uma **dmpatterns_getstarted_service.js** de ficheiros a **triggerfwupdateondevice** pasta.
4. Adicione as seguintes declarações no início de "necessitam" a **dmpatterns_getstarted_service.js** ficheiro:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Adicione as seguintes declarações de variável e substitua os valores de marcador de posição:
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. Adicione a seguinte função para localizar e apresentar o valor da firmwareUpdate comunicado propriedade.
   
    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```
7. Adicione a seguinte função para invocar o método firmwareUpdate reiniciar o dispositivo de destino:
   
    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
   
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
   
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
   
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```
8. Por fim, adicione a seguinte função para o código para iniciar a sequência de atualização de firmware e comece a ser apresentado periodicamente as propriedades comunicadas:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Guarde e feche o **dmpatterns_fwupdate_service.js** ficheiro.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>Executar as aplicações
Já está pronto para executar as aplicações.

1. A linha de comandos do **manageddevice** pasta, execute o seguinte comando para começar a escutar o método direto de reinício.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. A linha de comandos do **triggerfwupdateondevice** pasta, execute o seguinte comando para acionar a reinicialização remota e a consulta para o dispositivo duplo localizar a última hora de reinício.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. Ver a resposta de dispositivo para o método direto na consola do.

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, utilizou um método direto para acionar uma atualização de firmware remoto num dispositivo e utilizadas as propriedades reportadas para seguir o progresso da atualização de firmware.

Para saber como expandir o seu IoT chama o método de solução e a agenda em vários dispositivos, veja a [agendar e transmitir tarefas] [ lnk-tutorial-jobs] tutorial.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
