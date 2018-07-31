---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 74b9da1b3b81f9f35f231ef5caef8eafedc9aefc
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39343039"
---
<!-- N.B. no header, language-agnostic -->

O [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece a forma mais simples de usar **tradução por voz** em seu aplicativo.
O SDK fornece todas as funcionalidades do serviço. O processo básico para a execução de tradução de voz inclui os seguintes passos:

1. Criar uma fábrica de voz, fornecendo uma chave de subscrição do serviço de voz ou um token de autorização e um [região](~/articles/cognitive-services/speech-service/regions.md).
   
1. Crie um reconhecedor de tradução da fábrica de voz. Pode configurar os idiomas de tradução de origem e destino, bem como especificar se pretende que a saída de texto ou de voz. Existem vários tipos de reconhecedores de tradução com base na origem de áudio que estiver a utilizar.

1. Obstruir os eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais, bem como um evento de síntese da saída de áudio opcional. Caso contrário, a aplicação irá receber um resultado final transcrição.

1. Inicie o reconhecimento. Para utilização de tradução única RecognizeAsync(), que retorna a primeira expressão a ser reconhecido. Para traduções de longa execução, utilize StartContinuousRecognitionAsync() e resolvem os eventos para resultados de reconhecimento assíncrono.

Consulte os trechos de código abaixo para cenários de tradução de voz com o SDK de voz.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
