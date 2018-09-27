---
title: 'Tutorial 3: Padrões para melhorar as previsões de LUIS'
titleSuffix: Azure Cognitive Services
description: Utilize padrões para aumentar a predição de intenção e entidade ao mesmo tempo, menos expressões de exemplo. O padrão é fornecido por meio de um exemplo de expressão de modelo, que inclui a sintaxe para identificar as entidades e ignorable texto.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: f4b267dda3c05d490d91fe02fbcfde4e49674603
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166406"
---
# <a name="tutorial-3-add-common-utterance-formats"></a>Tutorial 3: Adicionar formatos comuns de expressão

Neste tutorial, utilize padrões para aumentar a predição de intenção e entidade ao mesmo tempo, menos expressões de exemplo. O padrão é fornecido por meio de um exemplo de expressão de modelo, que inclui a sintaxe para identificar as entidades e ignorable texto. Um padrão é uma combinação de correspondência de expressões e machine learning.  O exemplo de expressão de modelo, juntamente com as expressões de intenção, dê uma melhor compreensão de quais expressões com ajustar a intenção de LUIS. 

**Neste tutorial, ficará a saber como:**

> [!div class="checklist"]
> * Utilizar o tutorial de aplicação existente 
> * Criar intenção
> * Preparar
> * Publicar
> * Obter as intenções e entidades do ponto final
> * Criar um padrão
> * Certifique-se de melhorias de predição do padrão
> * Marcar texto como ignorable e aninhar dentro de padrão
> * Utilize o painel de teste para verificar o êxito do padrão

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente

Continuar com a aplicação criada no tutorial última, com o nome **RecursosHumanos**. 

Se não tiver a aplicação de RecursosHumanos no tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-batchtest-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Do **Manage** na secção a **versões** separador, clonar a versão e nomeie- `patterns`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter quaisquer carateres que não são válidos num URL.

## <a name="create-new-intents-and-their-utterances"></a>Criar novo intenções e suas expressões de com

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Na página **Intents** (Intenções), selecione **Create new intent** (Criar nova intenção). 

3. Introduza `OrgChart-Manager` na caixa de diálogo de pop-up e, em seguida, selecione **Concluído**.

    ![Criar nova janela de pop-up de mensagem](media/luis-tutorial-pattern/hr-create-new-intent-popup.png)

4. Adicione expressões de exemplo à intenção.

    |Expressões de exemplo|
    |--|
    |John W. Smith quem é o subordinado?|
    |Que John W. Smith reportar a?|
    |Quem é o Gestor de John W. Smith?|
    |Quem Jill Jones diretamente informa ao?|
    |Quem é o supervisor de Jill Jones?|

    [![Captura de ecrã do LUIS adicionar expressões novo com a intenção](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "captura de ecrã do LUIS adicionar expressões novo com a intenção")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

    Não se preocupe se a entidade de keyPhrase tem o nome nas expressões da intenção em vez da entidade employee. Ambos são previstas corretamente no painel de teste e no ponto final. 

5. Selecione **Intenções** no painel de navegação esquerdo.

6. Selecione **Create new intent** (Criar nova intenção). 

7. Introduza `OrgChart-Reports` na caixa de diálogo de pop-up e, em seguida, selecione **Concluído**.

8. Adicione expressões de exemplo à intenção.

    |Expressões de exemplo|
    |--|
    |Quem são os subordinados de John W. Smith?|
    |Que reporta a John W. Smith?|
    |Que faça a gestão de John W. Smith?|
    |Quem são os relatórios diretos de Jill Jones?|
    |Quem supervisionar Jill Jones?|

## <a name="caution-about-example-utterance-quantity"></a>Cuidado sobre a quantidade de expressão de exemplo

[!include[Too few examples](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obter a intenção e entidades do ponto final

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `Who is the boss of Jill Jones?`. O último parâmetro querystring é `q`, a expressão **query**. 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.353984952
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.353984952
            },
            {
                "intent": "OrgChart-Reports",
                "score": 0.214128986
            },
            {
                "intent": "EmployeeFeedback",
                "score": 0.08434003
            },
            {
                "intent": "MoveEmployee",
                "score": 0.019131
            },
            {
                "intent": "GetJobInformation",
                "score": 0.004819009
            },
            {
                "intent": "Utilities.Confirm",
                "score": 0.0043958663
            },
            {
                "intent": "Utilities.StartOver",
                "score": 0.00312064588
            },
            {
                "intent": "Utilities.Cancel",
                "score": 0.002265454
            },
            {
                "intent": "Utilities.Help",
                "score": 0.00133465114
            },
            {
                "intent": "None",
                "score": 0.0011388344
            },
            {
                "intent": "Utilities.Stop",
                "score": 0.00111166481
            },
            {
                "intent": "FindForm",
                "score": 0.0008900076
            },
            {
                "intent": "ApplyForJob",
                "score": 0.0007836131
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                }
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

Esta consulta foi bem-sucedida? Para este ciclo de treinamento foi bem-sucedida. As pontuações dos dois objetivos principais estão próximos. Uma vez que LUIS formação não é exatamente o mesmo cada vez, é um pouco da variação, nestas duas classificações foi possível inverter no próximo ciclo de treinamento. O resultado é que a intenção de errado pode ser retornada. 

Utilize padrões para tornar significativamente maior de pontuação a intenção correto, medida como percentagem e mais distantes a pontuação mais alta seguinte. 

Deixe esta segunda janela do browser aberta. Utilizá-lo novamente mais tarde no tutorial. 

## <a name="template-utterances"></a>Expressão de modelo
Devido à natureza do domínio de recursos humanos, existem algumas formas comuns de perguntar sobre relações de funcionário em organizações. Por exemplo:

|Expressões|
|--|
|Quem Jill Jones reportar a?|
|Que reporta a Jill Jones?|

Estas expressões são demasiado fechar para determinar a exclusividade contextual de cada sem fornecer muitos exemplos de expressão. Ao adicionar um padrão para um objetivo, o LUIS aprende os padrões de expressão comuns para um objetivo sem fornecer muitos exemplos de expressão. 

Exemplos de expressão de modelo intenção incluem:

|Exemplos de expressões com modelo|significado de sintaxe|
|--|--|
|Quem faz {funcionário} reportar a [?]|intercambiável {funcionário}, ignorar [?]}|
|Que reporta a {funcionário} [?]|intercambiável {funcionário}, ignorar [?]}|

