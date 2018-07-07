---
title: Compreender a pontuação de predição devolvida pelo LUIS - Azure | Documentos da Microsoft
description: Saiba o que significa a pontuação de predição em LUIS
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 88d5eb22186248024a356610addab0d43f68a961
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/06/2018
ms.locfileid: "37887135"
---
# <a name="prediction-score"></a>Classificação da predição
Uma pontuação de predição indica o grau de confiança que Luis tem para resultados de predição. 

Uma pontuação de predição é normalmente entre zero (0) e um (1). Um exemplo de uma pontuação de LUIS elevada confiança será 0,99. Um exemplo de uma pontuação de confiança baixa é a 0,01. 

|Valor de pontuação|Confiança|
|--|--|
|1|correspondência definida|
|0,99|confiança elevada|
|0.01|confiança baixa|
|0|falha definida para corresponder|

Quando uma expressão resulta numa pontuação de confiança de baixa, LUIS destaca que, na [LUIS](luis-reference-regions.md) site **intenção** página, com o identificados **intenção rotulado** descritos com red. 

![Discrepância de pontuação](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Intenção da parte superior de classificação
Predição de cada expressão retorna uma intenção da parte superior de classificação. Esta é uma comparação numérica de pontuações de predição. As pontuações de duas principais podem ter uma muito pequena diferença entre eles. LUIS não indicar este proximidade que não seja retornando as pontuações.  

Se estiver preocupado a proximidade das pontuações superior, deverá devolver a classificação para todas as intenções. Pode adicionar expressões com para os dois objetivos que indicam as diferenças com a escolha do word e disposição ou pode ter o aplicativo de chamada de LUIS, como um chatbot, fazer escolhas programáticas sobre como lidar com os dois objetivos principais. 

## <a name="return-prediction-score-for-all-intents"></a>Devolver a pontuação de previsão para todos os objetivos
Um resultado de teste ou ponto de extremidade pode incluir todas as intenções. Esta configuração é definida no [ponto final](https://aka.ms/v1-endpoint-api-docs) com o `verbose=true` par nome/valor de cadeia de caracteres de consulta. 

## <a name="review-intents-with-similar-scores"></a>Reveja os objetivos com pontuações semelhantes
Rever a classificação para todas as intenções é uma excelente forma de verificar que não só é identificada a intenção correta, mas que a próxima identificadas pontuação da intenção é significativamente menor consistentemente para expressões com. 

Se tem de objetivos várias pontuações de predição de fechar, com base no contexto de uma expressão, LUIS pode alternar entre os objetivos. Para corrigir este problema, continue a adicionar expressões em cada intenção com uma maior variedade de diferenças contextuais.   

## <a name="e-exponent-notation"></a>Notação de I (expoente)

Pontuações de predição podem usar a notação expoente, *que aparece* acima 0-1 intervalo, tais como `9.910309E-07`. Esta pontuação é uma indicação de um muito **pequeno** número.

|Pontuação de notação E |Pontuação real|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="differences-with-predictions"></a>Diferenças de previsões
Quando treinar o mesmo modelo num aplicativo diferente, e as pontuações não nesse mesmas, isto acontece porque existe um elemento um pouco da aleatoriedade no treinamento. Em segundo lugar, a qualquer sobrepõem-se de uma expressão em mais do que uma intenção significa que a intenção de superior para a mesma expressão pode mudar com base na formação.

Se sua chatbot exigir uma pontuação de LUIS específica para indicar a confiança numa intenção, deve usar a diferença de pontuação entre os objetivos de duas principais. Isto proporciona flexibilidade para variações no treinamento. 

## <a name="next-steps"></a>Passos Seguintes

Ver [adicionar entidades](luis-how-to-add-entities.md) para saber mais sobre como adicionar entidades à sua aplicação LUIS.