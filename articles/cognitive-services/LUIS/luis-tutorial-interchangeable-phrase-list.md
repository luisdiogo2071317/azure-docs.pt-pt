---
title: Tutorial com uma lista de expressão para melhorar as predições LUIS - Azure | Microsoft Docs
description: Neste tutorial, adicione uma lista de expressão para uma aplicação LUIS e ver como a melhoria da pontuação.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: feb8acb674fd2dc62b62c26da6a6b42515f30242
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265976"
---
# <a name="tutorial-add-phrase-list-to-improve-predictions"></a>Tutorial: Adicionar a lista de expressão para melhorar as predições
Neste tutorial, melhorar a exatidão da intenção pontuações e identificar entidades para palavras que têm o mesmo significado (sinónimos), adicionando um permutáveis [funcionalidade da lista de expressão](./luis-concept-feature.md).

> [!div class="checklist"]
* Importar uma nova aplicação  
* Ponto final de consulta com utterance conhecido 
* Consultar o ponto final com _desconhecido_ utterance
* Adicionar a lista de expressão para melhorar a pontuação utterance desconhecido
* Certifique-se de que é encontrar a entidade ao utilizar a lista de expressão

Para este artigo, precisa de um livre [LUIS] [ LUIS] conta para criar a sua aplicação LUIS.

## <a name="import-a-new-app"></a>Importar uma nova aplicação
1. Transferir o [exemplo LUIS aplicação] [ LuisSampleApp] que foi concebido para este tutorial. Irá utilizá-lo no próximo passo. 

