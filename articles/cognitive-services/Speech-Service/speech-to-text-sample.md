---
title: Exemplo para texto de reconhecimento de voz | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Eis um exemplo para texto de reconhecimento de voz.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 9e0b82cc9a034f1e25ef0cc656e3770a166a6ad7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355663"
---
# <a name="sample-for-speech-to-text"></a>Exemplo para texto de reconhecimento de voz

> [!NOTE]
> Para obter instruções transferir este exemplo e outros utilizadores, consulte [amostras para o SDK de reconhecimento de voz](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Para todos os exemplos abaixo, as seguintes declarações de nível superior devem ser implementados:
>
> [!code-csharp[](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#toplevel)]
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="speech-recognition-using-the-microphone"></a>Utilizar microfone de reconhecimento de voz

O fragmento de código abaixo mostra como reconhecer a entrada de voz do microfone no idioma predefinido (`en-US`).

[!code-csharp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!code-cpp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

- - -

## <a name="speech-recognition-from-a-file"></a>Reconhecimento de voz de um ficheiro

O seguinte fragmento de código reconhece a entrada de voz de um ficheiro de áudio no idioma predefinido (`en-US`), o formato suportado é WAV (mono) de canal único / PCM com uma frequência de amostragem de 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-csharp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs?name=recognitionFromFile)]

[!code-cpp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp?name=SpeechRecognitionWithFile)]

- - -

## <a name="speech-recognition-using-a-customized-model"></a>Reconhecimento de voz através de um modelo personalizado

O [serviço de reconhecimento de voz personalizado (CRIS)](https://www.cris.ai/) permite que a personalização do motor de reconhecimento de voz para texto da Microsoft para a sua aplicação. O fragmento abaixo mostra como reconhecer o reconhecimento de voz de um microfone utilizando o seu modelo CRIS; Preencha a chave de subscrição CRIS e a seus próprios identificação de implementação antes de o executar.

[!code-csharp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionCustomized)]

[!code-cpp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

- - -

## <a name="continuous-speech-recognition"></a>Reconhecimento de voz contínua

[!code-csharp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionContinuous)]

[!code-cpp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Código de origem de exemplo

A versão mais recente de exemplos e amostras ainda mais avançadas estão num dedicado [repositório do GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="next-steps"></a>Passos Seguintes

- [Reconhecimento intenção](./intent.md)

- [Tradução](./translation.md)
