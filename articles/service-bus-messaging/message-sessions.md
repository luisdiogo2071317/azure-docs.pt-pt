---
title: Sessões de mensagens do Service Bus do Azure | Microsoft Docs
description: Processe as sequências de mensagens do Service Bus do Azure com sessões.
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
ms.date: 01/02/2018
ms.author: sethm
ms.openlocfilehash: 551432cd13c16fdd5423c46ed9c6f740353808f8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 03/23/2018
---
# <a name="message-sessions-first-in-first-out-fifo"></a>Sessões de mensagens: primeiro, primeiro out (FIFO) 

Sessões do Microsoft Azure Service Bus permitem processamento conjunta e ordenado de unbounded sequências de mensagens relacionadas. Para tirar partido uma garantia FIFO no Service Bus, utilize sessões. Barramento de serviço não é prescritiva sobre a natureza da relação entre as mensagens e também não definir um modelo particular para determinar em que uma sequência de mensagem inicia ou termina.

Qualquer remetente pode criar uma sessão quando enviar mensagens para um tópico ou uma fila definindo a [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId) propriedade para algumas Identificador definido pela aplicação que é exclusiva para a sessão. Ao nível do protocolo AMQP 1.0, este valor é mapeado para o *id do grupo* propriedade.

Em filas com suporte para a sessão ou subscrições, sessões entrem em existência quando existe, pelo menos, uma mensagem com a sessão [SessionId](/dotnet/api/microsoft.azure.servicebus.message.sessionid#Microsoft_Azure_ServiceBus_Message_SessionId). Quando existe uma sessão, não há nenhum tempo definido ou a API para quando a sessão expirar ou desaparece. Teoria, pode ser recebida uma mensagem para uma sessão de hoje em dia, a mensagem seguinte na hora de um ano e se o **SessionId** corresponder, a sessão é o mesmo da perspetiva de Service Bus.

Normalmente, no entanto, uma aplicação tem uma noção limpar de onde um conjunto de mensagens relacionadas inicia e termina. Barramento de serviço não define quaisquer regras específicas.

Um exemplo de como delinear uma sequência para transferir um ficheiro é para definir o **etiqueta** propriedade para a primeira mensagem para **iniciar**, mensagens intermédio para **conteúdo**, e para a última mensagem para **final**. A posição relativa das mensagens de conteúdo pode ser calculada como a mensagem atual *SequenceNumber* delta do **iniciar** mensagem *SequenceNumber*.

A funcionalidade de sessão ativa de Service Bus específico receber operação, sob a forma de [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) na c# e APIs de Java. Ativar a funcionalidade definindo a [Requersessão](/azure/templates/microsoft.servicebus/namespaces/queues#property-values) propriedade a fila ou a subscrição através do Azure Resource Manager, ou ao definir o sinalizador no portal. Isto é necessário antes de tentar a utilizar as operações de API relacionadas.

No portal, defina o sinalizador com caixa de verificação seguinte:

![][2]

As APIs para sessões existem em clientes de fila e a subscrição. Há um modelo de imperativo que controla quando as sessões e as mensagens são recebidas e um baseado no processador de modelo, semelhante à *OnMessage*, que oculta a complexidade de gerir a receção cíclicas.

## <a name="session-features"></a>Funcionalidades de sessão

Sessões fornecem simultâneas anular multiplexação de fluxos de mensagens intercalado ao preservar e guaranteeing entrega ordenada.

![][1]

A [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) recetor é criado pelo cliente de aceitação de uma sessão. As chamadas de cliente [QueueClient.AcceptMessageSession](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesession#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSession) ou [QueueClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.servicebus.messaging.queueclient.acceptmessagesessionasync#Microsoft_ServiceBus_Messaging_QueueClient_AcceptMessageSessionAsync) em c#. No modelo de chamada de retorno reativa, regista um processador de sessão.

Quando o [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objeto é aceite e enquanto esta está retida por um cliente, esse cliente mantém um bloqueio exclusivo em todas as mensagens com nessa sessão [SessionId](/en-us/dotnet/api/microsoft.servicebus.messaging.messagesession.sessionid#Microsoft_ServiceBus_Messaging_MessageSession_SessionId) que existe na fila ou subscrição, e também em todas as mensagens com que **SessionId** que chegam ainda enquanto a sessão é mantida.

O bloqueio é libertado quando **fechar** ou **CloseAsync** são denominados, ou quando o bloqueio expira nos casos em que a aplicação é não é possível efetuar a operação de fecho. O bloqueio de sessão deve ser tratado como um bloqueio exclusivo num ficheiro, que significa que a aplicação deve fechar a sessão, assim já não tem-e/ou não espera mais mensagens.

Quando vários recetores simultâneos solicitar a partir da fila, as mensagens que pertencem a uma determinada sessão são distribuídas para o recetor específico que atualmente detém o bloqueio para essa sessão. Com essa operação, um fluxo de mensagens intercalado que reside numa fila ou a subscrição está corretamente anular multiplexado a recetores diferentes e os recetores também podem em direto em máquinas de cliente diferente, uma vez que a gestão de bloqueio acontece lado do serviço, no interior Barramento de serviço.

A ilustração anterior mostra três recetores de sessão em simultâneo. Uma sessão com `SessionId` = 4 não tem nenhum Active Directory proprietário de cliente, que significa que não existem mensagens são entregues desta sessão específica. Uma sessão funciona em várias formas, como um uma fila sub.

O bloqueio de sessão, o recetor de sessão é um umbrella para os bloqueios de mensagem utilizado pelo *peek bloqueio* modo settlement. Um recetor não pode ter duas mensagens simultaneamente "em trânsito", mas as mensagens têm de ser processadas por ordem. Uma nova mensagem só pode ser obtida quando a mensagem anterior foi concluída ou lettered de mensagens não. Abandonar um causas de mensagem a mesma mensagem para ser servidos novamente com a próxima operação de receção.

## <a name="message-session-state"></a>Estado da sessão de mensagem

Quando os fluxos de trabalho são processados em grande escala, os sistemas em nuvem de elevada disponibilidade, o processador de fluxo de trabalho associado uma determinada sessão devem ser capazes de recuperar de falhas inesperadas e também de ser capaz de retomar o trabalho parcialmente concluído no outro máquina em que o trabalho data de início ou processo.

A instalações de estado de sessão permite uma anotação definido pela aplicação de uma sessão de mensagem dentro do Mediador, para que o estado de processamento registado relativamente à sessão torna-se disponíveis de imediato quando a sessão é adquirida por um processador de novo.

Da perspetiva de Service Bus, o estado da sessão mensagem é um objeto de binário opaco que possa englobar os dados do tamanho de uma mensagem é 256 KB para padrão de barramento de serviço e de 1 MB para Premium do Service Bus. O estado de processamento relativo para uma sessão pode ser contido dentro o estado da sessão ou o estado da sessão pode apontar para alguns localização de armazenamento ou o registo de base de dados que contém essas informações.

As APIs para gerir o estado da sessão, [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) e [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState), pode ser encontrado no [MessageSession](/dotnet/api/microsoft.servicebus.messaging.messagesession) objeto c# tanto nas APIs de Java. Numa sessão que tinha anteriormente sem estado de sessão definido devolve um **nulo** referenciar para **GetState**. Limpar o estado da sessão previamente definido é feito com [SetState(null)](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_).

Tenha em atenção que o estado da sessão permanece desde que não está a ser desmarcada cópias de segurança (devolver **nulo**), mesmo se todas as mensagens existentes numa sessão são consumidas.

Todas as sessões existentes numa fila ou a subscrição podem ser enumeradas com o **SessionBrowser** método na Java API e com [GetMessageSessions](/dotnet/api/microsoft.servicebus.messaging.queueclient.getmessagesessions#Microsoft_ServiceBus_Messaging_QueueClient_GetMessageSessions) no [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) e [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) no cliente do .NET.

O estado da sessão contido uma fila ou em que contagens de uma subscrição para a quota de armazenamento essa entidade. Quando a aplicação for concluída com uma sessão, recomenda-se, por conseguinte, para a aplicação Limpar estado retido para evitar o custo de gestão externo.

## <a name="next-steps"></a>Passos Seguintes

- [Um exemplo completo](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) enviar e receber mensagens baseados em sessão do barramento de serviço e escalona as utilizando a biblioteca .NET padrão.
- [Um exemplo](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/Sessions) que utiliza o cliente do .NET Framework para processar mensagens com suporte para a sessão. 

Para obter mais informações sobre mensagens do Service Bus, consulte os tópicos seguintes:

* [Noções básicas sobre o Service Bus](service-bus-fundamentals-hybrid-solutions.md)
* [Filas, tópicos e subscrições do Service Bus](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Como utilizar os tópicos e as subscrições do Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/message-sessions/sessions.png
[2]: ./media/message-sessions/queue-sessions.png