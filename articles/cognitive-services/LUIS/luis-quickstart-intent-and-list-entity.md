---
title: Tutorial para criar uma aplicação LUIS para obter dados listados de correspondência exata de texto - Azure | Microsoft Docs
description: Neste tutorial, saiba como criar uma aplicação LUIS simples com intenções e entidades de lista para extrair dados.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 33394dff1091f27c79c74d8648a90724ba8d6698
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264832"
---
# <a name="tutorial-create-app-using-a-list-entity"></a>Tutorial: Criar uma aplicação com uma entidade de lista
Neste tutorial, vai criar uma aplicação que demonstra como obter dados que correspondem a uma lista predefinida. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender as entidades de lista 
> * Criar uma nova aplicação LUIS para o domínio de bebidas com a intenção OrderDrinks
> * Adicionar a intenção _None_ (Nenhuma) e adicionar expressões de exemplo
> * Adicionar a entidade de lista para extrair itens de bebidas da expressão
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS

Para este artigo, precisa de uma conta do [LUIS][LUIS] gratuita para criar a sua aplicação LUIS.

## <a name="purpose-of-the-list-entity"></a>Objetivo da entidade de lista
Esta aplicação utiliza pedidos de bebidas, tais como `1 coke and 1 milk please`, e devolve os dados, tais como o tipo de bebida. Uma entidade de **lista** de bebidas procura correspondências exatas de texto e devolve essas correspondências. 

Uma entidade de lista é uma boa opção para este tipo de dados quando os valores dos dados são um conjunto conhecido. Os nomes das bebidas podem variar, incluindo gíria e abreviaturas, mas os nomes não mudam frequentemente. 

