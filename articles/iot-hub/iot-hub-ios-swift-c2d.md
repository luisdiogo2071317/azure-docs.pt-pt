---
title: Mensagens da nuvem para dispositivo IoT hub do Azure (iOS) | Microsoft Docs
description: Como enviar mensagens da nuvem para o dispositivo a um dispositivo de um hub IoT do Azure com os SDKs IoT do Azure para iOS.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: ''
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2018
ms.author: kgremban
ms.openlocfilehash: 23dbd1f359f947b8e87ab4115887120dfd55907a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
---
# <a name="send-cloud-to-device-messages-with-iot-hub-ios"></a>Enviar mensagens da nuvem para o dispositivo com o IoT Hub (iOS)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]


IoT Hub do Azure é um serviço completamente gerido que o ajuda a ativar fiáveis e seguras comunicações bidirecionais entre milhões de dispositivos e uma solução de back-end. O [enviar telemetria a partir de um dispositivo a um IoT hub] artigo mostra como criar um hub IoT, aprovisionar uma identidade de dispositivo no mesmo e código de uma aplicação de dispositivo simulado que envia mensagens do dispositivo para a nuvem.

Este artigo mostra como para:

* Da sua solução de back-end, envie mensagens de nuvem para o dispositivo para um único dispositivo através do IoT Hub.
* Receba mensagens da nuvem para o dispositivo num dispositivo.
* Da sua solução de back-end, pedir confirmação de entrega (*comentários*) para mensagens enviadas para um dispositivo a partir do IoT Hub.

Pode encontrar mais informações sobre mensagens da nuvem para o dispositivo no [guia para programadores do IoT Hub][IoT Hub developer guide - C2D].

No final deste artigo, executar Swift dois projetos de iOS:

* **dispositivo de exemplo**, a mesma aplicação criada na [enviar telemetria a partir de um dispositivo a um IoT hub], que liga ao seu IoT hub e recebe mensagens da nuvem para o dispositivo.
* **serviço de exemplo**, que envia uma mensagem da nuvem para o dispositivo para a aplicação de dispositivo simulado através do IoT Hub e, em seguida, receber a confirmação de entrega.

> [!NOTE]
> IoT Hub tem suporte SDK para várias plataformas de dispositivos e idiomas (incluindo C, Java e Javascript) através de SDKs do dispositivo IoT do Azure. Para obter instruções passo a passo sobre como ligar o seu dispositivo para código neste tutorial e, geralmente, IoT Hub do Azure, consulte o [Centro de programadores do IoT do Azure].

Para concluir este tutorial, precisa do seguinte:

