---
title: Ligar a pesquisa do Bing - as do Azure Logic Apps | Microsoft Docs
description: Localizar notícias com APIs de REST de pesquisa do Bing e Azure Logic Apps
author: ecfan
manager: jeconnoc
ms.author: estfan
ms.date: 05/21/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 87045d5dbbc1221a770e44bd9e9cf2451a9ac522
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/11/2018
ms.locfileid: "35295541"
---
# <a name="find-news-with-bing-search-and-azure-logic-apps"></a>Localizar notícias de última hora com a pesquisa do Bing e Azure Logic Apps 

Este artigo mostra como pode encontrar notícias de última hora, vídeos e outros itens através da pesquisa do Bing de dentro de uma aplicação lógica com o conector de pesquisa do Bing. Dessa forma, pode criar as logic apps que automatizem tarefas e fluxos de trabalho para processamento de resultados da pesquisa e tornar esses itens disponíveis para outras ações. 

Por exemplo, pode localizar itens de notícias de última hora com base nos critérios de pesquisa e tem de publicar esses itens como tweets no seu Twitter feed do Twitter.

Se não tiver uma subscrição do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se para obter uma conta do Azure gratuita</a>. Se estiver familiarizado com as logic apps, reveja [que é o Azure Logic Apps](../logic-apps/logic-apps-overview.md) e [início rápido: criar a sua primeira aplicação de lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).
Para obter informações técnicas específicas do conector, consulte o <a href="https://docs.microsoft.com/connectors/bingsearch/" target="blank">referência de conector de pesquisa do Bing</a>.

## <a name="prerequisites"></a>Pré-requisitos

* A [conta de serviços cognitivos](../cognitive-services/cognitive-services-apis-create-account.md)

* A [chave de API de pesquisa do Bing](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api), que fornece acesso a partir da sua aplicação lógica para as APIs de pesquisa do Bing 

* A aplicação lógica onde pretende aceder ao seu Hub de eventos. Para iniciar a sua aplicação lógica com um acionador de pesquisa do Bing, é necessário um [aplicação lógica em branco](../logic-apps/quickstart-create-first-logic-app-workflow.md). 

<a name="add-trigger"></a>

## <a name="add-a-bing-search-trigger"></a>Adicionar um acionador de pesquisa do Bing

