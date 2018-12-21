---
title: Correspondência exata de texto
titleSuffix: Azure Cognitive Services
description: Obtenha dados que correspondam a uma lista de itens predefinida. Cada item na lista pode ter sinónimos também com correspondência exata
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: cf2cd8a3dade408bf98a6fcc64af0d4ee4419fa0
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53718253"
---
# <a name="tutorial-4-extract-exact-text-matches"></a>Tutorial 4: Extrair correspondências de texto exatas
Neste tutorial, vai aprender a obter dados que correspondam a uma lista de itens predefinida. Cada item na lista pode incluir uma lista de sinónimos. Para a aplicação de recursos humanos, um colaborador pode ser identificado através de várias informações cruciais, como o nome, e-mail, número de telefone e número de contribuinte. 

A aplicação Recursos Humanos precisa de determinar que colaborador está a mudar de um edifício para outro. Para uma expressão sobre uma mudança de colaborador, o LUIS determina a intenção e extrai o colaborador para que a aplicação cliente crie uma ordem padrão para o mover.

Esta aplicação utiliza uma entidade de lista para extrair o colaborador. O colaborador pode ser referenciado pelo nome, extensão da empresa, número de telemóvel, e-mail ou número da segurança social. 

Uma entidade de lista é uma boa opção para este tipo de dados quando:

* Os valores dos dados são um conjunto conhecido.
* O conjunto não excede os [limites](luis-boundaries.md) máximos do LUIS para este tipo de entidade.
* O texto na expressão é uma correspondência exata com um sinónimo ou o nome canónico. 

**Neste tutorial, ficará a saber como:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Utilizar a aplicação de tutorial existente
> * Adicionar intenção MoveEmployee
> * Adicionar entidade de lista 
> * Preparar 
> * Publicar
> * Obter as intenções e as entidades do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente
Continue com a aplicação criada no último tutorial, com o nome **RecursosHumanos**. 