## <a name="app-intents"></a>Intenções da aplicação
As intenções são categorias daquilo que o utilizador pretende. Esta aplicação tem duas intenções: OrderDrink e None. A intenção [None](luis-concept-intent.md#none-intent-is-fallback-for-app) é intencional, para indicar qualquer coisa fora da aplicação.  

## <a name="list-entity-is-an-exact-text-match"></a>A entidade de lista é uma correspondência exata de texto
O objetivo da entidade é localizar e categorizar partes do texto na expressão. A entidade de [lista](luis-concept-entity-types.md) permite uma correspondência exata de palavras ou expressões.  

Para esta aplicação de bebidas, o LUIS extrai o pedido de bebidas de forma a que possa ser criado e preenchido um pedido padrão. O LUIS permite que as expressões tenham variações, abreviaturas e gíria. 

As expressões de exemplo simples dos utilizadores incluem:

```
2 glasses of milk
3 bottles of water
2 cokes
```

As versões abreviadas ou de gíria das expressões incluem:

```
5 milk
3 h2o
1 pop
```
 
A entidade de lista corresponde `h2o` a água e `pop` a refrigerante.  

## <a name="what-luis-does"></a>O que faz o LUIS
Quando a intenção e as entidades da expressão são identificadas, [extraídas](luis-concept-data-extraction.md#list-entity-data) e devolvidas em JSON a partir do [ponto final](https://aka.ms/luis-endpoint-apis), o LUIS está concluído. A aplicação de chamada ou chatbot pega nessa resposta JSON e satisfaz o pedido, da forma que a aplicação ou o chatbot foi concebido para o fazer. 

## <a name="create-a-new-app"></a>Criar uma nova aplicação
1. Inicie sessão no site do [LUIS][LUIS]. Certifique-se de que inicia sessão na [região][LUIS-regions] onde precisa que os pontos finais do LUIS sejam publicados.

2. No site do [LUIS][LUIS], selecione **Create new app** (Criar nova aplicação).  

    ![Criar nova aplicação](./media/luis-quickstart-intent-and-list-entity/app-list.png)

3. Na caixa de diálogo de pop-up, introduza o nome `MyDrinklist`. 

    ![Atribua à aplicação o nome MyDrinkList](./media/luis-quickstart-intent-and-list-entity/create-app-dialog.png)

4. Quando esse processo terminar, a aplicação mostra a página **Intents** (Intenções) com a intenção **None** (Nenhuma). 

    [![](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png "Captura de ecrã da página Intenções")](media/luis-quickstart-intent-and-list-entity/intents-page-none-only.png#lightbox)

## <a name="create-a-new-intent"></a>Criar uma nova intenção

1. Na página **Intents** (Intenções), selecione **Create new intent** (Criar nova intenção). 

    [![](media/luis-quickstart-intent-and-list-entity/create-new-intent.png "Captura de ecrã da página Intenções com o botão Criar nova intenção realçado")](media/luis-quickstart-intent-and-list-entity/create-new-intent.png#lightbox)

2. Introduza o nome da nova intenção `OrderDrinks`. Esta intenção deve ser selecionada sempre que um utilizador pretenda pedir uma bebida.

    Ao criar uma intenção, está a criar a categoria principal de informações que pretende identificar. Atribuir um nome à categoria permite que qualquer outra aplicação que utilize os resultados da consulta do LUIS utilize esse nome de categoria para encontrar uma resposta adequada ou para executar a ação apropriada. O LUIS não responde a estas perguntas, apenas identifica o tipo de informação que está a ser pedida em linguagem natural. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png "Captura de ecrã da criação da nova intenção OrderDrinks")](media/luis-quickstart-intent-and-list-entity/intent-create-dialog-order-drinks.png#lightbox)

3. Adicione várias expressões à intenção `OrderDrinks`, que espera que um utilizador peça, tais como:

    | Expressões de exemplo|
    |--|
    |Enviar 2 Coca-Colas e uma garrafa de água para o meu quarto|
    |2 Perriers com uma casca de lima|
    |h20|

    [![](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png "Captura de ecrã da introdução de expressões na página da intenção OrderDrinks")](media/luis-quickstart-intent-and-list-entity/intent-order-drinks-utterance.png#lightbox)

## <a name="add-utterances-to-none-intent"></a>Adicionar expressões à intenção None

Atualmente, a aplicação LUIS não tem expressões para a intenção **None** (Nenhuma). Precisa de expressões que não queira que a aplicação responda e, por isso, tem de ter expressões na intenção **None** (Nenhuma). Não deixe em branco. 

1. Selecione **Intents** (Intenções) no painel esquerdo. 

    [![](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png "Captura de ecrã da seleção da ligação Intenções no painel esquerdo")](media/luis-quickstart-intent-and-list-entity/left-panel-intents.png#lightbox)

2. Selecione a intenção **None** (Nenhuma). Adicione três expressões que o utilizador poderá introduzir, mas que não são relevantes para a sua aplicação:

    | Expressões de exemplo|
    |--|
    |Cancelar!|
    |Adeus|
    |O que está a acontecer?|

## <a name="when-the-utterance-is-predicted-for-the-none-intent"></a>Quando a expressão é prevista para a intenção None
Na sua aplicação de chamada do LUIS (como um chatbot), quando o LUIS devolve a intenção **None** (Nenhuma) para uma expressão, o seu bot pode perguntar se o utilizador quer terminar a conversa. O bot também pode dar mais instruções para continuar a conversa se o utilizador não pretender terminá-la. 

As entidades funcionam na intenção **None** (Nenhuma). Se a intenção com a melhor classificação for **None**, mas for extraída uma entidade que é relevante para o seu chatbot, o chatbot pode dar seguimento com uma pergunta que realce a intenção do cliente. 

## <a name="create-a-menu-entity-from-the-intent-page"></a>Criar uma entidade de menu a partir da página Intenção
Agora que as duas intenções têm expressões, o LUIS tem de compreender o que é uma bebida. Navegue de volta para a intenção `OrderDrinks` e identifique (marque) as bebidas numa expressão, seguindo os passos:

1. Volte à intenção `OrderDrinks`, selecionando **Intents** (Intenções) no painel esquerdo.

2. Selecione `OrderDrinks` na lista de intenções.

3. Na expressão `Please send 2 cokes and a bottle of water to my room`, selecione a palavra `water`. É apresentado um menu pendente com uma caixa de texto na parte superior para criar uma nova entidade. Introduza o nome da entidade `Drink` na caixa de texto e selecione **Create new entity** (Criar nova entidade) no menu pendente. 

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png "Captura de ecrã da criação da nova entidade, selecionando a palavra na expressão")](media/luis-quickstart-intent-and-list-entity/intent-label-h2o-in-utterance.png#lightbox)

4. Na janela de pop-up, selecione o tipo de entidade **List** (Lista). Adicione o sinónimo `h20`. Selecione a tecla Enter após cada sinónimo. Não adicione `perrier` à lista de sinónimos. É adicionado no passo seguinte como exemplo. Selecione **Done** (Concluído).

    [![](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png "Captura de ecrã da configuração da nova entidade")](media/luis-quickstart-intent-and-list-entity/create-list-ddl.png#lightbox)

5. Agora que a entidade está criada, identifique os outros sinónimos para água, selecionando o sinónimo para água e, em seguida, selecione `Drink` na lista pendente. Siga o menu à direita, selecione `Set as synonym` e, em seguida, selecione `water`.

    [![](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png "Captura de ecrã da identificação da expressão com a entidade existente")](media/luis-quickstart-intent-and-list-entity/intent-label-perriers.png#lightbox)

## <a name="modify-the-list-entity-from-the-entity-page"></a>Modificar a entidade de lista na página Entidade
A entidade de lista de bebidas é criada, mas não tem muitos itens e sinónimos. Se conhecer alguns dos termos, abreviaturas e gíria, é mais rápido preencher a lista na página **Entity** (Entidade). 

1. Selecione **Entities** (Entidades) no painel esquerdo.

    [![](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png "Captura de ecrã da seleção da ligação Entidades no painel esquerdo")](media/luis-quickstart-intent-and-list-entity/intent-select-entities.png#lightbox)

2. Selecione `Drink` na lista de entidades.

    [![](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png "Captura de ecrã da seleção da entidade Drink na lista de entidades")](media/luis-quickstart-intent-and-list-entity/entities-select-drink-entity.png#lightbox)

3. Na caixa de texto, introduza `Soda pop` e selecione Enter. Este é um termo que é aplicado de um modo geral às bebidas gaseificadas. Cada cultura tem uma alcunha ou um termo de gíria para este tipo de bebida.

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png "Captura de ecrã da introdução do nome canónico")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-canonical-name.png#lightbox)

4. Na mesma linha que `Soda pop`, introduza sinónimos, tais como: 

    ```
    coke
    cokes
    coca-cola
    coca-colas
    ```

    Os sinónimos podem incluir expressões, pontuação, pronomes possessivos e plurais. Uma vez que a entidade de lista é uma correspondência exata de texto (à exceção das maiúsculas e minúsculas), os sinónimos têm de ter todas as variações. Pode expandir a lista à medida que aprende mais variações nos registos de consulta ou ao rever os resultados do ponto final. 

    Este artigo tem apenas alguns sinónimos, para manter o exemplo curto. Uma aplicação LUIS de nível de produção teria muitos mais sinónimos e seria revista e expandida regularmente. 

    [![](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png "Captura de ecrã da adição de sinónimos")](media/luis-quickstart-intent-and-list-entity/drink-entity-enter-synonyms.png#lightbox)

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações às intenções e entidades (o modelo), até ser preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Preparar a aplicação](./media/luis-quickstart-intent-and-list-entity/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Preparação concluída com êxito](./media/luis-quickstart-intent-and-list-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

    [![](media/luis-quickstart-intent-and-list-entity/publish.png "Captura de ecrã da seleção do botão Publicar")](media/luis-quickstart-intent-and-list-entity/publish.png#lightbox)

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar). 

    [![](media/luis-quickstart-intent-and-list-entity/publish-to-production.png "Captura de ecrã da seleção do botão Publicar no bloco de produção")](media/luis-quickstart-intent-and-list-entity/publish-to-production.png#lightbox)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    [![](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png "Captura de ecrã do URL do ponto final na página Publicar")](media/luis-quickstart-intent-and-list-entity/publish-select-endpoint.png#lightbox)

2. Vá para o final do URL no endereço e introduza `2 cokes and 3 waters`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver a intenção `OrderDrinks` com os dois tipos de bebida `cokes` e `waters`.

```
{
  "query": "2 cokes and 3 waters",
  "topScoringIntent": {
    "intent": "OrderDrinks",
    "score": 0.999998569
  },
  "intents": [
    {
      "intent": "OrderDrinks",
      "score": 0.999998569
    },
    {
      "intent": "None",
      "score": 0.23884207
    }
  ],
  "entities": [
    {
      "entity": "cokes",
      "type": "Drink",
      "startIndex": 2,
      "endIndex": 6,
      "resolution": {
        "values": [
          "Soda pop"
        ]
      }
    },
    {
      "entity": "waters",
      "type": "Drink",
      "startIndex": 14,
      "endIndex": 19,
      "resolution": {
        "values": [
          "h20"
        ]
      }
    }
  ]
}
```

## <a name="where-is-the-natural-language-processing-in-the-list-entity"></a>Onde está o processamento da linguagem natural na entidade de lista? 
Uma vez que a entidade de lista é uma correspondência exata de texto, não depende do processamento de linguagem natural (ou aprendizagem automática). O LUIS utiliza o processamento de linguagem natural (ou aprendizagem automática) para selecionar a intenção correta com a melhor classificação. Além disso, uma expressão pode ser uma mistura de mais de uma entidade ou mesmo de mais do que um tipo de entidade. Cada expressão é processada para todas as entidades na aplicação, incluindo as entidades de processamento de linguagem natural (ou aprendizagem automática), como a entidade **Simple** (Simples).

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com apenas duas intenções e uma entidade de lista, identificou uma intenção de consulta de linguagem natural e devolveu os dados extraídos. 

Agora, o seu chatbot tem informações suficientes para determinar a ação principal, `OrderDrinks`, e os tipos de bebidas que foram pedidas a partir da entidade de lista Drink. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados da entidade para executar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Delete** (Eliminar). Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Saiba como adicionar uma entidade de expressão regular](luis-quickstart-intents-regex-entity.md)

Adicione a [entidade pré-criada](luis-how-to-add-entities.md#add-prebuilt-entity) **number** para extrair o número. 

<!--References-->
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[LUIS-regions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#publishing-regions
