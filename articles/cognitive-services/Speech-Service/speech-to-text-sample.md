---
title: Exemplo de voz em texto | Documentos da Microsoft
titleSuffix: Microsoft Cognitive Services
description: Eis um exemplo de voz em texto.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 5740586eca902fa63ca7b8590b07f4b276d21e4d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071373"
---
# <a name="sample-for-speech-to-text"></a>Exemplo de voz em texto

[!include[Get a Subscription Key](../../../includes/cognitive-services-speech-service-get-subscription-key.md)]

## <a name="top-level-declarations"></a>Declarações de nível superior

Para todos os exemplos abaixo, as seguintes declarações de nível superior devem estar no local:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#toplevel)]

## <a name="speech-recognition-using-the-microphone"></a>Reconhecimento de voz com o microfone

O fragmento de código abaixo mostra como reconhecer a entrada de voz do microfone no idioma padrão (`en-US`).

[!code-csharp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!code-cpp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

[!code-java[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionWithMicrophone)]

## <a name="speech-recognition-using-a-customized-model"></a>Reconhecimento de voz com um modelo personalizado

O [serviço de voz personalizado (CRIS)](https://www.cris.ai/) permite a personalização do motor de voz em texto da Microsoft para a sua aplicação. O fragmento abaixo mostra como reconhecer a conversão de voz de um microfone com o seu modelo CRIS; Preencha a chave de subscrição CRIS e sua própria identificação de implementação antes de o executar.

[!code-csharp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionCustomized)]

[!code-cpp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

[!code-java[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionCustomized)]

## <a name="continuous-speech-recognition-from-a-file"></a>Reconhecimento de fala contínua de um ficheiro

O fragmento de código seguinte continuamente reconhece a entrada de voz de um arquivo de áudio no idioma padrão (`en-US`), o formato suportado é (mono) de canal único WAV / PCM com uma taxa de amostragem de 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-csharp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/speech_recognition_samples.cs#recognitionContinuousWithFile)]

[!code-cpp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionWithFile)]

[!code-java[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/SpeechRecognitionSamples.java#recognitionContinuousWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Passos Seguintes

- [Reconhecimento de Intenção](./intent.md)

- [Tradução](./translation.md)
