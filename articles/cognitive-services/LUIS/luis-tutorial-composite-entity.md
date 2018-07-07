---
title: Criar uma entidade composta para extrair dados complexos - Azure | Documentos da Microsoft
description: Saiba como criar uma entidade composta na sua aplicação LUIS para extrair os diferentes tipos de dados de entidade.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: article
ms.date: 03/28/2018
ms.author: v-geberr
ms.openlocfilehash: 375b52f9206f55e620d5e664844b8fa1d7249a07
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888750"
---
# <a name="use-composite-entity-to-extract-complex-data"></a>Utilizar entidade composta para extrair dados complexos
Esta aplicação simple tem dois [intenções](luis-concept-intent.md) e várias entidades. Sua finalidade é reservar voos, como "1 bilhete de Seattle para Cairo na sexta-feira" e retornar todas as especificidades da reserva como uma parte dos dados. 

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Adicionar entidades pré-concebidas datetimeV2 e número
* Criar uma entidade composta
* Consultar o LUIS e receber dados de entidade compostos

## <a name="before-you-begin"></a>Antes de começar
* A aplicação LUIS a partir do  **[início rápido hierárquico](luis-tutorial-composite-entity.md)**. 

> [!Tip]
> Se ainda não tiver uma subscrição, pode se registrar para uma [conta gratuita](https://azure.microsoft.com/free/).

## <a name="composite-entity-is-a-logical-grouping"></a>Entidade composta é um agrupamento lógico 
O objetivo da entidade é localizar e categorizar partes do texto na expressão. R [compostos](luis-concept-entity-types.md) entidade é composta por outros tipos de entidade aprendidos em contexto. Para esta aplicação de viagens demora reservas de voo, existem várias partes de informações como as datas, localizações e número de utilizadores individuais. 

As informações existem como entidades separadas antes da criação de uma composição. Crie uma entidade composta quando as entidades separadas podem ser agrupadas logicamente e este agrupamento lógico é útil para o chatbot ou outra aplicação de consumo de LUIS. 

As expressões de exemplo simples dos utilizadores incluem:

```
Book a flight to London for next Monday
2 tickets from Dallas to Dublin this weekend
Reserve a seat from New York to Paris on the first of April
```
 
A entidade composta corresponde à contagem de utilizadores, localização de origem, localização de destino e data. 

## <a name="what-luis-does"></a>O que faz o LUIS
Quando a intenção e as entidades da expressão são identificadas, [extraídas](luis-concept-data-extraction.md#list-entity-data) e devolvidas em JSON a partir do [ponto final](https://aka.ms/luis-endpoint-apis), o LUIS está concluído. A aplicação de chamada ou chatbot pega nessa resposta JSON e satisfaz o pedido, da forma que a aplicação ou o chatbot foi concebido para o fazer. 

## <a name="add-prebuilt-entities-number-and-datetimev2"></a>Adicionar número de entidades previamente concebidas e datetimeV2
1. Selecione o `MyTravelApp` aplicação na lista de aplicações no [LUIS](luis-reference-regions.md#luis-website) Web site.

2. Quando a aplicação abre, selecione o **entidades** ligação de navegação esquerdo.

    ![Selecione o botão de entidades](./media/luis-tutorial-composite-entity/intents-page-select-entities.png)    

3. Selecione **Gerir entidades pré-concebidas**.

    ![Selecione o botão de entidades](./media/luis-tutorial-composite-entity/manage-prebuilt-entities-button.png)

4. Na caixa pop-up, selecione **número** e **datetimeV2**.

    ![Selecione o botão de entidades](./media/luis-tutorial-composite-entity/prebuilt-entity-ddl.png)

5. Para as novas entidades a ser extraído efetuada, selecione **Train** na barra de navegação superior.

    ![Selecionar o botão de preparação](./media/luis-tutorial-composite-entity/train.png)

## <a name="use-existing-intent-to-create-composite-entity"></a>Utilizar intenção existente para criar a entidade composta
1. Selecione **intenções** no painel de navegação esquerda. 

    ![Selecionar objetivos de página](./media/luis-tutorial-composite-entity/intents-from-entities-page.png)

2. Selecione `BookFlight` partir de **intenções** lista.  

    ![Selecione BookFlight intenção da lista](./media/luis-tutorial-composite-entity/intent-page-with-prebuilt-entities-labeled.png)

    O número e datetimeV2 entidades pré-concebidas estão identificadas nas expressões.

3. Para a expressão `book 2 flights from seattle to cairo next monday`, selecione a azul `number` entidade, em seguida, selecione **encapsular na entidade composta** da lista. Uma linha verde, sob as palavras, segue o cursor, pois ela se move para a direita, que indica uma entidade composta. Em seguida, mover para a direita para selecionar a última entidade pré-criados `datetimeV2`, em seguida, introduza `FlightReservation` na caixa de texto da janela de pop-up, em seguida, selecione **compostos nova de criar**. 

    ![Criar entidade composta na página de intenções](./media/luis-tutorial-composite-entity/create-new-composite.png)

4. É apresentada uma caixa de diálogo pop-up a que lhe permite verificar os filhos de entidade compostos. Selecione **Done** (Concluído).

    ![Criar entidade composta na página de intenções](./media/luis-tutorial-composite-entity/validate-composite-entity.png)

## <a name="wrap-the-entities-in-the-composite-entity"></a>Encapsular as entidades na entidade composta
Depois de criar a entidade composta, as expressões restantes na entidade composta da etiqueta. Para encapsular uma frase como uma entidade composta, terá de selecionar o word mais à esquerda, em seguida, selecione **encapsular na entidade composta** na lista que aparece, em seguida, selecione o word mais à direita, em seguida, selecione a entidade nomeada composta `FlightReservation`. Este é um passo de tranqüila e rápido de seleções, divididas nos seguintes passos:

1. Na expressão `schedule 4 seats from paris to london for april 1`, selecione o 4 como entidade pré-criados numérica.

    ![Selecionar palavra mais à esquerda](./media/luis-tutorial-composite-entity/wrap-composite-step-1.png)

2. Selecione **encapsular na entidade composta** na lista que é apresentada.

    ![Selecione moldagem da lista](./media/luis-tutorial-composite-entity/wrap-composite-step-2.png)

3. Selecione o word mais à direita. É apresentada uma linha verde sob a frase, indicando uma entidade composta.

    ![Selecionar palavra mais à direita](./media/luis-tutorial-composite-entity/wrap-composite-step-3.png)

4. Selecionar nome composto `FlightReservation` na lista que é apresentada.

    ![Selecione a entidade nomeada de compostos](./media/luis-tutorial-composite-entity/wrap-composite-step-4.png)

    Para a última expressão, encapsular `London` e `tomorrow` na entidade composta, com as mesmas instruções. 

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações às intenções e entidades (o modelo), até ser preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Preparar a aplicação](./media/luis-tutorial-composite-entity/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Preparação concluída com êxito](./media/luis-tutorial-composite-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    ![publicar aplicação](./media/luis-tutorial-composite-entity/publish-to-production.png)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    ![Selecione o URL de ponto final](./media/luis-tutorial-composite-entity/publish-select-endpoint.png)

2. Vá para o final do URL no endereço e introduza `reserve 3 seats from London to Cairo on Sunday`. O último parâmetro de cadeia de consulta é `q`, a consulta de expressão. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver a intenção `BookFlight` com a entidade hierárquica extraída.

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

Esta expressão devolve uma matriz de entidades compostas incluindo o **flightreservation** objeto com os dados extraídos.  

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com apenas dois objetivos e uma entidade composta, identificou uma intenção da consulta de linguagem natural e devolveu os dados extraídos. 

Agora, o seu chatbot tem informações suficientes para determinar a ação principal, `BookFlight`e as informações de reserva encontradas na expressão. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados da entidade para executar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="next-steps"></a>Passos Seguintes

[Saiba mais sobre entidades](luis-concept-entity-types.md). 
