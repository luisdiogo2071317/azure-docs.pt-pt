---
title: Adicionar dados LUIS ao Application Insights com o Node.js | Microsoft Docs
titleSuffix: Azure
description: Crie um bot integrado com uma aplicação de LUIS e o Application Insights com o Node.js.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/18/2018
ms.author: v-geberr
ms.openlocfilehash: 929b6e1cc980d7215f91a616820e257aed26bab7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355466"
---
# <a name="add-luis-results-to-application-insights-from-a-web-app-bot"></a>Adicionar LUIS resultados para o Application Insights a partir de um bot de aplicação web
Este tutorial adiciona LUIS as informações de pedido e resposta para [Application Insights](https://azure.microsoft.com/services/application-insights/) armazenamento de dados de telemetria. Assim que tiver dados, pode consultá-lo com o idioma de Kusto ou Power BI para analisar, Agregar e criar relatórios sobre entidades de utterance em tempo real e pendentes. Isto ajuda a análise determinar se deve adicionar ou editar as entidades da sua aplicação LUIS e pendentes.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Adicionar a biblioteca do Application Insights para um bot de aplicação web
* Capturar e enviar LUIS os resultados da consulta para o Application Insights
* Consultar o Application Insights para intenção superior, pontuação e utterance

## <a name="prerequisites"></a>Pré-requisitos

* O bot de aplicação web LUIS do **[tutorial anterior](luis-nodejs-tutorial-build-bot-framework-sample.md)** com o Application Insights ativada. 

> [!Tip]
> Se ainda não tiver uma subscrição, pode registar-se para uma [conta gratuita](https://azure.microsoft.com/free/).

Todo o código neste tutorial de está disponível na [repositório do github LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/nodejs) e cada linha associada a este tutorial é Comentada com `//APPINSIGHT:`. 

## <a name="web-app-bot-with-luis"></a>Bot de aplicação Web com LUIS
Este tutorial parte do princípio de que tem o código que se parece com o seguinte ou que concluiu o [outro tutorial](luis-nodejs-tutorial-build-bot-framework-sample.md): 

   [!code-javascript[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/nodejs/app.js "Web app bot with LUIS")]

## <a name="add-application-insights-library-to-web-app-bot"></a>Adicionar a biblioteca do Application Insights para bot de aplicação web
Atualmente, o serviço de Application Insights, utilizado em bot esta aplicação web, recolhe telemetria do Estado geral para o bot. Não recolhe informações de pedido e resposta LUIS que terá de verificar e corrigir as entidades e pendentes. 

Para capturar o pedido de LUIS e de resposta, bot de aplicação web tem o **[Application Insights](https://www.npmjs.com/package/applicationinsights)** pacote NPM instalado e configurado no **app.js** ficheiro. Em seguida, os processadores de caixa de diálogo intenção tem de enviar o LUIS informações de pedido e resposta para o Application Insights. 

1. No portal do Azure, no serviço de bot de aplicações web, selecione **criar** sob o **Bot gestão** secção. 

    ![Pesquisa de informações de aplicação](./media/luis-tutorial-appinsights/build.png)

2. Um novo separador do browser abre-se com o Editor de serviço de aplicações. Selecione o nome de aplicação na barra superior, em seguida, selecione **abrir a consola do Kudu**. 

    ![Pesquisa de informações de aplicação](./media/luis-tutorial-appinsights/kudu-console.png)

3. Na consola, introduza o seguinte comando para instalar os pacotes de um caráter de sublinhado e o Application Insights:

    ```
    cd site\wwwroot && npm install applicationinsights && npm install underscore
    ```

    ![Pesquisa de informações de aplicação](./media/luis-tutorial-appinsights/npm-install.png)

    Aguarde que os pacotes instalar:

    ```
    luisbot@1.0.0 D:\home\site\wwwroot
    `-- applicationinsights@1.0.1 
      +-- diagnostic-channel@0.2.0 
      +-- diagnostic-channel-publishers@0.2.1 
      `-- zone.js@0.7.6 
    
    npm WARN luisbot@1.0.0 No repository field.
    luisbot@1.0.0 D:\home\site\wwwroot
    +-- botbuilder-azure@3.0.4
    | `-- azure-storage@1.4.0
    |   `-- underscore@1.4.4 
    `-- underscore@1.8.3 
    ```

    São efetuadas com separador de browser da consola do kudu.

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Capturar e enviar LUIS os resultados da consulta para o Application Insights
1. No separador do browser de Editor do serviço de aplicações, abra o **app.js** ficheiro.

2. Adicione as seguintes bibliotecas NPM em existente `requires` linhas:

   [!code-javascript[Add NPM packages to app.js](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=12-16 "Add NPM packages to app.js")]

3. Criar o objeto do Application Insights e utilizar a definição da aplicação web app bot **BotDevInsightsKey**: 

   [!code-javascript[Create the Application Insights object](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=68-80 "Create the Application Insights object")]

4. Adicionar o **appInsightsLog** função:

   [!code-javascript[Add the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=82-109 "Add the appInsightsLog function")]

    A última linha da função é onde os dados sejam adicionados ao Application Insights. O nome do evento é **LUIS resultados**, um nome exclusivo para além dos outros dados de telemetria recolhida por esta bot de aplicação web. 

5. Utilize o **appInsightsLog** função. Adicione-a cada caixa de diálogo intenção:

   [!code-javascript[Use the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=117-118 "Use the appInsightsLog function")]

6. Para testar a sua bot de aplicação web, utilize o **testar na Web Chat** funcionalidade. Não deverá ver nenhuma diferença porque todo o trabalho no Application Insights, não está nas respostas de bot.

## <a name="view-luis-entries-in-application-insights"></a>Vista LUIS entradas no Application Insights
Abrir o Application Insights para ver as entradas de LUIS. 

1. No portal, selecione **todos os recursos** , em seguida, filtrar por nome de bot da aplicação web. Clique no recurso com o tipo **Application Insights**. O ícone para o Application Insights é uma lâmpada. 

    ![Pesquisa de informações de aplicação](./media/luis-tutorial-appinsights/search-for-app-insights.png)



2. Quando o recurso for aberto, clique em de **pesquisa** ícone da Lupa no painel de extremidade direita. Um novo painel para a direita apresenta. Dependendo da quantidade de dados telemétricos for encontrado, o painel poderá demorar um segundo para apresentar. Procurar `LUIS-results` e prima introduza no teclado. A lista é narrowed apenas LUIS os resultados de consulta adicionada com este tutorial.

    ![Filtrar para dependências](./media/luis-tutorial-appinsights/app-insights-filter.png)

3. Selecione a entrada superior. Uma nova janela apresenta dados de mais detalhados, incluindo os dados personalizados para a consulta de LUIS na extremidade direita. Os dados incluem a intenção superior e a respetiva classificação.

    ![Detalhes da dependência](./media/luis-tutorial-appinsights/app-insights-detail.png)

    Quando tiver terminado, selecione a parte superior direita **X** para regressar à lista de itens de dependência. 


> [!Tip]
> Se pretender guardar a lista de dependência e regressar ao mesmo mais tarde, clique em **... Mais** e clique em **guardar favorito**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Consultar o Application Insights para intenção, pontuação e utterance
Application Insights dá-lhe a capacidade de consultar os dados com o [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) idioma, assim como a exportação para [PowerBI](https://powerbi.microsoft.com). 

1. Clique em **análise** no topo da dependência listagem acima caixa do filtro. 

    ![Botão Análise](./media/luis-tutorial-appinsights/analytics-button.png)

2. Abre uma nova janela com uma janela de consulta na parte superior e uma janela de tabela de dados seguintes aqueles que. Se tiver utilizado o bases de dados antes desta disposição está familiarizada. A consulta inclui todos os itens das últimas 24 horas, começando com o nome `LUIS-results`. O **CustomDimensions** coluna tem os resultados da consulta LUIS como pares nome/valor.

    ![Janela de consulta de análise](./media/luis-tutorial-appinsights/analytics-query-window.png)

3. Para solicitar a parte superior intenção, pontuação e utterance, adicionam o seguinte apenas acima da última linha na janela de consulta:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_intent_intent)
    | extend score = todouble(customDimensions.LUIS_intent_score)
    | extend utterance = tostring(customDimensions.LUIS_text)
    ```

4. Execute a consulta. Desloque-se na extremidade direita na tabela de dados. Novas colunas de topIntent, pontuação e utterance estão disponíveis. Clique na coluna topIntent para ordenar.

    ![Objetivo de principal de análise](./media/luis-tutorial-appinsights/app-insights-top-intent.png)


Saiba mais sobre o [idioma de consulta Kusto](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-queries) ou [exportar os dados ao Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Passos Seguintes

Outras informações que pretende adicionar aos dados do application insights incluem o ID da aplicação, ID da versão, data da última alteração modelo, data da última formação, a última data de publicação. Estes valores está podem ser obtidos a partir do URL de ponto final (ID de aplicação e o ID de versão) ou a partir de um [criação API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) chamada, em seguida, definir nas definições de bot da aplicação web e solicitados a partir daí.  

Se estiver a utilizar a mesma subscrição de ponto final para a mais do que uma aplicação de LUIS, também deve incluir o ID de subscrição e uma propriedade a indicar que é uma chave partilhada. 

> [!div class="nextstepaction"]
> [Saiba mais sobre utterances de exemplo](luis-how-to-add-example-utterances.md)
