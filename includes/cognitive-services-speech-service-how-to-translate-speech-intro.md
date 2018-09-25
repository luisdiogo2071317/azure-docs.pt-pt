---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 09/24/2018
ms.author: wolfma
ms.openlocfilehash: dc2e025cdd9fcc153f3cb81988a9ca3ec729c934
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "47021147"
---
<!-- N.B. no header, language-agnostic -->

Os serviços cognitivos da Microsoft [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece a forma mais simples de usar **tradução por voz** em seu aplicativo.
O SDK fornece todas as funcionalidades do serviço. O processo básico para a execução de tradução de voz inclui os seguintes passos:

1. Criar uma configuração de tradução de voz e fornecer uma chave de subscrição do serviço de voz (ou um token de autorização) e um [região](~/articles/cognitive-services/speech-service/regions.md) como parâmetros. Altere a configuração, conforme necessário. Por exemplo, pode configurar os idiomas de tradução de origem e destino, bem como especificar se pretende que a saída de texto ou de voz.

1. Crie um reconhecedor de tradução da fábrica de voz. Forneça uma configuração de áudio se pretender que a reconhecer a partir de uma origem diferente de seu microfone padrão (por exemplo, o fluxo de áudio ou o arquivo de áudio).

1. Obstruir os eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais, bem como um evento de síntese da saída de áudio opcional. Caso contrário, seu aplicativo recebe apenas um resultado final transcrição.

1. Inicie o reconhecimento. Para a tradução única utilizar o `RecognizeOnceAsync()` método, que retorna a primeira expressão reconhecido. Para traduções de longa execução, utilize o `StartContinuousRecognitionAsync()` método e empate os eventos para resultados de reconhecimento assíncrono.

Consulte os seguintes fragmentos de código para cenários de tradução de voz que utilizam o SDK de voz.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
