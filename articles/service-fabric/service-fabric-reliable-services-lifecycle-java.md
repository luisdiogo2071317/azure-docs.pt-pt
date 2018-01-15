---
title: Ciclo de vida de Service Fabric Reliable Services do Azure | Microsoft Docs
description: Saiba mais sobre os eventos de ciclo de vida no Service Fabric Reliable Services.
services: service-fabric
documentationcenter: java
author: PavanKunapareddyMSFT
manager: timlt
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: pakunapa;
ms.openlocfilehash: ad4228ade68f4494e5be0454643752e742c1cc81
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/13/2018
---
# <a name="reliable-services-lifecycle"></a>Ciclo de vida dos Reliable Services
> [!div class="op_single_selector"]
> * [C# no Windows](service-fabric-reliable-services-lifecycle.md)
> * [Java em Linux](service-fabric-reliable-services-lifecycle-java.md)
>
>

Reliable Services é um dos modelos de programação disponíveis no Service Fabric do Azure. Quando a aprendizagem sobre o ciclo de vida dos Reliable Services, é mais importante compreender os eventos de ciclo de vida básico. A ordenação exato dos eventos depende de detalhes de configuração. 

Em geral, o ciclo de vida Reliable Services inclui os seguintes eventos:

* Durante o arranque:
  * Os serviços são construídos.
  * Serviços tem uma oportunidade para construir e devolver zero ou mais serviços de escuta.
  * Os serviços de escuta devolvidos estão abertos para comunicação com o serviço.
  * O serviço `runAsync` método é chamado, pelo que o serviço pode não longa execução ou em segundo plano de trabalho.
* Durante o encerramento:
  * O token de cancelamento que foi transmitido aos `runAsync` , serão canceladas, e os serviços de escuta estão fechados.
  * O objeto de serviço próprio é destructed.

A ordem dos eventos na Reliable Services poderá alterar ligeiramente dependendo se o serviço fiável é sem monitorização de estado ou com monitorização de estado. 

Além disso, para os serviços com monitorização de estado, devem ser abordadas o cenário de comutação primário. Durante esta sequência, a função de principal é transferida para outra réplica (ou retornado) sem encerramento do serviço. 

Por fim, tem de pensar em condições de erro ou falha.

## <a name="stateless-service-startup"></a>Arranque do serviço sem monitorização de estado
O ciclo de vida de um serviço sem monitorização de estado é bastante simples. Segue-se a ordem de eventos:

1. O serviço é construído.
2. Estes eventos ocorrem em paralelo:
    - `StatelessService.createServiceInstanceListeners()`é invocado, e qualquer devolvido os serviços de escuta estão abertos. `CommunicationListener.openAsync()`é chamado para cada serviço de escuta.
    - O serviço `runAsync` método (`StatelessService.runAsync()`) é chamado.
3. Se estiver presente, do serviço próprio `onOpenAsync` método é chamado. Especificamente, `StatelessService.onOpenAsync()` é chamado. Esta é uma substituição invulgar, mas está disponível.

É importante salientar que não existe nenhum ordenação entre a chamada para criar e abrir os serviços de escuta e a chamada para `runAsync`. Poderão abrir os serviços de escuta antes `runAsync` é iniciado. Da mesma forma, `runAsync` pode ser invocado antes de abrir os serviços de escuta de comunicação ou antes mesmo ter sido criadas. Se for necessária qualquer sincronização, tem de ser efetuada pelo implementador. Seguem-se algumas soluções comuns:

* Por vezes, os serviços de escuta não podem funcionar até que outras informações são criadas ou outro trabalho está concluído. Para os serviços sem monitorização de estado, trabalho, normalmente, pode ser feito no construtor do serviço. Pode ser feita durante o `createServiceInstanceListeners()` chamar, ou como parte de construção do serviço de escuta.
* Por vezes, o código no `runAsync` não será iniciada até a escuta está aberta. Neste caso, coordenação adicional é necessária. É uma solução comum adicionar um sinalizador de serviços de escuta. O sinalizador indica quando tiver concluído a escuta. O `runAsync` método verifica isto antes de continuar o trabalho real.

## <a name="stateless-service-shutdown"></a>Encerramento do serviço sem monitorização de estado
Quando encerrar um serviço sem monitorização de estado, o mesmo padrão é seguido, mas em inversa:

1. Estes eventos ocorrem em paralelo:
    - Quaisquer serviços de escuta abertos são fechados. `CommunicationListener.closeAsync()`é chamado para cada serviço de escuta.
    - O token de cancelamento que foi transmitido aos `runAsync()` foi cancelada. A verificar o token de cancelamento `isCancelled` propriedade devolve `true`e se a chamada, o token `throwIfCancellationRequested` método gera um `CancellationException`.
2. Quando `closeAsync()` terminar em cada serviço de escuta e `runAsync()` também estiver concluído, o serviço `StatelessService.onCloseAsync()` se chama o método, se estiver presente. Novamente, este não é uma substituição comuns.
3. Depois de `StatelessService.onCloseAsync()` é destructed estiver concluída, o objeto de serviço.

## <a name="stateful-service-startup"></a>Arranque do serviço com estado
Os serviços com monitorização de estado têm um padrão que é semelhante aos serviços sem monitorização de estado, com algumas alterações. Segue-se a ordem dos eventos de início de um serviço com monitorização de estado:

1. O serviço é construído.
2. `StatefulServiceBase.onOpenAsync()`é chamado. Esta chamada não é normalmente substituída no serviço.
3. Estes eventos ocorrem em paralelo:
    - `StatefulServiceBase.createServiceReplicaListeners()`é invocado. 
      - Se o serviço é um serviço principal, todos os serviços de escuta devolvidos estão abertos. `CommunicationListener.openAsync()`é chamado para cada serviço de escuta.
      - Se o serviço é um serviço secundário, apenas os serviços de escuta marcada como `listenOnSecondary = true` estão abertas. É menos comum ter os serviços de escuta que são abertos em bases de dados secundárias.
    - Se o serviço está atualmente um site primário, o serviço `StatefulServiceBase.runAsync()` método é chamado.
4. Depois do todos os o serviço de escuta réplica `openAsync()` chama concluir e `runAsync()` denomina-se, `StatefulServiceBase.onChangeRoleAsync()` é chamado. Esta chamada não é normalmente substituída no serviço.

Semelhante aos serviços sem monitorização de estado, no serviço com estado, não há nenhum coordenação entre a ordem em que são criados e abrir os serviços de escuta e quando `runAsync` é chamado. Se precisar de coordenação, as soluções são muito os mesmos. Mas há um cenário adicional para o serviço de monitorização de estado. Imaginemos que as chamadas que chegam a escuta de comunicação necessitam de informações mantidas dentro de alguns [coleções fiável](service-fabric-reliable-services-reliable-collections.md). Uma vez que os serviços de escuta de comunicação poderá ser aberta antes das coleções fiável são legíveis ou gravável e antes de `runAsync` é iniciado, algumas adicional coordenação é necessária. A solução mais simples e mais comuns é para os serviços de escuta de comunicação devolver um código de erro. O cliente utiliza o código de erro saber para repetir o pedido.

## <a name="stateful-service-shutdown"></a>Encerramento do serviço com monitorização de estado
Como os serviços sem monitorização de estado, os eventos de ciclo de vida durante o encerramento são os mesmos que durante o arranque, mas invertido. Quando um serviço com monitorização de estado está a ser encerrado, ocorrem os seguintes eventos:

1. Estes eventos ocorrem em paralelo:
    - Quaisquer serviços de escuta abertos são fechados. `CommunicationListener.closeAsync()`é chamado para cada serviço de escuta.
    - O token de cancelamento que foi transmitido aos `runAsync()` foi cancelada. Uma chamada para o token de cancelamento `isCancelled()` método devolve `true`e se a chamada, o token `throwIfCancellationRequested()` método gera um `OperationCanceledException`.
2. Depois de `closeAsync()` terminar em cada serviço de escuta e `runAsync()` também estiver concluído, o serviço `StatefulServiceBase.onChangeRoleAsync()` é chamado. Esta chamada não é normalmente substituída no serviço.

   > [!NOTE]  
   > A aguardar `runAsync` para concluir é necessário apenas se esta réplica é uma réplica primária.

3. Depois do `StatefulServiceBase.onChangeRoleAsync()` método depois de concluída, o `StatefulServiceBase.onCloseAsync()` método é chamado. Esta chamada é uma substituição invulgar, mas está disponível.
3. Depois de `StatefulServiceBase.onCloseAsync()` é destructed estiver concluída, o objeto de serviço.

## <a name="stateful-service-primary-swaps"></a>Trocas de principal de serviço com estado
Enquanto estiver a executar um serviço com estado, os serviços de escuta de comunicação estão abertos e `runAsync` método denomina-se apenas para as réplicas primárias que serviços com monitorização de estado. Réplicas secundárias são construídas, mas não existem outras chamadas de ver. Enquanto um serviço com estado estiver em execução, a réplica que está atualmente principal pode alterar. Os eventos de ciclo de vida que pode ver uma réplica de monitorização de estado depende se está a réplica que está a ser despromovida ou promovida durante a troca.

### <a name="for-the-demoted-primary"></a>Para o principal demoted
Service Fabric tem da réplica primária, que é despromovida para parar o processamento das mensagens e parar qualquer trabalho em segundo plano. Este passo é semelhante à quando o serviço é encerrado. Uma diferença é que o serviço não se encontra destructed ou fechado, porque continua a ser como uma secundária. Ocorrerem os seguintes eventos:

1. Estes eventos ocorrem em paralelo:
    - Quaisquer serviços de escuta abertos são fechados. `CommunicationListener.closeAsync()`é chamado para cada serviço de escuta.
    - O token de cancelamento que foi transmitido aos `runAsync()` foi cancelada. Uma verificação do token de cancelamento `isCancelled()` método devolve `true`. Se a chamada, o token `throwIfCancellationRequested()` método gera um `OperationCanceledException`.
2. Depois de `closeAsync()` terminar em cada serviço de escuta e `runAsync()` também estiver concluído, o serviço `StatefulServiceBase.onChangeRoleAsync()` é chamado. Esta chamada não é normalmente substituída no serviço.

### <a name="for-the-promoted-secondary"></a>Para o secundário promovido
Da mesma forma, o Service Fabric tem a réplica secundária, que é promovida para começar a escutar mensagens na rede e iniciar quaisquer tarefas em segundo plano que necessita para concluir. Este processo é semelhante à quando o serviço é criado. A diferença é que a réplica próprio já existe. Ocorrerem os seguintes eventos:

1. Estes eventos ocorrem em paralelo:
    - `StatefulServiceBase.createServiceReplicaListeners()`é invocada e qualquer devolvido os serviços de escuta estão abertos. `CommunicationListener.openAsync()`é chamado para cada serviço de escuta.
    - O serviço `StatefulServiceBase.runAsync()` método é chamado.
2. Depois do todos os o serviço de escuta réplica `openAsync()` chama concluir e `runAsync()` denomina-se, `StatefulServiceBase.onChangeRoleAsync()` é chamado. Esta chamada não é normalmente substituída no serviço.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Problemas comuns durante o encerramento do serviço com monitorização de estado e despromoção primária
Service Fabric altera o principal de um serviço com monitorização de estado por vários motivos. Os motivos mais comuns são [cluster reequilíbrio](service-fabric-cluster-resource-manager-balancing.md) e [atualização da aplicação](service-fabric-application-upgrade.md). Durante estas operações, é importante que o serviço respeita o `cancellationToken`. Isto também se aplicam durante o encerramento do serviço normal, por exemplo, se o serviço foi eliminado.

Os serviços que não a processar corretamente cancelamento podem ter problemas de vários. Estas operações são lentas porque Service Fabric aguarda que os serviços para parar corretamente. Fundamentalmente, isto pode levar a falha de atualizações que o limite de tempo e a reversão. Falha que respeite o token de cancelamento também pode fazer com que imbalanced clusters. Clusters ficam desequilibrados porque nós obter acesso frequente. No entanto, os serviços não podem ser reequilibrados porque demora demasiado tempo a movê-los noutro local. 

Porque os serviços estão com monitorização de estado, também é provável que os serviços utilizam [coleções fiável](service-fabric-reliable-services-reliable-collections.md). No Service Fabric, quando um site primário é despromovido, um dos primeiros aspetos que ocorre é que o acesso de escrita para o estado subjacente foi revogado. Isto leva a um segundo conjunto de problemas que podem afetar o ciclo de vida do serviço. Coleções exceções retorno baseadas no período de tempo e se a réplica está a ser movida ou encerramento. É importante processar estas exceções corretamente. 

Exceções acionadas pelo serviço de recursos de infraestrutura são permanentes [(`FabricException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception) ou transitório [(`FabricTransientException`)](https://docs.microsoft.com/en-us/java/api/system.fabric.exception._fabric_transient_exception). Permanentes exceções devem ser registadas e emitidas. Podem ser repetidas exceções transitórias com base na lógica de repetição.

Uma parte importante de teste e a validar Reliable Services está a processar as exceções que vêm da utilização de `ReliableCollections` em conjunto com eventos de ciclo de vida do serviço. Recomendamos que são sempre executadas o serviço de carga. Também deve efetuar atualizações e [chaos testar](service-fabric-controlled-chaos.md) antes de implementar para produção. Os passos básicos ajudam a garantir que o seu serviço é implementado corretamente e processa os eventos de ciclo de vida corretamente.

## <a name="notes-on-service-lifecycle"></a>Notas sobre o ciclo de vida do serviço
* Tanto o `runAsync()` método e o `createServiceInstanceListeners/createServiceReplicaListeners` chamadas são opcionais. Um serviço pode ter um, ambos ou nenhum dos dois. Por exemplo, se o serviço não todos os respetivo trabalho em resposta a chamadas de utilizador, não é necessário para o mesmo implementar `runAsync()`. Apenas os serviços de escuta de comunicação e o respetivo código associado são necessárias. 

  Da mesma forma, criar e devolver os serviços de escuta de comunicação é opcional. O serviço poderá ter apenas trabalho em segundo plano para, pelo que só tem de implementar `runAsync()`.
* É válido para um serviço concluir `runAsync()` com êxito e retorno do mesmo. Esta não é considerada uma condição de falha. Representa o trabalho em segundo plano de serviço a concluir. Para Reliable Services com monitorização de estado, `runAsync()` seria possível novamente chamado se o serviço é despromovido do site primário e, em seguida, promovido novamente para o site primário.
* Se um serviço sai do `runAsync()` por argumentoutofrangeexception algumas exceção inesperada, esta é uma falha. O objeto de serviço é encerrado e é comunicado um erro de estado de funcionamento.
* Apesar de não existe nenhum limite de tempo na devolução destes métodos, imediatamente perderá a capacidade de escrever. Por conseguinte, não é possível concluir o trabalho real. Recomendamos que pode ser devolvido como rapidamente possível após receber o pedido de cancelamento. Se o serviço não responder a estas chamadas de API num período de tempo razoável, Service Fabric pode forçar a terminar o serviço. Normalmente, isto acontece apenas durante as atualizações de aplicações ou quando um serviço está a ser eliminado. Este tempo limite é de 15 minutos por predefinição.
* Falhas no `onCloseAsync()` resultado de caminho no `onAbort()` a ser chamado. Esta chamada é uma oportunidade de última oportunidade, melhor esforço limpar e quaisquer recursos que possam tem reclamado da versão do serviço.

## <a name="next-steps"></a>Passos Seguintes
* [Introdução a Reliable Services](service-fabric-reliable-services-introduction.md)
* [Guia de introdução de serviços fiável](service-fabric-reliable-services-quick-start-java.md)
* [Reliable Services avançadas de utilização](service-fabric-reliable-services-advanced-usage.md)
