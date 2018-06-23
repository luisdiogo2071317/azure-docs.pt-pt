---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: e37d3ef5b6f65ad31bc19f9f8c15350014d1c9ad
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35353354"
---
Com o [anomalias LCA API](https://labs.cognitive.microsoft.com/en-us/project-anomaly-finder), pode carregar dados de séries de tempo no formato JSON para o ponto final de API e, em seguida, lidos o resultado da resposta da API. Pode carregar os dados de séries de tempo, cada ponto de dados inclui:  
* Timestamp - timestamp para o ponto de dados. Certifique-se de que utiliza uma cadeia de tempo de data UTC, por exemplo, "2017-08-01T00:00:00Z"
* Valor - a medição que do ponto de dados

Os resultados é composto por:
* Período - a periodicidade que utiliza a API para detetar as anomalias
* WarningText - as informações de aviso possíveis
* ExpectedValue - o valor previsto pelo learning baseada em modelo
* IsAnomaly - o resultado indica se os pontos de dados estiverem anomalias ou não em ambas as direções (picos ou dips)
* IsAnomaly_Neg - o resultado indica se os pontos de dados estiverem anomalias na direção negativa (dips)
* IsAnomaly_Pos - o resultado indica se os pontos de dados estiverem anomalias na direção positiva (picos de)
* UpperMargin - a soma de ExpectedValue e UpperMargin determina o limite superior que o ponto de dados é ainda considerar como normal
* LowerMargin - (ExpectedValue - LowerMargin) determina vinculado de menor que o ponto de dados é ainda considerar como normal

Podem ser encontrados detalhes do contrato de dados [aqui](../apiref.md).

