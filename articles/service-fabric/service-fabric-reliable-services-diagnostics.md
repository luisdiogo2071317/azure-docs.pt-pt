---
title: Diagnóstico de Service Fabric com monitorização de estado Reliable Services do Azure | Microsoft Docs
description: Funcionalidade de diagnóstico para monitorização de estado Reliable Services no Azure Service Fabric
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2018
ms.author: dekapur
ms.openlocfilehash: 51895731efd466a314877e963a5fd2c6d868ec02
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110877"
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Funcionalidade de diagnóstico dos Reliable Services com Monitorização de Estado
A classe de Azure Service Fabric com monitorização de estado fiável serviços StatefulServiceBase emite [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) eventos que podem ser utilizados para depurar o serviço, fornecer informações sobre como o tempo de execução está a funcionar e ajudar a resolver problemas.

## <a name="eventsource-events"></a>Eventos de EventSource
O nome de EventSource para a classe de monitorização de estado Reliable Services StatefulServiceBase é "Microsoft-ServiceFabric-Services." Eventos a partir desta origem de evento são apresentados no [eventos de diagnóstico](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) janela quando o serviço está a ser [debugged no Visual Studio](service-fabric-debugging-your-application.md).

São exemplos de ferramentas e tecnologias que ajudam na recolha de e/ou a visualização de eventos de EventSource [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [diagnósticos do Azure](../cloud-services/cloud-services-dotnet-diagnostics.md)e o [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Eventos
| Nome do evento | ID do Evento | Nível | Descrição do evento |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Informativo |Emitidos quando a tarefa de runasync com de serviço é iniciada |
| StatefulRunAsyncCancellation |2 |Informativo |Emitidos quando a tarefa de runasync com do serviço foi cancelada |
| StatefulRunAsyncCompletion |3 |Informativo |Emitidos quando a tarefa de runasync com de serviço está concluída |
| StatefulRunAsyncSlowCancellation |4 |Aviso |Quando a tarefa de runasync com serviço demora demasiado tempo a completar o cancelamento de emitidos |
| StatefulRunAsyncFailure |5 |Erro |Emitidos quando a tarefa de runasync com serviço emite uma exceção |

## <a name="interpret-events"></a>Interpretar os eventos
Eventos de StatefulRunAsyncInvocation, StatefulRunAsyncCompletion e StatefulRunAsyncCancellation são úteis para o escritor do serviço para compreender o ciclo de vida de um serviço, bem como a temporização quando um serviço é iniciado, cancela ou termina. Estas informações podem ser úteis quando depurar problemas do serviço ou compreender o ciclo de vida do serviço.

Os escritores de serviço devem preste especial atenção a eventos StatefulRunAsyncSlowCancellation e StatefulRunAsyncFailure porque indicam problemas com o serviço.

StatefulRunAsyncFailure é emitida sempre que a tarefa de RunAsync() serviço emite uma exceção. Normalmente, uma exceção emitida indica um erro ou erros no serviço. Além disso, a exceção provoca o serviço falhar, pelo que este for movido para um nó diferente. Esta operação pode ser dispendiosa e pode atrasar pedidos recebidos, enquanto o serviço é movido. Os escritores de serviço devem determinar a causa da exceção e, se possível, mitigá-lo.

StatefulRunAsyncSlowCancellation é emitida sempre que um pedido de cancelamento da tarefa runasync com demora mais de quatro segundos. Quando um serviço demora demasiado tempo a completar o cancelamento, afeta a capacidade do serviço de rapidamente ser reiniciada noutro nó. Este cenário poderá afetar a disponibilidade geral do serviço.

## <a name="performance-counters"></a>Contadores de desempenho
O tempo de execução Reliable Services define as categorias de contador de desempenho seguintes:

| Categoria | Descrição |
| --- | --- |
| Replicador Transacional do Service Fabric |Contadores específicos para o replicador transacional do Service Fabric de Azure |
| TStore do Service Fabric |Contadores específicos para o TStore de recursos de infraestrutura de serviço do Azure |

Replicador transacional do Service Fabric é utilizado pelo [fiável Gestor de estado](service-fabric-reliable-services-reliable-collections-internals.md) para replicar as transações dentro de um determinado conjunto de [réplicas](service-fabric-concepts-replica-lifecycle.md).

O TStore de recursos de infraestrutura de serviço é um componente utilizado no [coleções fiável](service-fabric-reliable-services-reliable-collections-internals.md) para armazenar e obter pares chave-valor.

O [Monitor de desempenho do Windows](https://technet.microsoft.com/library/cc749249.aspx) aplicação que está disponível por predefinição no sistema operativo Windows pode ser utilizada para recolher e visualizar dados de contador de desempenho. [Diagnóstico do Azure](../cloud-services/cloud-services-dotnet-diagnostics.md) é outra opção para recolher dados de contador de desempenho e carregá-lo para as tabelas do Azure.

### <a name="performance-counter-instance-names"></a>Nomes de instâncias do contador de desempenho
Um cluster com um grande número de partições de serviço fiável ou serviços fiáveis terão um grande número de instâncias de contador de desempenho do replicador transacional. Também é o caso dos contadores de desempenho TStore, mas também é multiplicado pelo número de dicionários fiável e fiável filas utilizados. Os nomes das instâncias do contador de desempenho podem ajudar a identificar o específicos [partição](service-fabric-concepts-partitioning.md), a réplica de serviço e o fornecedor de estado no caso de TStore, que se encontra associada a instância do contador de desempenho.

#### <a name="service-fabric-transactional-replicator-category"></a>Categoria do replicador transacional do Service Fabric
Para a categoria de `Service Fabric Transactional Replicator`, os nomes de instância do contador estão no formato seguinte:

`ServiceFabricPartitionId:ServiceFabricReplicaId`

*ServiceFabricPartitionId* é a representação de cadeia do ID de partição de recursos de infraestrutura de serviço que se encontra associada a instância do contador de desempenho. O ID de partição é um GUID e respetiva representação de cadeia que é gerada através de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) com o especificador de formato "D".

*ServiceFabricReplicaId* é o ID associado uma réplica especificada de um serviço fiável. ID de réplica está incluído no nome de instância do contador de desempenho para garantir a exclusividade e evitar conflitos com outras instâncias do contador de desempenho geradas pela mesma partição. Podem encontrar mais detalhes sobre as réplicas e a respetiva função nos serviços de fiáveis [aqui](service-fabric-concepts-replica-lifecycle.md).

O nome de instância do contador seguinte é típico para um contador sob o `Service Fabric Transactional Replicator` categoria:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571`

No exemplo anterior, `00d0126d-3e36-4d68-98da-cc4f7195d85e` é a representação de cadeia de ID de partição Service Fabric, e `131652217797162571` é o ID de réplica.

#### <a name="service-fabric-tstore-category"></a>Categoria de TStore de recursos de infraestrutura de serviço
Para a categoria de `Service Fabric TStore`, os nomes de instância do contador estão no formato seguinte:

`ServiceFabricPartitionId:ServiceFabricReplicaId:ServiceFabricStateProviderId_PerformanceCounterInstanceDifferentiator`

*ServiceFabricPartitionId* é a representação de cadeia do ID de partição de recursos de infraestrutura de serviço que se encontra associada a instância do contador de desempenho. O ID de partição é um GUID e respetiva representação de cadeia que é gerada através de [ `Guid.ToString` ](https://msdn.microsoft.com/library/97af8hh4.aspx) com o especificador de formato "D".

*ServiceFabricReplicaId* é o ID associado uma réplica especificada de um serviço fiável. ID de réplica está incluído no nome de instância do contador de desempenho para garantir a exclusividade e evitar conflitos com outras instâncias do contador de desempenho geradas pela mesma partição. Podem encontrar mais detalhes sobre as réplicas e a respetiva função nos serviços de fiáveis [aqui](service-fabric-concepts-replica-lifecycle.md).

*ServiceFabricStateProviderId* é o ID associado um fornecedor de estado dentro de um serviço fiável. ID do fornecedor de estado está incluído o nome de instância do contador de desempenho para diferenciar uma TStore a partir de outra.

*PerformanceCounterInstanceDifferentiator* é um ID de differentiating associado uma instância do contador de desempenho dentro de um fornecedor de estado. Este differentiator está incluído o nome de instância do contador de desempenho para garantir a exclusividade e evitar conflitos com outras instâncias do contador de desempenho geradas pelo mesmo fornecedor de estado.

O nome de instância do contador seguinte é típico para um contador sob o `Service Fabric TStore` categoria:

`00d0126d-3e36-4d68-98da-cc4f7195d85e:131652217797162571:142652217797162571_1337`

No exemplo anterior, `00d0126d-3e36-4d68-98da-cc4f7195d85e` é a representação de cadeia de ID de partição Service Fabric, `131652217797162571` é o ID de réplica, `142652217797162571` é o ID do fornecedor de estado, e `1337` é o differentiator de instância do contador de desempenho.

### <a name="transactional-replicator-performance-counters"></a>Contadores de desempenho do replicador transacionais

O tempo de execução Reliable Services emite os seguintes eventos sob o `Service Fabric Transactional Replicator` categoria

 Nome do contador | Descrição |
| --- | --- |
| Início de Operações de Transações/segundo | O número de transações de escrita novas criadas por segundo.|
| Operações de Transações/segundo | O número de operações Adicionar/atualizar/eliminar efetuada em coleções fiáveis por segundo.|
| Média Esvaziar latência (ms) | O número de bytes a libertar no disco pelo replicador transacional por segundo |
| Operações Limitadas/segundo | O número de operações rejeitadas a cada segundo pelo replicador transacional devido a limitação. |
| Média Transação ms/consolidação | Latência média de consolidação por transação em milissegundos |
| Média Esvaziar latência (ms) | Duração média das operações de libertação de disco iniciadas pelo replicador transacional em milissegundos |

### <a name="tstore-performance-counters"></a>Contadores de desempenho TStore

O tempo de execução Reliable Services emite os seguintes eventos sob o `Service Fabric TStore` categoria

 Nome do contador | Descrição |
| --- | --- |
| Número de Itens | O número de chaves no arquivo.|

## <a name="next-steps"></a>Passos Seguintes
[Fornecedores de EventSource no PerfView](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
