---
title: Esquema de eventos de subscrição do Azure Event Grid
description: Descreve as propriedades que são fornecidas para eventos de subscrição com o Azure Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 08/02/2018
ms.author: tomfitz
ms.openlocfilehash: 59c41d4303cad00107793f3c09382347fbb1fb5b
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480390"
---
# <a name="azure-event-grid-event-schema-for-subscriptions"></a>Esquema de eventos do Azure Event Grid para subscrições

Este artigo fornece as propriedades e o esquema para eventos de subscrição do Azure. Para obter uma introdução aos esquemas de eventos, consulte [esquema de eventos do Azure Event Grid](event-schema.md).

As subscrições do Azure e grupos de recursos emitem os mesmos tipos de evento. Os tipos de evento estão relacionados a alterações em recursos. A principal diferença é que grupos de recursos emitem eventos para os recursos no grupo de recursos e subscrições do Azure emitem eventos para recursos entre a subscrição.

## <a name="available-event-types"></a>Tipos de eventos disponíveis

As subscrições do Azure emitem eventos de gestão do Azure Resource Manager, como quando é criada uma VM ou uma conta de armazenamento é eliminada.

| Tipo de evento | Descrição |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Gerado quando um recurso de criar ou atualizar a operação for concluída com êxito. |
| Microsoft.Resources.ResourceWriteFailure | Desencadeado quando criar um recurso ou falha de operação de atualização. |
| Microsoft.Resources.ResourceWriteCancel | Gerado quando um recurso de criar ou atualizar a operação foi cancelada. |
| Microsoft.Resources.ResourceDeleteSuccess | Gerado quando uma operação de eliminação do recurso é bem-sucedida. |
| Microsoft.Resources.ResourceDeleteFailure | Desencadeado quando ocorre uma falha de uma operação de eliminação de recursos. |
| Microsoft.Resources.ResourceDeleteCancel | Gerado quando uma operação de eliminação de recursos é cancelada. Este evento ocorre quando uma implementação do modelo é cancelada. |

## <a name="example-event"></a>Evento de exemplo

O exemplo seguinte mostra o esquema para uma **ResourceWriteSuccess** eventos. O mesmo esquema é utilizado para **ResourceWriteFailure** e **ResourceWriteCancel** eventos com valores diferentes para `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
}]
```

O exemplo seguinte mostra o esquema para uma **ResourceDeleteSuccess** eventos. O mesmo esquema é utilizado para **ResourceDeleteFailure** e **ResourceDeleteCancel** eventos com valores diferentes para `eventType`.

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}"
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
| dados | objeto | Dados de eventos de subscrição. |
| dataVersion | cadeia | A versão do esquema do objeto de dados. O publicador define a versão do esquema. |
| metadataVersion | cadeia | A versão do esquema dos metadados do evento. Grelha de eventos define o esquema das propriedades de nível superior. Event Grid fornece este valor. |

O objeto de dados tem as seguintes propriedades:

| Propriedade | Tipo | Descrição |
| -------- | ---- | ----------- |
| Autorização | objeto | O pedido de autorização para a operação. |
| afirmações | objeto | As propriedades de afirmações. Para obter mais informações, consulte [especificação do JWT](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | cadeia | Um ID de operação para resolução de problemas. |
| httpRequest | objeto | Os detalhes da operação. Este objeto só é incluído ao atualizar um recurso existente ou eliminar um recurso. |
| ResourceProvider | cadeia | O fornecedor de recursos, executar a operação. |
| resourceUri | cadeia | O URI do recurso na operação. |
| operationName | cadeia | A operação que foi executada. |
| status | cadeia | O estado da operação. |
| subscriptionId | cadeia | O ID de subscrição do recurso. |
| TenantId | cadeia | O ID de inquilino do recurso. |

## <a name="next-steps"></a>Passos Seguintes

* Para obter uma introdução ao Azure Event Grid, veja [o que é o Event Grid?](overview.md).
* Para obter mais informações sobre a criação de uma subscrição do Azure Event Grid, veja [esquema de subscrições do Event Grid](subscription-creation-schema.md).
