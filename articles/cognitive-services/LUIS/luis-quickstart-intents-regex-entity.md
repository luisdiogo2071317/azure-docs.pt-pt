---
title: Entidade de expressão regular
titleSuffix: Azure Cognitive Services
description: Extraia dados formatados de forma consistente a partir de uma expressão com a entidade Expressão Regular.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 654d3c4e7dd7ec8916b785f52a78388ec04b3cc9
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/21/2018
ms.locfileid: "53712785"
---
# <a name="tutorial-3-extract-well-formatted-data"></a>Tutorial 3: Extrair dados bem formatados
Neste tutorial, irá modificar a aplicação Recursos Humanos para extrair dados formatados de forma consistente a partir de uma expressão com a entidade **Expressão Regular**.

O objetivo de uma entidade é extrair dados importantes contidos na expressão. A utilização da aplicação da entidade de expressão regular é extrair os números de formulário de Recursos Humanos (RH) formatados a partir de uma expressão. Embora a intenção da expressão seja sempre determinada por machine learning, este tipo de entidade específica não é de machine-learned. 

**As expressões de exemplo incluem:**

|Expressões de exemplo|
|--|
|Onde posso encontrar HRF 123456?|
|Quem criados HRF 123234?|
|HRF 456098 está publicada em francês?|
|HRF 456098|
|Data de HRF 456098?|
 
Uma expressão regular é uma boa opção para este tipo de dados quando:

* Os dados estão bem formatados.

**Neste tutorial, ficará a saber como:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Utilizar a aplicação de tutorial existente
> * Adicionar a intenção FindForm
> * Adicionar entidade de expressão regular 
> * Preparar
> * Publicar
> * Obter as intenções e as entidades do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente
Continue com a aplicação criada no último tutorial, com o nome **RecursosHumanos**. 

Se não tiver a aplicação RecursosHumanos do tutorial anterior, utilize os seguintes passos:

1. Transfira e guarde o [ficheiro JSON da aplicação](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-prebuilts-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Na secção **Gerir**, no separador **Versões**, clone a versão e dê-lhe o nome `regex`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter carateres que não sejam válidos num URL. 

## <a name="findform-intent"></a>Intenção FindForm

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecione **Create new intent** (Criar nova intenção). 

3. Introduza `FindForm` na caixa de diálogo de pop-up e, em seguida, selecione **Concluído**. 

    ![Captura de ecrã da caixa de diálogo Criar nova intenção com Utilitários na caixa de pesquisa](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Adicione expressões de exemplo à intenção.

    |Expressões de exemplo|
    |--|
    |Qual é o URL para hrf-123456?|
    |Onde está hrf-345678?|
    |Quando foi atualizado hrf-456098?|
    |João Silva atualizou hrf-234639 na semana passada?|
    |Quantas versões existem de hrf-345123?|
    |Quem tem de autorizar o formulário hrf-123456?|
    |Quantas pessoas têm de terminar sessão em hrf-345678?|
    |Data de hrf-234123?|
    |Autor de hrf-546234?|
    |Título de hrf-456234?|

    [ ![Captura de ecrã da página Intenção com as novas expressões realçadas](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    A aplicação tem a entidade de número pré-concebida adicionada do tutorial anterior, pelo que cada número de formulário está etiquetado. Isto pode ser suficiente para a sua aplicação cliente, mas o número não incluirá o tipo de número. Criar uma nova entidade com um nome adequado permite à aplicação cliente processar corretamente a entidade quando é devolvida pelo LUIS.

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="regular-expression-entity"></a>Entidade de expressão regular 
A entidade de expressão regular para corresponder ao número de formulário é `hrf-[0-9]{6}`. Esta expressão regular corresponde aos carateres literais `hrf-`, mas ignora as maiúsculas e minúsculas, e as variantes culturais. Corresponde aos dígitos 0 a 9, para exatamente 6 dígitos.

HRF significa `human resources form`.

O LUIS divide a expressão em tokens quando esta é adicionada a uma intenção. A atomização para estas expressões adiciona espaços antes e depois do hífen, `Where is HRF - 123456?`. A expressão regular é aplicada à expressão no respetivo formato não processado, antes de ser atomizada. Uma vez que é aplicada ao formato _não processado_, a expressão regular não tem de lidar com limites de palavras. 

Crie uma entidade de expressão regular para indicar ao LUIS o que é um formato de número HRF nos passos seguintes:

1. Selecione **Entidades** no painel esquerdo.

2. Selecione o botão **Criar nova entidade** na página Entidades. 

3. Na caixa de diálogo de pop-up, introduza o novo nome de entidade `HRF-number`, selecione **RegEx** como tipo de entidade, introduza `hrf-[0-9]{6}` como o valor **Regex** e, em seguida, selecione **Concluído**.

    ![Captura de ecrã da definição Propriedades da nova entidade da caixa de diálogo de pop-up](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Selecione **Intenções** no menu à esquerda e, em seguida, a intenção **FindForm** para ver a expressão regular etiquetada nas expressões. 

    [![Captura de ecrã da expressão Etiqueta com o padrão de entidade e regex existente](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Uma vez que a entidade não é de aprendizagem automática, a etiqueta é aplicada às expressões e apresentada no site do LUIS assim que for criada.

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obter as intenções e as entidades do ponto final

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `When were HRF-123456 and hrf-234567 published in the last year?`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões etiquetadas, pelo que é um bom teste e deve devolver a intenção `FindForm` com os dois números de formulário de `HRF-123456` e `hrf-234567`.

    ```json
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    Os números na expressão são devolvidos duas vezes, uma vez como a nova entidade `hrf-number` e outra como uma entidade pré-concebida, `number`. Uma expressão pode ter mais do que uma entidade e mais do que uma do mesmo tipo, como mostra este exemplo. Ao utilizar uma entidade de expressão regular, o LUIS extrai dados com nome, o que é mais útil programaticamente para a aplicação cliente receber a resposta JSON.


## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos Seguintes
Este tutorial criou uma nova intenção, adicionou expressões de exemplo e criou uma entidade de expressão regular para extrair dados bem formatados das expressões. Depois de preparar e publicar a aplicação, uma consulta ao ponto final identifica a intenção e devolve os dados extraídos.

> [!div class="nextstepaction"]
> [Saiba mais sobre a entidade de lista](luis-quickstart-intent-and-list-entity.md)

