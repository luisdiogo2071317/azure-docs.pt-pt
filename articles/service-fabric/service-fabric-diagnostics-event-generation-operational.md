---
title: Lista de eventos de recursos de infraestrutura de serviço do Azure | Microsoft Docs
description: A lista completa dos eventos fornecidos pelo Azure Service Fabric para ajudar a monitorizar clusters.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: d397dcb1ecdc25dbd66ab3d6f2f010bc29f87c6c
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/03/2018
---
# <a name="list-of-service-fabric-events"></a>Lista de eventos de Service Fabric 

Service Fabric expõe um conjunto de principal de eventos de cluster para o informar sobre o estado do cluster como [eventos de recursos de infraestrutura de serviço](service-fabric-diagnostics-events.md). Estes são baseadas em ações executadas pelo Service Fabric nos nós e o cluster ou decisões de gestão efetuadas por um proprietário de cluster/operador. Estes eventos podem ser acedidos por consultar o [EventStore](service-fabric-diagnostics-eventstore.md) no seu cluster ou através do canal operacional. Nas máquinas do Windows, o canal operacional está também estabelecer ligação com ele cópias de segurança no registo de eventos - para que possa ver eventos de recursos de infraestrutura de serviço no Visualizador de eventos. 

>[!NOTE]
>Para obter uma lista de eventos de Service Fabric para clusters em versões < 6.2, consulte a secção seguinte. 

Aqui está disponível na plataforma, ordenada pela entidade que mapeiam para uma lista de todos os eventos.

## <a name="cluster-events"></a>Eventos de cluster

**Eventos de atualização de cluster**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStartOperational | CM | Informativo | 1 |
| 29628 | ClusterUpgradeCompleteOperational | CM | Informativo | 1 |
| 29629 | ClusterUpgradeRollbackStartOperational | CM | Informativo | 1 |
| 29630 | ClusterUpgradeRollbackCompleteOperational | CM | Informativo | 1 |
| 29631 | ClusterUpgradeDomainCompleteOperational | CM | Informativo | 1 |

**Eventos de relatório de estado de funcionamento do cluster**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 54428 | ProcessClusterReportOperational | HM | Informativo | 1 |
| 54437 | ExpiredClusterEventOperational | HM | Informativo | 1 |

**Eventos do serviço Chaos**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 50021 | ChaosStartedEvent | Teste | Informativo | 1 |
| 50023 | ChaosStoppedEvent | Teste | Informativo | 1 |

## <a name="node-events"></a>Eventos de nó

**Eventos de ciclo de vida do nó** 

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 18602 | DeactivateNodeCompletedOperational | FM | Informativo | 1 |
| 18603 | NodeUpOperational | FM | Informativo | 1 |
| 18604 | NodeDownOperational | FM | Informativo | 1 |
| 18605 | NodeAddedOperational | FM | Informativo | 1 |
| 18606 | NodeRemovedOperational | FM | Informativo | 1 |
| 18607 | DeactivateNodeStartOperational | FM | Informativo | 1 |
| 25620 | NodeOpening | FabricNode | Informativo | 1 |
| 25621 | NodeOpenedSuccess | FabricNode | Informativo | 1 |
| 25622 | NodeOpenedFailed | FabricNode | Informativo | 1 |
| 25623 | NodeClosing | FabricNode | Informativo | 1 |
| 25624 | NodeClosed | FabricNode | Informativo | 1 |
| 25625 | NodeAborting | FabricNode | Informativo | 1 |
| 25626 | NodeAborted | FabricNode | Informativo | 1 |

**Eventos de relatório de estado de funcionamento do nó**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 54423 | ProcessNodeReportOperational | HM | Informativo | 1 |
| 54432 | ExpiredNodeEventOperational | HM | Informativo | 1 |

**Eventos de nó Chaos**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 50033 | ChaosRestartNodeFaultScheduledEvent | Teste | Informativo | 1 |
| 50087 | ChaosRestartNodeFaultCompletedEvent | Teste | Informativo | 1 |

## <a name="application-events"></a>Eventos da aplicação

**Eventos de ciclo de vida de aplicação**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 29620 | ApplicationCreatedOperational | CM | Informativo | 1 |
| 29625 | ApplicationDeletedOperational | CM | Informativo | 1 |
| 23083 | ProcessExitedOperational | Alojamento | Informativo | 1 |

**Eventos de atualização de aplicações**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 29621 | ApplicationUpgradeStartOperational | CM | Informativo | 1 |
| 29622 | ApplicationUpgradeCompleteOperational | CM | Informativo | 1 |
| 29623 | ApplicationUpgradeRollbackStartOperational | CM | Informativo | 1 |
| 29624 | ApplicationUpgradeRollbackCompleteOperational | CM | Informativo | 1 |
| 29626 | ApplicationUpgradeDomainCompleteOperational | CM | Informativo | 1 |

**Eventos de relatório de estado de funcionamento de aplicações**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 54425 | ProcessApplicationReportOperational | HM | Informativo | 1 |
| 54426 | ProcessDeployedApplicationReportOperational | HM | Informativo | 1 |
| 54427 | ProcessDeployedServicePackageReportOperational | HM | Informativo | 1 |
| 54434 | ExpiredApplicationEventOperational | HM | Informativo | 1 |
| 54435 | ExpiredDeployedApplicationEventOperational | HM | Informativo | 1 |
| 54436 | ExpiredDeployedServicePackageEventOperational | HM | Informativo | 1 |

