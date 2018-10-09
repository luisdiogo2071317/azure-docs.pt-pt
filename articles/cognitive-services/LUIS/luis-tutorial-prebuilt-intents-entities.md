---
title: 'Tutorial 2: intenções e entidades pré-concebidas – utilizar expressões comuns pré-concebidas – extrair dados comuns no LUIS'
titleSuffix: Azure Cognitive Services
description: Adicione intenções pré-concebidas e entidades à aplicação do tutorial de Recursos Humanos para obter rapidamente a predição de intenção e a extração de dados. Não é necessário etiquetar expressões com entidades pré-concebidas. A entidade é detetada automaticamente.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: d42aed76ecdbc2bd840e17517db2ca0b6ba11aa0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47034438"
---
# <a name="tutorial-2-identify-common-intents-and-entities"></a>Tutorial 2: identificar intenções e entidades comuns
Neste tutorial, vai modificar a aplicação de Recursos Humanos. Adicione intenções pré-concebidas e entidades à aplicação do tutorial de Recursos Humanos para obter rapidamente a predição de intenção e a extração de dados. Não é necessário etiquetar expressões com entidades pré-concebidas porque a entidade é detetada automaticamente.

Os modelos pré-concebidos de domínios de assunto e tipos de dados comuns ajudam-no a criar rapidamente o modelo, bem como a fornecer um exemplo do aspeto do modelo. 

**Neste tutorial, ficará a saber como:**

> [!div class="checklist"]
> * Utilizar a aplicação de tutorial existente
> * Adicionar intenções pré-concebidas 
> * Adicionar entidades pré-concebidas 
> * Preparar 
> * Publicar 
> * Obter as intenções e as entidades do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente
Continue com a aplicação criada no último tutorial, com o nome **RecursosHumanos**. 

Se não tiver a aplicação RecursosHumanos do tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde o [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-intent-only-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Na secção **Gerir**, no separador **Versões**, clone a versão e dê-lhe o nome `prebuilts`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter carateres que não sejam válidos num URL. 

## <a name="add-prebuilt-intents"></a>Adicionar intenções pré-concebidas
O LUIS oferece várias intenções pré-concebidas para ajudar com intenções comuns do utilizador.  

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecione **Adicionar intenção pré-concebida**. 

3. Procurar `Utilities`. 

    [ ![Captura de ecrã da caixa de diálogo de intenções pré-concebidas com Utilitários na caixa de pesquisa](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

4. Selecione as intenções seguintes e selecione **Concluído**: 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.StartOver
    * Utilities.Stop


## <a name="add-prebuilt-entities"></a>Adicionar entidades pré-concebidas
O LUIS oferece várias entidades pré-concebidas para extração de dados comuns. 

1. Selecione **Entidades** no menu de navegação esquerdo.

2. Selecione o botão **Gerir entidade pré-concebida**.

3. Selecione **número** e **datetimeV2** na lista de entidades pré-concebidas e, em seguida, selecione **Concluído**.

    ![Captura de ecrã do número selecionado na caixa de diálogo de entidades pré-concebidas](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obter as intenções e as entidades do ponto final

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL na barra de endereço do browser e introduza `I want to cancel on March 3`. O último parâmetro de cadeia de consulta é `q`, a expressão **query**. 

    ```JSON
    {
      "query": "I want to cancel on March 3",
      "topScoringIntent": {
        "intent": "Utilities.Cancel",
        "score": 0.7818295
      },
      "intents": [
        {
          "intent": "Utilities.Cancel",
          "score": 0.7818295
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0237864349
        },
        {
          "intent": "GetJobInformation",
          "score": 0.017576348
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0130122062
        },
        {
          "intent": "Utilities.Help",
          "score": 0.006731322
        },
        {
          "intent": "None",
          "score": 0.00524190161
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.004912514
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00092950504
        }
      ],
      "entities": [
        {
          "entity": "march 3",
          "type": "builtin.datetimeV2.date",
          "startIndex": 20,
          "endIndex": 26,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2018-03-03"
              },
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2019-03-03"
              }
            ]
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 26,
          "endIndex": 26,
          "resolution": {
            "value": "3"
          }
        }
      ]
    }
    ```

    O resultado previu a intenção Utilities.Cancel e extraiu a data de 3 de março e o número 3. 

    Existem dois valores de 3 de março porque a expressão não indicou se 3 de março é no passado ou no futuro. Cabe à aplicação cliente fazer uma suposição ou pedir esclarecimentos, se assim for preciso. 

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes

Ao adicionar intenções e entidades pré-concebidas, a aplicação cliente pode determinar as intenções de utilizador comuns e extrair tipos de dados comuns. 

> [!div class="nextstepaction"]
> [Adicionar uma entidade de expressão regular à aplicação](luis-quickstart-intents-regex-entity.md)

