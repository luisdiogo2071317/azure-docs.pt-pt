---
title: Regiões de serviço de voz
description: Referência para as regiões do serviço de voz.
services: cognitive-services
author: mahilleb-msft
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 1cb00035dc8f1cdeabd1beb22ca69f47bf4bd89e
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379575"
---
# <a name="regions-of-the-speech-service"></a>Regiões do serviço de voz

O serviço de voz está disponível em regiões diferentes.
Quando cria uma subscrição pode escolher uma região disponível, consoante as suas necessidades.

Quando utilizar a sua subscrição tem em conta a região que selecionou.

## <a name="rest-api"></a>API REST

Com a API REST, escolha os pontos de extremidade direita específico da região.
Ver [REST APIs](rest-apis.md) para obter detalhes.

## <a name="speech-sdk"></a>API de Voz

Na [SDK de voz](speech-sdk.md), regiões são especificadas como uma cadeia de caracteres (por exemplo, como um parâmetro para [SpeechFactory.FromSubscription](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechfactory.fromsubscription) no SDK de voz para c#).

### <a name="regions-for-speech-recognition-and-translation"></a>Regiões para reconhecimento de fala e tradução

A tabela abaixo lista as regiões disponíveis para **reconhecimento de fala** e **tradução**:

Região| Valor do parâmetro de região no SDK de voz| Portal
-|-
EUA Oeste| `westus`| https://westus.cris.ai
E.U.A. Oeste 2| `westus2`| https://westus2.cris.ai
EUA Leste| `eastus`| https://eastus.cris.ai
E.U.A. Leste 2| `eastus2`| https://eastus2.cris.ai
Ásia Oriental| `eastasia`| https://eastasia.cris.ai
Sudeste Asiático| `southeastasia`| https://southeastasia.cris.ai
Europa do Norte| `northeurope`| https://northeurope.cris.ai
Europa Ocidental|  `westeurope`| https://westeurope.cris.ai

### <a name="regions-for-intent-recognition"></a>Regiões para o reconhecimento da intenção do

Regiões disponíveis para **reconhecimento da intenção** através do SDK de voz estão listados na [página de região do serviço de compreensão de idiomas](/azure/cognitive-services/luis/luis-reference-regions).
Para cada região publicação listado, o parâmetro de região do SDK de voz correspondente será determinado como a primeira parte do nome do domínio do ponto de extremidade.
Por exemplo, utilizar `westus` para especificar a região de publicação de EUA oeste.
