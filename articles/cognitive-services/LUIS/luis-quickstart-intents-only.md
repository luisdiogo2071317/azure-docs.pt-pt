---
title: 'Tutorial 1: Localizar intenções na aplicação LUIS personalizada'
titleSuffix: Azure Cognitive Services
description: Crie uma aplicação personalizada que prevê a intenção de um utilizador. Esta aplicação é o tipo mais simples de aplicação LUIS, porque não extrai vários elementos de dados do texto da expressão, como endereços de e-mail ou datas.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: cc631f139e7f11d82f2caac83770754060353d07
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277720"
---
# <a name="tutorial-1-build-custom-app-to-determine-user-intentions"></a>Tutorial 1: Criar uma aplicação personalizada para determinar as intenções do utilizador

Neste tutorial, irá criar uma aplicação personalizada de Recursos Humanos (RH) que prevê a intenção de um utilizador com base na expressão (texto). Quando terminar, obterá um ponto final do LUIS em execução na cloud.

A finalidade da aplicação é determinar a intenção do texto de linguagem natural conversacional. Estas intenções estão categorizadas em **Intenções**. Esta aplicação tem algumas intenções. A primeira intenção, **`GetJobInformation`**, identifica quando um utilizador quer informações sobre empregos disponíveis numa empresa. A segunda intenção, **`None`**, é utilizada para todas as expressões do utilizador fora do _domínio_ (âmbito) desta aplicação. Mais tarde, uma terceira intenção, **`ApplyForJob`**, é adicionada a todas as expressões sobre a candidatura a um cargo. Esta terceira intenção é diferente de `GetJobInformation` porque as informações do cargo já devem ser conhecidas quando alguém se candidata. No entanto, consoante a opção de palavras, determinar a intenção pode ser difícil, pois ambas são sobre um cargo.

Depois de devolver a resposta JSON, o LUIS conclui este pedido. O LUIS não fornece respostas às expressões do utilizador, apenas identifica o tipo de informação que está a ser pedida em linguagem natural. 

**Neste tutorial, ficará a saber como:**

> [!div class="checklist"]
> * Criar uma nova aplicação 
> * Criar intenções
> * Adicionar expressões de exemplo
> * Preparar a aplicação
> * Publicar aplicação
> * Obter a intenção do ponto final

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Criar uma nova aplicação

