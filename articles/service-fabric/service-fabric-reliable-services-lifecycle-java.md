---
title: "Descrição geral do ciclo de vida dos Reliable Services do Azure Service Fabric | Microsoft Docs"
description: "Saiba mais sobre os eventos de ciclo de vida de diferentes nos serviços de fiáveis de Service Fabric"
services: Service-Fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: Service-Fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: 93fd003ff5ba7673e5a719fb1ced0cbb97034610
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2017
---
# <a name="reliable-services-lifecycle-overview"></a>Descrição geral do ciclo de vida de serviços fiável
> [!div class="op_single_selector"]
> * [C# no Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java em Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Quando pensar sobre os ciclos de vida dos Reliable Services, as noções básicas do ciclo de vida são mais importantes. Em geral:

* Durante o arranque
  * Os serviços são construídos
  * Têm uma oportunidade para construir e devolver zero ou mais serviços de escuta
  * Os serviços de escuta devolvidos estão abertos, permitir a comunicação com o serviço
  * Método de runasync com o serviço é chamado, permitindo que o serviço à não execução longa ou em segundo plano de trabalho
* Durante o encerramento
  * O token de cancelamento transmitido a runasync com foi cancelado e os serviços de escuta estão fechados
  * Assim que estiver concluído, o objeto de serviço próprio é destructed

Existem detalhes à volta a ordenação exato destes eventos. Em particular, a ordem dos eventos podem ser alteradas ligeiramente dependendo se o serviço fiável é Stateless ou de monitorização de estado. Além disso, para os serviços com monitorização de estado, temos de lidar com o cenário de comutação primário. Durante esta sequência, a função de principal é transferida para outra réplica (ou retornado) sem encerramento do serviço. Por fim, temos de pensar em condições de erro ou falha.

## <a name="stateless-service-startup"></a>Arranque do serviço sem monitorização de estado
O ciclo de vida de um serviço sem monitorização de estado é bastante simples. Segue-se a ordem de eventos:

1. O serviço foi construído
2. Em seguida, em paralelas duas coisas ocorrer:
    - `StatelessService.createServiceInstanceListeners()`é invocada e quaisquer serviços de escuta devolvidos são abertos (`CommunicationListener.openAsync()` denomina-se cada serviço de escuta de)
    - Método de runasync com o serviço (`StatelessService.runAsync()`) é chamado
3. Se estiver presente, é chamado o método de onOpenAsync do serviço (especificamente, `StatelessService.onOpenAsync()` é chamado. Esta é uma substituição invulgar mas fica disponível).

É importante salientar que não existe nenhum ordenação entre as chamadas para criar e abrir os serviços de escuta e runasync com. Os serviços de escuta poderão abrir antes runasync com é iniciado. Da mesma forma, runasync com poderá acabar invocado antes dos serviços de escuta de comunicação estão abertos ou mesmo foi construídos. Se for necessária qualquer sincronização, será deixada como um exercício para o implementador. Soluções comuns:

* Por vezes, os serviços de escuta não podem funcionar até que outras informações é criado ou trabalho efectuadas. Para serviços sem monitorização de estado de trabalho, normalmente, pode ser feito no construtor do serviço, durante o `createServiceInstanceListeners()` chamar, ou como parte de construção do serviço de escuta.
* Por vezes, o código no runasync com não pretende iniciar até que os serviços de escuta estão abertos. Neste caso coordenação adicional é necessária. Uma solução comum é algumas sinalizador dentro de serviços de escuta que indicam quando se tem concluído, que é verificado runasync com antes de continuar com o trabalho real.

## <a name="stateless-service-shutdown"></a>Encerramento do serviço sem monitorização de estado
Quando encerrar um serviço sem monitorização de estado, o mesmo padrão é seguido apenas na inversa:

1. Em paralelo
    - Quaisquer serviços de escuta abertos são fechados (`CommunicationListener.closeAsync()` denomina-se cada serviço de escuta de)
    - O token de cancelamento transmitido a `runAsync()` foi cancelada (verificar o token de cancelamento `isCancelled` propriedade devolve true e se a chamada do token `throwIfCancellationRequested` método gera um `CancellationException`)
2. Uma vez `closeAsync()` conclusão em cada serviço de escuta e `runAsync()` também estiver concluída, o serviço `StatelessService.onCloseAsync()` se chama o método, se estiver presente (novamente esta é uma substituição invulgar).
3. Depois de `StatelessService.onCloseAsync()` estiver concluída, o objeto de serviço é destructed

## <a name="stateful-service-startup"></a>Arranque do serviço com estado
Os serviços com monitorização de estado têm um padrão semelhante aos serviços sem monitorização de estado, com algumas alterações. Para iniciar um serviço com estado, a ordem de eventos é o seguinte:

1. O serviço é construído.
2. `StatefulServiceBase.onOpenAsync()`é chamado. Esta chamada não é normalmente substituída no serviço.
3. As seguintes ações ocorrem em paralelo:
    - `StatefulServiceBase.createServiceReplicaListeners()`é invocado. 
      - Se o serviço é um serviço principal, todos os serviços de escuta devolvidos estão abertos. `CommunicationListener.openAsync()`é chamado para cada serviço de escuta.
      - Se o serviço é um serviço secundário, apenas esses serviços de escuta marcada como `listenOnSecondary = true` estão abertas. É menos comum ter os serviços de escuta que são abertos em bases de dados secundárias.
    - Se o serviço está atualmente um site primário, o serviço `StatefulServiceBase.runAsync()` método é chamado.
4. Depois do todos os o serviço de escuta réplica `openAsync()` chama concluir e `runAsync()` denomina-se, `StatefulServiceBase.onChangeRoleAsync()` é chamado. Esta chamada não é normalmente substituída no serviço.

Semelhante aos serviços sem monitorização de estado, não há nenhum coordenação entre a ordem em que são criados e abrir os serviços de escuta e quando **runasync com** é chamado. Se precisar de coordenação, as soluções são muito os mesmos. Não há um cenário adicional para o serviço de monitorização de estado. Imaginemos que as chamadas que chegam a escuta de comunicação necessitam de informações mantidas dentro de alguns [coleções fiável](service-fabric-reliable-services-reliable-collections.md). Porque os serviços de escuta de comunicação foi aberta antes das coleções fiáveis são legíveis ou gravável e antes de **runasync com** foi possível iniciar alguns coordenação adicional é necessária. A solução mais simples e mais comuns é para os serviços de escuta de comunicação devolver um código de erro que o cliente utiliza saber para repetir o pedido.

## <a name="stateful-service-shutdown"></a>Encerramento do serviço com monitorização de estado
Como os serviços sem monitorização de estado, os eventos de ciclo de vida durante o encerramento são os mesmos que durante o arranque, mas invertido. Quando um serviço com monitorização de estado está a ser encerrado, ocorrem os seguintes eventos:

1. Em paralelo:
    - Quaisquer serviços de escuta abertos são fechados. `CommunicationListener.closeAsync()`é chamado para cada serviço de escuta.
    - O token de cancelamento transmitido a `runAsync()` foi cancelada. Uma chamada para o token de cancelamento `isCancelled()` método devolve true e se a chamada, o token `throwIfCancellationRequested()` método gera um `OperationCanceledException`.
2. Depois de `closeAsync()` terminar em cada serviço de escuta e `runAsync()` também estiver concluído, o serviço `StatefulServiceBase.onChangeRoleAsync()` é chamado. Esta chamada não é normalmente substituída no serviço.

   > [!NOTE]  
   > A necessidade de aguardar **runasync com** concluir a apenas é necessário se esta réplica é uma réplica primária.

3. Depois do `StatefulServiceBase.onChangeRoleAsync()` método depois de concluída, o `StatefulServiceBase.onCloseAsync()` método é chamado. Esta chamada é uma substituição invulgar, mas está disponível.
3. Depois de `StatefulServiceBase.onCloseAsync()` é destructed estiver concluída, o objeto de serviço.

## <a name="stateful-service-primary-swaps"></a>Trocas de principal de serviço com estado
Enquanto estiver a executar um serviço com estado, as réplicas primárias que serviços com monitorização de estado de tem os respetivos serviços de escuta de comunicação abertos e as respetivas **runasync com** método chamado. Réplicas secundárias são construídas, mas não existem outras chamadas de ver. Enquanto um serviço com estado estiver em execução, a réplica que está atualmente principal pode alterar. O que significa em termos dos eventos de ciclo de vida que pode ver uma réplica? O comportamento que vê a réplica de monitorização de estado depende se está a réplica que está a ser despromovida ou promovida durante a troca.

### <a name="for-the-primary-thats-demoted"></a>Para o site primário que está a ser despromovido
Para a réplica primária, que está a ser despromovida, o Service Fabric tem esta réplica para parar o processamento das mensagens e sair de qualquer trabalho em segundo plano que esteja a executar. Consequentemente, este passo parece que necessitaria se o serviço está a ser encerrado. Uma diferença é que o serviço não se encontra destructed ou fechado porque continua a ser como uma secundária. As APIs seguintes são denominadas:

1. Em paralelo:
    - Quaisquer serviços de escuta abertos são fechados. `CommunicationListener.closeAsync()`é chamado para cada serviço de escuta.
    - O token de cancelamento transmitido a `runAsync()` foi cancelada. Uma verificação do token de cancelamento `isCancelled()` método devolve true e se a chamada, o token `throwIfCancellationRequested()` método gera um `OperationCanceledException`.
2. Depois de `closeAsync()` terminar em cada serviço de escuta e `runAsync()` também estiver concluído, o serviço `StatefulServiceBase.onChangeRoleAsync()` é chamado. Esta chamada não é normalmente substituída no serviço.

### <a name="for-the-secondary-thats-promoted"></a>Para o secundário que é promovido
Da mesma forma, o Service Fabric tem a réplica secundária, que é promovida para começar a escutar mensagens na rede e iniciar quaisquer tarefas em segundo plano, tem de concluir. Consequentemente, este processo parece que necessitaria se o serviço é criado, exceto que a réplica próprio já existe. As APIs seguintes são denominadas:

1. Em paralelo:
    - `StatefulServiceBase.createServiceReplicaListeners()`é invocada e qualquer devolvido os serviços de escuta estão abertos. `CommunicationListener.openAsync()`é chamado para cada serviço de escuta.
    - O serviço `StatefulServiceBase.runAsync()` método é chamado.
2. Depois do todos os o serviço de escuta réplica `openAsync()` chama concluir e `runAsync()` denomina-se, `StatefulServiceBase.onChangeRoleAsync()` é chamado. Esta chamada não é normalmente substituída no serviço.


### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Problemas comuns durante o encerramento do serviço com monitorização de estado e despromoção primária
Service Fabric altera o principal de um serviço com monitorização de estado para diversos motivos. As mais comuns são [cluster reequilíbrio](service-fabric-cluster-resource-manager-balancing.md) e [atualização da aplicação](service-fabric-application-upgrade.md). Durante estas operações (bem como durante o encerramento do serviço normal, como o que poderá ver se o serviço foi eliminado), é importante que o serviço Respeitamos a `cancellationToken`. 

Serviços que não processar corretamente cancelamento podem ter problemas de vários. Estas operações são lentas porque Service Fabric aguarda que os serviços para parar corretamente. Em última análise Isto pode levar a falha de atualizações que o limite de tempo e reverter. Falha que respeite o token de cancelamento também pode provocar imbalanced clusters. Clusters ficam desequilibrados porque nós obter frequente, mas os serviços não podem ser reequilibrados porque demora demasiado tempo a movê-los noutro local. 

Porque os serviços estão com monitorização de estado, também é provável que utilizam o [coleções fiável](service-fabric-reliable-services-reliable-collections.md). No Service Fabric, quando um site primário é despromovido, um dos primeiros aspetos que ocorre é que o acesso de escrita para o estado subjacente foi revogado. Isto leva a um segundo conjunto de problemas que podem afetar o ciclo de vida do serviço. Coleções exceções retorno baseadas no período de tempo e se a réplica está a ser movida ou encerramento. Estas exceções devem ser processadas corretamente. Exceções acionadas por Service Fabric enquadram permanente [(`FabricException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception) e transitório [(`FabricTransientException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception._fabric_transient_exception) categorias. Permanentes exceções devem ser registadas e emitidas enquanto as exceções transitórias podem ser repetidas com base em algumas lógica de repetição.

Processamento de exceções que vêm da utilização do `ReliableCollections` em conjunto com eventos de ciclo de vida do serviço é uma parte importante de teste e a validar um serviço fiável. Recomendamos que são sempre executadas o serviço sob carga ao efetuar atualizações e [chaos testar](service-fabric-controlled-chaos.md) antes de implementar para produção. Os passos básicos ajudam a garantir que o seu serviço é corretamente implementado e processa eventos de ciclo de vida corretamente.

## <a name="notes-on-service-lifecycle"></a>Notas sobre o ciclo de vida do serviço
* Tanto o `runAsync()` método e o `createServiceInstanceListeners/createServiceReplicaListeners` chamadas são opcionais. Um serviço pode ter um dos mesmos, ambos ou nenhum. Por exemplo, se o serviço não todos os respetivo trabalho em resposta a chamadas de utilizador, não é necessário para o mesmo implementar `runAsync()`. Apenas os serviços de escuta de comunicação e o respetivo código associado são necessárias. Da mesma forma, criar e devolver os serviços de escuta de comunicação é opcional, como o serviço pode ter apenas trabalho em segundo plano para o fazer e, por isso, só tem de implementar`runAsync()`
* É válido para um serviço concluir `runAsync()` com êxito e retorno do mesmo. Isto não é considerado uma condição de falha e representaria o trabalho em segundo plano da conclusão de serviço. Para serviços fiáveis com monitorização de estado `runAsync()` teria de ser chamado novamente se o serviço foram despromovido do site primário e, em seguida, promovido novamente para o site primário.
* Se um serviço sai do `runAsync()` por argumentoutofrangeexception algumas exceção inesperada, esta é uma falha e o objeto de serviço é encerrado e reportado um erro de estado de funcionamento.
* Enquanto não estiver não existe nenhum limite de tempo na devolução destes métodos, imediatamente perder a capacidade de escrever e, por conseguinte, não é possível concluir qualquer trabalho real. Recomenda-se que é devolvido como rapidamente possível após receber o pedido de cancelamento. Se o serviço não responder a estas chamadas de API num período de tempo razoável Service Fabric pode forçar a terminar o serviço. Normalmente, isto só ocorre durante as atualizações de aplicações ou quando um serviço está a ser eliminado. Este tempo limite é de 15 minutos por predefinição.
* Falhas no `onCloseAsync()` resultado de caminho no `onAbort()` chamados que é uma oportunidade de melhor esforço de última oportunidade para o serviço limpar configurar e quaisquer recursos que possam tem reclamado de versão.

## <a name="next-steps"></a>Passos Seguintes
* [Introdução a Reliable Services](service-fabric-reliable-services-introduction.md)
* [Início rápido de serviços fiável](service-fabric-reliable-services-quick-start-java.md)
* [Reliable Services avançadas de utilização](service-fabric-reliable-services-advanced-usage.md)
