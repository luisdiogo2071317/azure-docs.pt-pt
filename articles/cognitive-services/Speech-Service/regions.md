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
ms.openlocfilehash: 082002b25b02e1e496221f4686d0e636630dd438
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39324396"
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

Região| Valor do parâmetro de região no SDK de voz
-|-
EUA Oeste| `westus`
Ásia Oriental| `eastasia`
Europa do Norte| `northeurope`

### <a name="regions-for-intent-recognition"></a>Regiões para o reconhecimento da intenção do

Regiões disponíveis para **reconhecimento da intenção** através do SDK de voz estão listados na [página de região do serviço de compreensão de idiomas](/azure/cognitive-services/luis/luis-reference-regions).
Para cada região publicação listado, o parâmetro de região do SDK de voz correspondente será determinado como a primeira parte do nome do domínio do ponto de extremidade.
Por exemplo, utilizar `westus` para especificar a região de publicação de EUA oeste.
