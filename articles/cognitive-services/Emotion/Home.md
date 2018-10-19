---
title: O que é a API de Emoções?
titlesuffix: Azure Cognitive Services
description: Utilize o algoritmo de reconhecimento de emoções com base na cloud para compilar aplicações mais personalizadas.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: overview
ms.date: 02/06/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: abf94e0ab6ebb3df649b1958503c086feb4fa19e
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237131"
---
# <a name="what-is-the-emotion-api"></a>O que é a API de Emoções?

> [!IMPORTANT]
> A API de Emoções vai ser preterida no dia 15 de fevereiro de 2019. A função de reconhecimento de emoções está agora geralmente disponível como parte da [API Face](https://docs.microsoft.com/azure/cognitive-services/face/). 

Bem-vindo à API de Emoções do Microsoft, que permite criar aplicações mais personalizadas com o algoritmo de reconhecimento de emoções com base na cloud da Microsoft.

### <a name="emotion-recognition"></a>Reconhecimento de Emoções

A versão beta da API de Emoções utiliza uma imagem como uma entrada e devolve a confiança entre um conjunto de emoções para cada rosto na imagem, além de uma caixa delimitadora para o rosto da API Face. As emoções detetadas são felicidade, tristeza, surpresa, raiva, medo, desdém, repulsa ou neutras. Estas emoções são transmitidas de forma intercultural e universal através das mesmas expressões faciais básicas, sendo identificadas pela API de Emoções.

**Interpretar os Resultados:**

Ao interpretar os resultados da API de Emoções, a emoção detetada deve ser interpretada como a emoção com a pontuação mais elevada, pois as pontuações são normalizadas para somarem a unidade. Os utilizadores podem optar por definir um limiar mais elevado de confiança dentro da aplicação, dependendo das suas necessidades.

Para obter mais informações sobre a deteção de emoções, veja a Referência da API:
  * Básico: se um utilizador já tiver chamado a API Face, pode submeter o retângulo do rosto como uma entrada e utilizar o escalão básico. [Referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standard: se o utilizador não submeter um retângulo de rosto, deve utilizar o modo standard.  [Referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Para obter um exemplo sobre como interpretar a transmissão de vídeo com a API de Emoções, veja [Como Analisar Vídeos em Tempo Real](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
