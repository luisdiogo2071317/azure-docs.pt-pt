---
title: Criar um dashboard personalizado no Azure Log Analytics | Documentos da Microsoft
description: Este guia ajuda-o a compreender como os dashboards do Log Analytics podem visualizar-todas as pesquisas de registo guardadas, dando-lhe uma lente única para ver o seu ambiente.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: bf5b37bfa2cb103ef8151203359041fa4bd10f0b
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043334"
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Criar um dashboard personalizado para uso no Log Analytics

Este guia ajuda-o a compreender como os dashboards do Log Analytics podem visualizar-todas as pesquisas de registo guardadas, dando-lhe uma lente única para ver o seu ambiente.

>[!NOTE]
> Já não pode editar já existentes **meu painel**. Esta funcionalidade está a ser preterido.

![Dashboard de exemplo](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

Todos os dashboards personalizados que criou no portal do OMS também estão disponíveis na aplicação OMS Mobile. Consulte as seguintes páginas para obter mais informações sobre as aplicações.

* [Aplicação móvel do OMS da Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [Aplicação móvel do OMS pelo Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![dashboard móvel](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Como posso criar meu dashboard?
Para começar, vá para a página de descrição geral do OMS. Verá a **meu painel** mosaico no lado esquerdo. Clique nele para fazer uma busca detalhada no seu dashboard.

![Descrição geral](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Adicionar um mosaico
Nos dashboards, mosaicos têm a tecnologia suas pesquisas de registos guardadas. OMS é fornecido com muitos previamente criados pesquisas de registos guardadas, para que possa começar imediatamente. Utilize os seguintes passos descrevem como começar.

Na vista de meu painel, basta clicar **personalizar** inserir modo de personalização.

![Pictóricos](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 O painel que se abre no lado direito da página mostra todas as pesquisas de registos guardadas de sua área de trabalho. Para visualizar uma pesquisa de registos guardadas como um mosaico, Paire o rato sobre uma pesquisa guardada e, em seguida, clique nas **plus** símbolo.

![Adicionar mosaicos 1](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Quando clica no **plus** símbolo, um novo mosaico é apresentada na vista do Dashboard de My.

![Adicionar mosaicos 2](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Editar um mosaico
Na vista de meu painel, basta clicar **personalizar** inserir modo de personalização. Clique no mosaico que pretende editar. As alterações do painel direito para editar e oferece uma seleção de opções:

![Editar mosaico](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Editar mosaico](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Visualizações de mosaicos
Existem três tipos de visualizações de mosaicos à sua escolha:

| tipo de gráfico | O que faz |
| --- | --- |
| ![Gráfico de barras](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Apresenta uma linha cronológica de resultados da pesquisa de registos guardadas como um gráfico de barras ou uma lista de resultados por um campo dependendo se a sua pesquisa de registos agrega os resultados por um campo ou não. |
| ![métrica](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Apresenta sua acertos de resultado de pesquisa de registo total como um número num mosaico. Mosaicos de métricos permitem-lhe definir um limiar que irá realçar o mosaico quando é atingido o limiar. |
| ![Linha](./media/log-analytics-dashboards/oms-dashboards-line.png) |Apresenta uma linha cronológica de sua resultados de resultado de pesquisa de registos guardadas com valores como um gráfico de linhas. |

### <a name="threshold"></a>Limiar
Pode criar um limiar num mosaico usando a visualização de métrica. Selecione para criar um valor de limiar no mosaico. Escolha se pretende realçar o mosaico quando o valor é sobre ou abaixo do limiar escolhido, em seguida, defina o valor de limiar abaixo.

## <a name="organizing-the-dashboard"></a>Organizar o dashboard
Para organizar o seu dashboard, navegue para a vista de meu painel e clique em **personalizar** inserir modo de personalização. Clique e arraste o mosaico que pretende mover e movê-lo para onde pretende que o seu mosaico para ser.

![Organizar o seu Dashboard](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Remover um mosaico
Para remover um mosaico, navegue para a vista de meu painel e clique em **personalizar** inserir modo de personalização. Selecione o mosaico que pretende remover, em seguida, no painel direito, selecione **remover mosaico**.

![Remover um mosaico](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Passos Seguintes
* Crie [alertas](log-analytics-alerts.md) no Log Analytics para gerar notificações e a resolução de problemas.
