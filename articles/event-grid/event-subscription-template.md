---
title: "Subscrição de grelha de eventos do Azure com o modelo"
description: "Crie uma subscrição de grelha de eventos com um modelo do Resource Manager."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: tomfitz
ms.openlocfilehash: 2b9f55f8e944d688b622e30a773e1a34698f22ec
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/05/2018
---
# <a name="use-resource-manager-template-for-event-grid-subscription"></a>Utilize o Gestor de recursos de modelo para a subscrição de evento grelha

Este artigo mostra como utilizar um modelo Azure Resource Manager para criar subscrições de grelha de eventos. O formato a que utilizar é diferente com base em se estão subscrever para eventos de grupo de recursos ou eventos para um tipo de recurso específico. Ambos os formatos são apresentados neste artigo.

## <a name="subscribe-to-resource-group-events"></a>Subscrever eventos do grupo de recursos

Ao subscrever de eventos do grupo de recursos, utilize `Microsoft.EventGrid/eventSubscriptions` para o tipo de recurso. Para o evento de pontos de tipo, utilize a `WebHook` ou `EventHub`.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "type": "Microsoft.EventGrid/eventSubscriptions",
            "name": "mySubscription",
            "apiVersion": "2017-09-15-preview",
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
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01-preview/deploymentTemplate.json#",
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
            "apiVersion": "2017-09-15-preview",
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