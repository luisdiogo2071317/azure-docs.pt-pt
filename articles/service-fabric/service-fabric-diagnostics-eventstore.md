---
title: Arquivo de eventos de recursos de infraestrutura de serviço do Azure | Microsoft Docs
description: Saiba mais sobre EventStore do recurso de infraestrutura de serviço do Azure
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
ms.openlocfilehash: 7e401a310ce9a5f59473353227a9ce36767aac3c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/28/2018
---
# <a name="eventstore-service-overview"></a>Descrição geral do serviço EventStore

>[!NOTE]
>A partir do Service Fabric versão 6.2. as APIs de EventStore estão atualmente em pré-visualização para os clusters do Windows em execução no Azure apenas. Estamos a trabalhar transferências esta funcionalidade para o Linux, bem como as nossas clusters autónomos.

## <a name="overview"></a>Descrição geral

Introduzida na versão 6.2, o serviço de EventStore é uma opção de monitorização no Service Fabric, que fornece uma maneira de compreender o estado do seu cluster ou de cargas de trabalho num determinado ponto no tempo. O serviço de EventStore expõe eventos de Service Fabric através de APIs que pode efetuar chamadas. Estas APIs EventStore permitem-lhe consultar o cluster diretamente para obter dados de diagnóstico sobre qualquer entidade no seu cluster e deve ser utilizada para o ajudar:
* Diagnosticar problemas no desenvolvimento ou teste, ou onde pode estar a utilizar um pipeline de monitorização
* Confirme que as ações de gestão que está a efetuar no seu cluster estão a ser processadas corretamente por cluster

Para ver uma lista completa dos eventos disponíveis no EventStore, consulte [eventos de Service Fabric](service-fabric-diagnostics-event-generation-operational.md).

O serviço de EventStore pode ser consultado para os eventos que estão disponíveis para cada entidade e tipo de entidade no seu cluster. Isto significa que pode consultar eventos nos seguintes níveis de;
* Cluster: todos os eventos ao nível do cluster
* Nós: as todos os eventos ao nível de nó
* Nó: eventos específicos para um nó, com base no `nodeName`
* Aplicações: as todos os eventos ao nível de aplicação
* Aplicação: eventos específicos para uma aplicação
* Serviços: eventos de todos os serviços nos seus clusters
* Serviço: eventos de um serviço específico
* Partições: eventos de todas as partições
* Partição: eventos a partir de uma partição específica
* Réplicas: eventos de todas as réplicas / instâncias
* Réplica: eventos uma réplica específico / instância


O serviço de EventStore tem também a capacidade para correlacionar eventos no seu cluster. Ao observar os eventos que foram escritos em simultâneo de entidades diferentes que podem ter afetado entre si, o serviço de EventStore é capaz de ligar estes eventos para ajudar a identificar causas para atividades no seu cluster. Por exemplo, se uma das suas aplicações acontecer fique danificado sem quaisquer alterações induzidas, o EventStore olhos também outros eventos expostos pela plataforma do e vai foi correlacionar isto com um `NodeDown` eventos. Isto ajuda-o com a deteção de falha mais rápida e raiz faz com que o Analysis Services.

Para começar a utilizar o serviço de EventStore, consulte o artigo [consulta EventStore APIs para eventos de cluster](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Passos Seguintes
* Descrição geral da monitorização e diagnóstico no Service Fabric - [monitorização e diagnóstico para o Service Fabric](service-fabric-diagnostics-overview.md)
* Saiba mais sobre como monitorizar o seu cluster- [monitorização do cluster e a plataforma](service-fabric-diagnostics-event-generation-infra.md).