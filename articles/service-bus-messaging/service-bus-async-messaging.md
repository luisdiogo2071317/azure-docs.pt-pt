---
title: Mensagens assíncronas do Service Bus | Documentos da Microsoft
description: Descrição de mensagens assíncronas do Azure Service Bus.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 58ce1e4bd7ffee642c062fde47fd5aff7fa9da0b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55168827"
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Padrões de mensagens assíncronas e elevada disponibilidade

Mensagens assíncronas podem ser implementadas numa variedade de formas diferentes. Com filas, tópicos e subscrições, o Azure Service Bus suporta asynchronism através de um arquivo e o mecanismo de encaminhamento. Na operação de normal (síncrona), enviar mensagens para filas e tópicos e receber mensagens de filas e subscrições. Aplicativos que escreve dependem estas entidades sempre estejam disponíveis. Quando o estado de funcionamento da entidade é alterado, devido a uma variedade de circunstâncias, terá de uma maneira de fornecer uma entidade de capacidade reduzida que pode satisfazer a maioria das necessidades.

Aplicativos normalmente usam padrões de mensagens assíncronas para permitir que um número de cenários de comunicação. Pode criar aplicações em que os clientes podem enviar mensagens a serviços, mesmo quando o serviço não está em execução. Para aplicativos essa experiência rajadas de comunicações, uma fila podem ajudar a redistribuição de carga, fornecendo um lugar para comunicações de memória intermédia. Por fim, pode obter um balanceador de carga simples, mas eficaz para distribuir as mensagens em várias máquinas.

Para manter a disponibilidade de qualquer uma destas entidades, considere uma série de diferentes formas em que estas entidades podem aparecer indisponíveis para um sistema de mensagens durável. Em termos gerais, vemos a entidade se tornar disponível para aplicativos que escrevemos das seguintes formas diferentes:

* Não é possível enviar mensagens.
* Não é possível receber mensagens.
* Não é possível gerir as entidades (criar, obter, atualizar ou eliminar entidades).
* Não é possível contactar o serviço.

Para cada uma destas falhas, modos de falha diferentes existem que permitem que uma aplicação continuar a realizar o trabalho em algum nível de funcionalidade reduzida. Por exemplo, um sistema que pode enviar mensagens, mas não recebê-las pode receber pedidos de clientes, mas não é possível processar esses pedidos. Este tópico descreve problemas potenciais que podem ocorrer e como esses problemas são atenuados. Do Service Bus tem introduziu uma série de atenuações que deve optar, e este tópico também explica as regras que regem a utilização dessas atenuações participar.

## <a name="reliability-in-service-bus"></a>Confiabilidade no Service Bus
Existem várias formas de lidar com problemas de mensagem e das entidades e existem diretrizes que regem a utilização apropriada dessas atenuações. Para compreender as diretrizes, deve primeiro compreender o que pode falhar no Service Bus. Devido ao design de sistemas do Azure, todos esses problemas tendem a ser curta duração. Num alto nível, as causas diferentes de indisponibilidade aparecem da seguinte maneira:

* Limitação de um sistema externo de que depende do Service Bus. Limitação ocorre de interações com os recursos de computação e armazenamento.
* Problema para um sistema de que depende do Service Bus. Por exemplo, uma parte determinada de armazenamento pode sofrer problemas.
* Falha do Service Bus no subsistema único. Nesta situação, um nó de computação pode entrar num estado inconsistente e tem de reiniciar em si, fazendo com que todas as entidades que ele serve para equilibrar a outros nós. Isso por sua vez, pode causar um curto período de tempo de processamento de mensagens lenta.
* Falha do Service Bus num datacenter do Azure. Esta é uma "Falha catastrófica" durante o qual o sistema estiver inacessível por número de minutos ou algumas horas.

> [!NOTE]
> O termo **armazenamento** pode significar o armazenamento do Azure e SQL Azure.
> 
> 

Barramento de serviço contém um número de atenuações para esses problemas. As secções seguintes abordam cada problema e seus respectivas atenuações.

