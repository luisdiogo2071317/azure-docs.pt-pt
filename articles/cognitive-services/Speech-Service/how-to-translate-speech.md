---
title: Tradução de voz com serviços de voz
description: Saiba como utilizar a tradução de voz no serviço de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: d539fe5a1a031c196c0d40e989d83575715b278a
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285266"
---
# <a name="translate-speech-using-speech-service"></a>Tradução de voz com o serviço de voz

O [SDK de voz](speech-sdk.md) é a forma mais simples de utilizar a tradução de voz na sua aplicação. O SDK fornece todas as funcionalidades do serviço. O processo básico para a execução de tradução de voz inclui os seguintes passos:

1. Criar uma fábrica de voz e fornecer uma chave de subscrição do serviço de voz e [região](regions.md) ou um token de autorização. Também é configurar os idiomas de tradução de origem e destino neste momento, bem como especificar se pretende que a saída de texto ou de voz.

2. Obtenha um reconhecedor da fábrica. Para a tradução, selecione um reconhecedor de tradução. (Os reconhecedores tenham destinam *conversão de voz em texto*.) Existem vários tipos do reconhecedor de tradução com base na origem de áudio que estiver a utilizar.

4. Vincular eventos para uma operação assíncrona, se assim o desejar. O reconhecedor chama seus manipuladores de eventos quando tem resultados intermediárias e finais. Caso contrário, seu aplicativo recebe um resultado final de tradução.

5. Inicie o reconhecimento e a tradução.

# <a name="sdk-samples"></a>Amostras de SDK

Para o conjunto mais recente de exemplos, consulte a [cognitivos GitHub de exemplo do SDK de voz de serviços repositório](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Passos Seguintes

- [Obter a subscrição de avaliação de Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconhecer a conversão de voz em c#](quickstart-csharp-dotnet-windows.md)
