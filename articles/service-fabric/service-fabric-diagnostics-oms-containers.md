---
title: Monitorizar contentores no Azure Service Fabric com o Log Analytics | Microsoft Docs
description: Utilize a análise de registos para monitorização contentores executar em clusters de Service Fabric do Azure.
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
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: 79d30a47b017379107b63b0006a35534f68c43b9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/16/2018
---
# <a name="monitor-containers-with-log-analytics"></a>Monitor de contentores com a análise de registos
 
Este artigo aborda os passos necessários para configurar o contentor de análise de registos do OMS solução de monitorização para ver eventos de contentor. Para configurar o cluster para recolher eventos de contentor, consulte este [tutorial passo a passo](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>Configurar o solução de monitorização do contentor

> [!NOTE]
> Tem de ter definido de análise de registos a cópia de segurança para o cluster, bem como ter o agente do OMS implementados nos nós. Se não, siga os passos no [configurar a análise de registos](service-fabric-diagnostics-oms-setup.md) e [adicionar o agente do OMS a um cluster](service-fabric-diagnostics-oms-agent.md) primeiro.

1. Depois do cluster está configurado com a análise de registos e o agente do OMS, implemente os contentores. Aguarde que os contentores ser implementada antes de mover para o passo seguinte.

2. No Azure Marketplace, procure *solução de monitorização do contentor* e clique em de **solução de monitorização do contentor** recurso que aparece na monitorização + gestão categoria.

    ![Adicionar a solução de Contentores](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Crie a solução no interior da área de trabalho mesma que já tiver sido criada para o cluster. Esta alteração aciona automaticamente o agente para iniciar a recolha de dados de docker nos contentores. Em cerca de 15 minutos ou por isso, deverá ver a solução leve cópias de segurança com registos de entrada e de estatísticas. conforme mostrado na imagem abaixo.

    ![Dashboard do OMS básico](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

O agente permite a recolha de vários registos de contentor específico que pode ser consultada na OMS, ou utilizados para indicadores de desempenho visualizadas. Os tipos de registo que são recolhidos são:

* ContainerInventory: mostra informações sobre imagens, o nome e localização do contentor
* ContainerImageInventory: informações sobre imagens de implementada, incluindo os IDs ou tamanhos
* ContainerLog: registos de erro específico, registos de docker (stdout, etc.) e outras entradas
* ContainerServiceLog: comandos de daemon de docker que foram executados
* Desempenho: os contadores de desempenho incluindo contentor cpu, memória, o tráfego de rede do disco e/s e métricas personalizadas as máquinas de anfitrião



## <a name="next-steps"></a>Passos Seguintes
* Saiba mais sobre [solução de contentores do OMS](../log-analytics/log-analytics-containers.md).
* Saiba mais sobre a orquestração do contentor no Service Fabric - [Service Fabric e os contentores](service-fabric-containers-overview.md)
* Obter familiarized com o [de registo de pesquisa e consultar](../log-analytics/log-analytics-log-searches.md) funcionalidades disponibilizadas como parte da análise de registos
* Configurar a análise de registos para configurar [automatizada alertas](../log-analytics/log-analytics-alerts.md) regras para ajudar a detetar e diagnóstico