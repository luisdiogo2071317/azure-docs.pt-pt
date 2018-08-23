---
title: 'Pedido de início rápido: E filtrar imagens usando o SDK em Python'
description: Neste início rápido, pedir e filtrar as imagens devolvidas pela pesquisa de imagens Bing, com o Python.
titleSuffix: Azure Image Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: 4729f103bb9b50d4ff039907db8eb677f3dc290a
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "41987639"
---
 # <a name="quickstart-request-and-filter-images-using-the-sdk-and-python"></a>Início rápido: Imagens de pedido e o filtro utilizar o SDK e o Python

O SDK de pesquisa de imagens do Bing contém a funcionalidade da API REST para consultas de web e os resultados da análise. 

O [da origem de código para amostras de Python SDK de pesquisa do Bing imagem](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image_search_samples.py) está disponível no Hub de Git.

## <a name="application-dependencies"></a>Dependências de aplicações
Se não o tiver, instale o Python. O SDK é compatível com o Python 2.7, 3.3, 3.4, 3.5 e 3.6.

A recomendação geral para o desenvolvimento do Python é usar um [ambiente virtual](https://docs.python.org/3/tutorial/venv.html). Instalar e inicializar o ambiente virtual com o [venv módulo](https://pypi.python.org/pypi/virtualenv). Tem de instalar virtualenv para o Python 2.7.
```
python -m venv mytestenv
```
Instale as dependências do SDK de pesquisa de imagens do Bing:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-imagesearch
```
## <a name="image-search-client"></a>Cliente de pesquisa de imagem
Obter um [chave de acesso de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) sob *pesquisa*. Adicione as importações:
```
from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
from azure.cognitiveservices.search.imagesearch.models import ImageType, ImageAspect, ImageInsightModule
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Criar uma instância do `CognitiveServicesCredentials`e criar uma instância do cliente:
```
client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Procure imagens na consulta (Yosemite), filtradas para os GIF animados e amplo aspecto. Verifique se o número de resultados e imprimir insightsToken, URL de miniaturas e web URL do primeiro resultado.
```
image_results = client.images.search(
        query="Yosemite",
        image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
        aspect=ImageAspect.wide # Could be the str "Wide"
    )
    print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

    if image_results.value:
        first_image_result = image_results.value[0]
        print("Image result count: {}".format(len(image_results.value)))
        print("First image insights token: {}".format(first_image_result.image_insights_token))
        print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
        print("First image web search url: {}".format(first_image_result.web_search_url))
    else:
        print("Couldn't find image results!")

```
Pesquisa de imagens para (Yosemite), filtradas para os GIF animados e amplo aspecto.  Verifique se o número de resultados.  Imprima `insightsToken`, `thumbnail url` e `web url` do primeiro resultado.
```
image_results = client.images.search(
    query="Yosemite",
    image_type=ImageType.animated_gif, # Could be the str "AnimatedGif"
    aspect=ImageAspect.wide # Could be the str "Wide"
)
print("\r\nSearch images for \"Yosemite\" results that are animated gifs and wide aspect")

if image_results.value:
    first_image_result = image_results.value[0]
    print("Image result count: {}".format(len(image_results.value)))
    print("First image insights token: {}".format(first_image_result.image_insights_token))
    print("First image thumbnail url: {}".format(first_image_result.thumbnail_url))
    print("First image web search url: {}".format(first_image_result.web_search_url))
else:
    print("Couldn't find image results!")

```

Obter resultados mais populares:
```
trending_result = client.images.trending()
print("\r\nSearch trending images")

# Categorires
if trending_result.categories:
    first_category = trending_result.categories[0]
    print("Category count: {}".format(len(trending_result.categories)))
    print("First category title: {}".format(first_category.title))
    if first_category.tiles:
        first_tile = first_category.tiles[0]
        print("Subcategory tile count: {}".format(len(first_category.tiles)))
        print("First tile text: {}".format(first_tile.query.text))
        print("First tile url: {}".format(first_tile.query.web_search_url))
    else:
        print("Couldn't find subcategory tiles!")
    else:
        print("Couldn't find categories!")

```

## <a name="next-steps"></a>Passos Seguintes

[Amostras do SDK de Python de serviços cognitivas](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


