---
title: Adicionar pendentes prebuilt e entidades para extrair dados comuns na compreensão de idiomas - Azure | Microsoft Docs
description: Saiba como utilizar pendentes prebuilt e entidades para extrair diferentes tipos de dados de entidade.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 06/11/2018
ms.author: v-geberr
ms.openlocfilehash: 37d67bef7712012a95543041744706b240b16e2d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085502"
---
# <a name="tutorial-2-add-prebuilt-intents-and-entities"></a>Tutorial: 2. Adicionar intenções e entidades pré-concebidas
Adicione pendentes prebuilt e entidades para a aplicação de início rápido de recursos humanos para obter rapidamente intenção extração de dados e de predição. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Adicionar prebuilt pendentes 
* Adicionar entidades prebuilt datetimeV2 e número
* Dar formação e publicar
* Consultar LUIS e receber a resposta de predição

## <a name="before-you-begin"></a>Antes de começar
Se não tiver o [recursos humanos](luis-quickstart-intents-only.md) aplicação a partir do tutorial anterior, [importar](create-new-app.md#import-new-app) o JSON para uma nova aplicação no [LUIS](luis-reference-regions.md#luis-website) Web site, do [LUIS-Samples ](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-intent-only-HumanResources.json) Repositório do Github.

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `prebuilts`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="add-prebuilt-intents"></a>Adicionar prebuilt pendentes
LUIS fornece várias pendentes prebuilt para o ajudar com intenções comum do utilizador.  

1. Certifique-se a aplicação fica no **criar** secção LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png)](./media/luis-tutorial-prebuilt-intents-and-entities/first-image.png#lightbox)

2. Selecione **adicionar intenção de domínio prebuilt**. 

    [ ![Página de captura de ecrã de pendentes com o botão de intenção de Adicionar domínio prebuilt realçado](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png) ](./media/luis-tutorial-prebuilt-intents-and-entities/add-prebuilt-domain-button.png#lightbox)

3. Procurar `Utilities`. 

    [ ![Captura de ecrã da caixa de diálogo pendentes prebuilt com utilitários na caixa de pesquisa](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

4. Selecione os pendentes seguintes e selecione **feito**: 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.Stop
    * Utilities.StartOver

## <a name="add-prebuilt-entities"></a>Adicionar prebuilt entidades
LUIS fornece várias entidades prebuilt para extração de dados comuns. 

1. Selecione **entidades** no menu de navegação esquerdo.

    [ ![Lista de captura de ecrã de pendentes com entidades realçado na navegação à esquerda](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png)](./media/luis-tutorial-prebuilt-intents-and-entities/entities-navigation.png#lightbox)

2. Selecione **gerir entidades prebuilt** botão.

    [ ![Lista de captura de ecrã de entidades com gerir entidades prebuilt realçadas](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png)](./media/luis-tutorial-prebuilt-intents-and-entities/manage-prebuilt-entities-button.png#lightbox)

3. Selecione **número** e **datetimeV2** , em seguida, selecione na lista de entidades prebuilt **feito**.

    ![Captura de ecrã do selecione número na caixa de diálogo de entidades prebuilt](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train-and-publish-the-app"></a>Dar formação e publicar a aplicação
1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar). 

    ![Botão de formação](./media/luis-quickstart-intents-only/train-button.png)

    A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Na barra de estado preparado](./media/luis-quickstart-intents-only/trained.png)

2. Na parte superior, lado direito do Web site LUIS, selecione o **publicar** botão para abrir a página de publicar. A ranhura de produção está selecionada por predefinição. Selecione o **publicar** botão pela escolha da ranhura de produção. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    Não é necessário que criar uma chave de LUIS no portal do Azure antes de publicar ou antes de testar o URL de ponto final. Todas as aplicações de LUIS tem uma chave de arranque livre para a criação. Proporciona a criação ilimitados e um [alguns pedidos de ponto final](luis-boundaries.md#key-limits). 

## <a name="query-endpoint-with-an-utterance"></a>Ponto final de consulta com um utterance
Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. Vá para o final do URL no endereço e introduza `I want to cancel on March 3`. O último parâmetro de cadeia de consulta é `q`, o utterance **consulta**. 

O resultado prever a intenção de Utilities.Cancel e extraiu a data 3 de Março e o número 3. 

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

Adicionando mais rápida e facilmente pendentes prebuilt e entidades, a aplicação cliente pode adicionar a gestão de conversação e extrair os tipos de dados comuns. 

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicionar uma entidade de expressão regular para a aplicação](luis-quickstart-intents-regex-entity.md)

