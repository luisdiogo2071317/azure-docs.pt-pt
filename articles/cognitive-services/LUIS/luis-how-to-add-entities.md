---
title: Adicionar entidades nas aplicações de LUIS | Documentos da Microsoft
titleSuffix: Azure
description: Adicione entidades (dados de chave no domínio de seu aplicativo) em aplicações de compreensão de idiomas (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: e97f9a5391799849983bd98db5400e0a842627b7
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224131"
---
# <a name="manage-entities"></a>Gerir entidades
Depois de identificar seu aplicativo [intenções](luis-concept-intent.md), terá de [expressões de exemplo de etiqueta](luis-concept-utterance.md) com [entidades](luis-concept-entity-types.md). Entidades são partes importantes de um comando ou uma pergunta e podem ser essenciais para a sua aplicação de cliente executar a tarefa. 

Pode adicionar, editar ou eliminar entidades na sua aplicação através da **lista de entidades** no **entidades** página. LUIS oferece dois tipos principais de entidades: [entidades pré-concebidas](luis-reference-prebuilt-entities.md)e suas próprias entidades personalizadas.

As seções a seguir só estão disponíveis dentro de uma aplicação do LUIS, do **criar** secção. O **criar** a ligação está na barra de navegação superior. Uma vez por dentro do **crie** secção, selecione **entidades** no menu de navegação à esquerda. Depois de uma entidade é adicionada ao aplicativo, se a entidade é aprendidas por máquina, pode [Etiquetar a entidade](luis-how-to-add-example-utterances.md) dentro da expressão. Depois da aplicação é preparada e publicada, pode receber dados de entidade [extraídos](luis-concept-data-extraction.md) de predição. 

## <a name="add-prebuilt-entity"></a>Adicionar entidade pré-criados
Entidades pré-concebidas são definidas na [reconhecedores texto](https://github.com/Microsoft/Recognizers-Text) projeto. São entidades pré-concebidas comuns adicionadas a um aplicativo *número* e *datetimeV2*. 

1. Na sua aplicação, do **crie** secção e, em seguida, clique em **entidades** no painel esquerdo.
 
2. Sobre o **entidades** página, selecione **gerir entidades pré-concebidas**.

    ![Captura de ecrã da adição de entidade pré-criados na página de entidades](./media/add-entities/manage-prebuilt-entities-button.png)

3. Na **adicionar ou remover entidades pré-concebidas** caixa de diálogo, selecione a **número** e **datetimeV2** entidades pré-concebidas. Em seguida, selecione **feito**.

    ![Captura de ecrã de adicionar caixa de diálogo de entidade predefinidos](./media/add-entities/list-of-prebuilt-entities.png)

    Ver [extração de dados](luis-concept-data-extraction.md#prebuilt-entity-data) para saber mais sobre a extrair a entidade pré-criados do ponto de extremidade de resposta da consulta JSON.

## <a name="add-simple-entities"></a>Adicionar entidades simples
Uma entidade é uma entidade genérica que descreve um único conceito. 

1. Na sua aplicação, do **crie** secção e, em seguida, clique em **entidades** no painel esquerdo e, em seguida, selecione **criar nova entidade**.

    ![Página de captura de ecrã de entidades com o botão Criar nova entidade realçado](./media/add-entities/create-new-entity-button.png)

2. Na caixa de diálogo pop-up, escreva `Airline` no **nome da entidade** caixa, selecione **simples** partir a **tipo de entidade** lista e, em seguida, selecione **feito**.

    ![Captura de ecrã da caixa de diálogo para criar a entidade de companhia aérea simple](./media/add-entities/create-simple-airline-entity.png)

    Ver [extração de dados](luis-concept-data-extraction.md#simple-entity-data) para saber mais sobre a entidade simple a extração do ponto de extremidade de resposta da consulta JSON. Experimente a entidade simple [guia de introdução](luis-quickstart-primary-and-secondary-data.md) para saber mais sobre como utilizar uma entidade.

## <a name="add-regular-expression-entities"></a>Adicionar entidades de expressão regular
Uma entidade de expressão regular é utilizada para extrair dados de expressão com base numa expressão regular que fornecer. 

1. Na sua aplicação, selecione **entidades** na navegação à esquerda e, em seguida, selecione **criar nova entidade**.

2. No pop-up caixa de diálogo, escreva `AirFrance Flight` no **nome da entidade** caixa, selecione **expressão Regular** partir a **tipo de entidade** lista, introduza a expressão regular `AFR[0-9]{3,4}`e, em seguida, selecione **feito**. 

    Essa expressão regular de voo AirFrance espera três caracteres, literalmente `AFR`, em seguida, 3 ou 4 dígitos. Os dígitos podem ser qualquer número entre 0 e 9. A expressão regular que corresponde a números de voo AirFrance, tais como: "AFR101", "ARF1302" e "AFR5006". Ver [extração de dados](luis-concept-data-extraction.md) para saber mais sobre a entidade a extração do ponto de extremidade de resposta da consulta JSON.

    ![Imagem da caixa de diálogo para criar a entidade de expressão regular](./media/add-entities/regex-entity-create-dialog.png)

    Ver [extração de dados](luis-concept-data-extraction.md#regular-expression-entity-data) para saber mais sobre a entidade de expressão regular a extração do ponto de extremidade de resposta da consulta JSON. Experimente a entidade de expressão regular [guia de introdução](luis-quickstart-intents-regex-entity.md) para saber mais sobre como utilizar uma entidade de expressão regular.

## <a name="add-hierarchical-entities"></a>Adicionar entidades hierárquicas
Uma entidade hierárquica é uma categoria de entidades contextualmente aprendidas e conceitualmente relacionadas. No exemplo seguinte, a entidade contém localizações de origem e de destino. 

Na expressão `Book 2 tickets from Seattle to Cairo`, Seattle é a localização de origem e Cairo é a localização de destino. Cada localização é diferente e aprendidas contextualmente de ordem das palavras e a escolha do word na expressão.

Para adicionar entidades hierárquicas, conclua os seguintes passos: 

1. Na sua aplicação, selecione **entidades** na navegação à esquerda e, em seguida, selecione **criar nova entidade**.

2. Na caixa de diálogo pop-up, escreva `Location` no **nome da entidade** caixa e, em seguida, selecione **Hierarchical** partir a **tipo de entidade** lista.

    ![Adicionar entidade hierárquica](./media/add-entities/hier-location-entity-creation.png)

3. Selecione **adicionar secundário**, e, em seguida, escreva "Origem" no **subordinados n. º 1** caixa. 

4. Selecione **adicionar secundário**, e, em seguida, escreva "Destino" **subordinados n. º 2** caixa. Selecione **Done** (Concluído).
5. 
    >[!NOTE]
    >Para eliminar um filho, selecione o ícone de caixote do lixo junto ao mesmo.

    >[!CAUTION]
    >Os nomes de entidades de subordinado tem de ser exclusivos em todas as entidades numa única aplicação. Duas entidades hierárquicas diferentes não podem conter entidades subordinadas com o mesmo nome. 

    Ver [extração de dados](luis-concept-data-extraction.md#hierarchical-entity-data) para saber mais sobre a extrair a entidade hierárquica do ponto de extremidade de resposta da consulta JSON. Experimente a entidade hierárquica [guia de introdução](luis-quickstart-intent-and-hier-entity.md) para saber mais sobre como utilizar uma entidade hierárquica.

## <a name="add-composite-entities"></a>Adicionar entidades compostas
Pode definir relações entre várias entidades existentes através da criação de uma entidade composta. No exemplo seguinte, a entidade contém a contagem de pedidos de suporte, a origem e localizações de destino. 

Na expressão `Book 2 tickets from Seattle to Cairo`, o número 2 é comparado a uma entidade pré-criados, Seattle é a localização de origem e Cairo é a localização de destino. Cada entidade faz parte de uma entidade principal maior, depois da entidade composta é criada.

1. Na sua aplicação, adicione a entidade pré-criados **número**. Para obter instruções, consulte [adicionar entidades pré-concebidas](#add-prebuilt-entity). 

2. Adicionar a entidade hierárquica `Location`, incluindo os subtipos: `origin`, `destination`. Para obter mais instruções, consulte [adicionar entidades hierárquicas](#add-hierarchical-entities). 

3. Selecione **entidades** na navegação à esquerda e, em seguida, selecione **criar nova entidade**.

4. Na caixa de diálogo pop-up, escreva `TicketsOrder` no **nome da entidade** caixa e, em seguida, selecione **compostos** partir a **tipo de entidade** lista.

5. Selecione **adicionar secundário** para adicionar um novo item subordinado.

6. Na **filho n. º 1**, selecione a entidade **número** da lista.

7. Na **filho n. º 2**, selecione a entidade **Location::Origin** da lista. 

8. Na **3 subordinado**, selecione a entidade **Location::Destination** da lista. 

9. Selecione **Done** (Concluído).

    ![Imagem da caixa de diálogo para criar uma entidade composta](./media/add-entities/ticketsorder-composite-entity.png)

    >[!NOTE]
    >Para eliminar um filho, selecione o botão de caixote do lixo junto ao mesmo.

    Ver [extração de dados](luis-concept-data-extraction.md#composite-entity-data) para saber mais sobre a extrair a entidade composta do ponto de extremidade de resposta da consulta JSON. Experimente a entidade composta [tutorial](luis-tutorial-composite-entity.md) para saber mais sobre como utilizar uma entidade composta.


## <a name="add-patternany-entities"></a>Adicionar entidades de Pattern.any
[Pattern.ANY](luis-concept-entity-types.md) apenas são válidas em entidades [padrões](luis-how-to-model-intent-pattern.md). Esta entidade ajuda LUIS encontrar o fim de entidades de comprimento variável e a escolha do word. Como esta entidade é utilizada num padrão, o LUIS sabe em que o fim da entidade é na expressão.

Se um aplicativo tiver um `FindBookInfo` intenção, o título do livro pode interferir com a intenção de predição. Para esclarecer que palavras são no título do livro, use um Pattern.any dentro de um padrão. A predição de LUIS começa com a expressão. Em primeiro lugar, a expressão é verificada e correspondentes para entidades, quando as entidades são encontradas, em seguida, o padrão é verificado e correspondentes. 

Na expressão `Who wrote the book Ask and when was it published?`, o título do livro, peça ao, é difícil, porque não é óbvio contextualmente onde termina o título e onde começa o restante da expressão. Títulos de livros podem ser qualquer ordem das palavras incluindo uma única palavra, expressões complexas com pontuação e não fizer sentido a ordem das palavras. Um padrão permite-lhe criar uma entidade em que a entidade completa e exata pode ser extraída. Assim que o título do livro é encontrado, o `FindBookInfo` intenção é prevista uma vez que é a intenção para o padrão.

1. Na sua aplicação, do **crie** secção e, em seguida, clique em **entidades** no painel esquerdo e, em seguida, selecione **criar nova entidade**.

2. Na **adicionar entidade** caixa de diálogo, escreva `BookTitle` no **nome da entidade** caixa e selecione **Pattern.any** como o **tipo de entidade**.
 
    ![Captura de ecrã da criação de uma entidade de pattern.any](./media/add-entities/create-pattern-any-entity.png)

    Para utilizar a entidade de pattern.any, adicione um padrão no **padrões** página (sob a **melhorar o desempenho de aplicações** seção) com a sintaxe de chave de abertura correto, como "para **{BookTitle}** quem é o autor? ".

    Ver [extração de dados](luis-concept-data-extraction.md#patternany-entity-data) para saber mais sobre a entidade de Pattern.any a extração do ponto de extremidade de resposta da consulta JSON. Experimente o [padrão](luis-tutorial-pattern.md) tutorial para saber mais sobre como utilizar uma entidade de Pattern.any.

Se achar que seu padrão, quando ele inclui um Pattern.any, incorretamente, utilizam entidades extrai uma [lista explícita](luis-concept-patterns.md#explicit-lists) para corrigir este problema. 

## <a name="add-role-to-pattern-based-entity"></a>Adicionar função a entidade com base no padrão
Uma função é um subtipo com nome de uma entidade com base no contexto. É comparável para um [hierárquica](#add-hierarchical-entities) entidade, funções, mas só são utilizadas na [padrões](luis-how-to-model-intent-pattern.md). 

Por exemplo, um pedido de suporte do plano tem um *cidade de origem* e uma *cidade de destino*, mas ambos são cidades. LUIS determina que ambos são cidades e podem determinar a origem e destino cidades com base no contexto de ordem das palavras e a escolha do word. 

A sintaxe para uma função é **{nome da função: nome de entidade}** onde o nome da entidade é seguido por dois-pontos, em seguida, o nome da função. Por exemplo, "livro uma permissão de {localização: Origin} para {localização: destino}".

1. Na sua aplicação, do **crie** secção e, em seguida, selecione **entidades** no painel esquerdo.

2. Selecione **Criar nova entidade**. Introduza o nome do `Location`. Selecione o tipo **simples** e selecione **concluído**

3. Selecione **entidades** do painel esquerdo, em seguida, selecione a nova entidade **localização** criada no passo 2.

4. Na **nome da função** caixa de texto, introduza o nome da função de `Origin` e introduza. Adicionar um segundo nome da função de `Destination`. Por exemplo, uma viagem de avião pode ter uma origem e de uma cidade de destino. As duas funções são "Origem" e "Destino".

    ![Captura de ecrã de adicionar a função de origem para a entidade de localização](./media/add-entities/roles-enter-role-name-text.png)

    Ver [extração de dados](luis-concept-data-extraction.md) para saber mais sobre a extração de funções do ponto de extremidade de resposta da consulta JSON. Experimente o tutorial de padrão para obter mais informações sobre como utilizar uma entidade de Pattern.any.

## <a name="add-list-entities"></a>Adicionar entidades de lista
Lista de entidades representam um conjunto de fixo, fechado de palavras relacionadas no seu sistema. 

Para uma entidade de lista drinks, pode ter dois valores normalizados: água e soda pop. Cada nome normalizado tem sinónimos. Para água, os sinónimos são H20, gás, fixa. Para soda pop, os sinónimos são frutos, cola, ginger. Não precisa saber todos os valores quando criar a entidade. Pode adicionar mais depois de rever as expressões de utilizador real com sinónimos.

|Nome normalizado|Sinónimos|
|--|--|
|Água|H20, gás, fixa|
|Pop da soda|Frutos, cola, ginger|

1. Na sua aplicação, do **crie** secção e, em seguida, clique em **entidades** no painel esquerdo e, em seguida, selecione **criar nova entidade**.

2. Na **adicionar entidade** caixa de diálogo, escreva `Drinks` no **nome da entidade** caixa e selecione **lista** como o **tipo de entidade**. Selecione **Done** (Concluído).
 
    ![Imagem da caixa de diálogo Criar Drinks lista entidade](./media/add-entities/menu-list-dialog.png)
  
3.  A página de lista de entidades permite-lhe adicionar nomes normalizados. Na **valores** caixa de texto, introduza um item de lista, tal como `Water` para os drinks lista, em seguida, prima Enter no teclado. 

    ![Entidade de lista de captura de ecrã de Drinks com água normalizados valor na caixa de texto](./media/add-entities/entity-list-normalized-name.png)

4. À direita do valor normalizado **água**, introduza sinónimos `h20`, `flat`, e `gas`, premindo Enter no teclado após cada item.

    ![Entidade de lista de captura de ecrã de Drinks com itens de sinónimos para água realçado](./media/add-entities/menu-list-synonyms.png)

5. Se quiser que mais normalizados itens de lista, selecione **Recomendamos** para ver as opções da [dicionário semântico](luis-glossary.md#semantic-dictionary).

    ![Captura de ecrã de tomar a entidade de lista com itens recomendados realçado](./media/add-entities/entity-list-recommended-list.png)

6. Selecione um item da lista de recomendados para adicioná-la como um valor normalizado ou selecione **adicionar todos** para adicionar todos os itens. 

    ![Captura de ecrã de beber lista entidade com o item recomendada de cerveja e adicionar todos os botão realçado](./media/add-entities/list-entity-add-suggestions.png)

    Ver [extração de dados](luis-concept-data-extraction.md#list-entity-data) para saber mais sobre a extração de entidades de lista do ponto de extremidade de resposta da consulta JSON. Experimente o [guia de introdução](luis-quickstart-intent-and-list-entity.md) para saber mais sobre como utilizar uma entidade de lista.

## <a name="import-list-entity-values"></a>Valores de entidade de lista de importação
Pode importar valores para uma entidade de lista existente.

 1. Na página de entidade de lista, selecione **apresenta uma lista de importação**.

 2. Na **importar novas entradas** caixa de diálogo, selecione **Escolher ficheiro** e selecione o ficheiro JSON que inclui a lista.

    ![Caixa de diálogo pop-up de valores de entidade de lista de captura de ecrã de importação](./media/add-entities/menu-list-import-json-dialog-with-file.png)

    >[!NOTE]
    >LUIS importa os ficheiros com a extensão ". JSON" apenas.

 3. Para saber mais sobre a sintaxe de lista suportada em JSON, selecione **Saiba mais sobre a sintaxe de lista suportada** para expandir a caixa de diálogo e exibir um exemplo da sintaxe permitido. Para fechar a caixa de diálogo e ocultar a sintaxe, selecione o título de ligação novamente.

 4. Selecione **Done** (Concluído).

    Um exemplo de um json válido para um **cores** entidade de lista é mostrada no seguinte código de formato JSON:

    ```
    [
        {
            "canonicalForm": "Blue",
            "list": [
                "navy",
                "royal",
                "baby"
            ]
        },
        {
            "canonicalForm": "Green",
            "list": [
                "kelly",
                "forest",
                "avacado"
            ]
        }
    ]  
    ```

## <a name="edit-entity-name"></a>Editar nome da entidade
1. Sobre o **entidades** página, selecione a entidade na lista. Esta ação leva-o para o **entidade** página.

2. Sobre o **entidade** página, edita o nome da entidade, selecionando o ícone de edição junto ao nome da entidade. O tipo de entidade não é editável. 

## <a name="delete-entity"></a>Eliminar entidade

Sobre o **entidade** página, selecione a **eliminar entidade** botão. Em seguida, selecione **Ok** na mensagem de confirmação para confirmar a eliminação.
 
![Página de entidade de captura de ecrã de localização com o botão de eliminar entidade realçado](./media/add-entities/entity-delete.png)

>[!NOTE]
>* A eliminar uma entidade hierárquica elimina todas as entidades de seus filhos.
>* A eliminar uma entidade composta elimina apenas a composição e quebra a relação composta, mas não elimina as entidades que formam ele.

## <a name="changing-entity-type"></a>Alterar o tipo de entidade
LUIS não permite-lhe alterar o tipo da entidade, porque ele não sabe o que adicionar ou remover para construir essa entidade. Para alterar o tipo, é melhor criar uma nova entidade do tipo correto com um nome de um pouco diferente. Assim que a entidade é criada, em cada ocorrência de pronunciação, remova o nome da entidade com nome antigo e adicionar o novo nome de entidade. Depois de tem sido relabeled todas as expressões, elimine a entidade antiga. 

## <a name="create-a-pattern-from-an-utterance"></a>Criar um padrão a partir de uma expressão
Ver [adicionar padrão de expressão existente na página de intenção ou a entidade](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="search-utterances"></a>Expressões de pesquisa
Pode pesquisar e filtrar expressões com o ícone de lupa na barra de ferramentas. 

## <a name="train-your-app-after-changing-model-with-entities"></a>Preparar a sua aplicação depois de alterar o modelo de entidades
Depois de adicionar, editar ou remover entidades, [treinar](luis-how-to-train.md) e [publicar](luis-how-to-publish-app.md) as suas alterações afetar a consultas de ponto final na sua aplicação. 

## <a name="next-steps"></a>Passos Seguintes
Agora que adicionou intenções, expressões e entidades, terá uma aplicação básica do LUIS. Saiba como [treinar](luis-how-to-train.md), [testar](luis-interactive-test.md), e [publicar](luis-how-to-publish-app.md) seu aplicativo.
 
