---
title: Exportar os dados para os Hubs de eventos do Azure e o Azure Service Bus | Documentos da Microsoft
description: Como exportar dados da sua aplicação do Azure IoT Central para os Hubs de eventos do Azure e o Azure Service Bus
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 14b51f109ca76661ac10c99d42002dda45bc0500
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318717"
---
# <a name="export-your-data-in-azure-iot-central"></a>Exportar os dados no Azure IoT Central

*Este tópico aplica-se aos administradores.*

Este artigo se aprofunda-se em como usar a funcionalidade de exportação contínua de dados no Azure IoT Central para exportar os dados para os seus próprios **os Hubs de eventos do Azure**, e **do Azure Service Bus** instâncias. Pode exportar **medidas**, **dispositivos**, e **modelos de dispositivos** para o seu destino para caminho de acesso pouco frequente informações e análises. Isto inclui acionar as regras personalizadas no Azure Stream Analytics, acionar fluxos de trabalho personalizados no Azure Logic Apps, ou transformar os dados e transmitir através de funções do Azure. 

> [!Note]
> Mais uma vez, quando ativar a exportação contínua de dados, obtém apenas os dados a partir desse momento ou superior. Atualmente, não poderá obter dados para um tempo quando a exportação de dados contínua foi desativada. Para manter mais dados históricos, ative a exportação contínua de dados desde o início.


## <a name="prerequisites"></a>Pré-requisitos

- Tem de ser um administrador na sua aplicação do Centro de IoT

## <a name="export-to-azure-event-hubs-and-azure-service-bus"></a>Exportar para os Hubs de eventos do Azure e o Azure Service Bus

Medidas, dispositivos e dados de modelos de dispositivos são exportados para o seu hub de eventos ou fila do Service Bus ou tópico em tempo quase real. Dados de medidas exportado contém a totalidade da mensagem de seus dispositivos enviados ao IoT Central, não apenas os valores de medidas de si. Dispositivos exportados dados contém alterações às propriedades e configurações de todos os dispositivos e modelos de dispositivos exportado contém alterações para todos os modelos de dispositivos. Os dados exportados é na propriedade "corpo" e estão no formato JSON.

> [!NOTE]
> Ao escolher um barramento de serviço como um destino de exportação, as filas e tópicos **não tem de ter sessões ou de deteção de duplicados ativada**. Se qualquer uma dessas opções estiverem ativada, algumas mensagens não chegam a sua fila ou tópico.

### <a name="measurements"></a>Medições

Uma nova mensagem é exportada rapidamente depois do IoT Central recebe a mensagem a partir de um dispositivo. Cada mensagem exportada no Service Bus e Hubs de eventos contém a mensagem completa do dispositivo enviada na propriedade "corpo" no formato JSON. 

> [!NOTE]
> Os dispositivos que enviam as medidas são representados por identificações de dispositivo (consulte as seções a seguir). Para obter os nomes dos dispositivos, exportar dados de dispositivo e correlacionar cada salvação utilizando o **connectionDeviceId** que corresponda à **deviceId** da mensagem de dispositivo.

O exemplo seguinte mostra o recebida uma mensagem sobre dados de medidas no hub de eventos ou fila do Service Bus ou tópico.

```json
{
  "body": {
    "humidity": 29.06963648666288,
    "temp": 8.4503795661685,
    "pressure": 1075.8334910110093,
    "magnetometerX": 408.6966458887116,
    "magnetometerY": -532.8809796603962,
    "magnetometerZ": 174.70653875528205,
    "accelerometerX": 1481.546749013788,
    "accelerometerY": -1139.4316656437406,
    "accelerometerZ": 811.6928695575307,
    "gyroscopeX": 442.19879163299856,
    "gyroscopeY": 123.23710975717177,
    "gyroscopeZ": 708.5397575786151,
    "deviceState": "DANGER"
  },
  "annotations": {
    "iothub-connection-device-id": "<connectionDeviceId>",
    "iothub-connection-auth-method": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id": "<generationId>",
    "iothub-enqueuedtime": 1539381029965,
    "iothub-message-source": "Telemetry",
    "x-opt-sequence-number": 25325,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539381030200
  },
  "sequenceNumber": 25325,
  "enqueuedTimeUtc": "2018-10-12T21:50:30.200Z",
  "offset": "<offset>",
  "properties": {
    "content_type": "application/json",
    "content_encoding": "utf-8"
  }
}
```

