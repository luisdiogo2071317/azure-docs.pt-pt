---
title: Tutorial para criar uma aplicação LUIS para obter correspondência de dados com a expressão regular - Azure | Microsoft Docs
description: Neste tutorial, saiba como criar uma aplicação LUIS simples com intenções e uma entidade de expressão regular para extrair dados.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 06/18/2018
ms.author: v-geberr
ms.openlocfilehash: 317d5b37b90f6c436e3cecf0486d587f54960598
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36316547"
---
# <a name="tutorial-use-regular-expression-entity"></a>Tutorial: Utilizar a entidade de expressão regular
Neste tutorial, vai criar uma aplicação que demonstra como extrair dados formatados de forma consistente a partir de uma expressão com a entidade **Expressão Regular**.


<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender entidades de expressão regular 
> * Utilizar uma aplicação LUIS para um domínio de Recursos Humanos (RH) com a intenção FindForm
> * Adicionar a entidade de expressão regular para extrair o número de Formulário da expressão
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS

Para este artigo, precisa de uma conta do [LUIS](luis-reference-regions.md#luis-website) gratuita para criar a sua aplicação LUIS.

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de Recursos Humanos do tutorial de [domínio personalizado](luis-tutorial-prebuilt-intents-entities.md) das entidades pré-concebidas, [importe](create-new-app.md#import-new-app) o JSON para uma nova aplicação no site do [LUIS](luis-reference-regions.md#luis-website) a partir do repositório do Github [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-prebuilts-HumanResources.json).

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `regex`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 


## <a name="purpose-of-the-regular-expression-entity"></a>Objetivo da entidade de expressão regular
O objetivo de uma entidade é obter dados importantes contidos na expressão. A utilização da aplicação da entidade de expressão regular é extrair os números de Formulário de Recursos Humanos (RH) formatados de uma expressão. Não se trata de aprendizagem automática. 

As expressões de exemplo simples incluem:

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
```

As versões abreviadas ou de gíria das expressões incluem:

```
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
A entidade de expressão regular para corresponder ao número de formulário é `hrf-[0-9]{6}`. Esta expressão regular corresponde aos carateres literais `hrf -`, mas ignora as maiúsculas e minúsculas, e as variantes culturais. Corresponde aos dígitos 0 a 9, para exatamente 6 dígitos.

HRF representa o formulário de recursos humanos.

### <a name="tokenization-with-hyphens"></a>Atomização com hífenes
O LUIS automatiza a expressão quando esta é adicionada a uma intenção. A atomização para estas expressões adiciona espaços antes e depois do hífen, `Where is HRF - 123456?`. A expressão regular é aplicada à expressão no respetivo formato não processado, antes de ser atomizada. Uma vez que é aplicada ao formato _não processado_, a expressão regular não tem de lidar com limites de palavras. 


## <a name="add-findform-intent"></a>Adicionar a intenção FindForm

1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-quickstart-intents-regex-entity/first-image.png)](./media/luis-quickstart-intents-regex-entity/first-image.png#lightbox)

2. Selecione **Criar nova intenção**. 

    [ ![Captura de ecrã da página Intenções com o botão Criar nova intenção realçado](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png) ](./media/luis-quickstart-intents-regex-entity/create-new-intent-button.png#lightbox)

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

## <a name="create-a-hrf-number-regular-expression-entity"></a>Criar uma entidade de expressão regular de número HRF 
Crie uma entidade de expressão regular para indicar ao LUIS o que é um formato de número HRF nos passos seguintes:

1. Selecione **Entidades** no painel esquerdo.

2. Selecione o botão **Criar nova entidade** na página Entidades. 

    [![Captura de ecrã da página Entidades com o botão Criar nova entidade realçado](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png)](./media/luis-quickstart-intents-regex-entity/create-new-entity-1.png#lightbox)

3. Na caixa de diálogo de pop-up, introduza o novo nome de entidade `HRF-number`, selecione **RegEx** como tipo de entidade, introduza `hrf-[0-9]{6}` como Regex e, em seguida, selecione **Concluído**.

    ![Captura de ecrã da definição Propriedades da nova entidade da caixa de diálogo de pop-up](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Selecione **Intenções** e, em seguida, a intenção **FindForm** para ver a expressão regular etiquetada nas expressões. 

    [![Captura de ecrã da expressão Etiqueta com o padrão de entidade e regex existente](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Uma vez que a entidade não é de aprendizagem automática, a etiqueta é aplicada às expressões e apresentada no site do LUIS assim que for criada.

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
Uma entidade de expressão regular não requer preparação, mas a nova intenção e expressões requerem. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

    ![Imagem do botão de preparação](./media/luis-quickstart-intents-regex-entity/train-button.png)

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

    ![Imagem da barra de notificação de êxito](./media/luis-quickstart-intents-regex-entity/trained.png)

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter uma predição do LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publicar**. 

    ![Captura de ecrã de FindKnowledgeBase com o botão Publicar na navegação superior realçado](./media/luis-quickstart-intents-regex-entity/publish-button.png)

2. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

    ![Captura de ecrã da página Publicar com o botão Publicar no bloco de produção realçado](./media/luis-quickstart-intents-regex-entity/publish-to-production.png)

3. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consultar o ponto final com uma expressão diferente
1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

    ![Captura de ecrã da página Publicar com o URL de ponto final realçado](./media/luis-quickstart-intents-regex-entity/publish-select-endpoint.png)

2. Vá para o final do URL no endereço e introduza `When were HRF-123456 and hrf-234567 published?`. O último parâmetro querystring é `q`, a expressão **query**. Esta expressão não é igual a qualquer uma das expressões etiquetadas, pelo que é um bom teste e deve devolver a intenção `FindForm` com os dois números de formulário de `HRF-123456` e `hrf-234567`.

    ```
    {
      "query": "When were HRF-123456 and hrf-234567 published?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.970179737
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.970179737
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0131893409
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.00364777143
        },
        {
          "intent": "GetJobInformation",
          "score": 0.0024568392
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00173760345
        },
        {
          "intent": "None",
          "score": 0.00173070864
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00130692765
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00130328839
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0006671795
        }
      ],
      "entities": [
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

## <a name="what-has-this-luis-app-accomplished"></a>O que conseguiu esta aplicação LUIS?
Esta aplicação identificou a intenção e devolveu os dados extraídos. 

O chatbot tem agora informações suficientes para determinar a ação principal, `FindForm`, e saber que números de formulário foram incluídos na pesquisa. 

## <a name="where-is-this-luis-data-used"></a>Onde são utilizados estes dados do LUIS? 
O LUIS concluiu este pedido. A aplicação de chamada, como um chatbot, pode utilizar o resultado topScoringIntent e os números de formulário, e procurar uma API de terceiros. O LUIS não permite isso. O LUIS apenas determina a intenção do utilizador e extrai os dados sobre essa intenção. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Para tal, selecione o menu de três pontos (…) à direita do nome da aplicação na lista de aplicações e selecione **Delete** (Eliminar). Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre a entidade KeyPhrase](luis-quickstart-intent-and-key-phrase.md)

