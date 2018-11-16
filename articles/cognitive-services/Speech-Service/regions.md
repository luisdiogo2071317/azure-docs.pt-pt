---
title: Regiões de serviço de voz
titlesuffix: Azure Cognitive Services
description: Referência para as regiões do serviço de voz.
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: 8d36036332e939075ffac8763bec9c23d8e4a3f7
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712896"
---
# <a name="speech-service-supported-regions"></a>Regiões suportadas do serviço de voz

O serviço de voz permite que seu aplicativo converter áudio em texto, efetuar a tradução de voz e converter texto em voz. O serviço está disponível em várias regiões com pontos de extremidade exclusivos para o SDK de voz e REST APIs.

Certifique-se de que use o ponto final que corresponde à região para a sua subscrição.

## <a name="speech-sdk"></a>API de Voz

Na [SDK do serviço de voz](speech-sdk.md), regiões são especificadas como uma cadeia de caracteres (por exemplo, como um parâmetro para `SpeechConfig.FromSubscription` no SDK de voz para c#).

### <a name="speech-recognition-and-translation"></a>Reconhecimento de fala e tradução

O SDK de voz está disponível nestas regiões para **reconhecimento de fala** e **tradução**:

  Região | Parâmetro do SDK de voz | Portal de personalização de voz
 ------|-------|--------
 EUA Oeste | `westus` | https://westus.cris.ai
 E.U.A. Oeste 2 | `westus2` | https://westus2.cris.ai
 EUA Leste | `eastus` | https://eastus.cris.ai
 E.U.A. Leste 2 | `eastus2` | https://eastus2.cris.ai
 Ásia Oriental | `eastasia` | https://eastasia.cris.ai
 Sudeste Asiático | `southeastasia` | https://southeastasia.cris.ai
 Europa do Norte | `northeurope` | https://northeurope.cris.ai
 Europa Ocidental | `westeurope` | https://westeurope.cris.ai


### <a name="intent-recognition"></a>Reconhecimento da intenção do

**Reconhecimento da intenção** para as partilhas do SDK de voz as regiões suportam com os LUIS. Para obter uma lista completa de regiões disponíveis, consulte [publicação regiões e pontos finais - LUIS](https://docs.microsoft.comazure/cognitive-services/luis/luis-reference-regions)

Regiões disponíveis para **reconhecimento da intenção** através do SDK de voz estão listados no [página de região do serviço de compreensão de idiomas](/azure/cognitive-services/luis/luis-reference-regions).

Para cada região publicação listado, utilize o fornecido **nome da região de API**. Por exemplo, utilizar `westus` para E.U.A. oeste.

## <a name="rest-apis"></a>APIs REST

O serviço de voz também expõe os pontos finais REST para pedidos de voz em texto e texto para voz.

### <a name="speech-to-text"></a>Conversão de voz em texto

Para obter documentação de referência de fala em texto, consulte [REST APIs](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="text-to-speech"></a>Conversão de texto em voz

Para obter documentação de referência de texto para voz, consulte [REST APIs](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#speech-to-text).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]
