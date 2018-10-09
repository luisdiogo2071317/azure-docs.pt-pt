---
title: Lista de eventos de recursos de infraestrutura do serviço do Azure | Documentos da Microsoft
description: Lista completa dos eventos fornecidos pelo Azure Service Fabric para ajudar a monitorizar clusters.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: 03dac03405588ba00a0f8ca5b127956c40853e36
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868518"
---
# <a name="list-of-service-fabric-events"></a>Lista de eventos do Service Fabric 

Service Fabric expõe um conjunto principal de eventos de cluster para o informar do Estado do seu cluster como [eventos de Service Fabric](service-fabric-diagnostics-events.md). Estes são baseiam-se a ações executadas pelo Service Fabric no seu cluster e os nós ou decisões de gerenciamento feitas por um operador/proprietário de cluster. Esses eventos podem ser acedidos através da consulta a [EventStore](service-fabric-diagnostics-eventstore.md) no seu cluster, ou por meio do canal operacional. Nas máquinas do Windows, o canal operacional também conectado ao registo de eventos - para que possa ver eventos no Visualizador de eventos de Service Fabric. 

>[!NOTE]
>Para obter uma lista de eventos do Service Fabric para clusters em versões < 6.2, consulte a secção seguinte. 

Aqui está disponível na plataforma, classificada pela entidade que podem ser mapeados para uma lista de todos os eventos.

## <a name="cluster-events"></a>Eventos de cluster

**Eventos de atualização de cluster**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | --- | --- | --- | --- |
| 29627 | ClusterUpgradeStarted | Foi iniciada uma atualização de cluster | CM | Informativo | 1 |
| 29628 | ClusterUpgradeCompleted | Uma atualização do cluster foi concluída| CM | Informativo | 1 |
| 29629 | ClusterUpgradeRollbackStarted | Uma atualização do cluster foi iniciado para reversão | CM | Informativo | 1 |
| 29630 | ClusterUpgradeRollbackCompleted | Uma atualização do cluster foi concluída de reversão | CM | Informativo | 1 |
| 29631 | ClusterUpgradeDomainCompleted | Uma atualização de domínio foi concluída durante uma atualização do cluster | CM | Informativo | 1 |

## <a name="node-events"></a>Eventos de nó

**Eventos de ciclo de vida de nó** 

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 18602 | NodeDeactivateCompleted | Desativação de um nó foi concluída | FM | Informativo | 1 |
| 18603 | NodeUp | O cluster detetou que um nó de ter iniciado | FM | Informativo | 1 |
| 18604 | NodeDown | O cluster detetou que um nó foi encerrado |  FM | Informativo | 1 |
| 18605 | NodeAddedToCluster | Foi adicionado um novo nó ao cluster | FM | Informativo | 1 |
| 18606 | NodeRemovedFromCluster | Foi removido um nó do cluster | FM | Informativo | 1 |
| 18607 | NodeDeactivateStarted | Desativação de um nó foi iniciada | FM | Informativo | 1 |
| 25620 | NodeOpening | Um nó está a iniciar. Primeira fase do ciclo de vida de nó | FabricNode | Informativo | 1 |
| 25621 | NodeOpenSucceeded | Um nó iniciado com êxito | FabricNode | Informativo | 1 |
| 25622 | NodeOpenFailed | Um nó falha ao iniciar | FabricNode | Informativo | 1 |
| 25623 | NodeClosing | Um nó está a ser encerrado. Início da fase final do ciclo de vida de nó | FabricNode | Informativo | 1 |
| 25624 | NodeClosed | Um nó desligado com êxito | FabricNode | Informativo | 1 |
| 25625 | NodeAborting | Um nó está a começar a maneira brusca encerrar | FabricNode | Informativo | 1 |
| 25626 | NodeAborted | Um nó de maneira brusca foi encerrado | FabricNode | Informativo | 1 |

## <a name="application-events"></a>Eventos da aplicação

