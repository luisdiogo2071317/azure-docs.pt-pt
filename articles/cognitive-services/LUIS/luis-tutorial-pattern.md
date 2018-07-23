---
title: Tutorial sobre como utilizar padrões para melhorar as previsões de LUIS – Azure | Documentos da Microsoft
titleSuffix: Cognitive Services
description: Neste tutorial, utilize o padrão para objetivos para melhorar as previsões de intenção e entidade do LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 07/20/2018
ms.author: v-geberr;
ms.openlocfilehash: 5d486272f7f713c5d4e6f7b598073c5c09875d43
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/20/2018
ms.locfileid: "39172471"
---
# <a name="tutorial-improve-app-with-patterns"></a>Tutorial: Melhorar a aplicação com padrões

Neste tutorial, utilize padrões para aumentar a predição de intenção e entidade.  

> [!div class="checklist"]
* Como identificar que um padrão ajudaria a sua aplicação
* Como criar um padrão 
* Como verificar as melhorias de predição do padrão

Para este artigo, precisa de uma conta do [LUIS](luis-reference-regions.md) gratuita para criar a sua aplicação LUIS.

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de recursos humanos do [teste de lote](luis-tutorial-batch-testing.md) tutorial, [importar](luis-how-to-start-new-app.md#import-new-app) o JSON para uma nova aplicação no [LUIS](luis-reference-regions.md#luis-website) Web site. A aplicação para importar se encontra no [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-batchtest-HumanResources.json) repositório do GitHub.

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `patterns`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Padrões de ensinam LUIS expressões comuns com menos de exemplos
Devido à natureza do domínio de recursos humanos, existem algumas formas comuns de perguntar sobre relações de funcionário em organizações. Por exemplo:

```
Who does Jill Jones report to?
Who reports to Jill Jones? 
```

Estas expressões são demasiado fechar para determinar a exclusividade contextual de cada sem fornecer muitos exemplos de expressão. Ao adicionar um padrão para um objetivo, o LUIS aprende os padrões de expressão comuns para um objetivo sem fornecer muitos exemplos de expressão. 

Expressão de modelo de exemplo incluem intenção:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

O padrão é fornecido por meio de um exemplo de expressão de modelo, que inclui a sintaxe para identificar as entidades e ignorable texto. Um padrão é uma combinação de correspondência de expressões regulares e de machine learning.  O exemplo de expressão de modelo, juntamente com as expressões de intenção, dê uma melhor compreensão de quais expressões com ajustar a intenção de LUIS.

Para um padrão para corresponder a uma expressão, as entidades dentro da expressão tem de acordo com as entidades na expressão de modelo pela primeira vez. No entanto, o modelo não ajuda a prever as entidades, apenas os objetivos. 

**Embora os padrões permitem que forneça menos expressões de exemplo, se as entidades não são detetadas, o padrão não corresponde.**

Lembre-se de que os funcionários foram criados no [tutorial de entidade de lista](luis-quickstart-intent-and-list-entity.md).

## <a name="create-new-intents-and-their-utterances"></a>Criar novo intenções e suas expressões de com
Adicionar dois intenções novo: `OrgChart-Manager` e `OrgChart-Reports`. Uma vez o LUIS devolve uma predição para a aplicação de cliente, o nome de intenção pode ser utilizada como um nome de função na aplicação cliente e que a entidade Employee poderia ser usada como um parâmetro para essa função.

```
OrgChart-Manager(employee){
    ///
}
```

1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

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

    [![](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png "Captura de ecrã do LUIS adicionar expressões novo com a intenção")](media/luis-tutorial-pattern/hr-orgchart-manager-intent.png#lightbox)

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
A quantidade de expressões de exemplo nestes objetivos não é suficiente para preparar o LUIS corretamente. Num aplicativo do mundo real, cada intenção deve ter um mínimo de 15 expressões com uma variedade de comprimento de opção e a expressão do word. Estas expressões com alguns são selecionados especificamente para realçar os padrões. 

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
A intenção de novo e expressões com necessitam de treinamento. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Imagem do botão de preparação](./media/luis-tutorial-pattern/hr-train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Imagem da barra de notificação de êxito](./media/luis-tutorial-pattern/hr-trained-inline.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    [ ![Captura de ecrã de publicar página com a funcionalidade Publicar para botão de ranhura de produção realçado](./media/luis-tutorial-pattern/hr-publish-to-production.png)](./media/luis-tutorial-pattern/hr-publish-to-production.png#lightbox)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    [ ![Página de captura de ecrã de publicação de mensagens em fila com o URL de ponto final realçado](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png)](./media/luis-tutorial-pattern/hr-publish-select-endpoint.png#lightbox)


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

## <a name="add-the-template-utterances"></a>Adicionar as expressões de modelo

1. Selecione **criar** no menu superior.

2. No painel de navegação esquerdo, sob **melhorar o desempenho da aplicação**, selecione **padrões** no painel de navegação esquerda.

3. Selecione o **OrgChart Manager** intenção, em seguida, introduza as seguintes expressões de modelo, um de cada vez, selecione introduzir após cada ocorrência de pronunciação de modelo:

    |Expressão de modelo|
    |:--|
    |Quem é {funcionário} o subordinado [?]|
    |Quem faz {funcionário} reportar a [?]|
    |Quem é o Gestor de {funcionário} [da] [?]|
    |Quem faz {funcionário} relatório diretamente para [?]|
    |Quem é o supervisor de {funcionário} [da] [?]|
    |Quem é o chefe de {funcionário} [?]|

    O `{Employee}` sintaxe marca a localização de entidade dentro da expressão de modelo como bem como a entidade é. 
    
    Entidades com as funções utilizam sintaxe que inclui o nome de função e são abordados num [tutorial separada para funções](luis-tutorial-pattern-roles.md). 

    A sintaxe opcional, `[]`, marca palavras ou pontuação que são opcionais. LUIS corresponde à expressão, ignorando o texto opcional dentro dos parênteses Retos.

    Se digitar a expressão de modelo, a ajuda de LUIS preencher a entidade ao introduzir o parênteses Reto de abertura chave, `{`.

    [ ![Captura de ecrã da introdução de expressões de modelo de intenção](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png)](./media/luis-tutorial-pattern/hr-pattern-missing-entity.png#lightbox)



4. Selecione o **OrgChart relatórios** intenção, em seguida, introduza as seguintes expressões de modelo, um de cada vez, selecione introduzir após cada ocorrência de pronunciação de modelo:

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

2. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

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

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione as reticências (***...*** ) à direita do nome da aplicação na lista de aplicações, selecione **eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba como utilizar funções com um padrão](luis-tutorial-pattern-roles.md)