---
title: Utilize o batch para testes para melhorar as predições LUIS | Microsoft Docs
titleSuffix: Azure
description: Teste de batch de carga, reveja os resultados e melhorar predições LUIS com as alterações.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 5788f17f2724a0354a1db506971c2343c1800f01
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266401"
---
# <a name="use-batch-testing-to-find-prediction-accuracy-issues"></a>Utilize o batch para testes encontrar problemas de precisão de predição

Este tutorial demonstra como utilizar o teste de batch para localizar utterance problemas de predição.  

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Crie um ficheiro de teste do batch 
* Executar um teste de batch
* Reveja os resultados do teste
* Corrija os erros para pendentes
* Foram o lote

## <a name="prerequisites"></a>Pré-requisitos

> [!div class="checklist"]
> * Para este artigo, terá também uma [LUIS][LUIS] conta para criar a sua aplicação LUIS.

> [!Tip]
> Se ainda não tiver uma subscrição, pode registar-se para uma [conta gratuita](https://azure.microsoft.com/free/).

## <a name="create-new-app"></a>Criar nova aplicação
Este artigo utiliza o domínio prebuilt HomeAutomation. O domínio prebuilt tem utterances para controlar os dispositivos de HomeAutomation como lights, entidades e pendentes. Criar a aplicação, adicione o domínio, formação e publicar.

1. No [LUIS] Web site, criar uma nova aplicação, selecionando **criar nova aplicação** no **MyApps** página. 

    ![Criar nova aplicação](./media/luis-tutorial-batch-testing/create-app-1.png)

2. Introduza o nome `Batchtest-HomeAutomation` na caixa de diálogo.

    ![Introduza o nome da aplicação](./media/luis-tutorial-batch-testing/create-app-2.png)

3. Selecione **domínios Prebuilt** no canto inferior esquerdo. 

    ![Selecione o domínio Prebuilt](./media/luis-tutorial-batch-testing/prebuilt-domain-1.png)

4. Selecione **Adicionar domínio** para HomeAutomation.

    ![Adicionar domínio HomeAutomation](./media/luis-tutorial-batch-testing/prebuilt-domain-2.png)

5. Selecione **formação** na barra de navegação direito superior.

    ![Selecione o botão de formação](./media/luis-tutorial-batch-testing/train-button.png)

## <a name="batch-test-criteria"></a>Critérios de teste do batch
Teste do batch, pode testar até 1000 utterances cada vez. O lote não deve ter os duplicados. [Exportar](create-new-app.md#export-app) a aplicação para ver a lista de utterances atuais.  

A estratégia de teste para LUIS utiliza três conjuntos de dados distintos: modelo utterances, utterances de teste do batch e utterances de ponto final. Para este tutorial, certifique-se que não estiver a utilizar o utterances a partir de qualquer um dos utterances de modelo (adicionadas ao objetivo) ou utterances de ponto final. 

Não utilize qualquer um dos utterances já na aplicação para o teste de lote:

```
'breezeway on please',
'change temperature to seventy two degrees',
'coffee bar on please',
'decrease temperature for me please',
'dim kitchen lights to 25 .',
'fish pond off please',
'fish pond on please',
'illuminate please',
'living room lamp on please',
'living room lamps off please',
'lock the doors for me please',
'lower your volume',
'make camera 1 off please',
'make some coffee',
'play dvd',
'set lights bright',
'set lights concentrate',
'set lights out bedroom',
'shut down my work computer',
'silence the phone',
'snap switch fan fifty percent',
'start master bedroom light .',
'theater on please',
'turn dimmer off',
'turn off ac please',
'turn off foyer lights',
'turn off living room light',
'turn off staircase',
'turn off venice lamp',
'turn on bathroom heater',
'turn on external speaker',
'turn on my bedroom lights .',
'turn on the furnace room lights',
'turn on the internet in my bedroom please',
'turn on thermostat please',
'turn the fan to high',
'turn thermostat on 70 .' 
```

## <a name="create-a-batch-to-test-intent-prediction-accuracy"></a>Criar um lote para testar a exatidão da previsão intenção
1. Criar `homeauto-batch-1.json` num editor de texto, tal como [VSCode](https://code.visualstudio.com/). 

2. Adicionar utterances com o **intenção** pretende que o previsto no teste. Para este tutorial, para torná-lo simples, execute utterances na `HomeAutomation.TurnOn` e `HomeAutomation.TurnOff` e mudar o `on` e `off` o texto as utterances. Para o `None` intenção, adicionar alguns utterances que não fazem parte do [domínio](luis-glossary.md#domain) área (assunto). 

    Para compreender a forma como os resultados do teste batch correlacionar para o JSON do batch, adicione apenas seis pendentes.

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": []
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": []
        }
    ]
    ```

## <a name="run-the-batch"></a>A executar o batch
1. Selecione **teste** na barra de navegação superior. 

    ![Selecione o teste na barra de navegação](./media/luis-tutorial-batch-testing/test-1.png)

2. Selecione **painel de teste do Batch** no painel do lado direito. 

    ![Selecione o painel de teste do Batch](./media/luis-tutorial-batch-testing/test-2.png)

3. Selecione **conjunto de dados de importação**.

    ![Selecione o conjunto de dados de importação](./media/luis-tutorial-batch-testing/test-3.png)

4. Escolha a localização do sistema de ficheiros do `homeauto-batch-1.json` ficheiro.

5. Nome do conjunto de dados `set 1`.

    ![Selecionar ficheiro](./media/luis-tutorial-batch-testing/test-4.png)

6. Selecionar o botão **Executar**. Aguarde até que o teste é concluído.

    ![Selecionar](./media/luis-tutorial-batch-testing/test-5.png)

7. Selecione **ver resultados**.

    ![Ver os resultados](./media/luis-tutorial-batch-testing/test-6.png)

8. Reveja os resultados no gráfico e da legenda.

    ![Resultados do batch](./media/luis-tutorial-batch-testing/batch-result-1.png)

## <a name="review-batch-results"></a>Reveja os resultados do batch
Os resultados do batch estão em duas secções. A secção superior contém o gráfico e a legenda. A secção de inferior apresenta utterances Quando seleciona um nome de área do gráfico.

Os erros são indicados por cor vermelho. O gráfico é quatro secções, com dois as secções apresentadas vermelho. **Estas são as secções concentrar-se no**. 

Canto superior direito secção indica o teste incorretamente prever a existência de um objetivo ou entidade. Secção na parte inferior esquerda indica que o teste de prever a ausência de um objetivo ou entidade incorretamente.

### <a name="homeautomationturnoff-test-results"></a>Resultados do teste HomeAutomation.TurnOff
Na legenda, selecione o `HomeAutomation.TurnOff` intenção. Tem um ícone verde com êxito para a esquerda do nome na legenda. Não existirem erros para este objetivo. 

![Resultados do batch](./media/luis-tutorial-batch-testing/batch-result-1.png)

### <a name="homeautomationturnon-and-none-intents-have-errors"></a>HomeAutomation.TurnOn e nenhum pendentes têm erros
Os outros dois pendentes tem erros, o que significa que serão as predições de teste não correspondiam as expectativas do ficheiro batch. Selecione o `None` intenção na legenda para rever o primeiro erro. 

![Nenhum intenção](./media/luis-tutorial-batch-testing/none-intent-failures.png)

Falhas de aparecem no gráfico nas secções vermelhos: **falsos positivos** e **falso negativo**. Selecione o **falso negativo** nome de secção do gráfico para ver os utterances falhadas abaixo do gráfico. 

![Falsas falhas negativos](./media/luis-tutorial-batch-testing/none-intent-false-negative.png)

Utterance falhar, `help` era esperado como um `None` intenção, mas o teste de prever `HomeAutomation.TurnOn` intenção.  

Existem duas falhas, um em HomeAutomation.TurnOn e um em nenhum. Ambos foram causadas pelo utterance `help` porque não conseguiu fornecer a expectativa em nenhum e foi uma correspondência para o objetivo de HomeAutomation.TurnOn inesperada. 

Para determinar o motivo de `None` utterances estão a falhar, reveja os utterances atualmente nos `None`. 

## <a name="review-none-intents-utterances"></a>Revisão da intenção nenhum utterances

1. Fechar o **teste** painel selecionando o **teste** botão na barra de navegação superior. 

2. Selecione **criar** partir do painel de navegação superior. 

3. Selecione **nenhum** intenção da lista de pendentes.

4. Selecione o controlo + E para ver a vista de token dos utterances 
    
    |Nenhum intenção 's utterances|Classificação de predição|
    |--|--|
    |"diminuir temperatura para me permitir."|0.44|
    |"dim lights cozinha como 25."|0.43|
    |"reduzir o volume"|0.46|
    |"ativar na internet no meu bedroom"|0.28|

## <a name="fix-none-intents-utterances"></a>Corrigir nenhum intenção do utterances
    
Qualquer utterances no `None` deveria estar fora do domínio de aplicação. Estes utterances são relativamente à HomeAutomation, pelo que estão a ser a intenção errada. 

LUIS também oferece o utterances inferior a 50% (<.50) pontuação de predição. Se observar utterances nas outros dois pendentes, verá muito superiores pontuações de predição. Quando LUIS tem baixas pontuações das classificações para utterances de exemplo, o que é um bom indicador de utterances são confusas para LUIS entre a intenção atual e outras pendentes. 

Para corrigir a aplicação, utterances atualmente no `None` intenção tem de ser movido para o objetivo correto e o `None` intenção tem pendentes novo, adequados. 

Três utterances no `None` intenção destinam-se para reduzir as definições do dispositivo de automatização. Utilize palavras como `dim`, `lower`, ou `decrease`. O utterance Quarta pergunta ativar na internet. Uma vez que todos os quatro utterances sobre ativando ou alterar o grau de energia para um dispositivo, deve ser movidas para o `HomeAutomation.TurnOn` intenção. 

Esta é apenas uma solução. Também pode criar um novo objetivo `ChangeSetting` mover utterances utilizando Dim. do, reduzir e diminuir para esse objetivo de novo. 

## <a name="fix-the-app-based-on-batch-results"></a>Corrija a aplicação com base nos resultados de batch
Mover os quatro utterances para o `HomeAutomation.TurnOn` intenção. 

1. Selecione a caixa de verificação acima lista utterance para que todos os utterances estão selecionadas. 

2. No **reatribuir intenção** pendente, selecione `HomeAutomation.TurnOn`. 

    ![Mover utterances](./media/luis-tutorial-batch-testing/move-utterances.png)

    Depois de quatro utterances são reatribuídas, o utterance lista para o `None` intenção está vazia.

3. Adicione quatro pendentes novo para a intenção nenhum:

    ```
    "fish"
    "dogs"
    "beer"
    "pizza"
    ```

    Estes utterances sem dúvida estão fora do domínio de HomeAutomation. Como introduzir cada utterance, veja a classificação para a mesma. A classificação pode ser baixa ou mesmo muito baixo (com uma caixa vermelha volta). Depois de preparar a aplicação, no passo 8, a classificação será muito superior. 

7. Remover quaisquer etiquetas, selecionando a etiqueta azul no utterance e selecione **remover etiqueta**.

8. Selecione **formação** na barra de navegação direito superior. A classificação de cada utterance é muito superior. Todas as pontuações das classificações para o `None` intenção deverá estar acima.80 agora. 

## <a name="verify-the-fix-worked"></a>Certifique-se de que a correção trabalhado
Para verificar que o utterances no teste batch estão corretamente prever para o **nenhum** intenção, execute novamente o teste de batch.

1. Selecione **teste** na barra de navegação superior. 

2. Selecione **painel de teste do Batch** no painel do lado direito. 

3. Selecione as reticências (…) à direita do nome do batch e selecione **executar o conjunto de dados**. Aguarde até que o teste de batch é efetuado.

    ![Execute o conjunto de dados](./media/luis-tutorial-batch-testing/run-dataset.png)

4. Selecione **ver resultados**. Os pendentes devem ter ícones verdes à esquerda dos nomes de intenção. Com o filtro adequado, como o `HomeAutomation.Turnoff` intenção, selecione a verde ponto no painel direito superior mais próximo para o meio do gráfico. O nome do utterance aparece na tabela abaixo do gráfico. A pontuação dos `breezeway off please` for muito baixa. Uma atividade opcional é adicionar mais utterances a intenção para aumentar este modelo de pontuação. 

    ![Execute o conjunto de dados](./media/luis-tutorial-batch-testing/turnoff-low-score.png)

<!--
    The Entities section of the legend may have errors. That is the next thing to fix.

## Create a batch to test entity detection
1. Create `homeauto-batch-2.json` in a text editor such as [VSCode](https://code.visualstudio.com/). 

2. Utterances have entities identified with `startPos` and `endPost`. These two elements identify the entity before [tokenization](luis-glossary.md#token), which happens in some [cultures](luis-supported-languages.md#tokenization) in LUIS. If you plan to batch test in a tokenized culture, learn how to [extract](luis-concept-data-extraction.md#tokenized-entity-returned) the non-tokenized entities.

    Copy the following JSON into the file:

    ```JSON
    [
        {
          "text": "lobby on please",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 4
            }
          ]
        },
        {
          "text": "change temperature to seventy one degrees",
          "intent": "HomeAutomation.TurnOn",
          "entities": [
            {
              "entity": "HomeAutomation.Operation",
              "startPos": 7,
              "endPos": 17
            }
          ]
        },
        {
          "text": "where is my pizza",
          "intent": "None",
          "entities": []
        },
        {
          "text": "help",
          "intent": "None",
          "entities": []
        },
        {
          "text": "breezeway off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Room",
              "startPos": 0,
              "endPos": 9
            }
          ]
        },
        {
          "text": "coffee bar off please",
          "intent": "HomeAutomation.TurnOff",
          "entities": [
            {
              "entity": "HomeAutomation.Device",
              "startPos": 0,
              "endPos": 10
            }
          ]
        }
      ]
    ```

3. Import the batch file, following the [same instructions](#run-the-batch) as the first import, and name the dataset `set 2`. Run the test.

## Possible entity errors
Since the intents in the right-side filter of the test panel still pass the test, this section focuses on correct entity identification. 

Entity testing is diferrent than intents. An utterance will have only one top scoring intent, but it may have several entities. An utterance's entity may be correctly identified, may be incorrectly identified as an entity other than the one in the batch test, may overlap with other entities, or not identified at all. 

## Review entity errors
1. Select `HomeAutomation.Device` in the filter panel. The chart changes to show a single false positive and several true negatives. 

2. Select the False positive section name. The utterance for this chart point is displayed below the chart. The labeled intent and the predicted intent are the same, which is consistent with the test -- the intent prediction is correct. 

    The issue is that the HomeAutomation.Device was detected but the batch expected HomeAutomation.Room for the utterance "coffee bar off please". `Coffee bar` could be a room or a device, depending on the environment and context. As the model designer, you can either enforce the selection as `HomeAutomation.Room` or change the batch file to use `HomeAutomation.Device`. 

    If you want to reinforce that coffee bar is a room, you nee to add an utterances to LUIS that help LUIS decide a coffee bar is a room. 

    The most direct route is to add the utterance to the intent but that to add the utterance for every entity detection error is not the machine-learned solution. Another fix would be to add an utterance with `coffee bar`.

## Add utterance to help extract entity
1. Select the **Test** button on the top navigation to close the batch test panel.

2. On the `HomeAutomation.TurnOn` intent, add the utterance, `turn coffee bar on please`. The uttterance should have all three entities detected after you select enter. 

3. Select **Train** on the top navigation panel. Wait until training completes successfully.

3. Select **Test** on the top navigation panel to open the Batch testing pane again. 

4. If the list of datasets is not visible, select **Back to list**. Select the three dots (...) at the end of `Set 2` and select `Run Dataset`. Wait for the test to complete.

5. Select **See results** to review the test results.

6. 
-->
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre utterances de exemplo](luis-how-to-add-example-utterances.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions