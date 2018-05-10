---
title: Enviar mensagens para um servidor MQTT utilizando a biblioteca de cliente do Azure MQTT | Microsoft Docs.
description: Utilize o DevKit como um cliente para enviar mensagens para um servidor MQTT.
services: iot-hub
documentationcenter: ''
author: liydu
manager: timlt
tags: ''
keywords: ''
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2018
ms.author: liydu
ms.openlocfilehash: 62fdd3dae9513973d0284eda5250d9eb29bfde91
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/10/2018
---
# <a name="send-messages-to-an-mqtt-server"></a>Enviar mensagens para um servidor MQTT

Sistemas de Internet das coisas (IoT), muitas vezes, lidam com qualidade intermitente, fraca, ou lenta ligações à internet. MQTT é um protocolo de conectividade do máquina-máquina (M2M), que foi desenvolvido com essas desafios em mente. 

A biblioteca de clientes MQTT utilizada aqui faz parte o [Eclipse Paho](http://www.eclipse.org/paho/) projeto, que fornece APIs para a utilização de MQTT através de vários meios de transporte.

## <a name="what-you-learn"></a>O que irá aprender

Este projeto, saber:
- Como utilizar a biblioteca de clientes de MQTT para enviar mensagens para um mediador MQTT.
- Como configurar o seu DevKit de Iot MXChip como um cliente MQTT.

## <a name="what-you-need"></a>Do que precisa

Concluir o [guia de introdução](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) para:

* Tem a sua DevKit ligado ao Wi-Fi
* Preparar o ambiente de desenvolvimento

## <a name="open-the-project-folder"></a>Abra a pasta do projeto

1. Desligar o DevKit do seu computador, se já estiver a ser ligado.

2. Inicie o VS Code.

3. Ligar o DevKit para o seu computador.

## <a name="open-the-mqttclient-sample"></a>Abra o exemplo de MQTTClient

Expanda o lado esquerdo **ARDUINO exemplos** secção, navegue para **exemplos de MXCHIP AZ3166 > MQTT**e selecione **MQTTClient**. Abre uma nova janela do código de VS com uma pasta do projeto no mesmo.

> [!NOTE]
> Pode também abrir o exemplo de paleta de comando. Utilize `Ctrl+Shift+P` (macOS: `Cmd+Shift+P`) para abrir a paleta de comandos, escreva **Arduino**e, em seguida, localize e selecione **Arduino: exemplos**.

## <a name="build-and-upload-the-arduino-sketch-to-the-devkit"></a>Criar e carregar o sketch Arduino para o DevKit

Tipo `Ctrl+P` (macOS: `Cmd+P`) para executar `task device-upload`. Depois do carregamento estiver concluído, DevKit reiniciado e executa o sketch.

![carregamento de dispositivo](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/device-upload.jpg)

> [!NOTE]
> Poderá receber um "erro: AZ3166: pacote desconhecido" mensagem de erro. Este erro ocorre quando o índice de pacote de placa não está atualizado corretamente. Para resolver este erro, consulte este [FAQ](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/#development).

## <a name="test-the-project"></a>Testar o projeto

No VS Code, siga este procedimento para abrir e configurar o Monitor de série:

1. Clique em de `COM[X]` word na barra de estado para definir a porta COM à direita com `STMicroelectronics`: ![porta de com conjunto](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-com-port.jpg)

2. Clique no ícone de plug de energia na barra de estado para abrir o Monitor de série: ![monitor de série](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-monitor.jpg)
  
3. Na barra de estado, clique no número que representa a velocidade de transmissão e defina-o como `115200`: ![set-velocidade de transmissão.](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/set-baud-rate.jpg)

O Monitor de série apresenta todas as mensagens enviadas pelo sketch de exemplo. O sketch liga o DevKit ao Wi-Fi. Depois da ligação Wi-Fi for bem sucedida, o sketch envia uma mensagem para o mediador MQTT. Depois disso, o exemplo envia repetidamente duas mensagens de "iot.eclipse.org" utilizando o QoS 0 e 1 de QoS, respetivamente.

![saída de série](media/iot-hub-arduino-iot-devkit-az3166-mqtt-helloworld/serial-output.jpg)

## <a name="problems-and-feedback"></a>Problemas e comentários

Se tiver problemas, consulte [perguntas mais frequentes](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) ou estabelecer ligação utilizando as seguintes canais:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="see-also"></a>Consulte também

* [Ligar IoT DevKit AZ3166 ao IoT Hub do Azure na nuvem]({{"/docs/getting-started/" | absolute_url }})
* [Abanar, abanar para um Tweet]({{"/docs/projects/shake-shake/" | absolute_url }})

## <a name="next-steps"></a>Passos Seguintes

Agora que aprendeu como configurar o seu DevKit de Iot MXChip como um cliente MQTT e utilizar a biblioteca de clientes de MQTT para enviar mensagens para um mediador MQTT, Eis os passos sugeridos:

* [Monitorização remota do IoT solução acelerador descrição geral do Azure](https://docs.microsoft.com/azure/iot-suite/)
* [Ligue um dispositivo de MXChip IoT DevKit à sua aplicação do Azure IoT Central](https://docs.microsoft.com/microsoft-iot-central/howto-connect-devkit)
