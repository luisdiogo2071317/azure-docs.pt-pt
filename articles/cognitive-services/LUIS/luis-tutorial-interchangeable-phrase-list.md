---
title: Tutorial sobre como utilizar uma lista de frase para melhorar as previsões de LUIS – Azure | Documentos da Microsoft
description: Neste tutorial, adicione uma lista de frase para uma aplicação do LUIS e veja a melhoria da pontuação.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2017
ms.author: v-geberr
ms.openlocfilehash: 2fd53225a455a34d03772487a10f62a94ac86735
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868978"
---
# <a name="tutorial-add-phrase-list-to-improve-predictions"></a>Tutorial: Adicionar a lista de frase para melhorar as previsões de indisponibilidade
Neste tutorial, melhorar a precisão das pontuações intenção e identificar as entidades para palavras que têm o mesmo significado (sinónimos) ao adicionar um intercambiáveis [funcionalidade de lista de frase](./luis-concept-feature.md).

> [!div class="checklist"]
* Importar uma nova aplicação  
* Ponto final de consulta com a expressão conhecido 
* Consultar o ponto final com _desconhecido_ expressão
* Adicionar a lista de frase para melhorar a pontuação de expressão desconhecido
* Certifique-se de que é encontrar a entidade ao utilizar a lista de frase

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="import-a-new-app"></a>Importar uma nova aplicação
1. Transfira o [exemplo de aplicação LUIS] [ LuisSampleApp] qual foi concebida para este tutorial. Irá utilizá-lo no próximo passo. 

