---
title: Eventos de recursos de infraestrutura de serviço do Azure | Microsoft Docs
description: Saiba mais sobre os eventos de Service Fabric fornecidos Box para ajudar a monitorizar o seu cluster do Service Fabric do Azure.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2018
ms.author: dekapur
ms.openlocfilehash: b9372c806eab1b0ca69ba078d972b076c8a7d6f6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="service-fabric-events"></a>Eventos do Service Fabric 

A plataforma de Service Fabric escreve vários eventos estruturados para atividades operacionais chaves a acontecer no interior do seu cluster. Estes intervalo de atualizações de cluster a decisões de colocação de réplica. Cada evento que o Service Fabric expõe mapeado para um dos seguintes entidades do cluster:
* Cluster
* Aplicação
* Serviço
* Partição
* Réplica 
* Contentor

Para ver uma lista completa dos eventos expostos pela plataforma - [eventos de lista de Service Fabric](service-fabric-diagnostics-event-generation-operational.md).

Seguem-se alguns exemplos de cenários importantes que deve ver eventos do cluster. 
1. Eventos de ciclo de vida do nó: como nós de pensar, abaixo, são ativado/desativado ou reiniciados, eventos ficará expostos que mostra o que aconteceu e ajudar a identificar se houver um problema com a máquina em si ou se tiver ocorrido uma API que foi chamada através de SF para modificar o estado de um nó.
1. Atualização do cluster: como o cluster é atualizado (SF versão ou uma configuração alterar), verá a atualização iniciar, reverta através de cada um dos seus UDs e concluir (ou reversão). 
1. As atualizações de aplicações: da mesma forma para o cluster atualiza, existe um conjunto abrangente de eventos como a atualização se faz através de. Estes eventos podem ser útil para entender quando uma atualização foi agendada, o estado atual de uma atualização e a sequência geral de eventos. Isto é útil para procurar novamente para ver que atualizações foram revertidas com êxito.
1. Implementação de aplicações/serviços / eliminação: há eventos para cada aplicação, o serviço e o contentor, que está a ser criada ou eliminada.
1. Partição move (reconfiguração): sempre que uma partição com monitorização de estado passa através de uma reconfiguração (uma alteração no conjunto de réplica), fica registado um evento. Isto é útil se estiver a tentar compreender com que frequência está a alterar o conjunto de réplicas de partição ou controlar nó que estava a ser executada a réplica primária em qualquer ponto no tempo.
1. Eventos de Chaos: ao utilizar o Service Fabric [Chaos](service-fabric-controlled-chaos.md) serviço, verá eventos sempre que o serviço está iniciado ou parado ou quando-injects uma falha no sistema.
1. Eventos de estado de funcionamento: O Service Fabric expõe eventos de estado de funcionamento sempre que é criado um aviso ou um relatório de estado de funcionamento de erro, uma entidade vai volta para um Estado de funcionamento OK ou a um relatório de estado de funcionamento expira. Estes eventos são muito úteis para controlar as estatísticas de histórico do Estado de funcionamento de uma entidade. 

## <a name="how-to-access-events"></a>Como os eventos de acesso

Existem várias formas através do qual podem ser acedidos eventos de Service Fabric:
* através do canal operacional. Estes podem ser recolhidos através da extensão de diagnóstico do Azure e enviados para uma tabela de armazenamento para consumo ou ingestão para uma ferramenta como a análise de registos do OMS. Quando "diagnóstico" está ativado para um cluster, o agente de diagnóstico do Azure está implementado no seu cluster, não sendo por predefinição, configuradas para ler nos registos do canal operacional. Leia mais sobre como configurar o [agente de diagnóstico do Azure](service-fabric-diagnostics-event-aggregation-wad.md) para modificar a configuração de diagnósticos do cluster para recolher mais registos ou os contadores de desempenho. 
* através APIs do serviço EventStore Rest, que permitem-lhe consultar o cluster, diretamente ou através da biblioteca de cliente do serviço de recursos de infraestrutura. Consulte [consulta EventStore APIs para eventos de cluster](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Passos Seguintes
* Obter mais informações na monitorização do seu cluster - [monitorização do cluster e a plataforma](service-fabric-diagnostics-event-generation-infra.md).
* Saiba mais sobre o serviço de EventStore - [EventStore descrição geral do serviço](service-fabric-diagnostics-eventstore.md)
