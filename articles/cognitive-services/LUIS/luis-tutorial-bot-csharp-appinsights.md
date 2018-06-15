---
title: Adicionar dados LUIS ao Application Insights com c# | Microsoft Docs
titleSuffix: Azure
description: Crie um bot integrado com uma aplicação de LUIS e o Application Insights com c#.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/07/2018
ms.author: v-geberr
ms.openlocfilehash: 52b6ae224b0e8da12eb4903f5100a6e5cc39704d
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356127"
---
# <a name="add-luis-results-to-application-insights-from-a-web-app-bot"></a>Adicionar LUIS resultados para o Application Insights a partir de um bot de aplicação web
Este tutorial adiciona as informações de resposta de LUIS para [Application Insights](https://azure.microsoft.com/services/application-insights/) armazenamento de dados de telemetria. Assim que tiver dados, pode consultá-lo com o idioma de Kusto ou Power BI para analisar, Agregar e criar relatórios sobre entidades de utterance em tempo real e pendentes. Isto ajuda a análise determinar se deve adicionar ou editar as entidades da sua aplicação LUIS e pendentes.

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Adicionar o Application Insights para um bot de aplicação web
* Capturar e enviar LUIS os resultados da consulta para o Application Insights
* Consultar o Application Insights para intenção superior, pontuação e utterance

## <a name="prerequisites"></a>Pré-requisitos

* O bot de aplicação web LUIS do **[tutorial anterior](luis-csharp-tutorial-build-bot-framework-sample.md)** com o Application Insights ativada. 
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) instalado localmente no seu computador.

