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
ms.openlocfilehash: 0f4eaaefb7d2080218e19574621a4962e61057c3
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/17/2018
ms.locfileid: "49394313"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Recursos do Application Insights para as funções do Azure suportados

Ofertas de funções do Azure [integração incorporada](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) com o Application Insights, que está disponível por meio da Interface de ILogger. Segue-se a lista de recursos atualmente suportados. Reveja o guia das funções do Azure para o [introdução ao](https://github.com/Azure/Azure-Functions/wiki/App-Insights).

## <a name="supported-features"></a>Funcionalidades suportadas

| Funções do Azure                       | V1                | V2 (Ignite de 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **O Application Insights SDK para .NET**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Recolha automática de**        |                 |                   |               
| &bull; Pedidos                     | Sim             | Sim               | 
| &bull; Exceções                   | Sim             | Sim               | 
| &bull; Dependências                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Sim               | 
| &nbsp;&nbsp;&nbsp;&mdash; serviceBus|                 | Sim               | 
| &nbsp;&nbsp;&nbsp;&mdash; eventHub  |                 | Sim               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Sim               | 
| | | | 
| **Funcionalidades suportadas**                |                   |                   |               
| &bull; QuickPulse/LiveMetrics       | Sim             | Sim               | 
| &bull; Amostragem                     | Sim             | Sim               | 
| &bull; Heartbeats                   |                 | Sim               | 
| | | | 
| **Correlação**                       |                   |                   |               
| &bull; serviceBus                     |                   | Sim               | 
| &bull; eventHub                       |                   | Sim               | 
| | | | 
| **Configurável**                      |                   |                   |           
| &bull;Totalmente configurável.<br/>Ver [as funções do Azure](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) para obter instruções.<br/>Ver [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) para todas as opções.               |                   | Sim                   | 


## <a name="sampling"></a>Amostragem

As funções do Azure permite a amostragem por predefinição na respetiva configuração. Para obter mais informações, consulte [amostragem configurar](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).