2. Conforme descrito em [criar uma aplicação](Create-new-app.md#import-new-app), importar o ficheiro que transferiu para o [LUIS] [ LUIS] Web site como uma nova aplicação. O nome da aplicação é "Tutorial minha lista de frase." Ele tem intenções, entidades e expressões com. 

3. [Train](luis-how-to-train.md) seu aplicativo. Até que ele é preparado, não pode [testar interativamente](interactive-test.md#interactive-testing) -na [LUIS] [ LUIS] Web site. 

4. No [Publish](luis-how-to-publish-app.md) página, selecione a **incluir todos os prever a intenção pontuações** caixa de verificação. Quando a caixa de verificação está selecionada, são devolvidas todas as intenções. Quando a caixa de verificação está desmarcada, apenas o objetivo principal é devolvido. 

5. [Publicar](luis-how-to-publish-app.md) a aplicação. Publicar a aplicação permite-lhe para testá-lo utilizando o ponto final HTTPS. 

## <a name="test-a-trained-utterance"></a>Testar uma expressão de preparado
Utilize o ponto de extremidade publicado para consultar uma expressão que já sabe que a aplicação. Como o LUIS já sabe que a expressão, a pontuação é alta e a entidade é detectada.

1. Na [compreensão de idiomas (LUIS)] [ LUIS] Web site, no **publicar** para a nova aplicação, selecione o URL de ponto final no **recursos e as chaves**secção. 

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

    A classificação de intenção da 0.973 e a pontuação de deteção de entidade de 0.846 é elevada porque a aplicação foi preparada com esta expressão. A expressão é na aplicação do LUIS, na página da intenção **GetHardware**. Texto da expressão, `computer`, é identificado como o **Hardware** entidade. 
    
    |Estado|Word| Pontuação de intenção | Pontuação de entidade |
    |--|--|--|--|
    |Preparado| pretende | 0.973 | 0.846 |
    
    
## <a name="test-an-untrained-utterance"></a>Testar uma expressão destreinado
No browser, utilize o mesmo ponto de final publicado para consulta com uma expressão que a aplicação já não sabe:

`I require a computer replacement`

Essa expressão usa sinônimo da expressão anterior:

| Word preparado | Sinónimos destreinado |
|--|--|
| pretende | exigir |

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

| Estado | Word | Pontuação de intenção | Pontuação de entidade |
|--|--|--|--|
| Preparado| pretende | 0.973 | 0.846 |
| Destreinado| exigir | 0.840 | - |

A pontuação de intenção de expressão destreinado é menor do que a expressão etiquetada como LUIS sabe que a frase sentenças gramaticalmente é o mesmo. Mas o LUIS não sabe que as expressões têm o mesmo significado. Além disso, sem a lista de frase, o **Hardware** entidade não foi encontrada.

Deve ensinar LUIS que *deseja* e *exigir* significar a mesma coisa neste domínio de aplicativo como uma palavra pode ter mais de um significado. 

## <a name="improve-the-score-of-untrained-utterance-with-phrase-list"></a>Melhorar a classificação de expressão destreinado com lista de frase 
1. Adicionar uma [lista de frase](luis-how-to-add-features.md) recurso chamado **pretende** com o valor de `want`e, em seguida, selecione **Enter**.

    > [!TIP]
    > Depois de cada palavra ou frase, selecione o **Enter** chave. A palavra ou expressão é adicionada à **frase valores da lista** caixa enquanto o cursor permanece no **valor** caixa. Pode introduzir muitos valores rapidamente com esta funcionalidade.

2. Para ver as palavras que recomenda o LUIS, selecione **Recomendamos**. 

    ![Recomendamos valores](./media/luis-tutorial-interchangeable-phrase-list/recommend.png)

3. Adicione todas as palavras. Se `require` é não da lista de recomendados, adicioná-la como um valor obrigatório. 

4. Como essas palavras são sinónimos, manter a *intercambiáveis* definição e, em seguida, selecione **guardar**.

    ![Valores da lista de frase](./media/luis-tutorial-interchangeable-phrase-list/phrase-list-values.png)

5. Na barra de navegação superior, selecione **treinar** para preparar a aplicação, mas não o publicar. Agora tem dois modelos. Pode comparar valores em dois modelos.

## <a name="compare-the-phrase-list-model-to-the-published-model"></a>Comparar o modelo de lista de frase para o modelo publicado
Nesta aplicação, o modelo publicado não está preparado com os sinónimos. Apenas o modelo atualmente editado inclui a lista de frase de sinónimos. Para comparar os modelos, utilize [teste interativo](interactive-test.md#interactive-testing). 

1. Abra o **teste** painel e introduza a seguinte expressão:

    `I require a computer replacement`

2. Para abrir o painel de inspeção, selecione **Inspect**. 

    ![Selecione inspecionar](./media/luis-tutorial-interchangeable-phrase-list/inspect-button.png)

3. Para comparar o modelo publicado para o novo modelo de lista de frase, selecione **Compare com publicado**.

    ![Inspecionar publicado em comparação com o atual](./media/luis-tutorial-interchangeable-phrase-list/inspect.png)

Depois de adicionar a lista de frase, a precisão maior do que a expressão e a **Hardware** é encontrar a entidade. 

|Estado | Lista de frase| Pontuação de intenção | Pontuação de entidade |
|--|--|--|--|
| Publicado | - | 0,84 | - |
| Atualmente a editar |✔| 0.92 | Entidade de hardware identificada |

> [!TIP]
> * Usando [teste interativo](interactive-test.md#interactive-testing), pode comparar o modelo publicado que quaisquer alterações treinados que são feitas depois de publicar. 
> * Usando [ponto final de teste](luis-how-to-publish-app.md#test-your-published-endpoint-in-a-browser), pode ver a resposta exata do LUIS JSON. 

## <a name="get-the-entity-score-with-the-endpoint-test"></a>Obter a classificação de entidade com o teste de ponto final
Para ver a classificação de entidade [publique o modelo](luis-how-to-publish-app.md) e consultar o ponto final. 

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

O **Hardware** entidade mostra uma pontuação igual a 0.595 com a lista de frase. Antes da lista de frase existia, a entidade não foi detetada. 

|Estado | Lista de frase| Pontuação de intenção | Pontuação de entidade |
|--|--|--|--|
| Publicado | - | 0,84 | - |
| Atualmente a editar |✔| 0.92 | 0.595 |


## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione as reticências (***...*** ) à direita do nome da aplicação na lista de aplicações, selecione **eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Obtenha a previsão de expressão com consulta de ponto final](luis-get-started-cs-get-intent.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
[LuisFeatures]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[LuisSampleApp]: https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/phrase_list/interchangeable/luis-app-before-phrase-list.json