> [!Tip]
> Se ainda não tiver uma subscrição, pode registar-se para uma [conta gratuita](https://azure.microsoft.com/free/).

Todo o código neste tutorial de está disponível na [repositório do github LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/csharp) e cada linha associada a este tutorial é Comentada com `//LUIS Tutorial:`. 

## <a name="review-luis-web-app-bot"></a>Reveja bot de aplicação web de LUIS
Este tutorial parte do princípio de que tem o código que se parece com o seguinte ou que concluiu o [outro tutorial](luis-csharp-tutorial-build-bot-framework-sample.md): 

   [!code-csharp[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs "Web app bot with LUIS")]

## <a name="application-insights-in-web-app-bot"></a>Application Insights no bot de aplicação web
Atualmente, o serviço de Application Insights, adicionado como parte da criação do serviço de web app bot, recolhe telemetria do Estado geral para o bot. Não recolhe informações de resposta de LUIS. Para analisar e melhorar LUIS, terá das informações de resposta LUIS.  

Para poder capturar a resposta de LUIS, tem de bot de aplicação web **[Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** instalado e configurado para o projeto. 

## <a name="download-web-app-bot"></a>Transferir bot de aplicação web
Utilize [Visual Studio 2017](https://www.visualstudio.com/downloads/) para adicionar e configurar o Application Insights para bot de aplicação web. Para utilizar o bot de aplicação web no Visual Studio, transferir o código de bot de aplicação web.

1. No portal do Azure, para bot de aplicação web, selecione **criar**.

    ![Selecione a compilação no portal](./media/luis-tutorial-bot-csharp-appinsights/download-build-menu.png)

2. Selecione **ficheiro zip de transferência** e aguarde até que o ficheiro está preparado.

    ![Transferir o ficheiro zip](./media/luis-tutorial-bot-csharp-appinsights/download-link.png)

3. Selecione **ficheiro zip de transferência** na janela de pop-up. Lembre-se a localização no seu computador, terá na secção seguinte.

    ![Transferir o pop-up de ficheiro zip](./media/luis-tutorial-bot-csharp-appinsights/download-popup.png)

## <a name="open-solution-in-visual-studio-2017"></a>Solução aberta no Visual Studio 2017

1. Extraia o ficheiro para uma pasta. 

2. Abra o Visual Studio 2017 e abra o ficheiro de solução, `Microsoft.Bot.Sample.LuisBot.sln`. Se o aviso de segurança foi aparece, selecione "OK".

    ![Solução aberta no Visual Studio 2017](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-security-warning.png)

3. Visual Studio tem de adicionar as dependências para a solução. No **Explorador de soluções**, faça duplo clique no **referências**e selecione **gerir pacotes NuGet...** . 

    ![Gerir pacotes NuGet](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-manage-nuget-packages.png)

4. O Gestor de pacotes NuGet mostra uma lista de pacotes instalados. Selecione **restaurar** na barra de amarela. Aguarde que o processo de restauro concluir.

    ![Restaurar pacotes NuGet](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-restore-packages.png)

## <a name="add-application-insights-to-the-project"></a>Adicionar o Application Insights ao projeto
Instalar e configurar o Application Insights no Visual Studio. 

1. No Visual Studio 2017, no menu superior, selecione **projeto**, em seguida, selecione **adicionar telemetria do Application Insights...** .

2. No **Application Insights configuração** janela, selecione **iniciar livre**

    ![Iniciar a configuração do Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configure-app-insights.png)

3. Registe a sua aplicação com o Application Insights. Poderá ter de introduzir as credenciais do portais do Azure. 

4. Visual Studio adiciona Application Insights ao projeto, que apresenta o estado como fazê-lo. 

    ![Estado do Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-adding-application-insights-to-project.png)

    Quando o processo for concluído, o **Application Insights configuração** mostra o estado do progresso. 

    ![Estado do progresso do Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configured-application-insights-to-project.png)

    O **ativar a recolha de rastreio** é vermelho, o que significa que não está ativada. Este tutorial não utiliza a funcionalidade. 

## <a name="build-and-resolve-errors"></a>Criar e resolver erros

1. Compilar a solução, selecionando o **criar** menu, em seguida, selecione **reconstruir solução**. Aguarde que a compilação concluir. 

2. Se a compilação falhar com `CS0104` erros, é necessário corrigir. No `Controllers` pasta, no `MessagesController.cs file`, corrija a utilização de ambígua `Activity` tipo por lhe o prefixo o tipo de atividade com o tipo de conector. Para fazer isto, altere o nome `Activity` em linhas 22 e 36 de `Activity` para `Connector.Activity`. Compilar a solução novamente. Não deverá haver nenhum mais erros de compilação.

    A origem completa que o ficheiro é:

    [!code-csharp[MessagesController.cs file](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/MessagesController.cs "MessagesController.cs file")]

## <a name="publish-project-to-azure"></a>Publicar o projeto no Azure
O **Application Insights** pacote está agora no projeto e configurado corretamente para as suas credenciais no portal do Azure. As alterações para o projeto tem de ser publicado para o Azure.

1. No **Explorador de soluções**, faça duplo clique no nome do projeto, em seguida, selecione **publicar**.

    ![Publicar o projeto no portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish.png)

2. No **publicar** janela, selecione **criar novo perfil**.

    ![Publicar o projeto no portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-1.png)

3. Selecione **importar perfil**e selecione **OK**.

    ![Publicar o projeto no portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-2.png)

4. No **publicar definições de ficheiro de importação de** windows, navegue até à pasta do projeto, navegue para o `PostDeployScripts` pasta, selecione o ficheiro que termine em `.PublishSettings`e selecione `Open`. Agora que configurou a publicação para este projeto. 

5. Publicar o seu código de origem local ao serviço Bot selecionando o **publicar** botão. O **saída** janela mostra o estado. O resto do tutorial está concluído no portal do Azure. Feche o Visual Studio 2017. 

## <a name="open-three-browser-tabs"></a>Abra os três separadores de browser
No portal do Azure, localize o bot de aplicação web e abri-lo. Os seguintes passos utilizam três vistas diferentes de bot de aplicação web. Poderá ser mais fácil de ter três separadores separados abrir no browser: 
  
>  * Teste no Chat na Web
>  * Editor de código online de compilação/Open-> Editor do serviço de aplicações
>  * Consola de serviço Editor/abra Kudu da aplicação -> consola diagnóstico

## <a name="modify-basicluisdialogcs-code"></a>Modificar BasicLuisDialog.cs código

1. No **Editor do serviço de aplicações** separador do browser, abra o `BasicLuisDialog.cs` ficheiro.

2. Adicione a seguinte dependência NuGet em existente `using` linhas:

   [!code-csharp[Add using statement](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=11-12 "Add using statement")]

3. Adicionar o `LogToApplicationInsights` função:

   [!code-csharp[Add the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=61-92 "Add the LogToApplicationInsights function")]

    A chave de instrumentação do Application Insights já está a ser o bot aplicação web aplicação definição denominada `BotDevInsightsKey`. 

    A última linha da função adiciona os dados para o Application Insights. Nome do rastreio é `LUIS`, um nome exclusivo para além dos outros dados de telemetria recolhida por esta bot de aplicação web. Todas as propriedades são também o prefixo `LUIS_` para que possa ver os dados que este tutorial adiciona compared com as informações que são fornecidas pela bot de aplicação web.

4. Chamar o `LogToApplicationInsights` função na parte superior do `ShowLuisResult` função:

   [!code-csharp[Use the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=114-115 "Use the LogToApplicationInsights function")]

## <a name="build-web-app-bot"></a>Criar bot de aplicação web
1. Crie bot de aplicação web de uma das duas formas. O primeiro método é com o botão direito no `build.cmd` no **Editor do serviço de aplicações**, em seguida, selecione **executar a partir da consola**. O resultado da consola apresenta e concluído com `Finished successfully.`

2. Se isto não concluir com êxito, terá de abrir a consola, navegue para o script e execute-o através dos seguintes passos. No **Editor do serviço de aplicações**, na barra superior blue, selecione o nome do seu bot, em seguida, selecione **abrir a consola do Kudu** na lista pendente.

    ![O Kudu abrir consola](./media/luis-tutorial-bot-csharp-appinsights/open-kudu-console.png)

3. Na janela da consola, introduza o seguinte comando:

    ```
    cd site\wwwroot && build.cmd
    ```

    Aguarde que a compilação concluir com `Finished successfully.`

## <a name="test-the-web-app-bot"></a>Testar bot de aplicação web

1. Para testar a sua bot de aplicação web, abra o **testar na Web Chat** funcionalidade no portal. 

2. Introduza a frase `Coffee bar on please`.  

    ![Testar bot de aplicação web no chat](./media/luis-tutorial-bot-csharp-appinsights/test-in-web-chat.png)

3. Não deverá ver nenhuma diferença na resposta chatbot. A alteração está a enviar dados para o Application Insights, não se encontra no bot respostas. Introduza alguns utterances mais, pelo que não existe um pequeno mais dados no Application Insights:

```
Please deliver a pizza
Turn off all the lights
Turn on the hall light
```

## <a name="view-luis-entries-in-application-insights"></a>Vista LUIS entradas no Application Insights
Abrir o Application Insights para ver as entradas de LUIS. 

1. No portal, selecione **todos os recursos** , em seguida, filtrar por nome de bot da aplicação web. Clique no recurso com o tipo **Application Insights**. O ícone para o Application Insights é uma lâmpada. 

    ![Pesquisa de informações de aplicação](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights.png)

2. Quando o recurso for aberto, clique em de **pesquisa** ícone da Lupa no painel de extremidade direita. Um novo painel para a direita apresenta. Dependendo da quantidade de dados telemétricos for encontrado, o painel poderá demorar um segundo para apresentar. Procurar `LUIS`. A lista é narrowed apenas LUIS os resultados de consulta adicionada com este tutorial.

    ![Procurar rastreios](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace.png)

3. Selecione a entrada superior. Uma nova janela apresenta dados de mais detalhados, incluindo os dados personalizados para a consulta de LUIS na extremidade direita. Os dados incluem a intenção superior e a respetiva classificação.

    ![Reveja o item de rastreio](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace-item.png)

    Quando tiver terminado, selecione a parte superior direita **X** para regressar à lista de itens de dependência. 


> [!Tip]
> Se pretender guardar a lista de dependência e regressar ao mesmo mais tarde, clique em **... Mais** e clique em **guardar favorito**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Consultar o Application Insights para intenção, pontuação e utterance
Application Insights dá-lhe a capacidade de consultar os dados com o [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) idioma, assim como a exportação para [PowerBI](https://powerbi.microsoft.com). 

1. Clique em **análise** no topo da dependência listagem acima caixa do filtro. 

    ![Botão Análise](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-analytics-button.png)

2. Abre uma nova janela com uma janela de consulta na parte superior e uma janela de tabela de dados seguintes aqueles que. Se tiver utilizado o bases de dados antes desta disposição está familiarizada. A consulta inclui todos os itens das últimas 24 horas, começando com o nome `LUIS`. O **CustomDimensions** coluna tem os resultados da consulta LUIS como pares nome/valor.

    ![Relatório de análise predefinida](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-1.png)

3. Para solicitar a parte superior intenção, pontuação e utterance, adicionam o seguinte apenas acima da última linha na janela de consulta:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

4. Execute a consulta. Desloque-se na extremidade direita na tabela de dados. Novas colunas de topIntent, pontuação e utterance estão disponíveis. Clique na coluna topIntent para ordenar.

    ![Relatório de análise personalizada](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-2.png)


Saiba mais sobre o [idioma de consulta Kusto](https://docs.loganalytics.io/docs/Learn/Getting-Started/Getting-started-with-queries) ou [exportar os dados ao Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Saiba mais sobre a arquitetura de Bot
Saiba mais sobre [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Passos Seguintes

Outras informações que pretende adicionar aos dados do application insights incluem o ID da aplicação, ID da versão, data da última alteração modelo, data da última formação, a última data de publicação. Estes valores está podem ser obtidos a partir do URL de ponto final (ID de aplicação e o ID de versão) ou a partir de um [criação API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d) chamada, em seguida, definir nas definições de bot da aplicação web e solicitados a partir daí.  

Se estiver a utilizar a mesma subscrição de ponto final para a mais do que uma aplicação de LUIS, também deve incluir o ID de subscrição e uma propriedade a indicar que é uma chave partilhada. 

> [!div class="nextstepaction"]
> [Saiba mais sobre utterances de exemplo](luis-how-to-add-example-utterances.md)
