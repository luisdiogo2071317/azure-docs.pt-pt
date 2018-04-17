---
title: Análise de eventos de recursos de infraestrutura de serviço do Azure com a análise de registos | Microsoft Docs
description: Saiba mais sobre como visualizar e analisar eventos utilizando a análise de registos para monitorização e diagnóstico de clusters de Service Fabric do Azure.
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
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 290b1d594cc1f874bcfdd0cef728fc78af96f702
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/16/2018
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Análise de eventos e visualização de análise de registos

Soluções de gestão do Azure são uma coleção de serviços de gestão que ajudam na monitorização e diagnóstico para aplicações e serviços alojados na nuvem. Para obter uma descrição mais detalhada de análise de registos e o que oferece, leia [o que é a análise de registos?](../operations-management-suite/operations-management-suite-overview.md)

## <a name="log-analytics-workspace"></a>Área de trabalho do Log Analytics

Análise de registos recolhe dados a partir dos recursos geridos, incluindo uma tabela de armazenamento do Azure ou um agente e mantém-lo num repositório central. Os dados, em seguida, podem ser utilizado para análise, alertas e visualização ou ainda mais a exportar. Análise de registos suporta eventos, dados de desempenho ou quaisquer outros dados personalizados.

Quando a análise de registos está configurado, terá acesso ao específico *área de trabalho de análise de registos*, a partir de onde os dados podem ser consultados ou visualizados nos dashboards.

Depois de dados são recebidos pelo análise de registos, o Azure tem vários *soluções de gestão* que são prepackaged soluções para monitorizar os dados de entrada, personalizados para vários cenários. Estes incluem uma *análise de recursos de infraestrutura de serviço* solução e um *contentores* solução, o que são os dois mais relevante para diagnóstico e monitorização quando utilizar clusters de Service Fabric. Existem várias outras bem que são vale a explorar e análise de registos também permite a criação de soluções personalizadas, que pode ler mais sobre [aqui](../operations-management-suite/operations-management-suite-solutions.md). Cada solução que pretenda utilizar para um cluster pode ser configurada na mesma área de análise de registos, juntamente com a análise de registos. Áreas de trabalho permitem dashboards personalizados e visualização de dados e as modificações de dados que pretende recolher, processo e analisar.

## <a name="setting-up-a-log-analytics-workspace-with-the-service-fabric-analytics-solution"></a>Configurar uma área de trabalho de análise de registos com a solução de análise de recursos de infraestrutura de serviço
É recomendado que incluem a solução de recursos de infraestrutura de serviço na sua área de trabalho de análise de registos - inclui um dashboard que mostra os vários canais de registo recebidos do nível da plataforma e a aplicação e fornece a capacidade de consulta específico do Service Fabric registos. Eis o aspeto de uma solução de recursos de infraestrutura de serviço relativamente simples, com uma única aplicação implementada num cluster:

![Solução SF do OMS](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-solution.png)

Consulte [configurar a análise de registos](service-fabric-diagnostics-oms-setup.md) para começar a utilizar com esta opção para o cluster.

## <a name="using-the-oms-agent"></a>Com o agente do OMS

É recomendado utilizar EventFlow e WAD como soluções de agregação porque permitem uma abordagem mais modulares para diagnóstico e monitorização. Por exemplo, não se pretender alterar as saídas da EventFlow, requer nenhuma alteração ao seu instrumento real, apenas uma modificação simple ao ficheiro de configuração. Se, no entanto, optar por investir em através da análise do registo, deve configurar o [agente do OMS](../log-analytics/log-analytics-windows-agent.md). Deve utilizar também o agente do OMS quando implementar contentores para o cluster, conforme mostrado abaixo. 

Vá ao longo para [adicionar o agente do OMS a um cluster](service-fabric-diagnostics-oms-agent.md) para obter os passos neste.

As vantagens deste são os seguintes:

* Dados mais rico no lado de métricas e contadores de desempenho
* Fácil de configurar as métricas que estão a ser recolhidas do cluster e sem ter de atualizar a configuração do cluster. Alterações às definições do agente podem ser feitas a partir do portal do OMS e o agente é reiniciado automaticamente para corresponder a configuração necessária. Para configurar o agente do OMS para recolher contadores de desempenho específica, aceda à área de trabalho **home page > Definições > dados > contadores de desempenho do Windows** e escolha os dados que gostaria de ver recolhidos
* Dados aparecem mais rapidamente do que o se ter de ser armazenados antes captado através da análise de registos
* Monitorização contentores é mais fácil, uma vez que os pode recolher registos de docker (stdout, stderr) e estatísticas (métricas de desempenho nos níveis de contentor e nó)

A principal consideração aqui é que uma vez que o agente será implementado no seu cluster juntamente com as suas aplicações, poderão existir alguns impacto para o desempenho das suas aplicações no cluster.

## <a name="monitoring-containers"></a>Monitorização contentores

Quando implementar contentores para um cluster do Service Fabric, recomenda-se que o cluster foi configurado com o agente do OMS e que a solução de contentores foi adicionada à sua área de trabalho de análise de registos para ativar a monitorização e diagnóstico. Eis o aspeto a solução de contentores numa área de trabalho:

![Dashboard do OMS básico](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

O agente permite a recolha de vários registos de contentor específico que pode ser consultada na análise de registos ou utilizado para indicadores de desempenho visualizadas. Os tipos de registo que são recolhidos são:

* ContainerInventory: mostra informações sobre imagens, o nome e localização do contentor
* ContainerImageInventory: informações sobre imagens de implementada, incluindo os IDs ou tamanhos
* ContainerLog: registos de erro específico, registos de docker (stdout, etc.) e outras entradas
* ContainerServiceLog: comandos de daemon de docker que foram executados
* Desempenho: os contadores de desempenho incluindo contentor cpu, memória, o tráfego de rede do disco e/s e métricas personalizadas as máquinas de anfitrião

[Monitorizar contentores com a análise de registos](service-fabric-diagnostics-oms-containers.md) aborda os passos necessários para configurar a monitorização do contentor para o cluster. Para saber mais sobre a solução de contentores de análise de registos, consulte os respetivos [documentação](../log-analytics/log-analytics-containers.md).

## <a name="next-steps"></a>Passos Seguintes

Explore as seguintes ferramentas de análise de registos e as opções para personalizar uma área de trabalho para as suas necessidades:

* Para os clusters no local, a análise de registos oferece um Gateway (reencaminhar Proxy HTTP) que pode ser utilizado para enviar dados para análise de registos. Leia mais sobre o que no [ligar os computadores sem acesso à Internet ao Log Analytics utilizando o Gateway do OMS](../log-analytics/log-analytics-oms-gateway.md)
* Configurar a análise de registos para configurar [automatizada alertas](../log-analytics/log-analytics-alerts.md) para ajudar a detetar e diagnóstico
* Obter familiarized com o [de registo de pesquisa e consultar](../log-analytics/log-analytics-log-searches.md) funcionalidades disponibilizadas como parte da análise de registos