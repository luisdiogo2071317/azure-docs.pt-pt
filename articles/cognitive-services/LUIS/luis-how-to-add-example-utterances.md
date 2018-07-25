---
title: Adicionar expressões de exemplo em aplicativos de LUIS | Documentos da Microsoft
titleSuffix: Azure
description: Saiba como adicionar expressões em aplicativos de compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: cf6fc131a1ae2ba0a77bdbeda942c9f85aadd189
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39226292"
---
# <a name="add-example-utterances-and-label-with-entities"></a>Adicionar expressões de exemplo e a etiqueta com entidades

Expressões com de exemplo são exemplos de texto de perguntas de utilizador ou de comandos. Ensinar a compreensão de idiomas (LUIS), precisa adicionar [expressões de exemplo](luis-concept-utterance.md) para um [intenção](luis-concept-intent.md).

Em geral, adicionar uma expressão de exemplo para um objetivo pela primeira vez e, em seguida, criar entidades e expressões de etiqueta na página de intenção. Se o primeiro, em vez disso, seria criar entidades, veja [adicionar entidades](luis-how-to-add-entities.md).

## <a name="add-an-utterance"></a>Adicionar uma expressão
Numa página de intenção, introduza uma expressão de exemplo relevantes que se espera de seus usuários, como `book 2 adult business tickets to Paris tomorrow on Air France` na caixa de texto abaixo do nome intenção e, em seguida, prima Enter. 
 
>[!NOTE]
>LUIS converte todas as expressões de com em minúsculas.

![Página de detalhes de captura de ecrã de objetivos, com a expressão realçado](./media/luis-how-to-add-example-utterances/add-new-utterance-to-intent.png) 

Expressões com são adicionados à lista de expressões com para o objetivo atual. 

