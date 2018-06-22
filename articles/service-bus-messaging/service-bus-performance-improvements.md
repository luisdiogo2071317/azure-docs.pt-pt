---
title: Melhores práticas para melhorar o desempenho da utilização do Azure Service Bus | Microsoft Docs
description: Descreve como utilizar o Service Bus para otimizar o desempenho quando trocar mensagens mediadas.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 06/14/2018
ms.author: sethm
ms.openlocfilehash: e168dcab182f9eb30291b58bdde252ec66d18e8c
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301806"
---
# <a name="best-practices-for-performance-improvements-using-service-bus-messaging"></a>Melhores práticas para melhorias de desempenho através de mensagens do Service Bus

Este artigo descreve como utilizar o Service Bus do Azure para otimizar o desempenho quando trocar mensagens mediadas. A primeira parte deste artigo descreve os diferentes mecanismos que são fornecidos para o ajudar a melhorar o desempenho. A segunda parte fornece orientações sobre como utilizar o Service Bus de forma a que pode oferecer o melhor desempenho num determinado cenário.

Ao longo deste artigo, o termo "cliente" refere-se a qualquer entidade que acede ao Service Bus. Um cliente pode levar a função de um remetente ou um recetor. O termo "remetente" é utilizado para um cliente de fila ou um tópico de barramento de serviço que envia mensagens a uma subscrição de fila ou um tópico de barramento de serviço. O termo "recetor" refere-se a um cliente de fila ou a subscrição de barramento de serviço que recebe mensagens a partir de uma fila do Service Bus ou uma subscrição.

Estas secções apresentam alguns conceitos que o Service Bus utiliza para ajudar o desempenho de aumento.

## <a name="protocols"></a>Protocolos

Service Bus permite que os clientes enviar e receber mensagens através de um dos três protocolos:

1. (AMQP) do protocolo de colocação de mensagens de avançadas
2. Mensagens (SBMP) do protocolo do Service Bus
3. HTTP

AMQP e SBMP são mais eficientes, porque estes mantêm a ligação ao Service Bus, desde que existe a fábrica de mensagens. Também implementa a criação de batches e prefetching. A menos que explicitamente mencionado, todo o conteúdo neste artigo pressupõe que a utilização de AMQP ou SBMP.

## <a name="reusing-factories-and-clients"></a>Reutilizar fábricas e clientes

Cliente do Service Bus objetos, tais como [QueueClient] [ QueueClient] ou [MessageSender][MessageSender], são criados através de um [ MessagingFactory] [ MessagingFactory] objeto, que também fornece gestão interno de ligações. É recomendado que não fechar as fábricas de mensagens ou clientes de fila, tópico e uma subscrição depois de enviar uma mensagem e, em seguida, recrie-los ao enviar a mensagem seguinte. Fechar uma fábrica de mensagens elimina a ligação ao serviço do Service Bus e uma nova ligação é estabelecida quando recriar a fábrica. Estabelecer uma ligação é uma operação dispendiosa, que pode evitar por reutilizar a mesma fábrica e os objetos de cliente para várias operações. Pode utilizar em segurança o [QueueClient] [ QueueClient] objeto para envio de mensagens de operações assíncronas em simultâneo e de vários threads. 

## <a name="concurrent-operations"></a>Operações simultâneas

Efetuar uma operação (enviar, receber, eliminar, etc.) demora algum tempo. Neste momento inclui o processamento da operação pelo serviço do Service Bus para além de latência do pedido e a resposta. Para aumentar o número de operações, por hora, operações tem de ser executado em simultâneo. 

