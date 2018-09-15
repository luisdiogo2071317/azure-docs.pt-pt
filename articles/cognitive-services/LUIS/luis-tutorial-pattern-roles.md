---
title: 'Tutorial 4: Funções padrão para o contexto de dados relacionados'
titleSuffix: Azure Cognitive Services
description: Utilize um padrão para extrair dados de uma expressão de modelo bem formatado. A expressão de modelo utiliza uma entidade e as funções para extrair dados relacionados, tais como a localização de origem e a localização de destino.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: f3ddbad350ed42823ca95136ae2a507c46c3c763
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45634545"
---
# <a name="tutorial-4-extract-contextually-related-patterns"></a>Tutorial: 4. Extrair contextualmente relacionados com padrões

Neste tutorial, utilize um padrão para extrair dados de uma expressão de modelo bem formatado. A expressão de modelo utiliza uma entidade e as funções para extrair dados relacionados, tais como a localização de origem e a localização de destino.  Ao usar padrões, expressões de exemplo menos são necessários para a intenção.

A finalidade das funções é extrair relacionados com as entidades numa expressão. Na expressão, `Move new employee Robert Williams from Sacramento and San Francisco`, a cidade de origem e destino cidade valores estão relacionados entre si e utilizam uma linguagem comum para indicar cada localização. 


O nome do novo funcionário, Billy Patterson, não faz parte da entidade lista **funcionário** ainda. O novo nome do funcionário é extraído em primeiro lugar, para enviar o nome para um sistema externo para criar as credenciais da empresa. Depois das credenciais da empresa são criadas, as credenciais de funcionário são adicionadas para a entidade de lista **funcionário**.

O novo funcionário e a família tem de ser movido da cidade de atual para uma cidade onde está localizada a empresa fictícia. Uma vez que um novo funcionário pode vir de qualquer cidade, as localizações tem de ser detetados. Uma lista de conjunto, como uma entidade de lista não funciona porque apenas as cidades na lista seriam extraídas.

Os nomes de função associados com as cidades de origem e de destino tem de ser exclusivo em todas as entidades. É uma forma fácil para se certificar de que as funções são exclusivas para associá-las para a entidade contentora por meio de uma estratégia de nomenclatura. O **NewEmployeeRelocation** entidade é uma entidade com duas funções: **NewEmployeeReloOrigin** e **NewEmployeeReloDestination**. Relo é a abreviação de reposicionamento.

Uma vez que a expressão de exemplo `Move new employee Robert Williams from Sacramento and San Francisco` tem apenas entidades aprendidas por máquina, é importante fornecer suficiente expressões de exemplo para a intenção, para que as entidades são detetadas.  

**Embora os padrões permitem que forneça menos expressões de exemplo, se as entidades não são detetadas, o padrão não corresponde.**

Se tiver dificuldade para com a detecção de entidade simples porque é um nome como uma cidade, considere adicionar uma lista de frase de valores similares. Isto ajuda a detetar o nome de cidade fornecendo o LUIS um sinal adicional sobre esse tipo de palavra ou frase. Listas de frase ajudam apenas o padrão, ajudando com deteção de entidade, o que é necessária para o padrão para corresponder. 

**Neste tutorial, ficará a saber como:**

> [!div class="checklist"]
> * Utilizar o tutorial de aplicação existente
> * Criar novas entidades
> * Criar intenção de novo
> * Preparar
> * Publicar
> * Obter as intenções e entidades do ponto final
> * Criar padrão com as funções
> * Criar expressão de lista de cidades
> * Obter as intenções e entidades do ponto final

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente
Continuar com a aplicação criada no tutorial última, com o nome **RecursosHumanos**. 

Se não tiver a aplicação de RecursosHumanos no tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-patterns-HumanResources-v2.json).

2. Importe o JSON para uma nova aplicação.

3. Do **Manage** na secção a **versões** separador, clonar a versão e nomeie- `roles`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter quaisquer carateres que não são válidos num URL.

