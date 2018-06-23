---
title: Batch testar a aplicação de LUIS - Azure | Microsoft Docs
description: Utilize o teste de batch para trabalhar continuamente na sua aplicação para refiná-lo e melhorar a compreensão de idiomas.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/14/2018
ms.author: v-geberr
ms.openlocfilehash: 3803df32d6431b8413e8df0837ed62b2e4344cdc
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353348"
---
# <a name="batch-testing-in-luis"></a>Teste no LUIS do batch

Teste de batch valida o [Active Directory](luis-concept-version.md#active-version) modelo treinado para medir a exatidão da previsão. Um teste de batch ajuda a visualizar a precisão do cada intenção e a entidade no seu modelo treinado atual de um gráfico. Reveja os resultados do teste batch tome medidas adequadas para melhorar a precisão, como adicionar mais utterances de exemplo para um objetivo se a sua aplicação com frequência falhar identificar a intenção correta.

## <a name="group-data-for-batch-test"></a>Dados do grupo de teste do batch
É importante que utterances utilizadas para fins de teste do batch são novos para LUIS. Se tiver um conjunto de dados de utterances, dividir utterances em três conjuntos: utterances adicionados ao objetivo, utterances recebidas o ponto final publicado e utterances utilizados para teste de batch LUIS depois está preparada. 

## <a name="a-dataset-of-utterances"></a>Um conjunto de dados de utterances
Submeter um ficheiro batch utterances, conhecido como um *dataset*, para fins de teste do batch. O conjunto de dados é um ficheiro formatado em JSON que contém um máximo de 1000 etiquetadas **não duplicado** utterances. Pode testar conjuntos de dados até 10 numa aplicação. Se precisar de mais de teste, elimine um conjunto de dados e, em seguida, adicionar uma nova.

|**Regras**|
|--|
|* Qualquer utterances duplicados|
|Não existem elementos subordinados hierárquica de entidade|
|1000 utterances ou menos|

* Duplicados são considerados correspondências de cadeia exacta, não corresponde ao que é atomizada primeiro. 

<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>
## <a name="batch-file-format"></a>Formato de ficheiro batch
O ficheiro batch é constituído por utterances. Cada utterance tem de ter uma predição de intenção esperada juntamente com qualquer [adquiridos por máquina entidades](luis-concept-entity-types.md#types-of-entities) que esperava ser detetado. 

Um exemplo de ficheiro batch segue:

   [!code-json[Valid batch test](~/samples-luis/documentation-samples/batch-testing/travel-agent-1.json)]


## <a name="common-errors-importing-a-batch"></a>Importar um lote de erros comuns
Erros comuns incluem: 

> * Mais de 1000 utterances
> * Um objeto JSON utterance que não tem uma propriedade de entidades

## <a name="batch-test-state"></a>Estado de teste do batch
LUIS controla o estado do teste cada conjunto de dados. Isto inclui a data de tamanho (número de utterances no lote,) da última execução e a última resultado (número de utterances previstos com êxito).

<a name="sections-of-the-results-chart"></a>
## <a name="batch-test-results"></a>Resultados do teste de batch
O resultado do teste do batch é um gráfico de gráfico de dispersão, conhecido como uma matriz de erro. Este gráfico é uma comparação de 4-vias de utterances na intenção previstas o modelo atual como o ficheiro e as entidades. 

Pontos de dados das **falsos positivos** e **falso negativo** secções indicam erros, o que devem ser investigados. Se todos os pontos de dados estiverem no **Verdadeiro positivo** e **Verdadeiro negativo** secções, em seguida, a precisão da sua aplicação é perfeito neste conjunto de dados.

![Quatro secções do gráfico](./media/luis-concept-batch-test/chart-sections.png)

Este gráfico ajuda-o a encontrar utterances LUIS prevê incorretamente com base no respetiva formação atual. Os resultados são apresentados por região do gráfico. Selecione pontos individuais no gráfico para rever as informações de utterance ou selecione o nome de região para rever os resultados de utterance nessa região.

![Teste do batch](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Erros nos resultados
Erros no teste batch indicam pendentes que não são prever conforme indicado no ficheiro batch. Erros são indicados em duas secções de vermelhos do gráfico. 

A secção positiva falsa indica que um utterance correspondido um objetivo ou entidade quando não deve ter. O negativo falso indica um utterance não correspondeu a um objetivo ou entidade quando deve ter. 

## <a name="fixing-batch-errors"></a>Corrigir os erros de batch
Se existirem erros nos testes de batch, pode a adicionar mais utterances ao objetivo e/ou etiqueta mais utterances com a entidade para ajudar a LUIS tornar discrimination entre pendentes. Se tiver adicionado utterances e etiqueta get-las e ainda os erros de predição nos testes de batch, considere adicionar um [lista frase](luis-concept-feature.md) funcionalidade com vocabulário específicas do domínio para o ajudar LUIS Saiba mais rapidamente. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [um lote de teste](luis-how-to-batch-test.md)