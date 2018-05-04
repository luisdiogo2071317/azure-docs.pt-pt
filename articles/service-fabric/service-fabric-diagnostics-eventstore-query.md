---
title: Consulta para eventos de cluster com as APIs de EventStore em clusters de Service Fabric do Azure | Microsoft Docs
description: Saiba como utilizar as APIs do Azure Service Fabric EventStore a consulta para eventos de plataforma
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: af97385981c61f32c4136921d3cf14a526fc6ddb
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="query-eventstore-apis-for-cluster-events"></a>APIs de EventStore de consulta para eventos de cluster

Este artigo abrange como consultar as APIs de EventStore que estão disponíveis no Service Fabric versão 6.2 e mais tarde - se de que pretende obter mais informações sobre o serviço de EventStore, consulte o [descrição geral do serviço EventStore](service-fabric-diagnostics-eventstore.md). Atualmente, o serviço de EventStore só pode aceder dados nos últimos 7 dias (isto é com base na política de retenção de dados de diagnóstico do cluster).

>[!NOTE]
>A partir do Service Fabric versão 6.2. as APIs de EventStore estão atualmente em pré-visualização para os clusters do Windows em execução no Azure apenas. Estamos a trabalhar transferências esta funcionalidade para o Linux, bem como as nossas clusters autónomos.

As APIs de EventStore pode ser acedida diretamente através de um ponto final de REST ou através de programação. Dependendo da consulta, existem vários parâmetros que são necessárias para recolher os dados adequados. Normalmente, incluem:
* `api-version`: a versão das APIs EventStore estiver a utilizar
* `StartTimeUtc`: define o início do período de estiver interessantes no observar
* `EndTimeUtc`: fim do período de tempo

Além destas, existem parâmetros opcionais disponíveis, tal como:
* `timeout`: o tempo limite de 60 segundos predefinidos para efetuar a operação de pedido de substituição
* `eventstypesfilter`: Isto dá-a opção para filtrar os tipos de evento específico
* `ExcludeAnalysisEvents`: não devolver eventos 'Analysis'. Por predefinição, as consultas EventStore irão devolver com eventos de "analysis" sempre que possível - análise eventos são mais rico canal operacional que contenham informações para além de um evento normal do Service Fabric ou contexto adicional e fornecer mais profundidade.
* `SkipCorrelationLookup`: não procurar eventos correlacionados potenciais no cluster. Por predefinição, o EventStore irá tentar correlacionar eventos de um cluster e liga os eventos entre si sempre que possível. 

Cada entidade de um cluster pode ser consultas de eventos. Também pode consultar eventos para todas as entidades do tipo. Por exemplo, pode consultar eventos para um nó específico ou para todos os nós do cluster. O conjunto de entidades para os quais pode consultar eventos atual é (como a consulta deverá ser estruturada):
* Cluster: `/EventsStore/Cluster/Events`
* Nós: `/EventsStore/Nodes/Events`
* Nó: `/EventsStore/Nodes/<NodeName>/$/Events`
* Aplicações: `/EventsStore/Applications/Events`
* Aplicação: `/EventsStore/Applications/<AppName>/$/Events`
* Serviços: `/EventsStore/Services/Events`
* Serviço: `/EventsStore/Services/<ServiceName>/$/Events`
* Partições: `/EventsStore/Partitions/Events`
* Partição: `/EventsStore/Partitions/<PartitionID>/$/Events`
* Réplicas: `/EventsStore/Partitions/<PartitionID>/$/Replicas/Events`
* Réplica: `/EventsStore/Partitions/<PartitionID>/$/Replicas/<ReplicaID>/$/Events`

>[!NOTE]
>Quando referenciar uma aplicação ou o nome do serviço, a consulta não tem de incluir o "fabric: /" prefixo. Além disso, se a aplicação ou nomes de serviço tem um "/" nos mesmos, mude para um "~" para manter o trabalho de consulta. Por exemplo, se a aplicação aparece como "fabric: / App1/FrontendApp", as consultas específico da aplicação deverá ser estruturadas como `/EventsStore/Applications/App1~FrontendApp/$/Events`.
>Além disso, os relatórios de estado de funcionamento para serviços hoje apareçam sob a aplicação correspondente, que deverá consultar para `DeployedServiceHealthReportCreated` eventos para a entidade de aplicação correto. 

## <a name="query-the-eventstore-via-rest-api-endpoints"></a>Consulta EventStore através de pontos finais da API de REST

Pode consultar o EventStore diretamente através de um ponto final REST, tornando `GET` pedidos: `<your cluster address>/EventsStore/<entity>/Events/`.

Por exemplo, para consultar todos os eventos de Cluster entre `2018-04-03T18:00:00Z` e `2018-04-04T18:00:00Z`, o pedido teria aspeto:

```
Method: GET 
URL: http://mycluster:19080/EventsStore/Cluster/Events?api-version=6.2-preview&StartTimeUtc=2018-04-03T18:00:00Z&EndTimeUtc=2018-04-04T18:00:00Z
```

Se esta foi devolver um erro se não há eventos disponíveis, ou se a consulta foi concluída com êxito, verá eventos devolvidos no json:

```json
Response: 200
Body:
[
  {
    "Kind": "ClusterUpgradeStart",
    "CurrentClusterVersion": "0.0.0.0:",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeType": "Rolling",
    "RollingUpgradeMode": "UnmonitoredAuto",
    "FailureAction": "Manual",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:18:59.4313064Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(0 1 2)",
    "UpgradeDomainElapsedTimeInMs": "78.5288",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:19:59.5729953Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeDomainComplete",
    "TargetClusterVersion": "6.2:1.0",
    "UpgradeState": "RollingForward",
    "UpgradeDomains": "(3 4)",
    "UpgradeDomainElapsedTimeInMs": "0",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.6271949Z",
    "HasCorrelatedEvents": false
  },
  {
    "Kind": "ClusterUpgradeComplete",
    "TargetClusterVersion": "6.2:1.0",
    "OverallUpgradeElapsedTimeInMs": "120196.5212",
    "EventInstanceId": "090add3c-8f56-4d35-8d57-a855745b6064",
    "TimeStamp": "2018-04-03T20:20:59.8134457Z",
    "HasCorrelatedEvents": false
  }
]
```

