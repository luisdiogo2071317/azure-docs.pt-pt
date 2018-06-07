---
title: Utilize a grelha de eventos do Azure com eventos no esquema CloudEvents
description: Descreve como definir o esquema de CloudEvents para eventos na grelha de eventos do Azure.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: babanisa
ms.openlocfilehash: a2cccbb4feaa7b6f3f51ac7204af4a3e1efc6349
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "34625598"
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Utilizar o esquema de CloudEvents com grelha de eventos

Para além respetivo [esquema de eventos de predefinição](event-schema.md), grelha de eventos do Azure suporta nativamente eventos o [esquema CloudEvents JSON](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](http://cloudevents.io/) é um [abrir especificação padrão](https://github.com/cloudevents/spec/blob/master/spec.md) para descrever os dados de eventos de uma forma comum.

CloudEvents simplifica a interoperabilidade, fornecendo um esquema de eventos comuns para publicar e consumir a nuvem com base em eventos. Este esquema permite uniform ferramentas, formas padrão de encaminhamento & processar eventos e universais formas de anulação da serialização do esquema de eventos externa. Com um esquema comum, pode integrar mais facilmente trabalho entre plataformas.

CloudEvents está a ser compilação por vários [colaboradores](https://github.com/cloudevents/spec/blob/master/community/contributors.md), incluindo Microsoft, através de [nuvem nativo computação Foundation](https://www.cncf.io/). Está atualmente disponível como versão 0.1.

Este artigo descreve como utilizar o esquema de CloudEvents com eventos de grelha.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>Esquema de CloudEvent

Eis um exemplo de um evento de Blob Storage do Azure no formato CloudEvents:

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

CloudEvents v0.1 tem as seguintes propriedades disponíveis:

| CloudEvents        | Tipo     | Valor de JSON de exemplo             | Descrição                                                        | Mapeamento de grelha de eventos
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| eventType          | Cadeia   | "com.example.someevent"          | Tipo de ocorrência que tenham acontecido                                   | eventType
| eventTypeVersion   | Cadeia   | "1.0"                            | A versão do eventType (opcional)                            | dataVersion
| cloudEventsVersion | Cadeia   | "0.1"                            | A versão da especificação de CloudEvents que utiliza o evento        | *passem*
| source             | URI      | "/ mycontext"                     | Descreve o produtor de eventos                                       | tópico #subject
| EventID            | Cadeia   | "1234-1234-1234"                 | ID do evento                                                    | ID
| eventTime          | Carimbo de data/hora| "2018-04-05T17:31:00Z"           | Carimbo de quando evento ter acontecido (opcional)                    | eventTime
| schemaURL          | URI      | "https://myschema.com"           | Uma ligação para o esquema que respeite o atributo de dados (opcional) | *não utilizado*
| contentType        | Cadeia   | "application/json"               | Descrever o formato de codificação de dados (opcional)                       | *não utilizado*
| extensões         | Mapa      | {"extA": "vA", "extB", "vB"}  | Quaisquer metadados adicional (opcional)                                 | *não utilizado*
| dados               | Object   | {"objA": "vA", "objB", "vB"}  | O payload do evento (opcional)                                       | dados

Para obter mais informações, consulte o [CloudEvents spec](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes).

## <a name="configure-event-grid-for-cloudevents"></a>Configurar a grelha de eventos para CloudEvents

Atualmente, a grelha de eventos do Azure tem pré-visualização do suporte para CloudEvents JSON formato entrada e saída no **Central EUA oeste**, **EUA Central**, e **Europa do Norte**.

Pode utilizar a grelha de eventos de entrada e saída de eventos no esquema CloudEvents. Pode utilizar CloudEvents para eventos de sistema, como eventos do Blob Storage e eventos de IoT Hub e eventos personalizados. Também se pode transformar esses eventos na linha anterior e descritos.


| Esquema de entrada       | Esquema de saída
|--------------------|---------------------
| Formato de CloudEvents | Formato de CloudEvents
| Formato de grelha de eventos  | Formato de CloudEvents
| Formato de CloudEvents | Formato de grelha de eventos
| Formato de grelha de eventos  | Formato de grelha de eventos

Para todos os esquemas de evento, eventos grelha necessita de validação ao publicar um tópico da grelha de eventos e ao criar uma subscrição de evento. Para obter mais informações, consulte [grelha de eventos de segurança e autenticação](security-authentication.md).

### <a name="input-schema"></a>Esquema de entrada

Para definir o esquema da entrada num tópico personalizado para CloudEvents, utilize o seguinte parâmetro na CLI do Azure quando criar um tópico `--input-schema cloudeventv01schema`. O tópico personalizado espera agora receber eventos no formato de v0.1 CloudEvents.

Para criar um tópico da grelha de eventos, utilize:

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

A versão atual do CloudEvents não suporta a criação de batches de eventos. Para publicar eventos com um esquema CloudEvent para um tópico, publica individualmente cada evento.

### <a name="output-schema"></a>Esquema de saída

Para definir o esquema de saída de uma subscrição de evento para CloudEvents, utilize o seguinte parâmetro na CLI do Azure ao criar a sua subscrição de evento `--event-delivery-schema cloudeventv01schema`. Eventos para esta subscrição de evento são agora ser entregues CloudEvents v0.1 formato.

Para criar uma subscrição de evento, utilize:

```azurecli
az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

A versão atual do CloudEvents não suporta a criação de batches de eventos. Uma subscrição de evento que está configurada para o esquema de CloudEvent recebe individualmente cada evento. Atualmente, não é possível utilizar um acionador de grelha de eventos para uma aplicação de funções do Azure, quando o evento é entregue no esquema CloudEvents. Tem de utilizar um acionador HTTP.

## <a name="next-steps"></a>Passos Seguintes

* Para obter informações sobre a monitorização de entregas de eventos, consulte [entrega de mensagens de grelha de eventos do Monitor](monitor-event-delivery.md).
* Aconselhamo-lo para testar, comentário e [contribuir](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) para CloudEvents.
* Para obter mais informações sobre como criar uma subscrição de grelha de eventos do Azure, consulte [esquema de subscrição de evento grelha](subscription-creation-schema.md).