**Eventos de ciclo de vida da aplicação**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 29620 | ApplicationCreated | Foi criada uma nova aplicação | CM | Informativo | 1 |
| 29625 | ApplicationDeleted | Um aplicativo existente foi eliminado | CM | Informativo | 1 |
| 23083 | ApplicationProcessExited | Um processo dentro de um aplicativo saiu | Hospedagem | Informativo | 1 |

**Eventos de atualização de aplicação**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 29621 | ApplicationUpgradeStarted | Foi iniciada uma atualização da aplicação | CM | Informativo | 1 |
| 29622 | ApplicationUpgradeCompleted | Uma atualização da aplicação foi concluída | CM | Informativo | 1 |
| 29623 | ApplicationUpgradeRollbackStarted | Uma atualização da aplicação foi iniciada para reversão |CM | Informativo | 1 |
| 29624 | ApplicationUpgradeRollbackCompleted | Uma atualização da aplicação foi concluída de reversão | CM | Informativo | 1 |
| 29626 | ApplicationUpgradeDomainCompleted | Uma atualização de domínio foi concluída durante uma atualização da aplicação | CM | Informativo | 1 |

## <a name="service-events"></a>Eventos do serviço

**Eventos de ciclo de vida do serviço**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 18657 | ServiceCreated | Foi criado um novo serviço | FM | Informativo | 1 |
| 18658 | ServiceDeleted | Um serviço existente foi eliminado | FM | Informativo | 1 |

## <a name="partition-events"></a>Eventos de partição

**Eventos de mudança de partição**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 18940 | PartitionReconfigured | Foi concluída uma reconfiguração da partição | RA | Informativo | 1 |

## <a name="container-events"></a>Eventos de contentor

**Eventos de ciclo de vida do contentor** 

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 23074 | ContainerActivated | Um contentor foi iniciada | Hospedagem | Informativo | 1 |
| 23075 | ContainerDeactivated | Um contentor foi parado | Hospedagem | Informativo | 1 |
| 23082 | ContainerExited | Um contentor saiu - Verifique o sinalizador de UnexpectedTermination | Hospedagem | Informativo | 1 |

## <a name="health-reports"></a>Relatórios de estado de funcionamento

**Eventos de relatório de estado de funcionamento do cluster**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | --- | --- | --- | --- |
| 54428 | ClusterNewHealthReport | Um novo relatório de estado de funcionamento do cluster está disponível | HM | Informativo | 1 |
| 54437 | ClusterHealthReportExpired | Expiração de um relatório de estado de funcionamento do cluster existente | HM | Informativo | 1 |

**Eventos de relatório de estado de funcionamento do nó**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 54423 | NodeNewHealthReport | Um relatório de estado de funcionamento do novo nó está disponível | HM | Informativo | 1 |
| 54432 | NodeHealthReportExpired | Expiração de um relatório de estado de funcionamento do nó existente | HM | Informativo | 1 |

**Eventos de relatório de estado de funcionamento de aplicações**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 54425 | ApplicationNewHealthReport | Foi criado um novo relatório de estado de funcionamento do aplicativo. Isto é para aplicações não implementadas. | HM | Informativo | 1 |
| 54426 | DeployedApplicationNewHealthReport | Foi criado um novo relatório de estado de funcionamento da aplicação implementada | HM | Informativo | 1 |
| 54427 | DeployedServicePackageNewHealthReport | Foi criado um novo relatório de estado de funcionamento do serviço implementado | HM | Informativo | 1 |
| 54434 | ApplicationHealthReportExpired | Expiração de um relatório de estado de funcionamento de aplicação existente | HM | Informativo | 1 |
| 54435 | DeployedApplicationHealthReportExpired | Um relatório de estado de funcionamento de aplicação implementada existente expirou | HM | Informativo | 1 |
| 54436 | DeployedServicePackageHealthReportExpired | Um relatório de estado de funcionamento do serviço implementado existente expirou | HM | Informativo | 1 |

**Eventos de relatório de estado de funcionamento do serviço**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 54424 | ServiceNewHealthReport | Foi criado um novo relatório de estado de funcionamento de serviço | HM | Informativo | 1 |
| 54433 | ServiceHealthReportExpired | Expiração de um relatório de estado de funcionamento de serviço existente | HM | Informativo | 1 |

