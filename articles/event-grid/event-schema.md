---
title: Esquema de eventos de grelha de eventos do Azure
description: Descreve as propriedades que são fornecidas para eventos com grelha de eventos do Azure
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 04/17/2018
ms.author: babanisa
ms.openlocfilehash: 3e0b7fd825b8e985cea2c32301986b3a7f8bb619
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="azure-event-grid-event-schema"></a>Esquema de eventos de grelha de eventos do Azure

Este artigo descreve as propriedades e o esquema que estão presentes todos os eventos. Eventos são compostos por um conjunto de propriedades de cadeia necessário cinco e um objeto de dados necessário. As propriedades são comuns a todos os eventos a partir de qualquer fabricante. O objeto de dados contém as propriedades que são específicas para cada publicador. Tópicos de sistema, estas propriedades são específicas ao fornecedor de recursos, tais como o Storage do Azure ou Event Hubs do Azure.

Origens de eventos enviam eventos à grelha de eventos do Azure numa matriz, que pode conter muitos objetos de eventos. Quando publicar eventos para um tópico da grelha de eventos, a matriz pode ter um tamanho total de 1 MB. Cada evento na matriz está limitado a 64 KB. Se um evento ou a matriz excede os limites de tamanho, receber a resposta **413 Payload demasiado grande**.

Grelha de eventos envia os eventos para os subscritores numa matriz que contenha um único evento. Este comportamento podem ser alterados no futuro.

Pode encontrar o esquema JSON para o evento de grelha de evento e o payload de dados de cada fabricante do Azure no [arquivo de esquema de eventos](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane).

## <a name="event-schema"></a>Esquema de eventos

O exemplo seguinte mostra as propriedades que são utilizadas por todos os publicadores de eventos:

```json
[
  {
    "topic": string,
    "subject": string,
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

Por exemplo, se o esquema publicado um evento de armazenamento de Blobs do Azure:

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```

## <a name="event-properties"></a>Propriedades do evento

Todos os eventos contenham os seguintes dados de nível superior mesmos:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| Tópico | cadeia | Caminho de recurso completo para a origem do evento. Este campo não é passível de escrita. Grelha de evento fornece este valor. |
| Requerente | cadeia | Caminho definida pelo fabricante para o assunto do evento. |
| eventType | cadeia | Um dos tipos de eventos registados para esta origem de evento. |
| eventTime | cadeia | A hora que do evento é gerado com base na hora UTC do fornecedor. |
| ID | cadeia | Identificador exclusivo para o evento. |
| dados | objeto | Dados de eventos específicos do fornecedor de recursos. |
| dataVersion | cadeia | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | cadeia | A versão do esquema dos metadados do evento. Grelha de evento define o esquema das propriedades de nível superior. Grelha de evento fornece este valor. |

Para saber mais sobre as propriedades no objecto de dados, consulte a origem do evento:

* [Subscrições do Azure (operações de gestão)](event-schema-subscriptions.md)
* [Armazenamento de blobs](event-schema-blob-storage.md)
* [Hubs de Eventos](event-schema-event-hubs.md)
* [Service Bus](event-schema-service-bus.md)
* [Hub IoT](event-schema-iot-hub.md)
* [Grupos de recursos (operações de gestão)](event-schema-resource-groups.md)

Para tópicos personalizados, o publicador de eventos determina o objeto de dados. Os dados de nível superior devem conter os campos mesmos como eventos definidos pelo recurso padrão.

Quando publica eventos personalizados tópicos, crie assuntos para os eventos que tornam mais fácil para os subscritores de saber se estão interessados no evento. Os subscritores utilizam o assunto e rotas de filtro de eventos. Considere fornecer o caminho para onde o evento foi efetuada para que subscritores podem filtrar por segmentos de caminho. O caminho permite aos subscritores narrowly ou amplamente filtrar eventos. Por exemplo, se fornecer um caminho de três segmento como `/A/B/C` no assunto, os subscritores podem filtrar pelo primeiro segmento `/A` para obter um conjunto amplo de eventos. Os subscritores obter eventos com assuntos como `/A/B/C` ou `/A/D/E`. Outros subscritores podem filtrar por `/A/B` para obter um conjunto de eventos quanto mais estreitos forem.

Por vezes, o requerente tem de obter mais detalhes sobre o que aconteceu. Por exemplo, o **contas do Storage** publicador fornece o assunto `/blobServices/default/containers/<container-name>/blobs/<file>` quando um ficheiro seja adicionado a um contentor. Um subscritor pode filtrar pelo caminho `/blobServices/default/containers/testcontainer` para obter todos os eventos para esse contentor, mas não outros contentores na conta de armazenamento. Um subscritor pode também filtrar ou rota pelo sufixo `.txt` apenas trabalhar com ficheiros de texto.

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos do Azure, consulte [Novidades grelha de evento?](overview.md)
* Para obter mais informações sobre como criar uma subscrição de grelha de eventos do Azure, consulte [esquema de subscrição de evento grelha](subscription-creation-schema.md).
