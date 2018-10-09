---
title: Contagem de mensagens do Service Bus do Azure | Documentos da Microsoft
description: Obter a contagem de mensagens do Service bus do Azure.
services: service-bus-messaging
documentationcenter: ''
author: clemensv
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2018
ms.author: spelluru
ms.openlocfilehash: 954c16cefe6d7ffe61a0b04b274b9bf92306a587
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48857589"
---
# <a name="message-counters"></a>Contadores de mensagens

Pode obter a contagem de mensagens contidas nas filas e subscrições através da utilização do Azure Resource Manager e o Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) APIs no SDK do .NET Framework.

Com o PowerShell, pode obter a contagem da seguinte forma:

```powershell
(Get-AzureRmServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue).CountDetails
```

## <a name="message-count-details"></a>Detalhes da contagem de mensagem

É útil para determinar se uma fila cria uma lista de pendências que requer mais recursos para processar do que o que atualmente tiver sido implementado saber a contagem de mensagens do Active Directory. Os seguintes detalhes de contador estão disponíveis no [MessageCountDetails](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails) classe:

-   [ActiveMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.activemessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ActiveMessageCount): mensagens na fila ou subscrição, que são ativo estaduais e pronto para entrega.
-   [DeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.deadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_DeadLetterMessageCount): mensagens na fila de mensagens não entregues.
-   [ScheduledMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.scheduledmessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_ScheduledMessageCount): mensagens no Estado agendado.
-   [TransferDeadLetterMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transferdeadlettermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferDeadLetterMessageCount): mensagens de falha de transferência para outra fila ou tópico e foram movidas para a fila de mensagens não entregues de transferência.
-   [TransferMessageCount](/dotnet/api/microsoft.servicebus.messaging.messagecountdetails.transfermessagecount#Microsoft_ServiceBus_Messaging_MessageCountDetails_TransferMessageCount): mensagens pendentes transferência em outra fila ou tópico.

Se um aplicativo quiser Dimensionar recursos com base no comprimento da fila, ele deve fazer isso com uma medida ritmo. A aquisição dos contadores de mensagem é uma operação dispendiosa dentro do Mediador de mensagens e executá-la com frequência diretamente e negativa afeta o desempenho de entidade.

## <a name="next-steps"></a>Passos Seguintes

Para saber mais sobre mensagens do Service Bus, consulte os seguintes tópicos:

* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)