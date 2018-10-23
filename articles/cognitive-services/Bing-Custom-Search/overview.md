---
title: O que é a Pesquisa Personalizada do Bing?
titlesuffix: Azure Cognitive Services
description: Fornece uma descrição geral de alto nível da Pesquisa Personalizada do Bing.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: overview
ms.date: 09/29/2017
ms.author: v-brapel
ms.openlocfilehash: f2946918f0c1a7a516788989042825e8f49d7b0b
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: pt-PT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49318729"
---
# <a name="what-is-bing-custom-search"></a>O que é a Pesquisa Personalizada do Bing?

Saiba como a Pesquisa Personalizada do Bing lhe permite criar experiências de pesquisa personalizada dos tópicos que mais lhe interessam. Por exemplo, se for proprietário de um site que proporciona uma experiência de pesquisa, pode especificar os domínios, sites e páginas Web que o Bing pesquisa. Os seus utilizadores veem resultados de pesquisa personalizados de acordo com os conteúdos que lhes interessam, em vez de terem de percorrer resultados de pesquisa que podem apresentar conteúdos irrelevantes.

Para criar a sua vista personalizada da Web, utilize a Pesquisa Personalizada do portal [Bing](https://customsearch.ai). O portal permite-lhe criar uma instância de pesquisa personalizada que especifica os domínios, sites e páginas Web nas quais pretende que o Bing pesquise, bem como os sites nos quais não pretende que pesquise. Além de especificar os URL dos conteúdos que conhece, também pode utilizar o portal para procurar conteúdos relevantes que pode querer adicionar.

O portal permite-lhe ainda afixar uma página Web específica na parte superior do resultado da pesquisa, se o utilizador inserir um termo de pesquisa específico. 

Depois de definir a sua instância, pode integrar a pesquisa personalizada no seu site, aplicação de ambiente de trabalho ou aplicação móvel, chamando a API de Pesquisa Personalizada. Se tiver um site ou aplicação baseados na Web, pode permitir que a IU alojada componha a interface de pesquisa.

A imagem seguinte mostra a simplicidade da integração da pesquisa personalizada.

![picture alt](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/cognitive-services/Bing-Custom-Search/media/BCS-Overview.png "Como Funciona a Pesquisa Personalizada do Bing.")

## <a name="adding-custom-search-box-suggestions"></a>Adicionar sugestões de caixa de pesquisa personalizada

Pode enriquecer a sua experiência de pesquisa personalizada com sugestões de caixa de pesquisa personalizada. Esta funcionalidade permite-lhe fornecer sugestões de pesquisa personalizada relevantes para a sua experiência de pesquisa. À medida que o utilizador vai escrevendo na caixa de pesquisa, a lista pendente contém cadeias de consulta sugeridas com base na cadeia de consulta parcial do utilizador. Pode especificar se pretende que sejam devolvidas apenas as suas sugestões personalizadas ou incluir também as sugestões do Bing. [Leia mais](define-custom-suggestions.md).

## <a name="adding-custom-image-search-experience"></a>Adicionar experiência de pesquisa de imagem personalizada

Pode enriquecer a sua experiência de pesquisa personalizada com imagens. À semelhança dos resultados da Web, a pesquisa personalizada suporta a pesquisa de imagens na lista de sites da sua instância. [Leia mais](get-images-from-instance.md).

## <a name="adding-custom-video-search-experience"></a>Adicionar experiência de pesquisa de vídeo personalizada

Pode enriquecer a sua experiência de pesquisa personalizada com vídeos. À semelhança dos resultados da Web, a pesquisa personalizada suporta a pesquisa de vídeos na lista de sites da sua instância. [Leia mais](get-videos-from-instance.md).

## <a name="sharing-your-custom-search-instance-with-others"></a>Partilhar a sua instância de pesquisa personalizada com outras pessoas

Pode permitir facilmente edição e teste de colaboração da sua instância, partilhando-a com membros da sua equipa. [Leia mais](share-your-custom-search.md).

## <a name="next-steps"></a>Passos seguintes

Para começar rapidamente, consulte [Criar a primeira instância de Pesquisa Personalizada do Bing](quick-start.md).

Para obter detalhes sobre como personalizar a sua instância de pesquisa, consulte [Definir uma instância de pesquisa personalizada](define-your-custom-view.md).

Familiarize-se com o conteúdo de referência para cada um dos pontos finais da pesquisa personalizada. A referência contém a lista de pontos finais, cabeçalhos e parâmetros de consulta que teria de utilizar para pedir os resultados da pesquisa. Também inclui as definições dos objetos de resposta.

- [API de Pesquisa Personalizada do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference)
- [API de Imagem Personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference)
- [API de Vídeo Personalizado](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference)
- [API de Sugestão Automática Personalizada](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-autosuggest-api-v7-reference)


Não se esqueça de ler o artigo [Requisitos de Utilização e Apresentação do Bing](./use-and-display-requirements.md) para que não infrinja nenhuma das regras relativas à utilização dos resultados da pesquisa.
