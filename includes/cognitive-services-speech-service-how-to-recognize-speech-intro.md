---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: ca342a812f6a8de19c732b5df4fab14a825f6c51
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39330928"
---
<!-- N.B. no header, no intents here, language-agnostic -->

O [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece a forma mais simples de usar **voz em texto** em seu aplicativo com funcionalidade completa.

1. Criar uma fábrica de voz, fornecendo uma chave de subscrição do serviço de voz ou um token de autorização e um [região](~/articles/cognitive-services/speech-service/regions.md).
   Também pode configurar opções, como o idioma do reconhecimento ou um ponto final personalizado para seus próprios modelos de reconhecimento de voz.

1. Obtenha um reconhecedor de voz da fábrica de voz.
   Um reconhecedor pode utilizar o microfone de padrão do seu dispositivo, um fluxo de áudio ou áudio de um ficheiro.

1. Obstruir os eventos para a operação assíncrona, se assim o desejar.
   O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais.
   Caso contrário, a aplicação irá receber um resultado final transcrição.

1. Inicie o reconhecimento.
   Para o reconhecimento de única, como reconhecimento de comando ou uma consulta, utilize `RecognizeAsync()`, que retorna a primeira expressão a ser reconhecido.
   Para o reconhecimento de longa execução, como transcrição, utilize `StartContinuousRecognitionAsync()` automático e ligar os eventos para resultados de reconhecimento assíncrono.

Veja a seguir, vamos mostrar vários fragmentos de código para cenários de reconhecimento de fala com o SDK de voz.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
