---
title: "Esquema de subscrição de evento grelha do Azure"
description: "Descreve as propriedades de subscrição a um evento com grelha de eventos do Azure."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: babanisa
ms.openlocfilehash: a915473c67a7577582837b56d1a9ccec4d21c461
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/01/2018
---
# <a name="event-grid-subscription-schema"></a>Esquema de subscrição de grelha de eventos

Para criar uma subscrição de evento grelha, envie um pedido para a operação de subscrição de evento de criar. Utilize o seguinte formato:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Por exemplo criar uma subscrição de evento para uma conta de armazenamento com o nome `examplestorage` num grupo de recursos denominado `examplegroup`, utilize o seguinte formato:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

O artigo descreve as propriedades e o esquema para o corpo do pedido.
 
## <a name="event-subscription-properties"></a>Propriedades de subscrição de evento

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| Destino | objeto | O objeto que define o ponto final. |
| filtro | objeto | Um campo opcional para filtrar os tipos de eventos. |

### <a name="destination-object"></a>objeto de destino

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| endpointType | cadeia | O tipo de ponto final da subscrição (webhook/HTTP, Hub de eventos ou fila). | 
| endpointUrl | cadeia | O URL de destino para eventos nesta subscrição de evento. | 

### <a name="filter-object"></a>objeto de filtro

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| includedEventTypes | array | Correspondência quando o tipo de evento na mensagem de evento é uma correspondência exata com um destes nomes de tipo de evento. Gera um erro ao nome do evento não corresponder os nomes de tipo de evento registado para a origem do evento. Predefinição corresponde a todos os tipos de eventos. |
| subjectBeginsWith | cadeia | Uma correspondência de prefixo filtrar no campo do requerente de eventos mensagem. A predefinição ou uma cadeia vazia corresponde a todos. | 
| subjectEndsWith | cadeia | Uma correspondência de sufixo filtrar no campo do requerente de eventos mensagem. A predefinição ou uma cadeia vazia corresponde a todos. |
| subjectIsCaseSensitive | cadeia | Controlos de maiúsculas e minúsculas de correspondência de filtros. |


## <a name="example-subscription-schema"></a>Esquema de subscrição de exemplo

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "blobCreated", "blobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos, consulte [Novidades grelha de evento?](overview.md)