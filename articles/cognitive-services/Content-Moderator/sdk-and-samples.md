---
title: SDKs e exemplos - Content Moderator, Python, Java, Node.js e .NET
titlesuffix: Azure Cognitive Services
description: Obter SDKs e exemplos do Content Moderator
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: sample
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: a57f6a312b00d7ec3d927c6fda319f1de8663c9c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220455"
---
# <a name="content-moderator-sdks-and-samples"></a>SDKs e exemplos do Content Moderator

## <a name="sdks-for-python-java-nodejs-and-net"></a>SDKs de Python, Java, Node.js e .NET

- [SDK do Content Moderator para Python](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [SDK do Content Moderator para Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [SDK do Content Moderator para Node.js](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [SDK do Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>Exemplos de SDK .NET

A lista seguinte inclui ligações para exemplos de código criados com o SDK do Azure Content Moderator para .NET.

- **Biblioteca auxiliar**: [crie um cliente Content Moderator para utilização nos outros exemplos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Veja o [início rápido](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Moderação 
- **Moderação de imagens**: [avalie uma imagem relativamente a conteúdo para adultos e indecoroso, texto e rostos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Veja o [início rápido](image-moderation-quickstart-dotnet.md).
- **Imagens personalizadas**: [moderar com listas personalizadas de imagens](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Veja o [início rápido](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Existe um limite máximo de **5 listas de imagens**, sendo que cada lista **não pode exceder 10 000 imagens**.
>

- **Moderação de texto**: [analise texto relativamente a linguagem inapropriada e informação pessoal (PII)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Veja o [início rápido](text-moderation-quickstart-dotnet.md).
- **Termos personalizados**: [modere com listas personalizadas de termos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Veja o [início rápido](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Existe um limite máximo de **5 listas de termos**, sendo que cada lista **não pode exceder 10 000 termos**.
>

- **Moderação de vídeos**: [analise um vídeo relativamente a conteúdo para adultos e indecoroso, e obtenha resultados](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Veja o [início rápido](video-moderation-api.md).

### <a name="review"></a>Rever
- **Tarefas de imagem**: [inicie uma tarefa de moderação que analisa e cria revisões](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Veja o [início rápido](moderation-jobs-quickstart-dotnet.md).
- **Revisões de imagem**: [crie as revisões para interação humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Veja o [início rápido](moderation-reviews-quickstart-dotnet.md).
- **Revisões de vídeo**: [crie revisões de vídeos para interação humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Ver o [início rápido](video-reviews-quickstart-dotnet.md)
- **Revisões de conversão de vídeo**: [crie revisões de conversão de vídeo para interação humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Ver o [início rápido](video-reviews-quickstart-dotnet.md)

Veja todos os exemplos de .NET nos [exemplos de .NET do Content Moderator no GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).

## <a name="rest-api-samples-in-c"></a>Exemplos de API REST em C#

- [Moderação de imagens](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [Moderação de texto](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [Moderação de vídeo](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [Revisões de imagem](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [Tarefas de imagem](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

Para instruções destes exemplos, consulte o [webinar a pedido](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html).

## <a name="tutorials"></a>Tutoriais
- [Moderação do catálogo de eCommerce](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration). Veja o [tutorial](ecommerce-retail-catalog-moderation.md).
- [Moderação de conteúdos do Facebook](https://github.com/MicrosoftContentModerator/samples-fbPageModeration). Veja o [tutorial](facebook-post-moderation.md).
- [Solução de moderação e revisão de conversão e vídeo ](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) Ver [tutorial](video-transcript-moderation-review-tutorial-dotnet.md)

## <a name="on-demand-webinars"></a>Webinars a pedido
- [Moderação de conteúdos assistida por computador à escala com o Content Moderator](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)
