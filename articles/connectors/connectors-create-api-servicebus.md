---
title: Saiba como utilizar o conector do Service Bus do Azure nas suas logic apps | Microsoft Docs
description: "Crie aplicações lógicas com o App service do Azure. Ligar ao Service Bus do Azure para enviar e receber mensagens. Pode efetuar ações como envie a fila, enviar para o tópico, receber da fila e receber de subscrição."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 89bf0ffec759fca4af5f99af1b6a2dd8d641ff6f
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-azure-service-bus-connector"></a>Começar a utilizar o conector do Service Bus do Azure
Ligar ao Service Bus do Azure para enviar e receber mensagens. Pode efetuar ações como envie a fila, enviar para o tópico, receber da fila e receber de subscrição.

Para utilizar [qualquer conector](apis-list.md), terá primeiro de criar uma aplicação lógica. Pode começar a utilizar pelo [criar uma aplicação lógica agora](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-service-bus"></a>Ligar ao Service Bus
Antes da aplicação lógica pode aceder a qualquer serviço, terá primeiro de criar uma ligação ao serviço. A [ligação](connectors-overview.md) fornece conectividade entre uma aplicação lógica e outro serviço.  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a>Utilizar um acionador de Service Bus
Um acionador é um evento que pode ser utilizado para iniciar o fluxo de trabalho definido numa aplicação lógica. [Saiba mais sobre acionadores](../logic-apps/logic-apps-overview.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a>Utilizar uma ação de barramento de serviço
Uma ação é uma operação levada a cabo pelo fluxo de trabalho definido numa aplicação lógica. [Saiba mais sobre as ações](../logic-apps/logic-apps-overview.md#logic-app-concepts).

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a>Detalhes específicos do conector

Ver todos os acionadores e ações definidas no swagger e consulte também os limites no [detalhes do conector](/connectors/servicebus/). 

## <a name="next-steps"></a>Passos Seguintes
[Criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

