---
title: "Navegação da mensagem de Service Bus do Azure | Microsoft Docs"
description: Procurar e observar as mensagens do Service Bus
services: service-bus-messaging
documentationcenter: 
author: clemensv
manager: timlt
editor: 
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2018
ms.author: sethm
ms.openlocfilehash: 124c4592a41bf9f3e2a148ba5c3b928bb051d160
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2018
---
# <a name="message-browsing"></a>Mensagem de navegação

Mensagem de navegação ("leitura") permite que um cliente de Service Bus enumerar todas as mensagens que reside numa fila ou a subscrição, normalmente de diagnóstico e fins de depuração.

As operações de peek devolvem todas as mensagens que existem no registo de mensagens fila ou a subscrição, não apenas os disponíveis para a aquisição de imediato com `Receive()` ou `OnMessage()` ciclo. O `State` propriedade de cada mensagem a indicar se a mensagem está ativa (disponível para ser recebida) [diferida](message-deferral.md), ou [agendada](message-sequencing.md).

As mensagens foram consumidas e expiradas são limpos por um assíncrona "libertação" executar e não necessariamente exatamente quando mensagens expirarem e, por isso `Peek` realmente podem devolver mensagens que já expiraram e serão removidas ou quando lettered de mensagens não uma operação de receção seguinte é invocada na fila ou a subscrição.

Isto é especialmente importante para manter em mente quando tenta recuperar diferidas mensagens da fila. Uma mensagem para o qual o [ExpiresAtUtc](/dotnet/api/microsoft.azure.servicebus.message.expiresatutc#Microsoft_Azure_ServiceBus_Message_ExpiresAtUtc) instantâneas passou já não é elegível para regular obtenção através de outros meios, mesmo quando está a ser devolvido por observar. Devolver estas mensagens é deliberate, uma vez que observar é uma ferramenta de diagnóstico ao refletir o estado atual do registo.

Também observar devolve mensagens que foram bloqueadas e estão atualmente a ser processadas por outros recetores, mas ainda não foram concluídas. No entanto, porque observar devolve um instantâneo de desligado, o estado de bloqueio de uma mensagem não é possível observar nas mensagens peeked e o [LockedUntilUtc](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.lockeduntilutc#Microsoft_Azure_ServiceBus_Core_MessageReceiver_LockedUntilUtc) e [LockToken](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.locktoken#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_LockToken) propriedades throw um [ InvalidOperationException](/dotnet/api/system.invalidoperationexception) quando a aplicação tenta lê-los.

## <a name="peek-apis"></a>Observar APIs

O [observar/PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync#Microsoft_Azure_ServiceBus_Core_MessageReceiver_PeekAsync) e [PeekBatch/PeekBatchAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatchasync#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatchAsync_System_Int64_System_Int32_) métodos existem em todos os .NET e Java bibliotecas de cliente e em todos os objetos de recetor: **MessageReceiver**, **MessageSession**, **QueueClient**, e **SubscriptionClient**. Observar funciona em todas as filas e subscrições e as respetivas filas de entregues.

Quando chamado repetidamente, o método observar enumera todas as mensagens que existe no registo de fila ou a subscrição, por ordem de número de sequência, do número mais baixa sequência disponíveis para o maior. Esta é a ordem em que as mensagens foram colocados em fila; Não é a ordem na qual mensagens, eventualmente, poderão ser obtidas.

[PeekBatch](/dotnet/api/microsoft.servicebus.messaging.queueclient.peekbatch#Microsoft_ServiceBus_Messaging_QueueClient_PeekBatch_System_Int32_) obtém várias mensagens e devolve-os como uma enumeração. Se não existem mensagens estiverem disponíveis, o objeto de enumeração está vazio, não nulo.

Também pode seed uma sobrecarga do método com um [SequenceNumber](/dotnet/api/microsoft.azure.servicebus.message.systempropertiescollection.sequencenumber#Microsoft_Azure_ServiceBus_Message_SystemPropertiesCollection_SequenceNumber) no qual pretende iniciar e, em seguida, chame a sobrecarga do método sem parâmetros ao enumerar adicional. **PeekBatch** equivalently funciona, mas obtém um conjunto de mensagens ao mesmo tempo.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre mensagens do Service Bus, consulte os tópicos seguintes:

* [Noções básicas sobre o Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)