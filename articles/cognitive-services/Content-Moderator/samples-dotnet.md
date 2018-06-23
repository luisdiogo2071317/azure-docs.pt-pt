---
title: Exemplos de código Moderator conteúdo do Azure | Microsoft Docs
description: Utilizar conteúdo Moderator nas aplicações
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2018
ms.author: sajagtap
ms.openlocfilehash: a7f5b0a7433631e303de47667871cc1354053c08
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352568"
---
# <a name="net-sdk-samples"></a>Exemplos de SDK do .NET

A lista seguinte inclui ligações para os exemplos de código criadas utilizando o SDK de Moderator conteúdo do Azure para .NET.

- **Biblioteca auxiliar**: [criar um cliente Moderator conteúdo para utilização nos outros exemplos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Consulte [início rápido](content-moderator-helper-quickstart-dotnet.md).

## <a name="moderation"></a>Moderação interrupção

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

## <a name="review"></a>Rever

- **As tarefas de imagem**: [iniciar uma tarefa de moderação interrupção que analisa e cria revisões](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Consulte [início rápido](moderation-jobs-quickstart-dotnet.md).
- **Imagem revisões**: [criar revisões humanos no ciclo for](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Consulte [início rápido](moderation-reviews-quickstart-dotnet.md).
- **Vídeo revê**: [revê as vídeo de criar para humanos no ciclo](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Consulte [início rápido](video-reviews-quickstart-dotnet.md)
- **Vídeo transcript revê**: [transcript vídeo criar revê humanos no ciclo for](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs) consulte [início rápido](video-reviews-quickstart-dotnet.md)

Consulte todas as amostras de .NET no [amostras de conteúdo Moderator .NET no GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).
