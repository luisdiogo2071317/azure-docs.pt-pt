---
title: 'Tutorial 8: extração de expressões-chave no LUIS'
titleSuffix: Azure Cognitive Services
description: Utilize a entidade keyPhrase pré-concebida para extrair o assunto-chave das expressões. Não é necessário etiquetar expressões com entidades pré-concebidas. A entidade é detetada automaticamente.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: d6feb24b1e59aee70204d8438f1a4c51f71d1835
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281817"
---
# <a name="tutorial-8-extract-key-phrases-of-utterance"></a>Tutorial 8: extrair expressões-chave da expressão
Neste tutorial, vai utilizar a entidade keyPhrase pré-concebida para extrair o assunto-chave das expressões. Não é necessário etiquetar expressões com entidades pré-concebidas. A entidade é detetada automaticamente.

As expressões seguintes mostram exemplos de expressões-chave:

|Expressão|Valores da entidade KeyPhrase|
|--|--|
|Existe um novo plano médico com um valor deduzível inferior oferecido no próximo ano?|"deduzível inferior"<br>"novo plano médico"<br>"ano"|
|A terapia de visão está incluída no plano médico deduzível?|"plano médico deduzível"<br>"terapia de visão"|

A sua aplicação cliente pode utilizar estes valores, juntamente com outras entidades extraídas, para decidir o próximo passo da conversa.

**Neste tutorial, ficará a saber como:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Utilizar a aplicação de tutorial existente
> * Adicionar a entidade KeyPhrase 
> * Preparar
> * Publicar
> * Obter as intenções e as entidades do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente

Continue com a aplicação criada no último tutorial, com o nome **RecursosHumanos**. 

Se não tiver a aplicação RecursosHumanos do tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde o [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-simple-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Na secção **Gerir**, no separador **Versões**, clone a versão e dê-lhe o nome `keyphrase`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter carateres que não sejam válidos num URL.

## <a name="add-keyphrase-entity"></a>Adicionar a entidade KeyPhrase 
Adicione a entidade pré-concebida keyPhrase para extrair o assunto das expressões.

1. [!INCLUDE[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecione **Entidades** no menu esquerdo.

3. Selecione **Adicionar entidade pré-compilada**.

4. Na caixa de diálogo de pop-up, selecione **keyPhrase** e, em seguida, **Concluído**. 

    [ ![Captura de ecrã da caixa de diálogo de pop-up Lista de entidades](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png)](./media/luis-quickstart-intent-and-key-phrase/hr-add-or-remove-prebuilt-entities.png#lightbox)

5. Selecione **Intenções** no menu do lado esquerdo e, em seguida, selecione a intenção **Utilities.Confirm**. A entidade keyPhrase está etiquetada em várias expressões. 

    [ ![Captura de ecrã da intenção Utilities.Confirm com keyPhrases etiquetada nas expressões](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png)](./media/luis-quickstart-intent-and-key-phrase/hr-keyphrase-labeled.png#lightbox)

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obter as intenções e as entidades do ponto final

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `does form hrf-123456 cover the new dental benefits and medical plan`. O último parâmetro querystring é `q`, a expressão **query**. 
    
    ```JSON
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

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes

Neste tutorial, adicionou a entidade keyPhrase pré-concebida, fornecendo rapidamente expressões-chave em expressões sem ter de etiquetar as expressões. 

> [!div class="nextstepaction"]
> [Adicionar análise de sentimentos a uma aplicação](luis-quickstart-intent-and-sentiment-analysis.md)