---
title: "Analisar padrões de utilização do Azure CDN | Microsoft Docs"
description: "Cliente pode ativar a análise de registos para a CDN do Azure."
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2017
ms.author: v-semcev
ms.openlocfilehash: ce9ec021250a3548e23ad87273a9225cdf700c70
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/06/2017
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analisar padrões de utilização da CDN do Azure

Depois de ativar a CDN para a sua aplicação, pode monitorizar a utilização da CDN, verifique o estado de funcionamento do seu entrega e potenciais problemas de resolução de problemas. CDN do Azure fornece mais capacidades das seguintes formas: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Análise de núcleo através de registos de diagnóstico do Azure

Análise de núcleo está disponível para todos os pontos finais da CDN que pertencem da Verizon (Standard e Premium) e perfis da CDN (Standard) Akamai. Registos de diagnóstico do Azure permitem a análise de núcleo ser exportados para o armazenamento do Azure, os event hubs ou análise de registos do Operations Management Suite (OMS). Análise de registos do OMS oferece uma solução com gráficos configurável pelo utilizador e personalizáveis. Para obter mais informações, consulte [registos de diagnóstico do Azure](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Relatórios de núcleos da Verizon

Como um utilizador CDN do Azure com uma standard da Verizon ou um perfil de premium da Verizon, pode ver relatórios de núcleos da Verizon no portal suplementar da Verizon. Relatórios de núcleos da Verizon está acessível através de **gerir** opção do portal do Azure e oferece uma variedade de vistas e gráficos. Para obter mais informações, consulte [relatórios de núcleos da Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Relatórios personalizados da Verizon

Como um utilizador CDN do Azure com uma standard da Verizon ou um perfil de premium da Verizon, pode ver relatórios personalizados da Verizon no portal suplementar da Verizon. Relatórios personalizados da Verizon está acessível através de **gerir** opção do portal do Azure. A página de relatórios personalizados da Verizon mostra o número de pedidos ou dados transferido para cada contorno CName que pertencem a um perfil de CDN do Azure. Os dados podem ser agrupados por Estado de cache ou de código de resposta HTTP durante qualquer período de tempo. Para obter mais informações, consulte [relatórios personalizados da Verizon](cdn-verizon-custom-reports.md).

## <a name="verizon-premium-reports"></a>Relatórios de premium da Verizon

Com **CDN do Azure Premium da Verizon**, também pode aceder os relatórios seguintes:
   * [Relatórios HTTP avançados](cdn-advanced-http-reports.md)
   * [Estatísticas em tempo real](cdn-real-time-stats.md)
   * [Desempenho do nó de extremidade](cdn-edge-performance.md)

