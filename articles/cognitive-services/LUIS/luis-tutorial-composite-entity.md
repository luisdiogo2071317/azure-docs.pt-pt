---
title: 'Tutorial 6: Extrair dados compostos com entidade composto do LUIS'
titleSuffix: Azure Cognitive Services
description: Adicione uma entidade composta para reunir dados extraídos de vários tipos com uma única entidade contentora. Ao agrupar os dados, a aplicação cliente pode, facilmente, extrair dados relacionados em diferentes tipos de dados.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 192be1561559ac17ae98ae24ee92b292f38313a9
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45629139"
---
# <a name="tutorial-6-group-and-extract-related-data"></a>Tutorial 6: Agrupar e extrair dados relacionados
Neste tutorial, adicione uma entidade composta para reunir dados extraídos de vários tipos com uma única entidade contentora. Ao agrupar os dados, a aplicação cliente pode, facilmente, extrair dados relacionados em diferentes tipos de dados.

O objetivo da entidade composto consiste em Agrupar entidades relacionadas numa entidade da categoria principal. As informações existem como entidades separadas antes da criação de uma composição. Ele é semelhante à entidade hierárquica, mas pode conter diferentes tipos de entidades. 

A entidade composta é uma boa opção para este tipo de dados, porque os dados:

* Estão relacionados entre si. 
* Utilize uma variedade de tipos de entidade.
* Tem de ser agrupados e processados pela aplicação de cliente como uma unidade de informações.

**Neste tutorial, ficará a saber como:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Utilizar o tutorial de aplicação existente
> * Adicionar entidade composta 
> * Preparar
> * Publicar
> * Obter as intenções e entidades do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente
Continuar com a aplicação criada no tutorial última, com o nome **RecursosHumanos**. 

Se não tiver a aplicação de RecursosHumanos no tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-hier-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Do **Manage** na secção a **versões** separador, clonar a versão e nomeie- `composite`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter quaisquer carateres que não são válidos num URL.


## <a name="composite-entity"></a>entidade composta
Crie uma entidade composta quando as entidades separadas podem ser agrupadas logicamente e este agrupamento lógico é útil para a aplicação cliente. 

Nesta aplicação, o nome do funcionário é definido no **funcionário** listar entidade e inclui sinónimos de nome, endereço de e-mail, a extensão de telefone da empresa, número de telemóvel e dos EUA ID do imposto sobre Federal. 

O **MoveEmployee** intenção tem expressões de exemplo para pedir um funcionário ser movido de um edifício e do office para outro. Os nomes de construção são alfabéticos: "A", "B", etc. Embora escritórios sejam numéricos: "1234", "13245". 

Expressões com de exemplo no **MoveEmployee** intenção incluem:

|Expressões de exemplo|
|--|
|Mova John W. Cardoso a-2345|
|trocar x12345 para h-1234 amanhã|
 
O pedido de movimentação deve incluir o funcionário (usando qualquer sinónimos) e a localização final de criação e o office. O pedido também pode incluir o escritório de origem, bem como uma data que a movimentação deve acontecer. 

Os dados extraídos do ponto de extremidade devem conter essas informações e retorná-lo no `RequestEmployeeMove` composta entidade:

```JSON
"compositeEntities": [
  {
    "parentType": "RequestEmployeeMove",
    "value": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
    "children": [
      {
        "type": "builtin.datetimeV2.datetime",
        "value": "march 3 2 p.m"
      },
      {
        "type": "Locations::Destination",
        "value": "z - 2345"
      },
      {
        "type": "Employee",
        "value": "jill jones"
      },
      {
        "type": "Locations::Origin",
        "value": "a - 1234"
      }
    ]
  }
]
```

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Sobre o **intenções** página, selecione **MoveEmployee** intenção. 

