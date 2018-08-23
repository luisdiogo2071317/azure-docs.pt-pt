---
title: Esquema de grelha de eventos do Azure para o IoT Hub | Documentos da Microsoft
description: Página de referência para o formato de esquema de eventos e propriedades do IoT Hub
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.service: event-grid
ms.topic: reference
ms.date: 08/17/2018
ms.author: kgremban
ms.openlocfilehash: 4bb33eae53d31701b66d13cb4e810b1a0b8a4b0b
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055015"
---
# <a name="azure-event-grid-event-schema-for-iot-hub"></a>Esquema de eventos do Azure Event Grid para o IoT Hub

Este artigo fornece as propriedades e o esquema para eventos do IoT Hub do Azure. Para obter uma introdução aos esquemas de eventos, consulte [esquema de eventos do Azure Event Grid](event-schema.md). 

Para obter uma lista de scripts de exemplo e tutoriais, consulte [origem de evento do IoT Hub](event-sources.md#iot-hub).

## <a name="available-event-types"></a>Tipos de eventos disponíveis

O IoT Hub do Azure emite os seguintes tipos de evento:

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Publicado quando um dispositivo é registado para um hub IoT. |
| Microsoft.Devices.DeviceDeleted | Publicado quando um dispositivo é eliminado de um hub IoT. | 

## <a name="example-event"></a>Evento de exemplo

O esquema para eventos DeviceCreated e DeviceDeleted têm a mesma estrutura. Este evento de exemplo mostra o esquema de um evento que ocorre quando um dispositivo é registado para um hub IoT:

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
      "status": "enabled",
      "statusUpdateTime": "0001-01-01T00:00:00",
      "connectionState": "Disconnected",
      "lastActivityTime": "0001-01-01T00:00:00",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "sas",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "version": 2,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "operationTimestamp": "2018-01-02T19:17:44.4383997Z",
    "opType": "DeviceCreated"
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="event-properties"></a>Propriedades do evento

Todos os eventos contêm os mesmos dados de nível superior: 

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| ID | cadeia | Identificador exclusivo para o evento. |
| tópico | cadeia | Caminho de recurso completo para a origem do evento. Este campo não é gravável. Event Grid fornece este valor. |
| Assunto | cadeia | Caminho definidos pelo publicador para o assunto de evento. |
| eventType | cadeia | Um dos tipos de eventos registrados para esta origem de evento. |
| eventTime | cadeia | O tempo que o evento é gerado com base no fuso horário UTC do fornecedor. |
| dados | objeto | Dados de eventos do IoT Hub.  |
| dataVersion | cadeia | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | cadeia | A versão do esquema dos metadados do evento. Grelha de eventos define o esquema das propriedades de nível superior. Event Grid fornece este valor. |

O conteúdo do objeto de dados é diferente para cada editor de eventos. Eventos do IoT, Hub o objeto de dados contém as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| HubName | cadeia | Nome do IoT Hub em que o dispositivo foi criado ou eliminado. |
| deviceId | cadeia | O identificador exclusivo do dispositivo. Esta cadeia de maiúsculas e minúsculas pode ter até 128 carateres e suporta carateres de alfanuméricos ASCII de 7 bits, bem como os seguintes carateres especiais: `- : . + % _ # * ? ! ( ) , = @ ; $ '`. |
| operationTimestamp | cadeia | O carimbo de hora ISO8601 da operação. |
| opType | cadeia | O tipo de evento especificado para esta operação pelo IoT Hub: seja `DeviceCreated` ou `DeviceDeleted`.
| duplo | objeto | Informações sobre o dispositivo duplo, que é o represenation de cloud de metadados de dispositivo do aplicativo. | 
| deviceID | cadeia | O identificador exclusivo do dispositivo duplo. | 
| ETag | cadeia | Uma parte da informação que descreve o conteúdo do dispositivo duplo. Cada etag é garantido que seja exclusivo por dispositivo duplo. | 
| status | cadeia | Se o dispositivo duplo está ativado ou desativado. | 
| statusUpdateTime | cadeia | Atualizar o carimbo de hora ISO8601 do último Estado de twin do dispositivo. |
| connectionState | cadeia | Se o dispositivo está ligado ou desligado. | 
| lastActivityTime | cadeia | O carimbo de hora ISO8601 da última atividade. | 
| cloudToDeviceMessageCount | inteiro | Contagem da cloud para mensagens de dispositivo enviadas para este dispositivo. | 
| authenticationType | cadeia | Tipo de autenticação utilizado para este dispositivo: ambos `SAS`, `SelfSigned`, ou `CertificateAuthority`. |
| x509Thumbprint | cadeia | O thumbprint é um valor exclusivo para o x509 certificado, frequentemente utilizado para localizar um determinado certificado num arquivo de certificados. O thumbprint é gerado dinamicamente usando o algoritmo SHA1 e não existem fisicamente no certificado. | 
| primaryThumbprint | cadeia | Thumbprint primário para o x509 certificado. |
| secondaryThumbprint | cadeia | Thumbprint secundário para o x509 certificado. | 
| versão | inteiro | Um número inteiro que é incrementado em um cada vez que o dispositivo duplo é atualizado. |
| pretendido | objeto | Uma parte das propriedades que podem ser gravados apenas pelo back-end da aplicação e ler pelo dispositivo. | 
| comunicado | objeto | Uma parte das propriedades que pode ser escrito apenas pelo dispositivo e ler o back-end da aplicação. |
| lastUpdated | cadeia | Atualizar o carimbo de hora ISO8601 da última propriedade de twin do dispositivo. | 

## <a name="next-steps"></a>Passos Seguintes

* Para obter uma introdução ao Azure Event Grid, consulte [o que é o Event Grid?](overview.md)
* Para saber mais sobre como o IoT Hub e o Event Grid funcionam em conjunto, consulte [reagir a eventos do IoT Hub com o Event Grid para realizar ações](../iot-hub/iot-hub-event-grid.md).