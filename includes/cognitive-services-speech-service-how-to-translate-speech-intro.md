---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: ee50a104a75d3cd5ff958bd49a1ff7010c5d5083
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43144561"
---
<!-- N.B. no header, language-agnostic -->

Os serviços cognitivos da Microsoft [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece a forma mais simples de usar **tradução por voz** em seu aplicativo.
O SDK fornece todas as funcionalidades do serviço. O processo básico para a execução de tradução de voz inclui os seguintes passos:

1. Criar uma fábrica de voz e fornecer uma chave de subscrição do serviço de voz ou um token de autorização e um [região](~/articles/cognitive-services/speech-service/regions.md) como parâmetros.
   
1. Crie um reconhecedor de tradução da fábrica de voz. Pode configurar os idiomas de tradução de origem e destino, bem como especificar se pretende que a saída de texto ou de voz. Existem vários tipos de reconhecedores de tradução com base na origem de áudio que utilizar.

1. Obstruir os eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais, bem como um evento de síntese da saída de áudio opcional. Caso contrário, seu aplicativo recebe apenas um resultado final transcrição.

1. Inicie o reconhecimento. Para a tradução única utilizar o `RecognizeAsync()` método, que retorna a primeira expressão reconhecido. Para traduções de longa execução, utilize o `StartContinuousRecognitionAsync()` método e empate os eventos para resultados de reconhecimento assíncrono.

Consulte os seguintes fragmentos de código para cenários de tradução de voz que utilizam o SDK de voz.

[!INCLUDE [Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
