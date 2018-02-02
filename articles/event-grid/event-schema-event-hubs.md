---
title: Azure esquema de eventos do hubs de eventos de grelha de eventos
description: "Descreve as propriedades que são fornecidas para eventos de hubs de eventos com grelha de eventos do Azure"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 9fdc8816d8db88d4f1fd7b6ce722b7d2763eeaeb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Esquema de eventos de grelha de eventos do Azure para os event hubs

Este artigo fornece as propriedades e o esquema para eventos de hubs de eventos. Para uma introdução para esquemas de eventos, consulte [esquema de eventos de grelha de eventos do Azure](event-schema.md).

### <a name="available-event-types"></a>Tipos de evento disponíveis

Os Event Hubs emite o **Microsoft.EventHub.CaptureFileCreated** tipo de evento quando é criado um ficheiro de captura.

## <a name="example-event"></a>Eventos de exemplo

Este evento de exemplo mostra o esquema de um evento de hubs de eventos é desencadeado quando a funcionalidade de captura armazena um ficheiro: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        },
        "dataVersion": "",
        "metadataVersion": "1"
    }
]
```

## <a name="event-properties"></a>Propriedades do evento

Um evento tem os seguintes dados de nível superior:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| Tópico | cadeia | Caminho de recurso completo para a origem do evento. Este campo não é passível de escrita. Grelha de evento fornece este valor. |
| Requerente | cadeia | Caminho definida pelo fabricante para o assunto do evento. |
| eventType | cadeia | Um dos tipos de eventos registados para esta origem de evento. |
| eventTime | cadeia | A hora que do evento é gerado com base na hora UTC do fornecedor. |
| ID | cadeia | Identificador exclusivo para o evento. |
| dados | objeto | Dados de eventos de hub de eventos. |
| dataVersion | cadeia | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | cadeia | A versão de esquema dos metadados do evento. Grelha de evento define o esquema das propriedades de nível superior. Grelha de evento fornece este valor. |

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| fileUrl | cadeia | O caminho para o ficheiro de captura. |
| fileType | cadeia | O tipo de ficheiro do ficheiro de captura. |
| partitionId | cadeia | O ID de partição horizontal. |
| sizeInBytes | inteiro | O tamanho do ficheiro. |
| eventCount | inteiro | O número de eventos no ficheiro. |
| firstSequenceNumber | inteiro | O menor número de sequência da fila. |
| lastSequenceNumber | inteiro | O último número de sequência da fila. |
| firstEnqueueTime | cadeia | A primeira da fila. |
| lastEnqueueTime | cadeia | A hora da última da fila. |

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos do Azure, consulte [Novidades grelha de evento?](overview.md)
* Para obter mais informações sobre como criar uma subscrição de grelha de eventos do Azure, consulte [esquema de subscrição de evento grelha](subscription-creation-schema.md).
* Para obter informações sobre como lidar com eventos de hubs de eventos, consulte [transmitir macrodados para um armazém de dados](event-grid-event-hubs-integration.md).