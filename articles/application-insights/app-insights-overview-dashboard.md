---
title: Dashboard de descrição geral do Application Insights | Microsoft Docs
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
ms.openlocfilehash: bccb56ad45d9054a437bf2d85e74a8d81fbc3db1
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 05/12/2018
---
# <a name="overview-dashboard-preview"></a>Descrição geral dashboard (pré-visualização)

Application Insights sempre tiver fornecido um painel de descrição geral de resumo para permitir o rápido, uma leitura avaliação de estado de funcionamento e o desempenho da aplicação. A iniciar uma nova experiência mais flexível mais rápida no dia 15 de Maio de 2018 será lançada como uma pré-visualização. Em 29 de Maio de 2018 será descontinuada a experiência de descrição geral clássico.

## <a name="how-do-i-test-out-the-new-experience"></a>Como testar enviados a nova experiência?

No dia 15 de Maio, a nova experiência irá começar a aparecer no Application Insights em: _investigar_ > _descrição geral (pré-visualização)_.

![Pré-visualização de descrição geral](.\media\app-insights-overview-dashboard\01.png)

Este procedimento inicia o dashboard de descrição geral do novo predefinido:

![Painel de pré-visualização de descrição geral](.\media\app-insights-overview-dashboard\02.png)

## <a name="better-performance"></a>Melhorar o desempenho

Seleção de intervalo de tempo foi simplificada para uma interface simples de um clique.

![Intervalo de tempo](.\media\app-insights-overview-dashboard\04.png)

Em geral desempenho foi significativamente aumentado. Cada predefinição atualizar dinamicamente o mosaico KPI está ligada a funcionalidade correspondente do Application Insights. Por exemplo, a seleção de pedidos falhados iniciará o _falhas_ painel:

![Falhas](.\media\app-insights-overview-dashboard\03.png)

## <a name="application-dashboard"></a>Dashboard de aplicações

Dashboard de aplicação tira partido da tecnologia de dashboard existente no Azure para fornecer uma vista de painel único totalmente personalizável do Estado de funcionamento da aplicação e desempenho.

Para aceder a dashboard predefinido, selecione _Dashboard de aplicações_ no canto superior esquerdo.

![Vista de dashboard](.\media\app-insights-overview-dashboard\0009.png)

Se esta for a primeira vez ao aceder ao dashboard iniciará uma vista predefinida:

![Vista de dashboard](.\media\app-insights-overview-dashboard\06.png)

Enquanto pode manter a vista predefinida se pretendê-lo, também pode adicionar e eliminar a partir do dashboard para se adaptar melhor se ajustar às necessidades da sua equipa.

Para navegar para a experiência de descrição geral apenas selecione:

![Botão de descrição geral](.\media\app-insights-overview-dashboard\07.png)

Há também um novo botão chamado _partes de Pin_.

![Botão de descrição geral](.\media\app-insights-overview-dashboard\008.png)

Isto replica uma funcionalidade ligeiramente conhecida da descrição de geral clássica que permite-lhe tirar qualquer um dos mosaicos da experiência de descrição geral antigo _(alertas, disponibilidade, métricas em direto, utilização, deteções Proativa e o mapeamento de aplicações)_ e Adicione-os para dashboards personalizados. 

No caso da predefinição _Dashboard de aplicações_ , já foram adicionados estes mosaicos. Mas se criar dashboards personalizados adicionais ou se alguém na sua equipa elimina um mosaico clássico e adicioná-lo novamente, _partes de Pin_ fornece essa funcionalidade em qualquer seja fácil encontrar local.