O `{Employee}` sintaxe marca a localização de entidade dentro da expressão de modelo como bem como a entidade é. A sintaxe opcional, `[?]`, marca palavras ou pontuação que são opcionais. LUIS corresponde à expressão, ignorando o texto opcional dentro dos parênteses Retos.

Embora a sintaxe se parece com expressões regulares, não é expressões regulares. Apenas a chaveta, `{}`e o parêntesis Reto de fecho, `[]`, a sintaxe é suportada. Eles podem ser aninhados até dois níveis.

Para um padrão para corresponder a uma expressão, as entidades dentro da expressão tem de acordo com as entidades na expressão de modelo pela primeira vez. No entanto, o modelo não ajuda a prever as entidades, apenas os objetivos. 

**Embora os padrões permitem que forneça menos expressões de exemplo, se as entidades não são detetadas, o padrão não corresponde.**

Neste tutorial, adicione dois objetivos novo: `OrgChart-Manager` e `OrgChart-Reports`. 

|Intenção|Expressão|
|--|--|
|OrgChart-Manager|Quem Jill Jones reportar a?|
|Relatórios de OrgChart|Que reporta a Jill Jones?|

Uma vez o LUIS devolve uma predição para a aplicação de cliente, o nome de intenção pode ser utilizada como um nome de função na aplicação cliente e que a entidade Employee poderia ser usada como um parâmetro para essa função.

```Javascript
OrgChartManager(employee){
    ///
}
```

Lembre-se de que os funcionários foram criados no [tutorial de entidade de lista](luis-quickstart-intent-and-list-entity.md).

1. Selecione **criar** no menu superior.

2. No painel de navegação esquerdo, sob **melhorar o desempenho da aplicação**, selecione **padrões** no painel de navegação esquerda.

