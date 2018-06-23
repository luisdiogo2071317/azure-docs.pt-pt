---
title: Guia de introdução do Python para os serviços do Azure cognitivos, pesquisa de vídeo do Bing API | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de pesquisa do Bing vídeo no Microsoft serviços cognitivos no Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: ce4356f05e69540bc3bc3241e2ec1751ff7a7276
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352340"
---
# <a name="quickstart-for-bing-video-search-api-with-python"></a>Início rápido para pesquisa de vídeo do Bing API com o Python

Estas instruções mostram como utilizar a API do Bing vídeo de pesquisa, parte dos serviços cognitivos da Microsoft no Azure. Consulte o [referência da API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) para detalhes técnicos sobre as APIs.

Pode executar este exemplo como um bloco de notas do Jupyter [MyBinder](https://mybinder.org) ao clicar no início do Gestor de enlaces de destaque: 

[![Gestor de enlaces](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingVideoSearchAPI.ipynb)


## <a name="prerequisites"></a>Pré-requisitos
Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa do Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para este início rápido. Tem a chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="running-the-walkthrough"></a>Executar as instruções

Em primeiro lugar, defina `subscription_key` a sua chave de API para o serviço de API do Bing.


```python
subscription_key = None
assert subscription_key
```

Em seguida, certifique-se de que o `search_url` ponto final está correta. Desta redação, apenas um ponto final é utilizado para a pesquisa do Bing APIs. Se ocorrerem erros de autorização, verificar este valor contra o ponto final da pesquisa Bing no dashboard do Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

Definir `search_term` serve para vídeos do kittens


```python
search_term = "kittens"
```

O seguinte bloquear utiliza o `requests` biblioteca do Python para a chamada para a pesquisa do Bing APIs e devolvem os resultados como um objeto JSON. Observe que podemos passa a chave de API através de `headers` dicionário e a pesquisa termo através o `params` dicionário. Para obter uma lista completa das opções que podem ser utilizadas para filtrar os resultados da pesquisa, consulte o [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) documentação.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

O `search_results` objeto contém os vídeos relevantes, juntamente com metadados avançado. Para ver um dos vídeos, utilize o `embedHtml` propriedade e insira-o para um `IFrame`.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"].replace("autoplay=1","autoplay=0"))
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Vídeos de paginação](paging-videos.md)
> [Resizing e cropping imagens em miniatura](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Consulte também 

 [Pesquisa na web vídeos](search-the-web.md) [experimente](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
