---
title: Exemplo para tradução | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Eis um exemplo de tradução de reconhecimento de voz.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 1151628ed695e74e2196c20b08e33fa5eaf33282
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/27/2018
ms.locfileid: "37028392"
---
# <a name="sample-for-translation"></a>Exemplo para conversão

> [!NOTE]
> Para obter instruções transferir este exemplo e outros utilizadores, consulte [amostras para o SDK de reconhecimento de voz](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Para todos os exemplos abaixo as seguintes declarações de nível superior devem ser implementados:
>
> [!code-csharp [Using Statements](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#toplevel)]
>
> - - -

## <a name="translation-using-the-microphone"></a>Tradução utilizando microfone

O fragmento de código abaixo mostra como traduzir entrada de voz do inglês para alemão e também obter o resultado de voz do texto traduzido. Utiliza o microfone.

[!code-csharp[Translation Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithMicrophoneAsync)]

- - -

## <a name="translation-using-file-input"></a>Tradução de entrada de ficheiro

O fragmento de código abaixo mostra como converter a entrada de voz do inglês alemão e francês.
Utiliza ficheiros como entrada.

[!code-csharp[Translation Using File Input](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithFileAsync)]

- - -

## <a name="sample-source-code"></a>Código de origem de exemplo

A versão mais recente de exemplos e amostras ainda mais avançadas estão num dedicado [repositório do GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="next-steps"></a>Passos Seguintes

- [Reconhecimento de Voz](./speech-to-text-sample.md)

- [Reconhecimento de Intenção](./intent.md)
