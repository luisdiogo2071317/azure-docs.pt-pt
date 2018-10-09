---
title: 'Tutorial 9: análise de sentimentos incluindo positivos, negativos e neutros no LUIS'
titleSuffix: Azure Cognitive Services
description: Neste tutorial, vai criar uma aplicação que demonstra como extrair sentimentos positivos, negativos e neutros de expressões. O sentimento é determinado a partir da expressão inteira.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: ff5a47f977f34535c5ad1fde7e6cac5995e7f7dd
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031463"
---
# <a name="tutorial-9--extract-sentiment-of-overall-utterance"></a>Tutorial 9: extrair o sentimento da expressão geral
Neste tutorial, vai criar uma aplicação que demonstra como extrair sentimentos positivos, negativos e neutros de expressões. O sentimento é determinado a partir da expressão inteira.

A análise de sentimentos é a capacidade de determinar se a expressão de um utilizador é positiva, negativa ou neutra. 

As expressões seguintes mostram exemplos de sentimentos:

|Sentimento|Classificação|Expressão|
|:--|:--|:--|
|positiva|0,91 |John W. Smith fez um excelente trabalho na apresentação em Paris.|
|positiva|0,84 |jill-jones@mycompany.com fez um excelente trabalho no pitch de vendas da Parker.|

A análise de sentimentos é uma definição de publicação que se aplica a cada expressão. Não precisa de encontrar as palavras que indicam o sentimento na expressão e rotulá-las porque a análise de sentimento aplica-se a toda a expressão. 

Como é uma definição de publicação, não o vê nas páginas de intenções ou entidades. Pode vê-lo no painel [teste interativo](luis-interactive-test.md#view-sentiment-results) ou quando testar o URL de ponto final. 

**Neste tutorial, ficará a saber como:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Utilizar a aplicação de tutorial existente 
> * Adicionar a análise de sentimentos como definição de publicação
> * Preparar
> * Publicar
> * Obter o sentimento da expressão a partir do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Utilizar a aplicação existente

Continue com a aplicação criada no último tutorial, com o nome **RecursosHumanos**. 

Se não tiver a aplicação RecursosHumanos do tutorial anterior, utilize os seguintes passos:

1.  Transfira e guarde o [ficheiro JSON da aplicação](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-keyphrase-HumanResources.json).

2. Importe o JSON para uma nova aplicação.

3. Na secção **Gerir**, no separador **Versões**, clone a versão e dê-lhe o nome `sentiment`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. Como o nome da versão é utilizado como parte da rota de URL, o nome não pode conter carateres que não sejam válidos num URL.

## <a name="employeefeedback-intent"></a>Intenção EmployeeFeedback 
Adicione uma nova intenção para capturar os comentários dos colaboradores membros da empresa. 

1. [!include[Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Selecione **Create new intent** (Criar nova intenção).

3. Nomeie a nova intenção `EmployeeFeedback`.

    ![Criar nova caixa de diálogo de intenção com EmployeeFeedback como nome](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent-ddl.png)

4. Adicione várias expressões que indiquem que um colaborador está a fazer algo bem ou uma área que precisa de ser melhorada:

    Lembre-se de que nesta aplicação de Recursos Humanos, os colaboradores estão definidos na entidade de lista, `Employee`, pelo nome, e-mail, número de extensão do telefone, número de telemóvel e os respetivos E.U.A. federal segurança números de segurança social federais dos E.U.A. 

    |Expressões|
    |--|
    |425-555-1212 fez um bom trabalho ao dar as boas-vindas a uma colega que voltou da licença de maternidade|
    |234-56-7891 fez um bom trabalho ao confortar um colega no seu momento de luto.|
    |jill-jones@mycompany.com não tinha todas as faturas precisas para a documentação.|
    |john.w.smith@mycompany.com entregou os formulários exigidos com um mês de atraso sem assinaturas|
    |x23456 faltou à reunião importante de marketing externa.|
    |x12345 faltou à reunião das revisões de junho.|
    |Jill Jones foi um sucesso no pitch de vendas em Harvard|
    |John W. Smith fez um excelente trabalho na apresentação em Stanford|

    [ ![Captura de ecrã da aplicação LUIS com expressões de exemplo na intenção EmployeeFeedback](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png#lightbox)

## <a name="train"></a>Preparar

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Configurar a aplicação para incluir análise de sentimentos
1. Selecione **Gerir** no painel de navegação superior direito e, em seguida, selecione **Definições de publicação** no menu à esquerda.

2. Selecione **Análise de Sentimentos** para ativar/desativar esta definição. 

    ![](./media/luis-quickstart-intent-and-sentiment-analysis/turn-on-sentiment-analysis-as-publish-setting.png)

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-sentiment-of-utterance-from-endpoint"></a>Obter o sentimento da expressão a partir do ponto final

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `Jill Jones work with the media team on the public portal was amazing`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões etiquetadas, pelo que é um bom teste e deve devolver a intenção `EmployeeFeedback` com a análise de sentimentos extraída.
    
    ```JSON
    {
      "query": "Jill Jones work with the media team on the public portal was amazing",
      "topScoringIntent": {
        "intent": "EmployeeFeedback",
        "score": 0.4983256
      },
      "intents": [
        {
          "intent": "EmployeeFeedback",
          "score": 0.4983256
        },
        {
          "intent": "MoveEmployee",
          "score": 0.06617523
        },
        {
          "intent": "GetJobInformation",
          "score": 0.04631853
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0103248553
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.007531875
        },
        {
          "intent": "FindForm",
          "score": 0.00344597152
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00337914471
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0026357458
        },
        {
          "intent": "None",
          "score": 0.00214573368
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00157622492
        },
        {
          "intent": "Utilities.Confirm",
          "score": 7.379545E-05
        }
      ],
      "entities": [
        {
          "entity": "jill jones",
          "type": "Employee",
          "startIndex": 0,
          "endIndex": 9,
          "resolution": {
            "values": [
              "Employee-45612"
            ]
          }
        },
        {
          "entity": "media team",
          "type": "builtin.keyPhrase",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "public portal",
          "type": "builtin.keyPhrase",
          "startIndex": 43,
          "endIndex": 55
        },
        {
          "entity": "jill jones",
          "type": "builtin.keyPhrase",
          "startIndex": 0,
          "endIndex": 9
        }
      ],
      "sentimentAnalysis": {
        "label": "positive",
        "score": 0.8694164
      }
    }
    ```

    O sentimentAnalysis é positivo com uma pontuação de 0,86. 

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes
Neste tutorial, adicione a análise de sentimentos como uma definição de publicação para extrair valores de sentimentos da expressão como um todo.

> [!div class="nextstepaction"] 
> [Rever expressões de ponto final na aplicação de RH](luis-tutorial-review-endpoint-utterances.md) 