No Azure Logic Apps, cada aplicação lógica tem de começar com uma [acionador](../logic-apps/logic-apps-overview.md#logic-app-concepts), que é desencadeado quando um evento específico acontece ou quando for cumprida uma condição específica. Sempre que o acionador é desencadeado, o motor de Logic Apps cria uma instância de aplicação lógica e inicia a executar o fluxo de trabalho da sua aplicação.

1. No portal do Azure ou Visual Studio, crie uma aplicação de lógica em branco, o qual abre o Designer de aplicação lógica. Este exemplo utiliza o portal do Azure.

2. Na caixa de pesquisa, introduza "Pesquisa do Bing" como o filtro. Na lista de acionadores, selecione o acionador que quiser. 

   Este exemplo utiliza este acionador: **pesquisa do Bing - no novo artigo de notícias de última hora**

   ![Localizar o acionador de pesquisa do Bing](./media/connectors-create-api-bing-search/add-trigger.png)

3. Se lhe for pedida para detalhes de ligação, [criar a ligação de pesquisa do Bing agora](#create-connection). Em alternativa, se a ligação já existir, forneça as informações necessárias para o acionador.

   Neste exemplo, forneça critérios para regressar correspondente artigos de notícias de última hora de pesquisa do Bing.

   | Propriedade | Necessário | Valor | Descrição | 
   |----------|----------|-------|-------------| 
   | Consulta de Pesquisa | Sim | <*palavras de pesquisa*> | Introduza as palavras-chave de pesquisa que pretende utilizar. |
   | Comercializar | Sim | <*Região*> | A região de pesquisa. A predefinição é "en-US" mas, pode selecionar outro valor. | 
   | Pesquisa segura | Sim | <*nível de pesquisa*> | O nível de filtro para excluir o conteúdo para adultos. A predefinição é "Moderada", mas que selecionar outro nível. | 
   | Contagem | Não | <*Contagem de resultados*> | Devolva o número especificado de resultados. A predefinição é 20, mas pode especificar outro valor. O número real de resultados devolvidos pode ser inferior ao número especificado. | 
   | Desvio | Não | <*Ignorar-valor*> | O número de resultados para ignorar antes de o devolver resultados | 
   ||||| 

   Por exemplo:

   ![Configurar o acionador](./media/connectors-create-api-bing-search/bing-search-trigger.png)

4. Selecione o intervalo de frequência e para a frequência com que pretende que o acionador para verificar os resultados.

5. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**. 

6. Agora continue a adicionar uma ou mais ações à sua aplicação lógica para as tarefas que pretende executar com os resultados de Acionador.

<a name="add-action"></a>

## <a name="add-a-bing-search-action"></a>Adicionar uma ação de pesquisa do Bing

No Azure Logic Apps, um [ação](../logic-apps/logic-apps-overview.md#logic-app-concepts) é um passo no fluxo de trabalho que se segue um acionador ou outra ação. Neste exemplo, a aplicação lógica começa com um acionador de pesquisa do Bing devolve novos artigos que correspondam aos critérios especificados. 

1. No portal do Azure ou Visual Studio, abra a aplicação de lógica no Designer de aplicação lógica. Este exemplo utiliza o portal do Azure.

2. Sob o acionador ou ação, escolha **novo passo** > **adicionar uma ação**.

   Este exemplo utiliza este acionador: **pesquisa do Bing - no novo artigo de notícias de última hora**

   ![Adicionar ação](./media/connectors-create-api-bing-search/add-action.png)

   Para adicionar uma ação entre passos existentes, mova o rato sobre a seta para a ligação. 
   Escolha o sinal de adição (**+**) que é apresentado e, em seguida, escolha **adicionar uma ação**.

3. Na caixa de pesquisa, introduza "Pesquisa do Bing" como o filtro.
Na lista de ações, selecione a ação que pretende.

   Este exemplo utiliza esta ação: **pesquisa do Bing - notícias de lista pela consulta**

   ![Encontrar a ação de pesquisa do Bing](./media/connectors-create-api-bing-search/bing-search-select-action.png)

4. Se lhe for pedida para detalhes de ligação, [criar a ligação de pesquisa do Bing agora](#create-connection). Em alternativa, se a ligação já existir, forneça as informações necessárias para a ação. 

   Neste exemplo, forneça os critérios para devolver um subconjunto dos resultados do acionador.

   | Propriedade | Necessário | Valor | Descrição | 
   |----------|----------|-------|-------------| 
   | Consulta de Pesquisa | Sim | <*expressão de pesquisa*> | Introduza uma expressão para consultar os resultados de Acionador. Pode selecionar do om campos na lista de conteúdo dinâmica ou crie uma expressão com o construtor de expressão. |
   | Comercializar | Sim | <*Região*> | A região de pesquisa. A predefinição é "en-US" mas, pode selecionar outro valor. | 
   | Pesquisa segura | Sim | <*nível de pesquisa*> | O nível de filtro para excluir o conteúdo para adultos. A predefinição é "Moderada", mas que selecionar outro nível. | 
   | Contagem | Não | <*Contagem de resultados*> | Devolva o número especificado de resultados. A predefinição é 20, mas pode especificar outro valor. O número real de resultados devolvidos pode ser inferior ao número especificado. | 
   | Desvio | Não | <*Ignorar-valor*> | O número de resultados para ignorar antes de o devolver resultados | 
   ||||| 

   Por exemplo, suponha que pretende que esses resultados cujo nome de categoria inclui a palavra "técnico". 
   
   1. Clique no **consulta de pesquisa** caixa, pelo que é apresentada a lista de conteúdo dinâmica. 
   Essa lista, escolha **expressão** para o construtor de expressão é apresentada. 

      ![Acionador de pesquisa do Bing](./media/connectors-create-api-bing-search/bing-search-action.png)

      Agora pode começar a criar a expressão.

   2. Na lista de funções, selecione o **contains()** função, o que, em seguida, é apresentado na caixa de expressão. Clique em **conteúdo dinâmico** para que a lista de campos reappears, mas certifique-se de que o cursor permanece dentro de parênteses.

      ![Selecione uma função](./media/connectors-create-api-bing-search/expression-select-function.png)

   3. Na lista de campos, selecione **categoria**, que converte um parâmetro. 
   Adicionar uma vírgula, após o primeiro parâmetro e, após a vírgula, adicione esta palavra: `'tech'` 

      ![Selecione um campo](./media/connectors-create-api-bing-search/expression-select-field.png)
   
   4. Quando tiver terminado, escolha **OK**.

      A expressão agora aparece no **consulta de pesquisa** caixa neste formato:

      ![Expressão concluído](./media/connectors-create-api-bing-search/resolved-expression.png)

      Na vista de código, esta expressão for apresentada neste formato:

      `"@{contains(triggerBody()?['category'],'tech')}"`

5. Quando tiver terminado, na barra de ferramentas estruturador, escolha **guardar**.

<a name="create-connection"></a>

## <a name="connect-to-bing-search"></a>Ligar a pesquisa do Bing

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)] 

1. Quando lhe for pedida informações de ligação, forneça estes detalhes:

   | Propriedade | Necessário | Valor | Descrição | 
   |----------|----------|-------|-------------| 
   | Nome da Ligação | Sim | <*nome da ligação*> | O nome para criar para a ligação |
   | Versão de API | Sim | <*Versão de API*> | Por predefinição, a versão da API de pesquisa do Bing está definida para a versão atual. Pode selecionar uma versão anterior, conforme necessário. | 
   | Chave de API | Sim | <*Chave de API*> | A chave de API de pesquisa do Bing que obteve anteriormente. Se não tiver uma chave, obter o [agora a chave de API](https://azure.microsoft.com/try/cognitive-services/?api=bing-news-search-api). |  
   |||||  

   Por exemplo:

   ![Criar ligação](./media/connectors-create-api-bing-search/bing-search-create-connection.png)

2. Quando tiver terminado, escolha **Create** (Criar).

## <a name="connector-reference"></a>Referência do conector

Para obter detalhes técnicos, como acionadores, ações e limites, conforme descrito pelo ficheiro Swagger do conector, consulte o [página de referência do conector](/connectors/bingsearch/). 

## <a name="get-support"></a>Obter suporte

* Relativamente a dúvidas, visite o [fórum do Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para submeter ou votar em ideias para funcionalidades, visite o [site de comentários dos utilizadores do Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Passos seguintes

* Saiba mais sobre outros [conectores Logic Apps](../connectors/apis-list.md)