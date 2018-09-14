---
title: Descrição geral da API de emoções | Documentos da Microsoft
description: Utilize o algoritmo de reconhecimento de emoções inovadores, baseados na nuvem do Microsoft para criar aplicações mais personalizadas, com a API de emoções nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 210990b0f436fd75cb36e71ea28928c457a5232e
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45573559"
---
# <a name="what-is-emotion-api"></a>O que é a API de Emoções?

> [!IMPORTANT]
> API de emoções foi preterido no dia 30 de Outubro de 2017. A funcionalidade faz agora parte do [a API Face](https://docs.microsoft.com/azure/cognitive-services/face/).

Bem-vindo à API de emoções do Microsoft, que permite-lhe criar aplicações mais personalizadas com o algoritmo de reconhecimento de emoções com base na cloud da Microsoft.

### <a name="emotion-recognition"></a>Reconhecimento de emoções

A versão beta de API de emoções utiliza uma imagem como entrada e devolve a confiança entre um conjunto de emoções para cada rosto na imagem, bem como uma caixa delimitadora para o rosto da Face API. As emoções detetadas são felicidade, tristeza, surpresa, irritação, medo, desdém, repulsa ou neutra. Estas emoções são comunicadas interculturais e universalmente através das mesmo expressões faciais básicas, quando são identificadas pela API de emoções. 

**Interpretar os resultados:** 

Em interpretar os resultados da API de emoções, as emoções detetadas devem ser interpretada como emoções com a pontuação mais alta, como as pontuações são normalizadas à soma para um. Os utilizadores podem optar por definir um limiar mais elevado de confiança dentro de seu aplicativo, dependendo das suas necessidades. 

Para obter mais informações sobre a deteção de emoções, consulte a referência de API: 
  * Básica: Se um utilizador já tiver chamado a API Face, podem submeter o retângulo da face como entrada e utilizar o escalão básico. [Referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standard: Se um utilizador não submeterem um retângulo de rostos, eles devem utilizar o modo padrão.  [Referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Para obter um exemplo sobre como interpretar o vídeo de transmissão em fluxo com a API de emoções, veja [como analisar vídeos em Tempo Real](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
