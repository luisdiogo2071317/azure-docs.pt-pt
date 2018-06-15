---
title: Compreender a classificação de predição devolvida pelo LUIS - Azure | Microsoft Docs
description: Saiba o que significa a classificação de predição na LUIS
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 1620fe80b032245c6ca19279c3981dcff4b9820f
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/05/2018
ms.locfileid: "35356111"
---
# <a name="prediction-score"></a>Classificação de predição
Uma pontuação de predição indica o nível de confiança que Luis tem para resultados de predição. 

Uma pontuação de predição é normalmente entre zero (0) e um (1). Um exemplo de uma pontuação LUIS certeza elevada disponibilidade será 0,99. Um exemplo de uma pontuação de confiança baixa é 0.01. 

|Valor de pontuação|Confiança|
|--|--|
|1|correspondência definite|
|0,99|confiança elevada|
|0.01|confiança baixa|
|0|Falha de definite para corresponder|

Quando um utterance resulta numa pontuação baixa confiança, LUIS realça que no [LUIS] [ LUIS] Web site **intenção** página, com o identificados **intenções de negócios com a etiqueta**  descritas com vermelho. 

![Discrepância de pontuação](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>A classificação superior intenção
Cada predição utterance devolve um objetivo de classificação de topo. Esta é uma comparação numérica de pontuações de predição. As dois pontuações superiores podem ter uma diferença entre eles muito pequena. LUIS não indicar este proximidade diferente pontuações a devolver.  

Se estiver preocupados com a proximidade do pontuações superiores, deverá devolver a classificação de todos os pendentes. Pode adicionar utterances as dois pendentes que indicam as diferenças com a opção de palavra e disposição ou pode ter a aplicação de chamar LUIS, tal como um chatbot, certifique-programáticas opções sobre como lidar com os dois pendentes superiores. 

## <a name="return-prediction-score-for-all-intents"></a>Devolver o modelo de pontuação de predição para todos os pendentes
Um resultado de teste ou de ponto final pode incluir todos os pendentes. Esta configuração está definida no [endpoint](https://aka.ms/v1-endpoint-api-docs) com o `verbose=true` par nome/valor de cadeia de consulta. 

## <a name="review-intents-with-similar-scores"></a>Reveja pendentes com pontuações semelhantes
Rever a classificação de todos os pendentes é uma boa forma de verificar que não só é identificada a intenção correta, mas que a próxima identificado pontuação da intenção é significativamente inferior consistentemente para utterances. 

Se tem de vários pendentes pontuações de predição fechar, com base no contexto de um utterance LUIS pode alternar entre os pendentes. Para corrigir este problema, continue a adicionar utterances para cada tentativa com uma vasta variedade de nível contextual das diferenças.   

## <a name="e-exponent-notation"></a>Notação de I (expoente)

Pontuações de predição podem utilizar a notação expoente; *apresentação* acima de 0-1-se no intervalo, tais como `9.910309E-07`. Este modelo de pontuação é uma indicação de um muito **pequeno** número.

|Classificação de notação i |Pontuação real|
|--|--|
|9.910309E-07|.0000009910309|

## <a name="differences-with-predictions"></a>Diferenças com predições
Quando preparar o mesmo modelo de uma aplicação diferente e as pontuações não são neste mesmas, isto acontece porque há um elemento de randomness de preparação. Lugar, qualquer sobrepõem-se de um utterance a intenção de mais do que um significa que a intenção superior para o mesmo utterance pode alterar com base na formação.

Se o seu chatbot requer uma pontuação LUIS específica para indicar a confiança no objetivo, em vez disso, deve utilizar a diferença de pontuação entre os dois pendentes principais. Isto proporciona flexibilidade variações nos formação. 

## <a name="next-steps"></a>Passos Seguintes

Consulte [adicionar entidades](luis-how-to-add-entities.md) para saber mais sobre como adicionar entidades à sua aplicação LUIS.

[LUIS]:luis-reference-regions.md