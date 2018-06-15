---
title: Traduzir voz utilizando os serviços de reconhecimento de voz | Microsoft Docs
description: Saiba como utilizar a tradução de reconhecimento de voz no serviço de reconhecimento de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 7f39f284998489574049d82c44b3d3a0a3797adb
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/07/2018
ms.locfileid: "35356287"
---
# <a name="translate-speech-using-speech-service"></a>Traduzir voz utilizando o serviço de reconhecimento de voz

O [SDK de reconhecimento de voz](speech-sdk.md) é a forma mais simples para utilizar a tradução de reconhecimento de voz na sua aplicação. O SDK fornece todas as funcionalidades do serviço. O processo básico para efetuar a conversão de reconhecimento de voz inclui os seguintes passos:

1. Criar uma fábrica de reconhecimento de voz e fornecer uma chave de subscrição do serviço de reconhecimento de voz ou um token de autorização. Também configurar os idiomas de tradução de origem e destino neste momento, bem como especificar se pretende que a saída de texto ou de voz.

2. Obter um reconhecedor a fábrica. Para tradução, selecione um utilitário de conversão. (Destinam-se a outros recognizers *reconhecimento de voz para texto*.) Existem vários tipos de reconhecedor de tradução com base na origem de áudio que estiver a utilizar.

4. Associar eventos para a operação assíncrona, se assim o desejar. O reconhecedor chama os processadores de eventos quando tem resultados intermédio e finais. Caso contrário, a aplicação recebe um resultado de tradução final.

5. Inicie o reconhecimento e a tradução.

# <a name="sdk-samples"></a>Exemplos SDK

Para o conjunto mais recente de exemplos, consulte o [repositório cognitivos GitHub de exemplo do SDK de reconhecimento de voz de serviços](https://aka.ms/csspeech/samples).

# <a name="next-steps"></a>Passos Seguintes

- [Obter a sua subscrição de avaliação de reconhecimento de voz](https://azure.microsoft.com/try/cognitive-services/)
- [Reconhecer o reconhecimento de voz em c#](quickstart-csharp-windows.md)
