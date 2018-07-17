---
title: Exemplo para o reconhecimento da intenção | Documentos da Microsoft
titleSuffix: Microsoft Cognitive Services
description: Eis um exemplo para o reconhecimento da intenção.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 4cf3bbfa24e102c544b0e3215a20b73d323f15df
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/17/2018
ms.locfileid: "39070965"
---
# <a name="sample-for-intent-recognition"></a>Exemplo para o reconhecimento da intenção do

Primeiro obtenha uma chave de subscrição. Ao contrário de outros serviços suportados pelo SDK de voz cognitivos do serviço, os serviços de reconhecimento de intenção requer uma chave de subscrição específica. [Aqui](https://www.luis.ai) pode encontrar informações adicionais sobre a tecnologia de reconhecimento da intenção, bem como informações sobre como adquirir uma chave de subscrição. Substitua a sua própria chave de subscrição de compreensão de idiomas, o [região da sua subscrição](regions.md)e o AppId do seu modelo de intenção nos lugares apropriados nos exemplos.

## <a name="top-level-declarations"></a>Declarações de nível superior

Para todos os exemplos abaixo, as seguintes declarações de nível superior devem estar no local:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#toplevel)]

## <a name="intent-recognition-using-microphone"></a>Reconhecimento da intenção a utilizar o microfone

O fragmento de código abaixo mostra como reconhecer intenções da entrada do microfone no idioma padrão (`en-US`).

[!code-csharp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

[!code-java[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithMicrophone)]

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Reconhecimento da intenção usando microfone num idioma especificado

O fragmento de código abaixo mostra como reconhecer intenções da entrada do microfone num idioma especificado, neste caso em alemão (`de-de`).

[!code-csharp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithLanguage)]

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

[!code-java[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithLanguage)]

## <a name="intent-recognition-from-a-file-using-events"></a>Reconhecimento da intenção de um arquivo, usando eventos

O fragmento de código mostra como reconhecer intenções no idioma padrão (`en-US`) de forma contínua. Esse código permite o acesso a informações adicionais, como os resultados intermediários. Entrada é retirada de um arquivo de áudio, o formato suportado é (mono) de canal único WAV / PCM com uma taxa de amostragem de 16 KHz.

[!code-csharp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

[!code-cpp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentContinuousRecognitionWithFile)]

[!code-java[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentContinuousRecognitionWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Passos Seguintes

- [Reconhecimento de Voz](./speech-to-text-sample.md)

- [Tradução](./translation.md)
