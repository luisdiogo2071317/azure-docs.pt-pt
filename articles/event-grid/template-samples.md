---
title: Exemplos de modelos do Azure Resource Manager - Event Grid | Microsoft Docs
description: Exemplos de modelos do Azure Resource Manager para Event Grid
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.date: 03/27/2018
ms.author: tomfitz
ms.openlocfilehash: f4d6a663b4e0d2c2166028051e713668534b20bc
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/28/2018
---
# <a name="azure-resource-manager-templates-for-event-grid"></a>Modelos do Azure Resource Manager para Event Grid

A tabela seguinte inclui ligações para modelos do Azure Resource Manager para o Event Grid.

| | |
|-|-|
|**Subscrições do Event Grid**||
| [Tópico personalizado e subscrição com o ponto final do WebHook](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid)| Implementa um tópico personalizado do Event Grid. Cria uma subscrição para esse tópico personalizado que utiliza um ponto final do WebHook. |
| [Subscrição do tópico personalizado com ponto final do EventHub](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/subscribeCustomTopicToEventHub.json)| Cria uma subscrição do Event Grid para um tópico personalizado que já existe. A subscrição utiliza um Hub de Eventos para o ponto final. |
| [Subscrição do grupo de recursos](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/subscribeResourceGroupToWebHook.json)| Subscreve eventos para um grupo de recursos. O grupo de recursos que especificar como destino durante a implementação é a origem de eventos. A subscrição utiliza um Hub de Eventos para o ponto final. |
| [Conta de armazenamento de blobs e subscrição](https://github.com/Azure/azure-docs-json-samples/blob/master/event-grid/createBlobAndSubscribe.json)| Implementa uma conta de armazenamento de Blobs do Azure e subscreve eventos para essa conta de armazenamento. |
| | |
