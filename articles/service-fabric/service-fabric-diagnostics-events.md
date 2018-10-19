---
title: Eventos de recursos de infraestrutura do serviço do Azure | Documentos da Microsoft
description: Saiba mais sobre os eventos do Service Fabric vêm prontos para o ajudar a monitorizar o seu cluster do Azure Service Fabric.
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
ms.openlocfilehash: ca63d67f6d7c19b4ca6928c4cc0f9ccb06eace2b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/18/2018
ms.locfileid: "49402986"
---
# <a name="service-fabric-events"></a>Eventos do Service Fabric 

A plataforma do Service Fabric escreve vários eventos estruturados para as atividades operacionais chave acontecendo dentro do cluster. Estas variam desde atualizações de cluster para decisões de colocação de réplica. Cada evento que o Service Fabric expõe é mapeado para uma das seguintes entidades do cluster:
* Cluster
* Aplicação
* Serviço
* Partição
* Réplica 
* Contentor

Para ver uma lista completa dos eventos expostos pela plataforma - [eventos da lista de Service Fabric](service-fabric-diagnostics-event-generation-operational.md).

Aqui estão alguns exemplos de cenários importantes que deve ver eventos no cluster. 
1. Eventos de ciclo de vida de nó: como nós surgem, vá para baixo, são ativada/desativada ou reiniciados, eventos vão ser expostos mostrando a o que aconteceu e ajudar a identificar se existe um problema com a própria máquina ou se tiver ocorrido uma API que foi chamada por meio de SF para modificar o estado de um nó.
1. Atualização do cluster: como o cluster é atualizado (SF versão ou configuração de alteração), verá a atualização iniciar, implementar através de cada um dos seus domínios de atualização e concluir (ou reversão). 
1. As atualizações de aplicações: da mesma forma em cluster atualiza, há um conjunto abrangente de eventos, como a atualização é aplicada nos. Esses eventos podem ser útil para entender quando uma atualização foi agendada, o estado atual de uma atualização e a sequência geral de eventos. Isto é útil para observar novamente para ver o que as atualizações têm foi implementadas com êxito.
1. Implementação de aplicações/serviços / eliminação: existem eventos para cada aplicação, o serviço e o contentor, que está a ser criada ou eliminada.
1. Partição move (reconfiguração): sempre que uma partição com monitoração de estado fica através de uma reconfiguração (uma alteração no conjunto de réplica), um evento é registado. Isto é útil se estiver a tentar compreender a frequência com que seu conjunto de réplicas de partição está mudando ou controlar o nó que estava a ser executada a réplica primária a qualquer momento no tempo.
1. Eventos de Chaos: ao utilizar o Service Fabric [Chaos](service-fabric-controlled-chaos.md) serviço, verá eventos sempre que o serviço é iniciado ou parado, ou quando ele injeta uma falha no sistema.
1. Eventos de estado de funcionamento: O Service Fabric expõe eventos de estado de funcionamento sempre que é criado um aviso ou um relatório de estado de funcionamento de erro, ou uma entidade tem a ver com um Estado de funcionamento OK ou um relatório de estado de funcionamento expira. Esses eventos são muito úteis para controlar as estatísticas de históricos de estado de funcionamento de uma entidade. 

## <a name="how-to-access-events"></a>Como os eventos de acesso

Existem algumas formas diferentes através do qual os eventos do Service Fabric podem ser acessados:
* através do canal operacional. Estes podem ser recolhidos através da extensão de diagnóstico do Azure e enviadas para uma tabela de armazenamento para consumo ou uma ingestão numa ferramenta como o Azure Log Analytics. Quando "diagnóstico" está ativado para um cluster, o agente de diagnóstico do Azure é implementado num cluster e é por predefinição, configuradas para ler nos registos do canal operacional. Leia mais sobre como configurar o [agente do diagnóstico do Azure](service-fabric-diagnostics-event-aggregation-wad.md) para modificar a configuração de diagnósticos do seu cluster para recolher mais registos ou contadores de desempenho. 
* através de APIs de Rest do serviço de EventStore que permitem-lhe consultar o cluster diretamente ou por meio da biblioteca de cliente de recursos de infraestrutura do serviço. Ver [APIs de EventStore de consulta para eventos de cluster](service-fabric-diagnostics-eventstore-query.md).

## <a name="next-steps"></a>Passos Seguintes
* Obter mais informações na monitorização do seu cluster - [monitorizar o cluster e a plataforma](service-fabric-diagnostics-event-generation-infra.md).
* Saiba mais sobre o serviço de EventStore - [descrição geral do serviço de EventStore](service-fabric-diagnostics-eventstore.md)
