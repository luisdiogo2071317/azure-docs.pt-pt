---
title: Utilize o Azure Event Grid com eventos no esquema do CloudEvents
description: Descreve como definir o esquema do CloudEvents para eventos no Azure Event Grid.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: babanisa
ms.openlocfilehash: f3541fb3dbca6243f6c10611aa968603745f97a1
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38314435"
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Utilizar o esquema do CloudEvents com o Event Grid

Além seu [esquema de eventos padrão](event-schema.md), Azure Event Grid os eventos no suporta nativamente o [esquema do CloudEvents JSON](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](http://cloudevents.io/) é um [especificação padrão de abrir](https://github.com/cloudevents/spec/blob/master/spec.md) para descrever os dados de eventos de uma maneira comum.

CloudEvents simplifica a interoperabilidade, fornecendo um esquema de evento comum para publicação e utilizam a nuvem com base em eventos. Esse esquema permite ferramentas uniforme, formas de roteamento e processamento de eventos e universais formas de anular a serialização do esquema de eventos externo. Com um esquema comum, pode integrar mais facilmente o trabalho entre plataformas.

CloudEvents está a ser a compilação por várias [colaboradores](https://github.com/cloudevents/spec/blob/master/community/contributors.md), incluindo Microsoft, através do [Foundation computação nativa da Cloud](https://www.cncf.io/). Está atualmente disponível como versão 0,1.

Este artigo descreve como utilizar o esquema do CloudEvents com o Event Grid.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>Esquema de CloudEvent

Eis um exemplo de um evento de armazenamento de Blobs do Azure no formato do CloudEvents:

``` JSON
{
    "cloudEventsVersion" : "0.1",
    "eventType" : "Microsoft.Storage.BlobCreated",
    "eventTypeVersion" : "",
    "source" : "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}#blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "eventID" : "173d9985-401e-0075-2497-de268c06ff25",
    "eventTime" : "2018-04-28T02:18:47.1281675Z",
    "data" : {
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
    }
}
```

Do CloudEvents v0.1 tem as seguintes propriedades disponíveis:

| CloudEvents        | Tipo     | Valor de JSON de exemplo             | Descrição                                                        | Mapeamento do Event Grid
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| eventType          | Cadeia   | "com.example.someevent"          | Tipo de ocorrência que tenham acontecido                                   | eventType
| eventTypeVersion   | Cadeia   | "1.0"                            | A versão do eventType (opcional)                            | dataVersion
| cloudEventsVersion | Cadeia   | "0.1"                            | A versão da especificação do CloudEvents que utiliza o evento        | *passar por*
| source             | URI      | "/ mycontext"                     | Descreve o produtor de eventos                                       | tópico #subject
| eventID            | Cadeia   | "1234-1234-1234"                 | ID do evento                                                    | ID
| eventTime          | Carimbo de data/hora| "2018-04-05T17:31:00Z"           | Timestamp de quando o evento ocorreu (opcional)                    | eventTime
| schemaURL          | URI      | "https://myschema.com"           | Uma ligação para o esquema que seguem o atributo de dados (opcional) | *não utilizado*
| contentType        | Cadeia   | "application/json"               | Descrever o formato de codificação de dados (opcional)                       | *não utilizado*
| Extensões         | Mapa      | {"extA": "vA", "extB", "vB"}  | Quaisquer metadados adicionais (opcional)                                 | *não utilizado*
| dados               | Object   | {"objA": "vA", "objB", "vB"}  | O payload do evento (opcional)                                       | dados

Para obter mais informações, consulte a [especificação do CloudEvents](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes).

## <a name="configure-event-grid-for-cloudevents"></a>Configurar o Event Grid para do CloudEvents

Atualmente, o Azure Event Grid tem pré-visualização de saída no e suporte para entrada de formato JSON do CloudEvents **e.u.a. Centro-Oeste**, **centro dos E.U.A.**, e **Europa do Norte**.

Pode utilizar o Event Grid para entrada e saída de eventos no esquema do CloudEvents. Pode usar do CloudEvents para eventos de sistema, como eventos de armazenamento de BLOBs e eventos do IoT Hub e eventos personalizados. Ele também pode transformar esses eventos na conexão e volta.


| Esquema de entrada       | Esquema de saída
|--------------------|---------------------
| Formato do CloudEvents | Formato do CloudEvents
| Formato de grade do evento  | Formato do CloudEvents
| Formato do CloudEvents | Formato de grade do evento
| Formato de grade do evento  | Formato de grade do evento

Para todos os esquemas de eventos, o Event Grid exige validação quando publicar um tópico do event grid e ao criar uma subscrição de evento. Para obter mais informações, consulte [Event Grid segurança e autenticação](security-authentication.md).

### <a name="input-schema"></a>Esquema de entrada

Para definir o esquema de entrada num tópico personalizado do CloudEvents, utilize o seguinte parâmetro na CLI do Azure quando criar um tópico `--input-schema cloudeventv01schema`. O tópico personalizado agora espera que eventos de entrada no formato de v0.1 do CloudEvents.

Para criar um tópico do event grid, utilize:

```azurecli
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventv01schema
```

A versão atual do CloudEvents não suporta a criação de batches de eventos. Para publicar eventos com o esquema de CloudEvent para um tópico, publicar individualmente a cada evento.

### <a name="output-schema"></a>Esquema de saída

Para definir o esquema de saída numa subscrição de evento do CloudEvents, utilize o seguinte parâmetro na CLI do Azure quando criar a sua subscrição de evento `--event-delivery-schema cloudeventv01schema`. Eventos para esta subscrição de evento são agora ser entregues no formato de v0.1 do CloudEvents.

Para criar uma subscrição de evento, utilize:

```azurecli
az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

A versão atual do do CloudEvents não suporta a criação de batches de eventos. Uma subscrição de evento que está configurada para o esquema de CloudEvent recebe individualmente a cada evento. Atualmente, não é possível utilizar um acionador do Event Grid para uma aplicação de funções do Azure quando o evento é entregue no esquema do CloudEvents. Tem de utilizar um acionador HTTP. Para obter exemplos de implementação de um acionador HTTP que recebe eventos no esquema do CloudEvents, consulte [utilizar um acionador HTTP como um acionador do Event Grid](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

## <a name="next-steps"></a>Passos Seguintes

* Para obter informações sobre a monitorização de entregas de eventos, consulte [entrega de mensagens do Monitor Event Grid](monitor-event-delivery.md).
* Incentivamos a testar, comentário, e [contribuir](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) do CloudEvents.
* Para obter mais informações sobre a criação de uma subscrição do Azure Event Grid, veja [esquema de subscrições do Event Grid](subscription-creation-schema.md).
