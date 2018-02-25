---
title: Esquema de eventos de evento grelha Service Bus do Azure
description: "Descreve as propriedades que são fornecidas para eventos de Service Bus com grelha de eventos do Azure"
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: article
ms.date: 02/21/2018
ms.author: babanisa
ms.openlocfilehash: 72780bff3807534efb456a9a7998f7d4de3c6f12
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/24/2018
---
# <a name="azure-event-grid-event-schema-for-service-bus"></a>Esquema de eventos de grelha de eventos do Azure para o Service Bus

Este artigo fornece as propriedades e esquema de eventos do Service Bus. Para uma introdução para esquemas de eventos, consulte [esquema de eventos de grelha de eventos do Azure](event-schema.md).

## <a name="available-event-types"></a>Tipos de evento disponíveis

Barramento de serviço emite os seguintes tipos de evento:

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners | É desencadeado quando existem mensagens Active Directory de uma fila ou a subscrição e não recetores à escuta. |
| Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener | É desencadeado quando existem Active Directory mensagens de uma fila entregues e não os serviços de escuta de Active Directory. |

## <a name="example-event"></a>Eventos de exemplo

O exemplo seguinte mostra o esquema de uma mensagens Active Directory com nenhum evento de serviços de escuta:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

O esquema para um evento de fila entregues é semelhante:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Propriedades do evento

Um evento tem os seguintes dados de nível superior:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| Tópico | string | Caminho de recurso completo para a origem do evento. Este campo não é passível de escrita. Grelha de evento fornece este valor. |
| Requerente | string | Caminho definida pelo fabricante para o assunto do evento. |
| eventType | string | Um dos tipos de eventos registados para esta origem de evento. |
| eventTime | string | A hora que do evento é gerado com base na hora UTC do fornecedor. |
| ID | string | Identificador exclusivo para o evento. |
| dados | objeto | Dados de eventos de armazenamento de Blobs. |
| dataVersion | string | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | string | A versão de esquema dos metadados do evento. Grelha de evento define o esquema das propriedades de nível superior. Grelha de evento fornece este valor. |

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| nameSpaceName | string | O espaço de nomes do Service Bus o recurso existe. |
| requestUri | string | O URI para a fila específica ou a emitir o evento de subscrição. |
| entityType | string | O tipo de entidade de barramento de serviço emitir eventos (fila ou subscrição). |
| queueName | string | A fila com o Active Directory mensagens se subscrever uma fila. Valor nulo se utilizar tópicos / subscrições. |
| topicName | string | O tópico a subscrição do Service Bus com mensagens de Active Directory pertence. Valor nulo se utilizar uma fila. |
| subscriptionName | string | A subscrição do Service Bus com mensagens de Active Directory. Valor nulo se utilizar uma fila. |

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos do Azure, consulte [Novidades grelha de evento?](overview.md)
* Para obter mais informações sobre como criar uma subscrição de grelha de eventos do Azure, consulte [esquema de subscrição de evento grelha](subscription-creation-schema.md).
* Para obter mais informações sobre como utilizar a grelha de eventos do Azure com o Service Bus, consulte o [Service Bus para descrição geral da integração de grelha de evento](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md).
* Tente [receber eventos de Service Bus com funções ou Logic Apps](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json).
