---
title: Descrição geral do processamento de transações no Azure Service Bus | Documentos da Microsoft
description: Visão geral das transações atômicas de Azure Service Bus e envio através de
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 64449247-1026-44ba-b15a-9610f9385ed8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/22/2018
ms.author: spelluru
ms.openlocfilehash: bc0e0bfb8d4dc8637c191785a98f5d15e086cbf5
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696436"
---
# <a name="overview-of-service-bus-transaction-processing"></a>Descrição geral do processamento de transações do Service Bus

Este artigo descreve os recursos de transação do Microsoft Azure Service Bus. Grande parte da discussão é ilustrada pela [transações Atômicas com de exemplo do Service Bus](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions). Este artigo está limitado a uma visão geral do processamento de transações e o *enviar através de* funcionalidade do Service Bus, enquanto o exemplo de transações Atômicas é mais abrangente e mais complexas no âmbito.

## <a name="transactions-in-service-bus"></a>Transações no Service Bus

R [ *transação* ](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions#what-are-transactions) agrupa dois ou mais operações em conjunto num *âmbito de execução*. Por natureza, tal uma transação tem de garantir que todas as operações que pertencem a um determinado grupo de operações com êxito ou falham em conjunto. Nesse aspecto transações atuam como uma unidade, o que é, muitas vezes, denominada *atomicidade*. 

Service Bus é um mediador de mensagens transacionais e garante a integridade transacional para todas as operações internas em relação a seus arquivos de mensagem. Todas as transferências de mensagens dentro de barramento de serviço, por exemplo, transferindo mensagens para um [fila de mensagens não entregues](service-bus-dead-letter-queues.md) ou [encaminhamento automático](service-bus-auto-forwarding.md) de mensagens entre entidades, são transacionais. Como tal, se o Service Bus aceita uma mensagem, ele já foi armazenado e rotulado com um número de sequência. De ora em diante, todas as transferências de mensagem dentro do Service Bus são operações de coordenada em entidades e será nenhum levar à perda (for concluída com êxito de origem e destino falha) ou para duplicação (falha de origem e destino for concluída com êxito) da mensagem.

O Service Bus suporta operações de agrupamento em relação a uma entidade de mensagens única (fila, tópico, subscrição) no âmbito de uma transação. Por exemplo, pode enviar várias mensagens para uma fila de dentro de um âmbito de transação e as mensagens só pode ser comprometidas em log da fila quando a transação é concluída com êxito.

## <a name="operations-within-a-transaction-scope"></a>Operações dentro de um âmbito de transação

As operações que podem ser executadas dentro de um âmbito de transação são os seguintes:

* **[QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient), [MessageSender](/dotnet/api/microsoft.azure.servicebus.core.messagesender), [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient)**: enviar, SendAsync, SendBatch, SendBatchAsync 
* **[BrokeredMessage](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage)**: concluir o procedimento, CompleteAsync, abandono, AbandonAsync, mensagens não entregues, DeadletterAsync, diferir, DeferAsync, RenewLock, RenewLockAsync 

Receber operações não são incluídas, porque se presume que o aplicativo adquire mensagens utilizando o [ReceiveMode.PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) loop de recebimento de modo, dentro de alguns ou com um [OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage) retorno de chamada, e só que, em seguida, abre-se um escopo de transação para o processamento da mensagem.

Em seguida, ocorre a disposição da mensagem (completa, diferir de abandono, mensagens não entregues) dentro do escopo e dependentes na, o resultado geral da transação.

## <a name="transfers-and-send-via"></a>As transferências e "enviar por meio de"

Para ativar a entregar direito de permanência transacional de dados de uma fila para um processador e, em seguida, a outra fila, o Service Bus suporta *transferências*. Numa operação de transferência, um remetente primeiro envia uma mensagem para um *fila de transferência*, e a fila de transferência imediatamente move a mensagem para a fila de destino pretendido utilizando a mesma implementação de transferência robusta que o reencaminhamento capacidade de se baseia em. A mensagem nunca tem o compromisso de registo de fila de transferência de uma forma que ele se torna visível para os consumidores de fila de transferência.

O poder desta capacidade transacional se torna aparente quando a fila de transferência em si é a origem de mensagens de entrada do remetente. Em outras palavras, do Service Bus pode transferir a mensagem para a fila de destino "via" fila de transferência, ao efetuar uma completa (ou diferir, ou mensagens não entregues) operação na mensagem de entrada, tudo numa operação atômica. 

### <a name="see-it-in-code"></a>Vê-lo em código

Para configurar a essas transferências, vai criar um remetente da mensagem direcionada para a fila de destino através da fila de transferência. Também tem um recetor que obtém as mensagens nessa mesma fila. Por exemplo:

```csharp
var sender = this.messagingFactory.CreateMessageSender(destinationQueue, myQueueName);
var receiver = this.messagingFactory.CreateMessageReceiver(myQueueName);
```

Uma transação simple, em seguida, usa esses elementos, como no exemplo seguinte:

```csharp
var msg = receiver.Receive();

using (scope = new TransactionScope())
{
    // Do whatever work is required 

    var newmsg = ... // package the result 

    msg.Complete(); // mark the message as done
    sender.Send(newmsg); // forward the result

    scope.Complete(); // declare the transaction done
} 
```

## <a name="next-steps"></a>Passos Seguintes

Veja os artigos seguintes para obter mais informações sobre as filas do Service Bus:

* [Como utilizar as filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Encadeamento de entidades do Service Bus com o reencaminhamento automático](service-bus-auto-forwarding.md)
* [Exemplo de reencaminhamento](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AutoForward)
* [Transações atômicas com de exemplo do Service Bus](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/AtomicTransactions)
* [Filas do Azure e do Service Bus filas em comparação comparadas](service-bus-azure-and-service-bus-queues-compared-contrasted.md)