Aqui, pode ver que entre `2018-04-03T18:00:00Z` e `2018-04-04T18:00:00Z`, este cluster concluída com êxito a primeira atualização quando este foi stood pela primeira vez, dos `"CurrentClusterVersion": "0.0.0.0:"` para `"TargetClusterVersion": "6.2:1.0"`, na `"OverallUpgradeElapsedTimeInMs": "120196.5212"`.

## <a name="query-the-eventstore-programmatically"></a>Consultar o EventStore através de programação

Também pode consultar o EventStore programaticamente, através de [biblioteca de cliente de Service Fabric](https://docs.microsoft.com/dotnet/api/overview/azure/service-fabric?view=azure-dotnet#client-library).

Assim que tiver o cliente de recursos de infraestrutura de serviço configurado, pode consultar eventos acedendo EventStore como esta: ` sfhttpClient.EventStore.<request>`

Eis um exemplo de pedido para o cluster todos os eventos entre `2018-04-03T18:00:00Z` e `2018-04-04T18:00:00Z`, através de `GetClusterEventListAsync` função.

```csharp
var sfhttpClient = ServiceFabricClientFactory.Create(clusterUrl, settings);

var clstrEvents = sfhttpClient.EventsStore.GetClusterEventListAsync(
    "2018-04-03T18:00:00Z",
    "2018-04-04T18:00:00Z")
    .GetAwaiter()
    .GetResult()
    .ToList();
```

## <a name="sample-scenarios-and-queries"></a>Cenários de exemplo e consultas

Seguem-se alguns exemplos sobre como pode chamar as APIs REST do arquivo de eventos para compreender o estado do cluster.

1. Atualizações do cluster:

Para ver a última vez que o cluster foi com êxito ou foi efetuada uma tentativa ser actualizado última semana, pode consultar as APIs para recentemente completadas atualizações para o cluster, consultando os eventos de "ClusterUpgradeComplete" no EventStore: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ClusterUpgradeComplete`

1. Problemas de atualização de cluster:

Da mesma forma, se existirem problemas com uma atualização recente do cluster, poderia consultar todos os eventos para a entidade de cluster. Verá vários eventos, incluindo o início de atualizações e cada UD para os quais a atualização revertida através de com êxito. Também poderá ver eventos para o ponto em que a reversão iniciado e eventos de estado de funcionamento de correspondente. Segue-se a consulta que pretende utilizar para este: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

1. Alterações de estado do nó:

Para ver que o estado do nó alterações ao longo do último alguns dias - quando nós correu ou reduzir verticalmente, foram ativados ou desativados (pela plataforma do, o serviço de chaos, ou de entrada do utilizador) - utilizam a seguinte consulta: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Nodes/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

1. Eventos da aplicação:

Também pode controlar o recente implementações de aplicações e atualizações. Utilize a seguinte consulta para ver todas as aplicações relacionadas com eventos do cluster: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z`

1. Estado de funcionamento histórico para uma aplicação:

Para além de ver apenas os eventos de ciclo de vida de aplicações, também poderá ver dados históricos sobre o estado de funcionamento de uma aplicação específica. Pode fazê-lo especificando o nome da aplicação para o qual pretende recolher os dados. Utilize esta consulta para obter todos os eventos de estado de funcionamento de aplicação: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Applications/myApp/$/Events?api-version=6.2-preview&starttimeutc=2018-03-24T17:01:51Z&endtimeutc=2018-03-29T17:02:51Z&EventsTypesFilter=ProcessApplicationReport`. Se pretender incluir eventos de estado de funcionamento que podem ter expirado (removido transmitido o respectivo período para (TTL) em direto), adicionar `,ExpiredDeployedApplicationEvent` ao fim da consulta, para filtrar dois tipos de eventos.

1. Estado de funcionamento histórico para todos os serviços no "myApp":

Atualmente, eventos de relatório de estado de funcionamento para serviços apresentados como `DeployedServiceHealthReportCreated` eventos sob a entidade correspondente da aplicação. Para ver como os serviços de tem sido fazer para "App1", utilize a seguinte consulta: `https://winlrc-staging-10.southcentralus.cloudapp.azure.com:19080/EventsStore/Applications/myapp/$/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=DeployedServiceHealthReportCreated`

1. Reconfiguração da partição:

Para ver todos os movimentos de partição que tenham acontecido no seu cluster de consultar o `ReconfigurationCompleted` eventos. Isto pode ajudar a descobrir o que cargas de trabalho foi executado no nó que em alturas específicas, quando diagnosticar problemas no seu cluster. Segue-se uma consulta de exemplo que que: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Partitions/Events?api-version=6.2-preview&starttimeutc=2018-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=PartitionReconfigurationCompleted`

1. Serviço de Chaos:

Não há um evento para quando o Chaos o serviço foi iniciado ou parado ou seja exposto ao nível do cluster. Para ver utilização recente do serviço Chaos, utilize a seguinte consulta: `https://mycluster.cloudapp.azure.com:19080/EventsStore/Cluster/Events?api-version=6.2-preview&starttimeutc=2017-04-22T17:01:51Z&endtimeutc=2018-04-29T17:02:51Z&EventsTypesFilter=ChaosStarted,ChaosStopped`

