---
title: O que é a API de Pesquisa de Imagens do Bing?
titleSuffix: Azure Cognitive Services
description: A API de Pesquisa de Imagens do Bing permite-lhe utilizar as capacidades cognitivas de pesquisa de imagens do Bing na sua aplicação. Com o envio de consultas de pesquisa de utilizadores com a API, pode obter e apresentar imagens relevantes e de alta qualidade semelhantes às Imagens do Bing.
services: cognitive-services
author: aahill
manager: nitinme
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: b5fc297850ef4b8bfbc0e4bdcf21877e1b94ffe1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55861756"
---
# <a name="what-is-the-bing-image-search-api"></a>O que é a API de Pesquisa de Imagens do Bing?

A API de Pesquisa de Imagens do Bing permite-lhe utilizar as capacidades cognitivas de pesquisa de imagens do Bing na sua aplicação. Com o envio de consultas de pesquisa de utilizadores com a API, pode obter e apresentar imagens relevantes e de alta qualidade semelhantes às [Imagens do Bing](https://www.bing.com/images).

Lembre-se de que a API de Pesquisa de Imagens do Bing fornece apenas resultados da pesquisa com imagens. Utilize a [API de Pesquisa na Web do Bing](../bing-web-search/search-the-web.md), a [API de Pesquisa de Vídeos do Bing](https://docs.microsoft.com/azure/cognitive-services/Bing-Video-Search) e a [API de Pesquisa de Notícias do Bing](https://review.docs.microsoft.com/azure/cognitive-services/bing-news-search) para outros tipos de conteúdo web.

## <a name="bing-image-search-features"></a>Funcionalidades da Pesquisa de Imagens do Bing

Enquanto a Pesquisa de Imagens do Bing localiza e devolve principalmente imagens relevantes a partir de uma consulta de pesquisa, o serviço também fornece várias funcionalidades adicionais para a obtenção de imagens na web inteligente e focada.


| Funcionalidade                                                                                                                                                                                 | Descrição                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Sugerir termos de pesquisa em tempo real](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) | Melhore a sua experiência de aplicação utilizando a [API de Sugestão Automática do Bing](../bing-autosuggest/get-suggested-search-terms.md) para apresentar os termos de pesquisa sugeridos, à medida que estão a ser escritos. |
| [Filtrar e restringir os resultados de imagem](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images)                       | Filtre as imagens que o Bing devolve ao editar os parâmetros de consulta.                                                                                                       |
| [Recortar, redimensionar e apresentar miniaturas](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/resize-and-crop-thumbnails)                                                | Edite e apresente visualizações de miniaturas para as imagens devolvidas pela Pesquisa de Imagens do Bing.                                                                                      |
| [Dinamizar e expandir consultas de pesquisa do utilizador](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries)               | Expanda as capacidades de pesquisa ao incluir e apresentar os termos de pesquisa para consultas sugeridos pelo Bing.                                                                    |
| [Obter imagens populares](https://review.docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images)                                                                     | Personalize uma pesquisa para obter imagens populares em todo o mundo.                                                                                                          |

## <a name="workflow"></a>Fluxo de trabalho

A API de Pesquisa de Imagens do Bing é um serviço Web RESTful fácil de chamar a partir de qualquer linguagem que possa fazer pedidos e analisar as respostas JSON. Pode utilizar o serviço através da [API REST](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp?) ou o [SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

1. Crie uma [conta de API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com acesso às APIs de Pesquisa do Bing. Se não tiver uma subscrição do Azure, pode [criar uma conta](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) gratuita.
2. Envie um pedido para a API com uma [consulta de pesquisa](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) válida.
3. Processe a resposta da API ao analisar a mensagem JSON devolvida.

## <a name="next-steps"></a>Passos Seguintes

Em primeiro lugar, experimente a [demonstração interativa](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/) da API de Pesquisa de Imagens do Bing.
Esta demonstração mostra como pode rapidamente personalizar uma consulta de pesquisa e fazer uma busca de imagens na web.

Quando estiver pronto para chamar a API, crie uma [conta dos serviços cognitivos da API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Se não tiver uma subscrição do Azure, pode [criar uma conta](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) gratuita.

Para começar rapidamente com o primeiro pedido da API, pode aprender a:

* [Enviar consultas de pesquisa para o Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/quickstarts/csharp) com a API REST ou
* [Pedir e filtrar](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) as imagens que o Bing devolve utilizando o SDK.

## <a name="see-also"></a>Consulte também

* A secção de referência [API de Pesquisa de Imagens do Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) contém definições e informações sobre pontos finais, cabeçalhos, respostas de API e parâmetros de consulta que pode utilizar para solicitar resultados de pesquisa baseados em imagem.

* Os [Requisitos de apresentação e utilização do Bing](./useanddisplayrequirements.md) especificam as utilizações aceitáveis do conteúdo e as informações obtidas por meio das APIs de Pesquisa do Bing.

* O tópico [Obter imagens da web com a API de Pesquisa de Imagens do Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-get-images) descreve como pesquisar e obter imagens da web.

* O tópico [Enviar e trabalhar com consultas de pesquisa](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/concepts/bing-image-search-sending-queries) descreve como fazer, personalizar e dinamizar consultas de pesquisa.
