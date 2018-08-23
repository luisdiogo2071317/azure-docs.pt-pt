---
title: Esquema de eventos de registo de contentor de grelha de eventos do Azure
description: Descreve as propriedades que são fornecidas para eventos de Reigstry de contentor com o Azure Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 08/13/2018
ms.author: tomfitz
ms.openlocfilehash: d18a6718e4c29f3d04639644dc752b0733f15ba8
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/14/2018
ms.locfileid: "42061505"
---
# <a name="azure-event-grid-event-schema-for-container-registry"></a>Esquema de eventos do Azure Event Grid para o registo de contentor

Este artigo fornece as propriedades e o esquema para eventos de registo de contentor. Para obter uma introdução aos esquemas de eventos, consulte [esquema de eventos do Azure Event Grid](event-schema.md).

## <a name="available-event-types"></a>Tipos de eventos disponíveis

Armazenamento de BLOBs emite os seguintes tipos de evento:

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.ContainerRegistry.ImagePushed | Gerado quando é enviada por push uma imagem. |
| Microsoft.ContainerRegistry.ImageDeleted | Desencadeado quando uma imagem é eliminada. |

## <a name="example-event"></a>Evento de exemplo

O exemplo seguinte mostra o esquema de uma imagem que enviou o evento: 

```json
[{
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld:v1",
  "eventType": "ImagePushed",
  "eventTime": "2018-04-25T21:39:47.6549614Z",
  "data": {
    "id": "31c51664-e5bd-416a-a5df-e5206bc47ed0",
    "timestamp": "2018-04-25T21:39:47.276585742Z",
    "action": "push",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "size": 3023,
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "length": 3023,
      "repository": "aci-helloworld",
      "tag": "v1"
    },
    "request": {
      "id": "7c66f28b-de19-40a4-821c-6f5f6c0003a4",
      "host": "demo.azurecr.io",
      "method": "PUT",
      "useragent": "docker/18.03.0-ce go/go1.9.4 git-commit/0520e24 os/windows arch/amd64 UpstreamClient(Docker-Client/18.03.0-ce \\\\(windows\\\\))"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

O esquema para um evento de imagem eliminado é semelhante:

```json
[{
  "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
  "topic": "/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.ContainerRegistry/registries/<name>",
  "subject": "aci-helloworld",
  "eventType": "ImageDeleted",
  "eventTime": "2018-04-26T17:56:01.8211268Z",
  "data": {
    "id": "f06e3921-301f-42ec-b368-212f7d5354bd",
    "timestamp": "2018-04-26T17:56:00.996603117Z",
    "action": "delete",
    "target": {
      "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
      "digest": "sha256:213bbc182920ab41e18edc2001e06abcca6735d87782d9cef68abd83941cf0e5",
      "repository": "aci-helloworld"
    },
    "request": {
      "id": "aeda5b99-4197-409f-b8a8-ff539edb7de2",
      "host": "demo.azurecr.io",
      "method": "DELETE",
      "useragent": "python-requests/2.18.4"
    }
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Propriedades do evento

Um evento tem os seguintes dados de nível superior:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| tópico | cadeia | Caminho de recurso completo para a origem do evento. Este campo não é gravável. Event Grid fornece este valor. |
| Assunto | cadeia | Caminho definidos pelo publicador para o assunto de evento. |
| eventType | cadeia | Um dos tipos de eventos registrados para esta origem de evento. |
| eventTime | cadeia | O tempo que o evento é gerado com base no fuso horário UTC do fornecedor. |
| ID | cadeia | Identificador exclusivo para o evento. |
| dados | objeto | Dados de eventos de armazenamento de Blobs. |
| dataVersion | cadeia | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | cadeia | A versão do esquema dos metadados do evento. Grelha de eventos define o esquema das propriedades de nível superior. Event Grid fornece este valor. |

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| ID | cadeia | O ID de evento. |
| carimbo de data/hora | cadeia | A hora em que ocorreu o evento. |
| action | cadeia | A ação que abrange o evento fornecido. |
| destino | objeto | O destino do evento. |
| pedido | objeto | O pedido que gerou o evento. |

O objeto de destino tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| mediaType | cadeia | O tipo MIME do objeto referenciado. |
| tamanho | inteiro | O número de bytes do conteúdo. Mesmo que o campo de comprimento. |
| Resumo | cadeia | O resumo do conteúdo, conforme definido pela especificação de API de HTTP do registo V2. |
| Comprimento | inteiro | O número de bytes do conteúdo. Mesmo que o campo de tamanho. |
| Repositório | cadeia | O nome do repositório. |
| etiqueta | cadeia | O nome da etiqueta. |

O objeto de solicitação tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| ID | cadeia | O ID do pedido que deu início ao evento. |
| Ender | cadeia | O IP ou nome de anfitrião e, possivelmente, porta da ligação de cliente que deu início ao evento. Este valor é o RemoteAddr no pedido de http padrão. |
| anfitrião | cadeia | O nome de anfitrião acessível externamente da instância do Registro, conforme especificado pelo cabeçalho de anfitrião http em solicitações de entrada. |
| método | cadeia | O método de pedido que gerou o evento. |
| USERAGENT | cadeia | O cabeçalho do agente de utilizador do pedido. |

## <a name="next-steps"></a>Passos Seguintes

* Para obter uma introdução ao Azure Event Grid, consulte [o que é o Event Grid?](overview.md)
* Para obter mais informações sobre a criação de uma subscrição do Azure Event Grid, veja [esquema de subscrições do Event Grid](subscription-creation-schema.md).
