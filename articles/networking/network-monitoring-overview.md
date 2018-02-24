---
title: "Sobre a monitorização de rede na análise de registos | Microsoft Docs"
description: "Descrição geral de soluções, incluindo NPM, para gerir redes em ambientes de nuvem, no local e híbridos da monitorização de rede."
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: 
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 6793bd8d2c561554213e9fe645aab018dba7a925
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/23/2018
---
# <a name="network-monitoring-solutions"></a>Soluções de monitorização de rede 

O Azure disponibiliza um anfitrião de soluções para monitorizar os recursos de rede. O Azure tem soluções e utilitários para monitorizar a conectividade de rede, o estado de funcionamento dos circuitos do ExpressRoute e analisar o tráfego de rede na nuvem.

## <a name="network-performance-monitor-npm"></a>Monitor de desempenho de rede (NPM)

Monitor de desempenho de rede (NPM) é um conjunto de capacidades, cada um dos quais é adaptada para monitorizar o estado de funcionamento da sua rede, a conectividade de rede para as suas aplicações e fornece informações sobre o desempenho da rede. NPM é baseado na nuvem e fornece uma solução que monitoriza a conectividade entre da monitorização de rede híbrida:
 
* Localizações no local e de implementações de nuvem
* Vários centros de dados e sucursais
* Missão críticos multicamados aplicações/microserviços
* Localizações de utilizador e as aplicações baseadas na web (HTTP/HTTPs) 

## <a name="performance-monitor"></a>Monitor de desempenho

Monitor de desempenho faz parte de NPM e é a monitorização de rede para nuvens, híbridas e ambientes no local. Pode monitorizar a conectividade de rede através de sucursais remotas e escritórios de campo, localizações de arquivo, os centros de dados e nuvens. Pode detetar problemas de rede antes dos utilizadores queixarem. As vantagens de chaves são:

* Monitorizar a perda e latência em várias sub-redes e conjunto de alertas
* Monitorizar todos os caminhos (incluindo caminhos redundantes) na rede
* Resolver problemas de rede transitórios e ponto no tempo, que são difíceis de replicar
* Determinar o segmento específico na rede, que é responsável por degradação do desempenho
* Monitorizar o estado de funcionamento da rede, sem a necessidade de SNMP

Para obter mais informações, veja os artigos seguintes:

* [Configurar uma solução de monitorização de desempenho de rede na análise de registos](../log-analytics/log-analytics-network-performance-monitor.md) 
* [Casos de utilização](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
*  Atualizações do produto: [Fevereiro de 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/), [de 2017 de Agosto](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>ExpressRoute Monitor

NPM para o ExpressRoute oferece monitorização ExpressRoute completa para ligações de peering privadas. Pode monitorizar o desempenho entre as filiais e o Azure e conectividade de E2E através do ExpressRoute. Principais capacidades são:

* Deteção automática de circuitos ER associados à subscrição
* Deteção de topologia de rede no local para as suas aplicações na nuvem
* Planeamento de análise da utilização da capacidade
* Monitorização e alertas sobre os caminhos principais e secundários
* Detetar degradação de conectividade para VNets

Para obter mais informações, veja os artigos seguintes:

* [Configurar o Monitor de Desempenho de Rede para o ExpressRoute](../expressroute/how-to-npm.md)
* [mensagem de blogue](https://aka.ms/NPMExRmonitorGA)

## <a name="next-steps"></a>Passos Seguintes

* [Configurar o Monitor de desempenho de rede](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor)
* [Configurar o Monitor de Desempenho de Rede para o ExpressRoute](../expressroute/how-to-npm.md)