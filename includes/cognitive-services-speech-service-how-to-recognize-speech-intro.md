---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: d7d73d9c5e85ac550b24c73797d536dc4d0fc6ed
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39346273"
---
<!-- N.B. no header, no intents here, language-agnostic -->

O [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece a forma mais simples de usar **voz em texto** em seu aplicativo com funcionalidade completa.

1. Criar uma fábrica de voz, fornecendo uma chave de subscrição do serviço de voz ou um token de autorização e um [região](~/articles/cognitive-services/speech-service/regions.md). Também pode especificar um ponto final personalizado para especificar um ponto de extremidade de serviço não padrão.

1. Obtenha um reconhecedor de voz da fábrica de voz. Pode configurar o idioma de entrada, bem como o formato de saída.  Um reconhecedor pode utilizar o microfone de padrão do seu dispositivo, um fluxo de áudio ou áudio de um ficheiro.

1. Obstruir os eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais.  Caso contrário, a aplicação irá receber um resultado final transcrição.

1. Inicie o reconhecimento. Para o reconhecimento de única, como reconhecimento de comando ou uma consulta, utilize `RecognizeAsync()`, que retorna a primeira expressão a ser reconhecido. Para o reconhecimento de longa execução, como transcrição, utilize `StartContinuousRecognitionAsync()` automático e ligar os eventos para resultados de reconhecimento assíncrono.

Consulte os trechos de código abaixo para cenários de reconhecimento de fala com o SDK de voz.

[!include[Get a Subscription Key](cognitive-services-speech-service-get-subscription-key.md)]
