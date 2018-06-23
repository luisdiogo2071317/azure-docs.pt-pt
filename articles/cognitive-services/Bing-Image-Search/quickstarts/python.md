---
title: Chamada e resposta - guia de introdução do Python para serviços do Azure cognitivos, API de pesquisa do Bing imagem | Microsoft Docs
description: Exemplos de código e informações de GET para o ajudar a rapidamente começar a utilizar a API de pesquisa do Bing imagens nos serviços cognitivos Microsoft no Azure.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: 3b5d6a961ce4bcde8aaf73f1fbd30689a6c2c2d1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352346"
---
# <a name="call-and-response-your-first-bing-image-search-query-in-python"></a>Chamada e resposta: a primeira consulta de pesquisa do Bing imagem no Python

A API de pesquisa do Bing imagem fornece uma experiência semelhante ao Bing.com/Images, permitindo-lhe enviar uma consulta de pesquisa de utilizador para o Bing e voltar a uma lista de imagens relevantes.

Esta explicação passo a passo demonstra um exemplo simples da chamada para a API de pesquisa do Bing imagem e processamento pós-cópia o objeto JSON resultante. Para obter mais informações, consulte [documentação de pesquisa do Bing imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

Pode executar este exemplo como um bloco de notas do Jupyter [MyBinder](https://mybinder.org) ao clicar no início do Gestor de enlaces de destaque: 

[![Gestor de enlaces](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

Tem de ter um [conta da API de serviços cognitivos](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) com **APIs de pesquisa do Bing**. O [avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) é suficiente para este início rápido. Tem a chave de acesso fornecida quando ativar a avaliação gratuita, ou pode utilizar uma chave de subscrição paga do dashboard do Azure.

## <a name="running-the-walkthrough"></a>Executar as instruções
Para prosseguir com as instruções, defina `subscription_key` a sua chave de API para o serviço de API do Bing.


```python
subscription_key = None
assert subscription_key
```

Em seguida, certifique-se de que o `search_url` ponto final está correta. Desta redação, apenas um ponto final é utilizado para a pesquisa do Bing APIs. Se ocorrerem erros de autorização, verificar este valor contra o ponto final da pesquisa Bing no dashboard do Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Definir `search_term` serve para imagens de puppies.


```python
search_term = "puppies"
```

O seguinte bloquear utiliza o `requests` biblioteca do Python para a chamada para a pesquisa do Bing APIs e devolvem os resultados como um objeto JSON. Observe que podemos passa a chave de API através de `headers` dicionário e a pesquisa termo através o `params` dicionário. Para obter uma lista completa das opções que podem ser utilizadas para filtrar os resultados da pesquisa, consulte o [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) documentação.


```python
import requests

headers = {"Ocp-Apim-Subscription-Key" : subscription_key}
params  = {"q": search_term, "license": "public", "imageType": "photo"}
response = requests.get(search_url, headers=headers, params=params)
response.raise_for_status()
search_results = response.json()
```

O `search_results` objeto contém as imagens reais, juntamente com metadados avançados, tais como itens relacionados. Por exemplo, a seguinte linha de código pode extrair a miniatura URLS para os primeiros 16 resultados.


```python
thumbnail_urls = [img["thumbnailUrl"] for img in search_results["value"][:16]]
```

Em seguida, podemos utilizar o `PIL` biblioteca para transferir as imagens em miniatura e `matplotlib` biblioteca para compor-los na grelha $4 \times 4$.


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
```

## <a name="next-steps"></a>Passos Seguintes

> [!div class="nextstepaction"]
> [Tutorial de aplicação de página única de pesquisa de imagem do Bing](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral de pesquisa de imagem do Bing](../overview.md)  
[Experimente](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Referência da API de pesquisa do Bing imagem](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