3. Selecione o **OrgChart Manager** intenção, em seguida, introduza as seguintes expressões de modelo:

    |Expressão de modelo|
    |:--|
    |Quem é {funcionário} o subordinado [?]|
    |Quem faz {funcionário} reportar a [?]|
    |Quem é o Gestor de {funcionário} [da] [?]|
    |Quem faz {funcionário} relatório diretamente para [?]|
    |Quem é o supervisor de {funcionário} [da] [?]|
    |Quem é o chefe de {funcionário} [?]|

    Entidades com as funções de utilizar a sintaxe que inclui o nome da função e são abordadas num [tutorial separada para funções](luis-tutorial-pattern-roles.md). 

    Se digitar a expressão de modelo, a ajuda de LUIS preencher a entidade ao introduzir o parênteses Reto de abertura chave, `{`.

    [![Captura de ecrã da introdução de expressões de modelo de intenção](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)

4. Selecione o **OrgChart relatórios** intenção, em seguida, introduza as seguintes expressões de modelo:

    |Expressão de modelo|
    |:--|
    |Quem são {funcionário} [da] subordinada [?]|
    |Que reporta a {funcionário} [?]|
    |Quem faz {funcionário} gerir [?]|
    |Quem são os relatórios diretos de {funcionário} [?]|
    |Quem faz {funcionário} supervisionar [?]|
    |Quem faz {funcionário} chefe [?]|

## <a name="query-endpoint-when-patterns-are-used"></a>Ponto final de consulta quando são utilizados padrões

1. Formar e publicar a aplicação novamente.

2. Mude os separadores de browser para o separador de URL de ponto final.

3. Vá para o final do URL o endereço e introduza `Who is the boss of Jill Jones?` como a expressão. O último parâmetro querystring é `q`, a expressão **query**. 

    ```JSON
    {
        "query": "who is the boss of jill jones?",
        "topScoringIntent": {
            "intent": "OrgChart-Manager",
            "score": 0.9999989
        },
        "intents": [
            {
                "intent": "OrgChart-Manager",
                "score": 0.9999989
            },
            {
                "intent": "OrgChart-Reports",
                "score": 7.616303E-05
            },
            {
                "intent": "EmployeeFeedback",
                "score": 7.84204349E-06
            },
            {
                "intent": "GetJobInformation",
                "score": 1.20674213E-06
            },
            {
                "intent": "MoveEmployee",
                "score": 7.91245157E-07
            },
            {
                "intent": "None",
                "score": 3.875E-09
            },
            {
                "intent": "Utilities.StartOver",
                "score": 1.49E-09
            },
            {
                "intent": "Utilities.Confirm",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Help",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Stop",
                "score": 1.34545453E-09
            },
            {
                "intent": "Utilities.Cancel",
                "score": 1.225E-09
            },
            {
                "intent": "FindForm",
                "score": 1.123077E-09
            },
            {
                "intent": "ApplyForJob",
                "score": 5.625E-10
            }
        ],
        "entities": [
            {
                "entity": "jill jones",
                "type": "Employee",
                "startIndex": 19,
                "endIndex": 28,
                "resolution": {
                    "values": [
                        "Employee-45612"
                    ]
                },
                "role": ""
            },
            {
                "entity": "boss of jill jones",
                "type": "builtin.keyPhrase",
                "startIndex": 11,
                "endIndex": 28
            }
        ]
    }
    ```

A predição de intenção agora é significativamente maior.

## <a name="working-with-optional-text-and-prebuilt-entities"></a>Trabalhar com o texto opcional e entidades pré-concebidas

As expressões de modelo padrão anterior neste tutorial tinham alguns exemplos de texto opcional, como usar a letra s, possessive `'s`e a utilização de ponto de interrogação, `?`. Suponha que as expressões de ponto final mostram que os gerentes e representantes de recursos humanos estiver à procura de dados históricos, bem como planejado movimentações de funcionários dentro da empresa a acontecer no futuro.

Expressões com de exemplo são:

|Intenção|Expressões com de exemplo com o texto opcional e entidades pré-concebidas|
|:--|:--|
|OrgChart-Manager|`Who was Jill Jones manager on March 3?`|
|OrgChart-Manager|`Who is Jill Jones manager now?`|
|OrgChart-Manager|`Who will be Jill Jones manager in a month?`|
|OrgChart-Manager|`Who will be Jill Jones manager on March 3?`|

Cada um desses exemplos utiliza um tempo verbal verbo, `was`, `is`, `will be`, bem como uma data `March 3`, `now`, e `in a month`, que precisa de LUIS prever corretamente. Tenha em atenção que os dois últimos exemplos usam quase o mesmo texto, exceto para `in` e `on`.

Expressões com modelo de exemplo:
|Intenção|Expressões com de exemplo com o texto opcional e entidades pré-concebidas|
|:--|:--|
|OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?`]|
|OrgChart-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
|OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

O uso da sintaxe de colchetes, opcional `[]`, facilita esse texto opcional adicionar a expressão de modelo e podem ser aninhados até um segundo nível, `[[]]`e incluem entidades ou texto.

**A pergunta é: Por que não foi possível as expressões de duas últimas exemplo combinar numa expressão de modelo único?** O modelo padrão não suporta sintaxe OR. Para poder capturar ambos os `in` versão e o `on` versão, cada um tem de ser uma expressão de modelo separado.

**A pergunta é: Por que motivo são todos o `w` letras, a primeira letra de cada expressão de modelo, em minúsculas? Não devem eles ser, opcionalmente, maiúsculas ou minúsculas?** A expressão submetido para o ponto final de consulta, pela aplicação de cliente, é convertido em minúsculas. A expressão de modelo pode estar em maiúsculas ou minúsculas e a expressão de ponto de extremidade também pode ser uma. A comparação sempre é feita após a conversão em minúsculas.

**A pergunta é: Por que não é o número de pré-criados parte do modelo de expressão se 3 de Março for previsto ambos como número `3` e a data `March 3`?** A expressão de modelo personalizada está a utilizar uma data, ou, literalmente, como em `March 3` ou abstrata como `in a month`. Uma data pode conter um número, mas um número necessariamente não pode ser visto como uma data. Utilize sempre a entidade que melhor representa o tipo que quer que sejam devolvidas nos resultados JSON de predição.  

**Pergunta: E o mal dizer expressões com como `Who will {Employee}['s] manager be on March 3?`.** Verbais sentenças gramaticalmente diferentes, como este, em que o `will` e `be` são separados têm de ser uma expressão de modelo novo. A expressão de modelo existente não corresponderão-lo. Embora a intenção da expressão não foi alterado, a colocação de palavras na expressão foi alterado. Esta alteração irá afetar a predição de LUIS.

**Lembre-se: entidades são encontradas em primeiro lugar, em seguida, o padrão é correspondido.**

## <a name="edit-the-existing-pattern-template-utterance"></a>Editar a expressão de modelo padrão existente

1. No site do LUIS, selecione **crie** no menu superior, em seguida, selecione **padrões** no menu à esquerda. 

2. Encontrar a expressão de modelo existente, `Who is {Employee}['s] manager[?]`e selecione as reticências (***...*** ) à direita. 

3. Selecione **editar** no menu de pop-up. 

4. Altere a expressão de modelo para: `who is {Employee}['s] manager [[on]{datetimeV2}?]]`

## <a name="add-new-pattern-template-utterances"></a>Adicionar novas expressões de modelo padrão

1. Enquanto estiver da **padrões** secção **criar**, adicionar várias novas padrão de expressão de modelo. Selecione **OrgChart Manager** no menu pendente intenção e introduza cada um a seguinte expressão de modelo:

    |Intenção|Expressões com de exemplo com o texto opcional e entidades pré-concebidas|
    |--|--|
    |OrgChart-Manager|`who was {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who is {Employee}['s] manager [[on]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[in]{datetimeV2}?]`|
    |OrgChart-Manager|`who will be {Employee}['s] manager [[on]{datetimeV2}?]`|

2. Prepare a aplicação.

3. Selecione **teste** na parte superior do painel para abrir o painel de teste. 

4. Introduza várias expressões de teste para verificar se o padrão é correspondido e a pontuação de intenção é significativamente maior. 

    Depois de introduzir a primeira expressão, selecione **Inspect** sob o resultado para que possa ver todos os resultados de predição.

    |Expressão|
    |--|
    |Quem será o Gestor de Jill Jones|
    |quem será o Gestor do jones jill|
    |Quem será o Gestor do Jill Jones?|
    |quem será o Gestor de jones Jill 3 de Março|
    |Quem será o Gestor de Jill Jones no próximo mês|
    |Quem será o Gestor de Jill Jones num mês?|

Encontrar todas essas expressões com as entidades dentro dela, portanto correspondem o mesmo padrão e ter uma pontuação alta de predição.

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos Seguintes

Este tutorial adiciona dois objetivos para expressões que eram difíceis de previsão com precisão elevada sem ter muitas expressões de exemplo. Padrões de adição para estes LUIS permitidos para melhor preveem a intenção com uma pontuação significativamente superior. Marcar entidades e texto ignorable permitido o LUIS aplicar o padrão para uma maior variedade de expressões.

> [!div class="nextstepaction"]
> [Saiba como utilizar funções com um padrão](luis-tutorial-pattern-roles.md)