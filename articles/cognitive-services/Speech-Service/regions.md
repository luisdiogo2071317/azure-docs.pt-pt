---
title: Regiões do serviço de reconhecimento de voz | Microsoft Docs
description: Referência para regiões do serviço de reconhecimento de voz.
services: cognitive-services
author: mahilleb-msft
manager: wolmfa61
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 06/28/2018
ms.author: mahilleb
ms.openlocfilehash: 1eb3768f5a5c5a27a45dde3f62f862f36fa3e8ac
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/29/2018
ms.locfileid: "37098503"
---
# <a name="regions-of-the-speech-service"></a>Regiões do serviço de reconhecimento de voz

O serviço de reconhecimento de voz está disponível em regiões diferentes.
Quando cria uma subscrição pode escolher uma região disponível, consoante as suas necessidades.

Quando utilizar a sua subscrição tem em conta a região que é selecionado.

## <a name="rest-api"></a>API REST

Utilizando a API REST, escolha os pontos finais direito específico da região.
Consulte [REST APIs](rest-apis.md) para obter mais detalhes.



## <a name="speech-sdk"></a>API de Voz

No [SDK de reconhecimento de voz](speech-sdk.md), regiões são especificadas como uma cadeia (por exemplo, como um parâmetro para [SpeechFactory.FromSubscription](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechfactory.fromsubscription) no SDK do reconhecimento de voz para c#).

A tabela abaixo lista as regiões para reconhecimento de voz e tradução disponíveis:

Região| Valor do parâmetro de região no SDK do reconhecimento de voz
-|-
EUA Oeste| `westus`
Ásia Oriental| `eastasia`
Europa do Norte| `northeurope`

São listadas regiões disponíveis para intenção reconhecimento através do SDK de reconhecimento de voz no [página de região de serviço de compreensão de idiomas](/azure/cognitive-services/luis/luis-reference-regions).
Para cada região publicação listado, o parâmetro de região do SDK de reconhecimento de voz correspondente é determinado como a primeira parte do nome de domínio do ponto final.
Por exemplo, utilizar `westus` para especificar a região de publicação de EUA oeste.
