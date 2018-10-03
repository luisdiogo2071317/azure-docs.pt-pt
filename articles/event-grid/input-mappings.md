---
title: Campo personalizado do mapa ao esquema do Azure Event Grid
description: Descreve como converter seu esquema personalizado para o esquema do Azure Event Grid.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: tomfitz
ms.openlocfilehash: f79fa096484edc34294ea0a69584e12788dba647
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043402"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>Mapear campos personalizados ao esquema do Event Grid

Se os dados de evento não corresponderem a esperada [esquema de grelha de eventos](event-schema.md), ainda pode utilizar o Event Grid para eventos de rota para assinantes. Este artigo descreve como mapear o seu esquema para o esquema do Event Grid.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Esquema de eventos do original

Vamos supor que tenha um aplicativo para enviar eventos no seguinte formato:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Embora esse formato não corresponde ao esquema necessário, o Event Grid permite-lhe mapear seus campos para o esquema. Em alternativa, pode receber os valores no esquema original.

## <a name="create-custom-topic-with-mapped-fields"></a>Criar um tópico personalizado com campos mapeados

Ao criar um tópico personalizado, especifique como mapear campos do seu evento original para o esquema de grelha de eventos. Existem três propriedades que usar para personalizar o mapeamento:

* O `--input-schema` parâmetro especifica o tipo de esquema. As opções disponíveis são *cloudeventv01schema*, *customeventschema*, e *eventgridschema*. O valor predefinido é eventgridschema. Quando criar personalizado mapeamento entre o esquema e o esquema de grelha de eventos, utilize customeventschema. Quando eventos forem no esquema do CloudEvents, use cloudeventv01schema.

* O `--input-mapping-default-values` parâmetro especifica os valores predefinidos para a campos no esquema do Event Grid. Pode definir os valores predefinidos para `subject`, `eventtype`, e `dataversion`. Normalmente, utilize este parâmetro quando o seu esquema personalizado não inclui um campo que corresponde a um desses três campos. Por exemplo, pode especificar essa versão de dados está sempre definido como **1.0**.

* O `--input-mapping-fields` parâmetro mapeia os campos do seu esquema para o esquema de grelha de eventos. Especifique os valores em pares chave/valor, separadas por espaços. O nome da chave, utilize o nome do campo de grelha de eventos. O valor, utilize o nome do campo. Pode utilizar nomes de chaves para `id`, `topic`, `eventtime`, `subject`, `eventtype`, e `dataversion`.

O exemplo seguinte cria um tópico personalizado com algumas mapeado e campos predefinidos:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema \
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

## <a name="subscribe-to-event-grid-topic"></a>Subscrever o tópico do event grid

Ao subscrever o tópico personalizado, especifique o esquema que pretende utilizar para receber os eventos. Utilizar o `--event-delivery-schema` parâmetro e defina-o como *cloudeventv01schema*, *eventgridschema*, ou *inputeventschema*. O valor predefinido é eventgridschema.

Os exemplos nesta secção utilizam um armazenamento de filas para o manipulador de eventos. Para obter mais informações, consulte [encaminhar eventos personalizados para o armazenamento de filas do Azure](custom-event-to-queue-storage.md).

O exemplo seguinte subscreve um tópico do event grid e usa o esquema de grelha do evento:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub1 \
  --event-delivery-schema eventgridschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

O exemplo seguinte utiliza o esquema de entrada do evento:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub2 \
  --event-delivery-schema inputeventschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

## <a name="publish-event-to-topic"></a>Publicar o evento para o tópico

Agora, está pronto para enviar um evento para o tópico personalizado e ver o resultado do mapeamento. O seguinte script para publicar um evento no [esquema de exemplo](#original-event-schema):

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

event='[ { "myEventTypeField":"Created", "resource":"Users/example/Messages/1000", "resourceData":{"someDataField1":"SomeDataFieldValue"} } ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
```

Agora, examine seu armazenamento de filas. As duas subscrições entregar eventos em esquemas diferentes.

A primeira assinatura utilizado o esquema de grelha de eventos. O formato do evento entregue é:

```json
{
  "Id": "016b3d68-881f-4ea3-8a9c-ed9246582abe",
  "EventTime": "2018-05-01T20:00:25.2606434Z",
  "EventType": "Created",
  "DataVersion": "1.0",
  "MetadataVersion": "1",
  "Topic": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.EventGrid/topics/demotopic",
  "Subject": "DefaultSubject",
  "Data": {
    "myEventTypeField": "Created",
    "resource": "Users/example/Messages/1000",
    "resourceData": { "someDataField1": "SomeDataFieldValue" } 
  }
}
```

Estes campos contêm os mapeamentos do tópico personalizado. **myEventTypeField** é mapeado para **EventType**. Os valores predefinidos para **DataVersion** e **assunto** são utilizados. O **dados** objeto contém os campos de esquema original do evento.

A segunda assinatura utilizado o esquema de eventos de entrada. O formato do evento entregue é:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": { "someDataField1": "SomeDataFieldValue" }
}
```

Tenha em atenção que os campos originais foram entregues.

## <a name="next-steps"></a>Passos Seguintes

* Para obter informações sobre a entrega de eventos e repetições, [entrega de mensagens do Event Grid e volte a tentar](delivery-and-retry.md).
* Para obter uma introdução ao Event Grid, veja [Sobre o Azure Event Grid](overview.md).
* Para começar rapidamente com o Event Grid, veja [criar e encaminhar eventos personalizados com o Azure Event Grid](custom-event-quickstart.md).
