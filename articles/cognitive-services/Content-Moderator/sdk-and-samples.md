---
title: Moderação interrupção SDKs e amostras de conteúdo para o Moderator de conteúdo do Azure | Microsoft Docs
description: Obter SDKs e ferramentas para Moderator conteúdo
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: 40b8fc0f63383e837f0813f876f20806e6e8c6eb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35351739"
---
# <a name="content-moderator-sdks-and-samples"></a>Conteúdo Moderator SDKs e amostras

## <a name="sdks-for-python-java-nodejs-and-net"></a>SDKs para Python, Java, Node.js e .NET

- [Conteúdo Moderator SDK para Python](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [Conteúdo Moderator SDK para Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [Conteúdo Moderator SDK para Node.js](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [Conteúdo Moderator SDK para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>Exemplos de SDK do .NET

A lista seguinte inclui ligações para os exemplos de código criadas utilizando o SDK de Moderator conteúdo do Azure para .NET.

- **Biblioteca auxiliar**: [criar um cliente Moderator conteúdo para utilização nos outros exemplos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Consulte [início rápido](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Moderação interrupção 
- **Imagem moderação interrupção**: [avaliar uma imagem de conteúdo para adultos e racy, texto e faces](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Consulte [início rápido](image-moderation-quickstart-dotnet.md).
- **Imagens personalizadas**: [moderada com listas de imagem personalizada](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Consulte [início rápido](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Há um limite máximo de **apresenta uma lista de imagem de 5** com cada lista para **não pode exceder 10 000 imagens**.
>

- **Moderação de interrupção de texto**: [ecrã de texto para profanity e informações de identificação pessoal (PII)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Consulte [início rápido](text-moderation-quickstart-dotnet.md).
- **Termos personalizados**: [moderada com listas de termo personalizado](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Consulte [início rápido](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Há um limite máximo de **apresenta uma lista de 5 termo** com cada lista para **não pode exceder 10 000 termos**.
>

- **Moderação de interrupção gráfica**: [analisar um vídeo para o conteúdo para adultos e racy e obter resultados](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Consulte [início rápido](video-moderation-api.md).

### <a name="review"></a>Rever
- **As tarefas de imagem**: [iniciar uma tarefa de moderação interrupção que analisa e cria revisões](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Consulte [início rápido](moderation-jobs-quickstart-dotnet.md).
- **Imagem revisões**: [criar revisões humanos no ciclo for](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Consulte [início rápido](moderation-reviews-quickstart-dotnet.md).
- **Vídeo revê**: [revê as vídeo de criar para humanos no ciclo](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Consulte [início rápido](video-reviews-quickstart-dotnet.md)
- **Vídeo transcript revê**: [transcript vídeo criar revê humanos no ciclo for](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) consulte [início rápido](video-reviews-quickstart-dotnet.md)

Consulte todas as amostras de .NET no [amostras de conteúdo Moderator .NET no GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).

## <a name="rest-api-samples-in-c"></a>Exemplos de REST API em c#

- [Moderação de imagens](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [Moderação de texto](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [Moderação de vídeo](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [Revisões de imagem](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [Tarefas de imagem](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

Para instruções destes exemplos, veja o [a pedido webinar](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html).

## <a name="tutorials"></a>Tutoriais
- [Moderação de interrupção do eCommerce catálogo](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration). Consulte [tutorial](ecommerce-retail-catalog-moderation.md).
- [Moderação de interrupção conteúda do Facebook](https://github.com/MicrosoftContentModerator/samples-fbPageModeration). Consulte [tutorial](facebook-post-moderation.md).
- [Solução de moderação interrupção e revisão de vídeos e transcript](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp) consulte [tutorial](video-transcript-moderation-review-tutorial-dotnet.md)

## <a name="on-demand-webinars"></a>Webinars a pedido
- [Assistida por máquina moderação de interrupção conteúda à escala com Moderator de conteúdo](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)
