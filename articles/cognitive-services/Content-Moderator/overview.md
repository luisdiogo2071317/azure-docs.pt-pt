---
title: O que é o Content Moderator do Azure?
titlesuffix: Azure Cognitive Services
description: Saiba como utilizar o Content Moderator para controlar, sinalizar, avaliar e filtrar informação inadequada em conteúdos gerados pelo utilizador.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: overview
ms.date: 10/05/2018
ms.author: sajagtap
ms.openlocfilehash: 5756e8fb451b073c68271359848ab27373ad85ed
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309557"
---
# <a name="what-is-content-moderator"></a>O que é o Content Moderator?

A moderação de conteúdos é o processo de monitorização de texto, imagem ou conteúdo de vídeo para obter informações potencialmente ofensivas, indesejáveis ou de risco. O conteúdo sinalizado pode, em seguida, ser ocultado ou, caso contrário, resolvido para cumprir os regulamentos ou manter um ambiente desejado para os utilizadores.

## <a name="where-it-is-used"></a>Onde é utilizado

A lista seguinte mostra alguns cenários de exemplo onde o Content Moderator pode ser utilizado:

- Mercados online que moderam catálogos de produtos e conteúdos gerados pelo utilizador
- Empresas de jogos que moderam artefactos de jogo e salas de conversa gerados pelo utilizador
- Plataformas de redes sociais que moderam as imagens, o texto e os vídeos adicionados pelos utilizadores
- Empresas multimédia empresariais que implementam a moderação de conteúdos centralizada dos respetivos conteúdos
- Fornecedores de soluções de educação primária e secundária que filtram conteúdo inadequado e ofensivo para estudantes e educadores

## <a name="what-it-includes"></a>O que inclui

O Content Moderator consiste em várias APIs de serviço Web e uma ferramenta de revisão com interação humana incorporada, que ajuda a moderar imagens, texto e vídeos.

![Diagrama de bloco do Content Moderator](images/content-moderator-block-diagram.png)

### <a name="apis"></a>APIs

O serviço Content Moderator inclui as seguintes APIs:
  - [**API de moderação de texto**](text-moderation-api.md): utilize esta API para analisar o texto relativamente a possível linguagem inapropriada, explícita, sugestiva, ofensiva e informação pessoal (PII).
  - [**API de lista personalizada de termos**](try-terms-list-api.md): utilize esta API para comparar com listas personalizadas de termos, além dos termos incorporados. Utilize estas listas para bloquear ou permitir conteúdo de acordo com as suas políticas de conteúdo.  
  - [**API de moderação de imagens**](image-moderation-api.md): utilize esta API para procurar imagens com conteúdos para adultos e indecorosos, detetar texto em imagens com a função de OCR (reconhecimento ótico de carateres) e detetar rostos.
  - [**API de lista personalizada de imagens**](try-image-list-api.md): utilize esta API para comparar a listas personalizadas de imagens, que incluem conteúdo previamente identificado que não precisa de classificar novamente.
  - [**API de moderação de vídeo**](video-moderation-api.md): utilize esta API para analisar vídeos relativamente a potenciais conteúdos para adultos e indecorosos.
  - [**Reveja as APIs**](try-review-api-job.md): utilize as operações [Tarefas](try-review-api-job.md), [Revisões](try-review-api-review.md) e [Fluxo de Trabalho](try-review-api-workflow.md) para criar e automatizar fluxos de trabalho com interação humana na ferramenta de revisão.

### <a name="human-review-tool"></a>Ferramenta de revisão humana

A sua subscrição do Content Moderator inclui a [ferramenta de revisão humana](Review-Tool-User-Guide/human-in-the-loop.md) incorporada. Utilize a API de Revisão mencionada anteriormente para criar revisões de texto, imagens e vídeos para que os seus moderadores humanos tomem as decisões finais.

![Ferramenta de revisão de vídeo do Content Moderator](images/video-review-default-view.png)

## <a name="next-steps"></a>Passos seguintes

Utilize o [Início Rápido](quick-start.md) para começar a utilizar o Content Moderator.
