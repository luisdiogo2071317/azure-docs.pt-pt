---
title: O que é a Pesquisa Personalizada do Bing? | Microsoft Docs
description: Fornece uma visão geral de pesquisa personalizada do Bing
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: b6f50844d6571cca6d63c1db7a85863e3d22d411
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948082"
---
# <a name="what-is-bing-custom-search"></a>O que é a Pesquisa Personalizada do Bing?

Pesquisa personalizada do Bing permite-lhe criar experiências de pesquisa personalizadas para tópicos que mais lhe interessa. Por exemplo, se for proprietário de um site que fornece uma experiência de pesquisa, pode especificar os domínios, sites e páginas da Web que pesquisa do Bing. Resultados de pesquisa personalizados para o conteúdo que ele se preocupa em vez de ter os seus utilizadores verão a página através de resultados de pesquisa que possuem conteúdo irrelevante.

Para criar a sua vista personalizada da web, utilize a pesquisa personalizada do Bing [portal](https://customsearch.ai). O portal permite-lhe criar uma instância de pesquisa personalizada que especifica os domínios, sites e páginas da Web que pretende que o Bing para pesquisar e os sites que não quê-lo para pesquisar. Além de especificar os URLs do conteúdo que conhece, também pode utilizar o portal para encontrar o conteúdo relevante que pretende adicionar.

O portal também lhe permite afixar uma página Web específica na parte superior do resultado da pesquisa, se o usuário inserir um termo de pesquisa específico. 

Depois de definir a sua instância, pode integrar a pesquisa personalizada no seu site, aplicação de ambiente de trabalho ou aplicação móvel, chamando a API de pesquisa personalizada. Se tiver um site baseado na web ou aplicação, pode permitir que a interface do Usuário alojada composição da interface de pesquisa para.

A imagem seguinte mostra a simplicidade da integração de pesquisa personalizada.

![alt de imagem](./media/bcs-overview.png "funciona como a pesquisa personalizada do Bing.")

## <a name="adding-custom-search-box-suggestions"></a>Adição de sugestões de caixa de pesquisa personalizada

Pode enriquecer a sua experiência de pesquisa personalizada com sugestões de caixa de pesquisa personalizada. Esta funcionalidade permite-lhe fornecer sugestões de pesquisa personalizada relevantes para a sua experiência de pesquisa. Conforme o usuário digita na caixa de pesquisa, na lista suspensa contém cadeias de caracteres de consulta sugerida com base na cadeia de consulta parcial do utilizador. Pode especificar se pretende devolver apenas suas sugestões personalizadas ou também incluem sugestões do Bing. [Leia mais](define-custom-suggestions.md).

## <a name="adding-custom-image-search-experience"></a>Adicionar experiência de pesquisa de imagem personalizada

Pode enriquecer a sua experiência de pesquisa personalizada com imagens. É semelhante aos resultados da web, suporta a pesquisa personalizada a procurar imagens na lista de sua instância dos Web sites. [Leia mais](get-images-from-instance.md).

## <a name="adding-custom-video-search-experience"></a>Adicionar a experiência de pesquisa de vídeo personalizada

Pode enriquecer a sua experiência de pesquisa personalizada com vídeos. Semelhante para os resultados da web, suporta a pesquisa personalizada a procurar vídeos na lista de sua instância dos Web sites. [Leia mais](get-videos-from-instance.md).

## <a name="sharing-your-custom-search-instance-with-others"></a>A instância de pesquisa personalizada de partilha com outras pessoas

Pode facilmente permitir colaboração, edição e teste da sua instância, partilhando-a com membros de sua equipe. [Leia mais](share-your-custom-search.md).

## <a name="next-steps"></a>Passos Seguintes

Para começar rapidamente a utilizar, veja [criar a primeira instância de pesquisa personalizada do Bing](quick-start.md).

Para obter detalhes sobre como personalizar a sua instância de pesquisa, veja [definem uma instância de pesquisa personalizada](define-your-custom-view.md).

Familiarize-se com o conteúdo de referência para cada um dos pontos de extremidade de pesquisa personalizada. A referência contém os pontos de extremidade, cabeçalhos e os parâmetros de consulta que usaria para solicitar os resultados da pesquisa. Também inclui as definições dos objetos de resposta.

- [API de pesquisa personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [API de imagem personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [API de Vidoe personalizado](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [API de sugestão automática personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)


Não se esqueça de ler o artigo [Requisitos de Utilização e Apresentação do Bing](./use-and-display-requirements.md) para que não infrinja nenhuma das regras relativas à utilização dos resultados da pesquisa.