---
title: Tutorial para rever expressões de ponto final no Language Understanding (LUIS) - Azure | Microsoft Docs
description: Neste tutorial, saiba como rever as expressões de ponto final no domínio de Recursos Humanos (RH) do LUIS.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: luis
ms.topic: tutorial
ms.date: 07/03/2018
ms.author: v-geberr
ms.openlocfilehash: cd8374b3804594f96212dbe741f99ba22d33a4e8
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/12/2018
ms.locfileid: "38970511"
---
# <a name="tutorial-review-endpoint-utterances"></a>Tutorial: rever expressões de ponto final
Neste tutorial, melhore as predições da aplicação ao validar ou corrigir expressões recebidas através do ponto final de HTTP do LUIS. 

<!-- green checkmark -->
> [!div class="checklist"]
> * Compreender a revisão de expressões de ponto final 
> * Utilizar a aplicação LUIS para o domínio de Recursos Humanos (RH) 
> * Rever pronunciações de ponto final
> * Preparar e publicar a aplicação
> * Consultar o ponto final da aplicação para ver a resposta JSON de LUIS

Para este artigo, precisa de uma conta do [LUIS](luis-reference-regions.md#luis-website) gratuita para criar a sua aplicação LUIS.

## <a name="before-you-begin"></a>Antes de começar
Se não tiver a aplicação de Recursos Humanos do tutorial [sentimento](luis-quickstart-intent-and-sentiment-analysis.md), importe a aplicação do repositório [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-sentiment-HumanResources.json) do Github. Se utilizar este tutorial como uma aplicação nova e importada, também terá de preparar, publicar e, então, adicionar as expressões ao ponto final com um [script](https://github.com/Microsoft/LUIS-Samples/blob/master/examples/demo-upload-endpoint-utterances/endpoint.js) ou a partir do ponto final num browser. As expressões a adicionar são:

   [!code-nodejs[Node.js code showing endpoint utterances to add](~/samples-luis/examples/demo-upload-endpoint-utterances/endpoint.js?range=15-26)]

Se quiser manter a aplicação de Recursos Humanos original, clone a versão na página [Definições](luis-how-to-manage-versions.md#clone-a-version) e dê-lhe o nome `review`. A clonagem é uma excelente forma de utilizar várias funcionalidades do LUIS sem afetar a versão original. 

Se tiver todas as versões da aplicação, através da série de tutoriais, pode ficar surpreendido ao constatar que a lista para **Rever expressões de ponto final** não é alterada, com base na versão. Há um único conjunto de expressões para rever, independentemente da versão da expressão que está ativamente a editar ou da versão da aplicação que foi publicada no ponto final. 

## <a name="purpose-of-reviewing-endpoint-utterances"></a>Objetivo da revisão de expressões de ponto final
Este processo de revisão é outra maneira de o LUIS saber qual é o domínio da aplicação. O LUIS selecionou as expressões da lista de revisão. Esta lista é:

* Específica da aplicação.
* Destina-se a melhorar a precisão de predição da aplicação. 
* Deve ser revista periodicamente. 

Ao rever as expressões de ponto final, está a validar ou corrigir a intenção prevista da expressão. Além disso, identifica as entidades personalizadas que não foram previstas. 

## <a name="review-endpoint-utterances"></a>Rever pronunciações de ponto final

1. Certifique-se de que a aplicação de Recursos Humanos está na secção **Criar** do LUIS. Pode alterar para esta secção ao selecionar **Criar** na barra de menus superior direita. 

    [ ![Captura de ecrã da aplicação LUIS com o botão Criar realçado na barra de navegação superior direita](./media/luis-tutorial-review-endpoint-utterances/first-image.png)](./media/luis-tutorial-review-endpoint-utterances/first-image.png#lightbox)

1. Selecione **Rever expressões de ponto final** na navegação à esquerda. A lista está filtrada para a intenção **ApplyForJob**. 

    [ ![Captura de ecrã do botão para Rever expressões de ponto final na navegação à esquerda](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png)](./media/luis-tutorial-review-endpoint-utterances/entities-view-endpoint-utterances.png#lightbox)

2. Alterne a **Vista de entidades** para ver as entidades identificadas. 
    
    [ ![Captura de ecrã de Rever expressões de ponto final com o botão para alternar a Vista de entidades realçado](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png)](./media/luis-tutorial-review-endpoint-utterances/select-entities-view.png#lightbox)

    |Expressão|Intenção correta|Entidades em falta|
    |:--|:--|:--|
    |Estou à procura de uma tarefa com o Processamento de Linguagem Natural|GetJobInfo|Tarefa - "Processo de Linguagem Natural"|

    Esta expressão não está na intenção correta e tem uma classificação inferior a 50%. A intenção **ApplyForJob** tem 21 expressões, em comparação com as sete expressões na **GetJobInformation**. Juntamente com o alinhamento correto das expressões de ponto final, devem ser adicionadas mais expressões à intenção **GetJobInformation**. Fica como um exercício para ser concluído por conta própria. Cada intenção, exceto para a intenção**None** (Nenhuma), deverá ter aproximadamente o mesmo número de expressões de exemplo. A intenção **None** (Nenhuma) deve ter 10% do total de expressões na aplicação. 

    Quando estiver na **Vista de Tokens**, pode colocar o cursor sobre qualquer texto azul da expressão para ver o nome da entidade prevista. 

3. Para a intenção `I'm looking for a job with Natual Language Processing`, selecione a intenção correta, **GetJobInformation** na coluna **Intenção alinhada**. 

    [ ![Captura de ecrã de Rever expressões de ponto final que alinham a expressão à intenção](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png)](./media/luis-tutorial-review-endpoint-utterances/align-intent-1.png#lightbox)

4. Na mesma expressão, a entidade de `Natural Language Processing` é keyPhrase. Deve ser uma entidade **Tarefa** em vez disso. Selecione `Natural Language Processing` e, em seguida, selecione a entidade **Tarefa** na lista.

    [ ![Captura de ecrã de Rever expressões de ponto final que identificam a entidade na expressão](./media/luis-tutorial-review-endpoint-utterances/label-entity.png)](./media/luis-tutorial-review-endpoint-utterances/label-entity.png#lightbox)

5. Na mesma linha, selecione a marca de verificação com um círculo à volta na coluna **Adicionar a intenção alinhada**. 

    [ ![Captura de ecrã de finalização do alinhamento de expressão na intenção](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png)](./media/luis-tutorial-review-endpoint-utterances/align-utterance.png#lightbox)

    Esta ação move a expressão de **Rever expressões de ponto final** para a intenção **GetJobInformation**. A expressão de ponto final é agora uma expressão de exemplo dessa intenção. 

6. Reveja as restantes expressões nesta intenção, identificando as expressões e corrigindo a **Intenção alinhada**, se as expressões estiverem incorretas.

7. Quando todas as expressões estiverem corretas, selecione a caixa de verificação em cada linha e, em seguida, selecione **Adicionar seleção** para alinhar as expressões corretamente. 

    [ ![Captura de ecrã de finalização das restantes expressões com a intenção alinhada](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png)](./media/luis-tutorial-review-endpoint-utterances/finalize-utterance-alignment.png#lightbox)

8. A lista já não deve ter essas expressões. Se surgirem mais expressões, continue a trabalhar na lista, corrigindo as intenções e identificando quaisquer entidades em falta, até que a lista esteja vazia. Selecione a intenção seguinte na lista Filtro e, em seguida, continue a corrigir as expressões e a identificar as entidades. Lembre-se de que o último passo de cada intenção é selecionar **Adicionar a intenção alinhada** na linha da expressão ou marcar a caixa ao lado de cada intenção e selecionar **Adicionar seleção**, acima da tabela. 

    Esta aplicação é muito pequena. O processo de revisão demora apenas alguns minutos.

## <a name="add-new-job-name-to-phrase-list"></a>Adicionar o nome da nova tarefa à lista de expressões
Mantenha a lista de expressões atualizada com quaisquer nomes de tarefas recentemente detetadas. 

1. Selecione **Listas de expressões** na navegação à esquerda.

2. Selecione a lista de expressões **Tarefas**.

3. Adicione `Natural Language Processing` como um valor e selecione **Guardar**. 

## <a name="train-the-luis-app"></a>Preparar a aplicação LUIS
O LUIS desconhece as alterações até estar preparado. 

1. No lado direito superior do site do LUIS, selecione o botão **Train** (Preparar).

2. A preparação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicar a aplicação para obter o URL de ponto final
Para obter o modelo atualizado da aplicação LUIS num chatbot ou noutra aplicação, tem de publicar a aplicação. 

1. No lado direito superior do site do LUIS, selecione o botão **Publish** (Publicar). 

2. Se tiver importado esta aplicação, tem de selecionar a **Análise de sentimentos**. 

3. Selecione o bloco Production (Produção) e o botão **Publish** (Publicar).

4. A publicação está concluída quando for apresentada a barra de estado verde na parte superior do site a confirmar o êxito.

## <a name="query-the-endpoint-with-an-utterance"></a>Consultar o ponto final com uma expressão
Experimente uma expressão próxima da expressão corrigida. 

1. Na página **Publish** (Publicar), selecione a ligação do **ponto final** na parte inferior da página. Esta ação abre outra janela de browser com o URL de ponto final na barra de endereço. 

2. Vá para o final do URL no endereço e introduza `Are there any natural language processing jobs in my department right now?`. O último parâmetro querystring é `q`, a expressão **query**. 

```JSON
{
  "query": "are there any natural language processing jobs in my department right now?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.9247605
  },
  "intents": [
    {
      "intent": "GetJobInformation",
      "score": 0.9247605
    },
    {
      "intent": "ApplyForJob",
      "score": 0.129989788
    },
    {
      "intent": "FindForm",
      "score": 0.006438211
    },
    {
      "intent": "EmployeeFeedback",
      "score": 0.00408575451
    },
    {
      "intent": "Utilities.StartOver",
      "score": 0.00194211153
    },
    {
      "intent": "None",
      "score": 0.00166400627
    },
    {
      "intent": "Utilities.Help",
      "score": 0.00118593348
    },
    {
      "intent": "MoveEmployee",
      "score": 0.0007885918
    },
    {
      "intent": "Utilities.Cancel",
      "score": 0.0006373631
    },
    {
      "intent": "Utilities.Stop",
      "score": 0.0005980781
    },
    {
      "intent": "Utilities.Confirm",
      "score": 3.719905E-05
    }
  ],
  "entities": [
    {
      "entity": "right now",
      "type": "builtin.datetimeV2.datetime",
      "startIndex": 64,
      "endIndex": 72,
      "resolution": {
        "values": [
          {
            "timex": "PRESENT_REF",
            "type": "datetime",
            "value": "2018-07-05 15:23:18"
          }
        ]
      }
    },
    {
      "entity": "natural language processing",
      "type": "Job",
      "startIndex": 14,
      "endIndex": 40,
      "score": 0.9869922
    },
    {
      "entity": "natural language processing jobs",
      "type": "builtin.keyPhrase",
      "startIndex": 14,
      "endIndex": 45
    },
    {
      "entity": "department",
      "type": "builtin.keyPhrase",
      "startIndex": 53,
      "endIndex": 62
    }
  ],
  "sentimentAnalysis": {
    "label": "positive",
    "score": 0.8251864
  }
}
}
```

A intenção correta foi prevista com uma classificação alta, e a entidade **Tarefa** é detetada como `natural language processing`. 

## <a name="can-reviewing-be-replaced-by-adding-more-utterances"></a>A revisão pode ser substituída ao adicionar mais expressões? 
Pode questionar-se sobre o motivo pelo qual não deve adicionar mais expressões de exemplo. Qual é o objetivo da revisão de expressões de ponto final? Numa aplicação LUIS do mundo real, as expressões de ponto final são provenientes de utilizadores com uma escolha e disposição de palavras que ainda não utilizou. Se tivesse utilizado a mesma escolha e disposição de palavras, a predição original teria uma percentagem mais elevada. 

## <a name="why-is-the-top-intent-on-the-utterance-list"></a>Por que motivo a intenção principal está na lista de expressões? 
Algumas expressões de ponto final terão uma percentagem elevada na lista de revisão. Ainda tem de rever e validar essas expressões. Estão na lista porque a intenção mais alta seguinte tinha uma classificação demasiado próxima da classificação da intenção principal. 

## <a name="what-has-this-tutorial-accomplished"></a>O que conseguiu este tutorial?
A precisão de predição desta aplicação aumentou ao rever as expressões do ponto final. 

## <a name="clean-up-resources"></a>Limpar recursos
Quando já não precisar, elimine a aplicação LUIS. Selecione **As minhas aplicações** no menu do canto superior esquerdo. Selecione as reticências **…** à direita do nome da aplicação na lista de aplicações e selecione **Eliminar**. Na caixa de diálogo de pop-up **Delete app?** (Eliminar aplicação?), selecione **OK**.

## <a name="next-steps"></a>Passos seguintes

> [!div class="nextstepaction"]
> [Saiba como utilizar padrões](luis-tutorial-pattern.md)
