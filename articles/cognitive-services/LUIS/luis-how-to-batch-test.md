---
title: Batch testar a aplicação de LUIS - Azure | Microsoft Docs
description: Utilize o teste de batch compreensão de idiomas (LUIS) para localizar utterances com entidades e pendentes incorretos.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 822fb1e2d5b13941527d242e8501b423bd6b81cb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265518"
---
# <a name="batch-testing"></a>Teste do batch
 Teste do batch é um teste completo no seu modelo treinado atual para medir o desempenho no LUIS. 

<a name="batch-testing"></a>
## <a name="import-a-dataset-file-for-batch-testing"></a>Importar um ficheiro de conjunto de dados para fins de teste do batch

1. Selecione **teste** na parte superior barras e, em seguida, selecione **painel de teste do Batch**.

    ![Ligação de teste do batch](./media/luis-how-to-batch-test/batch-testing-link.png)

2. Selecione **conjunto de dados de importação**. O **importar novo conjunto de dados** é apresentada a caixa de diálogo. Selecione **Escolher ficheiro** e localize o [JSON](luis-concept-batch-test.md#batch-file-format) ficheiro que contém *mais do que 1000* utterances para testar.

    ![Ficheiro de importação de conjunto de dados](./media/luis-how-to-batch-test/batchtest-importset.png)

    Importar erros reportados numa barra de notificação vermelho na parte superior do browser. Quando uma importação tem erros, não é criado nenhum conjunto de dados. Para obter mais informações, consulte [erros comuns](luis-concept-batch-test.md#common-errors-importing-a-batch).

3. No **nome de Dataset** campo, introduza um nome para o ficheiro de conjunto de dados. O ficheiro de conjunto de dados inclui um **matriz de utterances** , incluindo o *etiqueta intenção* e *entidades*. Reveja o [exemplo de ficheiro batch](luis-concept-batch-test.md#batch-file-format) de sintaxe. 

4. Selecione **feito**. O ficheiro de conjunto de dados é adicionado.

## <a name="run-rename-export-or-delete-dataset"></a>Executar, mudar o nome, exportar ou eliminar o conjunto de dados
Para executar, mudar o nome, exportar ou elimine o conjunto de dados, utilize reticências (**...** ) no final da linha de conjunto de dados.

![Ações de conjunto de dados](./media/luis-how-to-batch-test/batch-testing-options.png)

## <a name="run-a-batch-test-on-your-trained-app"></a>Executar um teste de batch na sua aplicação preparada

Para executar o teste, selecione o nome do conjunto de dados. Quando a conclusão do teste, esta linha apresenta o resultado do teste do conjunto de dados.

![Resultado do teste do batch](./media/luis-how-to-batch-test/run-test.png)

O conjunto de dados transferível é o mesmo ficheiro que foi carregado para fins de teste do batch.

|Estado|Significado|
|--|--|
|![Ícone de círculo verde de teste com êxito](./media/luis-how-to-batch-test/batch-test-result-green.png)|Todos os utterances forem efetuadas com êxito.|
|![Teste falhar Vermelho x ícone](./media/luis-how-to-batch-test/batch-test-result-red.png)|Intenção de pelo menos um utterance não correspondeu a predição.|
|![Pronto para o ícone de teste](./media/luis-how-to-batch-test/batch-test-result-blue.png)|Teste está pronto para ser executado.|

<a name="access-batch-test-result-details-in-a-visualized-view"></a>
## <a name="view-batch-test-results"></a>Ver os resultados do teste do batch 
Para rever os resultados de teste do batch, selecione **ver resultados**.

![Resultados do teste de batch](./media/luis-how-to-batch-test/run-test-results.png)

<!--
 Select the **See results** link that appears after you run the test. A scatter graph known as an error matrix displays. The data points represent the utterances in the dataset. 

Green points indicate correct prediction, and red ones indicate incorrect prediction.

The filtering panel on the right side of the screen displays a list of all intents and entities in the app, with a green point for intents/entities that were predicted correctly in all dataset utterances, and a red point for those items with errors. Also, for each intent/entity, you can see the number of correct predictions out of the total utterances.

-->


<a name="filter-chart-results-by-intent-or-entity"></a> ## Filtrar os resultados do gráfico

Para filtrar o gráfico por um objetivo específico ou entidade, selecione o objetivo ou entidade no painel de filtragem do lado direito. Os pontos de dados e a respetiva distribuição atualizar no gráfico de acordo com a sua seleção. 
 
![Resultado do teste do Batch visualizadas](./media/luis-how-to-batch-test/filter-by-entity.png) 

<!--
## Investigate false sections
Data points on the **[False Positive][false-positive]** and **[False Negative][false-negative]** sections indicate errors, which should be investigated. If all data points are on the **[True Positive][true-positive]** and **[True Negative][true-negative]** sections, then your application's performance is perfect on this dataset.


The graph indicates [F-measure][f-measure], [recall][recall], and [precision][precision].  
-->
## <a name="view-single-point-utterance-data"></a>Visualizar os dados única ponto utterance
No gráfico, coloque o cursor sobre um ponto de dados para ver a classificação de certainty da sua predição. Selecione um ponto de dados para obter o respetivo utterance correspondente na lista utterances na parte inferior da página. 

![Utterance selecionado](./media/luis-how-to-batch-test/selected-utterance.png)


<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="view-section-data"></a>Ver dados de secção
O gráfico de secção quatro, selecione o nome de secção, tal como **falsos positivos** na parte superior direita do gráfico. Abaixo do gráfico, todos os utterances na secção apresentam abaixo do gráfico numa lista. 

![Utterances selecionadas pela secção](./media/luis-how-to-batch-test/selected-utterances-by-section.png)

Nesta imagem anterior, o utterance `switch on` assinalada como com a tentativa de TurnAllOn, mas foram recebidos a predição de intenção None. Esta é uma indicação de que a intenção de TurnAllOn precisa de mais utterances de exemplo para efetuar a predição esperada. 

As duas secções do gráfico vermelho indicam utterances que não correspondia a predição esperada. Estas marcas indicam utterances que LUIS precisa de mais preparação. 

As duas secções do gráfico verde corresponde a predição esperada.

## <a name="next-steps"></a>Passos Seguintes

Se testar a indicar que a sua aplicação LUIS não reconhece as entidades e pendentes corretos, pode trabalhar para melhorar o desempenho da sua aplicação LUIS etiquetas utterances mais ou adicionar funcionalidades. 

* [Etiqueta utterances sugeridos com LUIS](Label-Suggested-Utterances.md) 
* [Utilize as funcionalidades para melhorar o desempenho da sua aplicação LUIS](luis-how-to-add-features.md) 
* [Compreender os testes com este tutorial do batch](luis-tutorial-batch-testing.md)
* [Saiba mais testes conceitos de batch](luis-concept-batch-test.md).

[true-positive]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#true-positive
[true-negative]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#true-negative
[false-positive]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#false-positive
[false-negative]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#false-negative
[f-measure]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#f-measure
[recall]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#recall
[precision]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-glossary#precision

