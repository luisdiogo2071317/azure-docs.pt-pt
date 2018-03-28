---
title: Subscrição de grelha de eventos do Azure com o modelo
description: Crie uma subscrição de grelha de eventos com um modelo do Resource Manager.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: ee0b2c228ae4ea53c0ee9794529aa190334ceed9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="use-resource-manager-template-for-event-grid-subscription"></a>Utilize o Gestor de recursos de modelo para a subscrição de evento grelha

Este artigo mostra como utilizar um modelo Azure Resource Manager para criar subscrições de grelha de eventos. O formato a que utilizar é diferente com base em se estão subscrever para eventos de grupo de recursos ou eventos para um tipo de recurso específico. Ambos os formatos são apresentados neste artigo.

## <a name="subscribe-to-resource-group-events"></a>Subscrever eventos do grupo de recursos

Ao subscrever de eventos do grupo de recursos, utilize `Microsoft.EventGrid/eventSubscriptions` para o tipo de recurso. Para o evento de pontos de tipo, utilize a `WebHook` ou `EventHub`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.EventGrid/eventSubscriptions",
            "name": "mySubscription",
            "apiVersion": "2018-01-01",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false,
                    "includedEventTypes": ["All"]
                }
            }
        }
    ]
}
```

Ao implementar este modelo para um grupo de recursos, subscrever eventos para esse grupo de recursos.

## <a name="subscribe-to-resource-events"></a>Subscrever eventos de recursos

Quando subscrever eventos de recursos, associar a subscrição para o recurso correta, incluindo o nome e tipo de recurso na definição da subscrição. Tipo de recurso, utilize `<provider-namespace>/<resource-type>/providers/eventSubscriptions`. Para o nome, utilize `<resource-name>/Microsoft.EventGrid/<subscription-name>`. Para o evento de pontos de tipo, utilize a `WebHook` ou `EventHub`.

O exemplo seguinte mostra como subscrever eventos de armazenamento de Blobs.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageName": {
            "type": "string"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
            "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/myStorageSubscription')]",
            "apiVersion": "2018-01-01",
            "properties": {
                "destination": {
                    "endpointType": "WebHook",
                    "properties": {
                        "endpointUrl": "https://requestb.in/ynboxiyn"
                    }
                },
                "filter": {
                    "subjectBeginsWith": "",
                    "subjectEndsWith": "",
                    "isSubjectCaseSensitive": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Passos Seguintes

* Para uma introdução à grelha de eventos, consulte [sobre eventos grelha](overview.md).
* Para uma introdução ao Gestor de recursos, consulte [descrição geral do Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
* Para começar a grelha de eventos, consulte o artigo [criar e rota eventos personalizados com o Azure eventos grelha](custom-event-quickstart.md).