## <a name="ignoring-words-and-punctuation"></a>A ignorar a palavras e pontuação
Se pretender ignorar palavras específicas ou pontuação na expressão de exemplo, utilize um [padrão](luis-concept-patterns.md#pattern-syntax) com o _ignorar_ sintaxe. 

## <a name="add-simple-entity-label"></a>Adicionar etiqueta de entidade
O procedimento seguinte, cria e entidades personalizadas dentro da expressão seguinte na página da intenção da etiqueta:

```
book me 2 adult business tickets to Paris tomorrow on Air France
```

1. Selecione "Ar França" na expressão que identifique-la como uma entidade.

    > [!NOTE]
    > Quando selecionar as palavras que identifique-los como entidades:
    > * Para uma única palavra, basta selecioná-lo. 
    > * Para um conjunto de duas ou mais palavras, selecione no início e, em seguida, no final do conjunto.

2. Na caixa de lista pendente de entidade que aparece, pode selecionar uma entidade existente ou adicionar uma nova entidade. Para adicionar uma nova entidade, digite seu nome na caixa de texto e, em seguida, selecione **criar nova entidade**. 
 
    ![Página de detalhes de captura de ecrã de objetivos, com a entidade simple etiquetagem opção realçada](./media/luis-how-to-add-example-utterances/create-airline-simple-entity.png)

3. Na **o tipo de entidade que pretende criar?** caixa de diálogo pop-up, verifique se o nome da entidade e selecione o tipo de entidade simples e, em seguida, selecione **feito**.

    ![Imagem da caixa de diálogo de confirmação](./media/luis-how-to-add-example-utterances/create-simple-airline-entity.png)

    Ver [extração de dados](luis-concept-data-extraction.md#simple-entity-data) para saber mais sobre a entidade simple a extração do ponto de extremidade de resposta da consulta JSON. Experimente a entidade simple [guia de introdução](luis-quickstart-primary-and-secondary-data.md) para saber mais sobre como utilizar uma entidade.


## <a name="add-list-entity-and-label"></a>Adicionar entidade de lista e etiqueta
Lista de entidades representam um conjunto fixo, fechado (corresponde a texto exato) de palavras relacionadas no seu sistema. 

Para uma entidade de lista drinks, pode ter dois valores normalizados: água e soda pop. Cada nome normalizado tem sinónimos. Para água, os sinónimos são H20, gás, fixa. Para soda pop, os sinónimos são frutos, cola, ginger. Não precisa saber todos os valores quando criar a entidade. Pode adicionar mais depois de rever as expressões de utilizador real com sinónimos.

|Nome normalizado|Sinónimos|
|--|--|
|Água|H20, gás, fixa|
|Pop da soda|Frutos, cola, ginger|

Ao criar uma nova entidade de lista a partir da página de intenção, estão a fazer duas coisas que talvez não sejam óbvias. Em primeiro lugar, está a criar uma nova lista ao adicionar o primeiro item da lista. Em segundo lugar, o primeiro item da lista com o nome com a palavra ou frase que tiver escolhido a expressão. Enquanto pode alterá-las mais tarde a partir da página de entidade, pode ser mais rápido selecionar uma expressão com a palavra que pretende para o nome do item da lista.

Por exemplo, se quiser criar uma lista de tipos de bebida e é selecionada a palavra `h2o` da expressão para criar a entidade, a lista teria um item, cujo nome foi h20. Se quisesse um nome mais genérico, deve escolher uma expressão que utiliza o nome mais genérico. 

1. Na expressão, selecione a palavra que é o primeiro item na lista, e, em seguida, introduza o nome da lista na caixa de texto, em seguida, selecione **criar nova entidade**.   

    ![Página de detalhes de captura de ecrã de objetivos, com a nova entidade de criar realçada](./media/luis-how-to-add-example-utterances/create-drink-list-entity.png)

2. Na **o tipo de entidade que pretende criar?** diálogo caixa, adicionar sinónimos deste item de lista. Para o item de água numa lista de bebida, adicione `h20`, `perrier`, e `waters`e selecione **feito**. Tenha em atenção que "coisas" são adicionadas uma vez que correspondem os sinónimos de lista no nível do token. Não seriam possível corresponder na cultura inglês, que nível está no nível do word tão "coisas" para "água", a menos que ele estava na lista. 

    ![Captura de ecrã do tipo de entidade fazer que pretende criar a caixa de diálogo](./media/luis-how-to-add-example-utterances/drink-list-ddl.png)

    Esta lista de drinks tem o tipo de bebida apenas uma, água. Pode adicionar mais tipos de bebida por etiquetagem outras expressões ou ao editar a entidade a partir da **entidades** na navegação à esquerda. [Edição](luis-how-to-add-entities.md#add-list-entities) as entidades dá-lhe as opções da introdução itens adicionais com sinónimos correspondentes ou [importar](luis-how-to-add-entities.md#import-list-entity-values) uma lista. 

    Ver [extração de dados](luis-concept-data-extraction.md#list-entity-data) para saber mais sobre a extração de entidades de lista do ponto de extremidade de resposta da consulta JSON. Experimente o [guia de introdução](luis-quickstart-intent-and-list-entity.md) para saber mais sobre como utilizar uma entidade de lista.

## <a name="add-synonyms-to-the-list-entity"></a>Adicionar sinónimos para a entidade de lista 
Adicione um sinónimos para a entidade de lista ao selecionar a palavra ou frase na expressão. Se tiver uma bebida lista entidade e quiser adicionar `agua` como sinônimo de água, siga os passos:

Na expressão, selecione a palavra sinônimos, tal como `aqua` de água, em seguida, selecione o nome da entidade de lista na lista pendente, tais como **beber**, em seguida, selecione **definir como sinónimos**, em seguida, selecione a lista item é sinônimo, tal como **água**.

![Página de detalhes de captura de ecrã de objetivos, com criar um novo sinónimo realçado](./media/luis-how-to-add-example-utterances/set-agua-as-synonym.png)

## <a name="create-new-item-for-list-entity"></a>Criar novo item para a entidade de lista
Crie um novo item de uma entidade de lista existentes ao selecionar a palavra ou frase na expressão. Se tiver uma bebida, listar e quiser adicionar `tea` como um novo item, siga os passos:

A expressão, selecione como o word para o novo item de lista, `tea`, em seguida, selecione o nome da entidade de lista na lista pendente, tais como **beber**, em seguida, selecione **criar um novo sinónimos**. 

![Captura de ecrã de adicionar novo item da lista](./media/luis-how-to-add-example-utterances/list-entity-create-new-item.png)

A palavra agora é realçada em azul. Se passar o mouse sobre a palavra, uma etiqueta mostra o que mostra o nome do item de lista, como o chá.

![Captura de ecrã da nova etiqueta de item de lista](./media/luis-how-to-add-example-utterances/list-entity-item-name-tag.png)

## <a name="wrap-entities-in-composite-label"></a>Encapsular as entidades na etiqueta composta
Entidades compostas são criadas a partir **entidades**. Não é possível criar uma entidade composta da página de intenção. Depois de criar a entidade composta, pode encapsular as entidades numa expressão na página de intenção. 

Supondo que a expressão, `book 2 tickets from Seattle to Cairo`, uma expressão composta pode retornar informações da entidade de contagem de pedidos de suporte (2), a origem (Seattle) e o destino localizações (Cairo) numa entidade único elemento principal. 

Siga estes [passos](luis-how-to-add-entities.md#add-prebuilt-entity) para adicionar o **número** entidade pré-criados. Depois da entidade é criada, o `2` a expressão é azul, que indica que é uma entidade com nome. Entidades pré-concebidas são etiquetadas pelo LUIS. Não é possível adicionar ou remover a etiqueta de entidade pré-criados de uma única expressão. Apenas pode adicionar ou remover todas as etiquetas pré-criados adicionando ou removendo a entidade pré-criados do aplicativo.

Siga estes [passos](#add-hierarchical-entity-and-label) para criar um **localização** entidades hierárquicas. As localizações de origem e de destino a expressão de exemplo da etiqueta. 

Antes de moldar as entidades numa entidade composta, certifique-se todas as entidades subordinadas são realçadas em azul, o que significa que ter foram rotuladas na expressão.

1. Encapsular as entidades individuais numa composição, selecione a primeira entidade etiquetada a expressão para a entidade composta. Na expressão de exemplo, `book 2 tickets from Seattle to Cairo`, a primeira entidade é o número 2. É apresentada uma lista de lista pendente que mostra as opções para esta seleção.

    ![Captura de ecrã do número selecionado e opções de menu pendente realçadas](./media/luis-how-to-add-example-utterances/wrap-1.png)

2. Selecione **encapsular entidade composta** na lista pendente. 

    ![Captura de ecrã das opções de menu pendente para encapsulamento entidade composta com moldagem na entidade composta realçada](./media/luis-how-to-add-example-utterances/wrap-2.png)

3. Selecione a última palavra da entidade composta. Na ocorrência de pronunciação de neste exemplo, selecione "Location::Destination" (representando Cairo). A linha verde está agora em todas as palavras, incluindo palavras não entidade, na expressão que estão a composição.

    ![Página de captura de ecrã de BookFlight intenção, com o número de realçado](./media/luis-how-to-add-example-utterances/wrap-composite.png)

4. Selecione o nome da entidade composta na lista pendente. Neste exemplo, que é **TicketOrder**.

    ![Captura de ecrã do encapsulamento palavras com entidade composta, com o nome da entidade compostos realçado na lista pendente](./media/luis-how-to-add-example-utterances/wrap-4.png)

    Quando encapsula as entidades corretamente, uma linha verde está sob a frase inteira.

    ![Captura de ecrã da expressão com entidade composta realçada](./media/luis-how-to-add-example-utterances/wrap-5.png)

    Ver [extração de dados](luis-concept-data-extraction.md#composite-entity-data) para saber mais sobre a extrair a entidade composta do ponto de extremidade de resposta da consulta JSON. Experimente a entidade composta [tutorial](luis-tutorial-composite-entity.md) para saber mais sobre como utilizar uma entidade composta.

## <a name="add-hierarchical-entity-and-label"></a>Adicionar entidades hierárquicas e etiqueta
Uma entidade hierárquica é uma categoria de entidades contextualmente aprendidas e conceitualmente relacionadas. No exemplo seguinte, a entidade contém localizações de origem e de destino. 

Na expressão `Book 2 tickets from Seattle to Cairo`, Seattle é a localização de origem e Cairo é a localização de destino. Cada localização é diferente e aprendidas contextualmente de ordem das palavras e a escolha do word na expressão.

1. Na página de intenção, na expressão, selecione "Seattle", em seguida, introduza o nome da entidade "local e, em seguida, selecione **criar nova entidade**.

    ![Caixa de diálogo de captura de ecrã de criar hierárquica entidade etiquetagem](./media/luis-how-to-add-example-utterances/create-hier-ent-1.png)

2. Na caixa de diálogo pop-up, selecione hierárquica para **tipo de entidade**, em seguida, adicione `Origin` e `Destination` como filhos e, em seguida, selecione **feito**.

    ![Página de detalhes de captura de ecrã de objetivos, com a entidade de ToLocation realçada](./media/luis-how-to-add-example-utterances/create-location-hierarchical-entity.png)

3. A palavra a expressão foi identificada com a entidade de hierárquica do principal. Terá de atribuir a palavra a entidade subordinada. Voltar para a expressão na página de intenção. Selecione o word, em seguida, na lista pendente, escolha o nome da entidade que criou e siga o menu à direita para escolher a entidade de filho correta.

    ![Página de detalhes de captura de ecrã de objetivos, com a entidade de ToLocation realçada](./media/luis-how-to-add-example-utterances/label-tolocation.png)

    >[!CAUTION]
    >Os nomes de entidades de subordinado tem de ser exclusivos em todas as entidades numa única aplicação. Duas entidades hierárquicas diferentes não podem conter entidades subordinadas com o mesmo nome. 

    Ver [extração de dados](luis-concept-data-extraction.md#hierarchical-entity-data) para saber mais sobre a extrair a entidade hierárquica do ponto de extremidade de resposta da consulta JSON. Experimente a entidade hierárquica [guia de introdução](luis-quickstart-intent-and-hier-entity.md) para saber mais sobre como utilizar uma entidade hierárquica.


## <a name="remove-entity-labels-from-utterances"></a>Remover etiquetas de entidade de expressões
Pode remover as etiquetas de entidade aprendidas de máquina de uma expressão na página de intenção. Se a entidade não ficou a saber de máquina, não pode ser removido de uma expressão. Se precisar de remover a expressão a uma entidade não aprendidas máquina, terá de eliminar a entidade de todo o aplicativo. 

Para remover uma etiqueta de entidade aprendidas de máquina de uma expressão, selecione a entidade a expressão. Em seguida, selecione **remover etiqueta** na caixa de lista pendente de entidade é apresentada.

![Página de detalhes de captura de ecrã de objetivos, com remover etiqueta realçada](./media/luis-how-to-add-example-utterances/remove-label.png) 

## <a name="add-prebuilt-entity-label"></a>Adicionar etiqueta de entidade predefinidos
Se adicionar as entidades previamente concebidas para a sua aplicação LUIS, não precisa de expressões de etiqueta com estas entidades. Para saber mais sobre entidades previamente concebidas e como adicioná-las, veja [adicionar entidades](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="add-regular-expression-entity-label"></a>Adicionar etiqueta de entidade de expressão regular
Se adicionar as entidades de expressão regular para a sua aplicação LUIS, não precisa de expressões de etiqueta com estas entidades. Para saber mais sobre entidades de expressão regular e como adicioná-las, veja [adicionar entidades](luis-how-to-add-entities.md#add-regular-expression-entities).

## <a name="create-a-pattern-from-an-utterance"></a>Criar um padrão a partir de uma expressão
Ver [adicionar padrão de expressão existente na página de intenção ou a entidade](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="add-patternany-entity-label"></a>Adicionar etiqueta de entidade pattern.any
Se adicionar as entidades de pattern.any à sua aplicação LUIS, não é possível Etiquetar expressões com estas entidades. Apenas são válidas em padrões. Para saber mais sobre entidades pattern.any e como adicioná-las, veja [adicionar entidades](luis-how-to-add-entities.md#add-patternany-entities).

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
## <a name="train-your-app-after-changing-model-with-utterances"></a>Preparar a sua aplicação depois de alterar o modelo com expressões com
Depois de adicionar, editar ou remover expressões, [treinar](luis-how-to-train.md) e [publicar](luis-how-to-publish-app.md) as suas alterações afetar a consultas de ponto final na sua aplicação. 

## <a name="next-steps"></a>Passos Seguintes

Depois de etiquetagem discursos em suas intenções, agora, pode criar uma [entidade composta](luis-how-to-add-entities.md).
