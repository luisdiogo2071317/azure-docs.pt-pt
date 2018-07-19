---
title: Compreender o formato de mensagem do IoT Hub do Azure | Documentos da Microsoft
description: Guia do programador - descibes o formato e o conteúdo esperado de mensagens do IoT Hub.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: a1296565384e60117d883a1f1407362482ba1a3e
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125018"
---
# <a name="create-and-read-iot-hub-messages"></a>Criar e ler as mensagens de IoT Hub

Para suportar a perfeita interoperabilidade entre protocolos, o IoT Hub define um formato de mensagem comum para todos os protocolos de voltado para o dispositivo. Esse formato de mensagem é usado para ambos [dispositivo-para-cloud] [ lnk-d2c] e [cloud-para-dispositivo] [ lnk-c2d] mensagens. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Uma [mensagem do IoT Hub] [ lnk-messaging] consiste em:

* Um conjunto de *propriedades do sistema*. Propriedades que o IoT Hub interpreta ou define. Este conjunto é predeterminado.
* Um conjunto de *propriedades da aplicação*. Um dicionário de propriedades de cadeia de caracteres que pode definir o aplicativo e de acesso, sem a necessidade de desserializar o corpo da mensagem. IoT Hub nunca modifica essas propriedades.
* Um corpo de binário opaco.

Valores e nomes de propriedade só podem conter carateres alfanuméricos ASCII, além de ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` quando:  

* Envie mensagens dispositivo-para-cloud utilizando o protocolo HTTPS.
* Envie mensagens da cloud para o dispositivo.

Para obter mais informações sobre como codificar e decodificar as mensagens enviadas através de protocolos diferentes, consulte [SDKs do Azure IoT][lnk-sdks].

A tabela seguinte lista o conjunto de propriedades do sistema de mensagens do IoT Hub.

| Propriedade | Descrição |
| --- | --- |
| MessageId |Um identificador de utilizador definível para a mensagem utilizado para padrões de solicitação-resposta. : Uma diferencia maiúsculas de minúsculas cadeia de formato (Até 128 carateres de comprimento) de carateres de alfanuméricos ASCII de 7 bits + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| Número de sequência |Um número de (exclusivo por fila de dispositivo) atribuído pelo IoT Hub para cada mensagem de cloud-para-dispositivo. |
| Para |Um destino especificado na [Cloud-para-dispositivo] [ lnk-c2d] mensagens. |
| ExpiryTimeUtc |Data e hora de expiração de mensagem. |
| EnqueuedTime |Data e hora a [Cloud-para-dispositivo] [ lnk-c2d] mensagem foi recebida pelo IoT Hub. |
| CorrelationId |Uma propriedade de cadeia de caracteres numa mensagem de resposta que contém, normalmente, a MessageId da solicitação, em padrões de solicitação-resposta. |
| UserId |Um ID utilizado para especificar a origem de mensagens. Quando as mensagens são geradas pelo IoT Hub, ele é definido como `{iot hub name}`. |
| ACK |Um gerador de mensagem de comentários. Esta propriedade é utilizada nas mensagens de cloud para o dispositivo ao IoT Hub para gerar mensagens de comentários como resultado o consumo da mensagem do pedido pelo dispositivo. Valores possíveis: **none** (predefinição): nenhuma mensagem de comentários é gerada, **positivo**: receber uma mensagem de comentários, se a mensagem foi concluída, **negativo**: receba uma mensagem de comentários, se a mensagem expirou (ou contagem máxima de entrega foi atingida) sem que está a ser concluída por dispositivo, ou **completo**: positivos e negativos. Para obter mais informações, consulte [comentários da mensagem][lnk-feedback]. |
| ConnectionDeviceId |Um ID definido pelo IoT Hub nas mensagens do dispositivo para a cloud. Contém os **deviceId** do dispositivo que enviou a mensagem. |
| ConnectionDeviceGenerationId |Um ID definido pelo IoT Hub nas mensagens do dispositivo para a cloud. Contém a **generationId** (como por [das propriedades de identidade do dispositivo][lnk-device-properties]) do dispositivo que enviou a mensagem. |
| ConnectionAuthMethod |Um método de autenticação definido pelo IoT Hub nas mensagens do dispositivo para a cloud. Esta propriedade contém informações sobre o método de autenticação utilizado para autenticar o dispositivo que envia a mensagem. Para obter mais informações, consulte [dispositivo para a cloud antisspoofing][lnk-antispoofing]. |
| CreationTimeUtc | Data e hora que a mensagem foi criada num dispositivo. Um dispositivo tem de definir este valor explicitamente. |

## <a name="message-size"></a>Tamanho da mensagem

IoT Hub mede o tamanho da mensagem de forma independente de protocolo, Considerando apenas a carga real. O tamanho em bytes é calculado como a soma dos seguintes procedimentos:

* O tamanho do corpo em bytes.
* O tamanho em bytes de todos os valores de propriedades do sistema de mensagem.
* O tamanho em bytes de todos os valores e nomes de propriedade do utilizador.

Os nomes das propriedades e os valores são limitadas aos caracteres ASCII, para que o comprimento das cadeias de caracteres é igual ao tamanho em bytes.

## <a name="next-steps"></a>Passos Seguintes

Para obter informações sobre limites de tamanho de mensagem do IoT Hub, veja [IoT Hub quotas e limitação][lnk-quotas].

Para saber como criar e ler mensagens em várias linguagens de programação de IoT Hub, veja a [inícios Rápidos][lnk-get-started].

[lnk-messaging]: iot-hub-devguide-messaging.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-get-started]: quickstart-send-telemetry-node.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-feedback]: iot-hub-devguide-messages-c2d.md#message-feedback
[lnk-device-properties]: iot-hub-devguide-identity-registry.md#device-identity-properties
[lnk-antispoofing]: iot-hub-devguide-messages-d2c.md#anti-spoofing-properties
