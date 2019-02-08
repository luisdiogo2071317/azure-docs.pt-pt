---
title: Tradução de voz com o SDK de voz para c#
titleSuffix: Azure Cognitive Services
description: Este artigo inclui código de exemplo para a tradução de voz com o SDK de voz num C# ambiente.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: b4e3c2c678789e661b4d6e2d857da2a298ae68f6
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55877189"
---
# <a name="translate-speech-with-the-cognitive-services-speech-sdk-for-c"></a>Traduzir a conversão de voz com o SDK de voz dos serviços cognitivos para c#

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-translate-speech-selector.md)]

[!INCLUDE [Introduction](../../../includes/cognitive-services-speech-service-how-to-translate-speech-intro.md)]

[!INCLUDE [Introduction to top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#toplevel)]

[!INCLUDE [Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-translate-speech-microphone.md)]

[!code-csharp[Translation from a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#TranslationWithMicrophoneAsync)]

[!INCLUDE [Introduction to using a file](../../../includes/cognitive-services-speech-service-how-to-translate-speech-file.md)]

[!code-csharp[Translation from file input](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/translation_samples.cs#TranslationWithFileAsync)]

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure o código que é utilizado neste artigo na pasta de exemplos/csharp/sharedcontent/consola.

## <a name="next-steps"></a>Passos Seguintes

- [How to recognize speech](how-to-recognize-speech-csharp.md) (Como reconhecer voz)
- [Como reconhecer intenções de voz](how-to-recognize-intents-from-speech-csharp.md)
