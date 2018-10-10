---
title: Recursos de suporte do Azure Application Insights - as funções do Azure | Documentos da Microsoft
description: O Application Insights funcionalidades suportadas para as funções do Azure
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 05958f35f80a53da27e020d367799519ef5a9bd7
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901588"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Recursos do Application Insights para as funções do Azure suportados

Segue-se a lista suportada atualmente de recursos para o [integração do Application Insights com o Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-monitoring). Reveja o guia das funções do Azure para o [introdução ao](https://github.com/Azure/Azure-Functions/wiki/App-Insights).


| Funções do Azure                       | V1                | V2 (Ignite de 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **O Application Insights SDK para .NET**         | **2.5.0**             | **2.7.2**                 |
| | | | 
| **Recolha automática de**              |                   |                   |               
| &bull; Pedidos                           | Sim               | Sim               | 
| &bull; Exceções                         | Sim               | Sim               | 
| &bull; Dependências               |                   |                   |               
| &mdash; HTTP                              |                   | Sim               | 
| &mdash; serviceBus                        |                   | Sim               | 
| &mdash; eventHub                          |                   | Sim               | 
| &mdash; SQL                               |                   | Sim               | 
| | | | 
| **Funcionalidades suportadas**                    |                   |                   |               
| &bull; QuickPulse/LiveMetrics                         | Sim               | Sim               | 
| &bull; Amostragem                           | Sim               | Sim               | 
| &bull; Heartbeats                         |       | Sim               | 
| | | | 
| **Correlação**                           |                   |                   |               
| &bull; serviceBus                         |                   | Sim               | 
| &bull; eventHub                           |                   | Sim               | 
| | | | 
| **Configurável**                  |                   |                   |           
| &bull;Totalmente configurável.<br/>Ver [as funções do Azure](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) para obter instruções.<br/>Ver [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) para todas as opções.               |                   | Sim                   | 
