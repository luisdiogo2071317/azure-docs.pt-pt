---
title: O Azure Service Bus emparelhado espaços de nomes | Documentos da Microsoft
description: Detalhes de implementação do espaço de nomes emparelhado e os custos
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 2440c8d3-ed2e-47e0-93cf-ab7fbb855d2e
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: aschhab
ms.openlocfilehash: 6b1dfc3b9b2d54922b8d2de2e4f7e168ce21fb8f
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55661052"
---
# <a name="paired-namespace-implementation-details-and-cost-implications"></a>Emparelhadas detalhes de implementação do espaço de nomes e as implicações de custos

O [PairNamespaceAsync] [ PairNamespaceAsync] método, usando um [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] de instância, executa tarefas visível em seu nome. Uma vez que existem são considerações de custos ao utilizar a funcionalidade, é útil para entender essas tarefas, para que esperar o comportamento quando isso acontece. A API interage com o seguinte comportamento automático em seu nome:

* Criação de filas de registo de segurança.
* Criação de um [MessageSender] [ MessageSender] objeto que se comunica com filas ou tópicos.
* Quando uma entidade de mensagens fica indisponível, envia ping mensagens para a entidade numa tentativa de detectar quando essa entidade fica novamente disponível.
* Opcionalmente, cria um conjunto de "bombas de mensagem" que mover mensagens das filas de registo de segurança para as filas primárias.
* Coordena/falha de fecho da primária e secundária [MessagingFactory] [ MessagingFactory] instâncias.

Num alto nível, o recurso funciona da seguinte forma: quando a entidade principal está em bom estada, sem alterações de comportamento ocorrerem. Quando o [FailoverInterval] [ FailoverInterval] decorrer de duração e vê a entidade principal não bem-sucedida envia após uma não transitórias [MessagingException] [ MessagingException] ou uma [TimeoutException][TimeoutException], ocorre o seguinte comportamento:

1. Envie operações para a entidade principal estão desativadas e o sistema efetua ping a entidade principal até que a pings podem ser entregue com êxito.
2. Uma fila de registo de segurança aleatório está selecionada.
3. [BrokeredMessage] [ BrokeredMessage] objetos são encaminhados para a fila de registo de segurança escolhida.
4. Se uma operação de envio na fila de registo de segurança escolhida falhar, essa fila é extraída de um a rotação e uma nova fila é selecionada. Todos os remetentes sobre o [MessagingFactory] [ MessagingFactory] instância saiba da falha.

Imagens seguintes descrever a sequência. Em primeiro lugar, o remetente envia mensagens.

![Espaços de nomes emparelhados][0]

Após a falha ao enviar para a fila primária, o remetente começa a enviar mensagens para uma fila de registo de segurança escolhido aleatoriamente. Simultaneamente, ele inicia uma tarefa de ping.

![Espaços de nomes emparelhados][1]

Neste momento, as mensagens estão ainda na fila secundária e não tem sido disponibilizadas para a fila primária. Depois da fila primária está em bom estada novamente, pelo menos um processo deve estar a executar o syphon. O syphon envia as mensagens de todas as várias filas de registo de segurança para as entidades de destino adequado (filas e tópicos).

![Espaços de nomes emparelhados][2]

O resto deste tópico aborda os detalhes específicos de como essas partes funcionam.

## <a name="creation-of-backlog-queues"></a>Criação de filas de registo de segurança
O [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] objeto passado para o [PairNamespaceAsync] [ PairNamespaceAsync] método indica o número de filas de registo de segurança que pretende utilizar. Cada fila de registo de segurança, em seguida, é criada com as seguintes propriedades explicitamente definido (todos os outros valores são definidos para o [QueueDescription] [ QueueDescription] predefinições):

| Caminho | [espaço de nomes principal] / x-servicebus-transferência / [Índice] em que [Índice] é um valor na [0, BacklogQueueCount) |
| --- | --- |
| MaxSizeInMegabytes |5120 |
| MaxDeliveryCount |int.MaxValue |
| DefaultMessageTimeToLive |TimeSpan.MaxValue |
| AutoDeleteOnIdle |TimeSpan.MaxValue |
| LockDuration |1 minuto |
| EnableDeadLetteringOnMessageExpiration |true |
| EnableBatchedOperations |true |

Por exemplo, a primeira fila de registo de segurança criada para espaço de nomes **contoso** com o nome `contoso/x-servicebus-transfer/0`.

Ao criar as filas, o código verifica primeiro se existe uma fila. Se a fila não existir, a fila é criada. Não contém o código de limpeza do wsu filas de registo de segurança "extra". Especificamente, se a aplicação com o espaço de nomes primário **contoso** solicita cinco filas de registo de segurança, mas uma fila de registo de segurança com o caminho `contoso/x-servicebus-transfer/7` existir, essa fila de registo de segurança extra ainda está presente, mas não é utilizada. O sistema permite explicitamente filas de registo de segurança adicionais a existir, que não poderia ser utilizadas. Como o proprietário do espaço de nomes, é responsável por limpar quaisquer filas de registo de segurança não utilizados/indesejável. O motivo para essa decisão é que o Service Bus não é possível saber com que fins existem para todas as filas no espaço de nomes. Além disso, se uma fila existe com o nome fornecido, mas não cumpre o assumida [QueueDescription][QueueDescription], em seguida, os motivos pelos quais são suas próprias para alterar o comportamento predefinido. Não existem garantias são feitas alterações de filas a lista de pendências com seu código. Lembre-se de que realize testes minuciosos após as alterações.

