---
title: Saída e os pontos finais no duplos Digital do Azure | Documentos da Microsoft
description: Orientação sobre como criar pontos finais duplos Digital do Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: c09ee84cda5f0a9747d3ee1f8f1b37d1323f2cc2
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212255"
---
# <a name="egress-and-endpoints"></a>Saída e os pontos finais

Os gémeos Digital do Azure suporta o conceito de _pontos de extremidade_ em que cada ponto de extremidade representa um mediador de mensagens/eventos na subscrição do Azure do utilizador. Eventos e as mensagens podem ser enviadas para **Hub de eventos**, **Event Grid**, e **tópicos do Service Bus**.

Eventos serão enviados para os pontos finais de acordo com as preferências de encaminhamento predefinidos: o utilizador pode especificar o ponto final deve receber qualquer um dos seguintes eventos: **TopologyOperation**, **UdfCustom**, **SensorChange**, **SpaceChange**, ou **DeviceMessage**.

Para obter uma compreensão básica de encaminhamento de eventos e tipos de eventos, consulte [mensagens de eventos de roteamento e](concepts-events-routing.md).

## <a name="event-types-description"></a>Descrição de tipos de eventos

Aqui estão os formatos de evento para cada um dos tipos de evento:

- **TopologyOperation**

  Aplica-se às alterações de gráfico. O *assunto* propriedade especifica o tipo de objeto afetado. Tipos de objetos que podem acionar este evento são: **dispositivo**, **DeviceBlobMetadata**, **DeviceExtendedProperty**, **ExtendedPropertyKey**, **ExtendedType**, **KeyStore**, **relatório**, **RoleDefinition**, **Sensor**, **SensorBlobMetadata**, **SensorExtendedProperty**, **espaço**, **SpaceBlobMetadata**,  **SpaceExtendedProperty**, **SpaceResource**, **SpaceRoleAssignment**, **sistema**, **utilizador**, **UserBlobMetadata**, **UserExtendedProperty**.

  Exemplo:

  ```JSON
  {
    "id": "00000000-0000-0000-0000-000000000000",
    "subject": "ExtendedPropertyKey",
    "data": {
      "SpacesToNotify": [
        "3a16d146-ca39-49ee-b803-17a18a12ba36"
      ],
      "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
      "AccessType": "Create"
    },
    "eventType": "TopologyOperation",
    "eventTime": "2018-04-17T17:41:54.9400177Z",
    "dataVersion": "1",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Nome do atributo personalizado | Substituir por |
    | --- | --- |
    | *yourTopicName* | O nome do seu tópico personalizado |

- **UdfCustom**

  Um evento enviado por uma função definida pelo utilizador (UDF). 
  
  > [!IMPORTANT]
  > Neste evento tem de ser explicitamente enviados a partir do UDF em si.

  Exemplo:

  ```JSON
  {
    "id": "568fd394-380b-46fa-925a-ebb96f658cce",
    "subject": "UdfCustom",
    "data": {
      "TopologyObjectId": "7c799bfc-1bff-4b9e-b15a-669933969d20",
      "ResourceType": "Space",
      "Payload": "\"Room is not available or air quality is poor\"",
      "CorrelationId": "568fd394-380b-46fa-925a-ebb96f658cce"
    },
    "eventType": "UdfCustom",
    "eventTime": "2018-10-02T06:50:15.198Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Nome do atributo personalizado | Substituir por |
    | --- | --- |
    | *yourTopicName* | O nome do seu tópico personalizado |

- **SensorChange**

  Uma atualização para o estado de um sensor com base nas alterações de telemetria.

  Exemplo:

  ```JSON
  {
    "id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
    "subject": "SensorChange",
    "data": {
      "Type": "Classic",
      "DataType": "Motion",
      "Id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
      "Value": "False",
      "PreviousValue": "True",
      "EventTimestamp": "2018-04-17T17:46:15.4964262Z",
      "MessageType": "sensor",
      "Properties": {
        "ms-client-request-id": "c9e576b7-5eea-4f61-8617-92a57add5179",
        "ms-activity-id": "ct22YwXEGJ5u.605.0"
      }
    },
    "eventType": "SensorChange",
    "eventTime": "2018-04-17T17:46:18.5452993Z",
    "dataVersion": "1",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Nome do atributo personalizado | Substituir por |
    | --- | --- |
    | *yourTopicName* | O nome do seu tópico personalizado |

- **SpaceChange**

  Uma atualização para o estado de um espaço com base nas alterações de telemetria.

  Exemplo:

  ```JSON
  {
    "id": "42522e10-b1aa-42ff-a5e7-7181788ffc4b",
    "subject": "SpaceChange",
    "data": {
      "Type": null,
      "DataType": "AvailableAndFresh",
      "Id": "7c799bfc-1bff-4b9e-b15a-669933969d20",
      "Value": "Room is not available or air quality is poor",
      "PreviousValue": null,
      "RawData": null,
      "transactionId": null,
      "EventTimestamp": null,
      "MessageType": null,
      "Properties": null,
      "CorrelationId": "42522e10-b1aa-42ff-a5e7-7181788ffc4b"
    },
    "eventType": "SpaceChange",
    "eventTime": "2018-10-02T06:50:20.128Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/yourTopicName"
  }
  ```

    | Nome do atributo personalizado | Substituir por |
    | --- | --- |
    | *yourTopicName* | O nome do seu tópico personalizado |

- **DeviceMessage**

  Permite-lhe especificar uma **EventHub** ligação para o qual os eventos de telemetria não processados podem ser roteados também de duplos Digital do Azure.

> [!NOTE]
> - **DeviceMessage** é combináveis apenas com **EventHub**; não poderá combinar **DeviceMessage** com qualquer um dos outros tipos de evento.
> - Pode especificar apenas um ponto final da combinação do tipo **EventHub** ou **DeviceMessage**.

## <a name="configuring-endpoints"></a>Configurando pontos de extremidade

Gestão de ponto final é exercido por meio da API de pontos de extremidade. Aqui estão alguns exemplos sobre como configurar os pontos finais suportados diferentes. Preste atenção especial à matriz de tipos de eventos, porque definem o encaminhamento para o ponto final:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
```

- Encaminhar para **do Service Bus** tipos de evento: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourTopicName"
  }
  ```

    | Nome do atributo personalizado | Substituir por |
    | --- | --- |
    | *yourNamespace* | O espaço de nomes do seu ponto final |
    | *yourPrimaryKey* | A cadeia de ligação primária utilizada para autenticar |
    | *yourSecondaryKey* | A cadeia de ligação secundária utilizada para autenticar |
    | *yourTopicName* | O nome do seu tópico personalizado |

- Encaminhar para **Event Grid** tipos de evento: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "yourPrimaryKey",
    "secondaryConnectionString": "yourSecondaryKey",
    "path": "yourTopicName.westus-1.eventgrid.azure.net"
  }
  ```

    | Nome do atributo personalizado | Substituir por |
    | --- | --- |
    | *yourPrimaryKey* | A cadeia de ligação primária utilizada para autenticar|
    | *yourSecondaryKey* | A cadeia de ligação secundária utilizada para autenticar |
    | *yourTopicName* | O nome do seu tópico personalizado |

