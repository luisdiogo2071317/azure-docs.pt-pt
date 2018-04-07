---
title: Sobre a monitorização de rede na análise de registos | Microsoft Docs
description: Descrição geral de soluções, incluindo NPM, para gerir redes em ambientes de nuvem, no local e híbridos da monitorização de rede.
services: monitoring-and-diagnostics
documentationcenter: na
author: agummadi
manager: ''
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2018
ms.author: ajaycode
ms.openlocfilehash: 306d0e57449de41080d5473034e585f772771d51
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 04/06/2018
---
# <a name="network-monitoring-solutions"></a>Soluções de monitorização de rede 

O Azure disponibiliza um anfitrião de soluções para monitorizar os recursos de rede. O Azure tem soluções e utilitários para monitorizar a conectividade de rede, o estado de funcionamento dos circuitos do ExpressRoute e analisar o tráfego de rede na nuvem.

## <a name="network-performance-monitor-npm"></a>Monitor de desempenho de rede (NPM)

Monitor de desempenho de rede (NPM) é um conjunto de capacidades, cada um dos quais é adaptada para monitorizar o estado de funcionamento da sua rede, a conectividade de rede para as suas aplicações e fornece informações sobre o desempenho da rede. NPM é baseado na nuvem e fornece uma solução que monitoriza a conectividade entre da monitorização de rede híbrida:
 
* Localizações no local e de implementações de nuvem
* Vários centros de dados e sucursais
* Missão críticos multicamados aplicações/microserviços
* Localizações de utilizador e as aplicações baseadas na web (HTTP/HTTPs) 

Monitor de desempenho, o Monitor de ExpressRoute e o Monitor de ponto final de serviço estão a monitorizar capacidades dentro NPM e são descritos abaixo.

## <a name="performance-monitor"></a>Monitorização de Desempenho

Monitor de desempenho faz parte de NPM e é a monitorização de rede para nuvens, híbridas e ambientes no local. Pode monitorizar a conectividade de rede através de sucursais remotas e escritórios de campo, localizações de arquivo, os centros de dados e nuvens. Pode detetar problemas de rede antes dos utilizadores queixarem. As vantagens de chaves são:

* Monitorizar a perda e latência em várias sub-redes e conjunto de alertas
* Monitorizar todos os caminhos (incluindo caminhos redundantes) na rede
* Resolver problemas de rede transitórios e ponto no tempo, que são difíceis de replicar
* Determinar o segmento específico na rede, que é responsável por degradação do desempenho
* Monitorizar o estado de funcionamento da rede, sem a necessidade de SNMP

![Mapa de topologia NPM](./media/network-monitoring-overview/npm-topology-map.png) 

Para obter mais informações, veja os artigos seguintes:

* [Configurar uma solução de monitorização de desempenho de rede na análise de registos](../log-analytics/log-analytics-network-performance-monitor.md) 
* [Casos de utilização](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/)
*  Atualizações do produto: [Fevereiro de 2017](https://blogs.technet.microsoft.com/msoms/2017/02/27/oms-network-performance-monitor-is-now-generally-available/), [de 2017 de Agosto](https://blogs.technet.microsoft.com/msoms/2017/08/14/improvements-to-oms-network-performance-monitor/)

## <a name="expressroute-monitor"></a>ExpressRoute Monitor

NPM para o ExpressRoute oferece monitorização ExpressRoute completa para ligações de peering privadas. Pode monitorizar o desempenho entre as filiais e o Azure e conectividade de E2E através do ExpressRoute. Principais capacidades são:

* Deteção automática de circuitos ER associados à subscrição
* Deteção de topologia de rede no local para as suas aplicações na nuvem
* O planeamento de capacidade, análise da utilização de utilização de largura de banda por rede Virtual
* Monitorização e alertas sobre os caminhos principais e secundários
* Detetar degradação de conectividade para VNets

![Tráfego de apresentação do mapa Georreplicação em regiões](./media/network-monitoring-overview/expressroute-topology-map.png) 

Para obter mais informações, veja os artigos seguintes:

* [Configurar o Monitor de Desempenho de Rede para o ExpressRoute](../expressroute/how-to-npm.md)
* [mensagem de blogue](https://aka.ms/NPMExRmonitorGA)

## <a name="service-endpoint-monitor"></a>Monitor de ponto final de serviço

Com a monitorização do ponto final de serviço, pode agora testar reachability das aplicações e detetar congestionamentos de desempenho no local, redes de carrier e centros de dados de nuvem/privadas.

* Monitorizar a conectividade de rede ponto a ponto para aplicações
* Correlacionar entrega de aplicações com o desempenho da rede, detetar localização exatos de degradação pelo caminho entre o utilizador e a aplicação
* Acessibilidade de aplicação de teste a partir de várias localizações de utilizador em todo o mundo
* Determinar a perda de latência e pacotes de rede para a linha de negócio e aplicações SaaS
* Determinar as oportunidades de frequente na rede, que poderá estar a causar o desempenho da aplicação fraco
* Monitorizar reachability para aplicações do Office 365, utilizando os testes incorporados para o Microsoft Office 365, Dynamics 365, Skype para empresas e outros serviços Microsoft

Para obter mais informações, veja os artigos seguintes:

* [Configurar o Monitor de desempenho de rede para a monitorização de pontos finais de serviço](https://aka.ms/applicationconnectivitymonitorguide)
* [mensagem de blogue](https://aka.ms/svcendptmonitor)

## <a name="traffic-analytics"></a>Análise de Tráfego
Análise de tráfego é uma solução baseada na nuvem que fornece visibilidade para a atividade de utilizador e da aplicação nas redes em nuvem. Fluxo de NSG registos serem analisados para fornecer informações sobre:

* Fluxos de tráfego entre as redes entre o Azure e Internet, regiões de nuvem pública, VNETs e sub-redes
* As aplicações e os protocolos na sua rede, sem a necessidade de sniffers ou aparelhos de recoletor de fluxo dedicado
* Talkers superiores, aplicações chatty, conversações de VM na nuvem, hotspots de tráfego
* As origens e destinos de tráfego entre as VNETs, entre as relações entre aplicações e serviços críticos de negócio
* Segurança – tráfego malicioso, portas abertas para a Internet, aplicações ou VMs tentativa de acesso à Internet...
* Utilização de capacidade - ajuda a eliminar os problemas de sobre-aprovisionar ou uma subutilização pela monitorização de tendências de utilização de gateways de VPN e outros serviços

Análise de tráfego equips-lhe informações acionáveis, que ajuda a auditoria da sua organização atividade de rede, proteger aplicações e dados, otimizar o desempenho da carga de trabalho e a manter em conformidade.

![Tráfego de apresentação do mapa Georreplicação em regiões](../network-watcher/media/traffic-analytics/geo-map-view-showcasing-traffic-distribution-to-countries-and-continents.png) 

Ligações relacionadas:
* [Mensagem de blogue](https://aka.ms/trafficanalytics), [documentação](https://aka.ms/trafficanalyticsdocs), [FAQ](https://docs.microsoft.com/azure/network-watcher/traffic-analytics-faq)

## <a name="dns-analytics"></a>Análise de DNS
Incorporado para os administradores de DNS, esta solução recolhe, analisa e está correlacionada com registos DNS para fornecer segurança, operações e informações relacionadas com o desempenho.  Algumas das capacidades são:

* Identificação de clientes que tentam resolver a domínios maliciosos
* Identificação dos registos de recursos obsoletos
* Visibilidade frequentemente consultado nomes de domínio e talkative clientes DNS
* Visibilidade para o pedido de carga nos servidores DNS
* Monitorização de falhas de registo DNS dinâmicas

![Dashboard de análise DNS](./media/network-monitoring-overview/dns-analytics-overview.png) 

Ligações relacionadas:
* [Mensagem de blogue](https://blogs.technet.microsoft.com/msoms/2017/04/19/introducing-oms-dns-analytics/), [documentação](https://docs.microsoft.com/azure/log-analytics/log-analytics-dns)

## <a name="miscellaneous"></a>Diversos

* [Novos preços](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor-pricing-faq)
