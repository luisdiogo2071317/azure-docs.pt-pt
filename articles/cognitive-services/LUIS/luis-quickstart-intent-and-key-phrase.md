---
title: Tutorial para criar uma aplicação LUIS que devolva expressões-chave - Azure | Microsoft Docs
description: Neste tutorial, aprenda a adicionar e devolver a entidade keyPhrase para a sua aplicação LUIS para analisar expressões para o assunto-chave.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: d5b92681b9677db085b3c271c37490154d781d84
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494315"
---
# <a name="tutorial-8-add-keyphrase-entity"></a>Tutorial: 8. Adicionar a entidade KeyPhrase 
Neste tutorial, vai utilizar uma aplicação que demonstra como extrair o assunto-chave de expressões.

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades keyPhrase 
> * Utilizar a aplicação LUIS no domínio de Recursos Humanos (RH) 
> * Adicionar a entidade keyPhrase para extrair conteúdo da expressão
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS incluindo expressões-chave

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de Recursos Humanos do tutorial [entidade simples](luis-quickstart-primary-and-secondary-data.md), [importe](luis-how-to-start-new-app.md#import-new-app) o JSON para uma nova aplicação no site do [LUIS](luis-reference-regions.md#luis-website). A aplicação a importar está no repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-simple-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `keyphrase`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="keyphrase-entity-extraction"></a>Extração da entidade keyPhrase
O assunto-chave é indicado pela entidade previamente criada, **keyPhrase**. Esta entidade devolve o assunto-chave na expressão.

As expressões seguintes mostram exemplos de expressões-chave:

|Expressão|Valores da entidade KeyPhrase|
|--|--|
|Existe um novo plano médico com um valor deduzível inferior oferecido no próximo ano?|"deduzível inferior"<br>"novo plano médico"<br>"ano"|
|A terapia de visão está incluída no plano médico deduzível?|"plano médico deduzível"<br>"terapia de visão"|

A sua aplicação cliente pode utilizar estes valores, juntamente com outras entidades extraídas, para decidir o próximo passo da conversa.

## <a name="add-keyphrase-entity"></a>Adicionar a entidade KeyPhrase 
Adicione a entidade pré-concebida keyPhrase para extrair o assunto das expressões.

1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

2. Selecione **Entidades** no menu esquerdo.

    [ ![Captura de ecrã de Entidades realçado no painel de navegação esquerdo da secção Criar](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png)](./media/luis-quickstart-intent-and-key-phrase/hr-select-entities-button.png#lightbox)

3. Selecione **Gerir entidades pré-concebidas**.

    [ ![Captura de ecrã da caixa de diálogo de pop-up Lista de entidades](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-manage-prebuilt-entities.png#lightbox)

4. Na caixa de diálogo de pop-up, selecione **keyPhrase** e, em seguida, **Concluído**. 

    [ ![Captura de ecrã da caixa de diálogo de pop-up Lista de entidades](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

    <!-- TBD: asking Carol
    You won't see these entities labeled in utterances on the intents pages. 
    -->
5. Selecione **Intenções** no menu do lado esquerdo e, em seguida, selecione a intenção **Utilities.Confirm**. A entidade keyPhrase está etiquetada em várias expressões. 

    [ ![Captura de ecrã da intenção Utilities.Confirm com keyPhrases etiquetada nas expressões](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Publicar a aplicação no ponto final

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]


## <a name="query-the-endpoint-with-an-utterance"></a>Consultar o ponto final com uma expressão

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `does form hrf-123456 cover the new dental benefits and medical plan`. O último parâmetro querystring é `q`, a expressão **query**. 

```
{
  "query": "does form hrf-123456 cover the new dental benefits and medical plan",
  "topScoringIntent": {
    "intent": "FindForm",
    "score": 0.9300641
  },
  "intents": [
    {
      "intent": "FindForm",
      "score": 0.9300641
    },
    {
      "intent": "ApplyForJob",
      "score": 0.0359598845
    },
    {
      "intent": "GetJobInformation",
      "score": 0.0141798034
    },
    {
      "intent": "MoveEmployee",
      "score": 0.0112197418
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00507669244
    },
    {
      "intent": "None",
      "score": 0.00238501839
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00202810857
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.00102957746
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0008688423
    },
    {
      "intent": "Utilities.Confirm",
      "score": 3.557994E-05
    }
  ],
  "entities": [
    {
      "entity": "hrf-123456",
      "type": "HRF-number",git 
      "startIndex": 10,
      "endIndex": 19
    },
    {
      "entity": "new dental benefits",
      "type": "builtin.keyPhrase",
      "startIndex": 31,
      "endIndex": 49
    },
    {
      "entity": "medical plan",
      "type": "builtin.keyPhrase",
      "startIndex": 55,
      "endIndex": 66
    },
    {
      "entity": "hrf",
      "type": "builtin.keyPhrase",
      "startIndex": 10,
      "endIndex": 12
    },
    {
      "entity": "-123456",
      "type": "builtin.number",
      "startIndex": 13,
      "endIndex": 19,
      "resolution": {
        "value": "-123456"
      }
    }
  ]
}
```

Ao pesquisar por um formulário, o utilizador deu mais informações do que era preciso para o encontrar. A informação adicional é devolvida como **builtin.keyPhrase**. A aplicação cliente pode utilizar estas informações adicionais para uma pergunta de seguimento, como "Gostaria de falar com um representante dos Recursos Humanos sobre os novos benefícios dentários" ou oferecer um menu com mais opções, incluindo "Mais informações sobre novos benefícios dentários ou plano médico."

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com a deteção da entidade keyPhrase, identificou uma intenção de consulta de linguagem natural e devolveu os dados extraídos, incluindo o assunto principal. 

O chatbot tem agora informações suficientes para determinar o passo seguinte na conversação. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados de keyPhrase da expressão para efetuar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicionar análise de sentimentos a uma aplicação](luis-quickstart-intent-and-sentiment-analysis.md)