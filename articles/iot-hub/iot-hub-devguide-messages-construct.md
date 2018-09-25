---
title: Compreender o formato de mensagem do IoT Hub do Azure | Documentos da Microsoft
description: Guia do programador - descibes o formato e o conteúdo esperado de mensagens do IoT Hub.
author: ash2017
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: asrastog
ms.openlocfilehash: edea20343c2a261902c082dbc5c96b78db6b470d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973225"
---
# <a name="create-and-read-iot-hub-messages"></a>Criar e ler mensagens do Hub IoT

Para suportar a perfeita interoperabilidade entre protocolos, o IoT Hub define um formato de mensagem comum para todos os protocolos de voltado para o dispositivo. Esse formato de mensagem é usado para ambos [dispositivo-para-cloud encaminhamento](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c) e [cloud-para-dispositivo](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-c2d) mensagens. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

IoT Hub implementa com um padrão de mensagens de transmissão em fluxo de mensagens do dispositivo para a cloud. Mensagens do dispositivo-para-cloud do IoT Hub são mais semelhantes [os Hubs de eventos](https://docs.microsoft.com/azure/event-hubs/) *eventos* que [do Service Bus](https://docs.microsoft.com/azure/service-bus-messaging/) *mensagens* em que existe um grande volume de eventos, passando por meio do serviço que pode ser lidos por vários leitores.

Uma mensagem do IoT Hub é composta por:
* Um conjunto predeterminado de *propriedades do sistema* conforme indicado abaixo.
* Um conjunto de *propriedades da aplicação*. Um dicionário de propriedades de cadeia de caracteres que pode definir o aplicativo e de acesso, sem a necessidade de desserializar o corpo da mensagem. IoT Hub nunca modifica essas propriedades.
* Um corpo de binário opaco.

Valores e nomes de propriedade só podem conter carateres alfanuméricos ASCII, além de ```{'!', '#', '$', '%, '&', "'", '*', '+', '-', '.', '^', '_', '`', '|', '~'}``` quando envia mensagens dispositivo-para-cloud com o HTTPS de protocolo ou enviam mensagens da cloud para o dispositivo.

Dispositivo-para-cloud mensagens com o IoT Hub com as seguintes características:

* Mensagens dispositivo-para-cloud são mantidos no padrão de um hub IoT e durável **mensagens/eventos** ponto final até sete dias.
* Mensagens do dispositivo para a cloud podem ter um máximo de 256 KB e podem ser agrupadas em lotes para otimizar envia. Lotes podem ser, no máximo, 256 KB.
* IoT Hub não permite a criação de partições arbitrário. Mensagens do dispositivo para cloud são particionadas com base no respetivo tipo de origem **deviceId**.
* Conforme explicado no [controlar o acesso ao IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security) secção, o IoT Hub permite o controlo de acesso e autenticação por dispositivo.

Para obter mais informações sobre como codificar e decodificar as mensagens enviadas através de protocolos diferentes, consulte [do Azure IoT SDKs](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks).

A tabela seguinte lista o conjunto de propriedades do sistema de mensagens do IoT Hub.

| Propriedade | Descrição | É utilizador definível? |
| --- | --- | --- |
| MessageId |Um identificador de utilizador definível para a mensagem utilizado para padrões de solicitação-resposta. : Uma diferencia maiúsculas de minúsculas cadeia de formato (Até 128 carateres de comprimento) de carateres de alfanuméricos ASCII de 7 bits + `{'-', ':',’.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. | Sim |
| Número de sequência |Um número de (exclusivo por fila de dispositivo) atribuído pelo IoT Hub para cada mensagem de cloud-para-dispositivo. | Não para mensagens de C2D; Sim, caso contrário. |
| Para |Um destino especificado nas mensagens de [Cloud-para-dispositivo] [lnk-c2d]. | Não para mensagens de C2D; Sim, caso contrário. |
| ExpiryTimeUtc |Data e hora de expiração de mensagem. | Sim |
| EnqueuedTime |Data e hora que a mensagem de [Cloud-para-dispositivo] [lnk-c2d] foi recebida pelo IoT Hub. | Não para mensagens de C2D; Sim, caso contrário. |
| CorrelationId |Uma propriedade de cadeia de caracteres numa mensagem de resposta que contém, normalmente, a MessageId da solicitação, em padrões de solicitação-resposta. | Sim |
| UserId |Um ID utilizado para especificar a origem de mensagens. Quando as mensagens são geradas pelo IoT Hub, ele é definido como `{iot hub name}`. | Não |
| ACK |Um gerador de mensagem de comentários. Esta propriedade é utilizada nas mensagens de cloud para o dispositivo ao IoT Hub para gerar mensagens de comentários como resultado o consumo da mensagem do pedido pelo dispositivo. Valores possíveis: **none** (predefinição): nenhuma mensagem de comentários é gerada, **positivo**: receber uma mensagem de comentários, se a mensagem foi concluída, **negativo**: receba uma mensagem de comentários, se a mensagem expirou (ou contagem máxima de entrega foi atingida) sem que está a ser concluída por dispositivo, ou **completo**: positivos e negativos. Para obter mais informações, consulte [comentários de mensagem] [lnk-comentários]. | Sim |
| ConnectionDeviceId |Um ID definido pelo IoT Hub nas mensagens do dispositivo para a cloud. Contém os **deviceId** do dispositivo que enviou a mensagem. | Não para mensagens D2C; Sim, caso contrário. |
| ConnectionDeviceGenerationId |Um ID definido pelo IoT Hub nas mensagens do dispositivo para a cloud. Contém os **generationId** (de acordo com [properties][lnk-device-properties]) de identidade de dispositivo do dispositivo que enviou a mensagem. | Não para mensagens D2C; Sim, caso contrário. |
| ConnectionAuthMethod |Um método de autenticação definido pelo IoT Hub nas mensagens do dispositivo para a cloud. Esta propriedade contém informações sobre o método de autenticação utilizado para autenticar o dispositivo que envia a mensagem. Para obter mais informações, consulte [dispositivo para a cloud antisspoofing] [lnk-antispoofing]. | Não para mensagens D2C; Sim, caso contrário. |
| CreationTimeUtc | Data e hora que a mensagem foi criada num dispositivo. Um dispositivo tem de definir este valor explicitamente. | Sim |

## <a name="message-size"></a>Tamanho da mensagem

IoT Hub mede o tamanho da mensagem de forma independente de protocolo, Considerando apenas a carga real. O tamanho em bytes é calculado como a soma dos seguintes procedimentos:

* O tamanho do corpo em bytes.
* O tamanho em bytes de todos os valores de propriedades do sistema de mensagem.
* O tamanho em bytes de todos os valores e nomes de propriedade do utilizador.

Os nomes das propriedades e os valores são limitadas aos caracteres ASCII, para que o comprimento das cadeias de caracteres é igual ao tamanho em bytes.

## <a name="anti-spoofing-properties"></a>Propriedades anti-spoofing

Para evitar falsificação nas mensagens do dispositivo para a cloud, IoT Hub do dispositivo os carimbos de todas as mensagens com as seguintes propriedades:

* **ConnectionDeviceId**
* **ConnectionDeviceGenerationId**
* **ConnectionAuthMethod**

Os dois primeiros contêm os **deviceId** e **generationId** do dispositivo de origem, como por [propriedades de identidade de dispositivo](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-identity-registry#device-identity-properties).

O **ConnectionAuthMethod** propriedade contém um objeto JSON serializado com as seguintes propriedades:

```json
{
  "scope": "{ hub | device }",
  "type": "{ symkey | sas | x509 }",
  "issuer": "iothub"
}
```

## <a name="next-steps"></a>Passos Seguintes

* Para obter informações sobre limites de tamanho de mensagem do IoT Hub, veja [IoT Hub quotas e limitação](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-quotas-throttling).
* Para saber como criar e ler mensagens em várias linguagens de programação de IoT Hub, veja a [inícios Rápidos](https://docs.microsoft.com/azure/iot-hub/quickstart-send-telemetry-node).