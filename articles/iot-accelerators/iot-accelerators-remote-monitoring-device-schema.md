---
title: Esquema de dispositivos na solução de monitorização remota - Azure | Microsoft Docs
description: Este artigo descreve o esquema JSON que define um dispositivo simulado na solução de monitorização remota.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 1ba73d24aaa113a9124e17ea91946c205b21fba6
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627438"
---
# <a name="understand-the-device-model-schema"></a>Compreender o esquema do modelo de dispositivo

Pode utilizar dispositivos simulados na solução de monitorização remota para testar o respetivo comportamento. Ao implementar a solução de monitorização remota, uma coleção de dispositivos simulados é aprovisionada automaticamente. Pode personalizar os dispositivos simulados existentes ou criar os seus próprios.

Este artigo descreve o esquema do modelo de dispositivo Especifica as capacidades e o comportamento de um dispositivo simulado. O modelo de dispositivo é armazenado num ficheiro JSON.

Os artigos seguintes estão relacionadas com o artigo atual:

* [Implementar o comportamento de modelo do dispositivo](iot-accelerators-remote-monitoring-device-behavior.md) descreve os ficheiros de JavaScript que utilizar para implementar o comportamento de um dispositivo simulado.
* [Criar um novo dispositivo simulado](iot-accelerators-remote-monitoring-test.md) coloca-o em todos os conjunto e mostra-lhe como implementar um novo tipo de dispositivo simulado à sua solução.

Neste artigo, vai aprender a:

>[!div class="checklist"]
> * Utilizar um ficheiro JSON para definir um modelo de dispositivo simulada
> * Especifique as propriedades do dispositivo simulado
> * Especifique a dispositivo simulado envia a telemetria
> * Especifique os métodos de nuvem para o dispositivo, que o dispositivo responde a

## <a name="the-parts-of-the-device-model-schema"></a>As partes do esquema do modelo de dispositivo

Cada modelo de dispositivo, tal como um chiller ou camião, define um tipo de dispositivo simulado para ligar à solução de monitorização remota. O modelo de cada dispositivo é armazenado num ficheiro JSON com o esquema de nível superior seguinte:

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "elevator-01",
  "Version": "0.0.1",
  "Name": "Elevator",
  "Description": "Elevator with floor, vibration and temperature sensors.",
  "Protocol": "AMQP",
  "Simulation": {
    // Specify the simulation behavior
  },
  "Properties": {
    // Define properties
  },
  "Telemetry": [
    // Specify telemetry
  ],
  "CloudToDeviceMethods": {
    // Specify methods
  }
}
```

Pode ver os ficheiros de esquema para os dispositivos simulado de predefinição no [devicemodels pasta](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels) no GitHub.

A tabela seguinte descreve as entradas de nível superior do esquema:

| Entrada de esquema | Descrição |
| -- | --- |
| `SchemaVersion` | A versão do esquema é sempre `1.0.0` e é específico para o formato deste ficheiro. |
| `Id` | Um ID exclusivo para este modelo de dispositivo. |
| `Version` | Identifica a versão do modelo de dispositivo. |
| `Name` | Um nome amigável para o modelo do dispositivo. |
| `Description` | Uma descrição do modelo de dispositivo. |
| `Protocol` | Utiliza o protocolo de ligação do dispositivo. Pode ser um dos `AMQP`, `MQTT`, e `HTTP`. |

As secções seguintes descrevem as outras secções no esquema JSON:

## <a name="simulation"></a>Simulação

No `Simulation` secção, é possível definir o estado interno do dispositivo simulado. Quaisquer valores de telemetria enviadas pelo dispositivo devem fazer parte neste estado do dispositivo.

A definição do Estado do dispositivo tem dois elementos:

* `InitialState` Define valores iniciais para todas as propriedades do objeto de estado do dispositivo.
* `Script` identifica um ficheiro de JavaScript é executado numa agenda, para atualizar o estado do dispositivo. Pode utilizar este ficheiro de script para utilize uma ordem aleatória os valores de telemetria enviados pelo dispositivo.

Para saber mais sobre o ficheiro de JavaScript que atualiza o objeto de estado do dispositivo, consulte [compreender o comportamento de modelo do dispositivo](iot-accelerators-remote-monitoring-device-behavior.md).

O exemplo seguinte mostra a definição do objeto de estado do dispositivo para um dispositivo simulado chiller:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Interval": "00:00:10",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

O serviço de simulação é executado o **chiller-01-state.js** ficheiro a cada cinco segundos para atualizar o estado do dispositivo. Pode ver os ficheiros de JavaScript para os dispositivos simulado de predefinição no [pasta de scripts](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) no GitHub. Por convenção, estes ficheiros JavaScript tem o sufixo **-estado** para distingui-los a partir dos ficheiros que implementam os comportamentos de método.

## <a name="properties"></a>Propriedades

O `Properties` secção do esquema define os valores de propriedade, o dispositivo comunica à solução. Por exemplo:

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

Quando a solução é iniciada, consulta todos os dispositivos simulados para criar uma lista de `Type` valores a utilizar na IU. A solução utiliza o `Latitiude` e `Longitude` propriedades para adicionar a localização do dispositivo para o mapa no dashboard.

## <a name="telemetry"></a>Telemetria

O `Telemetry` matriz apresenta uma lista de todos os tipos de telemetria do dispositivo simulado envia à solução.

O exemplo seguinte envia uma mensagem de telemetria JSON cada 10 segundos com `floor`, `vibration`, e `temperature` dados de sensores a abordagem:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate": "{\"floor\":${floor},\"vibration\":${vibration},\"vibration_unit\":\"${vibration_unit}\",\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
    "MessageSchema": {
      "Name": "elevator-sensors;v1",
      "Format": "JSON",
      "Fields": {
        "floor": "integer",
        "vibration": "double",
        "vibration_unit": "text",
        "temperature": "double",
        "temperature_unit": "text"
      }
    }
  }
]
```

