---
title: incluir ficheiro
description: incluir ficheiro
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-finder
ms.topic: include
ms.date: 04/13/2018
ms.author: chliang
ms.custom: include file
ms.openlocfilehash: 9280790f6692096a0b3909c9d1dfab2e94a8c0d7
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/09/2018
ms.locfileid: "48904509"
---
Com o [API do localizador de anomalias](https://labs.cognitive.microsoft.com/en-us/project-anomaly-finder), pode carregar dados de séries de tempo no formato JSON para o ponto final da API e, em seguida, leia o resultado da resposta da API. Pode carregar os dados de séries de tempo, cada ponto de dados inclui:  
* Timestamp - o carimbo de hora para o ponto de dados. Certifique-se de que utiliza uma cadeia de hora de data UTC, por exemplo, "2017-08-01T00:00:00Z"
* Valor - a medição desse ponto de dados

Os resultados são compostas por:
* Período - a periodicidade que a API utiliza para detetar as anomalias
* WarningText - as informações de aviso possíveis
* Modelo baseado em ExpectedValue - o valor previsto de aprendizagem
* IsAnomaly - o resultado no se os pontos de dados são anomalias ou não em ambas as direções (picos ou as quedas anormais)
* IsAnomaly_Neg - o resultado no se os pontos de dados são anomalias na direção negativa (dips)
* IsAnomaly_Pos - o resultado no se os pontos de dados são anomalias na direção positiva (picos)
* UpperMargin - a soma de ExpectedValue e UpperMargin determina o limite superior que o ponto de dados ainda é considerado como normal
* LowerMargin - (ExpectedValue - LowerMargin) determina a menor vinculado a que ponto de dados ainda é considerado como normal

Detalhes do contrato de dados podem ser encontrados [aqui](../apiref.md).

