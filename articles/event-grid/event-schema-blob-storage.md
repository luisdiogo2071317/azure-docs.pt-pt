---
title: Azure esquema de eventos do armazenamento de BLOBs de grelha de eventos
description: "Descreve as propriedades que são fornecidas para eventos de armazenamento de Blobs com a grelha de eventos do Azure"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: d0a8a3726ac3c33668d8ad91c97c35937c299b46
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Esquema de eventos de grelha de eventos do Azure para o Blob storage

Este artigo fornece as propriedades e o esquema para eventos de armazenamento de Blobs. Para uma introdução para esquemas de eventos, consulte [esquema de eventos de grelha de eventos do Azure](event-schema.md).

## <a name="available-event-types"></a>Tipos de evento disponíveis

Armazenamento de BLOBs emite os seguintes tipos de evento:

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.Storage.BlobCreated | É desencadeado quando é criado um blob. |
| Microsoft.Storage.BlobDeleted | É desencadeado quando um blob é eliminado. |

## <a name="example-event"></a>Eventos de exemplo

O exemplo seguinte mostra o esquema de um blob criado eventos: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

O esquema para um evento de blob eliminado é semelhante: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
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
| dados | objeto | Dados de eventos de armazenamento de Blobs. |
| dataVersion | cadeia | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | cadeia | A versão de esquema dos metadados do evento. Grelha de evento define o esquema das propriedades de nível superior. Grelha de evento fornece este valor. |

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| api | cadeia | A operação que acionou o evento. |
| clientRequestId | cadeia | Um valor opaco gerados pelo cliente, com um limite de caracteres de 1 KB. Quando tiver ativado o registo de análise de armazenamento, é registada nos registos de análise. |
| requestId | cadeia | O identificador exclusivo para o pedido. Utilizá-lo para o pedido de resolução de problemas. |
| eTag | cadeia | O valor que pode utilizar para executar operações condicionalmente. |
| contentType | cadeia | O tipo de conteúdo especificado para o blob. |
| contentLength | inteiro | O tamanho do blob em bytes. |
| blobType | cadeia | O tipo de blob. Os valores válidos são "BlockBlob" ou "PageBlob". |
| url | cadeia | O caminho para o blob. |
| sequencer | cadeia | Um valor controlados pelo utilizador que pode utilizar para monitorizar os pedidos. |
| storageDiagnostics | objeto | Informações sobre o diagnóstico de armazenamento. |
 
## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos do Azure, consulte [Novidades grelha de evento?](overview.md)
* Para obter mais informações sobre como criar uma subscrição de grelha de eventos do Azure, consulte [esquema de subscrição de evento grelha](subscription-creation-schema.md).
* Para uma introdução ao trabalhar com eventos de armazenamento de BLOBs, consulte [eventos de armazenamento de BLOBs de rota - CLI do Azure](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
