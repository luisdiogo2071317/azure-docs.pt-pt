---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 07/27/2018
ms.author: wolfma
ms.openlocfilehash: 79dc1fe9fffd8c44c97cf68a12bb90655b672577
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/06/2018
ms.locfileid: "39577980"
---
<!-- N.B. no header, language-agnostic -->

O [SDK de voz](~/articles/cognitive-services/speech-service/speech-sdk.md) fornece uma forma de reconhecer **objetivos do discurso**, fornecida pelo serviço de voz e o [serviço de compreensão de idiomas (LUIS)](https://www.luis.ai/home).

1. Criar uma fábrica de voz, fornecendo uma chave de subscrição do serviço de compreensão de idiomas e [região](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition). A chave de subscrição do serviço de compreensão de idiomas é chamada **chave de ponto final** na documentação do serviço. Não é possível utilizar o serviço de compreensão de idiomas a chave de criação. Consulte também os **nota** abaixo.

1. Obtenha um reconhecedor intenção da fábrica de voz. Um reconhecedor pode utilizar o microfone de padrão do seu dispositivo, um fluxo de áudio ou áudio de um ficheiro.

1. Obtenha o modelo de compreensão de idioma com base na sua AppId e adicionar intenções que necessitar. 

1. Obstruir os eventos para a operação assíncrona, se assim o desejar. O reconhecedor, em seguida, chama seus manipuladores de eventos quando tem resultados intermediárias e finais (incluindo objetivos). Caso contrário, a aplicação irá receber um resultado final transcrição.

1. Inicie o reconhecimento da intenção. Para o reconhecimento de única, como reconhecimento de comando ou uma consulta, utilize `RecognizeAsync()`, que retorna a primeira expressão a ser reconhecido. Para reconhecimento de longa execução utilizar `StartContinuousRecognitionAsync()` automático e ligar os eventos para resultados de reconhecimento assíncrono.

Consulte os trechos de código abaixo para cenários de reconhecimento da intenção utilizando o SDK de voz. Substitua a sua própria chave de subscrição de compreensão de idiomas (chave de ponto final), o [região da sua subscrição](~/articles/cognitive-services/speech-service/regions.md#regions-for-intent-recognition)e o AppId do seu modelo de intenção nos lugares apropriados nos exemplos.

> [!NOTE]
> Ao contrário de outros serviços suportados pelo SDK de voz, reconhecimento da intenção requer uma chave de subscrição específica (chave de ponto final de serviço de compreensão de idiomas). [Aqui](https://www.luis.ai) pode encontrar informações adicionais sobre a tecnologia de reconhecimento da intenção. Como adquirir o **chave de ponto final** descrita [aqui](https://docs.microsoft.com/en-us/azure/cognitive-services/LUIS/luis-how-to-azure-subscription#create-luis-endpoint-key).
