---
title: O mapeamento de campos personalizados para o esquema de grelha de eventos do Azure
description: Descreve como converter o esquema personalizado para o esquema de grelha de eventos do Azure.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: tomfitz
ms.openlocfilehash: 32f93f383ec4044afb0696fcef1705c9ed65d673
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/17/2018
---
# <a name="map-custom-fields-to-event-grid-schema"></a>O mapeamento de campos personalizados para o esquema de grelha de eventos

Se os dados de eventos não corresponde a esperada [esquema de grelha de evento](event-schema.md), pode continuar a utilizar a grelha de eventos para o evento de rota para subscritores. Este artigo descreve como mapear o esquema para o esquema de grelha de eventos.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>Esquema de evento original

Vamos imaginar que tem uma aplicação que envia eventos no seguinte formato:

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

Embora esse formato não corresponde ao esquema necessário, o evento grelha permite-lhe mapear os campos para o esquema. Em alternativa, pode receber os valores no esquema original.

## <a name="create-custom-topic-with-mapped-fields"></a>Criar tópico personalizado com campos mapeados

Ao criar um tópico personalizado, especifique como mapear os campos do seu evento original para o esquema de grelha de eventos. Existem três propriedades a que utilizar para personalizar o mapeamento de:

* O `--input-schema` parâmetro especifica o tipo de esquema. As opções disponíveis são *cloudeventv01schema*, *customeventschema*, e *eventgridschema*. O valor predefinido é eventgridschema. Ao criar personalizado mapeamento entre o esquema e o esquema de grelha de eventos, utilize customeventschema. Eventos no esquema CloudEvents, utilize cloudeventv01schema.

* O `--input-mapping-default-values` parâmetro especifica os valores predefinidos para os campos no esquema grelha de eventos. Pode definir os valores predefinidos para *requerente*, *eventtype*, e *dataversion*. Normalmente, utilize este parâmetro quando o esquema personalizado não inclui um campo que corresponde a uma desses três campos. Por exemplo, pode especificar que dataversion está sempre definido como **1.0**.

* O `--input-mapping-fields` parâmetro mapeia os campos do seu esquema para o esquema de grelha de eventos. Especifique os valores no espaço de valores separados por pares chave-valor. Para o nome da chave, utilize o nome do campo da grelha de eventos. Para o valor, utilize o nome do seu campo. Pode utilizar os nomes de chaves para *id*, *tópico*, *eventtime*, *requerente*, *eventtype*e *dataversion*.

O exemplo seguinte cria um tópico personalizado com algumas mapeado e predefinido campos:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

## <a name="subscribe-to-event-grid-topic"></a>Subscrever o tópico de grelha de eventos

Quando subscrever o tópico personalizado, especifique o esquema que pretende utilizar para receber os eventos. Utilizar o `--event-delivery-schema` parâmetro e defina-o como *cloudeventv01schema*, *eventgridschema*, ou *inputeventschema*. O valor predefinido é eventgridschema.

Os exemplos nesta secção utilizam o armazenamento de filas para o processador de eventos. Para obter mais informações, consulte [encaminhar eventos personalizados para o armazenamento de filas do Azure](custom-event-to-queue-storage.md).

O exemplo seguinte subscreve um tópico da grelha de eventos e utiliza o esquema de grelha do evento predefinido:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub1 \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

O exemplo seguinte utiliza o esquema da entrada do evento:

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub2 \
  --event-delivery-schema inputeventschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

## <a name="publish-event-to-topic"></a>Publicar o evento de tópico

Agora está pronto para enviar um evento para o tópico personalizado e ver o resultado do mapeamento. O script seguinte para publicar um evento no [esquema de exemplo](#original-event-schema):

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

body=$(eval echo "'$(curl https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/event-grid/mapeventfields.json)'")

curl -X POST -H "aeg-sas-key: $key" -d "$body" $endpoint
```

Agora, observe o armazenamento de filas. As duas subscrições entregar eventos em esquemas diferentes.

A subscrição primeiro utilizado o esquema de grelha de eventos. O formato do evento fornecido é:

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

Estes campos contêm os mapeamentos do tópico personalizado. **myEventTypeField** está mapeado para **EventType**. Os valores predefinidos para **DataVersion** e **requerente** são utilizados. O **dados** objeto contém os campos de esquema de evento original.

A segunda subscrição utilizado o esquema de eventos de entrada. O formato do evento fornecido é:

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": { "someDataField1": "SomeDataFieldValue" }
}
```

Tenha em atenção que os campos originais foram fornecidos.

## <a name="next-steps"></a>Passos Seguintes

* Para obter informações sobre a entrega de eventos e de tentativas, [entrega de mensagens de evento grelha e volte a tentar](delivery-and-retry.md).
* Para uma introdução à grelha de eventos, consulte [sobre eventos grelha](overview.md).
* Para rapidamente começar a utilizar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).
