---
title: 'Início rápido: Explorador do Time Series Insights do Azure | Documentos da Microsoft'
description: Este guia de introdução mostra como começar com o explorador do Azure Time Series Insights no seu browser para visualizar grandes volumes de dados de IoT. Visita guiada pelas principais funcionalidades num ambiente de demonstração.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 11/15/2017
ms.openlocfilehash: 32ed0ec178f7a0064fae88357011197e71604885
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555545"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Início rápido: Explorar o Azure Time Series Insights
Este guia de introdução mostra como começar a utilizar o explorador do Azure Time Series Insights num ambiente de demonstração gratuito. Irá aprender a utilizar o seu browser para visualizar grandes volumes de dados de IoT e realizar uma visita guiada das principais funcionalidades do explorador do Time Series Insights. 

O Azure Time Series Insights é um serviço de análise, armazenamento e visualização totalmente gerido que simplifica a exploração e a análise de milhões de eventos de IoT em simultâneo. Apresenta uma vista global dos seus dados, o que lhe permite validar rapidamente a sua solução IoT e evitar um período de indisponibilidade dispendioso para dispositivos fundamentais para a sua atividade ao ajudá-lo a descobrir tendências ocultas, detetar anomalias e realizar análises de causa raiz praticamente em tempo real.  Se está a criar uma aplicação que precisa de armazenar ou consultar dados de série de tempo, pode programar com as APIs REST do Time Series Insights.

Se não tiver uma subscrição do Azure, crie uma [conta do Azure gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Explore o explorador do Time Series Insights num ambiente de demonstração

1. No browser, navegue até [https://insights.timeseries.azure.com/demo](https://insights.timeseries.azure.com/demo). 

2. Se lhe for solicitado, inicie sessão no explorador do Time Series Insights com as suas credenciais de conta do Azure. 
 
3. A página de visita guiada rápida do Time Series Insights é apresentada. Clique em **Seguinte** para começar a visita guiada rápida.

   ![Clique em Seguinte](media/quickstart/quickstart1.png)

4. O **painel de seleção de tempo** é apresentado. Utilize este painel para selecionar um intervalo de tempo para visualizar.

   ![Painel de seleção de tempo](media/quickstart/quickstart2.png)

5. Clique e arraste na região e, em seguida, clique no botão **Pesquisa**.
 
   ![Selecione um intervalo de tempo](media/quickstart/quickstart3.png) 

   O Time Series Insights apresenta uma visualização de gráfico para o intervalo de tempo especificado. Pode realizar diversas ações no gráfico de linhas, como a filtragem, afixação, ordenação e pilha. 

   Para voltar para o **Painel de seleção de tempo**, clique na seta para baixo, conforme mostrado:

   ![Gráfico](media/quickstart/quickstart4.png)

6. Clique em **Adicionar** no **Painel de termos** para adicionar um novo termo de pesquisa.

   ![Adicionar item](media/quickstart/quickstart5.png)

7. No gráfico, pode selecionar uma região, clicar com o botão direito na região e selecionar **Explorar Eventos**.
 
   ![Explorar Eventos](media/quickstart/quickstart6.png)

   É apresentada uma grelha dos dados não processados na região que está a explorar:

   ![Vista de grelha](media/quickstart/quickstart7.png)

8. Edite os termos para alterar os valores no gráfico e adicione outro termo para diferentes tipos de valores correlacionados entre si:

   ![Adicionar um termo](media/quickstart/quickstart8.png)

9. Introduza um termo de filtro na caixa **Filtrar série...** para filtrar séries ad hoc. Para o guia de introdução, introduza **Station5** para correlacionar entre si a temperatura e a pressão dessa estação.
 
   ![Filtrar série](media/quickstart/quickstart9.png)

Depois de concluir o guia de introdução, pode experimentar com o conjunto de dados de exemplo para criar visualizações diferentes. 

### <a name="next-steps"></a>Passos Seguintes
Está pronto para criar o seu próprio ambiente do Time Series Insights:
> [!div class="nextstepaction"]
> [Planeie o seu ambiente do Time Series Insights](time-series-insights-environment-planning.md)
