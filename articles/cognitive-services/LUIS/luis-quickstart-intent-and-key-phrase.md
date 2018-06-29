---
title: Tutorial para criar uma aplicação LUIS que devolva expressões-chave - Azure | Microsoft Docs
description: Neste tutorial, aprenda a adicionar e devolver a entidade keyPhrase para a sua aplicação LUIS para analisar expressões para o assunto-chave.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 12c306b5199da5862302c28d1690b81c6e1edb0e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264620"
---
# <a name="tutorial-create-app-that-returns-keyphrases-entity-data-found-in-utterances"></a>Tutorial: Criar uma aplicação que devolva dados da entidade keyPhrases encontrados em expressões
Neste tutorial, vai criar uma aplicação que demonstra como extrair o assunto-chave de expressões.

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades keyPhrase 
> * Criar nova aplicação LUIS para o domínio de recursos humanos
> * Adicionar a intenção _None_ (Nenhuma) e adicionar expressões de exemplo
> * Adicionar a entidade keyPhrase para extrair conteúdo da expressão
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS

Para este artigo, pode utilizar uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="keyphrase-entity-extraction"></a>Extração da entidade keyPhrase
O assunto-chave é fornecido pela entidade Prebuilt, **keyPhrase**. Esta entidade devolve o assunto-chave na expressão

As expressões seguintes mostram exemplos de expressões-chave:

|Expressão|Valores da entidade KeyPhrase|
|--|--|
|Existe um novo plano médico com um valor deduzível inferior oferecido no próximo ano?|"deduzível inferior"<br>"novo plano médico"<br>"ano"|
|A terapia de visão está incluída no plano médico deduzível?|"plano médico deduzível"<br>"terapia de visão"|

O chatbot pode considerar estes valores, além de todas as outras entidades extraídas, quando decidir o passo seguinte na conversação.

## <a name="download-sample-app"></a>Transferir aplicação de exemplo
Transfira a aplicação [Recursos Humanos](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/HumanResources.json) e guarde-a num ficheiro com a extensão *.json. Esta aplicação de exemplo reconhece as expressões relevantes para benefícios de funcionários, organogramas e recursos físicos.

## <a name="create-a-new-app"></a>Criar uma nova aplicação
1. Inicie sessão no site do [LUIS][LUIS]. Certifique-se de que inicia sessão na [região][LUIS-regions] onde precisa que os pontos finais do LUIS sejam publicados.

2. No site do [LUIS][LUIS], selecione **Importar nova aplicação** para importar a aplicação Recursos Humanos transferida na secção anterior. 

    [![](media/luis-quickstart-intent-and-key-phrase/app-list.png "Captura de ecrã da página Listas de aplicações")](media/luis-quickstart-intent-and-key-phrase/app-list.png#lightbox)

3. Na caixa de diálogo **Importar nova aplicação**, dê um nome à aplicação `Human Resources with Key Phrase entity`. 

    ![Imagem da caixa de diálogo Criar nova aplicação](./media/luis-quickstart-intent-and-key-phrase/import-new-app-inline.png)

    Quando tiver concluído o processo de criação da aplicação, o LUIS apresenta a lista de intenções.

    [![](media/luis-quickstart-intent-and-key-phrase/intents-list.png "Captura de ecrã da página Listas de intenções")](media/luis-quickstart-intent-and-key-phrase/intents-list.png#lightbox)

## <a name="add-keyphrase-entity"></a>Adicionar a entidade KeyPhrase 
Adicione a entidade pré-concebida keyPhrase para extrair o assunto das expressões.

1. Selecione **Entidades** no menu esquerdo.

    [ ![Captura de ecrã de Entidades realçado no painel de navegação esquerdo da secção Criar](./media/luis-quickstart-intent-and-key-phrase/select-entities.png)](./media/luis-quickstart-intent-and-key-phrase/select-entities.png#lightbox)

2. Selecione **Gerir entidades pré-concebidas**.

    [ ![Captura de ecrã da caixa de diálogo de pop-up Lista de entidades](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/manage-prebuilt-entities.png#lightbox)

3. Na caixa de diálogo de pop-up, selecione **keyPhrase** e, em seguida, **Concluído**. 

    [ ![Captura de ecrã da caixa de diálogo de pop-up Lista de entidades](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece esta alteração ao modelo até estar preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Captura de ecrã do botão Preparar realçado](./media/luis-quickstart-intent-and-key-phrase/train-button-expanded.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Captura de ecrã da barra de notificação de êxito da Preparação ](./media/luis-quickstart-intent-and-key-phrase/trained-inline.png)

## <a name="publish-app-to-endpoint"></a>Publicar a aplicação no ponto final

1. Selecione **Publicar** no painel de navegação superior direito.

    ![Captura de ecrã da página Entidade com o botão Publicar expandido ](./media/luis-quickstart-intent-and-key-phrase/publish-expanded.png)

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-key-phrase/publish-to-production-inline.png "Captura de ecrã da página Publicar com o botão Publicar no bloco de produção realçado")](media/luis-quickstart-intent-and-key-phrase/publish-to-production-expanded.png#lightbox)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-an-utterance"></a>Consultar o ponto final com uma expressão

1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    ![Captura de ecrã da página Publicar com o URL de ponto final realçado](media/luis-quickstart-intent-and-key-phrase/endpoint-url-inline.png )

2. Vá para o final do URL no endereço e introduza `Is there a new medical plan with a lower deductible offered next year?`. O último parâmetro querystring é `q`, a expressão **query**. 

```
{
  "query": "Is there a new medical plan with a lower deductible offered next year?",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.216838628
  },
  "entities": [
    {
      "entity": "lower deductible",
      "type": "builtin.keyPhrase",
      "startIndex": 35,
      "endIndex": 50
    },
    {
      "entity": "new medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 26
    },
    {
      "entity": "year",
      "type": "builtin.keyPhrase",
      "startIndex": 65,
      "endIndex": 68
    }
  ]
}
```

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com a deteção da entidade keyPhrase, identificou uma intenção de consulta de linguagem natural e devolveu os dados extraídos, incluindo o assunto principal. 

O chatbot tem agora informações suficientes para determinar o passo seguinte na conversação. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados de keyPhrase da expressão para efetuar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Delete** (Eliminar). Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Criar uma aplicação que devolva sentimentos juntamente com a predição de intenção](luis-quickstart-intent-and-sentiment-analysis.md)

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