### <a name="throttling"></a>Limitação
Com o Service Bus, a limitação permite a gestão de taxa de mensagem cooperativo. Cada nó do Service Bus individual hospeda muitas entidades. Cada uma dessas entidades exige o sistema em termos de CPU, memória, armazenamento e outras facetas. Quando qualquer um dessas facetas Deteta utilização exceder os limites definidos, do Service Bus pode negar uma determinada solicitação. O autor da chamada recebe uma [ServerBusyException] [ ServerBusyException] e repetições após 10 segundos.

Facilitar a, o código tem do erro de leitura e de parar qualquer repetições da mensagem por, pelo menos, 10 segundos. Uma vez que o erro pode acontecer em partes do aplicativo cliente, espera-se que cada parte independentemente executa a lógica de repetição. O código pode reduzir a probabilidade de limitação, permitindo a criação de partições numa fila ou tópico.

### <a name="issue-for-an-azure-dependency"></a>Problema de uma dependência do Azure
Outros componentes no Azure, ocasionalmente, podem ter problemas de serviço. Por exemplo, quando um sistema que utiliza o Service Bus está a ser atualizado, esse sistema temporariamente pode conhecer recursos reduzidos. Para contornar esses tipos de problemas, o Service Bus regularmente investiga e implementa atenuações. Efeitos colaterais dessas atenuações aparecer. Por exemplo, para lidar com problemas transitórios com o armazenamento, o Service Bus implementa um sistema que permite operações de envio de mensagem trabalhar de forma consistente. Devido à natureza da atenuação, uma mensagem enviada pode demorar até 15 minutos a aparecer na fila afetada ou subscrição e estarão prontos para uma operação de receção. Em termos gerais, a maior parte das entidades não irá ocorrer este problema. No entanto, dado o número de entidades no Service Bus no Azure, esta redução, às vezes, é necessário para um pequeno subconjunto de clientes do Service Bus.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Falha do Service Bus num subsistema único
Com qualquer aplicativo, circunstâncias podem fazer com que um componente interno do Service Bus para se tornar inconsistente. Quando o Service Bus detete isso, recolhe os dados da aplicação para ajudar a diagnosticar o que aconteceu. Quando os dados são recolhidos, o aplicativo for reiniciado numa tentativa para devolvê-lo para um estado consistente. Este processo ocorre rapidamente, e os resultados numa entidade de parecerem estar a ser indisponível para até alguns minutos, embora típicos redução são muito menores.

Nestes casos, a aplicação cliente gera um [System.TimeoutException] [ System.TimeoutException] ou [MessagingException] [ MessagingException] exceção. Barramento de serviço contém uma atenuação para este problema na forma de lógica de repetição de clientes automatizada. Depois do período de repetição se esgota, e não é possível entregar a mensagem, pode explorar através de outros recursos, como [emparelhado espaços de nomes][paired namespaces]. Espaços de nomes emparelhados têm outras limitações que são abordadas nesse artigo.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Falha do Service Bus num datacenter do Azure
A razão mais provável para uma falha no datacenter do Azure é uma falha na implementação de atualização do Service Bus ou um sistema dependente. Como a plataforma amadureceu, foi reduzida a probabilidade deste tipo de falha. Também pode ocorrer uma falha de centro de dados por motivos que incluem o seguinte:

* Falha de elétrica (fonte de alimentação e gerar power desaparecer).
* Conectividade (intervalo de internet entre a clientes e o Azure).