## <a name="custom-messagesender"></a>MessageSender personalizado
Ao enviar, todas as mensagens de passam por um interno [MessageSender] [ MessageSender] filas de objeto que se comporta normalmente quando tudo está funcionando e redireciona para o registo de segurança quando as coisas "fugir." Após o recebimento de uma falha não transitórias, inicia um temporizador. Depois de um [TimeSpan] [ TimeSpan] período que consiste o [FailoverInterval] [ FailoverInterval] durante o qual são enviadas sem mensagens com êxito, o valor da propriedade a ativação pós-falha estiver envolvida. Neste momento, os seguintes procedimentos ocorrem para cada entidade:

* Executa uma tarefa de ping cada [PingPrimaryInterval] [ PingPrimaryInterval] para verificar se a entidade está disponível. Assim que esta tarefa for concluída com êxito, todo o código de cliente que utiliza a entidade imediatamente começa a enviar novas mensagens para o espaço de nomes principal.
* As solicitações futuras para enviar para essa mesma entidade de quaisquer outros remetentes resultará no [BrokeredMessage] [ BrokeredMessage] a ser enviados para ser modificado sentar-se a fila de registo de segurança. A modificação remove algumas propriedades a partir da [BrokeredMessage] [ BrokeredMessage] de objeto e armazena-os em outro lugar. As seguintes propriedades são limpos e adicionadas num novo alias, permitindo que o Service Bus e o SDK processar mensagens de maneira uniforme:

| Antigo nome da propriedade | Novo nome de propriedade |
| --- | --- |
| SessionId |x-ms-sessionid |
| TimeToLive |x-ms-timetolive |
| ScheduledEnqueueTimeUtc |x-ms-path |

O caminho de destino original é armazenado na mensagem de como uma propriedade chamada x-ms-path. Esse design permite que as mensagens para várias entidades coexistir numa fila única lista de pendências. As propriedades são traduzidas volta a syphon.

O personalizado [MessageSender] [ MessageSender] objeto pode sofrer problemas quando o limite de 256 KB abordam as mensagens, e ativação pós-falha está participando. O personalizado [MessageSender] [ MessageSender] objeto armazena mensagens para todas as filas e tópicos em conjunto nas filas de registo de segurança. Este objeto mistura mensagens a partir de várias cores primárias em conjunto dentro das filas de registo de segurança. Para lidar com balanceamento de carga entre vários clientes que não sabem entre si, o SDK seleciona aleatoriamente uma fila de registo de segurança para cada [QueueClient] [ QueueClient] ou [TopicClient] [ TopicClient] que criar no código.

## <a name="pings"></a>Os pings do
Uma mensagem ping está vazio [BrokeredMessage] [ BrokeredMessage] com seu [ContentType] [ ContentType] propriedade definida como aplicação/vnd.ms-servicebus-ping e um [TimeToLive] [ TimeToLive] valor de 1 segundo. Este ping tem uma característica especial no Service Bus: o servidor nunca fornece um ping quando qualquer chamador solicita uma [BrokeredMessage][BrokeredMessage]. Portanto, nunca precisa que aprender a receber e a ignorar essas mensagens. Cada entidade (exclusiva fila ou tópico) por [MessagingFactory] [ MessagingFactory] instância por cliente irá ser alcançados pelo ping quando eles são considerados fique indisponível. Por predefinição, isto ocorre uma vez por minuto. Mensagens ping são consideradas como mensagens do Service Bus regulares e podem resultar em custos de largura de banda e de mensagens. Assim que os clientes detetarem que o sistema estiver disponível, param as mensagens.

## <a name="the-syphon"></a>O syphon
Pelo menos um programa executável do aplicativo deve estar ativamente em execução o syphon. O syphon executa um longo poll receber que tem uma duração de 15 minutos. Quando todas as entidades estão disponíveis e tem 10 filas de registo de segurança, o aplicativo que hospeda o syphon chama a operação receive 40 vezes por hora, 960 vezes por dia e tempos de 28800 em 30 dias. Quando o syphon ativamente é passar mensagens da lista de pendências para a fila primária, cada mensagem ocorre com os seguintes custos (custos padrão para o tamanho da mensagem e largura de banda aplicam-se em todas as fases):

1. Envie para o registo de segurança.
2. Receba da lista de pendências.
3. Envie para o primário.
4. Receba dos principais.

## <a name="closefault-behavior"></a>Comportamento de fechar/falhas
Dentro de um aplicativo que aloja o syphon, uma vez a primária ou secundária [MessagingFactory] [ MessagingFactory] de falhas ou está fechado sem o respetivo parceiro também com falhas ou fechado e o syphon Deteta neste estado, o syphon funciona. Se o outro [MessagingFactory] [ MessagingFactory] não seja fechado dentro de 5 segundos, o syphon de falhas do abrir ainda [MessagingFactory][MessagingFactory].

## <a name="next-steps"></a>Passos Seguintes
Ver [elevada disponibilidade e padrões de mensagens assíncronas] [ Asynchronous messaging patterns and high availability] para um debate detalhado de mensagens assíncronas do Service Bus. 

[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[MessageSender]: /dotnet/api/microsoft.servicebus.messaging.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[FailoverInterval]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[TimeoutException]: https://msdn.microsoft.com/library/azure/system.timeoutexception.aspx
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[QueueDescription]: /dotnet/api/microsoft.servicebus.messaging.queuedescription
[TimeSpan]: https://msdn.microsoft.com/library/azure/system.timespan.aspx
[PingPrimaryInterval]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_PingPrimaryInterval
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[ContentType]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
[TimeToLive]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md
[0]: ./media/service-bus-paired-namespaces/IC673405.png
[1]: ./media/service-bus-paired-namespaces/IC673406.png
[2]: ./media/service-bus-paired-namespaces/IC673407.png