- Uma conta ativa do Azure. (Se não tiver uma conta, pode criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.)
- Um hub IoT Active Directory no Azure. 
- O exemplo de código do [exemplos do Azure](https://github.com/Azure-Samples/azure-iot-samples-ios/archive/master.zip) .
- A versão mais recente do [XCode](https://developer.apple.com/xcode/), com a última versão do SDK do iOS. Este início rápido foi testado com XCode 9.3 e o iOS 11.3.
- A versão mais recente do [CocoaPods](https://guides.cocoapods.org/using/getting-started.html).


## <a name="simulate-an-iot-device"></a>Simular um dispositivo IoT
Nesta secção, simular um dispositivo iOS com uma aplicação Swift para receber mensagens da nuvem para dispositivos do IoT hub. 

Este é o dispositivo de exemplo de exemplo que criar no artigo [enviar telemetria a partir de um dispositivo a um IoT hub]. Se já tiver que em execução, pode ignorar esta secção.

### <a name="install-cocoapods"></a>Instalar CocoaPods

O CocoaPods gere dependências para projetos de iOS que utilizam bibliotecas de terceiros.

Numa janela de terminal, navegue para a pasta Azure-IoT-Samples-iOS que transferiu nos pré-requisitos. Depois, navegue para o projeto de exemplo:

```sh
cd quickstart/sample-device
```

Confirme que XCode está fechado e execute o seguinte comando para instalar os CocoaPods que são declarados no ficheiro **podfile**:

```sh
pod install
```

Para além de instalar os pods necessários para o projeto, o comando de instalação também criou um ficheiro de área de trabalho do XCode, que já está configurado para utilizar os pods para dependências. 

### <a name="run-the-sample-device-application"></a>Executar a aplicação de dispositivo de exemplo 

1. Obter a cadeia de ligação para o seu dispositivo. Pode copiar esta cadeia do portal do Azure no painel de detalhes do dispositivo ou obtê-lo com o seguinte comando da CLI: 

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name {YourIoTHubName} --device-id {YourDeviceID} --output table
    ```

1. Abra a área de trabalho de exemplo no XCode.

   ```sh
   open "MQTT Client Sample.xcworkspace"
   ```

2. Expanda o **MQTT cliente exemplo** projeto e, em seguida, a pasta com o mesmo nome.  
3. Abra **ViewController.swift** para edição no XCode. 
4. Procure o **connectionString** variável e atualize o valor com a ligação do dispositivo de cadeia que é copiado no primeiro passo.
5. Guarde as alterações. 
6. Execute o projeto no emulador do dispositivo com o botão **Build and run** (Compilar e executar) ou a combinação de teclas **comando + r**. 

   ![Executar o projeto](media/quickstart-send-telemetry-ios/run-sample.png)


## <a name="simulate-a-service-device"></a>Simular um dispositivo de serviço

Nesta secção, simular um segundo dispositivo iOS com uma aplicação Swift que envia mensagens da nuvem para o dispositivo através do hub IoT. Esta configuração é útil para cenários de IoT onde existe um iPhone ou iPad a funcionar como um controlador para outros dispositivos iOS ligados a um IoT hub. 

### <a name="install-cocoapods"></a>Instalar CocoaPods

O CocoaPods gere dependências para projetos de iOS que utilizam bibliotecas de terceiros.

Navegue para a pasta de amostras de iOS de IoT do Azure que transferiu nos pré-requisitos. Em seguida, navegue para o projeto de serviço de exemplo:

```sh
cd quickstart/sample-service
```

Confirme que XCode está fechado e execute o seguinte comando para instalar os CocoaPods que são declarados no ficheiro **podfile**:

```sh
pod install
```

Para além de instalar os pods necessários para o projeto, o comando de instalação também criou um ficheiro de área de trabalho do XCode, que já está configurado para utilizar os pods para dependências.

### <a name="run-the-sample-service-application"></a>Executar a aplicação de serviço de exemplo

1. Obter a cadeia de ligação de serviço para o seu IoT hub. Pode copiar esta cadeia do portal do Azure a partir de **iothubowner** política no **políticas de acesso partilhado** painel, ou obtê-lo com o seguinte comando da CLI:  

    ```azurecli-interactive
    az iot hub show-connection-string --hub-name {YourIoTHubName} --output table
    ```

2. Abra a área de trabalho de exemplo no XCode.

   ```sh
   open AzureIoTServiceSample.xcworkspace
   ```

3. Expanda o **AzureIoTServiceSample** projeto e, em seguida, expanda a pasta com o mesmo nome.  
4. Abra **ViewController.swift** para edição no XCode. 
5. Procure o **connectionString** variável e atualize o valor com a cadeia de ligação de serviço que copiou anteriormente.
6. Guarde as alterações. 
7. No Xcode, altere as definições do emulador num dispositivo iOS diferentes que utilizou para executar o dispositivo IoT. XCode não é possível executar vários emuladores do mesmo tipo. 

   ![Alterar o dispositivo de emulador](media/iot-hub-ios-swift-c2d/change-device.png)

8. Executar o projeto no emulador do dispositivo com o **compilar e executar** botão ou a combinação de chave **comando + r**. 

   ![Executar o projeto](media/iot-hub-ios-swift-c2d/run-app.png)


## <a name="send-a-cloud-to-device-message"></a>Enviar uma mensagem da nuvem para o dispositivo
Agora está pronto para utilizar as duas aplicações para enviar e receber mensagens da nuvem para o dispositivo.

1. No **iOS exemplo de aplicação** aplicação em execução no dispositivo simulado do IoT, clique em **iniciar**. A aplicação inicia o envio de mensagens do dispositivo para a nuvem, mas também começa a escutar mensagens da nuvem para o dispositivo. 

   ![Ver a aplicação de exemplo de dispositivos de IoT](media/iot-hub-ios-swift-c2d/view-d2c.png)

2. No **exemplo de cliente do serviço de IoTHub** aplicação em execução no dispositivo simulado serviço, introduza o ID do dispositivo IoT que pretende que um para enviar uma mensagem. 
3. Escrever uma mensagem de texto simples, em seguida, clique em **enviar**. 

Várias ações acontecer, assim que clicar em enviar. O exemplo de serviço envia a mensagem ao seu IoT hub, o qual a aplicação tem acesso ao devido a ligação ao serviço de cadeia que fornecido. IoT hub verifica o ID de dispositivo, envia a mensagem para o dispositivo de destino e envia um recibo de confirmação para o dispositivo de origem. A aplicação em execução no seu dispositivo simulado do IoT verifica a existência de mensagens do IoT Hub e imprima o texto da mais recente no ecrã.

A saída deverá ter um aspeto semelhante ao seguinte exemplo:

   ![Ver mensagens da nuvem para dispositivo](media/iot-hub-ios-swift-c2d/view-c2d.png)


## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, aprendeu a enviar e receber mensagens da nuvem para o dispositivo. 

Para ver os exemplos de soluções ponto-a-ponto completas que utilizam o IoT Hub, consulte [acelerador de solução de monitorização remota do Azure IoT].

Para obter mais informações sobre como desenvolver soluções de IoT hub, consulte o [guia para programadores do IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-python-python-c2d/simulated-device.png
[img-send-command]:  media/iot-hub-python-python-c2d/send-command.png
[img-message-recieved]: media/iot-hub-python-python-c2d/message-recieved.png

<!-- Links -->
[enviar telemetria a partir de um dispositivo a um IoT hub]: quickstart-send-telemetry-ios.md

[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[guia para programadores do IoT Hub]: iot-hub-devguide.md
[Centro de programadores do IoT do Azure]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure portal]: https://portal.azure.com
[acelerador de solução de monitorização remota do Azure IoT]: https://azure.microsoft.com/documentation/suites/iot-suite/