- Encaminhar para **Hub de eventos** tipos de evento: **SensorChange**, **SpaceChange**, **TopologyOperation**

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourEventHubName"
  }
  ```

    | Nome do atributo personalizado | Substituir por |
    | --- | --- |
    | *yourNamespace* | O espaço de nomes do seu ponto final |
    | *yourPrimaryKey* | A cadeia de ligação primária utilizada para autenticar |
    | *yourSecondaryKey* | A cadeia de ligação secundária utilizada para autenticar |
    | *yourEventHubName* | O nome do seu **Hub de eventos** |

- Encaminhar para **Hub de eventos** tipo de evento: **DeviceMessage**. A inclusão de `EntityPath` no **connectionString** é obrigatório.

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey;EntityPath=yourEventHubName",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey;EntityPath=yourEventHubName",
    "path": "yourEventHubName"
  }
  ```

    | Nome do atributo personalizado | Substituir por |
    | --- | --- |
    | *yourNamespace* | O espaço de nomes do seu ponto final |
    | *yourPrimaryKey* | A cadeia de ligação primária utilizada para autenticar |
    | *yourSecondaryKey* | A cadeia de ligação secundária utilizada para autenticar |
    | *yourEventHubName* | O nome do seu **Hub de eventos** |

> [!NOTE]
> Após a criação de um novo ponto final, poderá demorar até 5 a 10 minutos para começar a receber eventos no ponto final.

## <a name="primary-and-secondary-connection-keys"></a>Chaves de ligação primária e secundária

Quando uma chave de ligação primária fica não autorizada, o sistema tenta automaticamente a chave de ligação secundária. Isso fornece uma cópia de segurança e permite que a possibilidade de maneira normal autenticar e atualizar a chave primária por meio da API de pontos de extremidade.

Se ambas as chaves de ligação primária e secundária não estão autorizadas, o sistema irá introduzir um tempo de espera de término exponencial de até 30 minutos. Eventos serão largados em cada tempo de espera de início de término acionada.

Sempre que o sistema está num término de estado de espera, atualização de ligações de chaves, através da API de pontos de extremidade podem demorar até 30 minutos para entrar em vigor.

## <a name="unreachable-endpoints"></a>Pontos de extremidade inacessíveis

Quando um ponto de extremidade fique inacessível, o sistema irá introduzir o um tempo de espera de término exponencial de até 30 minutos. Eventos serão largados em cada tempo de espera de início de término acionada.

## <a name="next-steps"></a>Passos Seguintes

Para saber como utilizar o Swagger do Azure Digital duplos, leia [Swagger do Azure Digital duplos](how-to-use-swagger.md).

Para saber mais sobre o encaminhamento de eventos e mensagens na duplos Digital do Azure, leia [mensagens de eventos de roteamento e](concepts-events-routing.md).
