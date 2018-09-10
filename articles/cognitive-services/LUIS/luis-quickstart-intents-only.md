---
title: Criar uma aplicação simples com duas intenções - Azure | Microsoft Docs
description: Saiba como criar uma aplicação LUIS simples com duas intenções e sem entidades para identificar expressões de utilizador neste início rápido.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 3f23ade2b0256c72c344e2a619227a79e3c79a47
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160120"
---
# <a name="tutorial-1-build-app-with-custom-domain"></a>Tutorial: 1. Criar aplicação com domínio personalizado
Neste tutorial, crie uma aplicação que demonstre como utilizar **intenções** para determinar a _intenção_ do utilizador com base na expressão (texto) enviada para a aplicação. Quando terminar, obterá um ponto final do LUIS em execução na cloud.

Esta aplicação é o tipo mais simples de aplicação LUIS, uma vez que não extrai dados das expressões. Apenas determina a intenção do utilizador da expressão.

<!-- green checkmark -->
> [!div class="checklist"]
> * Criar uma nova aplicação para um domínio de Recursos Humanos (RH) 
> * Adicionar a intenção GetJobInformation
> * Adicionar expressões de exemplo à intenção GetJobInformation 
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS
> * Adicionar a intenção ApplyForJob
> * Adicionar expressões de exemplo à intenção ApplyForJob 
> * Preparar, publicar e consultar novamente o ponto final 

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="purpose-of-the-app"></a>Objetivo da aplicação
Esta aplicação tem algumas intenções. A primeira intenção, **`GetJobInformation`**, identifica quando um utilizador quer informações sobre empregos disponíveis numa empresa. A segunda intenção, **`None`**, identifica todos os outros tipos de expressão. Mais adiante no início rápido, é adicionada uma terceira intenção, `ApplyForJob`. 

## <a name="create-a-new-app"></a>Criar uma nova aplicação
1. Inicie sessão no site do [LUIS](luis-reference-regions.md#luis-website). Certifique-se de que inicia sessão na [região](luis-reference-regions.md#publishing-regions) onde precisa que os pontos finais do LUIS sejam publicados.

2. No site do [LUIS](luis-reference-regions.md#luis-website), selecione **Create new app** (Criar nova aplicação).  

    [![](media/luis-quickstart-intents-only/app-list.png "Captura de ecrã da página As Minhas Aplicações")](media/luis-quickstart-intents-only/app-list.png#lightbox)

3. Na caixa de diálogo de pop-up, introduza o nome `HumanResources`. Esta aplicação abrange perguntas sobre o departamento de Recursos Humanos da sua empresa. Esse tipo de departamento lida com problemas relacionados com o emprego, como vagas na empresa que têm de ser preenchidas.

    ![A nova aplicação LUIS](./media/luis-quickstart-intents-only/create-app.png)

4. Quando esse processo terminar, a aplicação mostra a página **Intents** (Intenções) com a intenção **None** (Nenhuma). 

## <a name="create-getjobinformation-intention"></a>Criar a intenção GetJobInformation
1. Selecione **Create new intent** (Criar nova intenção). Introduza o nome da nova intenção `GetJobInformation`. Esta intenção é prever sempre que um utilizador quer informações sobre empregos na sua empresa.

    ![](media/luis-quickstart-intents-only/create-intent.png "Captura de ecrã da caixa de diálogo New intent (Nova intenção)")

    Ao criar uma intenção, está a criar uma categoria de informações que quer identificar. Atribuir um nome à categoria permite que qualquer outra aplicação que utilize os resultados da consulta do LUIS utilize esse nome de categoria para encontrar uma resposta adequada. O LUIS não responde a estas perguntas, apenas identifica o tipo de informação que está a ser pedida em linguagem natural. 

2. Adicione sete expressões a esta intenção, que espera que um utilizador peça, tais como:

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

3. Atualmente, a aplicação LUIS não tem expressões para a intenção **None** (Nenhuma). Precisa de expressões que a aplicação não responde. Não deixe em branco. Selecione **Intents** (Intenções) no painel esquerdo. 

4. Selecione a intenção **None** (Nenhuma). Adicione três expressões que o utilizador poderá introduzir, mas que não são relevantes para a sua aplicação. Se a aplicação for sobre as suas publicações de Empregos, algumas expressões **None** boas são:

    | Expressões de exemplo|
    |--|
    |Cães que ladram são irritantes|
    |Encomendar uma piza para mim|
    |Pinguins no oceano|

    Na aplicação de chamada do LUIS, como um chatbot, se o LUIS devolver a intenção **None** (Nenhuma) para uma expressão, o seu bot pode perguntar se o utilizador quer terminar a conversa. O chatbot também pode dar mais instruções para continuar a conversa se o utilizador não quiser terminá-la. 

## <a name="train-and-publish-the-app"></a>Preparar e publicar a aplicação

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-app-to-endpoint"></a>Publicar a aplicação no ponto final

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)] 

## <a name="query-endpoint-for-getjobinformation-intent"></a>Ponto final de consulta para a intenção GetJobInformation

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vá para o final do URL no endereço e introduza `I'm looking for a job with Natual Language Processing`. O último parâmetro de cadeia de consulta é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões no passo 4, pelo que é um bom teste e deve devolver a intenção `GetJobInformation` como a principal intenção de classificação. 

    ```
    {
      "query": "I'm looking for a job with Natual Language Processing",
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

## <a name="create-applyforjob-intention"></a>Criar a intenção ApplyForJob
Regresse ao separador do browser do site do LUIS e crie uma nova intenção para se candidatar a um emprego.

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

    [Prepare e publique](#train-and-publish-the-app) novamente. 

## <a name="query-endpoint-for-applyforjob-intent"></a>Ponto final de consulta para a intenção ApplyForJob

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Na nova janela do browser, introduza `Can I submit my resume for job 235986` no final do URL. 

    ```
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

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com apenas algumas intenções, identificou uma consulta em linguagem natural da mesma intenção, mas com uma ordem de palavras diferente. 

O resultado JSON identifica a intenção com a melhor classificação. Todas as classificações estão compreendidas entre 1 e 0, estando a melhor classificação próxima de 1. A classificação das intenções `GetJobInformation` e `None` está muito mais próxima de zero. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e localizar informações (não armazenadas no LUIS) para responder à pergunta ou terminar a conversa. Tratam-se de opções de programação para o bot ou a aplicação de chamada. O LUIS não permite isso. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Adicionar intenções e entidades pré-concebidas a esta aplicação](luis-tutorial-prebuilt-intents-entities.md)
