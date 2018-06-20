---
title: Descrição geral de mensagens de filas, tópicos e subscrições do Azure Service Bus | Microsoft Docs
description: Descrição geral de entidades de mensagens do Service Bus.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 06/18/2018
ms.author: sethm
ms.openlocfilehash: 424004a2a39bd0d05bce515dc17685e60f7a0c9b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231581"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Filas, tópicos e subscrições do Service Bus

Microsoft Azure Service Bus suporta um conjunto de tecnologias de middleware baseado na nuvem, orientado para a mensagem, incluindo a colocação de mensagens fiável e durável publicação de mensagens. Estas capacidades de mensagens "mediadas" podem considerar como desassociada funcionalidades de mensagens que o suporte de publicação-subscrição, desacoplamento temporal e cenários utilizando a carga de trabalho mensagens do Service Bus de balanceamento de carga. A comunicação desacoplada tem muitas vantagens; por exemplo, os clientes e os servidores podem ligar-se conforme necessário e efetuar as operações de forma assíncrona.

As entidades de mensagens que formam a core das capacidades de mensagens no Service Bus são filas, tópicos e subscrições e regras/ações.

## <a name="queues"></a>Filas

As filas oferecem *First In, Out primeiro* entrega de mensagens (FIFO) para um ou mais consumidores concorrentes. Ou seja, recetores normalmente recebem e processam mensagens pela ordem em que foram adicionadas à fila, e apenas um consumidor de mensagens recebe e processa cada mensagem. Uma vantagem de utilizar as filas é obter "desacoplamento temporal" dos componentes da aplicação. Por outras palavras, os produtores (remetentes) e os consumidores (recetores) não dispõe de enviar e receber mensagens ao mesmo tempo, porque as mensagens são armazenadas de maneira duradoura na fila. Além disso, o produtor não tem de aguardar uma resposta do consumidor para poder continuar a processar e enviar mensagens.

Uma vantagem relacionada é "nivelamento de carga," que permite aos produtores e consumidores enviar e receber mensagens a taxas diferentes. Em muitas aplicações, a carga de sistema varia ao longo do tempo; No entanto, o tempo de processamento necessário para cada unidade de trabalho é geralmente constante. A intermediação de mensagem de produtores e consumidores através de uma fila significa que a aplicação de consumo só tem de ser aprovisionada para conseguir processar a carga média em vez de pico de carga. A profundidade da fila aumenta e contrai à medida que a carga a receber varia. Esta capacidade poupa diretamente dinheiro em relação a quantidade de infraestrutura necessária para a carga de aplicação de serviço. À medida que a carga aumenta, mais processos de trabalho podem ser adicionados a leitura da fila. Cada mensagem é processada apenas por um dos processos de trabalho. Além disso, este balanceamento de carga baseado na solicitação permite que uma utilização ideal dos computadores de trabalho, mesmo se os computadores de trabalho diferirem em relação a potência de processamento, como estas mensagens de extração na sua própria taxa máxima. Este padrão é frequentemente denominado padrão o padrão "competir consumidor".

Utilizar filas para intermédio entre mensagem produtores e consumidores fornece um coupling soltas inerente entre os componentes. Porque os produtores e consumidores não têm conhecimento entre si, um consumidor possa ser atualizado sem ter qualquer efeito no produtor.

### <a name="create-queues"></a>Criar filas

Criar filas utilizando o [portal do Azure](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md), ou [modelos do Resource Manager](service-bus-resource-manager-namespace-queue.md). Em seguida, enviar e receber mensagens utilizando um [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) objeto. 

Para obter rapidamente como criar uma fila, em seguida, enviar e receber mensagens de e para a fila, consulte o [inícios Rápidos](service-bus-quickstart-portal.md) para cada método. Para consultar um tutorial mais aprofundado sobre como utilizar as filas de mensagens em fila, consulte [introdução às filas do Service Bus](service-bus-dotnet-get-started-with-queues.md). 

Para um exemplo de trabalho, consulte o [BasicSendReceiveUsingQueueClient exemplo](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) no GitHub.

### <a name="receive-modes"></a>Receber modos

