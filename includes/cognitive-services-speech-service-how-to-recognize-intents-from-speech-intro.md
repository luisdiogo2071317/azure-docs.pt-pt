---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: a4accb0846ec9cf5efc5ca5aa00c731edd41d4d4
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/28/2018
ms.locfileid: "43143902"
---
<!-- N.B. no header, language-agnostic -->

Os serviços cognitivos da Microsoft [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece uma forma de reconhecer **objetivos do discurso** e é suportada pelos serviços cognitivos [o serviço de compreensão de idiomas (LUIS)](https://www.luis.ai/home).

1. Criar uma fábrica de voz com uma chave de assinatura de LUIS e [região](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition) como parâmetros. Denomina-se a chave de subscrição de LUIS **chave de ponto final** na documentação do serviço. Não é possível utilizar o LUIS chave de criação. (Consulte a observação posteriormente nesta secção).

1. Obtenha um reconhecedor intenção da fábrica de voz. Um reconhecedor pode utilizar o microfone de padrão do seu dispositivo, um fluxo de áudio ou áudio de um ficheiro.

1. Obter o modelo de compreensão de idioma com base no seu **AppId**. Adicione intenções que necessitar. 

1. Obstruir os eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais (inclui intenções). Se não resolvem os eventos, o seu aplicativo recebe apenas um resultado final transcrição.

1. Inicie o reconhecimento da intenção. Para o reconhecimento de única, como o reconhecimento de comando ou uma consulta, utilize o `RecognizeAsync()` método. Este método devolve a primeira expressão reconhecido. Para o reconhecimento de longa execução, utilize o `StartContinuousRecognitionAsync()` método. Obstruir os eventos para resultados de reconhecimento assíncrono.

Consulte os seguintes fragmentos de código para cenários de reconhecimento da intenção que utilizam o SDK de voz. Substitua os valores no exemplo a sua própria chave de subscrição de LUIS (chave de ponto final), o [região da sua subscrição](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition)e o **AppId** do seu modelo de intenção.

> [!NOTE]
> Ao contrário de outros serviços suportados pelo SDK de voz, reconhecimento da intenção requer uma chave de subscrição específica (chave de ponto final de LUIS). Para obter informações sobre a tecnologia de reconhecimento da intenção, consulte a [Web site do LUIS](https://www.luis.ai). Para obter informações sobre como adquirir o **chave de ponto final**, consulte [crie uma chave de ponto final do LUIS](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-how-to-azure-subscription#create-luis-endpoint-key).
