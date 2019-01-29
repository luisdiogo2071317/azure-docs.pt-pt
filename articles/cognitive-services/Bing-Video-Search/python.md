---
title: 'Início rápido: Pesquisa de vídeos do Bing, Python'
titlesuffix: Azure Cognitive Services
description: Obtenha informações e exemplos de código para ajudá-lo a começar a utilizar rapidamente a API da Pesquisa de Vídeos do Bing.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 9/21/2017
ms.author: aahi
ms.openlocfilehash: 1032e6c071f7fb35921d1837f040bf6fa61a0269
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203900"
---
# <a name="quickstart-bing-video-search-api-with-python"></a>Início rápido: Pesquisa de vídeos do Bing API com Python

Estas instruções mostram como utilizar a API da Pesquisa de Vídeos do Bing, que faz parte dos Serviços Cognitivos da Microsoft no Azure. Consulte a [Referência da API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) para obter detalhes técnicos sobre as APIs.

Pode executar este exemplo como um bloco de notas do Jupyter no [MyBinder](https://mybinder.org), ao clicar no destaque de lançamento do Binder: 

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingVideoSearchAPI.ipynb)


## <a name="prerequisites"></a>Pré-requisitos
Tem de ter uma [conta da API dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de Pesquisa do Bing**. A [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para este início rápido. Precisa da chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure. Consulte também [dos serviços cognitivos preços - API de pesquisa Bing](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="running-the-walkthrough"></a>Executar as instruções

Em primeiro lugar, defina `subscription_key` para a sua chave de API para o serviço de API do Bing.


```python
subscription_key = None
assert subscription_key
```

Em seguida, verifique se que o ponto final `search_url` está correto. Até ao momento, só é utilizado um ponto final para as APIs de Pesquisa do Bing. Caso se depare com erros de autorização, verifique novamente este valor relativamente ao ponto final da pesquisa do Bing no dashboard do Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/videos/search"
```

Definir `search_term` para procurar vídeos de gatinhos


```python
search_term = "kittens"
```

O bloco seguinte utiliza a biblioteca `requests` em Python para chamar as APIs de Pesquisa do Bing e devolver os resultados como um objeto JSON. Tenha em atenção que passamos a chave de API através do dicionário `headers` e o termo de pesquisa através do dicionário `params`. Para ver a lista completa de opções que podem ser utilizadas para filtrar os resultados da pesquisa, veja a documentação da [API REST](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference).


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "count":5, "pricing": "free", "videoLength":"short"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

O objeto `search_results` contém os vídeos relevantes, juntamente com metadados detalhados. Para ver um dos vídeos, utilize a respetiva propriedade `embedHtml` e insira-o num `IFrame`.


```python
from IPython.display import HTML
HTML(search_results["value"][0]["embedHtml"].replace("autoplay=1","autoplay=0"))
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Vídeos da página](paging-videos.md)
> [Redimensionar e recortar imagens em miniatura](resize-and-crop-thumbnails.md)

## <a name="see-also"></a>Consulte também 

 [Pesquisar vídeos na Web](search-the-web.md) [Experimente](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/)
