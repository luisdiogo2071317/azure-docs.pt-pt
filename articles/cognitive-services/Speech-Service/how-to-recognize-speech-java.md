---
title: Reconhecer fala com o SDK de voz para Java
titleSuffix: Microsoft Cognitive Services
description: >
  Aprender a reconhecer a conversão de voz (de um arquivo, usando um microfone, com um modelo personalizado, continuamente ou da) com o SDK de voz para Java.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: e44bbd4a620e65f1d17ffe09a7130e624e6337bb
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43109695"
---
# <a name="recognize-speech-by-using-the-speech-sdk-for-java"></a>Reconhecer fala com o SDK de voz para Java

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

[!include[Introduction](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-intro.md)]

[!include[Introduction for top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#toplevel)]

[!include[Introduction to using a microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-microphone.md)]

[!code-java[Speech recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionWithMicrophone)]

[!include[Introduction to using customized recognition](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-customized.md)]

[!code-java[Speech recognition by using a customized model](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionCustomized)]

[!include[Introduction to using a continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-continuous.md)]

[!code-java[Continuous speech recognition](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionContinuousWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Procure o código que é utilizado neste artigo na pasta de exemplos/java/jre/consola.

## <a name="next-steps"></a>Passos Seguintes

- [Como reconhecer intenções de voz](how-to-recognize-intents-from-speech-java.md)
- [Como a conversão de voz](how-to-translate-speech-java.md)

