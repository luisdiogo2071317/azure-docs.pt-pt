---
title: Reconhecer a conversão de voz com a API de REST
description: Saiba como utilizar a conversão de voz em texto no serviço de voz
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 07/16/2018
ms.author: v-jerkin
ms.openlocfilehash: 24fa3882a65bf6605444a139ad5d4ee42800a8ef
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/23/2018
ms.locfileid: "42744934"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Reconhecer a conversão de voz com a API de REST

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

A API REST pode servir para reconhecer expressões com curtos usando um pedido HTTP POST.

A API REST é a forma mais simples de reconhecer a conversão de voz, se não estiver a utilizar um idioma suportado pelos [SDK](speech-sdk.md).
Pode fazer um HTTP POST para o ponto final de serviço do pedido, transmitir a expressão inteira no corpo do pedido; e receberá uma resposta que contém o texto reconhecido.

> [!NOTE]
> Expressões com são limitadas para 15 segundos ou menos ao utilizar a API de REST.
> Veja a [SDK de voz](how-to-recognize-speech-csharp.md) para reconhecimento de expressões com mais tempo.

Para obter mais informações sobre o **conversão de voz em texto** API de REST, veja [REST APIs](rest-apis.md#speech-to-text). Para ver em ação, transfira o [exemplos de REST API](https://github.com/Azure-Samples/SpeechToText-REST) do GitHub.

## <a name="next-steps"></a>Passos Seguintes

- [Consulte a visão geral da REST API](rest-apis.md)
