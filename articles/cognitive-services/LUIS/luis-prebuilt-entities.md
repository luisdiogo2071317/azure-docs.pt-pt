---
title: Entidades pré-concebidas no LUIS | Documentos da Microsoft
description: Este artigo contém listas de entidades pré-criadas que estão incluídas no Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: cahann
manager: hsalama
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/15/2018
ms.author: cahann
ms.reviewer: v-geberr
ms.openlocfilehash: f7abf6d8a9f0fe18017fe5c54801ac0d3b6c379e
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170788"
---
# <a name="prebuilt-entities"></a>Entidades pré-concebidas

LUIS inclui um conjunto de entidades previamente concebidas para reconhecimento de tipos comuns de informações, como as datas, horas, números, medidas e moeda. Suporte de entidade pré-criados varia consoante a cultura da sua aplicação LUIS. Para obter uma lista completa das entidades pré-criados que suporte o LUIS, incluindo o suporte por cultura, consulte a [referência de entidade pré-criados](./luis-reference-prebuilt-entities.md).

> [!NOTE]
> **Builtin.DateTime** foi preterido. Ele é substituído por [ **builtin.datetimeV2**](luis-reference-prebuilt-datetimev2.md), que fornece o reconhecimento de data e intervalos de tempo, bem como reconhecimento aperfeiçoado de ambíguas datas e horas.

## <a name="add-a-prebuilt-entity"></a>Adicionar uma entidade pré-criados

1. Abra a sua aplicação ao clicar em seu nome na **as minhas aplicações** página e, em seguida, clique em **entidades** no lado esquerdo. 
2. Sobre o **entidades** página, clique em **gerir entidades pré-concebidas**.

    ![Página de entidades - gerir entidades pré-concebidas](./media/luis-use-prebuilt-entity/add-prebuilt-entity-button.png)
3. Na **adicionar entidades pré-concebidas** caixa de diálogo, clique na entidade pré-criados que pretende adicionar (por exemplo, "datetimeV2"). Em seguida, clique em **Guardar**.

    ![Adicionar a caixa de diálogo de entidade predefinidos](./media/luis-use-prebuilt-entity/add-prebuilt-entity-dialog.png)

## <a name="use-a-prebuilt-number-entity"></a>Utilizar uma entidade de número pré-criados
Quando uma entidade pré-criados é incluída na sua aplicação, respetivas previsões são incluídos em seu aplicativo publicado. O comportamento das entidades pré-concebidas é com formação prévia e **não é possível** ser modificado. Siga estes passos para ver como funciona uma entidade predefinida:

1. Adicionar uma **número** entidade à sua aplicação, em seguida, [Train](luis-interactive-test.md) e [publicar](luis-how-to-publish-app.md) a aplicação.
2. Clique no URL do ponto final no **publicar aplicação** página para abrir o ponto de extremidade do LUIS num navegador da web. 
3. Acrescente uma expressão para o URL que contém uma expressão numérica. Por exemplo, pode digitar `buy two plane ticktets`e vê que identifica o LUIS `two` como um `builtin.number` entidade e identifica `2` ao respetivo valo no `resolution` campo. O `resolution` campo ajuda-o a resolver números e datas para um formato canónico, que é mais fácil para a sua aplicação de cliente utilizar. 

    ![expressão no browser que contém uma entidade de número](./media/luis-use-prebuilt-entity/browser-query.png)

LUIS inteligentemente pode reconhecer números que não estão no formulário não padrão. Experimente diferentes expressões numéricas em seus discursos e ver o que retorna o LUIS.

O exemplo seguinte mostra uma resposta JSON do LUIS, que inclui a resolução do valor 24, para a expressão "duas dezenas".

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
## <a name="use-a-prebuilt-datetimev2-entity"></a>Utilizar uma entidade de datetimeV2 pré-criados
O **datetimeV2** entidade pré-criados reconhece as datas, horas, intervalos de datas e horas de duração. Siga estes passos para ver como o `datetimeV2` pré-criados de entidade funciona:

1. Adicionar uma **datetimeV2** entidade à sua aplicação, em seguida, [Train](luis-interactive-test.md) e [publicar](luis-how-to-publish-app.md) a aplicação.
2. Clique no URL do ponto final no **publicar aplicação** página para abrir o ponto de extremidade do LUIS num navegador da web. 
3. Acrescente uma expressão para o URL que contém um intervalo de datas. Por exemplo, pode digitar `book a flight tomorrow`e vê que identifica o LUIS `tomorrow` como um `builtin.datetimeV2.date` entidade e identifica a data de amanhã ao respetivo valo no `resolution` campo. 

O exemplo seguinte mostra a resposta JSON do LUIS possível aparência se a data de hoje foram 31 de Outubro de 2017.

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
> [Referência de entidade predefinidos](./luis-reference-prebuilt-entities.md)
