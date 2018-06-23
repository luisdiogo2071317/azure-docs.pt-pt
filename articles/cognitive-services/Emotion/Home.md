---
title: Descrição geral de emoções API | Microsoft Docs
description: Utilize o algoritmo de reconhecimento de emoções inovadores, baseado na nuvem do Microsoft para criar mais aplicações personalizadas, com a API de emoções nos serviços cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/06/2017
ms.author: anroth
ms.openlocfilehash: 8383370cba3f78060e809f444f4ad3dab7380f4e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352562"
---
# <a name="what-is-emotion-api"></a>O que é emoções API?

> [!IMPORTANT]
> Emoções API foi preterida no 30 de Outubro de 2017. A funcionalidade faz agora parte [enfrentam API](https://docs.microsoft.com/en-us/azure/cognitive-services/face/).

Bem-vindo ao Microsoft emoções API, permite-lhe criar mais personalizadas aplicações com o algoritmo de reconhecimento de emoções baseado na nuvem da Microsoft.

### <a name="emotion-recognition"></a>Reconhecimento emoções

Versão beta emoções API demora uma imagem como entrada e devolve a confiança através de um conjunto de emotions para cada enfrentam reside na imagem, bem como uma caixa delimitadora para o tipo de letra a partir da API de rostos em. Emotions detetados são happiness, sadness, surprise, anger, fear, contempt, disgust ou independente. Estes emotions são comunicados cross-culturally e universalmente através das mesmas expressões facial básicas, onde são identificados pelo emoções API. 

**Interpretar os resultados:** 

No interpretar os resultados a partir da API de emoções, emoções detetada devem ser interpretada como emoções com a classificação mais elevada, como pontuações são normalizadas soma um. Os utilizadores podem optar por definir um limiar mais elevado de confiança na sua aplicação, consoante as suas necessidades. 

Para obter mais informações sobre a deteção de emoções, consulte a referência de API: 
  * Básico: Se um utilizador já foi chamada da API de letra, podem submeter o retângulo de rostos em como entrada e utilizar o escalão básico. [Referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/56f23eb019845524ec61c4d7)
  * Standard: Se um utilizador não submeter um retângulo de letra, devem utilizar modo padrão.  [Referência da API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa)

Para um exemplo sobre como interpretar as vídeo de transmissão em fluxo com a API de emoções, consulte [como analisar vídeos em Tempo Real](https://docs.microsoft.com/azure/cognitive-services/emotion/emotion-api-how-to-topics/howtoanalyzevideo_emotion).