1. Inicie sessão no portal do LUIS com o URL de [https://www.luis.ai](https://www.luis.ai). 

2. Selecione **Create new app** (Criar nova aplicação).  

    [![](media/luis-quickstart-intents-only/app-list.png "Captura de ecrã a mostrar a página My Apps (As Minhas Aplicações) do Language Understanding (LUIS)")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Na caixa de diálogo pop-up, introduza o nome `HumanResources` e mantenha a cultura predefinida, **Inglês**. Deixe a descrição em branco.

    ![A nova aplicação LUIS](./media/luis-quickstart-intents-only/create-app.png)

    Em seguida, a aplicação mostra a página **Intents** (Intenções) com a intenção **None** (Nenhuma).

## <a name="getjobinformation-intent"></a>Intenção GetJobInformation

1. Selecione **Create new intent** (Criar nova intenção). Introduza o nome da nova intenção `GetJobInformation`. Esta intenção é prevista sempre que um utilizador quer informações sobre cargos disponíveis na empresa.

    ![](media/luis-quickstart-intents-only/create-intent.png "Captura de ecrã a mostrar a caixa de diálogo New intent (Nova intenção) do Language Understanding (LUIS)")

2. Ao fornecer _expressões de exemplo_, está a ensinar ao LUIS os tipos de expressões que devem ser previstos para esta intenção. Adicione várias expressões de exemplo esperadas a esta intenção, tais como:

    | Expressões de exemplo|
    |--|
    |Novos empregos publicados hoje?|
    |Que vagas estão disponíveis para Engenheiros Sénior?|
    |Existe algum trabalho em bases de dados?|
    |Procuro um novo emprego com responsabilidades em contabilidade|
    |Onde posso encontrar as listas de empregos|
    |Novos empregos?|
    |Existem novas vagas no escritório de Seattle?|

    [![](media/luis-quickstart-intents-only/utterance-getstoreinfo.png "Captura de ecrã da introdução de novas expressões para a intenção MyStore")](media/luis-quickstart-intents-only/utterance-getstoreinfo.png#lightbox)

    [!INCLUDE[Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]    


## <a name="none-intent"></a>Intenção None (Nenhuma) 
A aplicação cliente tem de saber se uma expressão está fora do domínio da aplicação. Se o LUIS devolver a intenção **None** (Nenhuma) para uma expressão, a sua aplicação cliente pode perguntar se o utilizador quer terminar a conversação. A aplicação cliente também pode dar mais instruções para continuar a conversação se o utilizador não quiser terminá-la. 

Estas expressões de exemplo, fora do domínio de requerente, estão agrupadas na intenção **None** (Nenhuma). Não deixe em branco. 

1. Selecione **Intents** (Intenções) no painel esquerdo.

2. Selecione a intenção **None** (Nenhuma). Adicione três expressões que o utilizador poderá introduzir, mas que não são relevantes para a sua aplicação Recursos Humanos. Se a aplicação for sobre as suas publicações de empregos, algumas expressões **None** são:

    | Expressões de exemplo|
    |--|
    |Cães que ladram são irritantes|
    |Encomendar uma piza para mim|
    |Pinguins no oceano|


## <a name="train"></a>Preparar 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent"></a>Obter a intenção

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL na barra de endereço e introduza `I'm looking for a job with Natural Language Processing`. O último parâmetro de cadeia de consulta é `q`, a expressão **query**. Esta expressão não é igual a nenhuma das expressões de exemplo. É um bom teste e deverá devolver a intenção `GetJobInformation` como a intenção com a melhor classificação. 

    ```JSON
    {
      "query": "I'm looking for a job with Natural Language Processing",
      "topScoringIntent": {
        "intent": "GetJobInformation",
        "score": 0.8965092
      },
      "intents": [
        {
          "intent": "GetJobInformation",
          "score": 0.8965092
        },
        {
          "intent": "None",
          "score": 0.147104025
        }
      ],
      "entities": []
    }
    ```

    Os resultados incluem **todas as intenções** na aplicação, atualmente duas. A matriz de entidades está vazia porque esta aplicação não tem entidades atualmente. 

    O resultado JSON identifica a intenção com a melhor classificação como a propriedade **`topScoringIntent`**. Todas as classificações estão compreendidas entre 1 e 0, estando a melhor classificação próxima de 1. 

## <a name="applyforjob-intent"></a>Intenção ApplyForJob
Regresse ao site do LUIS e crie uma intenção para determinar se a expressão do utilizador é sobre a candidatura a um cargo.

1. Selecione **Build** (Criar) no menu direito superior para regressar à criação da aplicação.

2. Selecione **Intents** (Intenções) no menu esquerdo.

3. Selecione **Create new intent** (Criar nova intenção) e introduza o nome `ApplyForJob`. 

    ![Caixa de diálogo do LUIS para criar a nova intenção](./media/luis-quickstart-intents-only/create-applyforjob-intent.png)

4. Adicione várias expressões a esta intenção, que espera que um utilizador peça, tais como:

    | Expressões de exemplo|
    |--|
    |Quero candidatar-me ao novo trabalho de contabilidade|
    |Preencher a candidatura para o emprego 123456|
    |Submeter currículo para a vaga de engenharia|
    |Eis o meu CV para a vaga 654234|
    |Emprego 567890 e a minha documentação|

    [![](media/luis-quickstart-intents-only/utterance-applyforjob.png "Captura de ecrã da introdução de novas expressões para a intenção ApplyForJob")](media/luis-quickstart-intents-only/utterance-applyforjob.png#lightbox)

    A intenção etiquetada está realçada a vermelho porque o LUIS atualmente não tem a certeza se a intenção está correta. A aplicação de preparação informa o LUIS de que as expressões estão na intenção correta. 

## <a name="train-again"></a>Preparar novamente

[!INCLUDE[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-again"></a>Publicar novamente

[!INCLUDE[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="get-intent-again"></a>Obter a intenção novamente

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Na nova janela do browser, introduza `Can I submit my resume for job 235986` no final do URL. 

    ```JSON
    {
      "query": "Can I submit my resume for job 235986",
      "topScoringIntent": {
        "intent": "ApplyForJob",
        "score": 0.9166808
      },
      "intents": [
        {
          "intent": "ApplyForJob",
          "score": 0.9166808
        },
        {
          "intent": "GetJobInformation",
          "score": 0.07162977
        },
        {
          "intent": "None",
          "score": 0.0262826588
        }
      ],
      "entities": []
    }
    ```

    Os resultados incluem a nova intenção **ApplyForJob**, bem como as intenções existentes. 

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes

Este tutorial criou a aplicação Recursos Humanos (RH), criou duas intenções, adicionou expressões de exemplo a cada intenção, adicionou expressões de exemplo à intenção None, preparou, publicou e testou no ponto final. Estes são os passos básicos de criação de um modelo do LUIS. 

> [!div class="nextstepaction"]
> [Adicionar intenções e entidades pré-concebidas a esta aplicação](luis-tutorial-prebuilt-intents-entities.md)
