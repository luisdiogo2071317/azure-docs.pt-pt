---
title: O que é a Pesquisa Visual do Bing?
titleSuffix: Azure Cognitive Services
description: A Pesquisa Visual do Bing disponibiliza detalhes ou informações sobre imagens, como imagens semelhantes ou origens de compras.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: overview
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 9e323e9ec928d9f8b0592850adcd088a589ebf28
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863096"
---
# <a name="what-is-the-bing-visual-search-api"></a>O que é a API de Pesquisa Visual do Bing?

A API de Pesquisa Visual do Bing fornece detalhes de imagens semelhantes aos mostrados em Bing.com/images. Ao carregar uma imagem ou fornecer um URL para um, esta API pode identificar uma variedade de detalhes sobre o mesmo, incluindo imagens visualmente similares, fontes, páginas da Web que incluem a imagem e muito mais de compras. Se utilizar o [API de pesquisa de imagens do Bing](../bing-image-search/overview.md), é possível utilizar tokens de informações anexadas para os resultados da pesquisa a API em vez de carregar uma imagem.

## <a name="insights"></a>Informações

Seguem-se as informações que pesquisa Visual permite que descubra:

| Recomendações personalizadas                              | Descrição |
|--------------------------------------|-------------|
| Imagens visualmente similares              | Uma lista de imagens que são visualmente semelhantes à imagem de entrada. |
| Produtos visualmente semelhantes            | Produtos visualmente semelhantes para o produto mostrado.            |
| Origens de compras                     | Lugares onde pode adquirir o item mostrado na imagem de entrada.            |
| Pesquisas relacionadas                     | Pesquisas relacionadas feitas por outras pessoas ou que são baseadas no conteúdo da imagem.            |
| Páginas da Web que incluem a imagem     | Páginas da Web que incluem a imagem de entrada.            |
| Receitas                              | Páginas da Web que incluem receitas para tornar o dish mostrado na imagem de entrada            |

Para além destas informações, a Pesquisa Visual também devolve um conjunto de diversos termos (etiquetas) que derivam da imagem introduzida. Essas etiquetas permitem que os utilizadores explorem os conceitos presentes nas imagens. Por exemplo, se a imagem introduzida for de um atleta famoso, uma das etiquetas pode ser o nome e outra Desporto. Ou se a imagem introduzida for uma tarte de maçã, as etiquetas podem ser Tarte de Maçã, Maçãs, Sobremesas, para que os utilizadores possam explorar os conceitos associados.

Os resultados da Pesquisa Visual também incluem caixas delimitadoras para regiões de interesse nas imagens. Por exemplo, se uma imagem tiver várias celebridades, os resultados poderão incluir caixas delimitadoras para cada celebridade reconhecida na mesma. Ou se o Bing reconhecer um produto ou uma roupa na imagem, o resultado poderá incluir uma caixa delimitadora para esse produto ou essa roupa.

> [!IMPORTANT]
> Se obtiver as informações de imagem usando a API de pesquisa de imagens Bing, considere mudar para a API Visual do Bing Search, que fornece informações mais abrangentes.

## <a name="workflow"></a>Fluxo de trabalho

A API de pesquisa Visual do Bing é um serviço RESTful web, facilitando a chamar a partir de qualquer linguagem de programação que pode fazer solicitações HTTP e analisar JSON. Pode utilizar o serviço usando a API REST ou o SDK.

1. Crie uma conta de API dos serviços cognitivos com acesso às APIs de pesquisa do Bing. Se não tiver uma subscrição do Azure, pode criar uma conta gratuitamente.
2. Envie um pedido para a API, com uma consulta de pesquisa válidos.
3. Processe a resposta da API ao analisar a mensagem JSON devolvida.


## <a name="next-steps"></a>Passos Seguintes

Em primeiro lugar, experimente a [demonstração interativa](https://azure.microsoft.com/services/cognitive-services/bing-visual-search/) da API de Pesquisa de Imagens do Bing.
Esta demonstração mostra como pode rapidamente personalizar uma consulta de pesquisa e fazer uma busca de imagens na web.

Quando estiver pronto para chamar a API, crie uma [conta dos serviços cognitivos da API](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account). Se não tiver uma subscrição do Azure, pode [criar uma conta](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) gratuita.

Para começar rapidamente com o primeiro pedido, veja os inícios rápidos: [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).


## <a name="see-also"></a>Consulte também

* O [referência de pesquisa Visual do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) documentação contém definições e informações sobre a pontos de extremidade, cabeçalhos, as respostas de API e parâmetros de consulta que pode utilizar para solicitar os resultados da pesquisa baseada em imagem.

* Os [Requisitos de apresentação e utilização do Bing](./use-and-display-requirements.md) especificam as utilizações aceitáveis do conteúdo e as informações obtidas por meio das APIs de Pesquisa do Bing.
