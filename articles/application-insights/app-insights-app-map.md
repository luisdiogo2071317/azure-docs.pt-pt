---
title: Mapa da aplicação no Application Insights do Azure | Documentos da Microsoft
description: Monitorizar topologias de aplicativo complexo com o mapa da aplicação
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/14/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 1ecdbdfb657d0372fea87c4260226f9de8ded9ce
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682508"
---
# <a name="application-map-triage-distributed-applications"></a>Mapa da aplicação: Fazer a triagem de aplicações distribuídas

Mapa da aplicação ajuda-o a afunilamentos de desempenho spot ou hotspots de falha em todos os componentes da aplicação distribuída. Cada nó no mapa representa um componente da aplicação ou as respetivas dependências; e tem o estado de funcionamento KPI e estado de alerta. Pode clicar a partir de qualquer componente para diagnósticos mais detalhados, como eventos do Application Insights. Se a sua aplicação utilizar serviços do Azure, também pode clicar sucessivamente para diagnóstico do Azure, tais como as recomendações do Assistente de base de dados SQL.

## <a name="what-is-a-component"></a>O que é um componente?

Os componentes são partes implementáveis independentemente do aplicativo/microsserviços distribuídos. As equipes de operações e os desenvolvedores têm visibilidade de nível de código ou de acesso à telemetria gerado por estes componentes da aplicação. 

* Componentes são diferentes de "observadas" dependências externas, como SQL, etc. de EventHub que sua equipe/organização não pode ter acesso ao (código ou telemetria).
* Componentes são executados em qualquer número de instâncias de função/servidor/contentor.
* Componentes podem ser separadas chaves de instrumentação do Application Insights (mesmo que as subscrições são diferentes) ou de diferentes funções de relatório para uma única chave de instrumentação do Application Insights. A experiência de mapa de pré-visualização mostra os componentes independentemente da forma como eles são configurados.

## <a name="composite-application-map"></a>Mapa de aplicativo composto

Pode ver a topologia de completo da aplicação através de vários níveis dos componentes de aplicativos relacionados. Componentes podem ser diferentes recursos do Application Insights, ou funções diferentes num único recurso. O mapa da aplicação localiza os componentes seguintes chamadas de dependência HTTP feitas entre servidores com o Application Insights SDK instalado. 

Esta experiência é iniciado com a deteção de progressiva dos componentes. Quando carrega o mapa de aplicativo em primeiro lugar, um conjunto de consultas são acionados para detetar os componentes relacionados com este componente. Um botão no canto superior esquerdo será atualizado com o número de componentes na sua aplicação à medida que são detetados. 

Ao clicar em "Componentes de mapa de atualização", o mapa é atualizado com todos os componentes detetados até que ponto.

Se todos os componentes são funções dentro de um único recurso do Application Insights, em seguida, este passo de deteção não é necessário. O carregamento inicial para esse aplicativo terá todos os seus componentes.

![Captura de ecrã de mapa de aplicativo](media/app-insights-app-map/001.png)

Um dos principais objetivos com esta experiência é conseguir visualizar topologias complexas com centenas de componentes.

Clique em qualquer componente para ver informações relacionadas e vá para o desempenho e a experiência de triagem de falha desse componente.

![Lista de opções](media/app-insights-app-map/application-map-001.png)

### <a name="investigate-failures"></a>Investigar falhas

Selecione **investigar falhas** para iniciar o painel de falhas.

![Captura de ecrã do botão de falhas de investigar](media/app-insights-app-map/investigate-failures.png)

![Captura de ecrã da experiência de falhas](media/app-insights-app-map/failures.png)

### <a name="investigate-performance"></a>Investigar o desempenho

Para resolver problemas de desempenho problemas selecione **investigue o desempenho**

![Captura de ecrã do botão de desempenho de investigar](media/app-insights-app-map/investigate-performance.png)

![Captura de ecrã da experiência de desempenho](media/app-insights-app-map/performance.png)

### <a name="go-to-details"></a>Ir para os detalhes

Selecione **vá para detalhes** para explorar a experiência de transação de ponta a ponta que pode oferecer exibições feitas para o nível de pilha de chamada.

![Captura de ecrã do botão go-detalhes](media/app-insights-app-map/go-to-details.png)

![Captura de ecrã de detalhes da transação de ponta a ponta](media/app-insights-app-map/end-to-end-transaction.png)

### <a name="view-in-analytics"></a>Vista no Analytics

Para consultar e investigar o clique de mais de dados de aplicativos **ver na análise**.

![Captura de ecrã da vista no botão análise](media/app-insights-app-map/view-in-analytics.png)

![Captura de ecrã da experiência de análise](media/app-insights-app-map/analytics.png)

### <a name="alerts"></a>Alertas

Para ver alertas ativos e as regras subjacentes que fazem com que os alertas ser tiggered, selecione **alertas**.

![Captura de ecrã do botão de alertas](media/app-insights-app-map/alerts.png)

![Captura de ecrã da experiência de análise](media/app-insights-app-map/alerts-view.png)

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Comentários
Envie comentários por meio da opção de comentários do portal.

![Imagem de MapLink-1](./media/app-insights-app-map/13.png)

## <a name="next-steps"></a>Passos Seguintes

* [Portal do Azure](https://portal.azure.com)