## <a name="create-new-entities"></a>Criar novas entidades

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecione **entidades** no painel de navegação esquerda. 

3. Selecione **Criar nova entidade**.

4. Na janela de pop-up, introduza `NewEmployee` como uma **simples** entidade.

5. Selecione **Criar nova entidade**.

6. Na janela de pop-up, introduza `NewEmployeeRelocation` como uma **simples** entidade.

7. Selecione **NewEmployeeRelocation** na lista de entidades. 

8. Introduza a primeira função como `NewEmployeeReloOrigin` e selecione introduzir.

9. Introduza a segunda função como `NewEmployeeReloDestination` e selecione introduzir.

## <a name="create-new-intent"></a>Criar intenção de novo
Etiquetagem as entidades nestes passos pode ser mais fácil de se a entidade de keyPhrase pré-criados é removida antes de começar, em seguida, adicionado novamente depois de terminar com os passos nesta secção. 

1. Selecione **intenções** no painel de navegação esquerda.

2. Selecione **Create new intent** (Criar nova intenção). 

3. Introduza `NewEmployeeRelocationProcess` como o nome da intenção na caixa de diálogo pop-up.

4. Introduza as expressões de exemplo seguinte, a etiquetagem as novas entidades. Os valores de entidade e a função estão em negrito. Lembre-se mudar para o **vista de Tokens** se encontrá-lo mais fácil do texto da etiqueta. 

    Não especificar a função da entidade ao etiquetagem na intenção. Fazer isso mais tarde quando criar o padrão. 

    |Expressão|NewEmployee|NewEmployeeRelocation|
    |--|--|--|
    |Mover **Bob Jones** partir **Seattle** para **Colinas de Los**|BOB Jones|Seattle, Colinas de Los|
    |Mover **Dave C. Cooper** partir **Redmond** para **cidade de nova York**|Dave Cooper de C.|Cidade de Redmond, Nova Iorque|
    |Mover **Jim Paul Smith** partir **Toronto** para **Vancouver oeste**|Jim Paul Smith|Toronto, Vancouver oeste|
    |Mover **J. Benson** partir **Boston** para **Staines após Thames**|J. Benson|Boston, Staines após Thames|
    |Mover **Travis "Trav" Hinton** partir **Castelo Branco** para **Orlando**|Travis "Trav" Hinton|Castelo Branco, Orlando|
    |Mover **Trevor Nottington III** partir **Aranda Alemanha Duero** para **Boise**|Trevor Nottington III|Aranda Alemanha Duero, Boise|
    |Mover **Dr. Greg Williams** partir **Orlando** para **Ellicott cidade**|Dr. Greg Williams|Orlando, Ellicott cidade|
    |Mover **Robert "Bobby" Gregson** partir **Kansas Cidade** para **San Juan Capistrano**|Robert "Bobby" Gregson|Cidade de Kansas, San Juan Capistrano|
    |Mover **Patti Owens** partir **Bellevue** para **Rockford**|Patti Owens|Bellevue, Rockford|
    |Mover **Bartlet Júlia** partir **Tuscan** para **Santa fé**|Janet Bartlet|Tuscan, Santa fé|

    O nome do funcionário tem uma variedade de prefixo, contagem de palavras, sintaxe e o sufixo. Isso é importante para o LUIS compreender as variações de um novo nome do funcionário. Os nomes de cidade também tem uma variedade de contagem de palavras e sintaxe. Essa variedade é importante para ensinar LUIS como estas entidades podem aparecer numa expressão de um utilizador. 
    
    Se uma entidade tivesse sido da mesma contagem de palavras e não existem outras variações, seria ensinar LUIS que esta entidade tem apenas essa contagem de palavras e não existem outras variações. LUIS não seria capaz de prever corretamente um conjunto mais amplo de variações porque não foi apresentado qualquer. 

    Se removeu a entidade de keyPhrase, adicioná-lo novamente para a aplicação agora.

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obter a intenção e entidades do ponto final

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Vá para o final do URL no endereço e introduza `Move Wayne Berry from Miami to Mount Vernon`. O último parâmetro querystring é `q`, a expressão **query**. 

    ```JSON
    {
      "query": "Move Wayne Berry from Newark to Columbus",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.514479756
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.514479756
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.017118983
        },
        {
          "intent": "MoveEmployee",
          "score": 0.009982505
        },
        {
          "intent": "GetJobInformation",
          "score": 0.008637771
        },
        {
          "intent": "ApplyForJob",
          "score": 0.007115978
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.006120186
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00452428637
        },
        {
          "intent": "None",
          "score": 0.00400899537
        },
        {
          "intent": "OrgChart-Reports",
          "score": 0.00240071164
        },
        {
          "intent": "Utilities.Help",
          "score": 0.001770991
        },
        {
          "intent": "EmployeeFeedback",
          "score": 0.001697356
        },
        {
          "intent": "OrgChart-Manager",
          "score": 0.00168116146
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00163952739
        },
        {
          "intent": "FindForm",
          "score": 0.00112958835
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.629158735
        },
        {
          "entity": "newark",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 27,
          "score": 0.638941
        }
      ]
    }  
    ```

