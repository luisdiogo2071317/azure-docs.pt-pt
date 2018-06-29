---
title: Tutorial para criar uma aplicação LUIS que devolva análise de sentimentos - Azure | Microsoft Docs
description: Neste tutorial, aprenda a adicionar a análise de sentimentos à sua aplicação LUIS para analisar expressões para sentimentos positivos, negativos e neutros.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: d000637312619fc493e2f7bad8e8edf0d8d0d94b
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265339"
---
# <a name="tutorial-create-app-that-returns-sentiment-along-with-intent-prediction"></a>Tutorial: Criar uma aplicação que devolva sentimentos juntamente com a predição de intenção
Neste tutorial, vai criar uma aplicação que demonstra como extrair sentimentos positivos, negativos e neutros de expressões.

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades hierárquicas e os elementos subordinados com contexto adquirido 
> * Criar uma nova aplicação LUIS para um domínio de viagens com a intenção Bookflight
> * Adicionar a intenção _None_ (Nenhuma) e adicionar expressões de exemplo
> * Adicionar a entidade hierárquica de localização com elementos subordinados de origem e de destino
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS incluindo elementos subordinados hierárquicos 

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="sentiment-analysis"></a>Análise de sentimentos
A análise de sentimentos é a capacidade de determinar se a expressão de um utilizador é positiva, negativa ou neutra. 

As expressões seguintes mostram exemplos de sentimentos:

|Sentimento e pontuação|Expressão|
|:--|--|
|positiva - 0,89 |A combinação de sopa e salada foi excelente.|
|negativa - 0,07 |Não gostei da entrada durante o jantar.|

A análise de sentimentos é como uma definição da aplicação que se aplica a cada expressão. Não é necessário localizar as palavras que indicam o sentimento na expressão e etiquetá-las. O LUIS vai fazê-lo por si.

## <a name="create-a-new-app"></a>Criar uma nova aplicação
1. Inicie sessão no site do [LUIS][LUIS]. Certifique-se de que inicia sessão na [região][LUIS-regions] onde precisa que os pontos finais do LUIS sejam publicados.

2. No site do [LUIS][LUIS], selecione **Create new app** (Criar nova aplicação). 

    [![](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png "Captura de ecrã da página Listas de aplicações")](media/luis-quickstart-intent-and-sentiment-analysis/app-list.png#lightbox)

3. Na caixa de diálogo **Criar nova aplicação**, dê um nome à aplicação `Restaurant Reservations With Sentiment` e selecione **Concluído**. 

    ![Imagem da caixa de diálogo Criar nova aplicação](./media/luis-quickstart-intent-and-sentiment-analysis/create-app-ddl.png)

    Quando tiver concluído o processo de criação da aplicação, o LUIS apresenta a lista de intenções que contém a intenção None.

    [![](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png "Captura de ecrã da página Listas de intenções")](media/luis-quickstart-intent-and-sentiment-analysis/intents-list.png#lightbox)

## <a name="add-a-prebuilt-domain"></a>Adicionar um domínio pré-concebido
Adicione um domínio pré-concebido para adicionar rapidamente intenções, entidades e expressões etiquetadas.

1. Selecione **Domínios Pré-concebidos** no menu esquerdo.

    [ ![Captura de ecrã do botão Domínio Pré-concebido](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-button-expanded.png#lightbox)

2. Selecione **Adicionar domínio** para o domínio pré-concebido **RestaurantReservation**. Aguarde até o domínio ser adicionado.

    [ ![Captura de ecrã da lista Domínio Pré-concebido](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-expanded.png#lightbox)

3. Selecione **Intenções** no painel de navegação esquerdo. Este domínio pré-concebido tem uma intenção.

    [ ![Captura de ecrã da lista Domínio pré-concebido com as Intenções realçadas no painel de navegação esquerdo](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png)](./media/luis-quickstart-intent-and-sentiment-analysis/prebuilt-domains-list-domain-added-expanded.png#lightbox)

4.  Selecione a intenção **RestaurantReservation.Reserve**. 

    [ ![Captura de ecrã da lista Intenções com RestaurantReservation.Reserve realçado](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/select-intent.png#lightbox)

5. Ative/desative a **Vista de Entidades** para ver as inúmeras expressões fornecidas com entidades específicas do domínio etiquetadas.

    [ ![Captura de ecrã da intenção RestaurantReservation.Reserve com a Vista de Entidades ativada/desativada para a Vista de Tokens realçada](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-inline.png)](./media/luis-quickstart-intent-and-sentiment-analysis/utterance-list-expanded.png#lightbox)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações às intenções e entidades (o modelo), até ser preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Captura de ecrã do botão Preparar realçado](./media/luis-quickstart-intent-and-sentiment-analysis/train-button-expanded.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Captura de ecrã da barra de notificação de êxito da Preparação ](./media/luis-quickstart-intent-and-sentiment-analysis/trained-expanded.png)

## <a name="configure-app-to-include-sentiment-analysis"></a>Configurar a aplicação para incluir análise de sentimentos
A análise de sentimentos está ativada na página **Publicar**. 

1. Selecione **Publicar** no painel de navegação superior direito.

    ![Captura de ecrã da página Intenção com o botão Publicar expandido ](./media/luis-quickstart-intent-and-sentiment-analysis/publish-expanded.png)

2. Selecione **Ativar Análise de Sentimentos**.

    ![Captura de ecrã da página Publicar com Ativar Análise de Sentimentos realçado ](./media/luis-quickstart-intent-and-sentiment-analysis/enable-sentiment-expanded.png)

3. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-inline.png "Captura de ecrã da página Publicar com o botão Publicar no bloco de produção realçado")](media/luis-quickstart-intent-and-sentiment-analysis/publish-to-production-expanded.png#lightbox)

4. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-an-utterance"></a>Consultar o ponto final com uma expressão

1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    ![Captura de ecrã da página Publicar com o URL de ponto final realçado](media/luis-quickstart-intent-and-sentiment-analysis/endpoint-url-inline.png)

2. Vá para o final do URL no endereço e introduza `Reserve table for  10 on upper level away from kitchen`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões etiquetadas, pelo que é um bom teste e deve devolver a intenção `RestaurantReservation.Reserve` com a análise de sentimentos extraída.

```
{
  "query": "Reserve table for 10 on upper level away from kitchen",
  "topScoringIntent": {
    "intent": "RestaurantReservation.Reserve",
    "score": 0.9926384
  },
  "intents": [
    {
      "intent": "RestaurantReservation.Reserve",
      "score": 0.9926384
    },
    {
      "intent": "None",
      "score": 0.00961109251
    }
  ],
  "entities": [],
  "sentimentAnalysis": {
    "label": "neutral",
    "score": 0.5
  }
}
```

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com a análise de sentimentos ativada, identificou uma intenção de consulta de linguagem natural e devolveu os dados extraídos, incluindo o sentimento geral como uma pontuação. 

O chatbot tem agora informações suficientes para determinar o passo seguinte na conversação. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados de sentimento da expressão para efetuar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Delete** (Eliminar). Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"] 
> [Chamar a API de ponto final do LUIS com C#](luis-get-started-cs-get-intent.md) 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
