---
title: Reconhecer a conversão de voz com a API de REST
description: Saiba como utilizar a conversão de voz em texto API no serviço de voz de serviços cognitivos.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: erhopf
ms.openlocfilehash: 4e87feae7e3321efe7adcdbf1302762f539f034d
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167278"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Reconhecer a conversão de voz com a API de REST

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

A API REST pode servir para reconhecer expressões com curtos usando um pedido HTTP POST.

A API REST é a forma mais simples de reconhecer a conversão de voz, se não estiver a utilizar um idioma que é suportado pelos [SDK](speech-sdk.md). Fazer uma solicitação de HTTP POST para o ponto final de serviço e passar a expressão inteira no corpo do pedido. Receber uma resposta que tem o texto reconhecido.

> [!NOTE]
> Expressões com estão limitados a 15 segundos ou menos ao utilizar a API REST.
> Veja a [SDK de voz](how-to-recognize-speech-csharp.md) para reconhecimento de expressões com mais tempo.

Para obter mais informações sobre o **conversão de voz em texto** API de REST, consulte a [REST APIs](rest-apis.md#speech-to-text) artigo. Para ver a API em ação, transfira o [exemplos de REST API](https://github.com/Azure-Samples/SpeechToText-REST) do GitHub.

## <a name="next-steps"></a>Passos Seguintes

- Consulte a [descrição geral da API de REST](rest-apis.md).