Pode especificar dois modos diferentes em que o Service Bus recebe mensagens: *ReceiveAndDelete* ou *PeekLock*. No [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modo, a operação de receção única; ou seja, quando o Service Bus recebe o pedido, marca a mensagem como consumida e devolve a mesma à aplicação. **ReceiveAndDelete** modo é o modelo mais simples e funciona melhor para cenários em que a aplicação pode tolerar o não processamento de uma mensagem se ocorrer uma falha. Para compreender este cenário, considere um cenário em que o consumidor emite o pedido de receção e, em seguida, falhas antes do processamento-lo. Porque o Service Bus marca a mensagem como consumida, quando a aplicação reinicia e começa a consumir novamente mensagens, terá perdido a mensagem consumida antes da falha.

No [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modo, a operação de receção torna-se duas etapas que possibilita o suporte de aplicações que não toleram mensagens em falta. Quando o Service Bus recebe o pedido, localiza a mensagem seguinte a ser consumida, bloqueia-a para impedir a receção outros consumidores e, em seguida, devolve a mesma à aplicação. Após a aplicação concluir o processamento da mensagem (ou armazena a mesma forma fiável para processamento futuro), conclui a segunda etapa do processo de receção ao chamar [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) à mensagem recebida. Quando o Service Bus vê a **CompleteAsync** chamada, marca a mensagem como consumida.

Se a aplicação não é possível processar a mensagem por algum motivo, pode chamar o [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) método à mensagem recebida (em vez de [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync)). Este método permite que o Service Bus desbloqueie a mensagem e disponibilizá-lo ser novamente recebida, através do consumidor mesmo ou outro consumidor concorrente. Lugar, existe um tempo limite associado o bloqueio e se a aplicação não conseguir processar a mensagem antes do tempo limite de bloqueio expira (por exemplo, no caso de falha da aplicação), Service Bus desbloqueia a mensagem e torna disponível para ser recebida novamente ( essencialmente efetuar um [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) operação por predefinição).

No caso de falha da aplicação após o processamento da mensagem, mas antes o **CompleteAsync** pedido é emitido, a mensagem é reenviada para a aplicação quando esta reiniciar. Este processo é frequentemente designado *, pelo menos, uma vez* processamento; ou seja, cada mensagem é processada pelo menos uma vez. No entanto, em determinadas situações a mesma mensagem poderá ser reenviada. Se o cenário não pode tolerar o processamento duplicado, em seguida, é necessário lógica adicional na aplicação para detetar duplicados, que pode ser conseguido com base no [MessageId](/dotnet/api/microsoft.azure.servicebus.message.messageid) propriedade da mensagem, que permanece constante em tentativas de entrega. Esta funcionalidade é conhecida como *exatamente uma vez* processamento.

## <a name="topics-and-subscriptions"></a>Tópicos e subscrições

Contrariamente filas, em que cada mensagem é processada por um único consumidor, *tópicos* e *subscrições* proporcionar uma forma de um-para-muitos de comunicação, um *publicar/subscrever* padrão. Útil para dimensionamento para grandes quantidades de destinatários, cada mensagem publicada é disponibilizada para cada subscrição registada juntamente com o tópico. As mensagens enviadas para um tópico e entregar uma ou mais subscrições associadas, consoante as regras de filtro que podem ser definidas numa base por subscrição. As subscrições podem utilizar filtros adicionais para restringir as mensagens que pretende receber. As mensagens são enviadas para um tópico da mesma forma são enviadas para uma fila, mas mensagens não são recebidas do tópico diretamente. Em vez disso, são recebidos de subscrições. Uma subscrição de tópico é semelhante uma fila virtual que recebe cópias das mensagens que são enviadas para o tópico. As mensagens são recebidas a partir uma subscrição de forma idêntica ao modo como o que são recebidos a partir de uma fila.

A título de comparação, a funcionalidade de uma fila de envio de mensagem mapeia diretamente para um tópico e a funcionalidade de receção de mensagens mapeia para uma subscrição. Entre outras coisas, esta funcionalidade significa que as subscrições suportam os padrões mesmos descritos anteriormente nesta secção em relação a filas: consumidor concorrente, desacoplamento temporal, nivelamento de carga e balanceamento de carga.

### <a name="create-topics-and-subscriptions"></a>Criar tópicos e subscrições

Criar um tópico é semelhante ao criar uma fila, conforme descrito na secção anterior. Em seguida, enviar mensagens utilizando o [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient) classe. Para receber mensagens, crie uma ou mais subscrições para o tópico. Semelhantes a filas, as mensagens são recebidas a partir de uma subscrição a utilizar um [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) objeto em vez de um [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) objeto. Crie a subscrição do cliente, transmitir o nome do tópico, o nome da subscrição e (opcionalmente) o modo de receção como parâmetros. 

Para um completo funcionar exemplo, consulte o [BasicSendReceiveUsingTopicSubscriptionClient exemplo](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) no Github.

### <a name="rules-and-actions"></a>Regras e ações

Em vários cenários, as mensagens com características específicas tem de ser processadas de formas diferentes. Para ativar este processamento, pode configurar subscrições para localizar mensagens que tenham pretendido propriedades e, em seguida, efetuar determinadas modificações para essas propriedades. Enquanto as subscrições do Service Bus ver todas as mensagens enviadas para o tópico, só poderá copiar um subconjunto dessas mensagens para a fila virtual da subscrição. Esta filtragem é efetuada utilizando filtros de subscrição. Essas alterações são denominadas *filtrar ações*. Quando é criada uma subscrição, pode fornecer uma expressão de filtro que opera nas propriedades da mensagem, ambas as propriedades do sistema (por exemplo, **etiqueta**) e as propriedades de aplicação personalizada (por exemplo, **StoreName**.) A expressão de filtro do SQL Server é opcional neste caso; sem uma expressão de filtro do SQL Server, será efetuada qualquer ação do filtro definida em subscrições em todas as mensagens para essa subscrição.

Para um completo funcionar exemplo, consulte o [TopicSubscriptionWithRuleOperationsSample exemplo](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) no GitHub.

Para obter mais informações sobre os valores de filtro possíveis, consulte a documentação para o [SqlFilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) e [SqlRuleAction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) classes. 

## <a name="next-steps"></a>Passos Seguintes

Para obter mais informações e exemplos de como utilizar mensagens do Service Bus, consulte o seguinte avançadas tópicos:

* [Descrição geral das mensagens do Service Bus](service-bus-messaging-overview.md)
* [Início rápido: Enviar e receber mensagens utilizando o portal do Azure e o .NET](service-bus-quickstart-portal.md)
* [Tutorial: Atualizar inventário utilizando o portal do Azure e tópicos/subscrições](service-bus-tutorial-topics-subscriptions-portal.md)


