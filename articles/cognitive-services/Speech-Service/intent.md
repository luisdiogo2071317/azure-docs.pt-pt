---
title: Exemplo de reconhecimento de intenção | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Eis um exemplo de reconhecimento de intenção.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 47527087dc008c201949524f193820e636a8744f
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356232"
---
# <a name="sample-for-intent-recognition"></a>Exemplo de reconhecimento de intenção

> [!NOTE]
> Para obter instruções transferir este exemplo e outros utilizadores, consulte [amostras para o SDK de reconhecimento de voz](samples.md).

> [!NOTE]
> Obtenha primeiro uma chave de subscrição. Ao contrário de outros serviços suportados pelo SDK do reconhecimento de voz cognitivos do serviço, os serviços de reconhecimento de intenção requer uma chave de subscribtion específico. [Aqui](https://www.luis.ai) pode encontrar informações adicionais sobre a tecnologia de reconhecimento intenção, bem como informações sobre como adquirir uma chave de subscrição. Substitua a sua própria chave de subscrição, a região do serviço, bem como o AppId do seu modelo intenção no local adequado nos exemplos.

> [!NOTE]
> Para todos os exemplos abaixo as seguintes declarações de nível superior devem ser implementados:
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="intent-recognition-using-microphone"></a>Reconhecimento intenção utilizando microfone

O fragmento de código abaixo mostra como reconhecer a intenção de entrada do microfone no idioma predefinido (`en-US`).

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

- - -

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Reconhecimento intenção utilizando microfone num idioma especificado

O fragmento de código abaixo mostra como reconhecer a intenção de entrada do microfone num idioma especificado, neste caso no alemão (`de-de`).

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

- - -

## <a name="intent-recognition-from-a-file"></a>Reconhecimento intenção de um ficheiro

O seguinte fragmento de código reconhece intenção de um ficheiro de áudio no idioma predefinido (`en-US`), o formato suportado é WAV (mono) de canal único / PCM com uma frequência de amostragem de 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-cpp[Intent Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp?IntentRecognitionWithFile)]

- - -

## <a name="intent-recognition-using-events"></a>Reconhecimento intenção a utilização de eventos

O fragmento de código mostra como reconhecer a intenção de forma contínua. Este código permite o acesso a informações adicionais, como resultados intermédios. 

[!code-cpp[Intent Recognition Using Events](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Código de origem de exemplo

Para o conjunto mais recente de exemplos, consulte o [repositório cognitivos GitHub de exemplo do SDK de reconhecimento de voz de serviços](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Passos Seguintes

- [Reconhecimento de voz](./speech-to-text-sample.md)

- [Tradução](./translation.md)