**Eventos de relatório de estado de funcionamento de partição**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 54422 | PartitionNewHealthReport | Foi criado um novo relatório de estado de funcionamento de partição | HM | Informativo | 1 |
| 54431 | PartitionHealthReportExpired | Um relatório de estado de funcionamento de partição existente expirou | HM | Informativo | 1 |

**Eventos de relatório de estado de funcionamento de réplica**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 54429 | StatefulReplicaNewHealthReport | Foi criado um relatório de estado de funcionamento de réplica com monitoração de estado | HM | Informativo | 1 |
| 54430 | StatelessInstanceNewHealthReport | Foi criado um novo relatório de estado de funcionamento de instância sem monitoração de estado | HM | Informativo | 1 |
| 54438 | StatefulReplicaHealthReportExpired | Um relatório de estado de funcionamento com monitoração de estado de réplica existente expirou | HM | Informativo | 1 |
| 54439 | StatelessInstanceHealthReportExpired | Um relatório de estado de funcionamento de instância sem monitoração de estado existente expirou | HM | Informativo | 1 |

## <a name="chaos-testing-events"></a>Eventos de testes de Chaos 

**Eventos de sessão de Chaos**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 50021 | ChaosStarted | Foi iniciada uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |
| 50023 | ChaosStopped | Uma sessão de teste de Chaos parou | Capacidade de teste | Informativo | 1 |

**Eventos do nó de Chaos**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 50033 | ChaosNodeRestartScheduled | Um nó está agendado para reiniciar como parte de uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |
| 50087 | ChaosNodeRestartCompleted | Um nó concluiu a reiniciar como parte de uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |

**Eventos da aplicação caos**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 50053 | ChaosCodePackageRestartScheduled | Um reinício do pacote de código foi agendado durante uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |
| 50101 | ChaosCodePackageRestartCompleted | Um reinício do pacote de código foi concluída durante uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |

**Eventos de partição caos**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 50069 | ChaosPartitionPrimaryMoveScheduled | Uma partição primária está agendado para mover como parte de uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |
| 50077 | ChaosPartitionSecondaryMoveScheduled | Uma partição secundária está agendado para mover como parte de uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |
| 65003 | PartitionPrimaryMoveAnalysis | A análise mais aprofundada da mudança de partição primária está disponível | Capacidade de teste | Informativo | 1 |

**Eventos da réplica caos**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 50047 | ChaosReplicaRestartScheduled | Foi agendado um reinício de réplica como parte de uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |
| 50051 | ChaosReplicaRemovalScheduled | Foi agendada uma remoção de réplica como parte de uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |
| 50093 | ChaosReplicaRemovalCompleted | Uma remoção de réplica foi concluída como parte de uma sessão de teste de Chaos | Capacidade de teste | Informativo | 1 |

## <a name="other-events"></a>Outros eventos

**Eventos do correlacionador**

| EventId | Nome | Descrição |Origem (tarefa) | Nível | Versão |
| --- | --- | ---| --- | --- | --- |
| 65011 | CorrelationOperational | Uma correlação tem sido detacted | Capacidade de teste | Informativo | 1 |

## <a name="events-prior-to-version-62"></a>Eventos anteriores à versão 6.2

Aqui está uma lista completa dos eventos fornecidos pelo Service Fabric anterior à versão 6.2.

| EventId | Nome | Origem (tarefa) | Nível |
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
| 23074 | ContainerActivated | Hospedagem | Informativo |
| 23075 | ContainerDeactivated | Hospedagem | Informativo |
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

* Saiba mais sobre geral [geração de eventos ao nível da plataforma](service-fabric-diagnostics-event-generation-infra.md) no Service Fabric
* Modificar seus [diagnóstico do Azure](service-fabric-diagnostics-event-aggregation-wad.md) configuração para recolher registos mais
* [Configurar o Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) para ver seu operacional registos de canal
