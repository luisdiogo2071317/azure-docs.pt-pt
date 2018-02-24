---
title: IoT Hub do Azure e a grelha de eventos | Microsoft Docs
description: "Utilize a grelha de eventos do Azure para acionar processos com base nas ações que ocorrem no IoT Hub."
services: iot-hub
documentationcenter: 
author: kgremban
manager: timlt
editor: 
ms.service: iot-hub
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/14/2018
ms.author: kgremban
ms.openlocfilehash: 7c75a65714898f27ab0008ad5a30a5714d7174f4
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions---preview"></a>Reagir a eventos de IoT Hub, utilizando a grelha de evento a acionar ações - pré-visualização

IoT Hub do Azure integra grelha de eventos do Azure para que possa enviar notificações de eventos para outros serviços e acionar processos a jusante. Configure as suas aplicações empresariais para escutar eventos de IoT Hub para que possam reagir a eventos críticos de forma segura, fiável e escalável. Por exemplo, crie uma aplicação para efetuar várias ações como atualizar uma base de dados, criar um pedido e entrega de uma notificação por e-mail sempre que é registado um novo dispositivo IoT ao seu IoT hub. 

[Grelha de eventos do Azure] [ lnk-eg-overview] é um serviço de encaminhamento de eventos totalmente gerido que utiliza uma publicação-subscrição modelo. Grelha de evento têm suporte incorporado para serviços do Azure como [das funções do Azure](../azure-functions/functions-overview.md) e [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md)e pode fornecer a alertas de eventos para serviços do Azure não utilizando webhooks. Para obter uma lista completa de processadores de eventos que suporta a grelha de eventos, consulte [uma introdução à grelha de eventos do Azure][lnk-eg-overview]. 

![Arquitetura de grelha de eventos do Azure](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Disponibilidade regional

A integração de grelha de eventos está disponível para os hubs IoT localizados em regiões onde o evento grelha é suportada. Para obter a lista mais recente das regiões, consulte [uma introdução à grelha de eventos do Azure][lnk-eg-overview]. 

## <a name="event-types"></a>Tipos de evento

IoT Hub publica os seguintes tipos de evento: 

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Publicado quando um dispositivo é registado para um hub IoT. |
| Microsoft.Devices.DeviceDeleted | Publicado quando um dispositivo é eliminado a partir de um hub IoT. | 

Utilize o portal do Azure ou a CLI do Azure para configurar os eventos para publicar a partir do cada IoT hub. Por exemplo, experimente o tutorial [enviar notificações de correio eletrónico sobre eventos de IoT Hub do Azure utilizando as Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md). 

## <a name="event-schema"></a>Esquema de eventos

Os eventos de IoT Hub incluem todas as informações que necessárias para responder a alterações no seu ciclo de vida do dispositivo. Pode identificar um evento de IoT Hub ao verificar que a propriedade eventType começa com **Microsoft.Devices**. Para obter mais informações sobre como utilizar as propriedades de evento de grelha de eventos, consulte o [esquema de eventos de evento grelha](../event-grid/event-schema.md).

### <a name="device-created-schema"></a>Esquema de dispositivos criada

O exemplo seguinte mostra o esquema de um dispositivo criado eventos: 

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

Para obter uma descrição detalhada de cada propriedade, consulte [esquema de eventos de grelha de eventos do Azure para o IoT Hub](../event-grid/event-schema-iot-hub.md)

## <a name="filter-events"></a>Filtrar eventos

Subscrições de eventos do IoT Hub podem filtrar eventos com base no nome de dispositivo e o tipo de evento. Filtros de requerente em projetos de grelha de eventos com base no **prefixo** e **sufixo** corresponde. O filtro utiliza um `AND` operador, pelo que os eventos com um assunto que correspondem o prefixo e o sufixo são entregues para o subscritor. 

O requerente de eventos de IoT utiliza o formato:

```json
devices/{deviceId}
```

### <a name="tips-for-consuming-events"></a>Sugestões de consumo de eventos

As aplicações que processam eventos de IoT Hub devem seguir estas práticas sugeridas:

* Várias subscrições podem ser configuradas para eventos de rota para o mesmo processador de eventos, pelo que é importante não partem do princípio de que os eventos são de uma origem específica. Verifique sempre o tópico mensagem para se certificar de que são provenientes do hub IoT que pretende. 
* Não parta do princípio que recebe de todos os eventos são os tipos de que pretende. Verifique sempre o eventType antes de processamento da mensagem.
* Podem chegam mensagens fora de ordem ou após um atraso. Utilize o campo de etag para compreender se as informações sobre os objetos estão atualizadas.



## <a name="next-steps"></a>Passos Seguintes

* [Experimente o tutorial de eventos do IoT Hub](../event-grid/publish-iot-hub-events-to-logic-apps.md)
* [Saiba mais sobre a grelha de eventos][lnk-eg-overview]
* [Comparar as diferenças entre o encaminhamento de IoT Hub eventos e mensagens][lnk-eg-compare]

<!-- Links -->
[lnk-eg-overview]: ../event-grid/overview.md
[lnk-eg-compare]: iot-hub-event-grid-routing-comparison.md