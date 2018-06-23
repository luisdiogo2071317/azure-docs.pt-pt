---
title: Guia de introdução de pesquisa SDK Python de imagem | Microsoft Docs
description: Configuração de aplicação de consola do SDK de pesquisa de imagem.
titleSuffix: Azure Image Search SDK Python quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 02/14/2018
ms.author: v-gedod
ms.openlocfilehash: e30852439ad8ec2d5ddc667b75167e8b5d35be33
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355249"
---
# <a name="image-search-sdk-python-quickstart"></a>Guia de introdução de pesquisa SDK Python de imagem

O SDK de pesquisa do Bing imagem contém as funcionalidades da API REST para consultas de web e os resultados da análise. 

O [código para exemplos do SDK de pesquisa do Python Bing imagem de origem](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/image_search_samples.py) está disponível no Hub de Git.

## <a name="application-dependencies"></a>Dependências da aplicação
Se ainda não tivê-lo, instale o Python. O SDK é compatível com o Python 2.7, 3.3, 3.4, 3.5 e 3.6.

As recomendações gerais para desenvolvimento do Python é utilizar um [ambiente virtual](https://docs.python.org/3/tutorial/venv.html). Instalar e inicializar o ambiente virtual com o [venv módulo](https://pypi.python.org/pypi/virtualenv). Tem de instalar virtualenv para Python 2.7.
```
python -m venv mytestenv
```
Instale dependências do SDK de pesquisa do Bing imagem:
```
cd mytestenv
python -m pip install azure-cognitiveservices-search-imagesearch
```
## <a name="image-search-client"></a>Cliente de pesquisa de imagem
Obter um [chave de acesso de serviços cognitivos](https://azure.microsoft.com/try/cognitive-services/) em *pesquisa*. Adicione importações:
```
from azure.cognitiveservices.search.imagesearch import ImageSearchAPI
from azure.cognitiveservices.search.imagesearch.models import ImageType, ImageAspect, ImageInsightModule
from msrest.authentication import CognitiveServicesCredentials

subscription_key = "YOUR-SUBSCRIPTION-KEY"
```
Criar uma instância do `CognitiveServicesCredentials`e criar o cliente:
```
client = ImageSearchAPI(CognitiveServicesCredentials(subscription_key))
```
Procurar imagens numa consulta (Yosemite), filtradas para gifs animado e aspeto wide. Verifique o número de resultados e imprimir insightsToken, URL em miniatura e web URL do resultado primeiro.
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
Procurar imagens para (Yosemite), filtradas para gifs animado e aspeto wide.  Verifique o número de resultados.  Impressão saída `insightsToken`, `thumbnail url` e `web url` do resultado primeiro.
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

Obter resultados tendências:
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

[Exemplos de SDK Python de serviços cognitivos](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples)