`MessageTemplate` Define a estrutura da mensagem JSON enviada pelo dispositivo simulado. Os marcadores de posição no `MessageTemplate` utilize a sintaxe `${NAME}` onde `NAME` é uma chave do [objeto de estado do dispositivo](#simulation). Devem estar entre aspas cadeias, números não devem.

`MessageSchema` Define o esquema da mensagem enviada pelo dispositivo simulado. O esquema de mensagem também é publicado ao IoT Hub para ativar as aplicações de back-end reutilizar as informações para interpretar a telemetria de entrada.

Atualmente, só pode utilizar esquemas de mensagem JSON. Os campos indicados no esquema podem ser dos seguintes tipos:

* Objeto - serializado utilizando JSON
* Binário - serializado através de base64
* Texto
* Booleano
* Número inteiro
* duplo
* DateTime

Para enviar mensagens de telemetria intervalos diferentes, adicionar vários tipos de telemetria para o `Telemetry` matriz. O exemplo seguinte envia dados relativos à temperatura e humidade cada 10 segundos e o estado de cada minuto claro:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate":
      "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"humidity\":\"${humidity}\"}",
    "MessageSchema": {
      "Name": "RoomComfort;v1",
      "Format": "JSON",
      "Fields": {
        "temperature": "double",
        "temperature_unit": "text",
        "humidity": "integer"
      }
    }
  },
  {
    "Interval": "00:01:00",
    "MessageTemplate": "{\"lights\":${lights_on}}",
    "MessageSchema": {
      "Name": "RoomLights;v1",
      "Format": "JSON",
      "Fields": {
        "lights": "boolean"
      }
    }
  }
],
```

## <a name="cloudtodevicemethods"></a>CloudToDeviceMethods

Um dispositivo simulado pode responder a chamada a partir da solução de monitorização remota de métodos de nuvem para o dispositivo. O `CloudToDeviceMethods` secção no ficheiro de esquema do modelo de dispositivo:

* Define os métodos do que dispositivo simulado pode responder.
* Identifica o ficheiro de JavaScript que contém a lógica de execução.

O dispositivo simulado envia a lista de métodos suporta à solução de monitorização remota.

Para saber mais sobre o ficheiro de JavaScript que implementa o comportamento do dispositivo, consulte [compreender o comportamento de modelo do dispositivo](iot-accelerators-remote-monitoring-device-behavior.md).

O exemplo seguinte especifica três métodos suportados e os ficheiros de JavaScript que implementam os métodos:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Pode ver os ficheiros de JavaScript para os dispositivos simulado de predefinição no [pasta de scripts](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) no GitHub. Por convenção, estes ficheiros JavaScript tem o sufixo **-método** para distingui-los a partir dos ficheiros que implementam o comportamento de estado.

## <a name="next-steps"></a>Passos Seguintes

Este artigo descrito como criar o seu próprio modelo personalizado dispositivo simulado. Este artigo mostrou, como para:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Utilizar um ficheiro JSON para definir um modelo de dispositivo simulada
> * Especifique as propriedades do dispositivo simulado
> * Especifique a dispositivo simulado envia a telemetria
> * Especifique os métodos de nuvem para o dispositivo, que o dispositivo responde a

Agora que aprendeu sobre o esquema JSON, o passo seguinte sugerido é saber como [implementa o comportamento do seu dispositivo simulado](iot-accelerators-remote-monitoring-device-behavior.md).

Para obter mais informações de programador sobre a solução de monitorização remota, consulte:

* [Guia de Referência para Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Guia de Resolução de Problemas de Programadores](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->