**Eventos da aplicação Chaos**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 50053 | ChaosRestartCodePackageFaultScheduledEvent | Teste | Informativo | 1 |
| 50101 | ChaosRestartCodePackageFaultCompletedEvent | Teste | Informativo | 1 |

## <a name="service-events"></a>Eventos do serviço

**Eventos de ciclo de vida do serviço**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 18602 | ServiceCreatedOperational | FM | Informativo | 1 |
| 18658 | ServiceDeletedOperational | FM | Informativo | 1 |

**Eventos de relatório de estado de funcionamento do serviço**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 54424 | ProcessServiceReportOperational | HM | Informativo | 1 |
| 54433 | ExpiredServiceEventOperational | HM | Informativo | 1 |

## <a name="partition-events"></a>Eventos de partição

**Eventos de mudança de partições**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 18940 | ReconfigurationCompleted | RA | Informativo | 1 |

**Eventos de relatório de estado de funcionamento de partição**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 54422 | ProcessPartitionReportOperational | HM | Informativo | 1 |
| 54431 | ExpiredPartitionEventOperational | HM | Informativo | 1 |

**Eventos de partição Chaos**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 50069 | ChaosMovePrimaryFaultScheduledEvent | Teste | Informativo | 1 |
| 50077 | ChaosMoveSecondaryFaultScheduledEvent | Teste | Informativo | 1 |

**Eventos de análise de partição**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 65003 | PrimaryMoveAnalysisEvent | Teste | Informativo | 1 |

## <a name="replica-events"></a>Eventos de réplica

**Eventos de relatório de estado de funcionamento de réplica**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 54429 | ProcessStatefulReplicaReportOperational | HM | Informativo | 1 |
| 54430 | ProcessStatelessInstanceReportOperational | HM | Informativo | 1 |
| 54438 | ExpiredStatefulReplicaEventOperational | HM | Informativo | 1 |
| 54439 | ExpiredStatelessInstanceEventOperational | HM | Informativo | 1 |

**Eventos de réplica Chaos**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 50047 | ChaosRestartReplicaFaultScheduledEvent | Teste | Informativo | 1 |
| 50051 | ChaosRemoveReplicaFaultScheduledEvent | Teste | Informativo | 1 |
| 50093 | ChaosRemoveReplicaFaultCompletedEvent | Teste | Informativo | 1 |

## <a name="container-events"></a>Eventos de contentor

**Eventos de ciclo de vida do contentor** 

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 23074 | ContainerActivatedOperational | Alojamento | Informativo | 1 |
| 23075 | ContainerDeactivatedOperational | Alojamento | Informativo | 1 |
| 23082 | ContainerExitedOperational | Alojamento | Informativo | 1 |

## <a name="other-events"></a>Outros eventos

**Eventos de correlação**

| EventId | Nome | Origem (tarefas) | Nível | Versão |
| --- | --- | --- | --- | --- |
| 65011 | CorrelationOperational | Teste | Informativo | 1 |

## <a name="events-prior-to-version-62"></a>Eventos anteriores à versão 6.2

Eis uma lista completa dos eventos fornecidos pelo Service Fabric anteriores à versão 6.2.

| EventId | Nome | Origem (tarefas) | Nível |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Informativo |
| 25621 | NodeOpenedSuccess | FabricNode | Informativo |
| 25622 | NodeOpenedFailed | FabricNode | Informativo |
| 25623 | NodeClosing | FabricNode | Informativo |
| 25624 | NodeClosed | FabricNode | Informativo |
| 25625 | NodeAborting | FabricNode | Informativo |
| 25626 | NodeAborted | FabricNode | Informativo |
| 29627 | ClusterUpgradeStart | CM | Informativo |
| 29628 | ClusterUpgradeComplete | CM | Informativo |
| 29629 | ClusterUpgradeRollback | CM | Informativo |
| 29630 | ClusterUpgradeRollbackComplete | CM | Informativo |
| 29631 | ClusterUpgradeDomainComplete | CM | Informativo |
| 23074 | ContainerActivated | Alojamento | Informativo |
| 23075 | ContainerDeactivated | Alojamento | Informativo |
| 29620 | ApplicationCreated | CM | Informativo |
| 29621 | ApplicationUpgradeStart | CM | Informativo |
| 29622 | ApplicationUpgradeComplete | CM | Informativo |
| 29623 | ApplicationUpgradeRollback | CM | Informativo |
| 29624 | ApplicationUpgradeRollbackComplete | CM | Informativo |
| 29625 | ApplicationDeleted | CM | Informativo |
| 29626 | ApplicationUpgradeDomainComplete | CM | Informativo |
| 18566 | ServiceCreated | FM | Informativo |
| 18567 | ServiceDeleted | FM | Informativo |

## <a name="next-steps"></a>Passos Seguintes

* Saiba mais sobre o global [geração de eventos ao nível da plataforma](service-fabric-diagnostics-event-generation-infra.md) no Service Fabric
* Modificar o [diagnósticos do Azure](service-fabric-diagnostics-event-aggregation-wad.md) configuração para recolher registos mais
* [Configurar o Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) para ver o seu operacional em registos de canal
