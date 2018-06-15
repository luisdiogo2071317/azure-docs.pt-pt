---
title: Analisar padrões de utilização do Azure CDN | Microsoft Docs
description: Este artigo descreve os diferentes tipos de relatórios de análise disponíveis para produtos da CDN do Azure.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 95e18b3c-b987-46c2-baa8-a27a029e3076
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/05/2017
ms.author: rli; v-deasim
ms.openlocfilehash: 61fbe6e29df787048a9694138d3c9095f5cba76b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/07/2018
ms.locfileid: "33764898"
---
# <a name="analyze-azure-cdn-usage-patterns"></a>Analisar padrões de utilização da CDN do Azure

Depois de ativar a CDN para a sua aplicação, pode monitorizar a utilização da CDN, verifique o estado de funcionamento do seu entrega e potenciais problemas de resolução de problemas. CDN do Azure fornece mais capacidades das seguintes formas: 

## <a name="core-analytics-via-azure-diagnostic-logs"></a>Análise de núcleo através de registos de diagnóstico do Azure

Análise de núcleo está disponível para pontos finais da CDN para todos os escalões de preços. Registos de diagnóstico do Azure permitem a análise de núcleo ser exportados para o armazenamento do Azure, os event hubs ou Log Analytics do Azure. Análise de registos do Azure oferece uma solução com gráficos configurável pelo utilizador e personalizáveis. Para mais informações sobre os registos de diagnóstico do Azure, consulte [registos de diagnóstico do Azure](cdn-azure-diagnostic-logs.md).

## <a name="verizon-core-reports"></a>Relatórios de núcleos da Verizon

Como um utilizador CDN do Azure com um **CDN do Azure Standard da Verizon** ou **CDN do Azure Premium da Verizon** perfil, pode ver relatórios de núcleos da Verizon no portal suplementar da Verizon. Relatórios de núcleos da Verizon está acessível através de **gerir** opção do portal do Azure e oferece uma variedade de vistas e gráficos. Para obter mais informações, consulte [relatórios de núcleos da Verizon](cdn-analyze-usage-patterns.md).

## <a name="verizon-custom-reports"></a>Relatórios personalizados da Verizon

Como um utilizador CDN do Azure com um **CDN do Azure Standard da Verizon** ou **CDN do Azure Premium da Verizon** perfil, pode ver relatórios personalizados da Verizon no portal suplementar da Verizon. Relatórios personalizados da Verizon está acessível através de **gerir** opção do portal do Azure. A página de relatórios personalizados da Verizon mostra o número de pedidos ou dados transferido para cada contorno CName que pertencem a um perfil de CDN do Azure. Os dados podem ser agrupados por Estado de cache ou de código de resposta HTTP durante qualquer período de tempo. Para obter mais informações, consulte [relatórios personalizados da Verizon](cdn-verizon-custom-reports.md).

## <a name="azure-cdn-premium-from-verizon-reports"></a>Os relatórios do Azure CDN Premium da Verizon

Com **CDN do Azure Premium da Verizon**, também pode aceder os relatórios seguintes:
   * [Relatórios HTTP avançados](cdn-advanced-http-reports.md)
   * [Estatísticas em tempo real](cdn-real-time-stats.md)
   * [Desempenho do nó edge](cdn-edge-performance.md)

