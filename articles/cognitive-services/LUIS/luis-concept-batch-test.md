---
title: Batch testar seu aplicativo de LUIS - compreensão de idiomas
titleSuffix: Azure Cognitive Services
description: Use o teste de batch para trabalhar continuamente na sua aplicação para refiná-la e melhorar a compreensão de idiomas.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 1f26b82c5d92631e201ab243d2d49d3b4eb87391
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630231"
---
# <a name="batch-testing-in-luis"></a>Lote de teste de LUIS

Teste de batch valida sua [Active Directory](luis-concept-version.md#active-version) modelo preparado para medir a exatidão da previsão. Um teste de batch ajuda-o a ver a precisão de cada objetivo e a entidade no modelo de preparação atual num gráfico. Reveja os resultados de teste de batch para tomar as medidas adequadas para melhorar a precisão, por exemplo, adicionar mais expressões de exemplo para um objetivo se a sua aplicação falhar com frequência identificar a intenção correta.

## <a name="group-data-for-batch-test"></a>Dados de grupo para teste de batch
É importante que a expressão utilizada para fins de teste do batch estiver familiarizado com o LUIS. Se tiver um conjunto de dados de expressões, dividir as expressões em três conjuntos: discursos adicionados a um objetivo, expressões com recebido a partir do ponto final publicado e expressões com usado para teste de batch LUIS depois é preparado. 

## <a name="a-dataset-of-utterances"></a>Um conjunto de dados de expressões
Submeter um arquivo em lotes de expressões, conhecido como um *conjunto de dados*, para fins de teste do batch. O conjunto de dados é um ficheiro formatado em JSON que contém um máximo de 1000 rotulado **não duplicado** expressões com. Pode testar os conjuntos de dados de até 10 numa aplicação. Se precisar de mais de teste, elimine um conjunto de dados e, em seguida, adicionar um novo.

|**Regras**|
|--|
|* Nenhuma expressões com duplicado|
|Sem subordinados de entidades hierárquicas|
|expressões de 1000 ou menos|

* Duplicatas são consideradas correspondências de cadeia exacta, não as correspondências são indexadas pela primeira vez. 

## <a name="entities-allowed-in-batch-tests"></a>Entidades permitidas em testes de batch
Entidades incluem pais simples, hierárquicas e composição. Todas as entidades desses tipos de aparecem no filtro de entidades de teste de batch mesmo se não existirem não existem entidades correspondentes do arquivo em lotes.


<a name="json-file-with-no-duplicates"></a>
<a name="example-batch-file"></a>
## <a name="batch-file-format"></a>Formato de ficheiro de batch
O arquivo em lotes consiste em expressões. Cada expressão tem de ter uma previsão de intenção esperada, juntamente com quaisquer [entidades aprendidas por máquina](luis-concept-entity-types.md#types-of-entities) espera ser detetado. 

Segue-se um exemplo de um arquivo em lotes com a sintaxe adequada:

```JSON
[
  {
    "text": "Are there any janitorial jobs currently open?",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 14,
            "endPos": 23
        }
    ]
  },
  {
    "text": "I would like a fullstack typescript programming with azure job",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 15,
            "endPos": 46
        }
    ]
  },
  {
    "text": "Is there a database position open in Los Colinas?",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 11,
            "endPos": 18
        }
    ]
  },
  {
    "text": "Please find database jobs open today in Seattle",
    "intent": "GetJobInformation",
    "entities": 
    [
        {
            "entity": "Job",
            "startPos": 12,
            "endPos": 19
        }
    ]
  }
]
```

## <a name="batch-syntax-template"></a>Modelo de sintaxe de batch

Utilize o modelo seguinte para iniciar seu arquivo em lotes:

```JSON
[
  {
    "text": "example utterance goes here",
    "intent": "intent name goes here",
    "entities": 
    [
        {
            "entity": "entity name 1 goes here",
            "startPos": 14,
            "endPos": 23
        },
        {
            "entity": "entity name 2 goes here",
            "startPos": 14,
            "endPos": 23
        }
    ]
  }
]
```

O ficheiro batch utiliza o **startPos** e **endPos** propriedades observar de início e no fim de uma entidade. Os valores não são baseado em zero e devem começar ou terminar num espaço. 

Isso é diferente dos registos de consulta, o que utilizar propriedades startIndex e endIndex. 


## <a name="common-errors-importing-a-batch"></a>Importação de um lote de erros comuns
Erros comuns incluem: 

> * Expressões com mais de 1000
> * Um objeto JSON de expressão que não tem uma propriedade de entidades
> * Word(s) rotulado como em várias entidades

## <a name="batch-test-state"></a>Estado do teste de batch
LUIS controla o estado do último teste de cada conjunto de dados. Isto inclui a data de tamanho (número de expressões no lote), a última execução e o último resultado (número de expressões com prevista com êxito).

<a name="sections-of-the-results-chart"></a>
## <a name="batch-test-results"></a>Resultados do teste de batch
O resultado de teste do batch é um gráfico de dispersão, conhecido como uma matriz de erro. Este gráfico é uma comparação de 4 vias de expressões no arquivo e a intenção de prevista do modelo atual e entidades. 

Pontos de dados na **falsos positivos** e **falsos negativos** secções indicam erros, que devem ser investigados. Se todos os pontos de dados do **positivo verdadeiro** e **Verdadeiro negativo** secções, em seguida, a precisão da sua aplicação é perfeito neste conjunto de dados.

![Quatro seções de gráfico](./media/luis-concept-batch-test/chart-sections.png)

Este gráfico ajuda a localizar as expressões que prevê o LUIS incorretamente com base no seu treinamento atual. Os resultados são exibidos por região do gráfico. Selecione os pontos individuais no gráfico para consultar as informações de expressão ou selecione o nome da região para rever os resultados de expressão nessa região.

![Testes em lote](./media/luis-concept-batch-test/batch-testing.png)

## <a name="errors-in-the-results"></a>Erros nos resultados
Erros no teste de batch indicam intenções que não estão previstas conforme indicado no ficheiro batch. Erros são indicados nas duas secções vermelhas do gráfico. 

A secção positiva falso indica que uma expressão correspondido uma intenção ou a entidade que não deve ter. O negativo falso indica que uma expressão não correspondeu a um objetivo ou a entidade que deve ter. 

## <a name="fixing-batch-errors"></a>Corrigir erros de batch
Se houver erros no teste de lote, pode seja adicionar expressões mais numa intenção, e/ou expressões com mais com a entidade para o ajudar a tornar o discrimination entre objetivos de LUIS da etiqueta. Se tiver adicionado expressões e rotulado como get-los e ainda os erros de predição no teste do batch, considere adicionar uma [lista de frase](luis-concept-feature.md) funcionalidade com o vocabulário específicas de domínio para o ajudar a LUIS aprende mais depressa. 

## <a name="next-steps"></a>Passos Seguintes

* Saiba como [um lote de teste](luis-how-to-batch-test.md)
