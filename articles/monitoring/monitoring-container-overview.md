---
title: Descrição geral de contentores do Azure da monitorização | Microsoft Docs
description: Este artigo fornece uma descrição geral dos métodos diferentes disponíveis no Azure para monitorizar os contentores do Azure para perceber rapidamente um Estado de funcionamento de clusters e disponibilidade.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/26/2018
ms.author: magoedte
ms.openlocfilehash: 0d511c1f6dfd482e5754741da15b2852ee77c11e
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/04/2018
ms.locfileid: "33207532"
---
# <a name="overview-of-monitoring-containers-in-azure"></a>Descrição geral da monitorização de contentores no Azure
Com o Azure, pode monitorizar e gerir as cargas de trabalho implementadas nos contentores do Azure em execução Kubernetes ou Docker eficazmente. É importante compreender como contentores com várias aplicações de microsserviço estiver a efetuar para prestar um serviço fiável em escala e suporta o seu plano de monitorização. Este artigo fornece uma breve descrição geral da gestão de e capacidades de monitorização no Azure para ajudar a compreender cada e que são adequados com base nos seus requisitos.

Utilizar [o estado de funcionamento do Monitor de Azure contentor](monitoring-container-health.md), pode ver o desempenho e estado de funcionamento da sua infraestrutura de contentor do Linux rapidamente e investigar problemas rapidamente. A telemetria é armazenado numa área de trabalho de análise de registos e integrado no portal do Azure, onde pode explorar, filtrar e dados com dashboards para atento carga, desempenho e estado de funcionamento de agregados de segmento.  

Para ser executado fora do Azure Kubernetes no serviço alojado, a análise de registos de contentores [solução Windows e contentor de Docker](../log-analytics/log-analytics-containers.md) ajuda a visualizar e gerir os anfitriões de contentor do Windows e do Docker. Da sua área de trabalho de análise de registos, pode ver detalhes de inventário, o desempenho e eventos de nós e contentores no ambiente. Pode ver informações detalhadas de auditoria que mostra comandos utilizados com contentores, e pode resolver problemas de contentores ao visualizar e pesquisar registos centralizados sem ter de aceder remotamente a anfitriões de Docker ou Windows.

Para alcançar holística ou da aplicação de monitorização ponto a ponto, qualquer dependência se não for do Azure ou no local recurso, deve ser monitorizada com o Monitor do Azure ou a análise de registos.  Camada da aplicação deve ser incluída para poder adicionar uma camada adicional de deteção de estado de funcionamento, tanto ao nível da plataforma e a aplicação utilizar o Application Insights. Ao nível da plataforma, há SDKs do Application Insights para [Kubernetes]( https://github.com/Microsoft/ApplicationInsights-Kubernetes), [Docker](https://hub.docker.com/r/microsoft/applicationinsights/), e [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-analysis-appinsights). Para aplicações de microsserviço, não há suporte para [Java](../application-insights/app-insights-java-get-started.md), [Node.js](../application-insights/app-insights-nodejs-quick-start.md), [.Net](../application-insights/app-insights-asp-net.md), [.Net Core](../application-insights/app-insights-asp-net-core.md), bem como um número de outros [idiomas/estruturas](../application-insights/app-insights-platforms.md). 

Caso contrário, passará identificados problemas que pode afetar a disponibilidade da aplicação e destinos de nível de serviço não irão ser cumpridos.  
