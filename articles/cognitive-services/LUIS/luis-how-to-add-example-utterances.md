---
title: Adicionar utterances de exemplo em aplicações LUIS | Microsoft Docs
titleSuffix: Azure
description: Saiba como adicionar utterances em aplicações de compreensão de idiomas (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 12cbffdbd9ab299d4405aa50c0e49db9c20ab0fa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355759"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Adicionar utterances de exemplo e a etiqueta com entidades

Utterances são exemplos de perguntas de utilizador ou de comandos. Para lhe ensinar compreensão de idiomas (LUIS), tem de adicionar [utterances exemplo](luis-concept-utterance.md) para um [intenção](luis-concept-intent.md).

Geralmente, adicione um utterance primeiro e, em seguida, criar entidades e etiqueta utterances na página de intenção. Se em vez disso criaria entidades pela primeira vez, consulte o artigo [adicionar entidades](luis-how-to-add-entities.md).

## <a name="add-an-utterance"></a>Adicionar um utterance
Na página de intenção, introduza um utterance relevante que se espera de utilizadores, tais como `book 2 adult business tickets to Paris tomorrow on Air France` na caixa de texto abaixo intenção nome e, em seguida, prima Enter. 
 
>[!NOTE]
>LUIS converte todos os utterances em minúsculas.

![Página de detalhes de captura de ecrã de pendentes, com utterance realçado](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Utterances são adicionados à lista de utterances para o objetivo atual. 

## <a name="add-simple-entity-label"></a>Adicione a etiqueta de entidade simples
No procedimento seguinte, crie e etiqueta personalizadas entidades dentro de utterance seguinte na página de intenção:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. Selecione "França ondas Eletromagnéticas" utterance para identificá-lo como uma entidade simple.

    > [!NOTE]
    > Quando selecionar palavras etiquetá-los como entidades:
    > * Para uma única palavra, basta selecioná-lo. 
    > * Para um conjunto de dois ou mais palavras, selecione no início e, em seguida, no final do conjunto.

2. Na caixa de lista pendente de entidade apresentada, pode selecionar uma entidade existente ou adicionar uma nova entidade. Para adicionar uma nova entidade, escreva o respetivo nome na caixa de texto e, em seguida, selecione **criar nova entidade**. 
 
    ![Página de detalhes de captura de ecrã de pendentes, com entidade simple etiquetagem opção realçada](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. No **que tipo de entidade que pretende criar?** caixa de diálogo de pop-up, verifique o nome da entidade e selecione o tipo de entidade simples e, em seguida, selecione **feito**.

    ![Imagem da caixa de diálogo de confirmação](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Consulte [extração de dados](luis-concept-data-extraction.md#simple-entity-data) para saber mais sobre a extrair a entidade simple do ponto final de uma resposta à consulta JSON. Tente a entidade simple [início rápido](luis-quickstart-primary-and-secondary-data.md) para obter mais informações sobre como utilizar uma entidade simple.


## <a name="add-list-entity-and-label"></a>Adicionar a entidade de lista e a etiqueta
Lista de entidades representam um conjunto fixo, fechado (texto exato corresponde) das palavras relacionados no seu sistema. 

Para uma entidade de lista drinks, pode ter dois valores normalizados: máximo e soda pop. Cada nome normalizado tem sinónimos. Para os níveis máximos, os sinónimos são H20, gás, flat. Para o pop do soda, os sinónimos são acessíveis, cola, ginger. Não tem de conhecer todos os valores quando criar a entidade. Pode adicionar mais depois de rever utterances de utilizador reais com sinónimos.

|Nome normalizado|Sinónimos|
|--|--|
|Água|H20, gás, plana|
|Soda pop|Acessíveis, cola, ginger|

Ao criar uma nova entidade na lista da página de intenção, estão a fazer duas coisas que podem não ser óbvias. Em primeiro lugar, está a criar uma nova lista através da adição do primeiro item da lista. Segundo, o primeiro item de lista é denominado com a palavra ou expressão que tiver escolhido o utterance. Enquanto pode alterá-las mais tarde a partir da página de entidade, poderá ser mais rápida selecionar um utterance que tenha o word que pretende para o nome do item da lista.

Por exemplo, se pretender criar uma lista de tipos de drink e selecionado o word `h2o` de utterance para criar a entidade, a lista teria um item, cujo nome foi h20. Se quisesse um nome mais genérico, deve escolher um utterance que utiliza o nome mais genérico. 

1. No utterance, selecione o word que é o primeiro item na lista, e, em seguida, introduza o nome da lista na caixa de texto, em seguida, selecione **criar nova entidade**.   

    ![Página de detalhes de captura de ecrã de pendentes, com criar nova entidade realçada](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. No **que tipo de entidade que pretende criar?** diálogo caixa, adicionar sinónimos deste item de lista. Para o item máximo numa lista drink, adicionar `h20`, `perrier`, e `waters`e selecione **feito**. Tenha em atenção que "waters" foi adicionadas porque os sinónimos de lista são correspondidos ao nível do token. Na cultura inglês, se o nível é ao nível do word, por isso, "waters" seria não corresponder à "nível máximo", a menos que estava na lista. 

    ![Captura de ecrã do tipo de entidade pretende criar a caixa de diálogo](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    Esta lista de drinks tem drink apenas um tipo, máximo. Pode adicionar mais tipos de drink por etiquetagem outros utterances ou ao editar a entidade do **entidades** no painel de navegação esquerdo. [Editar](luis-how-to-add-entities.md#add-list-entities) as entidades dá-lhe as opções da introdução de itens adicionais com sinónimos correspondentes ou [importar](luis-how-to-add-entities.md#import-list-entity-values) uma lista. 

    Consulte [extração de dados](luis-concept-data-extraction.md#list-entity-data) para saber mais sobre a extração de entidades de lista o ponto final de uma resposta à consulta JSON. Repita o [início rápido](luis-quickstart-intent-and-list-entity.md) para obter mais informações sobre como utilizar uma entidade de lista.

## <a name="add-synonyms-to-the-list-entity"></a>Adicionar sinónimos para a entidade de lista 
Adicione um sinónimo para a entidade de lista, selecionando a palavra ou frase no utterance. Se tiver um Drink lista entidade e pretende adicionar `agua` como um sinónimo de níveis máximos, siga os passos:

Utterance, selecione como o word synonymous, `aqua` para máximo, em seguida, selecione o nome da entidade lista na lista pendente, tais como **Drink**, em seguida, selecione **definido como sinónimo**, em seguida, selecione na lista o item é synonymous com, tal como **máximo**.

![Página de detalhes de captura de ecrã de pendentes, com a criar um novo sinónimo realçado](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Criar um novo item para a entidade de lista
Crie um novo item de uma entidade de lista existentes, selecionando a palavra ou frase no utterance. Se tiver um Drink lista e pretende adicionar `tea` como um novo item, siga os passos:

Utterance, selecione como o word para o novo item da lista, `tea`, em seguida, selecione o nome da entidade lista na lista pendente, tais como **Drink**, em seguida, selecione **criar um novo sinónimo**. 

![Captura de ecrã do adicionar novo item da lista](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

A palavra está agora realçada azul. Se paira o rato sobre o word, uma tag mostra o nome do item de lista, tais como tea.

![Captura de ecrã da nova etiqueta de item de lista](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Moldar entidades na etiqueta composta
Entidades compostas criadas a partir de **entidades**. Não é possível criar uma entidade composta a partir da página de intenção. Assim que a entidade composta for criada, pode encapsular as entidades num utterance na página de intenção. 

Partindo do princípio de utterance, `book 2 tickets from Seattle to Cairo`, um utterance composto pode devolver informações de entidade do número de pedidos de suporte (2), a origem (Seattle) e o destino de localizações (Cairo) na entidade de um único elemento principal. 

Siga estes [passos](luis-how-to-add-entities.md#add-prebuilt-entity) para adicionar o **número** prebuilt entidade. Depois de criar a entidade, o `2` o utterance é azul, indicando que é uma entidade com nome. Entidades prebuilt são etiquetadas pelo LUIS. Não é possível adicionar ou remover a etiqueta de entidade prebuilt um utterance único. Só pode adicionar ou remover todas as etiquetas prebuilt adicionando ou removendo a entidade prebuilt da aplicação.

Siga estes [passos](#add-hierarchical-entity-and-label) para criar um **localização** entidade hierárquica. Etiqueta as localizações de origem e de destino na utterance de exemplo. 

Antes de moldar as entidades a uma entidade composto, certifique-se todas as entidades de subordinados são realçadas azul, o que significa que tem sido etiqueta no utterance.

1. Moldar as entidades individuais para um compostos, selecione a entidade identificada primeiro utterance para a entidade composta. No utterance exemplo, `book 2 tickets from Seattle to Cairo`, a primeira entidade é o número de 2. É apresentada uma lista de lista pendente que mostra as opções para esta seleção.

    ![Captura de ecrã do número selecionado e as opções de lista pendente realçadas](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Selecione **moldar entidade composta** na lista pendente. 

    ![Captura de ecrã das opções de lista pendente de moldagem entidade composta com moldagem na entidade composta realçada](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Selecione o word último da entidade composta. No utterance deste exemplo, selecione "Location::Destination" (que representa Cairo). A linha de verde é agora em todas as palavras, incluindo palavras não entidade, no utterance que estão o compostos.

    ![Página de captura de ecrã da intenção BookFlight, com o número de realçado](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Selecione o nome da entidade composto da lista pendente. Neste exemplo, que é **TicketOrder**.

    ![Captura de ecrã da palavras com composto entidade, de encapsulamento de aplicações com o nome da entidade composto realçado na lista pendente](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Quando moldar as entidades corretamente, uma linha verde está sob o frase completo.

    ![Captura de ecrã do utterance com entidade composta realçada](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Consulte [extração de dados](luis-concept-data-extraction.md#composite-entity-data) para saber mais sobre a extrair a entidade composta do ponto final de uma resposta à consulta JSON. Tente a entidade composta [tutorial](luis-tutorial-composite-entity.md) para obter mais informações sobre como utilizar uma entidade composta.

## <a name="add-hierarchical-entity-and-label"></a>Adicionar entidade hierárquica e etiqueta
Uma entidade hierárquica é uma categoria de entidades, essencialmente, relacionadas e reconhecimento do contexto adquiridas. No exemplo seguinte, a entidade contém localizações de origem e de destino. 

No utterance `Book 2 tickets from Seattle to Cairo`, Seattle é a localização de origem e Cairo é a localização de destino. Cada localização é reconhecimento do contexto diferente e adquiridas da ordem de word e escolha word no utterance.

1. Na página de intenção, de utterance, selecione "Seattle", em seguida, introduza o nome da entidade ' localização e, em seguida, selecione **criar nova entidade**.

    ![Captura de ecrã de criar hierárquica entidade etiquetagem caixa de diálogo](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. Na caixa de diálogo de pop-up, selecione hierárquica para **tipo de entidade**, em seguida, adicione `Origin` e `Destination` como elementos subordinados e, em seguida, selecione **feito**.

    ![Página de detalhes de captura de ecrã de pendentes, com a entidade de ToLocation realçada](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. A palavra no utterance foi etiquetada com a entidade hierárquica principal. Terá de atribuir a palavra a entidade subordinada. Regressar ao utterance na página de intenção. Selecione o word, em seguida, na lista pendente, escolha o nome da entidade que criou e siga o menu à direita para escolher a entidade subordinado correto.

    ![Página de detalhes de captura de ecrã de pendentes, com a entidade de ToLocation realçada](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Os nomes das entidades do subordinado devem ser exclusivos em todas as entidades numa única aplicação. Duas entidades hierárquicas diferentes não podem conter entidades subordinado com o mesmo nome. 

    Consulte [extração de dados](luis-concept-data-extraction.md#hierarchical-entity-data) para saber mais sobre a extrair a entidade hierárquica do ponto final de uma resposta à consulta JSON. Tente a entidade hierárquica [início rápido](luis-quickstart-intent-and-hier-entity.md) para obter mais informações sobre como utilizar uma entidade hierárquica.


## <a name="remove-entity-labels-from-utterances"></a>Remover etiquetas de entidade utterances
Pode remover etiquetas de entidade adquiridos a máquina a um utterance na página de intenção. Se a entidade não é adquiridos por máquina, não é possível remover de um utterance. Se precisar de remover uma entidade não aprendidas máquina o utterance, terá de eliminar a entidade da aplicação completa. 

Para remover uma etiqueta de entidade adquiridos a máquina de um utterance, selecione a entidade de utterance. Em seguida, selecione **remover etiqueta** na caixa de lista pendente de entidade que é apresentada.

![Página de detalhes de captura de ecrã de pendentes, com remover etiqueta realçada](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Adicione a etiqueta de entidade prebuilt
Se adicionar as entidades prebuilt à sua aplicação LUIS, não terá utterances de etiqueta com estas entidades. Para saber mais sobre entidades prebuilt e como adicioná-las, consulte o artigo [adicionar entidades](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="add-regular-expression-entity-label"></a>Adicione a etiqueta de entidade de expressão regular
Se adicionar as entidades de expressão regular para a sua aplicação LUIS, não terá utterances de etiqueta com estas entidades. Para saber mais sobre entidades de expressão regular e como adicioná-las, consulte o artigo [adicionar entidades](luis-how-to-add-entities.md#add-regular-expression-entities).

## <a name="create-a-pattern-from-an-utterance"></a>Criar um padrão a partir de um utterance
Consulte [adicionar padrão do utterance existente na página de intenção ou entidade](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="add-patternany-entity-label"></a>Adicionar pattern.any entidade etiqueta
Se adicionar as entidades de pattern.any à sua aplicação LUIS, não é possível etiqueta utterances com estas entidades. Só são válidos em padrões. Para saber mais sobre entidades pattern.any e como adicioná-las, consulte [adicionar entidades](luis-how-to-add-entities.md#add-patternany-entities).

<!--
Fix this - moved to luis-how-to-add-intents.md - how ?

## Search in utterances
## Prediction discrepancy errors
## Filter by intent prediction discrepancy errors
## Filter by entity type
## Switch to token view
## Delete utterances
## Edit an utterance
## Reassign utterances

-->
## <a name="train-your-app-after-changing-model-with-utterances"></a>Preparar a sua aplicação depois de alterar o modelo com utterances
Depois de adicionar, editar ou remover utterances, [preparar](luis-how-to-train.md) e [publicar](PublishApp.md) a aplicação para as suas alterações afetar a consultas de ponto final. 

## <a name="next-steps"></a>Passos Seguintes

Depois de etiquetagem utterances no seu pendentes, pode agora criar um [entidade composta](luis-how-to-add-entities.md).