Em ambos os casos, um desastre natural ou afastado causou o problema. Para contornar este problema e certifique-se de que ainda pode enviar mensagens, pode usar [emparelhado espaços de nomes] [ paired namespaces] para permitir mensagens sejam enviadas para uma segunda localização, enquanto a localização primária é feita em bom estado novamente. Para obter mais informações, consulte [melhores práticas para aplicações do Service Bus interrupções e desastres de separando][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Espaços de nomes emparelhados
O [emparelhado espaços de nomes] [ paired namespaces] recurso oferece suporte a cenários em que uma entidade do Service Bus ou a implantação num Datacenter fica indisponível. Enquanto este evento ocorre com pouca frequência, ainda existem sistemas distribuídos que tem de estar preparados para lidar com cenários de casos de pior. Normalmente, este evento ocorre porque algum elemento de que depende do Service Bus está a ter um problema de curto prazo. Para manter a disponibilidade da aplicação durante um período de indisponibilidade, os utilizadores do Service Bus podem utilizar dois espaços de nomes separados, preferencialmente, nos centros de dados separado, para alojar as suas entidades de mensagens. O restante desta seção utiliza a seguinte terminologia:

* Espaço de nomes principal: O espaço de nomes com que seu aplicativo interage, para enviar e receber operações.
* Espaço de nomes secundário: O espaço de nomes que age como uma cópia de segurança para o espaço de nomes principal. Lógica do aplicativo não interage com este espaço de nomes.
* Intervalo de ativação pós-falha: A quantidade de tempo para falhas normais de aceitar antes da aplicação muda do espaço de nomes primário para o espaço de nomes secundário.

Suporte de espaços de nomes de emparelhado *enviar disponibilidade*. Envie disponibilidade preserva a capacidade de enviar mensagens. Para utilizar a disponibilidade de envio, a aplicação tem de cumprir os seguintes requisitos:

1. As mensagens são recebidas apenas do espaço de nomes primário.
2. As mensagens enviadas para uma determinada fila ou tópico poderá chegar fora de ordem.
3. Mensagens dentro de uma sessão podem chegar fora de ordem. Esta é uma quebra de funcionalidade normal de sessões. Isso significa que seu aplicativo usa sessões logicamente agrupar mensagens.
4. Estado da sessão é apenas mantido no espaço de nomes primário.
5. A fila primária pode ficar online e iniciar a aceitação das mensagens antes da fila secundária oferece todas as mensagens na fila principal.

As secções seguintes abordam as APIs, como as APIs são implementadas e mostra o código de exemplo que utilizam a funcionalidade. Tenha em atenção de que há implicações de faturação associadas a esta funcionalidade.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>A API de MessagingFactory.PairNamespaceAsync
A funcionalidade de espaços de nomes emparelhados inclui a [PairNamespaceAsync] [ PairNamespaceAsync] método no [Microsoft.ServiceBus.Messaging.MessagingFactory] [ Microsoft.ServiceBus.Messaging.MessagingFactory] classe:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Quando a tarefa estiver concluída, o emparelhamento de espaço de nomes também é concluído e pronto para agir em [MessageReceiver][MessageReceiver], [QueueClient] [ QueueClient] , ou [TopicClient] [ TopicClient] criados com o [MessagingFactory] [ MessagingFactory] instância. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] [ Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] é a classe base para os diferentes tipos de emparelhamento que estão disponíveis com um [MessagingFactory] [ MessagingFactory] objeto. Atualmente, a única classe derivada é um denominado [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], que implementa os requisitos de disponibilidade de envio. [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] tem um conjunto de construtores que completam. Olhando para o construtor com a maioria dos parâmetros, pode compreender o comportamento dos outros construtores.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Esses parâmetros têm os significados seguintes:

* *secondaryNamespaceManager*: Um inicializado [NamespaceManager] [ NamespaceManager] instância para o espaço de nomes secundário que o [PairNamespaceAsync] [ PairNamespaceAsync] método pode usar para definir Se o espaço de nomes secundário. O Gestor de espaço de nomes é utilizado para obter a lista de filas no espaço de nomes e certifique-se de que as filas de registo de segurança necessária existem. Se não existirem nessas filas, são criados. [NamespaceManager] [ NamespaceManager] requer a capacidade de criar um token com o **gerir** de afirmação.
* *messagingFactory*: O [MessagingFactory] [ MessagingFactory] instância para o espaço de nomes secundário. O [MessagingFactory] [ MessagingFactory] objeto é utilizado para enviar e, se o [EnableSyphon] [ EnableSyphon] propriedade está definida como **true**, receber mensagens das filas de registo de segurança.
* *backlogQueueCount*: O número de filas de registo de segurança para criar. Este valor tem de ser, pelo menos, 1. Ao enviar mensagens para o registo de segurança, um destas filas é escolhido aleatoriamente. Se definir o valor como 1, em seguida, apenas uma fila pode nunca ser utilizada. Quando isso acontece e a fila de uma lista de pendências gera erros, o cliente não é possível experimentar uma fila de registo de segurança diferentes e pode não conseguir enviar a mensagem. Recomendamos a definição deste valor para alguns maior valor e padrão, o valor para o 10. Pode alterar isto para um valor superior ou inferior, consoante a quantidade de dados a aplicação envia por dia. Cada fila de registo de segurança pode conter até 5 GB de mensagens.
* *failoverInterval*: A quantidade de tempo durante o qual irá aceitar falhas no espaço de nomes primário antes de alternar qualquer entidade única para o espaço de nomes secundário. As ativações pós-falha ocorrerem com base numa entidade por entidade. Entidades de um único espaço de nomes com frequência em direto em diferentes nós dentro do Service Bus. Uma falha numa entidade não implica uma falha em outro. Pode definir este valor [System.TimeSpan.Zero] [ System.TimeSpan.Zero] a ativação pós-falha para o secundário imediatamente após a falha em primeiro lugar, não transitórias. Falhas que acionam o timer de ativação pós-falha são qualquer [MessagingException] [ MessagingException] em que o [IsTransient] [ IsTransient] propriedade é false, ou um [ System.TimeoutException][System.TimeoutException]. Outras exceções, tal como [UnauthorizedAccessException] [ UnauthorizedAccessException] não dão origem a ativação pós-falha, porque indicam que o cliente está configurado incorretamente. R [ServerBusyException] [ ServerBusyException] faz não causa ativação pós-falha porque o padrão correto está a aguardar 10 segundos, em seguida, enviar a mensagem novamente.
* *enableSyphon*: Indica que esse emparelhamento específico deve também syphon mensagens do espaço de nomes secundário para o espaço de nomes principal. Em geral, os aplicativos que enviam mensagens devem definir este valor como **false**; os aplicativos que recebem as mensagens devem definir este valor como **verdadeiro**. A razão disso é que com freqüência, há menos recetores de mensagens de remetentes de mensagens. Dependendo do número de recetores, pode optar por ter uma instância de aplicação única lidar com as tarefas de syphon. Usar vários recetores tem implicações de faturas para cada fila de registo de segurança.

Para utilizar o código, crie um site primário [MessagingFactory] [ MessagingFactory] instância de uma secundária [MessagingFactory] [ MessagingFactory] de instância, um secundário [ NamespaceManager] [ NamespaceManager] instância e um [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instância. A chamada pode ser tão simples quanto o seguinte:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Quando a tarefa retornada pela [PairNamespaceAsync] [ PairNamespaceAsync] método for concluído, tudo o que é definido e estão prontos a utilizar. Antes da task é retornada, poderá não concluir todo o necessário para o emparelhamento funcionava direito de trabalho em segundo plano. Como resultado, não deve começar a enviar mensagens, até que a tarefa devolve. Se tiver ocorreram quaisquer falhas, como credenciais incorretas ou falha para criar as filas de registo de segurança, essas exceções serão geradas uma vez concluída a tarefa. Quando a tarefa retorna, certifique-se de que as filas foram encontradas ou criadas, examinando os [BacklogQueueCount] [ BacklogQueueCount] propriedade no seu [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] instância. Para o código anterior, essa operação será exibida da seguinte forma:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Passos Seguintes
Agora que aprendeu as noções básicas de mensagens assíncronas no Service Bus, leia mais detalhes sobre [emparelhado espaços de nomes][paired namespaces].

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_EnableSyphon
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_BacklogQueueCount
[paired namespaces]: service-bus-paired-namespaces.md