A pontuação de predição de intenção é apenas cerca de 50%. Se a aplicação de cliente necessita de um número mais elevado, isso precisa ser corrigido. As entidades não eram previstas qualquer um.

Uma das localizações foi extraída, mas não era a outra localização. 

Padrões ajudará a pontuação de predição, no entanto, as entidades têm de ser previstas corretamente antes do padrão corresponde a expressão. 

## <a name="pattern-with-roles"></a>Padrão de funções

1. Selecione **criar** no painel de navegação superior.

2. Selecione **padrões** na navegação à esquerda.

3. Selecione **NewEmployeeRelocationProcess** partir do **selecione um objetivo** na lista pendente. 

4. Introduza o seguinte padrão: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Se preparar, publicar e consultar o ponto final, poderá estar Desapontado ver que as entidades não foram encontradas, para que o padrão não corresponder ao, por isso também não melhorou a predição. Esta é uma conseqüência do insuficiente expressões de exemplo com entidades etiquetadas. Em vez de adicionar mais exemplos, adicione uma lista de frase para corrigir este problema.

## <a name="cities-phrase-list"></a>Lista de frase de cidades
Cidades, como nomes de pessoas são complicadas que podem ser qualquer combinação de palavras e pontuação. As cidades da região e do mundo são conhecidas, por isso, LUIS tem uma lista de frase de cidades para começar a aprender. 

1. Selecione **lista de frase** partir o **melhorar o desempenho da aplicação** secção do menu à esquerda. 

2. Nomeie a lista `Cities` e adicione o seguinte `values` para obter a lista:

    |Valores da lista de frase|
    |--|
    |Seattle|
    |San Diego|
    |Cidade de nova York|
    |Los Angeles|
    |Portland|
    |Philadephia|
    |Miami|
    |Dallas|

    Não adicione todas as cidades no mundo ou até mesmo todas as cidades na região. LUIS tem de ser capaz de generalizar que uma localidade é a partir da lista. Certifique-se de manter **estes valores são intercambiáveis** selecionado. Esta definição significa que as palavras na lista no tratados como sinónimos. 

3. Formar e publicar a aplicação.