O cliente agenda operações simultâneas efetuando operações assíncronas. O pedido seguinte é iniciado antes do pedido anterior for concluído. O fragmento de código seguinte é um exemplo de uma operação de envio assíncrono:
  
 ```csharp
  Message m1 = new BrokeredMessage(body);
  Message m2 = new BrokeredMessage(body);
  
  Task send1 = queueClient.SendAsync(m1).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #1");
    });
  Task send2 = queueClient.SendAsync(m2).ContinueWith((t) => 
    {
      Console.WriteLine("Sent message #2");
    });
  Task.WaitAll(send1, send2);
  Console.WriteLine("All messages sent");
  ```
  
  O código seguinte é um exemplo de um assíncrona a operação de receção. Consulte o programa completo [aqui](https://github.com/Azure/azure-service-bus/blob/master/samples/DotNet/Microsoft.Azure.ServiceBus/SendersReceiversWithQueues):
  
  ```csharp
  var receiver = new MessageReceiver(connectionString, queueName, ReceiveMode.PeekLock);
  var doneReceiving = new TaskCompletionSource<bool>();

  receiver.RegisterMessageHandler(
  ```

## <a name="receive-mode"></a>Receber modo

Ao criar um cliente de fila ou a subscrição, pode especificar um modo de receção: *bloqueio observar* ou *receber e eliminar*. Modo de receber a predefinição é [PeekLock][PeekLock]. Durante o funcionamento neste modo, o cliente envia um pedido para receber uma mensagem a partir do Service Bus. Depois do cliente recebeu a mensagem, envia um pedido para concluir a mensagem.

Ao definir o modo de receção [ReceiveAndDelete][ReceiveAndDelete], os dois passos são combinados num único pedido. Estes passos reduzir o número global de operações e melhorar o débito global da mensagem. Este ganhos de desempenho é fornecido at the risk of perder mensagens.

Barramento de serviço não suporta transações para receber e-eliminação de operações. Além disso, é necessária para qualquer cenários em que o cliente pretende diferir semântica de bloqueio peek ou [entregues](service-bus-dead-letter-queues.md) uma mensagem.

## <a name="client-side-batching"></a>A criação de batches de lado do cliente

A criação de batches de lado do cliente permite que um cliente de fila ou um tópico atrasar o envio de uma mensagem para um determinado período de tempo. Se o cliente envia mensagens adicionais durante este período de tempo, transmite as mensagens num batch único. A criação de batches de lado do cliente também faz com que um cliente de fila ou a subscrição para vários do batch **concluída** pedidos num único pedido. Só está disponível para criação de batches assíncrona **enviar** e **concluída** operações. Operações síncronas são imediatamente enviadas para o serviço do Service Bus. Criação de batches não ocorrer para peek ou receber operações nem criação de batches ocorre em clientes.

Por predefinição, um cliente utiliza um intervalo de lote de 20 ms. Pode alterar o intervalo de lote, definindo o [BatchFlushInterval] [ BatchFlushInterval] propriedade antes de criar a fábrica de mensagens. Esta definição afeta todos os clientes que são criados por esta fábrica.

Para desativar a criação de batches, defina o [BatchFlushInterval] [ BatchFlushInterval] propriedade **TimeSpan**. Por exemplo:

```csharp
MessagingFactorySettings mfs = new MessagingFactorySettings();
mfs.TokenProvider = tokenProvider;
mfs.NetMessagingTransportSettings.BatchFlushInterval = TimeSpan.FromSeconds(0.05);
MessagingFactory messagingFactory = MessagingFactory.Create(namespaceUri, mfs);
```

Criação de batches não afeta o número de operações de mensagens sujeito a faturação e só está disponível para o Service Bus cliente protocolo a utilizar o [Microsoft.ServiceBus.Messaging](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) biblioteca. O protocolo HTTP não suporta a criação de batches.

## <a name="batching-store-access"></a>Acesso à loja de criação de batches

Para aumentar o débito de uma fila, tópico ou uma subscrição, o Service Bus lotes várias mensagens quando escreve no respectivo armazenamento interno. Se estiver ativada numa fila ou um tópico, escrever mensagens para o arquivo irá ser em lotes. Se estiver ativada num fila ou subscrição, eliminar as mensagens a partir da loja será possível criar batch. Se o acesso à loja de batches é ativado para uma entidade, o Service Bus atrasa uma operação de escrita de arquivo sobre essa entidade por até 20 ms. 

> [!NOTE]
> Não há nenhum risco de perda de mensagens com a criação de batches, mesmo se existir uma falha de Service Bus no final de um intervalo de lotes 20ms. 

Operações de arquivo adicionais que ocorrem durante este intervalo são adicionadas ao batch. Em lotes afeta apenas o arquivo de acesso **enviar** e **concluída** operações; receber operações não são afetadas. Acesso à loja de batches é uma propriedade numa entidade. Criação de batches ocorre em todas as entidades que permitem acesso à loja de batches.

Ao criar uma nova fila, tópico ou uma subscrição, o acesso à loja de batches está ativado por predefinição. Para desativar o acesso à loja de batches, defina o [EnableBatchedOperations] [ EnableBatchedOperations] propriedade **falso** antes de criar a entidade. Por exemplo:

```csharp
QueueDescription qd = new QueueDescription();
qd.EnableBatchedOperations = false;
Queue q = namespaceManager.CreateQueue(qd);
```

Acesso à loja de batches não afeta o número de operações de mensagens sujeito a faturação e é uma propriedade de uma fila, tópico ou uma subscrição. É independente do modo de receção e o protocolo que é utilizado entre um cliente e o serviço do Service Bus.

## <a name="prefetching"></a>Prefetching

[Prefetching](service-bus-prefetch.md) permite que o cliente de fila ou a subscrição a carregar mensagens adicionais do serviço quando executa uma operação de receção. O cliente armazena estas mensagens numa cache local. O tamanho da cache é determinado pelo [QueueClient.PrefetchCount] [ QueueClient.PrefetchCount] ou [SubscriptionClient.PrefetchCount] [ SubscriptionClient.PrefetchCount] propriedades. Cada cliente que permitem prefetching mantém a sua própria cache. Uma cache não é partilhada entre os clientes. Se o cliente inicia uma operação de receção e respetiva cache está vazia, o serviço transmite um lote de mensagens. O tamanho do lote é igual ao tamanho da cache ou 256 KB, que for mais pequeno. Se o cliente inicia uma operação de receção e a cache contém uma mensagem, a mensagem é retirada da cache.

Quando uma mensagem é prefetched, o serviço bloqueia a mensagem prefetched. Com o bloqueio não é possível receber a mensagem de prefetched por um recetor diferentes. Se o recetor não consegue concluir a mensagem antes do bloqueio expira, a mensagem fica disponível para outros recetores. A cópia da mensagem prefetched permanece na cache. O recetor que consome a cópia em cache expirada irá receber uma exceção ao tentar concluir essa mensagem. Por predefinição, o bloqueio de mensagem expira após 60 segundos. Este valor pode ser expandido para 5 minutos. Para impedir que o consumo de mensagens expiradas, o tamanho da cache deve ser sempre menor do que o número de mensagens que pode ser utilizada por um cliente no intervalo de tempo limite de bloqueio.

Quando utilizar a expiração do bloqueio de predefinição de 60 segundos, o valor é uma boa para [PrefetchCount] [ SubscriptionClient.PrefetchCount] é o máximo de 20 vezes taxas de processamento de todos os recetores do factory. Por exemplo, uma fábrica cria três recetores e cada recetor pode processar até 10 mensagens por segundo. A contagem de obtenção prévia não deve exceder 20 X 3, 10 = 600. Por predefinição, [PrefetchCount] [ QueueClient.PrefetchCount] está definido como 0, o que significa que não existem mensagens adicionais são obtidas do serviço.

Prefetching mensagens aumenta o débito global para uma fila ou a subscrição porque esta reduz o número global de operações de mensagem ou ida e volta. Obter a primeira mensagem, no entanto, irá demorar mais tempo (porque o tamanho da mensagem aumentada). Receber mensagens prefetched irá ser mais rápida porque estas mensagens já foram transferidas pelo cliente.

A propriedade de (TTL) time-to-live de uma mensagem está marcada pelo servidor ao tempo que o servidor envia a mensagem para o cliente. O cliente não verificar a propriedade TTL da mensagem quando a mensagem é recebida. Em vez disso, pode receber a mensagem, mesmo se o TTL da mensagem foi efectuada com êxito enquanto a mensagem foi colocado em cache pelo cliente.

Prefetching não afeta o número de operações de mensagens sujeito a faturação e só está disponível para o protocolo de cliente do Service Bus. O protocolo HTTP não suporta prefetching. Prefetching está disponível para operações síncronas e assíncronas recebem operações.

## <a name="express-queues-and-topics"></a>Express filas e tópicos

Entidades expressas ativar cenários de latência reduzidos e débito elevado, são suportadas e apenas na camada padrão do serviço de mensagens. Entidades criadas numa [espaços de nomes Premium](service-bus-premium-messaging.md) não suportam a opção rápida. Com entidades expressas, se é enviada uma mensagem para uma fila ou um tópico, a mensagem não será imediatamente armazenada no arquivo de mensagens. Em vez disso, este é colocado em cache na memória. Se uma mensagem permanece na fila de espera durante mais do que alguns segundos, é automaticamente escrito para armazenamento, deste modo, a proteger contra a perda de devido a uma falha de estável. Escrever a mensagem para uma cache de memória aumenta o débito e reduz a latência porque não há sem acesso a estável armazenamento momento que a mensagem é enviada. As mensagens que são consumidas dentro de alguns segundos não são escritas para o arquivo de mensagens. O exemplo seguinte cria um tópico rápido.

```csharp
TopicDescription td = new TopicDescription(TopicName);
td.EnableExpress = true;
namespaceManager.CreateTopic(td);
```

Se é enviada uma mensagem que contém informações críticas que não devem ser perdidas a uma entidade express, o remetente pode forçar o Service Bus para manter imediatamente a mensagem a estável armazenamento definindo a [ForcePersistence] [ ForcePersistence] propriedade **verdadeiro**.

> [!NOTE]
> Entidades expressas não suporta transações.

## <a name="partitioned-queues-or-topics"></a>Filas particionadas ou tópicos

Internamente, o Service Bus utiliza o mesmo nó e mensagens do arquivo para processar e armazenar todas as mensagens de uma entidade de mensagens (fila ou um tópico). A [particionada fila ou um tópico](service-bus-partitioning.md), por outro lado, é distribuído por vários nós e arquivos de mensagens. Particionada filas e tópicos não só produzem um débito mais elevado do que o regulares filas e tópicos, estes também apresentem um disponibilidade superior. Para criar uma entidade particionada, defina o [EnablePartitioning] [ EnablePartitioning] propriedade **verdadeiro**, conforme mostrado no exemplo seguinte. Para obter mais informações sobre entidades particionadas, consulte [entidades de mensagens Particionadas][Partitioned messaging entities].

> [!NOTE]
> Entidades particionadas não são suportadas no [Premium SKU](service-bus-premium-messaging.md). 

```csharp
// Create partitioned queue.
QueueDescription qd = new QueueDescription(QueueName);
qd.EnablePartitioning = true;
namespaceManager.CreateQueue(qd);
```

## <a name="multiple-queues"></a>Várias filas

Se não é possível utilizar uma fila particionada ou um tópico ou à carga esperada não pode ser processada por um único fila particionada ou um tópico, tem de utilizar várias entidades de mensagens. Ao utilizar várias entidades, crie um cliente dedicado para cada entidade, em vez de utilizar o mesmo cliente para todas as entidades.

## <a name="development-and-testing-features"></a>Desenvolvimento e teste de funcionalidades

Barramento de serviço possui uma funcionalidade, utilizada especificamente para o desenvolvimento, que **nunca deve ser utilizado em configurações de produção**: [TopicDescription.EnableFilteringMessagesBeforePublishing][].

Quando novas regras ou filtros são adicionados para o tópico, pode utilizar [TopicDescription.EnableFilteringMessagesBeforePublishing][] para verificar se a nova expressão de filtro está a funcionar conforme esperado.

## <a name="scenarios"></a>Cenários

As secções seguintes descrevem cenários típicos de mensagens e descrevem as definições preferenciais do Service Bus. Taxas de débito são classificadas como pequenos (menos 1 mensagem por segundo), moderada (1 mensagem por segundo ou superior, mas inferior a 100 mensagens por segundo) e a elevada (100 mensagens/segundo ou superior). O número de clientes que está classificado como pequena (5 ou menos), moderada (mais de 5 mas inferior ou igual a 20) e grande (mais do que 20).

### <a name="high-throughput-queue"></a>Fila de débito elevado

Objetivo: Maximize o débito de uma fila única. O número de remetentes e os recetores é pequeno.

* Para aumentar a velocidade global de envio na fila, utilize várias fábricas de mensagens para criar os remetentes. Para cada remetente, utilize operações assíncronas ou de vários threads.
* Para aumentar a taxa de receção geral da fila, utilize várias fábricas de mensagens para criar recetores.
* Utilize operações assíncronas para tirar partido da criação de batches do lado do cliente.
* Defina o intervalo de lotes para 50 ms para reduzir o número de transmissões de protocolo de cliente do Service Bus. Se forem utilizados várias os remetentes, aumente o intervalo de lotes para 100 ms.
* Deixe o acesso de arquivo de batches ativado. Este acesso aumenta a velocidade global em que as mensagens podem ser escritas na fila.
* Defina a contagem de obtenção prévia para 20 vezes as máximo as taxas de processamento de todos os recetores de uma fábrica. Esta contagem reduz o número das transmissões de protocolo de cliente do Service Bus.
* Utilize uma fila de mensagens particionada para um melhor desempenho e disponibilidade.

### <a name="multiple-high-throughput-queues"></a>Várias filas de débito elevado

Objetivo: Maximize o débito global de várias filas. O débito de uma fila individuais é moderada ou elevada.

Para obter o débito máximo por várias filas, utilize as definições descritas para maximizar o débito de uma fila única. Além disso, utilize fábricas de diferentes para criar os clientes que enviar ou recebem as filas diferentes.

### <a name="low-latency-queue"></a>Fila de latência baixa

Objetivo: Minimize a latência de ponto a ponto de uma fila ou tópico. O número de remetentes e os recetores é pequeno. O débito da fila é pequeno ou moderada.

* Desative a criação de batches do lado do cliente. O cliente envia imediatamente uma mensagem.
* Desative o acesso à loja de batches. O serviço escreve imediatamente a mensagem para o arquivo.
* Se utilizar um único cliente, defina a contagem de obtenção prévia para 20 vezes a taxa de processamento do recetor. Se várias mensagens chegam a fila ao mesmo tempo, o protocolo de cliente do Service Bus transmite-os todos ao mesmo tempo. Quando o cliente recebe a mensagem seguinte, essa mensagem já se encontra na local cache. A cache deve ser pequena.
* Se utilizar vários clientes, defina a contagem de obtenção prévia como 0. Ao definir o número, o segundo cliente pode receber a segunda mensagem enquanto o primeiro cliente ainda está a processar a primeira mensagem.
* Utilize uma fila de mensagens particionada para um melhor desempenho e disponibilidade.

### <a name="queue-with-a-large-number-of-senders"></a>Fila com um grande número de remetentes de

Objetivo: Maximize o débito de uma fila ou um tópico com um grande número de remetentes. Cada remetente envia mensagens com uma velocidade de moderada. O número de recetores é pequeno.

O Service Bus permite até 1000 ligações simultâneas para uma entidade de mensagens (ou 5000 através de AMQP). Este limite é aplicado ao nível do espaço de nomes, e tópicos/filas/subscrições são limitadas pelo limite de ligações em simultâneo por espaço de nomes. Para as filas, este número é partilhado entre os remetentes e os recetores. Se a todas as ligações de 1000 são necessárias para os remetentes, substitua a fila de um tópico e uma única subscrição. Um tópico aceita até 1000 ligações simultâneas de remetentes, enquanto a subscrição aceita um ligações simultâneas de 1000 adicionais de recetores. Se forem necessários mais de 1000 remetentes em simultâneo, os remetentes devem enviar mensagens para o protocolo de Service Bus através de HTTP.

Para maximizar o débito, execute os seguintes passos:

* Se o remetente de cada um reside num processo diferente, utilize apenas uma único fábrica por processo.
* Utilize operações assíncronas para tirar partido da criação de batches do lado do cliente.
* Utilize a predefinição de criação de batches de intervalo de 20 ms para reduzir o número das transmissões de protocolo de cliente do Service Bus.
* Deixe o acesso de arquivo de batches ativado. Este acesso aumenta a velocidade global em que as mensagens podem ser escritas para a fila ou tópico.
* Defina a contagem de obtenção prévia para 20 vezes as máximo as taxas de processamento de todos os recetores de uma fábrica. Esta contagem reduz o número das transmissões de protocolo de cliente do Service Bus.
* Utilize uma fila de mensagens particionada para um melhor desempenho e disponibilidade.

### <a name="queue-with-a-large-number-of-receivers"></a>Fila com um grande número de recetores

Objetivo: Maximize a taxa de receção de uma fila ou a subscrição com um grande número de recetores. Cada recetor recebe mensagens de uma taxa moderada. O número de remetentes é pequeno.

O Service Bus permite até 1000 ligações simultâneas a uma entidade. Se uma fila precisar de mais de 1000 recetores, substitua a fila de um tópico e várias subscrições. Cada subscrição pode suportar até 1000 de ligações em simultâneo. Em alternativa, os recetores podem aceder a fila através do protocolo HTTP.

Para maximizar o débito, efetue o seguinte:

* Se cada recetor reside num processo diferente, utilize apenas uma único fábrica por processo.
* Recetores podem utilizar operações síncronas ou assíncronas. Tendo em conta a taxa de receção moderado de um recetor individuais, do lado do cliente de lotes de um pedido de conclusão não afetam o débito de recetor.
* Deixe o acesso de arquivo de batches ativado. Este acesso reduz a carga geral da entidade. Também reduz a taxa de geral em que as mensagens podem ser escritas para a fila ou tópico.
* Defina a contagem de obtenção prévia para um valor pequeno (por exemplo, PrefetchCount = 10). Esta contagem impede recetores de inatividade enquanto outros recetores têm de grandes quantidades de mensagens colocadas em cache.
* Utilize uma fila de mensagens particionada para um melhor desempenho e disponibilidade.

### <a name="topic-with-a-small-number-of-subscriptions"></a>Tópico com um pequeno número de subscrições

Objetivo: Maximize o débito de um tópico com um pequeno número de subscrições. Uma mensagem é recebida por várias subscrições, que significa que a taxa de receção combinado ao longo de todas as subscrições é maior do que a taxa de envio. O número de remetentes é pequeno. O número de recetores por subscrição é pequeno.

Para maximizar o débito, efetue o seguinte:

* Para aumentar a velocidade global de envio para o tópico, utilize várias fábricas de mensagens para criar os remetentes. Para cada remetente, utilize operações assíncronas ou de vários threads.
* Para aumentar a taxa de receção geral de uma subscrição, utilize várias fábricas de mensagens para criar recetores. Para cada recetor, utilize operações assíncronas ou de vários threads.
* Utilize operações assíncronas para tirar partido da criação de batches do lado do cliente.
* Utilize a predefinição de criação de batches de intervalo de 20 ms para reduzir o número das transmissões de protocolo de cliente do Service Bus.
* Deixe o acesso de arquivo de batches ativado. Este acesso aumenta a velocidade global em que as mensagens podem ser escritas para o tópico.
* Defina a contagem de obtenção prévia para 20 vezes as máximo as taxas de processamento de todos os recetores de uma fábrica. Esta contagem reduz o número das transmissões de protocolo de cliente do Service Bus.
* Utilize um tópico particionado para um melhor desempenho e disponibilidade.

### <a name="topic-with-a-large-number-of-subscriptions"></a>Tópico com um grande número de subscrições

Objetivo: Maximize o débito de um tópico com um grande número de subscrições. Uma mensagem é recebida por várias subscrições, o que significa que a taxa de receção combinado ao longo de todas as subscrições é muito maior do que a velocidade de envio. O número de remetentes é pequeno. O número de recetores por subscrição é pequeno.

Tópicos com um grande número de subscrições expõem normalmente um débito global baixo se todas as mensagens são encaminhadas para todas as subscrições. Este débito baixo é causado pelo facto da que cada mensagem é recebida demasiadas vezes e todas as mensagens que estão contidas num tópico e todas as respetivas subscrições são armazenadas no arquivo do mesmo. Presume-se que o número de remetentes e número de recetores por subscrição são pequenos. Service Bus suporta até 2000 subscrições por tópico.

Para maximizar o débito, tente os seguintes passos:

* Utilize operações assíncronas para tirar partido da criação de batches do lado do cliente.
* Utilize a predefinição de criação de batches de intervalo de 20 ms para reduzir o número das transmissões de protocolo de cliente do Service Bus.
* Deixe o acesso de arquivo de batches ativado. Este acesso aumenta a velocidade global em que as mensagens podem ser escritas para o tópico.
* Defina a contagem de obtenção prévia 20 vezes a receção esperado taxa em segundos. Esta contagem reduz o número das transmissões de protocolo de cliente do Service Bus.
* Utilize um tópico particionado para um melhor desempenho e disponibilidade.

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações sobre como otimizar o desempenho do Service Bus, consulte [entidades de mensagens Particionadas][Partitioned messaging entities].

[QueueClient]: /dotnet/api/microsoft.azure.servicebus.queueclient
[MessageSender]: /dotnet/api/microsoft.azure.servicebus.core.messagesender
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[PeekLock]: /dotnet/api/microsoft.azure.servicebus.receivemode
[ReceiveAndDelete]: /dotnet/api/microsoft.azure.servicebus.receivemode
[BatchFlushInterval]: /dotnet/api/microsoft.servicebus.messaging.messagesender.batchflushinterval
[EnableBatchedOperations]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablebatchedoperations
[QueueClient.PrefetchCount]: /dotnet/api/microsoft.azure.servicebus.queueclient.prefetchcount
[SubscriptionClient.PrefetchCount]: /dotnet/api/microsoft.azure.servicebus.subscriptionclient.prefetchcount
[ForcePersistence]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage.forcepersistence
[EnablePartitioning]: /dotnet/api/microsoft.servicebus.messaging.queuedescription.enablepartitioning
[Partitioned messaging entities]: service-bus-partitioning.md
[TopicDescription.EnableFilteringMessagesBeforePublishing]: /dotnet/api/microsoft.servicebus.messaging.topicdescription.enablefilteringmessagesbeforepublishing
