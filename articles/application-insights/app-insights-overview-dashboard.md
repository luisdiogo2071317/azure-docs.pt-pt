---
title: Dashboard de descrição geral de informações de aplicação do Azure | Microsoft Docs
description: Monitorizar aplicações com a funcionalidade do Azure Application Insights e descrição geral do Dashboard.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mbullwin
ms.openlocfilehash: 0be54c47965c6a27c3506fd37a7bf67e4b2b7924
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/20/2018
---
# <a name="application-insights-overview-dashboard-preview"></a>Dashboard de descrição geral de informações de aplicação (pré-visualização)

Application Insights sempre tiver fornecido um painel de descrição geral de resumo para permitir o rápido, uma leitura avaliação de estado de funcionamento e o desempenho da aplicação. Dashboard da descrição geral de pré-visualização nova fornece uma experiência mais flexível mais rápida.

## <a name="how-do-i-test-out-the-new-experience"></a>Como testar enviados a nova experiência?

 No Application Insights em: _descrição geral_, selecione _tente descrição geral do novo até se tornar a experiência de predefinição_.

![Pré-visualização de descrição geral](.\media\app-insights-overview-dashboard\app-insights-overview-dashboard-01.png)

Este procedimento inicia o dashboard de descrição geral do novo predefinido:

![Painel de pré-visualização de descrição geral](.\media\app-insights-overview-dashboard\app-insights-overview-dashboard-02.png)

## <a name="better-performance"></a>Melhorar o desempenho

Seleção de intervalo de tempo foi simplificada para uma interface simples de um clique.

![Intervalo de tempo](.\media\app-insights-overview-dashboard\app-insights-overview-dashboard-03.png)

Em geral desempenho foi significativamente aumentado. Cada predefinição atualizar dinamicamente o mosaico KPI está ligada a funcionalidade correspondente do Application Insights. Por exemplo, a seleção de pedidos falhados iniciará o _falhas_ painel:

![Falhas](.\media\app-insights-overview-dashboard\app-insights-overview-dashboard-04.png)

## <a name="application-dashboard"></a>Dashboard de aplicações

Dashboard de aplicação tira partido da tecnologia de dashboard existente no Azure para fornecer uma vista de painel único totalmente personalizável do Estado de funcionamento da aplicação e desempenho.

Para aceder a dashboard predefinido, selecione _Dashboard de aplicações_ no canto superior esquerdo.

![Vista de dashboard](.\media\app-insights-overview-dashboard\app-insights-overview-dashboard-05.png)

Se esta for a primeira vez ao aceder ao dashboard iniciará uma vista predefinida:

![Vista de dashboard](.\media\app-insights-overview-dashboard\app-insights-overview-dashboard-06.png)

Enquanto pode manter a vista predefinida se pretendê-lo, também pode adicionar e eliminar a partir do dashboard para se adaptar melhor se ajustar às necessidades da sua equipa.

> [!NOTE]
> Todos os utilizadores com acesso ao recurso do Application Insights partilham a mesma experiência de dashboard de aplicações. As alterações efetuadas por um utilizador vai modificar a vista para todos os utilizadores.

Para navegar para a experiência de descrição geral apenas selecione:

![Botão de descrição geral](.\media\app-insights-overview-dashboard\app-insights-overview-dashboard-07.png)

## <a name="next-steps"></a>Passos Seguintes

- [Funis](usage-funnels.md)
- [Retenção](app-insights-usage-retention.md)
- [Fluxos do Utilizador](app-insights-usage-flows.md)
- [Dashboards](app-insights-dashboards.md)
