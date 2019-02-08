---
title: Reconhecer a conversão de voz com a API de REST
titleSuffix: Azure Cognitive Services
description: Saiba como utilizar a conversão de voz em texto API no serviço de voz de serviços cognitivos.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: 1d8ae196884d800f441943609ac0189260a0ffcf
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55865741"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Reconhecer a conversão de voz com a API de REST

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

A API REST pode servir para reconhecer expressões com curtos usando um pedido HTTP POST.

A API REST é a forma mais simples de reconhecer a conversão de voz, se não estiver a utilizar um idioma que é suportado pelos [SDK](speech-sdk.md). Fazer uma solicitação de HTTP POST para o ponto final de serviço e passar a expressão inteira no corpo do pedido. Receber uma resposta que tem o texto reconhecido.

> [!NOTE]
> Expressões com estão limitados a 15 segundos ou menos ao utilizar a API REST.
> Veja a [SDK de voz](how-to-recognize-speech-csharp.md) para reconhecimento de expressões com mais tempo.

Para obter mais informações sobre o **conversão de voz em texto** API de REST, consulte a [REST APIs](rest-apis.md#speech-to-text-api) artigo. Para ver a API em ação, transfira o [exemplos de REST API](https://github.com/Azure-Samples/SpeechToText-REST) do GitHub.

## <a name="next-steps"></a>Passos Seguintes

- Consulte a [descrição geral da API de REST](rest-apis.md).