### <a name="devices"></a>Dispositivos

Mensagens que contêm dados do dispositivo são enviadas ao seu hub de eventos ou fila do Service Bus ou tópico, uma vez intervalos de poucos minutos. Isso significa que intervalos de poucos minutos, um lote de mensagens vai deparar-se com os dados sobre
- Novos dispositivos que foram adicionados
- Dispositivos com a propriedade alterada e definir valores

Cada mensagem representa uma ou mais alterações a um dispositivo desde a última mensagem exportada. As informações que serão enviadas em cada mensagem incluem:
- `id` do dispositivo no Centro de IoT
- `name` do dispositivo
- `deviceId` de [serviço aprovisionamento de dispositivos](https://aka.ms/iotcentraldocsdps)
- Informações de modelo do dispositivo
- Valores de propriedade
- Valores de definição

> [!NOTE]
> Dispositivos eliminados, uma vez que o último lote não são exportadas. Atualmente, existem sem indicadores na exportados mensagens para dispositivos eliminados.
>
> O modelo de dispositivo que cada dispositivo pertence à é representado por um ID de modelo do dispositivo. Para obter o nome do modelo de dispositivo, certifique-se de que a exportar dados de modelo do dispositivo demasiado.

O exemplo seguinte mostra uma mensagem sobre dados de dispositivos no hub de eventos ou fila do Service Bus ou tópico:


```json
{
  "body": {
    "id": "<id>",
    "name": "<deviceName>",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
      "id": "<templateId>",
      "version": "1.0.0"
    },
    "properties": {
      "cloud": {
        "location": "Seattle"
      },
      "device": {
        "dieNumber": 5445.5862873026645
      }
    },
    "settings": {
      "device": {
        "fanSpeed": 0
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "devices",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 39740,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274959654
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 39740,
  "enqueuedTimeUtc": "2018-10-11T16:22:39.654Z",
  "offset": "<offset>",
}
```

### <a name="device-templates"></a>Modelos de dispositivos

Mensagens que contêm dados de modelos de dispositivo são enviadas ao seu hub de eventos ou fila do Service Bus ou tópico, uma vez intervalos de poucos minutos. Isso significa que intervalos de poucos minutos, um lote de mensagens vai deparar-se com os dados sobre
- Novos modelos de dispositivos que foram adicionados
- Modelos de dispositivos com medidas alteradas, propriedade e as definições de configuração

Cada mensagem representa uma ou mais alterações a um modelo de dispositivo desde a última mensagem exportada. As informações que serão enviadas em cada mensagem incluem:
- `id` o modelo de dispositivo
- `name` o modelo de dispositivo
- `version` o modelo de dispositivo
- Tipos de dados de medição e valores mínimos/máximos
- Tipos de dados de propriedade e os valores predefinidos
- Definir tipos de dados e valores predefinidos

> [!NOTE]
> Modelos de dispositivos eliminados desde o último lote não são exportados. Atualmente, existem sem indicadores na exportados mensagens para modelos de dispositivos eliminada.

O exemplo seguinte mostra uma mensagem sobre dados de modelos de dispositivos no hub de eventos ou fila do Service Bus ou tópico:

```json
{
  "body": {
    "id": "<id>",
    "version": "1.0.0",
    "name": "<templateName>",
    "measurements": {
      "telemetry": {
        "humidity": {
          "dataType": "double",
          "name": "humidity"
        },
        "pressure": {
          "dataType": "double",
          "name": "pressure"
        },
        "temp": {
          "dataType": "double",
          "name": "temperature"
        }
      }
    },
    "properties": {
      "cloud": {
        "location": {
          "dataType": "string",
          "name": "Location"
        }
      },
      "device": {
        "dieNumber": {
          "dataType": "double",
          "name": "Die Number"
        }
      }
    },
    "settings": {
      "device": {
        "fanSpeed": {
          "dataType": "double",
          "name": "Fan Speed",
          "initialValue": 0
        }
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "deviceTemplates",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 25315,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274985085
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 25315,
  "enqueuedTimeUtc": "2018-10-11T16:23:05.085Z",
  "offset": "<offset>",
}
```

## <a name="next-steps"></a>Passos Seguintes

Agora que sabe como exportar os dados para os Hubs de eventos do Azure e o Azure Service Bus, avance para o passo seguinte:

> [!div class="nextstepaction"]
> [Como acionar as funções do Azure](howto-trigger-azure-functions.md)
