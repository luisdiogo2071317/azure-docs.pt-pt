---
title: Criar consultas para mensagens B2B no Log Analytics - Azure Logic Apps | Documentos da Microsoft
description: Criar consultas que controlam o AS2, X12 e mensagens EDIFACT com o Log Analytics para o Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/19/2018
ms.openlocfilehash: baccd255fc2812eae0de3a98dfcef3dcbc7e1b46
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43124275"
---
# <a name="create-queries-for-tracking-as2-x12-and-edifact-messages-in-log-analytics-for-azure-logic-apps"></a>Criar consultas para controlo de AS2, X12 e EDIFACT as mensagens no Log Analytics para o Azure Logic Apps

Para localizar o AS2, X12 ou EDIFACT mensagens de que está a controlar com [do Azure Log Analytics](../log-analytics/log-analytics-overview.md), pode criar consultas que filtrarão ações com base em critérios específicos. Por exemplo, pode encontrar mensagens com base num número de controlo de intercâmbio específico.

## <a name="requirements"></a>Requisitos

* Uma aplicação de lógica que está configurada com o registo de diagnósticos. Saiba mais [como criar uma aplicação lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md) e [como configurar o registo para essa aplicação lógica](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Uma conta de integração que está configurada com a monitorização e registo. Saiba mais [como criar uma conta de integração](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) e [como configurar a monitorização e registo para essa conta](../logic-apps/logic-apps-monitor-b2b-message.md).

* Se ainda não o fez, [publicar dados de diagnóstico para o Log Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) e [configurar o controlo do Log Analytics da mensagem](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> Depois de cumprir os requisitos anteriores, deve ter uma área de trabalho do Log Analytics. Deve usar a mesma área de trabalho para rastrear a comunicação de B2B no Log Analytics. 
>  
> Se não tiver uma área de trabalho do Log Analytics, saiba [como criar uma área de trabalho do Log Analytics](../log-analytics/log-analytics-quick-create-workspace.md).

## <a name="create-message-queries-with-filters-in-log-analytics"></a>Criar consultas de mensagens com filtros no Log Analytics

Este exemplo mostra como pode encontrar mensagens com base no respetivo número de controlo de intercâmbio.

> [!TIP] 
> Se souber o nome de área de trabalho do Log Analytics, vá para a home page de sua área de trabalho (`https://{your-workspace-name}.portal.mms.microsoft.com`) e iniciar o passo 4. Caso contrário, comece no passo 1.

1. Na [portal do Azure](https://portal.azure.com), escolha **todos os serviços**. Procure "log analytics" e, em seguida, escolha **do Log Analytics** conforme mostrado aqui:

   ![Encontrar o Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Sob **do Log Analytics**, localize e selecione a sua área de trabalho do Log Analytics.

   ![Selecione a sua área de trabalho do Log Analytics](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Sob **gerenciamento**, escolha **pesquisa de registos**.

   ![Escolha Lo pesquisa](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/azure-portal-page.png)

4. Na caixa de pesquisa, introduza um campo que pretende localizar e prima **Enter**. Quando começa a digitar, do Log Analytics mostra possíveis correspondências e operações que pode utilizar. Saiba mais sobre [como encontrar dados no Log Analytics](../log-analytics/log-analytics-log-searches.md).

   Neste exemplo procura eventos com **tipo = AzureDiagnostics**.

   ![Comece a escrever a cadeia de consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

5. Na barra esquerda, escolha o período de tempo que pretende ver. Para adicionar um filtro à sua consulta, escolha **+ adicionar**.

   ![Adicionar filtro à consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

6. Sob **adicionar filtros**, introduza o nome do filtro para que possa encontrar o filtro que pretende. Selecione o filtro e escolha **+ adicionar**.

   Para localizar o número de controlo de intercâmbio, neste exemplo procura a palavra "intercâmbio" e seleciona **event_record_messageProperties_interchangeControlNumber_s** como o filtro.

   ![Selecione o filtro](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

7. Na barra esquerda, selecione o valor do filtro que pretende utilizar e escolha **aplicar**.

   Neste exemplo seleciona o número de controlo de intercâmbio para as mensagens que queremos.

   ![Selecione o valor do filtro](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

8. Agora é volte para a consulta que está a criar. A consulta foi atualizada com os seus eventos de filtro selecionado e o valor. Seus resultados anteriores agora são filtrados demasiado.

    ![Voltar à sua consulta com resultados filtrados](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Guardar a consulta para utilização futura

1. Da sua consulta sobre o **pesquisa de registos** página, selecione **guardar**. Dê um nome de sua consulta, selecione uma categoria e escolha **guardar**.

   ![Dar um nome e uma categoria a sua consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. Para ver a sua consulta, escolha **Favoritos**.

   ![Escolha "Favoritos"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Sob **pesquisas guardadas**, selecione a sua consulta, para que pode exibir os resultados. Para atualizar a consulta para que possa encontrar resultados diferentes, edite a consulta.

   ![Selecione a sua consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-log-analytics"></a>Localizar e executar consultas guardadas no Log Analytics

1. Abra a sua home page de área de trabalho do Log Analytics (`https://{your-workspace-name}.portal.mms.microsoft.com`) e escolha **pesquisa de registos**.

   ![Na sua home page do Log Analytics, escolha "Pesquisa de registos"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   -ou-

   ![No menu, escolha "Pesquisa de registos"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. Sobre o **pesquisa de registos** home page do, escolha **Favoritos**.

   ![Escolha "Favoritos"](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Sob **pesquisas guardadas**, selecione a sua consulta, para que pode exibir os resultados. Para atualizar a consulta para que possa encontrar resultados diferentes, edite a consulta.

   ![Selecione a sua consulta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Passos Seguintes

* [Esquemas de controlo de AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Esquemas de controlo de X12](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Esquemas de controlo personalizado](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)