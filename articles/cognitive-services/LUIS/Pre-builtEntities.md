---
title: As entidades prebuilt de LUIS | Microsoft Docs
description: Este artigo contém apresenta uma lista das entidades prebuilt que estão incluídas no idioma compreender inteligente serviços (LUIS).
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/15/2018
ms.author: cahann
ms.reviewer: v-geberr
ms.openlocfilehash: 4858de8b8517016ca385e9105ca8948b66aa683b
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36322052"
---
# <a name="prebuilt-entities"></a>Entidades prebuilt

LUIS inclui um conjunto de entidades prebuilt para reconhecer tipos comuns de informação, como as datas, horas, números, medidas e moeda. Suporte de entidade prebuilt varia consoante a cultura da sua aplicação LUIS. Para obter uma lista completa das entidades prebuilt que LUIS suporte, incluindo o suporte por idioma, consulte o [referência de entidade prebuilt](./luis-reference-prebuilt-entities.md).

> [!NOTE]
> **Builtin.DateTime** foi preterido. Está substituído pelo [ **builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), que fornece o reconhecimento de data e intervalos de tempo, bem como reconhecimento melhorado de ambíguas datas e horas.

## <a name="add-a-prebuilt-entity"></a>Adicionar uma entidade prebuilt

1. Abra a aplicação ao clicar em seu nome no **aplicações My** página e, em seguida, clique em **entidades** no lado esquerdo. 
2. No **entidades** página, clique em **gerir entidades prebuilt**.

    ![Página de entidades - gerir prebuilt entidades](./media/luis-use-prebuilt-entity/add-prebuilt-entity-button.png)
3. No **adicionar entidades prebuilt** caixa de diálogo, clique em prebuilt entidade que pretende adicionar (por exemplo, "datetimeV2"). Em seguida, clique em **Guardar**.

    ![Caixa de diálogo de entidade prebuilt adicionar](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

## <a name="use-a-prebuilt-number-entity"></a>Utilize uma entidade número prebuilt
Quando uma entidade prebuilt está incluída na sua aplicação, as predições estão incluídas na sua aplicação publicada. O comportamento de entidades prebuilt é previamente preparado e **não é possível** ser modificada. Siga estes passos para ver como funciona uma entidade prebuilt:

1. Adicionar um **número** entidade para a sua aplicação, em seguida, [formação](interactive-test.md) e [publicar](PublishApp.md) a aplicação.
2. Clique no URL de ponto final no **publicar aplicação** página, para abrir o ponto final LUIS num web browser. 
3. Acrescente um utterance para o URL que contém uma expressão numérica. Por exemplo, pode escrever na `buy two plane ticktets`e ver que identifica LUIS `two` como um `builtin.number` entidade e identifica `2` ao respetivo valo no `resolution` campo. O `resolution` campo ajuda-o a números e datas de resolver para um formato canónico que é mais fácil para a sua aplicação de cliente utilizar. 

    ![utterance no browser que contém uma número entidade](./media/luis-use-prebuilt-entity/browser-query.png)

LUIS inteligentemente pode reconhecer números que não estão no formato não padrão. Experimentar expressões numéricos diferentes na sua utterances e ver o que devolve LUIS.

O exemplo seguinte mostra uma resposta JSON de LUIS, que inclui a resolução do valor 24, para o utterance "dúzia de dois".

```json
{
  "query": "order two dozen tickets for group travel",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.905443209
  },
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "value": "24"
      }
    }
  ]
}
```
## <a name="use-a-prebuilt-datetimev2-entity"></a>Utilize uma entidade prebuilt datetimeV2
O **datetimeV2** entidade prebuilt reconhece as datas, horas, intervalos de data e duração de tempo. Siga estes passos para ver como o `datetimeV2` entidade prebuilt funciona:

1. Adicionar um **datetimeV2** entidade para a sua aplicação, em seguida, [formação](interactive-test.md) e [publicar](PublishApp.md) a aplicação.
2. Clique no URL de ponto final no **publicar aplicação** página, para abrir o ponto final LUIS num web browser. 
3. Acrescente um utterance ao URL que contenha um intervalo de datas. Por exemplo, pode escrever na `book a flight tomorrow`e ver que identifica LUIS `tomorrow` como um `builtin.datetimeV2.date` entidade e identifica a data do amanhã ao respetivo valo no `resolution` campo. 

O exemplo seguinte mostra a resposta JSON do LUIS poderá aspeto se a data de hoje foram 31 de Outubro de 2017.

```json
{
  "query": "book a flight tomorrow",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.9063408
  },
  "entities": [
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 14,
      "endIndex": 21,
      "resolution": {
        "values": [
          {
            "timex": "2017-11-01",
            "type": "date",
            "value": "2017-11-01"
          }
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Passos Seguintes
> [!div class="nextstepaction"]
> [Referência de entidade prebuilt](./luis-reference-prebuilt-entities.md)
