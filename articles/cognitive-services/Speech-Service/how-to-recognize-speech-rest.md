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
ms.openlocfilehash: 15db2d9b872c76d70fd531af07fb55c701e86494
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331478"
---
# <a name="recognize-speech-by-using-the-rest-api"></a>Reconhecer a conversão de voz com a API de REST

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-speech-selector.md)]

A API REST pode servir para reconhecer expressões com curtos usando um pedido HTTP POST.

A API REST é a forma mais simples para reconhecer a voz, se não estiver a utilizar um idioma suportado pelo SDK.
Fazer um pedido HTTP POST para o ponto final de serviço, passando a expressão inteira no corpo do pedido.
Receber uma resposta que contém o texto reconhecido.

> [!NOTE]
> Expressões com são limitadas para 15 segundos ou menos ao utilizar a API de REST.
> Veja a [SDK de voz](how-to-recognize-speech-csharp.md) para reconhecimento de expressões com mais tempo.

Para obter mais informações sobre o **conversão de voz em texto** API de REST, veja [REST APIs](rest-apis.md#speech-to-text). Para ver em ação, transfira o [exemplos de REST API](https://github.com/Azure-Samples/SpeechToText-REST) do GitHub.

## <a name="next-steps"></a>Passos Seguintes

- [Consulte a visão geral da REST API](rest-apis.md)
