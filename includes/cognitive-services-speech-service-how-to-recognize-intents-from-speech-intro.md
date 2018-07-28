---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 1b55576581ebddc90c0af6b99a04dbe66d2e3b87
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39330910"
---
<!-- N.B. no header, language-agnostic -->

O [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece uma forma de reconhecer intenções de conversão de voz, com tecnologia do serviço de voz e o [serviço de compreensão de idiomas (LUIS)](https://luis.ai).

1. Criar uma fábrica de voz, fornecendo uma chave de subscrição do serviço de compreensão de idiomas e [região](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition).


1. Obtenha um reconhecedor intenção da fábrica de voz.
   Um reconhecedor pode utilizar o microfone de padrão do seu dispositivo, um fluxo de áudio ou áudio de um ficheiro.

1. Obstruir os eventos para a operação assíncrona, se assim o desejar.
   O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais.
   Caso contrário, a aplicação irá receber um resultado final transcrição.

1. Inicie o reconhecimento.
   Para o reconhecimento de única, como reconhecimento de comando ou uma consulta, utilize `RecognizeAsync()`, que retorna a primeira expressão a ser reconhecido.
   Para o reconhecimento de longa execução, como transcrição, utilize `StartContinuousRecognitionAsync()` automático e ligar os eventos para resultados de reconhecimento assíncrono.

Veja a seguir, vamos mostrar vários fragmentos de código para cenários de reconhecimento da intenção utilizando o SDK de voz.

> [!NOTE]
> Primeiro obtenha uma chave de subscrição.
> Ao contrário de outros serviços suportados pelo SDK de voz, reconhecimento da intenção requer uma chave de subscrição específica.
> [Aqui](https://www.luis.ai) pode encontrar informações adicionais sobre a tecnologia de reconhecimento da intenção, bem como informações sobre como adquirir uma chave de subscrição.
> Substitua a sua própria chave de subscrição de compreensão de idiomas, o [região da sua subscrição](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition)e o AppId do seu modelo de intenção nos lugares apropriados nos exemplos.
