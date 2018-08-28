---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: b46eba502462e0cdbcb222082c5c885f84a8d1e5
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/27/2018
ms.locfileid: "43090681"
---
<!-- N.B. no header, no intents here, language-agnostic -->

Os serviços cognitivos [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece a forma mais simples de usar **voz em texto** em seu aplicativo com funcionalidade completa.

1. Criar uma fábrica de voz e fornecer uma chave de subscrição do serviço de voz (ou um token de autorização) e um [região](~/articles/cognitive-services/speech-service/regions.md) como parâmetros. Também pode fornecer um ponto final personalizado para especificar um ponto de extremidade de serviço não padrão.

1. Obtenha um reconhecedor de voz da fábrica de voz. Pode configurar o idioma de entrada e o formato de saída. Um reconhecedor pode utilizar o microfone de padrão do seu dispositivo, um fluxo de áudio ou áudio de um ficheiro.

1. Obstruir os eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais. Caso contrário, seu aplicativo recebe apenas um resultado final transcrição.

1. Inicie o reconhecimento. Para o reconhecimento de única, como o reconhecimento de comando ou uma consulta, utilize o `RecognizeAsync()` método. Este método devolve a primeira expressão reconhecido. Para o reconhecimento de longa execução, como de transcrição, utilize o `StartContinuousRecognitionAsync()` método. Obstruir os eventos para resultados de reconhecimento assíncrono.

Consulte os seguintes fragmentos de código para cenários de reconhecimento de fala que utilizam o SDK de voz.

[!include[Get a subscription key](cognitive-services-speech-service-get-subscription-key.md)]
