---
title: Tutorial sobre como utilizar funções padrão para melhorar as previsões de LUIS – Azure | Documentos da Microsoft
titleSuffix: Cognitive Services
description: Neste tutorial, utilize as funções de padrão para entidades relacionadas contextualmente para melhorar as previsões do LUIS.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 08/03/2018
ms.author: diberry
ms.openlocfilehash: 2fd473226dca2576be79b90bc05d66599f759713
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39524149"
---
# <a name="tutorial-improve-app-with-pattern-roles"></a>Tutorial: Melhorar a aplicação com funções padrão

Neste tutorial, utilize uma entidade com funções combinadas com padrões para aumentar a predição de intenção e entidade.  Ao usar padrões, expressões de exemplo menos são necessários para a intenção.

> [!div class="checklist"]
* Compreender as funções padrão
* Utilizar entidade simple com as funções 
* Criar padrão para expressões com usando entidade simple com as funções
* Como verificar as melhorias de predição do padrão

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de recursos humanos do [padrão](luis-tutorial-pattern.md) tutorial, [importar](luis-how-to-start-new-app.md#import-new-app) o JSON para uma nova aplicação no [LUIS](luis-reference-regions.md#luis-website) Web site. A aplicação para importar se encontra no [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-patterns-HumanResources-v2.json) repositório do GitHub.

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `roles`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="the-purpose-of-roles"></a>A finalidade das funções
A finalidade das funções é extrair relacionados com as entidades numa expressão. Na expressão, `Move new employee Robert Williams from Sacramento and San Francisco`, a cidade de origem e destino cidade valores estão relacionados entre si e utilizam uma linguagem comum para indicar cada localização. 

Ao usar padrões, qualquer entidades no padrão devem ser detetadas _antes de_ o padrão corresponde a expressão. 

Quando cria um padrão, a primeira etapa é selecionar a intenção para o padrão. Ao selecionar a intenção, se o padrão corresponder, a intenção correta é sempre devolvida com uma classificação alta (normalmente, 99-100%). 

### <a name="compare-hierarchical-entity-to-simple-entity-with-roles"></a>Comparar entidades hierárquicas a entidade simple com as funções

Na [tutorial hierárquica](luis-quickstart-intent-and-hier-entity.md), o **MoveEmployee** intenção detetado quando mover um funcionário existente de um edifício e do office para outro. As expressões de exemplo tinham as localizações de origem e destino, mas não utilizou a funções. Em vez disso, a origem e destino estavam subordinados da entidade hierárquica. 

Neste tutorial, a aplicação de recursos humanos Deteta expressões com sobre mover novos funcionários de uma cidade para outra. Esses dois tipos de expressões são semelhantes, mas resolvido com diferentes capacidades de LUIS.

|Tutorial|Expressão de exemplo|Localizações de origem e destino|
|--|--|--|
|[Hierárquica (não existem funções)](luis-quickstart-intent-and-hier-entity.md)|mV Jill Jones partir **a-2349** para **b-1298**|a-2349, b-1298|
|Neste tutorial (com funções)|Mover Billy Patterson partir **Yuma** ao **Denver**.|Yuma, Denver|

Não é possível utilizar a entidade hierárquica no padrão de uma vez que apenas hierárquicas pais são utilizados em pais. Para retornar as localizações com nome de origem e destino, se pergunta utilize um padrão.

### <a name="simple-entity-for-new-employee-name"></a>Entidade Simple para o novo nome de funcionário
O nome do novo funcionário, Billy Patterson, não faz parte da entidade lista **funcionário** ainda. O novo nome do funcionário é extraído em primeiro lugar, para enviar o nome para um sistema externo para criar as credenciais da empresa. Depois das credenciais da empresa são criadas, as credenciais de funcionário são adicionadas para a entidade de lista **funcionário**.

O **funcionário** lista foi criada no [tutorial lista](luis-quickstart-intent-and-list-entity.md).

O **NewEmployee** entidade é uma entidade com não existem funções. 

### <a name="simple-entity-with-roles-for-relocation-cities"></a>Entidade Simple com as funções para cidades reposicionamento
O novo funcionário e a família tem de ser movido da cidade de atual para uma cidade onde está localizada a empresa fictícia. Uma vez que um novo funcionário pode vir de qualquer cidade, as localizações tem de ser detetados. Uma lista de conjunto, como uma entidade de lista não funciona porque apenas as cidades na lista seriam extraídas.

Os nomes de função associados com as cidades de origem e de destino tem de ser exclusivo em todas as entidades. É uma forma fácil para se certificar de que as funções são exclusivas para associá-las para a entidade contentora por meio de uma estratégia de nomenclatura. O **NewEmployeeRelocation** entidade é uma entidade com duas funções: **NewEmployeeReloOrigin** e **NewEmployeeReloDestination**.

### <a name="simple-entities-need-enough-examples-to-be-detected"></a>Entidades simples precisam suficiente exemplos como detetada
Uma vez que a expressão de exemplo `Move new employee Robert Williams from Sacramento and San Francisco` tem apenas entidades aprendidas por máquina, é importante fornecer suficiente expressões de exemplo para a intenção, para que as entidades são detetadas.  

**Embora os padrões permitem que forneça menos expressões de exemplo, se as entidades não são detetadas, o padrão não corresponde.**

Se tiver dificuldade para com a detecção de entidade simples porque é um nome como uma cidade, considere adicionar uma lista de frase de valores similares. Isto ajuda a detetar o nome de cidade fornecendo o LUIS um sinal adicional sobre esse tipo de palavra ou frase. Listas de frase ajudam apenas o padrão, ajudando com deteção de entidade, o que é necessária para o padrão para corresponder. 

## <a name="create-new-entities"></a>Criar novas entidades
1. Selecione **criar** no menu superior.

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

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final

[!include[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-without-pattern"></a>O ponto de extremidade sem padrão de consulta

1. [!include[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

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

Padrões ajudará a pontuação de predição, no entanto, as entidades têm de ser previstas corretamente antes do padrão corresponde a expressão. 

## <a name="add-a-pattern-that-uses-roles"></a>Adicionar um padrão que utiliza funções
1. Selecione **criar** no painel de navegação superior.

2. Selecione **padrões** na navegação à esquerda.

3. Selecione **NewEmployeeRelocationProcess** partir do **selecione um objetivo** na lista pendente. 

4. Introduza o seguinte padrão: `move {NewEmployee} from {NewEmployeeRelocation:NewEmployeeReloOrigin} to {NewEmployeeRelocation:NewEmployeeReloDestination}[.]`

    Se preparar, publicar e consultar o ponto final, poderá estar Desapontado ver que as entidades não foram encontradas, para que o padrão não corresponder ao, por isso também não melhorou a predição. Esta é uma conseqüência do insuficiente expressões de exemplo com entidades etiquetadas. Em vez de adicionar mais exemplos, adicione uma lista de frase para corrigir este problema.

## <a name="create-a-phrase-list-for-cities"></a>Criar uma lista de frase para cidades
Cidades, como nomes de pessoas são complicadas que podem ser qualquer combinação de palavras e pontuação. Mas as cidades da região e do mundo são conhecidas, por isso, LUIS tem uma lista de frase de cidades para começar a aprender. 

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

    Não adicione todas as cidades no mundo ou até mesmo todas as cidades na região. LUIS tem de ser capaz de generalizar que uma localidade é a partir da lista. 

    Certifique-se de manter **estes valores são intercambiáveis** selecionado. Esta definição significa que as palavras na lista no tratados como sinónimos. Isso é exatamente como deve ser tratados no padrão.

    Lembre-se [a última vez](luis-quickstart-primary-and-secondary-data.md) série do tutorial, criado uma lista de frase também foi melhorar a deteção de entidade de uma entidade.  

3. Formar e publicar a aplicação.

## <a name="query-endpoint-for-pattern"></a>Ponto final da consulta para padrão
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

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

## <a name="clean-up-resources"></a>Limpar recursos

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Aprenda as melhores práticas para aplicações de LUIS](luis-concept-best-practices.md)