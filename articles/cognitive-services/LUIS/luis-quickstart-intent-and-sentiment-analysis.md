---
title: Tutorial para criar uma aplicação LUIS que devolva análise de sentimentos - Azure | Microsoft Docs
description: Neste tutorial, aprenda a adicionar a análise de sentimentos à sua aplicação LUIS para analisar expressões para sentimentos positivos, negativos e neutros.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/25/2018
ms.author: v-geberr
ms.openlocfilehash: 1a48810287c1639910db8e39af2da61d836b2988
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/02/2018
ms.locfileid: "37340938"
---
# <a name="tutorial-8--add-sentiment-analysis"></a>Tutorial: 8.  Adicionar análise de sentimentos
Neste tutorial, vai criar uma aplicação que demonstra como extrair sentimentos positivos, negativos e neutros de expressões.

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender a análise de sentimentos
> * Utilizar a aplicação LUIS no domínio de Recursos Humanos (RH) 
> * Adicionar análise de sentimentos
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS 

Para este artigo, precisa de uma conta do [LUIS](luis-reference-regions.md#luis-website) gratuita para criar a sua aplicação LUIS.

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de Recursos Humanos do tutorial [entidade de keyPhrase pré-concebida](luis-quickstart-intent-and-key-phrase.md), [importe](create-new-app.md#import-new-app) o JSON para uma nova aplicação no site do [LUIS](luis-reference-regions.md#luis-website). A aplicação a importar está no repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-keyphrase-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `sentiment`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

## <a name="sentiment-analysis"></a>Análise de sentimentos
A análise de sentimentos é a capacidade de determinar se a expressão de um utilizador é positiva, negativa ou neutra. 

As expressões seguintes mostram exemplos de sentimentos:

|Sentimento|Classificação|Expressão|
|:--|:--|:--|
|positiva|0,91 |John W. Smith fez um excelente trabalho na apresentação em Paris.|
|positiva|0,84 |jill-jones@mycompany.com fez um excelente trabalho no pitch de vendas da Parker.|

A análise de sentimentos é uma definição da aplicação que se aplica a cada expressão. Não precisa de encontrar as palavras que indicam o sentimento na expressão e rotulá-las porque a análise de sentimento aplica-se a toda a expressão. 

## <a name="add-employeefeedback-intent"></a>Adicionar intenção EmployeeFeedback 
Adicione uma nova intenção para capturar os comentários dos colaboradores membros da empresa. 

1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-first-image.png#lightbox)

2. Selecione **Create new intent** (Criar nova intenção).

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent.png#lightbox)

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

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
A LUIS desconhece a nova intenção e as respetivas expressões de exemplo até estar preparada. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Captura de ecrã do botão Preparar realçado](./media/luis-quickstart-intent-and-sentiment-analysis/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Captura de ecrã da barra de notificação de êxito da Preparação ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-trained-inline.png)

## <a name="configure-app-to-include-sentiment-analysis"></a>Configurar a aplicação para incluir análise de sentimentos
Configurar análise de sentimentos na página **Publicar**. 

1. Selecione **Publicar** no painel de navegação superior direito.

    ![Captura de ecrã da página Intenção com o botão Publicar expandido ](./media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-button-in-top-nav-highlighted.png)

2. Selecione **Ativar Análise de Sentimentos**. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    [![](media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-to-production-expanded.png "Captura de ecrã da página Publicar com o botão Publicar no bloco de produção realçado")](media/luis-quickstart-intent-and-sentiment-analysis/hr-publish-to-production-expanded.png#lightbox)

4. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-an-utterance"></a>Consultar o ponto final com uma expressão

1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    ![Captura de ecrã da página Publicar com o URL de ponto final realçado](media/luis-quickstart-intent-and-sentiment-analysis/hr-endpoint-url-inline.png)

2. Vá para o final do URL no endereço e introduza `Jill Jones work with the media team on the public portal was amazing`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões etiquetadas, pelo que é um bom teste e deve devolver a intenção `EmployeeFeedback` com a análise de sentimentos extraída.

```
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

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação, com a análise de sentimentos ativada, identificou uma intenção de consulta de linguagem natural e devolveu os dados extraídos, incluindo o sentimento geral como uma pontuação. 

O chatbot tem agora informações suficientes para determinar o passo seguinte na conversação. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os dados de sentimento da expressão para efetuar o passo seguinte. O LUIS não faz esse trabalho programático para o bot ou a aplicação de chamada. O LUIS apenas determina qual é a intenção do utilizador. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Selecione **As minhas aplicações** no menu do canto superior esquerdo. Selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"] 
> [Chamar a API de ponto final do LUIS com C#](luis-get-started-cs-get-intent.md) 

