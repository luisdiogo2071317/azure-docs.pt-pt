---
title: Descrição geral do Azure Moderator conteúdo | Microsoft Docs
description: Saiba como utilizar conteúdo Moderator para acompanhar, sinalizador, avaliar e filtrar o conteúdo inadequado conteúdos gerados pelo utilizador.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 06/15/2017
ms.author: sajagtap
ms.openlocfilehash: cd9e2e9c10e9dc5ba118c8319f76174bf6f0da9f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355142"
---
# <a name="what-is-content-moderator"></a>O que é Moderator conteúdo?

Moderação de interrupção de conteúdo é o processo de monitorização para o conteúdo de risco, indesejável e ofensiva possíveis. O conteúdo moderated pode ser imagens, o texto e vídeos.

## <a name="where-it-is-used"></a>Em que é utilizado

A lista seguinte mostra alguns cenários de exemplo em que é utilizado Moderator conteúdo:

- Online marketplaces moderating catálogos de produto e conteúdos gerados pelo utilizador
- As empresas de jogos moderating utilizador gerou artefactos jogos e gabinetes do chat
- Plataforma de mensagens social moderating imagens, o texto e vídeos adicionados pelos seus utilizadores
- Empresas de suporte de dados de empresa implementação centralizada moderação de interrupção de conteúdo para o respetivo conteúdo
- Fornecedores de soluções de educação K-12 filtragem de conteúdo incorrecto e ofensiva para estudantes e educators

## <a name="what-it-includes"></a>O que inclui:

Conteúdo Moderator é composta por serviço web de várias APIs e incorporado humanos-na-a-ciclo reveja ferramenta que o ajudam a moderada imagens, texto e vídeos.

![Diagrama de blocos do conteúdo Moderator](images/content-moderator-block-diagram.png)

## <a name="apis"></a>APIs

Conteúdo Moderator inclui as APIs seguintes:
  - [**Moderação de interrupção do texto API**](text-moderation-api.md): Utilize esta API para análise de texto para profanity possíveis, explícito, suggestive, ofensivas e identificáveis informações (PII).
  - [**Lista de termo personalizado API**](try-terms-list-api.md): utilizar esta API para correspondência listas personalizadas dos termos de licenciamento para além dos termos de licenciamento incorporados. Utilize estas listas para bloquear ou permitir que o conteúdo de acordo com as políticas de conteúdo.  
  - [**Imagem moderação de interrupção API**](image-moderation-api.md): Utilize esta API para procurar as imagens para o conteúdo para adultos e racy, detetar texto nas imagens com a capacidade de reconhecimento de caráter Optical (OCR) e detetar faces.
  - [**Lista de imagem personalizada API**](try-image-list-api.md): Utilize esta API para correspondência listas personalizadas de imagens, previamente identificado conteúdo que não precisa de classificar novamente.
  - [**Moderação de interrupção gráfica API**](video-moderation-api.md): Utilize esta API para analisar vídeos para o conteúdo para adultos e racy potencial.
  - [**Reveja as APIs**](try-review-api-job.md): Utilize o [tarefas](try-review-api-job.md), [revisões](try-review-api-review.md), e [fluxo de trabalho](try-review-api-workflow.md) operações para criar e automatizar fluxos de trabalho humanos em ciclo dentro do a ferramenta de revisão.

## <a name="human-review-tool"></a>Ferramenta de revisão humana

A subscrição de Moderator conteúdo inclui o incorporado [revisão humana ferramenta](Review-Tool-User-Guide/human-in-the-loop.md). Utilize a API de revisão mencionadas anteriormente para criar revisões de texto, imagens e vídeos para sua moderators humanos tomar decisões finais.

![Ferramenta de rever as vídeo de Moderator conteúda](images/video-review-default-view.png)

## <a name="next-steps"></a>Passos Seguintes

Utilize o [início rápido](quick-start.md) para começar com Moderator conteúdo.
