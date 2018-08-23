---
title: 'Início rápido: Consultas de pesquisa de envio com a API REST para a API de pesquisa de imagens do Bing em Python'
description: Neste início rápido, envia consultas de pesquisa para a API de pesquisa do Bing para obter uma lista de imagens relevantes com o Python.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: bc527ba39b580935f113f56aa63f7bdd283ba304
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: pt-PT
ms.lasthandoff: 08/13/2018
ms.locfileid: "41994389"
---
# <a name="quickstart-send-search-queries-using-the-rest-api-and-python"></a>Início rápido: Consultas de pesquisa de envio utilizar a REST API e Python

A API de pesquisa de imagens do Bing fornece uma experiência semelhante à Bing.com/Images, permitindo-lhe enviar uma consulta de pesquisa de usuário para o Bing e obter uma lista de imagens relevantes.

Este passo a passo demonstra um exemplo simples de chamar a API de pesquisa de imagens do Bing e pós-processamento o objeto JSON resultante. Para obter mais informações, consulte [documentação de pesquisa de imagens Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference).

Pode executar este exemplo como um bloco de notas do Jupyter no [MyBinder](https://mybinder.org) clicando no lançamento associador de destaque: 

[![Associador](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=BingImageSearchAPI.ipynb)

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="running-the-walkthrough"></a>Executar o passo a passo
Para continuar com o passo a passo, defina `subscription_key` à sua chave de API para o serviço de API do Bing.


```python
subscription_key = None
assert subscription_key
```

Em seguida, certifique-se de que o `search_url` ponto final está correta. No momento desta edição, apenas um ponto final é utilizado para APIs de pesquisa do Bing. Se encontrar erros de autorização, verifique novamente este valor relativamente ao ponto final da pesquisa do Bing no dashboard do Azure.


```python
search_url = "https://api.cognitive.microsoft.com/bing/v7.0/images/search"
```

Definir `search_term` procurar imagens de puppies.


```python
search_term = "puppies"
```

O seguinte bloquear utiliza o `requests` biblioteca em Python para chamar a APIs de pesquisa do Bing e retornar os resultados como um objeto JSON. Observar que, transmitimos a chave de API através do `headers` dicionário e a pesquisa prazo através do `params` dicionário. Para ver a lista completa de opções que podem ser utilizadas para filtrar os resultados da pesquisa, veja a [REST API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) documentação.


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

Em seguida, podemos usar o `PIL` biblioteca para transferir imagens em miniatura e o `matplotlib` biblioteca para capturá-los numa grade de $ $4 \times 4.


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
> [Tutorial de aplicação de página única de pesquisa de imagens Bing](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Consulte também 

[Descrição geral de pesquisa de imagens Bing](../overview.md)  
[Experimente-o](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Obter uma chave de acesso de avaliação gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Referência da API de pesquisa de imagens do Bing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
