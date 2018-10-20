---
title: Execuções da aplicação de lógica de monitor com o Log Analytics - Azure Logic Apps | Documentos da Microsoft
description: Obter informações e dados sobre execuções de aplicação lógica com o Log Analytics de depuração para resolução de problemas e diagnóstico
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 10/11/2018
ms.openlocfilehash: 177c361734a88acab5fc10d6b460645be82bf437
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457147"
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-log-analytics"></a>Monitorizar e obter conhecimentos sobre execuções de aplicação lógica com o Log Analytics

Para informações de depuração mais avançadas e monitorização, pode ativar o Log Analytics ao mesmo tempo, quando cria uma aplicação lógica. O log Analytics proporciona o diagnóstico de registo e monitorização para a aplicação lógica é executada através do portal do Azure. Quando adiciona a solução de gestão do Logic Apps, obter o estado agregado para as suas execuções da aplicação lógica e os detalhes específicos, como o estado, o tempo de execução, o estado de ressubmissão e o IDs de correlação.

Este artigo mostra como ativar o Log Analytics, para que pode ver eventos de tempo de execução e a execução de dados para a aplicação lógica.

 > [!TIP]
 > Para monitorizar as logic apps existentes, siga estes passos para [ativar o registo de diagnósticos e enviar dados de tempo de execução de aplicação lógica para o Log Analytics](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Requisitos

Antes de começar, tem de ter uma área de trabalho do Log Analytics. Saiba mais [como criar uma área de trabalho do Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Ativar o registo de diagnóstico durante a criação de aplicações lógicas

1. Na [portal do Azure](https://portal.azure.com), criar uma aplicação lógica. Escolher **criar um recurso** > **integração** > **aplicação lógica**.

   ![Criar uma aplicação lógica](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

1. Sob **Criar aplicação lógica**, realizar estas tarefas, conforme mostrado:

   1. Forneça um nome para a aplicação lógica e selecione a sua subscrição do Azure. 

   1. Crie ou selecione um grupo de recursos do Azure.

   1. Definir **do Log Analytics** ao **no**. 

   1. Na lista de área de trabalho do Log Analytics de lista, selecione a área de trabalho onde pretende enviar dados para a aplicação lógica é executada. 

      ![Criar uma aplicação lógica](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      Depois de concluir este passo, o Azure cria a aplicação lógica, que agora é associado à sua área de trabalho do Log Analytics. 
      Além disso, este passo instala também automaticamente a solução de gestão do Logic Apps em sua área de trabalho.

   1. Quando tiver terminado, escolha **Create** (Criar).

1. Para ver a sua aplicação lógica é executada, [continuar com estes passos](#view-logic-app-runs-oms).

## <a name="install-the-logic-apps-management-solution"></a>Instalar a solução de gestão do Logic Apps

Se já ativou o Log Analytics quando criou a sua aplicação lógica, ignore este passo. Já tem a solução de gestão do Logic Apps instalada.

1. No [portal do Azure](https://portal.azure.com), selecione **Todos os serviços**. Na caixa de pesquisa, introduza "do log analytics" como o filtro e selecione **do Log Analytics**.

   ![Selecione "Do Log Analytics"](./media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

1. Sob **do Log Analytics**, localize e selecione a sua área de trabalho do Log Analytics. 

   ![Selecione a sua área de trabalho do Log Analytics](./media/logic-apps-monitor-your-logic-apps-oms/select-log-analytics-workspace.png)

1. Sob **configurar soluções de monitorização**, escolha **ver soluções**.

   ![Escolha "Ver soluções"](media/logic-apps-monitor-your-logic-apps-oms/log-analytics-workspace.png)

1. Na página Descrição geral, escolha **Add**, que abre o **soluções de gestão** lista. A partir dessa lista, selecione **gestão do Logic Apps**. 

   ![Selecione "Gestão do Logic Apps"](./media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

   Se não conseguir encontrar a solução, na parte inferior da lista, escolha **carregar mais** até que apareça a solução.

1. Escolher **criar**, que instala a solução.

   ![Escolha "Adicionar" para "Gestão do Logic Apps"](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-logic-app-runs-in-log-analytics-workspace"></a>Aplicação de lógica de exibição é executado na área de trabalho do Log Analytics

1. Para ver a contagem e um Estado para execuções de aplicação lógica, aceda à sua área de trabalho do Log Analytics e abrir a página de descrição geral. 

   Detalhes sobre execuções de aplicação lógica são apresentados no **gestão do Logic Apps** mosaico. Para ver um resumo com mais detalhes sobre execuções de aplicação lógica, escolha o **gestão do Logic Apps** mosaico. 

   ![Mosaico de descrição geral que mostra a contagem de execução da aplicação lógica e o Estado](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   Aqui, as execuções de aplicação lógica são agrupadas por nome ou por Estado de execução. 
   Esta página também mostra detalhes sobre falhas de ações ou acionadores para execuções de aplicação lógica.

   ![Estado de resumo para a aplicação lógica é executada](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
1. Para ver todas as execuções de aplicação de lógica específica ou num Estado, selecione a linha para um Estado ou de uma aplicação lógica.

   Eis um exemplo que mostra todas as execuções para uma aplicação de lógica específica:

   ![Ver execuções de para um Estado ou de uma aplicação lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   Esta página tem estas opções avançadas:

   * **Propriedades controladas:**

     Esta coluna mostra propriedades controladas, as quais estão agrupadas por ações, a aplicação lógica. Para ver as propriedades controladas, escolha **vista**. 
     Para procurar as propriedades controladas, utilize o filtro de coluna.
   
     ![Ver propriedades controladas para uma aplicação lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-tracked-properties.png)

     Quaisquer propriedades controladas adicionadas recentemente poderão demorar 10 a 15 minutos antes de serem apresentados pela primeira vez. Saiba mais [como adicionar propriedades controladas à sua aplicação lógica](logic-apps-monitor-your-logic-apps.md#azure-diagnostics-event-settings-and-details).

   * **Volte a submeter:** pode submeter novamente um ou mais execuções da aplicação lógica que falhou, foi concluída com êxito, ou são ainda em execução. Selecione as caixas de verificação para as execuções que pretende submeter novamente e escolha **volte a submeter**. 

     ![Submeter novamente execuções da aplicação lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-resubmit.png)

1. Para filtrar esses resultados, pode realizar a filtragem do lado do cliente e servidor.

   * **Filtro do lado do cliente**: para cada coluna, selecione os filtros que pretende, por exemplo:

     ![Filtros de coluna de exemplo](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * **Filtro do lado do servidor**: escolher uma janela de tempo específico ou para limitar o número de execuções que aparecem, utilize o controlo de âmbito na parte superior da página. Por predefinição, apenas 1000 registos são apresentados ao mesmo tempo.
   
     ![Alterar a janela de tempo](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
1. Para ver todas as ações e os respetivos detalhes para uma execução específica, selecione uma linha para uma execução da aplicação lógica.

   Eis um exemplo que mostra todas as ações de uma execução da aplicação lógica específica:

   ![Executam as ações de modo de exibição de uma aplicação lógica](media/logic-apps-monitor-your-logic-apps-oms/logic-app-action-details.png)
   
1. Em qualquer página de resultados, para ver a consulta por trás os resultados ou para ver todos os resultados, escolha **ver todos os**, que abre a página de pesquisa de registos.
   
   ![Ver tudo nas páginas de resultados](media/logic-apps-monitor-your-logic-apps-oms/logic-app-seeall.png)
   
   Na página de pesquisa de registos,

   * Para ver os resultados da consulta numa tabela, escolha **tabela**.

   * Para alterar a consulta, pode editar a cadeia de consulta na barra de pesquisa. 
   Para uma melhor experiência, escolha **Advanced Analytics**.

     ![Ver ações e os detalhes de uma execução da aplicação lógica](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)
     
     Na página do Log Analytics do Azure, pode atualizar consultas e ver os resultados da tabela. Esta consulta utiliza [linguagem de consulta de Kusto](https://aka.ms/LogAnalyticsLanguageReference), que pode editar se desejar exibir resultados diferentes. 

     ![O Azure Log Analytics - vista de consulta](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Passos Seguintes

* [Monitorizar mensagens B2B](../logic-apps/logic-apps-monitor-b2b-message.md)