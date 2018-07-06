---
title: Usar o teste de batch para melhorar as previsões de LUIS | Documentos da Microsoft
titleSuffix: Azure
description: Lote de teste de carga, reveja os resultados e melhorar as previsões de LUIS com alterações.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/19/2018
ms.author: v-geberr
ms.openlocfilehash: 4a5ace10c171d17235051c5bd666526318829fd7
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867346"
---
# <a name="use-batch-testing-to-find-prediction-accuracy-issues"></a>Usar o teste de batch para localizar problemas de precisão de previsão

Este tutorial demonstra como usar o teste de batch para localizar problemas de predição de expressão.  

Neste tutorial, ficará a saber como:

> [!div class="checklist"]
* Crie um ficheiro de teste do batch 
* Executar um teste de batch
* Rever os resultados de teste
* Corrija os erros para intenções
* Testar novamente o batch

## <a name="prerequisites"></a>Pré-requisitos

> [!div class="checklist"]
> * Neste artigo, também tem um [LUIS][LUIS] conta para criar a sua aplicação LUIS.

> [!Tip]
> Se ainda não tiver uma subscrição, pode se registrar para uma [conta gratuita](https://azure.microsoft.com/free/).

## <a name="create-new-app"></a>Criar nova aplicação
Este artigo utiliza o domínio pré-criado HomeAutomation. O domínio pré-criado tem intenções, entidades e expressões com para controlar os dispositivos de HomeAutomation como luzes. Crie a aplicação, adicionar o domínio, formar e publicar.

1. Na [LUIS] Web site, criar uma nova aplicação ao selecionar **criar nova aplicação** no **MyApps** página. 

    ![Criar nova aplicação](./media/luis-tutorial-batch-testing/create-app-1.png)

2. Introduza o nome `Batchtest-HomeAutomation` na caixa de diálogo.

    ![Introduza o nome da aplicação](./media/luis-tutorial-batch-testing/create-app-2.png)

3. Selecione **domínios pré-concebidos** no canto inferior esquerdo. 

    ![Selecione o domínio pré-criado](./media/luis-tutorial-batch-testing/prebuilt-domain-1.png)

4. Selecione **Adicionar domínio** para HomeAutomation.

    ![Adicionar domínio HomeAutomation](./media/luis-tutorial-batch-testing/prebuilt-domain-2.png)

5. Selecione **Train** na barra de navegação direita superior.

    ![Selecione o botão de Train](./media/luis-tutorial-batch-testing/train-button.png)

## <a name="batch-test-criteria"></a>Critérios de teste do batch
Teste do batch, pode testar expressões com até 1000 por vez. O batch não deve ter duplicados. [Exportar](create-new-app.md#export-app) a aplicação para ver a lista de expressões com atuais.  

A estratégia de teste para LUIS utiliza três conjuntos separados de dados: expressões com expressões de teste do batch e expressões de ponto final de modelo. Para este tutorial, certifique-se de que não estiver a utilizar o expressões a partir de qualquer uma das expressões de modelo (adicionados numa intenção), ou expressões de ponto final. 

Não utilize qualquer uma das expressões a com já na aplicação para o teste de batch:

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

## <a name="create-a-batch-to-test-intent-prediction-accuracy"></a>Criar um lote para testar a exatidão da previsão de intenção
1. Crie `homeauto-batch-1.json` num editor de texto, como [VSCode](https://code.visualstudio.com/). 

2. Adicionar expressões com o **intenção** desejar prevista no teste. Para este tutorial, para que seja simples, tirar expressões com o `HomeAutomation.TurnOn` e `HomeAutomation.TurnOff` e mude a `on` e `off` texto nas expressões. Para o `None` intenção, adicionar algumas expressões que não são parte dos [domínio](luis-glossary.md#domain) área (assunto). 

    Para compreender como os resultados do teste de batch correlacionam para o batch JSON, adicione apenas seis intenções.

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

4. Escolha a localização de sistema de ficheiros do `homeauto-batch-1.json` ficheiro.

5. Nome do conjunto de dados `set 1`.

    ![Selecionar ficheiro](./media/luis-tutorial-batch-testing/test-4.png)

6. Selecionar o botão **Executar**. Aguarde até que o teste é concluído.

    ![Selecionar executar](./media/luis-tutorial-batch-testing/test-5.png)

7. Selecione **ver resultados**.

    ![Ver resultados](./media/luis-tutorial-batch-testing/test-6.png)

8. Reveja os resultados no gráfico e da legenda.

    ![Resultados de batch](./media/luis-tutorial-batch-testing/batch-result-1.png)

## <a name="review-batch-results"></a>Rever os resultados de batch
Os resultados de batch estão em duas seções. A secção superior contém o gráfico e legenda. A secção da parte inferior mostra expressões com ao selecionar um nome de área do gráfico.

Todos os erros são indicados por cor vermelho. O gráfico está em quatro seções, com duas das seções apresentadas a vermelho. **Estas são as secções concentrar-se no**. 

Canto superior direito de secção indica o teste incorretamente prever a existência de uma intenção ou a entidade. Secção na parte inferior esquerda indica que o teste previsto incorretamente a ausência de um objetivo ou a entidade.

### <a name="homeautomationturnoff-test-results"></a>Resultados do teste HomeAutomation.TurnOff
Na legenda, selecione o `HomeAutomation.TurnOff` intenção. Ele tem um ícone de êxito verde à esquerda do nome na legenda. Não há nenhum erro para este objetivo. 

![Resultados de batch](./media/luis-tutorial-batch-testing/batch-result-1.png)

### <a name="homeautomationturnon-and-none-intents-have-errors"></a>HomeAutomation.TurnOn e nenhum intenções têm erros
Os outros dois objetivos têm erros, que significa que as previsões de teste não correspondeu as expectativas de ficheiro de batch. Selecione o `None` intenção na legenda para rever o primeiro erro. 

![Nenhum intenção](./media/luis-tutorial-batch-testing/none-intent-failures.png)

Falhas são apresentados no gráfico das secções vermelho: **falsos positivos** e **falsos negativos**. Selecione o **falsos negativos** nome da seção no gráfico para ver as expressões com falha abaixo do gráfico. 

![Falhas de negativas FALSO](./media/luis-tutorial-batch-testing/none-intent-false-negative.png)

A expressão de falha `help` era esperado como uma `None` intenção, mas o teste previsto `HomeAutomation.TurnOn` intenção.  

Existem duas falhas, um nos HomeAutomation.TurnOn e um em nenhum. Ambos foram causadas pela expressão `help` porque não satisfazer as expectativas em nenhum e era uma correspondência inesperada para a intenção de HomeAutomation.TurnOn. 

Para determinar por que o `None` expressões com estão a falhar, reveja as expressões atualmente em `None`. 

## <a name="review-none-intents-utterances"></a>Revisão da intenção None expressões com

1. Fechar o **teste** painel ao selecionar o **teste** botão na barra de navegação superior. 

2. Selecione **criar** do painel de navegação superior. 

3. Selecione **None** intenção da lista de objetivos.

4. Selecione Control + E para ver a vista de token das expressões 
    
    |Nenhum intenção 's expressões com|Classificação da predição|
    |--|--|
    |"diminuir temperatura para mim."|0.44|
    |"dim luzes cozinha para 25."|0.43|
    |"reduzir o volume"|0.46|
    |"ligar à internet no meu volte carvalho"|0.28|

## <a name="fix-none-intents-utterances"></a>Corrigir None a intenção de expressões
    
Qualquer discursos em `None` devem para ser fora do domínio de aplicação. Estas expressões são relativas a HomeAutomation, para que elas tenham a intenção de errado. 

LUIS também oferece o expressões com menos de 50% (<.50) pontuação de predição. Se examinar as expressões dos outros dois objetivos, verá as pontuações de predição muito superiores. Quando o LUIS tem pontuações baixas para expressões de exemplo, o que é uma boa indicação as expressões são confusas para LUIS entre a intenção atual e outros objetivos. 

Para corrigir a aplicação, as expressões atualmente no `None` intenção precisam de ser movidos para a intenção correta e o `None` intenção tem intenções de novo, apropriadas. 

Três das expressões no `None` intenção destinam-se para reduzir as definições de dispositivo de automatização. Utilize palavras como `dim`, `lower`, ou `decrease`. A expressão quarta pede ativar a internet. Uma vez que todas as expressões de quatro com são sobre ativando ou alterar o nível de eficiência para um dispositivo, eles devem ser movidos para o `HomeAutomation.TurnOn` intenção. 

Essa é apenas uma solução. Também é possível criar um novo objetivo `ChangeSetting` e mover as expressões utilização dim, reduzir e diminuir para esse propósito de novo. 

## <a name="fix-the-app-based-on-batch-results"></a>Corrigir a aplicação com base nos resultados de batch
Mover as expressões quatro com o `HomeAutomation.TurnOn` intenção. 

1. Selecione a caixa de verificação acima da lista de expressão, por isso, todas as expressões são selecionadas. 

2. Na **reatribuir intenção** menu pendente, selecione `HomeAutomation.TurnOn`. 

    ![Mover expressões com](./media/luis-tutorial-batch-testing/move-utterances.png)

    Depois das expressões de quatro são reatribuídas, a expressão listar para o `None` intenção está vazia.

3. Adicione quatro intenções de novo para a intenção None:

    ```
    "fish"
    "dogs"
    "beer"
    "pizza"
    ```

    Estas expressões são definitivamente fora do domínio da HomeAutomation. Como introduzir cada ocorrência de pronunciação, assista a classificação para o mesmo. A classificação pode ser baixo, ou até mesmo muito baixa (com uma caixa vermelha à volta). Depois de preparar a aplicação, no passo 8, a pontuação será muito maior. 

7. Remover quaisquer etiquetas ao selecionar a etiqueta azul na expressão e selecione **remover etiqueta**.

8. Selecione **Train** na barra de navegação direita superior. A pontuação de cada ocorrência de pronunciação é muito superior. Todas as pontuações para a `None` intenção deve estar acima.80 agora. 

## <a name="verify-the-fix-worked"></a>Certifique-se de que a correção trabalhou
Para verificar que as expressões no teste de batch estão previstas corretamente para o **None** intenção, execute novamente o teste de batch.

1. Selecione **teste** na barra de navegação superior. 

2. Selecione **painel de teste do Batch** no painel do lado direito. 

3. Selecione as reticências (***...*** ) à direita do nome do batch e selecione **executar o conjunto de dados**. Aguarde até que o teste de batch é concluído.

    ![Execute o conjunto de dados](./media/luis-tutorial-batch-testing/run-dataset.png)

4. Selecione **ver resultados**. Os objetivos devem todos ter ícones verde à esquerda dos nomes de intenção. Com o filtro adequado, definido como o `HomeAutomation.Turnoff` intenção, selecione o verde ponto no painel direito superior mais próximo até o meio do gráfico. O nome da expressão aparece na tabela abaixo do gráfico. A pontuação de `breezeway off please` é muito baixa. Uma atividade opcional é adicionar expressões mais para a intenção de aumentar esta pontuação. 

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

4. If the list of datasets is not visible, select **Back to list**. Select the ellipsis (***...***) button at the end of `Set 2` and select `Run Dataset`. Wait for the test to complete.

5. Select **See results** to review the test results.

6. 
-->
## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Saiba mais sobre expressões de exemplo](luis-how-to-add-example-utterances.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions