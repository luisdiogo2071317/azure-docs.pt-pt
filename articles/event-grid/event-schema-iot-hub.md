---
title: Esquema de grelha de eventos do Azure para o IoT Hub | Microsoft Docs
description: Página de referência para o formato de esquema de eventos e as propriedades do IoT Hub
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.service: event-grid
ms.topic: reference
ms.date: 01/30/2018
ms.author: kgremban
ms.openlocfilehash: 812ca3ba546112f54a76319fda853d441ce34f1b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
ms.locfileid: "34303543"
---
# <a name="azure-event-grid-event-schema-for-iot-hub"></a>Esquema de eventos de grelha de eventos do Azure para o IoT Hub

Este artigo fornece as propriedades e esquema de eventos do IoT Hub do Azure. Para uma introdução para esquemas de eventos, consulte [esquema de eventos de grelha de eventos do Azure](event-schema.md). 

## <a name="available-event-types"></a>Tipos de evento disponíveis

IoT Hub do Azure emite os seguintes tipos de evento:

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Publicado quando um dispositivo é registado para um hub IoT. |
| Microsoft.Devices.DeviceDeleted | Publicado quando um dispositivo é eliminado a partir de um hub IoT. | 

## <a name="example-event"></a>Eventos de exemplo

O esquema para eventos DeviceCreated e DeviceDeleted ter a mesma estrutura. Este evento de exemplo mostra o esquema de um evento é desencadeado quando um dispositivo é registado para um hub IoT:

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

Todos os eventos contêm o mesmo nível superior de dados: 

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| ID | cadeia | Identificador exclusivo para o evento. |
| Tópico | cadeia | Caminho de recurso completo para a origem do evento. Este campo não é passível de escrita. Grelha de evento fornece este valor. |
| Requerente | cadeia | Caminho definida pelo fabricante para o assunto do evento. |
| eventType | cadeia | Um dos tipos de eventos registados para esta origem de evento. |
| eventTime | cadeia | A hora que do evento é gerado com base na hora UTC do fornecedor. |
| dados | objeto | Dados de eventos do IoT Hub.  |
| dataVersion | cadeia | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | cadeia | A versão do esquema dos metadados do evento. Grelha de evento define o esquema das propriedades de nível superior. Grelha de evento fornece este valor. |

O conteúdo do objeto de dados é diferente para cada publicador de eventos. Para eventos de IoT Hub, o objeto de dados contém as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| hubName | cadeia | Nome do IoT Hub onde o dispositivo foi criado ou eliminado. |
| deviceId | cadeia | O identificador exclusivo do dispositivo. Esta cadeia de maiúsculas e minúsculas pode ter até 128 carateres e suporta carateres alfanuméricos ASCII 7 bits plus os seguintes carateres especiais: `- : . + % _ # * ? ! ( ) , = @ ; $ '`. |
| operationTimestamp | cadeia | O carimbo de ISO8601 da operação. |
| opType | cadeia | O tipo de evento especificado para esta operação pelo IoT Hub: o `DeviceCreated` ou `DeviceDeleted`.
| duplo | objeto | Informações sobre o dispositivo duplo, que é o represenation de nuvem de metadados do dispositivo de aplicação. | 
| deviceID | cadeia | O identificador exclusivo do dispositivo duplo. | 
| ETag | cadeia | Um elemento de informação que descreva o conteúdo do dispositivo duplo. Cada etag é garantido ser únicos por dispositivo duplo. | 
| status | cadeia | Indica se o dispositivo duplo é ativado ou desativado. | 
| statusUpdateTime | cadeia | Atualizar o carimbo de ISO8601 o último duplo do estado dos dispositivos. |
| connectionState | cadeia | Se o dispositivo está ligado ou desligado. | 
| lastActivityTime | cadeia | O carimbo de ISO8601 da última atividade. | 
| cloudToDeviceMessageCount | inteiro | Contagem de nuvem para as mensagens do dispositivo enviadas para este dispositivo. | 
| authenticationType | cadeia | Tipo de autenticação utilizado para este dispositivo: um `SAS`, `SelfSigned`, ou `CertificateAuthority`. |
| X509Thumbprint | cadeia | O thumbprint é um valor exclusivo para o x509 certificado, normalmente utilizado para localizar um determinado certificado num arquivo de certificados. O thumbprint dinamicamente é gerado utilizando o algoritmo SHA1 e não existe fisicamente no certificado. | 
| primaryThumbprint | cadeia | Thumbprint primário para o x509 certificado. |
| secondaryThumbprint | cadeia | Thumbprint secundário para o x509 certificado. | 
| versão | inteiro | Tempo de um número inteiro que é aumentado em cada dispositivo duplo é atualizado. |
| pretendida | objeto | Uma parte das propriedades que podem ser escritos apenas pelo back-end da aplicação e ler pelo dispositivo. | 
| comunicado | objeto | Uma parte das propriedades que podem ser escritos apenas pelo dispositivo e o back-end da aplicação de leitura. |
| lastUpdated | cadeia | Atualizar o carimbo de ISO8601 da propriedade de duplo última do dispositivo. | 

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos do Azure, consulte [Novidades grelha de evento?](overview.md)
* Para saber mais sobre como o IoT Hub e eventos grelha funcionam em conjunto, consulte [reagir a eventos de IoT Hub, utilizando a grelha de evento a acionar ações](../iot-hub/iot-hub-event-grid.md).