## <a name="get-intent-and-entities-from-endpoint"></a>Obter a intenção e entidades do ponto final

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Vá para o final do URL no endereço e introduza `Move wayne berry from miami to mount vernon`. O último parâmetro querystring é `q`, a expressão **query**. 

    ```JSON
    {
      "query": "Move Wayne Berry from Miami to Mount Vernon",
      "topScoringIntent": {
        "intent": "NewEmployeeRelocationProcess",
        "score": 0.9999999
      },
      "intents": [
        {
          "intent": "NewEmployeeRelocationProcess",
          "score": 0.9999999
        },
        {
          "intent": "Utilities.Confirm",
          "score": 1.49678385E-06
        },
        {
          "intent": "MoveEmployee",
          "score": 8.240291E-07
        },
        {
          "intent": "GetJobInformation",
          "score": 6.3131273E-07
        },
        {
          "intent": "None",
          "score": 4.25E-09
        },
        {
          "intent": "OrgChart-Manager",
          "score": 2.8E-09
        },
        {
          "intent": "OrgChart-Reports",
          "score": 2.8E-09
        },
        {
          "intent": "EmployeeFeedback",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.StartOver",
          "score": 1.64E-09
        },
        {
          "intent": "Utilities.Help",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Stop",
          "score": 1.48181822E-09
        },
        {
          "intent": "Utilities.Cancel",
          "score": 1.35E-09
        },
        {
          "intent": "FindForm",
          "score": 1.23846156E-09
        },
        {
          "intent": "ApplyForJob",
          "score": 5.692308E-10
        }
      ],
      "entities": [
        {
          "entity": "wayne berry",
          "type": "builtin.keyPhrase",
          "startIndex": 5,
          "endIndex": 15
        },
        {
          "entity": "miami",
          "type": "builtin.keyPhrase",
          "startIndex": 22,
          "endIndex": 26
        },
        {
          "entity": "wayne berry",
          "type": "NewEmployee",
          "startIndex": 5,
          "endIndex": 15,
          "score": 0.9410646,
          "role": ""
        },
        {
          "entity": "miami",
          "type": "NewEmployeeRelocation",
          "startIndex": 22,
          "endIndex": 26,
          "score": 0.9853915,
          "role": "NewEmployeeReloOrigin"
        },
        {
          "entity": "mount vernon",
          "type": "NewEmployeeRelocation",
          "startIndex": 31,
          "endIndex": 42,
          "score": 0.986044347,
          "role": "NewEmployeeReloDestination"
        }
      ],
      "sentimentAnalysis": {
        "label": "neutral",
        "score": 0.5
      }
}
    ```

A pontuação de intenção agora é muito superior e os nomes de função fazem parte da resposta de entidade.

## <a name="hierarchical-entities-versus-roles"></a>Entidades hierárquicas em comparação com as funções

Na [tutorial hierárquica](luis-quickstart-intent-and-hier-entity.md), o **MoveEmployee** intenção detetado quando mover um funcionário existente de um edifício e do office para outro. As expressões de exemplo tinham as localizações de origem e destino, mas não utilizou a funções. Em vez disso, a origem e destino estavam subordinados da entidade hierárquica. 

Neste tutorial, a aplicação de recursos humanos Deteta expressões com sobre mover novos funcionários de uma cidade para outra. Esses dois tipos de expressões são os mesmos mas resolvido com diferentes capacidades de LUIS.

|Tutorial|Expressão de exemplo|Localizações de origem e destino|
|--|--|--|
|[Hierárquica (não existem funções)](luis-quickstart-intent-and-hier-entity.md)|mV Jill Jones partir **a-2349** para **b-1298**|a-2349, b-1298|
|Neste tutorial (com funções)|Mover Billy Patterson partir **Yuma** ao **Denver**.|Yuma, Denver|

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos Seguintes

Este tutorial adicionado uma entidade com funções e uma intenção com expressões de exemplo. A primeira predição de ponto final com a entidade corretamente prever a intenção mas com uma pontuação de confiança baixa. Apenas uma das duas entidades foi detetada. Em seguida, o tutorial adicionado um padrão que são utilizadas as funções de entidade e uma lista de frase para aumentar o valor dos nomes de cidade nas expressões. A predição de ponto final segundo devolveu uma classificação de confiança elevada e encontrar ambas as funções de entidade. 

> [!div class="nextstepaction"]
> [Aprenda as melhores práticas para aplicações de LUIS](luis-concept-best-practices.md)
