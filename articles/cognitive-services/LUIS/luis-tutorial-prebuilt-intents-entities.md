---
title: Adicionar intenções pré-concebidas e entidades para extrair dados comuns no Language Understanding - Azure | Microsoft Docs
description: Saiba como utilizar intenções pré-concebidas e entidades para extrair diferentes tipos de dados de entidade.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: diberry
ms.openlocfilehash: 0ec6f002b35b1224118b62accda1f69e7be22fb8
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358527"
---
# <a name="tutorial-2-add-prebuilt-intents-and-entities"></a>Tutorial: 2. Adicionar intenções e entidades pré-concebidas
Adicione intenções pré-concebidas e entidades à aplicação do tutorial de Recursos Humanos para obter rapidamente a predição de intenção e a extração de dados. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Adicionar intenções pré-concebidas 
* Adicionar datetimeV2 e número de entidades pré-concebidas
* Preparar e publicar
* Consultar o LUIS e receber a resposta de predição

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de [Recursos Humanos](luis-quickstart-intents-only.md) do tutorial anterior, [importe](luis-how-to-start-new-app.md#import-new-app) o JSON para uma nova aplicação no site do [LUIS](luis-reference-regions.md#luis-website) a partir do repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-intent-only-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `prebuilts`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="add-prebuilt-intents"></a>Adicionar intenções pré-concebidas
O LUIS oferece várias intenções pré-concebidas para ajudar com intenções comuns do utilizador.  

1. Certifique-se de que a aplicação está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

2. Selecione **Adicionar intenção de domínio pré-concebido**. 

    [ ![Captura de ecrã da página Intenções com o botão Adicionar intenção de domínio pré-concebido realçado](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png) ](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png#lightbox)

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

    [ ![Captura de ecrã da lista Intenções com Entidades destacadas na navegação esquerda](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png)](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png#lightbox)

2. Selecione o botão **Gerir entidades pré-concebidas**.

    [ ![Captura de ecrã da Lista de entidades com Gerir entidades pré-concebidas realçado](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png)](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png#lightbox)

3. Selecione **número** e **datetimeV2** na lista de entidades pré-concebidas e, em seguida, selecione **Concluído**.

    ![Captura de ecrã do número selecionado na caixa de diálogo de entidades pré-concebidas](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train-and-publish-the-app"></a>Preparar e publicar a aplicação

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Publicar a aplicação no ponto final

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-endpoint-with-an-utterance"></a>Consultar ponto final com uma expressão

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `I want to cancel on March 3`. O último parâmetro de cadeia de consulta é `q`, a expressão **query**. 

    O resultado previu a intenção Utilities.Cancel e extraiu a data de 3 de março e o número 3. 

    ```
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

    Existem dois valores de 3 de março porque a expressão não indicou se 3 de março é no passado ou no futuro. Cabe à aplicação de chamadas do LUIS fazer uma suposição ou pedir esclarecimentos, se assim for preciso. 

    Ao adicionar rápida e facilmente intenções e entidades pré-concebidas, a aplicação cliente pode adicionar gestão de conversação e extrair tipos de dados comuns. 

## <a name="clean-up-resources"></a>Limpar recursos

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicionar uma entidade de expressão regular à aplicação](luis-quickstart-intents-regex-entity.md)

