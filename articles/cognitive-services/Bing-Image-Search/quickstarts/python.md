---
title: 'Início rápido: Procure imagens - API de REST de pesquisa de imagens do Bing e Python'
titleSuffix: Azure Cognitive Services
description: Utilize este guia de introdução para enviar pedidos de pesquisa de imagem para a API de pesquisa de imagens do Bing REST com o Python e receber respostas JSON.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: quickstart
ms.date: 8/20/2018
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: a93a044279cccd883de5f946bb236cad4b088ae2
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 12/11/2018
ms.locfileid: "53261982"
---
# <a name="quickstart-search-for-images-using-the-bing-image-search-rest-api-and-python"></a>Início rápido: Procure imagens usando a API de REST de pesquisa de imagens do Bing e Python

Utilize este guia de início rápido para fazer a sua primeira chamada à API de Pesquisa de Imagens do Bing e receber uma resposta JSON. Esta aplicação Python simples envia uma consulta de pesquisa para a API e apresenta os resultados não processados.

Embora esta aplicação esteja escrita em Python, a API é um serviço Web RESTful compatível com a maioria das linguagens de programação.

Pode executar este exemplo como um bloco de notas do Jupyter no [MyBinder](https://mybinder.org), ao clicar no destaque de lançamento do Binder:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)


Além disso, o código fonte deste exemplo está disponível [no GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingImageSearchv7.py).

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="running-the-quickstart"></a>Executar o início rápido

Para começar, defina `subscription_key` para uma chave de subscrição válida para o serviço de API do Bing.

```python
subscription_key = None
assert subscription_key
```

Em seguida, verifique se o ponto final `search_url` está correto. Até ao momento, só é utilizado um ponto final para as APIs de Pesquisa do Bing. Caso se depare com erros de autorização, verifique novamente este valor relativamente ao ponto final da pesquisa do Bing no seu dashboard do Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Defina `search_term` para procurar imagens de cachorrinhos.


```python
search_term = "puppies"
```

O seguinte bloco utiliza a biblioteca `requests` em Python para chamar as APIs de Pesquisa do Bing e devolver os resultados como um objeto JSON. Tenha em atenção que passamos a chave de API através do dicionário `headers` e o termo de pesquisa através do dicionário `params`. Para ver a lista completa de opções que podem ser utilizadas para filtrar os resultados da pesquisa, veja a documentação da [API REST](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

O objeto `search_results` contém as imagens reais juntamente com metadados detalhados, como os itens relacionados. Por exemplo, a seguinte linha de código pode extrair os URLs das miniaturas dos primeiros 16 resultados.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Em seguida, utilize a biblioteca `PIL` para transferir as imagens das miniaturas e a biblioteca `matplotlib` para compô-las numa grelha de 4x4.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

f, axes = plt.subplots(4, 4)
for i in range(4):
    for j in range(4):
        image_data = requests.get(thumbnail_urls[i+4*j])
        image_data.raise_for_status()
        image = Image.open(BytesIO(image_data.content))        
        axes[i][j].imshow(image)
        axes[i][j].axis("off")
plt.show()
```

## <a name="sample-json-response"></a>Resposta JSON de exemplo

As respostas da API de Pesquisa de Imagens do Bing são devolvidas como JSON. Esta resposta de amostra foi truncada para mostrar um único resultado.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Bing Image Search single-page app tutorial](../tutorial-bing-image-search-single-page-app.md) (Tutorial de aplicação de página única da Pesquisa de Imagens do Bing)

## <a name="see-also"></a>Consulte também

* [O que é a Pesquisa de Imagens do Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Experimentar uma demonstração interativa online](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Obter uma chave de acesso aos Serviços Cognitivos gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentação dos Serviços Cognitivos do Azure](https://docs.microsoft.com/azure/cognitive-services)
* [Bing Image Search API reference](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) (Referência da API de Pesquisa de Imagens do Bing)