2. Conforme descrito em [criar uma aplicação](Create-new-app.md#import-new-app), importar o ficheiro que transferiu para a [LUIS] [ LUIS] Web site como uma nova aplicação. O nome da aplicação é "Os meus tutorial da lista de expressão." Tem utterances, entidades e pendentes. 

3. [Formação](luis-how-to-train.md) a aplicação. Até que está preparado, não é possível [interativamente testar](interactive-test.md#interactive-testing) -na [LUIS] [ LUIS] Web site. 

4. No [publicar](PublishApp.md) página, selecione o **prever a incluir todas as pontuações intenção** caixa de verificação. Quando a caixa de verificação está selecionada, são devolvidos todos os pendentes. Quando a caixa de verificação está desmarcada, apenas o objetivo principal é devolvido. 

5. [Publicar](PublishApp.md) a aplicação. Publicar a aplicação permite-lhe testá-lo utilizando o ponto final de HTTPS. 

## <a name="test-a-trained-utterance"></a>Testar um utterance preparado
Utilize o ponto final publicado para consultar um utterance que já sabe a aplicação. Uma vez LUIS já conhece o utterance, a classificação é elevada e a entidade é detetada.

1. No [compreensão de idiomas (LUIS)] [ LUIS] Web site, no **publicar** página para a nova aplicação, selecione o URL de ponto final no **recursos e as chaves**secção. 

    ![Publicar o URL de ponto final](./media/luis-tutorial-interchangeable-phrase-list/luis-publish-url.png)

2. No browser, no final do URL, adicione a seguinte consulta depois do `q=`.

    `I want a computer replacement`

    O ponto final responde com o seguinte JSON:
    
    ```JSON
    {
      "query": "I want a computer replacement",
      "topScoringIntent": {
        "intent": "GetHardware",
        "score": 0.9735458
      },
      "intents": [
        {
          "intent": "GetHardware",
          "score": 0.9735458
        },
        {
          "intent": "None",
          "score": 0.07053033
        },
        {
          "intent": "Whois",
          "score": 0.03760778
        },
        {
          "intent": "CheckCalendar",
          "score": 0.02285902
        },
        {
          "intent": "CheckInventory",
          "score": 0.0110072717
        }
      ],
      "entities": [
        {
          "entity": "computer",
          "type": "Hardware",
          "startIndex": 9,
          "endIndex": 16,
          "score": 0.8465915
        }
      ]
    }
    ```

    A intenção pontuação dos 0.973 e a classificação de deteção de entidade de 0.846 é elevada porque a aplicação foi preparada com este utterance. O utterance está na aplicação LUIS na página intenção de **GetHardware**. Texto a utterance, `computer`, com a etiqueta, como o **Hardware** entidade. 
    
    |Estado|Word| Pontuação intenção | Classificação de entidade |
    |--|--|--|--|
    |Preparado| pretende | 0.973 | 0.846 |
    
    
## <a name="test-an-untrained-utterance"></a>Testar um utterance untrained
No browser, utilize o mesmo ponto final de publicados para consulta com um utterance que já não souber a aplicação:

`I require a computer replacement`

Este utterance utiliza um sinónimo do utterance anterior:

| Word preparado | Sinónimo untrained |
|--|--|
| pretende | Exigir |

A resposta do ponto final é:

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.840912163
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.840912163
    },
    {
      "intent": "None",
      "score": 0.0972757638
    },
    {
      "intent": "Whois",
      "score": 0.0448251367
    },
    {
      "intent": "CheckCalendar",
      "score": 0.0291390456
    },
    {
      "intent": "CheckInventory",
      "score": 0.0137837809
    }
  ],
  "entities": []
}
```

| Estado | Word | Pontuação intenção | Classificação de entidade |
|--|--|--|--|
| Preparado| pretende | 0.973 | 0.846 |
| Untrained| Exigir | 0.840 | - |

A classificação de intenção untrained utterance é menor que o utterance identificado porque LUIS sabe que o frase grammatically é o mesmo. Mas LUIS não sabe que o utterances têm o mesmo significado. Além disso, sem lista o frase, o **Hardware** entidade não foi encontrada.

Tem ensinar LUIS que *pretende* e *requerem* significar a mesma coisa neste domínio de aplicação porque uma palavra pode ter mais do que um significado. 

## <a name="improve-the-score-of-untrained-utterance-with-phrase-list"></a>Melhorar a pontuação dos utterance untrained com a lista de expressão 
1. Adicionar um [lista frase](luis-how-to-add-features.md) funcionalidade com o nome **pretende** com o valor de `want`e, em seguida, selecione **Enter**.

    > [!TIP]
    > Depois de cada palavra ou expressão, selecione o **Enter** chave. A palavra ou expressão é adicionado ao **frase os valores da lista** caixa enquanto o cursor permanece no **valor** caixa. Pode introduzir vários valores rapidamente com esta funcionalidade.

2. Para ver as palavras que recomenda LUIS, selecione **Recomendamos**. 

    ![Recomendamos valores](./media/luis-tutorial-interchangeable-phrase-list/recommend.png)

3. Adicione todas as palavras. Se `require` é não estão na lista recomendada, adicione-o como um valor necessário. 

4. Uma vez que estas palavras são sinónimos, mantenha o *permutáveis* definição e, em seguida, selecione **guardar**.

    ![Valores da lista de expressão](./media/luis-tutorial-interchangeable-phrase-list/phrase-list-values.png)

5. Na barra de navegação superior, selecione **preparar** para preparar a aplicação, mas não publicá-lo. Agora, tem dois modelos. Pode comparar valores em dois modelos.

## <a name="compare-the-phrase-list-model-to-the-published-model"></a>Comparar o modelo de lista de expressão para o modelo publicado
Nesta aplicação, o modelo publicado não está preparado com os sinónimos. Apenas o modelo atualmente editado inclui a lista de expressão de sinónimos. Para comparar os modelos, utilize [testar interativa](interactive-test.md#interactive-testing). 

1. Abra o **teste** painel e introduza o utterance seguinte:

    `I require a computer replacement`

2. Para abrir o painel de inspeção, selecione **inspecionar**. 

    ![Selecione inspecionar](./media/luis-tutorial-interchangeable-phrase-list/inspect-button.png)

3. Para comparar o modelo publicado para o novo modelo de lista de expressão, selecione **comparar com publicados**.

    ![Inspecione publicados versus atual](./media/luis-tutorial-interchangeable-phrase-list/inspect.png)

Depois de adicionar a lista de frase, a maior precisão do utterance e **Hardware** encontra-se a entidade. 

|Estado | Lista de expressão| Pontuação intenção | Classificação de entidade |
|--|--|--|--|
| Publicado | - | 0.84 | - |
| Atualmente a editar |✔| 0.92 | Entidade de hardware identificada |

> [!TIP]
> * Ao utilizar [testar interativa](interactive-test.md#interactive-testing), pode comparar o modelo publicado para qualquer treinados as alterações efetuadas depois de publicar. 
> * Ao utilizar [teste de ponto final](PublishApp.md#test-your-published-endpoint-in-a-browser), pode ver a resposta exata de LUIS JSON. 

## <a name="get-the-entity-score-with-the-endpoint-test"></a>Obter a classificação de entidade com o teste de ponto final
Para ver a classificação de entidade, [publicar o modelo](PublishApp.md) e consultar o ponto final. 

`I require a computer replacement`

```JSON
{
  "query": "I require a computer replacement",
  "topScoringIntent": {
    "intent": "GetHardware",
    "score": 0.916503668
  },
  "intents": [
    {
      "intent": "GetHardware",
      "score": 0.916503668
    },
    {
      "intent": "None",
      "score": 0.136505231
    },
    {
      "intent": "Whois",
      "score": 0.02778677
    },
    {
      "intent": "CheckInventory",
      "score": 0.0144592477
    },
    {
      "intent": "CheckCalendar",
      "score": 0.01401332
    }
  ],
  "entities": [
    {
      "entity": "computer",
      "type": "Hardware",
      "startIndex": 12,
      "endIndex": 19,
      "score": 0.5959917
    }
  ]
}
```

O **Hardware** entidade mostra uma pontuação de 0.595 com a lista de expressão. Antes da lista de expressão existia, a entidade não foi detetada. 

|Estado | Lista de expressão| Pontuação intenção | Classificação de entidade |
|--|--|--|--|
| Publicado | - | 0.84 | - |
| Atualmente a editar |✔| 0.92 | 0.595 |


## <a name="clean-up-resources"></a>Limpar recursos
Quando já não é necessário elimine a aplicação de LUIS. Para tal, selecione o menu de três ponto (. …) para a direita do nome da aplicação na lista de aplicações, selecione **eliminar**. Na caixa de diálogo de pop-up **eliminar aplicação?**, selecione **Ok**.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Obter predição utterance com a consulta de ponto final](luis-get-started-cs-get-intent.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
[LuisFeatures]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[LuisSampleApp]: https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/phrase_list/interchangeable/luis-app-before-phrase-list.json
