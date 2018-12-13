---
title: Personalizar um modelo de pessoa no indexador de vídeo - Azure
titlesuffix: Azure Media Services
description: Este artigo fornece uma descrição geral sobre o que é um modelo de pessoa no indexador de vídeo e como personalizá-lo.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/05/2018
ms.author: anzaman
ms.openlocfilehash: 073cff22f17f496c2ff85cfbf716751dfea1e03e
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285026"
---
# <a name="customize-a-person-model-in-video-indexer"></a>Personalizar um modelo de pessoa Video Indexer


O Video Indexer suporta a deteção de rostos e reconhecimento de celebridade para conteúdo de vídeo. O recurso de reconhecimento de celebridade abrange aproximadamente 1 000 000 rostos com base na origem de dados frequentemente pedidos como IMDB, Wikipedia e influenciadores principais do LinkedIn. Rostos que não são reconhecidos pela funcionalidade de reconhecimento de celebridade são detetados; No entanto, eles são deixados sem nome. Depois de carregar o vídeo para o indexador de vídeos e obtenha os resultados, pode voltar atrás e nomear os rostos que não foram reconhecidos. Depois de identificar um rosto com um nome, o mostrador e o nome seja adicionados ao modelo de pessoa da sua conta. Indexador de vídeos, em seguida, irá reconhecer este rostos nos vídeos futuros e vídeos nos últimos.

Pode utilizar o Web site do indexador de vídeos ou a API para editar rostos que foram detetados um vídeo na sua conta, conforme descrito nos seguintes tópicos:

- [Personalizar o modelo de pessoa com APIs](customize-person-model-with-api.md)
- [Personalizar o modelo de pessoa com o Web site](customize-person-model-with-website.md)