Se não tiver a aplicação RecursosHumanos do tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde o [ficheiro JSON da aplicação](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-regex-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Na secção **Gerir**, no separador **Versões**, clone a versão e dê-lhe o nome `list`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter carateres que não sejam válidos num URL. 


## <a name="moveemployee-intent"></a>Intenção MoveEmployee

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecione **Create new intent** (Criar nova intenção). 

3. Introduza `MoveEmployee` na caixa de diálogo de pop-up e, em seguida, selecione **Concluído**. 

    ![Captura de ecrã da caixa de diálogo Criar nova de intenção com](./media/luis-quickstart-intent-and-list-entity/hr-create-new-intent-ddl.png)

4. Adicione expressões de exemplo à intenção.

    |Expressões de exemplo|
    |--|
    |mover John W. Smith de B-1234 para H-4452|
    |mv john.w.smith@mycompany.com do escritório b-1234 para o escritório h-4452|
    |trocar x12345 para h-1234 amanhã|
    |colocar 425-555-1212 no HH-2345|
    |mover 123-45-6789 de A-4321 para J-23456|
    |mv Jill Jones de D-2345 para J-23456|
    |trocar jill-jones@mycompany.com para M-12345|
    |x23456 para M-12345|
    |425-555-0000 para h-4452|
    |234-56-7891 para hh-2345|

    [ ![Captura de ecrã da página Intenção com as novas expressões realçadas](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png) ](./media/luis-quickstart-intent-and-list-entity/hr-enter-utterances.png#lightbox)

    Lembre-se de que o número e datetimeV2 foram adicionados num tutorial anterior e serão etiquetados automaticamente quando forem encontrados nas expressões de exemplo.

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="employee-list-entity"></a>Entidade de lista de colaboradores
Agora que a intenção **MoveEmployee** tem expressões de exemplo, o LUIS tem de compreender o que é um colaborador. 

O nome _canónico_ principal para cada item é o número de colaborador. Para este domínio, os exemplos dos sinónimos de cada nome canónico são: 

|Objetivo do sinónimo|Valor do sinónimo|
|--|--|
|Nome|John W. Smith|
|Endereço de e-mail|john.w.smith@mycompany.com|
|Extensão do telefone|x12345|
|Número do telemóvel pessoal|425-555-1212|
|Número de segurança social federal dos E.U.A.|123-45-6789|


1. Selecione **Entidades** no painel esquerdo.

2. Selecione **Criar nova entidade**.

3. Na caixa de diálogo pop-up de entidade, introduza `Employee` para o nome da entidade e **Lista** para o tipo de entidade. Selecione **Done** (Concluído).  

    [![Captura de ecrã da criação de caixa de diálogo de pop-up entidade novo](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png "captura de ecrã da criação de caixa de diálogo de pop-up entidade novo")](media/luis-quickstart-intent-and-list-entity/hr-list-entity-ddl.png#lightbox)

4. Na página de entidade de colaborador, introduza `Employee-24612` como o novo valor.

    [![Captura de ecrã da introdução de valor](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png "captura de ecrã da introdução de valor")](media/luis-quickstart-intent-and-list-entity/hr-emp1-value.png#lightbox)

5. Para Sinónimos, adicione os seguintes valores:

    |Objetivo do sinónimo|Valor do sinónimo|
    |--|--|
    |Nome|John W. Smith|
    |Endereço de e-mail|john.w.smith@mycompany.com|
    |Extensão do telefone|x12345|
    |Número do telemóvel pessoal|425-555-1212|
    |Número de segurança social federal dos E.U.A.|123-45-6789|

    [![Captura de ecrã da introdução de sinónimos](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png "captura de ecrã da introdução sinónimos")](media/luis-quickstart-intent-and-list-entity/hr-emp1-synonyms.png#lightbox)

6. Introduza o `Employee-45612` como um novo valor.

7. Para Sinónimos, adicione os seguintes valores:

    |Objetivo do sinónimo|Valor do sinónimo|
    |--|--|
    |Nome|Jill Jones|
    |Endereço de e-mail|jill-jones@mycompany.com|
    |Extensão do telefone|x23456|
    |Número do telemóvel pessoal|425-555-0000|
    |Número de segurança social federal dos E.U.A.|234-56-7891|

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obter as intenções e as entidades do ponto final

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Vá para o final do URL no endereço e introduza `shift 123-45-6789 from Z-1242 to T-54672`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões identificadas, pelo que é um bom teste e deve devolver a intenção `MoveEmployee` com o `Employee` extraído.

  ```json
  {
    "query": "shift 123-45-6789 from Z-1242 to T-54672",
    "topScoringIntent": {
      "intent": "MoveEmployee",
      "score": 0.9882801
    },
    "intents": [
      {
        "intent": "MoveEmployee",
        "score": 0.9882801
      },
      {
        "intent": "FindForm",
        "score": 0.016044287
      },
      {
        "intent": "GetJobInformation",
        "score": 0.007611245
      },
      {
        "intent": "ApplyForJob",
        "score": 0.007063288
      },
      {
        "intent": "Utilities.StartOver",
        "score": 0.00684710965
      },
      {
        "intent": "None",
        "score": 0.00304174074
      },
      {
        "intent": "Utilities.Help",
        "score": 0.002981
      },
      {
        "intent": "Utilities.Confirm",
        "score": 0.00212222221
      },
      {
        "intent": "Utilities.Cancel",
        "score": 0.00191026414
      },
      {
        "intent": "Utilities.Stop",
        "score": 0.0007461446
      }
    ],
    "entities": [
      {
        "entity": "123 - 45 - 6789",
        "type": "Employee",
        "startIndex": 6,
        "endIndex": 16,
        "resolution": {
          "values": [
            "Employee-24612"
          ]
        }
      },
      {
        "entity": "123",
        "type": "builtin.number",
        "startIndex": 6,
        "endIndex": 8,
        "resolution": {
          "value": "123"
        }
      },
      {
        "entity": "45",
        "type": "builtin.number",
        "startIndex": 10,
        "endIndex": 11,
        "resolution": {
          "value": "45"
        }
      },
      {
        "entity": "6789",
        "type": "builtin.number",
        "startIndex": 13,
        "endIndex": 16,
        "resolution": {
          "value": "6789"
        }
      },
      {
        "entity": "-1242",
        "type": "builtin.number",
        "startIndex": 24,
        "endIndex": 28,
        "resolution": {
          "value": "-1242"
        }
      },
      {
        "entity": "-54672",
        "type": "builtin.number",
        "startIndex": 34,
        "endIndex": 39,
        "resolution": {
          "value": "-54672"
        }
      }
    ]
  }
  ```

  O colaborador foi encontrado e devolvido como o tipo `Employee` com um valor de resolução de `Employee-24612`.

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos Seguintes
Neste tutorial, criou uma nova intenção, adicionou expressões de exemplo e criou uma entidade de lista para extrair correspondências de texto exatas a partir de expressões. Depois de preparar e publicar a aplicação, uma consulta ao ponto final identifica a intenção e devolve os dados extraídos.

> [!div class="nextstepaction"]
> [Adicione uma entidade hierárquica à aplicação](luis-quickstart-intent-and-hier-entity.md)

