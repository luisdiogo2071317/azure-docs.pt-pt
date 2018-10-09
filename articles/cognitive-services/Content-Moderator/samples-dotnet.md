---
title: Exemplos de código – Content Moderator, .NET
description: Utilizar o Content Moderator nas suas aplicações
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: sample
ms.date: 01/10/2018
ms.author: sajagtap
ms.openlocfilehash: c5465a2de0924037eb93b9de652e93e85f166737
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223156"
---
# <a name="net-sdk-samples"></a>Exemplos de SDK .NET

A lista seguinte inclui ligações para exemplos de código criados com o SDK do Azure Content Moderator para .NET.

- **Biblioteca auxiliar**: [Criar um cliente Content Moderator para utilização nos outros exemplos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Veja o [guia de início rápido](content-moderator-helper-quickstart-dotnet.md).

## <a name="moderation"></a>Moderação

- **Moderação de imagens**: [avalie uma imagem relativamente a conteúdo para adultos e indecoroso, texto e rostos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Veja o [guia de início rápido](image-moderation-quickstart-dotnet.md).
- **Imagens personalizadas**: [moderar com listas personalizadas de imagens](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Veja o [guia de início rápido](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Existe um limite máximo de **5 listas de imagens**, sendo que cada lista **não pode exceder 10 000 imagens**.
>

- **Moderação de texto**: [analise texto relativamente a linguagem inapropriada e informação pessoal (PII)](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Veja o [guia de início rápido](text-moderation-quickstart-dotnet.md).
- **Termos personalizados**: [Moderar com listas personalizadas de termos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Veja o [guia de início rápido](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Existe um limite máximo de **5 listas de termos**, sendo que cada lista **não pode exceder 10 000 termos**.
>

- **Moderação de vídeos**: [analise um vídeo relativamente a conteúdo para adultos e indecoroso, e obtenha resultados](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Veja o [guia de início rápido](video-moderation-api.md).

## <a name="review"></a>Rever

- **Tarefas de imagem**: [inicie uma tarefa de moderação que analisa e cria revisões](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Veja o [guia de início rápido](moderation-jobs-quickstart-dotnet.md).
- **Revisões de imagem**: [crie as revisões para interação humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Veja o [guia de início rápido](moderation-reviews-quickstart-dotnet.md).
- **Revisões de vídeo**: [crie revisões de vídeos para interação humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Ver o [guia de início rápido](video-reviews-quickstart-dotnet.md)
- **Revisões de conversão de vídeo**: [crie revisões de conversão de vídeo para interação humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) Ver o [guia de início rápido](video-reviews-quickstart-dotnet.md)

Veja todos os exemplos de .NET nos [exemplos de .NET do Content Moderator no GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).
