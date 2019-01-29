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
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: 06feece050835b2b9188eb702210770b44a6b49c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55185820"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Recursos do Application Insights para as funções do Azure suportados

Ofertas de funções do Azure [integração incorporada](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) com o Application Insights, que está disponível por meio da Interface de ILogger. Segue-se a lista de recursos atualmente suportados. Reveja o guia das funções do Azure para o [introdução ao](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Funcionalidades suportadas

| Funções do Azure                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **O Application Insights SDK para .NET**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Recolha automática de**        |                 |                   |               
| &bull; Pedidos                     | Sim             | Sim               | 
| &bull; Exceções                   | Sim             | Sim               | 
| &bull; Dependências                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Sim               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Sim               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Sim               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Sim               | 
| | | | 
| **Funcionalidades suportadas**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Sim             | Sim               | 
| &nbsp;&nbsp;&nbsp;&mdash; Proteger o canal de controlo|                 | Sim               | 
| &bull; Amostragem                     | Sim             | Sim               | 
| &bull; Heartbeats                   |                 | Sim               | 
| | | | 
| **Correlação**                       |                   |                   |               
| &bull; serviceBus                     |                   | Sim               | 
| &bull; eventHub                       |                   | Sim               | 
| | | | 
| **Configurável**                      |                   |                   |           
| &bull;Totalmente configurável.<br/>Ver [as funções do Azure](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) para obter instruções.<br/>Ver [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) para todas as opções.               |                   | Sim                   | 


## <a name="live-metrics--secure-control-channel"></a>Métricas em direto e o canal de controlo de segurança

Os critérios de filtros personalizados que especificar são enviados de volta para o componente de métricas em direto no Application Insights SDK. Os filtros poderiam potencialmente conter informações confidenciais, como customerIDs. Pode fazer o canal seguro com uma chave secreta da API. Ver [Proteja o canal de controlo](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel) para obter instruções.

## <a name="sampling"></a>Amostragem

As funções do Azure permite a amostragem por predefinição na respetiva configuração. Para obter mais informações, consulte [amostragem configurar](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).
