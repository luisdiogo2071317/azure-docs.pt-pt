---
title: Saída e os pontos finais no duplos Digital do Azure | Documentos da Microsoft
description: Diretrizes sobre como criar pontos finais duplos Digital do Azure
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: e140ca46a18fcab2194adb213d723ab67d40b0a8
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615165"
---
# <a name="egress-and-endpoints"></a>Saída e os pontos finais

Os gémeos Digital do Azure suporta o conceito de **pontos de extremidade**. Cada ponto de extremidade representa um mediador de mensagem ou eventos na subscrição do Azure do utilizador. Eventos e as mensagens podem ser enviadas para os tópicos do Event Hubs do Azure, o Azure Event Grid e o Azure Service Bus.

Eventos são enviados para pontos finais de acordo com as preferências de encaminhamento predefinidos. O utilizador pode especificar o ponto final deve receber qualquer um dos seguintes eventos: 

- TopologyOperation
- UdfCustom
- SensorChange
- SpaceChange
- DeviceMessage

Para obter uma compreensão básica de encaminhamento de eventos e tipos de eventos, consulte [mensagens de eventos de roteamento e](concepts-events-routing.md).

## <a name="event-types-description"></a>Descrição de tipos de eventos

Os formatos de evento para cada um dos tipos de eventos são descritos nas seções a seguir.

### <a name="topologyoperation"></a>TopologyOperation

**TopologyOperation** aplica-se às alterações de gráfico. O **assunto** propriedade especifica o tipo de objeto afetado. Os seguintes tipos de objetos podem disparar esse evento: 

- Dispositivo
- DeviceBlobMetadata
- DeviceExtendedProperty
- ExtendedPropertyKey
- ExtendedType
- KeyStore
- Relatório
- RoleDefinition
- Sensor
- SensorBlobMetadata
- SensorExtendedProperty
- Espaço
- SpaceBlobMetadata
- SpaceExtendedProperty
- SpaceResource
- SpaceRoleAssignment
- Sistema
- Utilizador
- UserBlobMetadata
- UserExtendedProperty

#### <a name="example"></a>Exemplo

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

| Nome do atributo personalizado | Substituir |
| --- | --- |
| yourTopicName | O nome do seu tópico personalizado |

### <a name="udfcustom"></a>UdfCustom

**UdfCustom** é um evento enviado por uma função definida pelo utilizador (UDF). 
  
> [!IMPORTANT]  
> Este evento deve ser explicitamente enviado a partir do UDF em si.

#### <a name="example"></a>Exemplo

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

| Nome do atributo personalizado | Substituir |
| --- | --- |
| yourTopicName | O nome do seu tópico personalizado |

### <a name="sensorchange"></a>SensorChange

**SensorChange** é uma atualização para o estado de um sensor com base nas alterações de telemetria.

#### <a name="example"></a>Exemplo

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

| Nome do atributo personalizado | Substituir |
| --- | --- |
| yourTopicName | O nome do seu tópico personalizado |

### <a name="spacechange"></a>SpaceChange

**SpaceChange** é uma atualização para o estado de um espaço com base nas alterações de telemetria.

#### <a name="example"></a>Exemplo

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

| Nome do atributo personalizado | Substituir |
| --- | --- |
| yourTopicName | O nome do seu tópico personalizado |

### <a name="devicemessage"></a>DeviceMessage

Usando **DeviceMessage**, pode especificar um **EventHub** ligação para o qual os eventos de telemetria não processados podem ser roteados também de duplos Digital do Azure.

> [!NOTE]
> - **DeviceMessage** é combináveis apenas com **EventHub**. Não é possível combinar **DeviceMessage** com qualquer um dos outros tipos de evento.
> - Pode especificar apenas um ponto final da combinação do tipo **EventHub** ou **DeviceMessage**.

## <a name="configure-endpoints"></a>Configurar pontos finais

Gestão de ponto final é exercido por meio da API de pontos de extremidade. Os exemplos seguintes demonstram como configurar os pontos finais suportados diferentes. Preste atenção especial à matriz de tipos de eventos, porque definem o encaminhamento para o ponto final:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
```

- Rota para tipos de eventos do Service Bus **SensorChange**, **SpaceChange**, e **TopologyOperation**:

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

    | Nome do atributo personalizado | Substituir |
    | --- | --- |
    | yourNamespace | O espaço de nomes do seu ponto final |
    | yourPrimaryKey | A cadeia de ligação primária utilizada para autenticar |
    | yourSecondaryKey | A cadeia de ligação secundária utilizada para autenticar |
    | yourTopicName | O nome do seu tópico personalizado |

- Rota para tipos de eventos do Event Grid **SensorChange**, **SpaceChange**, e **TopologyOperation**:

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

    | Nome do atributo personalizado | Substituir |
    | --- | --- |
    | yourPrimaryKey | A cadeia de ligação primária utilizada para autenticar|
    | yourSecondaryKey | A cadeia de ligação secundária utilizada para autenticar |
    | yourTopicName | O nome do seu tópico personalizado |

- Rota para tipos de eventos de Hubs de eventos **SensorChange**, **SpaceChange**, e **TopologyOperation**:

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

    | Nome do atributo personalizado | Substituir |
    | --- | --- |
    | yourNamespace | O espaço de nomes do seu ponto final |
    | yourPrimaryKey | A cadeia de ligação primária utilizada para autenticar |
    | yourSecondaryKey | A cadeia de ligação secundária utilizada para autenticar |
    | yourEventHubName | O nome do seu hub de eventos |

- Rota para o tipo de evento de Hubs de eventos **DeviceMessage**. A inclusão de `EntityPath` no **connectionString** é obrigatório:

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

    | Nome do atributo personalizado | Substituir |
    | --- | --- |
    | yourNamespace | O espaço de nomes do seu ponto final |
    | yourPrimaryKey | A cadeia de ligação primária utilizada para autenticar |
    | yourSecondaryKey | A cadeia de ligação secundária utilizada para autenticar |
    | yourEventHubName | O nome do seu hub de eventos |

> [!NOTE]  
> Após a criação de um novo ponto final, poderá demorar até 5 a 10 minutos para começar a receber eventos no ponto final.

## <a name="primary-and-secondary-connection-keys"></a>Chaves de ligação primária e secundária

Quando uma chave de ligação primária fica não autorizada, o sistema tenta automaticamente a chave de ligação secundária. Isso fornece uma cópia de segurança e permite que a possibilidade de maneira normal autenticar e atualizar a chave primária por meio da API de pontos de extremidade.

Se ambas as chaves de ligação primária e secundária não estão autorizadas, o sistema entra num tempo de espera de término exponencial de até 30 minutos. Eventos são soltos em cada tempo de espera de início de término acionada.

Sempre que o sistema está num término de estado de espera, atualização de ligações de chaves, através da API de pontos de extremidade podem demorar até 30 minutos para entrar em vigor.

## <a name="unreachable-endpoints"></a>Pontos de extremidade inacessíveis

Quando um ponto de extremidade fique inacessível, o sistema entra num tempo de espera de término exponencial de até 30 minutos. Eventos são soltos em cada tempo de espera de início de término acionada.

## <a name="next-steps"></a>Passos Seguintes

- Saiba mais [como utilizar o Swagger do Azure Digital duplos](how-to-use-swagger.md).

- Saiba mais sobre [encaminhamento de eventos e mensagens](concepts-events-routing.md) no duplos Digital do Azure.
