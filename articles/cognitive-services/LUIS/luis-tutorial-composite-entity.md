---
title: Criar uma entidade composta para extrair dados complexos - Azure | Microsoft Docs
description: Saiba como criar uma entidade composta na sua aplicação LUIS para extrair os diferentes tipos de dados de entidade.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 03/28/2018
ms.author: v-geberr
ms.openlocfilehash: 5e4d5a03db6210c159227530b94fbde6873bc211
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356119"
---
# <a name="use-composite-entity-to-extract-complex-data"></a>Utilizar a entidade composta para extrair dados complexo
Esta aplicação simple tem dois [pendentes](luis-concept-intent.md) e várias entidades. O objetivo é livro flights como "1 do mesmo Seattle para Cairo no sexta-feira" e devolva todas as especificações de reserva como um único conjunto de dados. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Adicionar entidades prebuilt datetimeV2 e número
* Criar uma entidade composta
* Consultar LUIS e receber dados composto de entidade

## <a name="before-you-begin"></a>Antes de começar
* A aplicação de LUIS do  **[início rápido hierárquico](luis-tutorial-composite-entity.md)**. 

> [!Tip]
> Se ainda não tiver uma subscrição, pode registar-se para uma [conta gratuita](https://azure.microsoft.com/free/).

## <a name="composite-entity-is-a-logical-grouping"></a>Entidade composta é um agrupamento lógico 
O objetivo da entidade é localizar e categorizar partes do texto no utterance. A [composto](luis-concept-entity-types.md) entidade é composta por outros tipos de entidade adquiridos a partir do contexto. Para esta aplicação levar que demora a reservas de voo, existem vários tipos de informações, tais como as datas, localizações e número de licenças. 

As informações existe como entidades separadas antes da criação de um compostos. Crie uma entidade composta quando as entidades separadas podem ser logicamente agrupadas e este agrupamento lógico é útil para a chatbot ou outra aplicação de consumo de LUIS. 

Utterances de exemplo simples dos utilizadores incluem:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Reserve a seat from New York to Paris on the first of April
```
 
A entidade composta corresponde ao número de licenças, localização de origem, a localização de destino e data. 

## <a name="what-luis-does"></a>O que faz LUIS
Quando o objetivo e entidades do utterance são identificadas, [extraídos](luis-concept-data-extraction.md#list-entity-data)e devolvido no JSON do [endpoint](https://aka.ms/luis-endpoint-apis), LUIS é efetuado. A aplicação de chamada ou chatbot assume que resposta JSON e satisfaz o pedido – forma que achar mais a aplicação ou chatbot foi concebido para o fazer. 

## <a name="add-prebuilt-entities-number-and-datetimev2"></a>Adicionar o número de entidades prebuilt e datetimeV2
1. Selecione o `MyTravelApp` aplicação na lista de aplicações no [LUIS] [ LUIS] Web site.

2. Quando abre a aplicação, selecione o **entidades** ligação de navegação esquerdo.

    ![Selecione o botão de entidades](./media/luis-tutorial-composite-entity/intents-page-select-entities.png)    

3. Selecione **gerir entidades prebuilt**.

    ![Selecione o botão de entidades](./media/luis-tutorial-composite-entity/manage-prebuilt-entities-button.png)

4. Na caixa de pop-up, selecione **número** e **datetimeV2**.

    ![Selecione o botão de entidades](./media/luis-tutorial-composite-entity/prebuilt-entity-ddl.png)

5. Por ordem para as entidades de novo a ser extraído, selecione **formação** na barra de navegação superior.

    ![Botão de formação selecione](./media/luis-tutorial-composite-entity/train.png)

## <a name="use-existing-intent-to-create-composite-entity"></a>Utilize intenção existente para criar a entidade composta
1. Selecione **pendentes** do painel de navegação esquerdo. 

    ![Selecione a página de pendentes](./media/luis-tutorial-composite-entity/intents-from-entities-page.png)

2. Selecione `BookFlight` do **pendentes** lista.  

    ![Selecione BookFlight intenção da lista](./media/luis-tutorial-composite-entity/intent-page-with-prebuilt-entities-labeled.png)

    O número e datetimeV2 prebuilt entidades são etiquetadas nos utterances.

3. Para o utterance `book 2 flights from seattle to cairo next monday`, selecione a azul `number` entidade, em seguida, selecione **moldado na entidade composta** da lista. Uma linha verde, sob as palavras, segue o cursor são transmitidos para a direita, que indica uma entidade composta. Em seguida, avance para a direita para selecionar a última entidade prebuilt `datetimeV2`, em seguida, introduza `FlightReservation` na caixa de texto da janela de pop-up, em seguida, selecione **compostos novo criar**. 

    ![Criar entidade composta na página de pendentes](./media/luis-tutorial-composite-entity/create-new-composite.png)

4. Uma caixa de diálogo de pop-up é apresentada, permitindo-lhe verificar os subordinados de entidade composto. Selecione **feito**.

    ![Criar entidade composta na página de pendentes](./media/luis-tutorial-composite-entity/validate-composite-entity.png)

## <a name="wrap-the-entities-in-the-composite-entity"></a>Moldar as entidades na entidade composta
Assim que a entidade composta for criada, etiqueta de utterances restantes na entidade composta. Para encapsular uma frase de acesso como uma entidade composta, tem de selecionar o word mais à esquerda, em seguida, selecione **moldado na entidade composta** na lista que aparece, em seguida, selecione o word mais à direita, em seguida, selecione a entidade composta nomeada `FlightReservation`. Este é um passo rápido, uniforme de seleções, divididos nos seguintes passos:

1. No utterance `schedule 4 seats from paris to london for april 1`, selecione de 4 como entidade prebuilt número.

    ![Selecione o word mais à esquerda](./media/luis-tutorial-composite-entity/wrap-composite-step-1.png)

2. Selecione **moldado na entidade composta** da lista que é apresentada.

    ![Selecione moldagem da lista](./media/luis-tutorial-composite-entity/wrap-composite-step-2.png)

3. Selecione o word mais à direita. Uma linha de verde é apresentado sob o frase, que indica uma entidade composta.

    ![Selecione o word mais à direita](./media/luis-tutorial-composite-entity/wrap-composite-step-3.png)

4. Selecione nome composto `FlightReservation` da lista que é apresentada.

    ![Selecione a entidade composta com o nome](./media/luis-tutorial-composite-entity/wrap-composite-step-4.png)

    Para o último utterance, moldar `London` e `tomorrow` na entidade composta, utilizando as mesmas instruções. 

## <a name="train-the-luis-app"></a>Preparar a aplicação de LUIS
LUIS desconheça as alterações aos pendentes e entidades (modelo), até que está preparado. 

1. No lado direito superior do Web site LUIS, selecione o **formação** botão.

    ![Preparar a aplicação](./media/luis-tutorial-composite-entity/train-button.png)

2. Formação está concluída quando for apresentada a barra de estado verde na parte superior do Web site confirmar o êxito.

    ![Formação foi concluída com êxito](./media/luis-tutorial-composite-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição LUIS um chatbot ou outra aplicação, terá de publicar a aplicação. 

1. No lado direito superior do Web site LUIS, selecione o **publicar** botão. 

2. Selecione a ranhura de produção e o **publicar** botão.

    ![Publicar aplicações](./media/luis-tutorial-composite-entity/publish-to-production.png)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do Web site confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com um utterance diferentes
1. No **publicar** página, selecione o **endpoint** ligação na parte inferior da página. Esta ação abre outra janela do browser com o URL de ponto final na barra de endereço. 

    ![Selecione o URL de ponto final](./media/luis-tutorial-composite-entity/publish-select-endpoint.png)

2. Vá para o final do URL o endereço e introduza `reserve 3 seats from London to Cairo on Sunday`. É o último parâmetro querystring `q`, a consulta utterance. Este utterance não é igual a qualquer um dos utterances identificados pelo é um teste de boa e deverá devolver o `BookFlight` intenção com a entidade hierárquica extraída.

```
{
  "query": "reserve 3 seats from London to Cairo on Sunday",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.999999046
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 0.999999046
    },
    {
      "intent": "None",
      "score": 0.227036044
    }
  ],
  "entities": [
    {
      "entity": "sunday",
      "type": "builtin.datetimeV2.date",
      "startIndex": 40,
      "endIndex": 45,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-03-25"
          },
          {
            "timex": "XXXX-WXX-7",
            "type": "date",
            "value": "2018-04-01"
          }
        ]
      }
    },
    {
      "entity": "3 seats from london to cairo on sunday",
      "type": "flightreservation",
      "startIndex": 8,
      "endIndex": 45,
      "score": 0.6892485
    },
    {
      "entity": "cairo",
      "type": "Location::Destination",
      "startIndex": 31,
      "endIndex": 35,
      "score": 0.557570755
    },
    {
      "entity": "london",
      "type": "Location::Origin",
      "startIndex": 21,
      "endIndex": 26,
      "score": 0.8933808
    },
    {
      "entity": "3",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "3"
      }
    }
  ],
  "compositeEntities": [
    {
      "parentType": "flightreservation",
      "value": "3 seats from london to cairo on sunday",
      "children": [
        {
          "type": "builtin.datetimeV2.date",
          "value": "sunday"
        },
        {
          "type": "Location::Destination",
          "value": "cairo"
        },
        {
          "type": "builtin.number",
          "value": "3"
        },
        {
          "type": "Location::Origin",
          "value": "london"
        }
      ]
    }
  ]
}
```

Este utterance devolve uma matriz de entidades compostas, incluindo o **flightreservation** objeto com os dados extraídos.  

## <a name="what-has-this-luis-app-accomplished"></a>O que tenha conseguido esta aplicação LUIS?
Esta aplicação, com apenas dois pendentes e uma entidade composta, identificado uma intenção de consulta de linguagem natural e devolveu dados extraídos. 

Agora, o seu chatbot tem informações suficientes para determinar a ação principal, `BookFlight`e as informações de reserva encontrado no utterance. 

## <a name="where-is-this-luis-data-used"></a>Onde são usados estes dados LUIS? 
LUIS é feito com este pedido. A aplicação de chamada, tais como um chatbot, pode tirar o resultado de topScoringIntent e os dados da entidade para tirar o passo seguinte. LUIS não que funcionam para a aplicação de chamada ou bot programática. LUIS apenas determina o que é a intenção do utilizador. 

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre entidades](luis-concept-entity-types.md). 

<!--References-->
[LUIS]:luis-reference-regions.md#luis-website
[LUIS-regions]:luis-reference-regions.md#publishing-regions
