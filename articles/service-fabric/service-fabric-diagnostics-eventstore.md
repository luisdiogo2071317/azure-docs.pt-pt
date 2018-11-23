---
title: Store de eventos do Azure Service Fabric | Documentos da Microsoft
description: Saiba mais sobre o Azure Service Fabric EventStore
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: cd58e24a51b153d6bf217f7d32a82e882183ed73
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/22/2018
ms.locfileid: "52290676"
---
# <a name="eventstore-service-overview"></a>Descrição geral do serviço de EventStore

## <a name="overview"></a>Descrição geral

Introduzida na versão 6.2, o serviço de EventStore é uma opção de monitorização no Service Fabric, que fornece uma maneira para que possa compreender o estado do seu cluster ou cargas de trabalho num determinado ponto no tempo. O serviço de EventStore expõe eventos do Service Fabric através de um conjunto de APIs (acessíveis por meio de pontos finais REST ou por meio da biblioteca de cliente). Estas APIs de EventStore permitem-lhe consultar o cluster diretamente para obter dados de diagnóstico sobre qualquer entidade no seu cluster e deve ser utilizado para o ajudar a:

* Diagnosticar problemas em desenvolvimento ou teste, ou onde poderá estar a utilizar um pipeline de monitorização
* Confirme que as ações de gestão que está a efetuar no seu cluster estão a ser processadas corretamente ao seu cluster

Para ver uma lista completa de eventos disponíveis no EventStore, consulte [eventos do Service Fabric](service-fabric-diagnostics-event-generation-operational.md).

>[!NOTE]
>A partir do Service Fabric versão 6.2. as APIs de EventStore estão atualmente em pré-visualização para clusters do Windows em execução no Azure apenas. Estamos a trabalhar no portar essa funcionalidade para Linux, bem como nossos clusters autónomos.

O serviço de EventStore pode ser consultado para os eventos que estão disponíveis para cada entidade e tipo de entidade no seu cluster. Isso significa que pode consultar os eventos nos seguintes níveis;
* Cluster: todos os eventos de nível de cluster
* Nós: as todos os eventos ao nível de nó
* Nó: eventos específicos para um nó, com base em `nodeName`
* Aplicações: as todos os eventos ao nível de aplicativo
* Aplicação: eventos específicos a um aplicativo
* Serviços: eventos de todos os serviços nos seus clusters
* Serviço: eventos um serviço específico
* Partições: eventos a partir de todas as partições
* Partição: eventos a partir de uma partição específica
* Réplicas: eventos a partir de todas as réplicas / instâncias
* Réplica: eventos a partir de uma réplica específico / instância


O serviço de EventStore também tem a capacidade de correlacionar eventos no seu cluster. Ao examinar os eventos que foram escritos em simultâneo de entidades diferentes que podem ter afetado entre si, o serviço de EventStore é capacidade de vincular esses eventos para o ajudar a identificar causas para as atividades no seu cluster. Por exemplo, se um de seus aplicativos acontecer para se tornar problemático sem quaisquer alterações induzidas, o EventStore será também a análise de outros eventos expostos pela plataforma e pode correlacionar isso com um `NodeDown` eventos. Isto ajuda a com deteção de falhas mais rápida e análise de causa raiz.

Recomendamos utilizar o EventStore para análise rápida e para ter uma idéia de instantâneo de funcionamento do seu cluster e, se as coisas estão acontecendo como esperado.

Para começar a utilizar o serviço de EventStore, consulte [APIs de EventStore de consulta para eventos de cluster](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Passos Seguintes
* Descrição geral da monitorização e diagnóstico no Service Fabric - [monitorização e diagnóstico para o Service Fabric](service-fabric-diagnostics-overview.md)
* Saiba mais sobre a monitorização do seu cluster- [monitorizar o cluster e a plataforma](service-fabric-diagnostics-event-generation-infra.md).