3. Selecione o ícone de lupa na barra de ferramentas para filtrar a lista de expressões. 

    [![](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png "Captura de ecrã do LUIS na intenção de 'MoveEmployee' com o botão da Lupa realçado")](media/luis-tutorial-composite-entity/hr-moveemployee-magglass.png#lightbox)

4. Introduza `tomorrow` na caixa de texto filtro para encontrar a expressão `shift x12345 to h-1234 tomorrow`.

    [![](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png "Captura de ecrã do LUIS na intenção de 'MoveEmployee' com o filtro do 'futuro' realçado")](media/luis-tutorial-composite-entity/hr-filter-by-tomorrow.png#lightbox)

    Outro método é filtrar a entidade por datetimeV2, selecionando **filtros de entidade** , em seguida, selecione **datetimeV2** da lista. 

5. Selecione a primeira entidade `Employee`, em seguida, selecione **encapsular na entidade composta** na lista de menu de pop-up. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-1.png "Captura de ecrã do LUIS na intenção 'MoveEmployee', selecionar a primeira entidade na composição realçada")](media/luis-tutorial-composite-entity/hr-create-entity-1.png#lightbox)


6. Em seguida, selecione imediatamente a última entidade `datetimeV2` na expressão. Uma barra de verde é desenhada sob as palavras selecionadas que indica uma entidade composta. No menu de pop-up, introduza o nome composto `RequestEmployeeMove` , em seguida, selecione introduzir. 

    [![](media/luis-tutorial-composite-entity/hr-create-entity-2.png "Captura de ecrã do LUIS na intenção \"MoveEmployee\" selecionando a última entidade no compostos e criar entidades realçado")](media/luis-tutorial-composite-entity/hr-create-entity-2.png#lightbox)

7. Na **o tipo de entidade que pretende criar?**, quase todos os campos necessários estão na lista. Apenas a localização de origem está em falta. Selecione **adicionar entidade subordinada**, selecione **Locations::Origin** da lista de entidades existentes, em seguida, selecione **feito**. 

    Tenha em atenção que a entidade pré-criados, número, foi adicionada à entidade composta. Se poderia ter uma entidade pré-criados aparecem entre o início e a terminar em tokens de uma entidade composta, a entidade composta tem de conter essas entidades criados previamente. Se a entidades pré-concebidas não estão incluídas, a entidade composta não está prevista corretamente, mas é de cada elemento individual.

    ![Captura de ecrã do LUIS na intenção "MoveEmployee" adicionar outra entidade na janela de pop-up](media/luis-tutorial-composite-entity/hr-create-entity-ddl.png)

8. Selecione a Lupa na barra de ferramentas para remover o filtro. 

9. Remover a palavra `tomorrow` do filtro para que possa ver todas as expressões de exemplo novamente. 

## <a name="label-example-utterances-with-composite-entity"></a>Expressões com de exemplo de etiqueta com a entidade composta


1. Em cada ocorrência de pronunciação de exemplo, selecione a entidade de mais à esquerda que deve estar na composição. Em seguida, selecione **encapsular na entidade composta**.

    [![](media/luis-tutorial-composite-entity/hr-label-entity-1.png "Captura de ecrã do LUIS na intenção 'MoveEmployee', selecionar a primeira entidade na composição realçada")](media/luis-tutorial-composite-entity/hr-label-entity-1.png#lightbox)

2. Selecione a última palavra na entidade composta, em seguida, selecione **RequestEmployeeMove** no menu de pop-up. 

    [![](media/luis-tutorial-composite-entity/hr-label-entity-2.png "Captura de ecrã do LUIS na intenção \"MoveEmployee\" selecionando a última entidade na composição realçada")](media/luis-tutorial-composite-entity/hr-label-entity-2.png#lightbox)

3. Certifique-se de que todas as expressões na intenção são rotuladas com a entidade composta. 

    [![](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png "Captura de ecrã do LUIS no MoveEmployee com todas as expressões com o nome")](media/luis-tutorial-composite-entity/hr-all-utterances-labeled.png#lightbox)

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obter a intenção e entidades do ponto final 

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `Move Jill Jones from a-1234 to z-2345 on March 3 2 p.m.`. O último parâmetro de cadeia de consulta é `q`, a consulta de expressão. 

    Uma vez que este teste é verificar que a composição é extraída corretamente, um teste ou pode incluir uma expressão de exemplo existente ou uma expressão de novo. É um bom teste incluir todas as entidades de subordinado na entidade composta.

    ```JSON
    {
      "query": "Move Jill Jones from a-1234 to z-2345 on March 3  2 p.m",
      "topScoringIntent": {
        "intent": "MoveEmployee",
        "score": 0.9959525
      },
      "intents": [
        {
          "intent": "MoveEmployee",
          "score": 0.9959525
        },
        {
          "intent": "GetJobInformation",
          "score": 0.009858314
        },
        {
          "intent": "ApplyForJob",
          "score": 0.00728598563
        },
        {
          "intent": "FindForm",
          "score": 0.0058053555
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.005371796
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00266987388
        },
        {
          "intent": "None",
          "score": 0.00123299169
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.00116407464
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00102653319
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.0006628214
        }
      ],
      "entities": [
        {
          "entity": "march 3 2 p.m",
          "type": "builtin.datetimeV2.datetime",
          "startIndex": 41,
          "endIndex": 54,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03T14",
                "type": "datetime",
                "value": "2018-03-03 14:00:00"
              },
              {
                "timex": "XXXX-03-03T14",
                "type": "datetime",
                "value": "2019-03-03 14:00:00"
              }
            ]
          }
        },
        {
          "entity": "jill jones",
          "type": "Employee",
          "startIndex": 5,
          "endIndex": 14,
          "resolution": {
            "values": [
              "Employee-45612"
            ]
          }
        },
        {
          "entity": "z - 2345",
          "type": "Locations::Destination",
          "startIndex": 31,
          "endIndex": 36,
          "score": 0.9690751
        },
        {
          "entity": "a - 1234",
          "type": "Locations::Origin",
          "startIndex": 21,
          "endIndex": 26,
          "score": 0.9713137
        },
        {
          "entity": "-1234",
          "type": "builtin.number",
          "startIndex": 22,
          "endIndex": 26,
          "resolution": {
            "value": "-1234"
          }
        },
        {
          "entity": "-2345",
          "type": "builtin.number",
          "startIndex": 32,
          "endIndex": 36,
          "resolution": {
            "value": "-2345"
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 47,
          "endIndex": 47,
          "resolution": {
            "value": "3"
          }
        },
        {
          "entity": "2",
          "type": "builtin.number",
          "startIndex": 50,
          "endIndex": 50,
          "resolution": {
            "value": "2"
          }
        },
        {
          "entity": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
          "type": "RequestEmployeeMove",
          "startIndex": 5,
          "endIndex": 54,
          "score": 0.4027723
        }
      ],
      "compositeEntities": [
        {
          "parentType": "RequestEmployeeMove",
          "value": "jill jones from a - 1234 to z - 2345 on march 3 2 p . m",
          "children": [
            {
              "type": "builtin.datetimeV2.datetime",
              "value": "march 3 2 p.m"
            },
            {
              "type": "Locations::Destination",
              "value": "z - 2345"
            },
            {
              "type": "Employee",
              "value": "jill jones"
            },
            {
              "type": "Locations::Origin",
              "value": "a - 1234"
            }
          ]
        }
      ]
    }
    ```

  Esta expressão devolve uma matriz de entidades compostos. Cada entidade é dado um tipo e valor. Para localizar mais de precisão de cada entidade subordinada, utilize a combinação de tipo e o valor do item de matriz compostos para localizar o item correspondente da matriz de entidades.  

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos Seguintes

Neste tutorial, criado uma entidade composta para encapsular entidades existentes. Isso permite que a aplicação de cliente localizar um grupo de dados relacionados em diferentes tipos de dados para continuar a conversa. Uma aplicação de cliente para esta aplicação de recursos humanos pode pedir que dia e hora a movimentação tem de começar e terminar. Também pode perguntar sobre outras logística do movesuch como um telefone físico. 

> [!div class="nextstepaction"] 
> [Saiba como adicionar uma entidade com uma lista de frase](luis-quickstart-primary-and-secondary-data.md)  